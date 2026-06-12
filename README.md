<div align="center">

# 🧠 LLM Dev Curriculum

### The open-source, community-driven roadmap to building with Large Language Models —
### from your first "Hello, GPT" to shipping production AI systems.

[![Stars](https://img.shields.io/github/stars/ruhaankumar2013-debug/Practical-AI-engineering?style=for-the-badge&color=yellow)](https://github.com/your-org/llm-dev-curriculum/stargazers)
[![Contributors](https://img.shields.io/github/contributors/ruhaankumar2013-debug/Practical-AI-engineering?style=for-the-badge&color=blue)](https://github.com/ruhaankumar2013-debug/Practical-AI-engineering/graphs/contributors)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](./LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=for-the-badge)](./CONTRIBUTING.md)
[![Last Updated](https://img.shields.io/badge/updated-2026-orange?style=for-the-badge)](./ROADMAP.md)

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
|----|-------------|
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
Start → 00-foundations/01-what-are-llms.md
```

### 🟢 "I know ML / data science, new to LLMs"
```
Start → 01-prompting/01-prompt-basics.md
```

### 🟢 "I'm a developer, just want to build an app NOW"
```
Start → 02-apis/01-openai-quickstart.md
```

### 🟡 "I want to connect LLMs to my own data"
```
Start → 03-rag/01-what-is-rag.md
```

### 🟡 "I want to customize / fine-tune a model"
```
Start → 04-fine-tuning/01-when-to-fine-tune.md
```

### 🔴 "I need to ship this to production"
```
Start → 07-production/01-deployment-options.md
```

---

## 📚 Modules

> **Legend:** 📖 Guide · 🧪 Notebook · 💻 Code · 🏗️ Project · 🟢 Beginner · 🟡 Intermediate · 🔴 Advanced

---

### 🟢 Phase 0 — Foundations
> *No prerequisites. If you're brand new, start here.*

| # | Module | Format | Est. Time |
|---|--------|--------|-----------|
| 0.1 | [What Are LLMs?](./00-00-foundations_what_are_llms.md /01-what-are-llms.md) | 📖 | 20 min |
| 0.2 | [How Transformers Work (Intuitively)](./00-foundations/02-transformers-intuition.md) | 📖 + visuals | 45 min |
| 0.3 | [Tokens, Embeddings & Context Windows](./00-foundations/03-tokens-and-embeddings.md) | 📖 + 🧪 | 30 min |
| 0.4 | [The LLM Landscape: Models, Labs & Trends](./00-foundations/04-llm-landscape.md) | 📖 | 20 min |
| 0.5 | [Setting Up Your Dev Environment](./00-foundations/05-dev-environment.md) | 💻 | 30 min |
| | 🏗️ **Project:** [Build a Token Counter](./00-foundations/projects/token-counter/) | 💻 | ~1 hr |

---

### 🟢 Phase 1 — Prompting
> *Learn to communicate with LLMs effectively. The highest ROI skill in this entire curriculum.*

| # | Module | Format | Est. Time |
|---|--------|--------|-----------|
| 1.1 | [Prompt Basics: Instructions, Context, Output](./01-prompting/01-prompt-basics.md) | 📖 | 30 min |
| 1.2 | [Prompt Engineering Techniques](./01-prompting/02-prompt-engineering.md) | 📖 + examples | 1 hr |
| 1.3 | [Chain-of-Thought & Reasoning Prompts](./01-prompting/03-chain-of-thought.md) | 📖 + 🧪 | 45 min |
| 1.4 | [System Prompts, Roles & Personas](./01-prompting/04-system-prompts.md) | 📖 | 30 min |
| 1.5 | [Few-Shot & Zero-Shot Learning](./01-prompting/05-few-shot-zero-shot.md) | 📖 + 🧪 | 45 min |
| 1.6 | [Prompt Anti-Patterns (What Not to Do)](./01-prompting/06-prompt-antipatterns.md) | 📖 | 20 min |
| | 🏗️ **Project:** [Build a Prompt Playground](./01-prompting/projects/prompt-playground/) | 💻 | ~2 hr |

---

### 🟢 Phase 2 — APIs & Integrations
> *Call LLMs from your code. Build your first real application.*

| # | Module | Format | Est. Time |
|---|--------|--------|-----------|
| 2.1 | [OpenAI API Quickstart](./02-apis/01-openai-quickstart.md) | 📖 + 💻 | 1 hr |
| 2.2 | [Anthropic (Claude) API](./02-apis/02-anthropic-api.md) | 📖 + 💻 | 1 hr |
| 2.3 | [HuggingFace & Open-Weight Models](./02-apis/03-huggingface.md) | 📖 + 🧪 | 1.5 hr |
| 2.4 | [LangChain Basics](./02-apis/04-langchain.md) | 📖 + 🧪 | 2 hr |
| 2.5 | [Streaming Responses & Async Calls](./02-apis/05-streaming.md) | 📖 + 💻 | 1 hr |
| 2.6 | [Structured Outputs & JSON Mode](./02-apis/06-structured-outputs.md) | 📖 + 💻 | 1 hr |
| 2.7 | [Managing API Costs from Day One](./02-apis/07-cost-management.md) | 📖 | 30 min |
| | 🏗️ **Project:** [Build a CLI Chatbot](./02-apis/projects/cli-chatbot/) | 💻 | ~3 hr |

---

### 🟡 Phase 3 — RAG (Retrieval-Augmented Generation)
> *Give your LLM a memory. Connect it to your own documents and data.*

| # | Module | Format | Est. Time |
|---|--------|--------|-----------|
| 3.1 | [What Is RAG and Why It Matters](./03-rag/01-what-is-rag.md) | 📖 | 30 min |
| 3.2 | [Vector Databases Explained](./03-rag/02-vector-databases.md) | 📖 + 🧪 | 1.5 hr |
| 3.3 | [Chunking Strategies for Documents](./03-rag/03-chunking.md) | 📖 + 💻 | 1 hr |
| 3.4 | [Embeddings Deep Dive](./03-rag/04-embeddings-deep-dive.md) | 📖 + 🧪 | 1.5 hr |
| 3.5 | [Building a RAG Pipeline End-to-End](./03-rag/05-rag-pipeline.md) | 🧪 | 2 hr |
| 3.6 | [Advanced RAG: Reranking, HyDE, Query Expansion](./03-rag/06-advanced-rag.md) | 📖 + 🧪 | 2 hr |
| | 🏗️ **Project:** [Chat with Your Documents](./03-rag/projects/doc-chatbot/) | 💻 | ~4 hr |

---

### 🟡 Phase 4 — Fine-Tuning
> *When prompting isn't enough — adapt a model to your exact task.*

| # | Module | Format | Est. Time |
|---|--------|--------|-----------|
| 4.1 | [When to Fine-Tune vs. Prompt Engineer](./04-fine-tuning/01-when-to-fine-tune.md) | 📖 | 30 min |
| 4.2 | [Dataset Preparation & Formatting](./04-fine-tuning/02-dataset-prep.md) | 📖 + 💻 | 2 hr |
| 4.3 | [LoRA and QLoRA: Efficient Fine-Tuning](./04-fine-tuning/03-lora-qlora.md) | 📖 + 🧪 | 3 hr |
| 4.4 | [Fine-Tuning with HuggingFace Transformers](./04-fine-tuning/04-hf-fine-tuning.md) | 🧪 | 3 hr |
| 4.5 | [OpenAI Fine-Tuning API](./04-fine-tuning/05-openai-fine-tuning.md) | 📖 + 💻 | 1.5 hr |
| 4.6 | [RLHF & Preference Tuning Basics](./04-fine-tuning/06-rlhf-basics.md) | 📖 | 1 hr |
| | 🏗️ **Project:** [Fine-Tune a Model on Custom Data](./04-fine-tuning/projects/custom-fine-tune/) | 💻 + 🧪 | ~6 hr |

---

### 🟡 Phase 5 — Agents
> *Build AI systems that can reason, plan, use tools, and take actions.*

| # | Module | Format | Est. Time |
|---|--------|--------|-----------|
| 5.1 | [What Are LLM Agents?](./05-agents/01-what-are-agents.md) | 📖 | 30 min |
| 5.2 | [Tool Use & Function Calling](./05-agents/02-tool-use.md) | 📖 + 💻 | 2 hr |
| 5.3 | [The ReAct Pattern](./05-agents/03-react-pattern.md) | 📖 + 🧪 | 1.5 hr |
| 5.4 | [Memory in Agents: Short & Long Term](./05-agents/04-agent-memory.md) | 📖 + 💻 | 1.5 hr |
| 5.5 | [Multi-Agent Systems](./05-agents/05-multi-agent.md) | 📖 + 🧪 | 2 hr |
| 5.6 | [Agent Frameworks: LangGraph, CrewAI, AutoGen](./05-agents/06-agent-frameworks.md) | 🧪 | 3 hr |
| | 🏗️ **Project:** [Build a Research Agent](./05-agents/projects/research-agent/) | 💻 | ~5 hr |

---

### 🟡 Phase 6 — Evaluation
> *You can't improve what you can't measure. Evals are the unsexy superpower.*

| # | Module | Format | Est. Time |
|---|--------|--------|-----------|
| 6.1 | [Why LLM Evals Are Hard](./06-evaluation/01-evals-intro.md) | 📖 | 30 min |
| 6.2 | [Types of Evals: Unit, Integration, Red-Team](./06-evaluation/02-eval-types.md) | 📖 | 45 min |
| 6.3 | [LLM-as-Judge: Using AI to Evaluate AI](./06-evaluation/03-llm-as-judge.md) | 📖 + 💻 | 1.5 hr |
| 6.4 | [Benchmarks & Public Leaderboards](./06-evaluation/04-benchmarks.md) | 📖 | 30 min |
| 6.5 | [Building an Eval Pipeline with Promptfoo](./06-evaluation/05-eval-pipelines.md) | 🧪 | 2 hr |
| | 🏗️ **Project:** [Eval Dashboard for Your App](./06-evaluation/projects/eval-dashboard/) | 💻 | ~3 hr |

---

### 🔴 Phase 7 — Production
> *Ship it. Scale it. Keep it working. Don't go broke.*

| # | Module | Format | Est. Time |
|---|--------|--------|-----------|
| 7.1 | [Deployment Options Overview](./07-production/01-deployment-options.md) | 📖 | 45 min |
| 7.2 | [Building a REST API for Your LLM App](./07-production/02-rest-api.md) | 📖 + 💻 | 3 hr |
| 7.3 | [Caching Strategies for Latency & Cost](./07-production/03-caching.md) | 📖 + 💻 | 2 hr |
| 7.4 | [Prompt Optimization & Cost Management](./07-production/04-cost-optimization.md) | 📖 | 1 hr |
| 7.5 | [Observability: Logging, Tracing & Monitoring](./07-production/05-monitoring.md) | 📖 + 💻 | 2 hr |
| 7.6 | [Safety, Guardrails & Content Filtering](./07-production/06-safety-guardrails.md) | 📖 + 💻 | 1.5 hr |
| 7.7 | [Rate Limiting & Abuse Prevention](./07-production/07-rate-limiting.md) | 📖 + 💻 | 1 hr |
| | 🏗️ **Project:** [Deploy a Full-Stack LLM App](./07-production/projects/full-stack-llm-app/) | 💻 | ~8 hr |

---

### 🔴 Phase 8 — Advanced Topics
> *For those who want to go deep. Read papers. Break things. Contribute upstream.*

| # | Module | Format | Est. Time |
|---|--------|--------|-----------|
| 8.1 | [Training a Small LLM from Scratch](./08-advanced/01-training-from-scratch.md) | 📖 + 🧪 | 5 hr |
| 8.2 | [Multimodal Models: Vision, Audio & More](./08-advanced/02-multimodal.md) | 📖 + 🧪 | 3 hr |
| 8.3 | [Mixture of Experts (MoE) Architecture](./08-advanced/03-moe.md) | 📖 | 2 hr |
| 8.4 | [Speculative Decoding & Inference Optimization](./08-advanced/04-inference-optimization.md) | 📖 | 2 hr |
| 8.5 | [Key Research Papers (Annotated)](./08-advanced/05-papers.md) | 📖 reading list | Ongoing |
| 8.6 | [Contributing to Open-Source LLMs](./08-advanced/06-open-source-contribution.md) | 📖 | 1 hr |

---

## 🏗️ Projects Summary

Each phase ends with a capstone project you can actually put on your portfolio.

| Phase | Project | Skills Demonstrated |
|-------|---------|---------------------|
| 0 | Token Counter | Tokenization, HuggingFace tokenizers |
| 1 | Prompt Playground | Prompt engineering, UI basics |
| 2 | CLI Chatbot | API calls, conversation history, streaming |
| 3 | Chat with Your Docs | RAG, vector DB, embeddings |
| 4 | Custom Fine-Tune | Dataset prep, LoRA, model training |
| 5 | Research Agent | Tool use, planning, multi-step reasoning |
| 6 | Eval Dashboard | Evals, LLM-as-judge, metrics |
| 7 | Full-Stack LLM App | Deployment, monitoring, guardrails |

---

## 🗂️ Repository Structure

```
llm-dev-curriculum/
├── 📁 00-foundations/
│   ├── README.md
│   ├── 01-what-are-llms.md
│   ├── 02-transformers-intuition.md
│   ├── 03-tokens-and-embeddings.md
│   ├── 04-llm-landscape.md
│   ├── 05-dev-environment.md
│   └── projects/token-counter/
├── 📁 01-prompting/
├── 📁 02-apis/
├── 📁 03-rag/
├── 📁 04-fine-tuning/
├── 📁 05-agents/
├── 📁 06-evaluation/
├── 📁 07-production/
├── 📁 08-advanced/
├── 📁 resources/
│   ├── glossary.md           ← definitions of every key term
│   ├── tools-and-libraries.md
│   └── further-reading.md
├── README.md                 ← you are here
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
└── ROADMAP.md
```

---

## 🔖 Format Legend

| Icon | Meaning |
|------|---------|
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

Please read [CONTRIBUTING.md](./CONTRIBUTING.md) before submitting. Be kind.

---

## 🌍 Translations

| Language | Status | Maintainer |
|----------|--------|------------|
| 🇬🇧 English | ✅ Complete | Core Team |
| 🇮🇳 Hindi | 🚧 In Progress | [Contribute →](./CONTRIBUTING.md) |
| 🇪🇸 Spanish | 🚧 In Progress | [Contribute →](./CONTRIBUTING.md) |
| 🇵🇹 Portuguese | 🙋 Needed | [Contribute →](./CONTRIBUTING.md) |
| 🇨🇳 Chinese (Simplified) | 🙋 Needed | [Contribute →](./CONTRIBUTING.md) |
| 🇯🇵 Japanese | 🙋 Needed | [Contribute →](./CONTRIBUTING.md) |

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

Built with ❤️ by the community · [Contribute](./CONTRIBUTING.md) · [Star it ⭐](https://github.com/your-org/llm-dev-curriculum)

*This curriculum is updated regularly. Watch the repo to stay notified.*

</div>
