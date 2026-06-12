# 03 Building a RAG Pipeline End-to-End

> RAG (Retrieval-Augmented Generation) is how you give an LLM access to your own data — documents, databases, wikis, anything. This module walks you through building a complete, working RAG pipeline.

✍️ Part of [LLM Dev Curriculum](./README.md) · 🟡 Intermediate · ⏱️ ~2 hr read + build

---

## 🎯 What You'll Learn

- How all the pieces of a RAG pipeline fit together
- How to load, clean, and chunk documents
- How to create and store embeddings in a vector database
- How to retrieve relevant context and inject it into a prompt
- How to evaluate whether your RAG pipeline is working
- A complete, runnable implementation you can adapt immediately

## ⏱️ Prerequisites

- [What Are LLMs?](./00-foundations_what_are_llms.md)
- [Prompt Engineering Techniques](./02-prompt_engineering.md)
- [OpenAI API Quickstart](./01-apis_quickstart.md)
- Basic Python. Familiarity with the OpenAI API.

---

## ✅ Key Takeaways

- RAG has two pipelines: **indexing** (offline) and **query** (real-time) — keep them separate
- Chunk quality is the single biggest factor in RAG performance — bad chunks = bad answers
- Measure in tokens, not characters — LLMs care about tokens
- Low temperature (0.1) is best for factual Q&A tasks
- Always evaluate retrieval quality before blaming the LLM for bad answers
- The system prompt's grounding instruction ("answer only from context") is what prevents hallucination

---

## ➡️ What's Next

**[LoRA and QLoRA: Efficient Fine-Tuning](./04-finetuning_lora.md)** or **[Safety, Guardrails & Content Filtering](./06-safety_guardrails.md)**
