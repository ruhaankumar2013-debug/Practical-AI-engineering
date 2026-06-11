# 🤝 Contributing to LLM Dev Curriculum

First off — **thank you**. This curriculum exists because of contributors like you.

Whether you're fixing a typo, improving an explanation, translating content, or building a whole new module — every contribution matters. This guide tells you how.

---

## 📋 Table of Contents

- [Code of Conduct](#-code-of-conduct)
- [Ways to Contribute](#-ways-to-contribute)
- [Before You Start](#-before-you-start)
- [Setting Up Locally](#-setting-up-locally)
- [Making a Contribution](#-making-a-contribution)
- [Writing Style Guide](#-writing-style-guide)
- [Module Format Guide](#-module-format-guide)
- [Pull Request Process](#-pull-request-process)
- [Reporting Issues](#-reporting-issues)
- [Translations](#-translations)
- [Recognition](#-recognition)

---

## 🧭 Code of Conduct

This project follows a simple rule: **be kind**.

- Assume good intent
- Critique content, not people
- Help beginners — everyone starts somewhere
- English is not everyone's first language — be patient

Persistent bad behavior → removal from the project. No exceptions.

---

## 🛠️ Ways to Contribute

You don't need to be an LLM expert to contribute. Here's what we need most:

| Type | Examples | Difficulty |
|------|---------|------------|
| **Fix** | Typos, broken links, outdated model names | 🟢 Easy |
| **Clarify** | Rewrite a confusing explanation | 🟢 Easy |
| **Add examples** | Add a code snippet, a use case, a diagram | 🟢 Easy |
| **Review** | Review open PRs for accuracy | 🟡 Medium |
| **New section** | Add a missing subsection to an existing module | 🟡 Medium |
| **New module** | Write a full new lesson (see Module Format Guide) | 🔴 Hard |
| **Translate** | Translate existing content to another language | 🟡 Medium |
| **Project** | Build a new capstone project for a phase | 🔴 Hard |

---

## 🔍 Before You Start

### For small changes (typos, links, minor wording)
Just open a Pull Request. No issue needed. We'll review it quickly.

### For medium changes (new sections, code rewrites)
Open a PR with a short description of what you changed and why.

### For large changes (new modules, new phases, major restructuring)
**Open an issue first.** Describe what you want to add, why it belongs here, and roughly what it would cover. Wait for a maintainer to confirm before writing. This saves everyone time.

---

## 💻 Setting Up Locally

```bash
# 1. Fork the repo on GitHub, then clone your fork
git clone https://github.com/YOUR-USERNAME/llm-dev-curriculum.git
cd llm-dev-curriculum

# 2. Create a branch for your changes
git checkout -b your-branch-name
# Example: git checkout -b fix/typo-in-rag-module
# Example: git checkout -b add/module-structured-outputs

# 3. Make your changes

# 4. Commit with a clear message (see commit style below)
git add .
git commit -m "fix: correct tokenization explanation in 03-tokens-and-embeddings.md"

# 5. Push and open a Pull Request
git push origin your-branch-name
```

### Commit Message Style

Use the format: `type: short description`

| Type | When to use |
|------|------------|
| `fix` | Correcting errors, broken links, outdated info |
| `docs` | New content, rewrites, clarifications |
| `code` | Adding or updating code snippets / notebooks |
| `translate` | Translation work |
| `project` | Adding or updating a capstone project |
| `chore` | Repo maintenance (not content) |

**Examples:**
```
fix: update GPT-4 pricing in cost-management.md
docs: add section on HyDE to advanced-rag.md
code: add async streaming example to 05-streaming.md
translate: add Hindi translation for phase 0
```

---

## ✍️ Writing Style Guide

Our goal is content that a smart 16-year-old with no ML background could follow, while still being useful to experienced developers. That's a hard balance — here's how we hit it:

### Tone
- **Friendly, not dumbed-down.** Explain clearly, don't patronize.
- **Direct.** Get to the point. Cut filler sentences.
- **Honest about complexity.** Don't pretend things are simpler than they are. Say "this is tricky, here's why."
- **First person plural.** Use "we" and "you", not "one" or passive voice.

### Structure
- Start every module with a **"What you'll learn"** box
- Use **headers** to break content into scannable chunks
- Use **bold** for key terms when introduced for the first time
- Put **code in code blocks**, always with a language tag
- End every module with a **"Key Takeaways"** section and a **"What's Next"** pointer

### Examples
- Always include at least one **concrete, runnable example** per concept
- Use **real tools and real APIs** (not pseudocode unless explaining a concept)
- When using placeholder API keys, always use `sk-...` or `YOUR_API_KEY_HERE`

### What to Avoid
- ❌ "As you can see..." / "Obviously..." / "Simply..." — these are condescending
- ❌ Walls of text without code or examples
- ❌ Jargon without definition on first use
- ❌ Content that will be outdated in 3 months (e.g. specific benchmark scores — link out instead)
- ❌ Recommending paid tools without mentioning free alternatives

---

## 📐 Module Format Guide

All modules follow this structure:

```markdown
# Module Title

> One-sentence description of what this module is about.

## 🎯 What You'll Learn
- Bullet 1
- Bullet 2
- Bullet 3

## ⏱️ Prerequisites
- What the reader should know before starting
- Link to the prerequisite module if relevant

---

## Section 1: ...

[Content]

## Section 2: ...

[Content]

## 💻 Code Example

[Runnable code block]

---

## ✅ Key Takeaways
- Takeaway 1
- Takeaway 2

## ➡️ What's Next
[Link to the next module and a one-line description]

## 📚 Further Reading
- [Resource title](URL) — one-line description
```

### Notebooks (`.ipynb`)
- Must run top-to-bottom without errors in a fresh environment
- First cell: install all dependencies with `pip install ...`
- Include a markdown cell at the top with: title, description, prerequisites, estimated time
- Comment your code generously — treat it like teaching material, not production code

### Code Projects
Each project folder must contain:
```
projects/your-project-name/
├── README.md         ← setup, run instructions, what you'll build
├── requirements.txt  ← or pyproject.toml
├── solution/         ← complete working solution
└── starter/          ← scaffold for learners to fill in (optional)
```

---

## 🔁 Pull Request Process

1. **Fill in the PR template** — describe what you changed and why
2. **Link any related issue** using `Closes #123` in the PR description
3. **Self-review** your diff before requesting review — catch your own typos
4. A maintainer will review within **5 business days** (usually faster)
5. Address any feedback — we'll work with you, not against you
6. Once approved, a maintainer will merge it

### PR Checklist

Before submitting, confirm:

- [ ] Content is accurate to the best of my knowledge
- [ ] Code examples run without errors
- [ ] No API keys or secrets in any file
- [ ] Follows the writing style guide
- [ ] Follows the module format (if adding a module)
- [ ] Links are working
- [ ] Commit messages follow the style guide

---

## 🐛 Reporting Issues

Found an error, outdated info, or broken link? [Open an issue](https://github.com/your-org/llm-dev-curriculum/issues/new).

**Good issue titles:**
- `fix: broken link in 03-rag/02-vector-databases.md`
- `outdated: OpenAI pricing in 02-apis/07-cost-management.md`
- `request: add module on function calling with Gemini`

**Include:**
- Which file / section the issue is in
- What the current content says
- What it should say (if you know)

---

## 🌍 Translations

We want this curriculum to be accessible worldwide. To start a translation:

1. Open an issue titled `translation: [Language Name]`
2. A maintainer will create a `/[lang-code]/` folder (e.g. `/hi/`, `/es/`, `/zh/`)
3. Start with Phase 0 — it's the most impactful for beginners
4. Each translated file mirrors the English source structure

**Translation notes:**
- Keep code blocks in English (variable names, etc.)
- You may adapt examples to be culturally relevant
- Mark translated files with a status badge at the top:
  ```
  > 🌍 Translation status: **In Progress** · [View English original](../en/...)
  ```

---

## 🏆 Recognition

Contributors are recognized in two ways:

1. **All contributors** are listed in our [Contributors](https://github.com/your-org/llm-dev-curriculum/graphs/contributors) page automatically via GitHub
2. **Module authors** are credited at the top of the files they write:
   ```markdown
   > ✍️ Written by [@your-handle](https://github.com/your-handle)
   ```

Thank you for making this better for everyone. 🙏
