# 04 — LoRA and QLoRA: Efficient Fine-Tuning

> Fine-tuning a 7B model used to require 8× A100 GPUs and thousands of dollars. LoRA changed that. Today you can fine-tune on a single consumer GPU — or even for free on Google Colab. This module explains how, from theory to working code.

✍️ Part of [Phase 4: Fine-Tuning](./README.md) · 🟡 Intermediate · ⏱️ ~3 hr read + build

---

## 🎯 What You'll Learn

- Why full fine-tuning is impractical for most people — and what LoRA does differently
- How LoRA works intuitively (no heavy math required)
- What QLoRA adds on top, and why it matters for consumer hardware
- How to fine-tune any HuggingFace model using LoRA end-to-end
- How to prepare a dataset correctly
- How to merge, save, and use your fine-tuned model
- How to avoid the most common fine-tuning mistakes

## ⏱️ Prerequisites

- [01 — When to Fine-Tune vs. Prompt Engineer](./01-when-to-fine-tune.md)
- [02 — Dataset Preparation](./02-dataset-prep.md)
- Basic Python. Familiarity with HuggingFace Transformers is helpful but not required.
- For QLoRA: a GPU with at least 8GB VRAM (or free Google Colab T4)

---

## 1. The Problem With Full Fine-Tuning

In full fine-tuning, you update every single parameter in the model. For a 7B parameter model stored at 16-bit precision:

```
7,000,000,000 parameters × 2 bytes (fp16) = 14 GB  just to store the model

During training you also need:
  + Gradients:          ~14 GB
  + Optimizer states:   ~28 GB (Adam keeps 2 states per parameter)
  ─────────────────────────────
  Total VRAM needed:    ~56 GB minimum
```

A single NVIDIA A100 (80GB) costs ~$2/hr on cloud. A full fine-tuning run takes hours to days. For most use cases, this is completely impractical.

**LoRA's insight:** You don't need to update all 7 billion parameters. Most of the model's knowledge is already there — you just need to adjust a small slice of it.

---

## 2. How LoRA Works

### The Core Idea

Every layer in a transformer has weight matrices — for example, the query matrix `W_q` in the attention mechanism might be shape `[4096, 4096]` (4096 × 4096 = ~16M parameters for that one matrix alone).

LoRA freezes all original weights and adds a pair of small trainable matrices alongside each one:

```
Original (frozen):     W  [4096 × 4096]   ← 16.7M parameters, NOT updated

LoRA addition:         A  [4096 × r]       ─┐
                       B  [r    × 4096]    ─┘ r << 4096, e.g. r=16

Output = W·x + (B·A)·x · scale
           ↑         ↑
     frozen      trainable
```

`r` is called the **LoRA rank**. At `r=16`, the two matrices have:
- `A`: 4096 × 16 = 65,536 parameters
- `B`: 16 × 4096 = 65,536 parameters
- Total: 131,072 parameters — vs 16.7M in the original matrix

**That's a 128× reduction in trainable parameters for just one matrix.**

Across the whole model, LoRA typically trains less than 1% of the parameters that full fine-tuning would.

### Visualised

```
FULL FINE-TUNING:
┌─────────────────────┐
│  Model (7B params)  │ ← all weights updated
│  ████████████████   │    56GB+ VRAM needed
└─────────────────────┘

LORA FINE-TUNING:
┌─────────────────────┐    ┌──────────────┐
│  Model (7B params)  │    │  LoRA adapter│ ← only adapter trained
│  ░░░░░░░░░░░░░░░░   │ +  │  (~20M)      │    ~16GB VRAM needed
│    (frozen)         │    └──────────────┘
└─────────────────────┘
```

### After Training: Merge or Keep Separate

You can either:
1. **Keep separate** — load the base model + LoRA adapter at inference time (more flexible, slightly slower)
2. **Merge** — mathematically combine `W + B·A` back into a single matrix (same size as original, no overhead at inference)

For production deployment, merge. For experimentation, keep separate so you can swap adapters.

---

## 3. QLoRA: LoRA on a Quantized Model

QLoRA (Quantized LoRA) takes the idea one step further: quantize the base model to **4-bit** before adding LoRA adapters.

```
QLORA:
┌─────────────────────┐    ┌──────────────┐
│  Model (7B @ 4-bit) │    │  LoRA adapter│ ← fp16/bf16
│  ░░░░░░░░░░░░░░░░   │ +  │  (~20M)      │
│  ~3.5 GB            │    │  ~80 MB      │
└─────────────────────┘    └──────────────┘
Total: ~4-5 GB VRAM  ← runs on a free Colab T4!
```

