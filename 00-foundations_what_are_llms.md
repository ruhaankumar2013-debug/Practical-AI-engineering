# 01 — What Are Large Language Models?

> A plain-English introduction to what LLMs are, how they came to exist, and why they're a genuinely big deal — no math required.

✍️ Part of [LLM Dev Curriculum](./README.md) · 🟢 Beginner · ⏱️ ~20 min read

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

A **Large Language Model (LLM)** is a computer program that has read an enormous amount of text — billions of web pages, books, articles, code repositories — and learned, from all of that reading, patterns about how language works.

That's it. That's the core idea.

Everything else — the conversations, the code generation, the reasoning, the creativity — emerges from scaling that one simple task to an almost incomprehensible degree.

> 💡 **Analogy:** Imagine you read every book, article, and website ever written. After all that reading, you'd develop a deep intuition for how language works, how ideas connect, what follows what. An LLM is like that — except it processes information in a way that's fundamentally different from human reading, and it does it at scales we can barely reason about.

---

## 2. What Makes Them "Large"?

The word "large" in LLM refers to two things:

### 2a. Large Training Data
Modern LLMs are trained on datasets measured in **trillions of tokens** — essentially, most of the readable text on the internet, plus books, code, scientific papers, and more.

To put that in perspective: if you read one word per second without stopping, it would take you over **31,000 years** to read what GPT-4 was trained on.

### 2b. Large Number of Parameters
An LLM's "knowledge" lives in its **parameters** — numerical values that get adjusted during training until the model predicts text well.

| Model | Approx. Parameters |
|-------|-----------|
| GPT-2 (2019) | 1.5 billion |
| GPT-3 (2020) | 175 billion |
| LLaMA 3 70B (2024) | 70 billion |
| GPT-4 (estimated) | ~1 trillion |

These numbers stopped being intuitive a long time ago. The key takeaway: **more parameters = more capacity to store patterns from training data.**

---

## 3. How Are LLMs Different from Traditional Software?

This is important. LLMs represent a fundamentally different kind of program.

| | Traditional Software | LLM |
|--|-----------|-----|
| **How it works** | A human writes explicit rules | Rules are learned from data |
| **How you change its behavior** | Edit the code | Change the training data or the prompt |
| **Handles unexpected inputs** | Usually breaks or errors | Usually generates something reasonable |
| **Explains its reasoning** | Yes (it's in the code) | Not reliably |
| **Deterministic?** | Yes (same input → same output) | No (has randomness by default) |
| **Can it be wrong?** | Only if the code has bugs | Yes, frequently and confidently |

The last two rows are crucial. LLMs are **probabilistic** — they don't look things up, they generate plausible text. This is why they can write beautifully about a topic and still get facts wrong. They have no internal "verify this against reality" step.

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

**[Prompt Engineering Techniques](./02-prompt_engineering.md)** or **[OpenAI API Quickstart](./01-apis_quickstart.md)**
