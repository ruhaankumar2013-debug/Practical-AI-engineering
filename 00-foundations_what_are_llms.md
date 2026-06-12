# 01 — What Are Large Language Models?

> A plain-English introduction to what LLMs are, how they came to exist, and why they're a genuinely big deal — no math required.

✍️ Part of [Phase 0: Foundations](./README.md) · 🟢 Beginner · ⏱️ ~20 min read

---

## 🎯 What You'll Learn

- What an LLM actually is (in plain English)
- How LLMs are different from traditional software
- The key ideas behind how they work — without the math
- What LLMs are good at, and where they fall short
- The vocabulary you'll need for everything else in this curriculum

## ⏱️ Prerequisites

None. Seriously — start here.

---

## 1. The Simple Answer

A **Large Language Model (LLM)** is a computer program that has read an enormous amount of text — billions of web pages, books, articles, code repositories — and learned, from all of that reading, how to predict what word (or token) comes next in a sequence.

That's it. That's the core idea.

Everything else — the conversations, the code generation, the reasoning, the creativity — emerges from scaling that one simple task to an almost incomprehensible degree.

> 💡 **Analogy:** Imagine you read every book, article, and website ever written. After all that reading, you'd develop a deep intuition for how language works, how ideas connect, what follows what. LLMs do something roughly like this — except they do it with billions of parameters and in a matter of weeks.

---

## 2. What Makes Them "Large"?

The word "large" in LLM refers to two things:

### 2a. Large Training Data
Modern LLMs are trained on datasets measured in **trillions of tokens** — essentially, most of the readable text on the internet, plus books, code, scientific papers, and more.

To put that in perspective: if you read one word per second without stopping, it would take you over **31,000 years** to read what GPT-4 was trained on.

### 2b. Large Number of Parameters
An LLM's "knowledge" lives in its **parameters** — numerical values that get adjusted during training until the model predicts text well.

| Model | Approx. Parameters |
|-------|--------------------|
| GPT-2 (2019) | 1.5 billion |
| GPT-3 (2020) | 175 billion |
| LLaMA 3 70B (2024) | 70 billion |
| GPT-4 (estimated) | ~1 trillion |

These numbers stopped being intuitive a long time ago. The key takeaway: **more parameters = more capacity to store patterns from training data.**

---

## 3. How Are LLMs Different from Traditional Software?

This is important. LLMs represent a fundamentally different kind of program.

