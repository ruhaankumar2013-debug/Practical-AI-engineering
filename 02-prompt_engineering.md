# 02 — Prompt Engineering Techniques

> Prompt engineering is the skill of designing inputs that reliably produce the outputs you want. It's the highest-ROI skill in this entire curriculum — and it applies to every LLM, every task, every time.

✍️ Part of [LLM Dev Curriculum](./README.md) · 🟢 Beginner · ⏱️ ~1 hr read + exercises

---

## 🎯 What You'll Learn

- The anatomy of an effective prompt
- 10 core prompt engineering techniques with real examples
- How to structure prompts for consistent, reliable outputs
- Common failure modes and how to fix them
- A repeatable framework for iterating on prompts

## ⏱️ Prerequisites

- [What Are LLMs?](./00-foundations_what_are_llms.md) — or any basic familiarity with chatting with an LLM

---

## ✅ Key Takeaways

- A prompt has four components: Role, Context, Task, Format — most weak prompts are missing one
- Few-shot examples are the single most reliable way to control output format
- Chain-of-thought ("step by step") dramatically improves reasoning accuracy
- Negative constraints ("do NOT") are underused and highly effective
- For programmatic use, always request JSON and use `response_format` where supported
- When a prompt fails, diagnose before rewriting — the fix is usually targeted

---

## ➡️ What's Next

**[Building a RAG Pipeline End-to-End](./03-rag_pipeline.md)** or **[LoRA and QLoRA: Efficient Fine-Tuning](./04-finetuning_lora.md)**
