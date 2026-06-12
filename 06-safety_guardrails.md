# 06 — Safety, Guardrails & Content Filtering

> Shipping an LLM app without guardrails is like deploying an API with no authentication. This module covers the real threats, practical defences, and the tools to implement them — without making false promises about perfect security.

✍️ Part of [LLM Dev Curriculum](./README.md) · 🔴 Advanced · ⏱️ ~1.5 hr read + build

---

## 🎯 What You'll Learn

- What can actually go wrong in production LLM apps (with real examples)
- The four layers of a defence-in-depth guardrail system
- How to detect and block prompt injection attacks
- How to filter harmful inputs and outputs
- How to prevent PII leakage
- How to build a classifier-based moderation pipeline
- Tools and libraries used at production scale

## ⏱️ Prerequisites

- [What Are LLMs?](./00-foundations_what_are_llms.md)
- [OpenAI API Quickstart](./01-apis_quickstart.md)
- [Prompt Engineering Techniques](./02-prompt_engineering.md)
- Comfortable with Python and basic API design

---

## ✅ Key Takeaways

- Use defence-in-depth — multiple layers catch what others miss
- System prompts are your first line of defence — write them defensively
- Always check both input and output
- Log everything for monitoring and debugging
- Moderation isn't perfect — combine multiple techniques
- PII detection prevents accidental data leaks
- Rate limiting and cost controls prevent abuse

---

## ➡️ What's Next

**[Glossary](./05-glossary.md)** for quick term lookups across all modules.