The quantization uses a technique called **NF4 (Normal Float 4)** which preserves model quality far better than naive int4 quantization. The LoRA adapters remain in full precision (bf16/fp16), so gradients are computed accurately.

**QLoRA makes fine-tuning a 7B model accessible on:**
- Google Colab (free T4 — 16GB VRAM)
- RTX 3080/3090/4090 (10–24GB VRAM)
- MacBook with Apple Silicon (using MPS backend)

For 13B+ models, you'll need more — but QLoRA still cuts requirements by ~4×.

---

## 4. Setup

```bash
pip install transformers datasets peft bitsandbytes accelerate trl
```

> - `peft` — HuggingFace library that implements LoRA/QLoRA
> - `bitsandbytes` — enables 4-bit and 8-bit quantization
> - `trl` — Transformer Reinforcement Learning; includes `SFTTrainer` for fine-tuning
> - `accelerate` — handles multi-GPU and mixed-precision training

---

## 5. Prepare Your Dataset

Your dataset must be formatted as instruction-response pairs. The most common format is **Alpaca style**:

```json
[
  {
    "instruction": "Translate this sentence to French.",
    "input": "The weather is beautiful today.",
    "output": "Le temps est magnifique aujourd'hui."
  },
  {
    "instruction": "Summarize this paragraph in one sentence.",
    "input": "Large language models have...",
    "output": "LLMs are neural networks trained on..."
  }
]
```

Or the simpler **prompt-completion style** (no separate `input` field):

```json
[
  {
    "prompt": "What is the capital of Japan?",
    "completion": "The capital of Japan is Tokyo."
  }
]
```

**Converting to a HuggingFace Dataset:**

```python
from datasets import Dataset
import json

# Load your data
with open("my_data.json") as f:
    data = json.load(f)

# Format into a single text string per example
# The model learns to continue from the prompt → completion pattern
def format_alpaca(example):
    if example.get("input"):
        text = (
            f"### Instruction:\n{example['instruction']}\n\n"
            f"### Input:\n{example['input']}\n\n"
            f"### Response:\n{example['output']}"
        )
    else:
        text = (
            f"### Instruction:\n{example['instruction']}\n\n"
            f"### Response:\n{example['output']}"
        )
    return {"text": text}

dataset = Dataset.from_list(data)
dataset = dataset.map(format_alpaca)

# Split into train and validation
dataset = dataset.train_test_split(test_size=0.1, seed=42)
train_dataset = dataset["train"]
eval_dataset  = dataset["test"]

print(f"Train: {len(train_dataset)} examples")
print(f"Eval:  {len(eval_dataset)} examples")
print("\nSample:")
print(train_dataset[0]["text"])
```

**Minimum dataset size:** 100–200 examples can produce noticeable changes. 1,000–5,000 is solid for most tasks. More is better, but quality matters more than quantity.

---

## 6. Load the Base Model with 4-bit Quantization

```python
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer, BitsAndBytesConfig

MODEL_NAME = "meta-llama/Llama-3.2-3B-Instruct"
# Other good choices:
# "mistralai/Mistral-7B-Instruct-v0.3"
# "google/gemma-2-2b-it"
# "microsoft/Phi-3-mini-4k-instruct"

# 4-bit quantization config (QLoRA)
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_quant_type="nf4",           # NF4 = best quality for 4-bit
    bnb_4bit_compute_dtype=torch.bfloat16,  # compute in bf16 for stability
    bnb_4bit_use_double_quant=True,      # quantize the quantization constants too
)

# Load model
model = AutoModelForCausalLM.from_pretrained(
    MODEL_NAME,
    quantization_config=bnb_config,
    device_map="auto",       # automatically places layers across available GPUs/CPU
    trust_remote_code=True,
)
model.config.use_cache = False               # required for gradient checkpointing
model.config.pretraining_tp = 1

# Load tokenizer
tokenizer = AutoTokenizer.from_pretrained(MODEL_NAME, trust_remote_code=True)
tokenizer.pad_token = tokenizer.eos_token   # LLaMA has no pad token by default
tokenizer.padding_side = "right"            # pad on the right for training

print(f"Model loaded. Parameters: {model.num_parameters():,}")
```

---

## 7. Configure LoRA