| | Traditional Software | LLM |
|--|---------------------|-----|
| **How it works** | A human writes explicit rules | Rules are learned from data |
| **How you change its behavior** | Edit the code | Change the training data or the prompt |
| **Handles unexpected inputs** | Usually breaks or errors | Usually generates something reasonable |
| **Explains its reasoning** | Yes (it's in the code) | Not reliably |
| **Deterministic?** | Yes (same input → same output) | No (has randomness by default) |
| **Can it be wrong?** | Only if the code has bugs | Yes, frequently and confidently |

The last two rows are crucial. LLMs are **probabilistic** — they don't look things up, they generate plausible text. This is why they can write beautifully about a topic and still get facts wrong. They're not retrieving truth; they're pattern-matching at massive scale.

---

## 4. The Core Mechanism: Next Token Prediction

At inference time (when you're actually using a model), here's what's happening:

```
Input:  "The capital of France is"
Model:  [looks at all patterns learned during training]
Output: "Paris"  ← highest probability next token
```

Then it appends "Paris" and does it again:

```
Input:  "The capital of France is Paris"
Output: "."  ← highest probability next token
```

This continues until the model generates a stop token or reaches the maximum length.

**The entire "intelligence" of an LLM — its ability to answer questions, write code, reason through problems — is an emergent property of doing this next-token prediction extremely well, at enormous scale.**

> 💡 This is one of the most surprising results in modern AI: a model trained purely to predict the next word develops capabilities that nobody explicitly programmed into it — translation, arithmetic, coding, logical reasoning, and more.

---

## 5. A Brief History (The Key Moments)

You don't need to memorize this, but the timeline helps you understand *why* things are the way they are today.

```
1950s–2010s  Traditional NLP
             Rule-based systems, then statistical models.
             Worked, but brittle. Couldn't generalize.

2013         Word2Vec
             First broadly useful word embeddings.
             "king - man + woman ≈ queen"
             Words as vectors — a key stepping stone.

2017         "Attention Is All You Need" (Google)
             The Transformer architecture is published.
             This paper is the foundation of every modern LLM.

2018         BERT (Google) + GPT-1 (OpenAI)
             First large pretrained language models.
             Fine-tune on specific tasks → huge improvements.

2020         GPT-3 (OpenAI)
             175B parameters. In-context learning.
             The first model that felt genuinely general-purpose.
             The world started paying attention.

2022         ChatGPT (OpenAI)
             GPT-3.5 with RLHF (human feedback training).
             100 million users in 2 months.
             LLMs go mainstream.

2023         GPT-4, Claude, LLaMA, Mistral, Gemini
             The field explodes. Open-weight models arrive.
             Multimodality (text + images).

2024–2025    Reasoning models, long context, agents
             o1, o3, Claude 3.5/3.7, Gemini 2.0, DeepSeek R1.
             LLMs that can "think before they answer."
             Context windows reach 1M+ tokens.
```

---

## 6. What LLMs Are Good At

After training on essentially all human writing, LLMs are genuinely excellent at:

- **Language tasks** — writing, editing, summarizing, translating, paraphrasing
- **Code** — generating, explaining, debugging, converting between languages
- **Reasoning** — working through problems step by step (with the right prompting)
- **Extraction** — pulling structured info out of unstructured text
- **Classification** — labeling text into categories
- **Question answering** — on topics well-represented in training data
- **Brainstorming** — generating ideas, variations, alternatives
- **Instruction following** — doing what you ask, in the format you ask

---

## 7. Where LLMs Fall Short

Just as important — knowing where they break:

| Limitation | Why it happens |
|------------|---------------|
| **Hallucinations** | They generate plausible text, not verified facts. There's no internal "check if this is true" step. |
| **Knowledge cutoff** | Training data has a cutoff date. They don't know what happened after that unless you tell them. |
| **Math & precise counting** | They process tokens, not numbers. Simple arithmetic can fail. Use code or tools for this. |
| **Long-term memory** | By default, each conversation starts fresh. They don't remember past sessions. |
| **Consistency** | Outputs are probabilistic. Ask the same question twice and you may get different answers. |
| **Reasoning limits** | They can seem like they're reasoning, but can fail on problems that require formal logic or planning. |
| **Private/real-time data** | They only know what was in their training data. Not your documents, not today's news (without tools). |

> ⚠️ **Important:** LLMs are often confidently wrong. They don't have an internal uncertainty meter that prevents them from making things up. This isn't a bug about to be fixed — it's a property of how they work. Always verify critical facts.

---

## 8. Key Vocabulary

These terms will appear throughout this curriculum. Get comfortable with them now.

| Term | Definition |
|------|-----------|
| **Token** | The basic unit an LLM works with. Roughly 0.75 words. "Hello" = 1 token. "Tokenization" = 3 tokens. |
| **Context window** | The maximum amount of text an LLM can "see" at once. Measured in tokens. |
| **Prompt** | The input you give to an LLM. |
| **Completion / Response** | The output an LLM generates. |
| **Parameters** | The numerical values in a model that encode everything it learned during training. |
| **Inference** | Running the model to generate output (as opposed to training). |
| **Training** | The process of adjusting parameters by showing the model data. Expensive. Done once. |
| **Fine-tuning** | Further training a pretrained model on a specific dataset to specialize it. |
| **Temperature** | A setting that controls randomness. Low = focused/deterministic. High = creative/varied. |
| **Hallucination** | When an LLM confidently generates false information. |
| **Embedding** | A numerical representation of text as a vector. Used in search, RAG, and similarity tasks. |
| **RLHF** | Reinforcement Learning from Human Feedback. How models are trained to be helpful and safe. |
| **Open-weight model** | A model whose parameters are publicly available (e.g. LLaMA, Mistral). |
| **Closed model** | A model only accessible via API (e.g. GPT-4, Claude). |

---

## 9. The Major Players (as of 2025)

| Organization | Models | Access |
|-------------|--------|--------|
| **OpenAI** | GPT-4o, o1, o3 | API + ChatGPT |
| **Anthropic** | Claude 3.5, Claude 3.7 | API + Claude.ai |
| **Google** | Gemini 2.0, Gemma | API + AI Studio |
| **Meta** | LLaMA 3, LLaMA 3.1 | Open-weight (free) |
| **Mistral AI** | Mistral, Mixtral | API + open-weight |
| **DeepSeek** | DeepSeek R1, V3 | API + open-weight |
| **Cohere** | Command R+ | API (enterprise focus) |

> 📌 This landscape changes fast. For the most current rankings, see [LMSYS Chatbot Arena](https://chat.lmsys.org) and [HuggingFace Open LLM Leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard).

---

## 💻 Try It: Your First LLM API Call

Let's make this concrete. Here's the simplest possible LLM call in Python:

```python
# pip install openai
from openai import OpenAI

client = OpenAI(api_key="YOUR_API_KEY_HERE")

response = client.chat.completions.create(
    model="gpt-4o-mini",        # cheap and fast — good for learning
    messages=[
        {
            "role": "user",
            "content": "In one sentence, what is a large language model?"
        }
    ]
)

print(response.choices[0].message.content)
```

That's it. One API call, one response. Everything else in this curriculum is built on top of this.

> 🔑 **Get an API key:** [platform.openai.com](https://platform.openai.com) → API Keys. New accounts get free credits.

---

## ✅ Key Takeaways

- An LLM is a model trained to predict the next token in a sequence — at massive scale
- "Large" means large training data + large number of parameters
- LLMs are probabilistic, not rule-based — they generate plausible text, not verified facts
- They're exceptional at language, code, reasoning, and extraction
- They hallucinate, have knowledge cutoffs, and don't have persistent memory by default
- The field moves fast — models from 6 months ago are already outdated

---

## ➡️ What's Next

**[02 — How Transformers Work (Intuitively)](./02-transformers-intuition.md)**
We'll look inside the architecture that makes all of this possible — without the matrix math.

---

## 📚 Further Reading

- [Andrej Karpathy — Intro to Large Language Models](https://www.youtube.com/watch?v=zjkBMFhNj_g) — the best 1-hour introduction that exists
- [Attention Is All You Need](https://arxiv.org/abs/1706.03762) — the original Transformer paper (2017)
- [Language Models are Few-Shot Learners (GPT-3 paper)](https://arxiv.org/abs/2005.14165) — the paper that changed everything
- [Simon Willison's LLM Glossary](https://simonwillison.net/2023/May/31/llm-glossary/) — community-maintained definitions
