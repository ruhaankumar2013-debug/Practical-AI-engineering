<div align="center">

# 🧠 LLM Dev Curriculum

### The open-source, community-driven roadmap to building with Large Language Models —
### from your first "Hello, GPT" to shipping production AI systems.

[![Stars](https://img.shields.io/github/stars/ruhaankumar2013-debug/Practical-AI-engineering?style=for-the-badge&color=yellow)](https://github.com/ruhaankumar2013-debug/Practical-AI-engineering/stargazers)
[![Contributors](https://img.shields.io/github/contributors/ruhaankumar2013-debug/Practical-AI-engineering?style=for-the-badge&color=blue)](https://github.com/ruhaankumar2013-debug/Practical-AI-engineering/graphs/contributors)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](./LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=for-the-badge)](./contributing.md)
[![Last Updated](https://img.shields.io/badge/updated-2026-orange?style=for-the-badge)](./roadmap.md)

**⭐ If this helped you, star it — it helps others find it too.**

[🚀 Start Learning](#-quick-start) · [📚 All Modules](#-modules) · [🏗️ Projects](#-projects) · [🤝 Contribute](#-contributing) · [💬 Community](#-community)

</div>

---

## 🌟 Why This Curriculum?

There are hundreds of LLM tutorials out there. Most are either:

- 🔴 Too shallow — "just call the API and you're done"
- 🔴 Too academic — dense math, no runnable code
- 🔴 Too opinionated — locked into one framework
- 🔴 Outdated — written in 2023 and never touched since

**This curriculum is different.** It's:

| ✅ | What you get |
|----|-----------|
| **Beginner → Production** | Start from zero ML knowledge. End with a deployed, monitored AI system. |
| **Framework-agnostic** | OpenAI, Anthropic, HuggingFace, open-weight models — you learn all of them. |
| **Hands-on first** | Every concept has code. Every phase has a capstone project you can put on your portfolio. |
| **Community-maintained** | Updated by practitioners, not just educators. PRs welcome. |
| **Clearly leveled** | Every module is tagged 🟢 Beginner · 🟡 Intermediate · 🔴 Advanced — no surprises. |

---

## 🗺️ The Big Picture

```
YOU ARE HERE
     │
     ▼
┌──────────────────────────────────────────────────────────────┐
│  PHASE 0 · Foundations                    🟢 No prerequisites │
│  What LLMs are, how transformers work, tokens & embeddings   │
└───────────────────────────┬──────────────────────────────────┘
                            │
                            ▼
┌──────────────────────────────────────────────────────────────┐
│  PHASE 1 · Prompting                           🟢 Beginner   │
│  Prompt engineering, CoT, few-shot, system prompts           │
└───────────────────────────┬──────────────────────────────────┘
                            │
                            ▼
┌──────────────────────────────────────────────────────────────┐
│  PHASE 2 · APIs & Integrations                 🟢 Beginner   │
│  Call LLMs from code. Build your first real app.             │
└───────────┬───────────────┴───────────────┬──────────────────┘
            │                               │
            ▼                               ▼
┌───────────────────────┐       ┌───────────────────────────┐
│  PHASE 3 · RAG  🟡    │       │  PHASE 4 · Fine-Tuning 🟡 │
│  Retrieval-Augmented  │       │  LoRA, QLoRA, RLHF,       │
│  Generation, vector   │       │  dataset prep             │
│  DBs, chunking        │       └───────────┬───────────────┘
└───────────┬───────────┘                   │
            └───────────────┬───────────────┘
                            │
                            ▼
┌──────────────────────────────────────────────────────────────┐
│  PHASE 5 · Agents                         🟡 Intermediate    │
│  Tool use, ReAct, memory, multi-agent systems                │
└───────────────────────────┬──────────────────────────────────┘
                            │
                            ▼
┌──────────────────────────────────────────────────────────────┐
│  PHASE 6 · Evaluation                     🟡 Intermediate    │
│  Evals, benchmarks, LLM-as-judge, eval pipelines            │
└───────────────────────────┬──────────────────────────────────┘
                            │
                            ▼
┌──────────────────────────────────────────────────────────────┐
│  PHASE 7 · Production                          🔴 Advanced   │
│  Deploy, scale, monitor, optimize, guardrails                │
└───────────────────────────┬──────────────────────────────────┘
                            │
                            ▼
┌──────────────────────────────────────────────────────────────┐
│  PHASE 8 · Advanced Topics                     🔴 Advanced   │
│  Train from scratch, multimodal, MoE, research papers        │
└──────────────────────────────────────────────────────────────┘
```

---

## 🏁 Quick Start

Not sure where to begin? Pick your entry point:

### 🟢 "I know nothing about LLMs or ML"
```
Start → 00-foundations_what_are_llms.md
```

### 🟢 "I know ML / data science, new to LLMs"
```
Start → 02-prompt_engineering.md
```

### 🟢 "I'm a developer, just want to build an app NOW"
```
Start → 01-apis_quickstart.md
```

### 🟡 "I want to connect LLMs to my own data"
```
Start → 03-rag_pipeline.md
```

### 🟡 "I want to customize / fine-tune a model"
```
Start → 04-finetuning_lora.md
```

### 🔴 "I need to ship this to production"
```
Start → 06-safety_guardrails.md
```

---

## 📚 Modules

> **Legend:** 📖 Guide · 🧪 Notebook · 💻 Code · 🏗️ Project · 🟢 Beginner · 🟡 Intermediate · 🔴 Advanced

---

### 🟢 Phase 0 — Foundations
> *No prerequisites. If you're brand new, start here.*

| # | Module | Format | Est. Time |
|---|--------|--------|----------|
| 0.1 | [What Are LLMs?](./00-foundations_what_are_llms.md) | 📖 | 20 min |

---

### 🟢 Phase 1 — Prompting
> *Learn to communicate with LLMs effectively. The highest ROI skill in this entire curriculum.*

| # | Module | Format | Est. Time |
|---|--------|--------|----------|
| 1.2 | [Prompt Engineering Techniques](./02-prompt_engineering.md) | 📖 + examples | 1 hr |

---

### 🟢 Phase 2 — APIs & Integrations
> *Call LLMs from your code. Build your first real application.*

| # | Module | Format | Est. Time |
|---|--------|--------|----------|
| 2.1 | [OpenAI API Quickstart](./01-apis_quickstart.md) | 📖 + 💻 | 1 hr |

---

### 🟡 Phase 3 — RAG (Retrieval-Augmented Generation)
> *Give your LLM a memory. Connect it to your own documents and data.*

| # | Module | Format | Est. Time |
|---|--------|--------|----------|
| 3.5 | [Building a RAG Pipeline End-to-End](./03-rag_pipeline.md) | 📖 + 🧪 | 2 hr |

---

### 🟡 Phase 4 — Fine-Tuning
> *When prompting isn't enough — adapt a model to your exact task.*

| # | Module | Format | Est. Time |
|---|--------|--------|----------|
| 4.3 | [LoRA and QLoRA: Efficient Fine-Tuning](./04-finetuning_lora.md) | 📖 + 🧪 | 3 hr |

---

### 🔴 Phase 7 — Production
> *Ship it. Scale it. Keep it working. Don't go broke.*

| # | Module | Format | Est. Time |
|---|--------|--------|----------|
| 7.6 | [Safety, Guardrails & Content Filtering](./06-safety_guardrails.md) | 📖 + 💻 | 1.5 hr |

---

## 📖 Additional Resources

| Resource | Link |
|----------|------|
| 📚 **Glossary** | [05-glossary.md](./05-glossary.md) |

---

## 🏗️ Projects Summary

Each phase ends with a capstone project you can actually put on your portfolio.

| Phase | Project | Skills Demonstrated |
|-------|---------|--------------------|
| 0 | Foundations | LLM basics |
| 1 | Prompt Playground | Prompt engineering |
| 2 | CLI Chatbot | API calls, conversation history |
| 3 | Chat with Your Docs | RAG, vector DB, embeddings |
| 4 | Custom Fine-Tune | Dataset prep, LoRA, model training |
| 6 | Safety Implementation | Guardrails, content filtering |

---

## 🗂️ Repository Structure

```
Practical-AI-engineering/
├── 00-foundations_what_are_llms.md
├── 01-apis_quickstart.md
├── 02-prompt_engineering.md
├── 03-rag_pipeline.md
├── 04-finetuning_lora.md
├── 05-glossary.md
├── 06-safety_guardrails.md
├── README.md                 ← you are here
├── contributing.md
├── roadmap.md
└── LICENSE
```

---

## 🔖 Format Legend

| Icon | Meaning |
|------|----------|
| 📖 | Written guide (Markdown) |
| 🧪 | Jupyter Notebook (runnable) |
| 💻 | Standalone code / project |
| 🏗️ | Capstone project |
| 🟢 | Beginner — no prior ML knowledge needed |
| 🟡 | Intermediate — comfortable with Python and APIs |
| 🔴 | Advanced — ML background helpful |

---

## 🤝 Contributing

This curriculum is **community-powered**. All contributions welcome — big or small.

- **Typo / small fix** → open a PR directly
- **Improve an explanation** → open a PR with a brief note on what was unclear
- **Add a new module** → open an issue first so we can align on scope
- **Translate to your language** → see the Translations section

Please read [contributing.md](./contributing.md) before submitting. Be kind.

---

## 🌍 Translations

| Language | Status | Maintainer |
|----------|--------|----------|
| 🇬🇧 English | ✅ Complete | Core Team |
| 🇮🇳 Hindi | 🚧 In Progress | [Contribute →](./contributing.md) |
| 🇪🇸 Spanish | 🚧 In Progress | [Contribute →](./contributing.md) |
| 🇵🇹 Portuguese | 🙋 Needed | [Contribute →](./contributing.md) |
| 🇨🇳 Chinese (Simplified) | 🙋 Needed | [Contribute →](./contributing.md) |
| 🇯🇵 Japanese | 🙋 Needed | [Contribute →](./contributing.md) |

---

## 💬 Community

- **GitHub Discussions** — questions, study groups, project showcases
- **Issues** — bug reports, content errors, suggestions

---

## 📜 License

**MIT** — free to use, share, fork, and build on.  
Attribution appreciated but not required.

---

<div align="center">

Built with ❤️ by the community · [Contribute](./contributing.md) · [Star it ⭐](https://github.com/ruhaankumar2013-debug/Practical-AI-engineering)

*This curriculum is updated regularly. Watch the repo to stay notified.*

</div>
