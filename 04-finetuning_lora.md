# 04 — LoRA and QLoRA: Efficient Fine-Tuning

> Fine-tuning a 7B model used to require 8× A100 GPUs and thousands of dollars. LoRA changed that. Today you can fine-tune on a single consumer GPU — or even for free on Google Colab.

✍️ Part of [LLM Dev Curriculum](./README.md) · 🟡 Intermediate · ⏱️ ~3 hr read + build

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

- [What Are LLMs?](./00-foundations_what_are_llms.md)
- [Prompt Engineering Techniques](./02-prompt_engineering.md)
- Basic Python. Familiarity with HuggingFace Transformers is helpful but not required.
- For QLoRA: a GPU with at least 8GB VRAM (or free Google Colab T4)

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

## ➡️ What's Next

**[Building a RAG Pipeline End-to-End](./03-rag_pipeline.md)** or **[Safety, Guardrails & Content Filtering](./06-safety_guardrails.md)**