```python
from peft import LoraConfig, get_peft_model, prepare_model_for_kbit_training

# Prepare model for QLoRA training
# (casts layer norms to fp32, enables gradient checkpointing)
model = prepare_model_for_kbit_training(model)

# LoRA configuration
lora_config = LoraConfig(
    r=16,                   # Rank — higher = more capacity, more memory
                            # Start with 16. Try 8 for smaller models, 32 for complex tasks.

    lora_alpha=32,          # Scaling factor. Rule of thumb: alpha = 2 × r
                            # Controls the magnitude of LoRA updates.

    target_modules=[        # Which weight matrices to apply LoRA to
        "q_proj",           # Query projection in attention
        "k_proj",           # Key projection
        "v_proj",           # Value projection
        "o_proj",           # Output projection
        "gate_proj",        # MLP layers (for LLaMA-style models)
        "up_proj",
        "down_proj",
    ],

    lora_dropout=0.05,      # Dropout for regularisation. 0.05–0.1 is typical.
    bias="none",            # Don't train bias terms
    task_type="CAUSAL_LM",  # Causal language modeling (text generation)
)

model = get_peft_model(model, lora_config)
model.print_trainable_parameters()
# → trainable params: 20,447,232 || all params: 3,232,014,336 || trainable%: 0.6326
```

> 💡 **Which modules to target?** The attention projections (`q_proj`, `v_proj`) are always a safe choice. Adding MLP layers (`gate_proj`, `up_proj`, `down_proj`) gives more capacity at the cost of more memory. Different model families use different names — check the model's config if you get an error.

---

## 8. Training

```python
from transformers import TrainingArguments
from trl import SFTTrainer

training_args = TrainingArguments(
    output_dir="./lora-output",          # where to save checkpoints

    # --- Core training settings ---
    num_train_epochs=3,                  # 1-3 epochs is usually enough for fine-tuning
    per_device_train_batch_size=4,       # reduce if you get OOM errors
    per_device_eval_batch_size=4,
    gradient_accumulation_steps=4,       # effective batch size = 4 × 4 = 16
                                         # simulate larger batches without more VRAM

    # --- Optimisation ---
    learning_rate=2e-4,                  # higher than pretraining LR — 1e-4 to 3e-4 typical
    weight_decay=0.001,
    optim="paged_adamw_32bit",           # memory-efficient AdamW for QLoRA
    lr_scheduler_type="cosine",          # cosine decay is standard
    warmup_ratio=0.03,                   # warm up for first 3% of steps

    # --- Memory & speed ---
    fp16=False,
    bf16=True,                           # bf16 is better than fp16 for training stability
    gradient_checkpointing=True,         # trade compute for memory (~30% more steps, less VRAM)
    dataloader_num_workers=4,

    # --- Logging & saving ---
    logging_steps=25,
    eval_strategy="steps",
    eval_steps=100,
    save_strategy="steps",
    save_steps=100,
    save_total_limit=3,                  # keep only the 3 most recent checkpoints
    load_best_model_at_end=True,
    report_to="none",                    # set to "wandb" if you use Weights & Biases
)

trainer = SFTTrainer(
    model=model,
    train_dataset=train_dataset,
    eval_dataset=eval_dataset,
    peft_config=lora_config,
    dataset_text_field="text",           # the field containing the formatted text
    max_seq_length=2048,                 # truncate sequences longer than this
    tokenizer=tokenizer,
    args=training_args,
    packing=False,                       # set True to pack short sequences together (faster)
)

# Train!
trainer.train()

# Save the LoRA adapter
trainer.save_model("./lora-adapter")
tokenizer.save_pretrained("./lora-adapter")
print("✓ LoRA adapter saved to ./lora-adapter")
```

---

## 9. Merge and Use Your Model

### Option A — Keep Adapter Separate (for experimentation)

```python
from peft import PeftModel
from transformers import AutoModelForCausalLM, AutoTokenizer

base_model = AutoModelForCausalLM.from_pretrained(
    MODEL_NAME,
    torch_dtype=torch.bfloat16,
    device_map="auto"
)
model = PeftModel.from_pretrained(base_model, "./lora-adapter")
tokenizer = AutoTokenizer.from_pretrained("./lora-adapter")
```

### Option B — Merge Adapter Into Base Model (for production)

```python
# Merge LoRA weights into the base model
merged_model = model.merge_and_unload()

# Save the merged model — now it's a standalone model with no adapter dependency
merged_model.save_pretrained("./merged-model", safe_serialization=True)
tokenizer.save_pretrained("./merged-model")
print("✓ Merged model saved to ./merged-model")
```

### Running Inference

```python
def generate(prompt: str, model, tokenizer, max_new_tokens: int = 256) -> str:
    formatted = f"### Instruction:\n{prompt}\n\n### Response:\n"
    inputs = tokenizer(formatted, return_tensors="pt").to(model.device)

    with torch.no_grad():
        outputs = model.generate(
            **inputs,
            max_new_tokens=max_new_tokens,
            temperature=0.7,
            do_sample=True,
            pad_token_id=tokenizer.eos_token_id,
        )

    # Decode only the newly generated tokens (not the prompt)
    new_tokens = outputs[0][inputs["input_ids"].shape[1]:]
    return tokenizer.decode(new_tokens, skip_special_tokens=True)

# Test it
print(generate("Explain what a transformer is in simple terms.", model, tokenizer))
```

---

## 10. Monitoring Training

Watch for these signals during training:

```
Good training:
  Step  25: train_loss=2.34, eval_loss=2.41  ← eval slightly higher, normal
  Step  50: train_loss=1.87, eval_loss=1.92
  Step 100: train_loss=1.45, eval_loss=1.53
  Step 200: train_loss=1.12, eval_loss=1.21  ← both decreasing, healthy

Overfitting (stop early or reduce epochs):
  Step 200: train_loss=0.45, eval_loss=1.89  ← huge gap = memorising training data

Underfitting (train longer or increase rank r):
  Step 200: train_loss=1.95, eval_loss=1.97  ← barely moved
```

---

## 11. Hyperparameter Guide

| Parameter | Default | When to increase | When to decrease |
|-----------|---------|-----------------|-----------------|
| `r` (rank) | 16 | Complex tasks, large dataset | Simple tasks, limited VRAM |
| `lora_alpha` | `2 × r` | — | Keep at `2 × r` |
| `learning_rate` | 2e-4 | Model barely learning | Loss unstable/exploding |
| `num_train_epochs` | 3 | Underfitting | Overfitting |
| `per_device_train_batch_size` | 4 | More VRAM available | OOM errors |
| `gradient_accumulation_steps` | 4 | Increase effective batch size | Training is very slow |
| `max_seq_length` | 2048 | Long documents in dataset | VRAM is limited |

---

## 12. Common Mistakes

| Mistake | Symptom | Fix |
|---------|---------|-----|
| Wrong chat template | Model outputs garbage after fine-tuning | Use the model's actual chat template, not custom formatting |
| Dataset too small | Model barely changes behavior | Minimum ~500 examples for noticeable change |
| Learning rate too high | Loss explodes (NaN) early in training | Lower to 1e-4 or add warmup steps |
| Too many epochs | Model only outputs training data verbatim | Reduce epochs; watch eval loss, stop when it rises |
| Formatting mismatch at inference | Model outputs instruction text, not response | Use exact same prompt format at inference as during training |
| Not saving tokenizer | Loading merged model fails | Always `tokenizer.save_pretrained(output_dir)` |

---

## ✅ Key Takeaways

- Full fine-tuning requires 56GB+ VRAM for a 7B model — impractical for most people
- LoRA freezes the base model and trains only small adapter matrices — typically < 1% of parameters
- QLoRA = 4-bit quantized base model + LoRA adapters — fits a 7B fine-tune in 5–8GB VRAM
- Rank `r=16` is a safe default; increase for complex tasks, decrease to save memory
- Quality of dataset matters more than size — 500 clean examples > 5,000 noisy ones
- Always watch eval loss, not just train loss — divergence means overfitting
- Merge the adapter into the base model before production deployment

---

## 🏗️ Capstone Project

Build the **[Custom Fine-Tune Project](./projects/custom-fine-tune/)**:
- Pick a task (customer support bot, code explainer, domain Q&A, etc.)
- Collect or generate 500–1,000 training examples
- Fine-tune a 3B model using QLoRA on free Colab
- Compare outputs vs the base model on 20 test prompts
- Document what improved and what didn't

---

## ➡️ What's Next

**[04 — Fine-Tuning with HuggingFace Transformers](./04-hf-fine-tuning.md)**
The full HuggingFace training loop in detail — custom callbacks, gradient clipping, mixed precision, and multi-GPU setup.

---

## 📚 Further Reading

- [QLoRA Paper (Dettmers et al., 2023)](https://arxiv.org/abs/2305.14314) — the original QLoRA paper, very readable
- [PEFT Library Docs](https://huggingface.co/docs/peft) — full LoRA configuration reference
- [TRL SFTTrainer Docs](https://huggingface.co/docs/trl/sft_trainer) — all SFTTrainer parameters
- [Axolotl](https://github.com/OpenAccess-AI-Collective/axolotl) — popular config-driven fine-tuning framework built on top of these same libraries
- [LLaMA Factory](https://github.com/hiyouga/LLaMA-Factory) — GUI + CLI for fine-tuning with zero code
