# 02 — Prompt Engineering Techniques

> Prompt engineering is the skill of designing inputs that reliably produce the outputs you want. It's the highest-ROI skill in this entire curriculum — and it applies to every LLM, every task, every model size.

✍️ Part of [Phase 1: Prompting](./README.md) · 🟢 Beginner · ⏱️ ~1 hr read + exercises

---

## 🎯 What You'll Learn

- The anatomy of an effective prompt
- 10 core prompt engineering techniques with real examples
- How to structure prompts for consistent, reliable outputs
- Common failure modes and how to fix them
- A repeatable framework for iterating on prompts

## ⏱️ Prerequisites

- [01 — Prompt Basics](./01-prompt-basics.md) — or any basic familiarity with chatting with an LLM

---

## 1. Why Prompting Matters More Than You Think

The same model, with different prompts, can produce wildly different results.

Here's the same task — summarize a research paper — with two prompts:

**Prompt A:**
```
Summarize this paper.
[paper text]
```

**Prompt B:**
```
You are a research assistant helping a software engineer with no ML background.

Summarize the following research paper in 3 parts:
1. The problem it's solving (1-2 sentences, plain English)
2. The key idea / approach (2-3 sentences)
3. Why it matters for practitioners (1-2 sentences)

Avoid jargon. If you must use a technical term, define it inline.

Paper:
[paper text]
```

Both prompts use the exact same model. Prompt B will almost always produce a more useful, more consistent, more actionable output.

The model's capability is fixed. **Your prompt is the interface to that capability.**

---

## 2. The Anatomy of a Prompt

A well-structured prompt has up to four components. Not every prompt needs all four — but knowing them helps you diagnose what's missing when a prompt fails.

```
┌─────────────────────────────────────────────┐
│  ROLE (optional)                            │
│  Who is the model playing?                  │
│  "You are a senior Python developer..."     │
├─────────────────────────────────────────────┤
│  CONTEXT (often essential)                  │
│  What background does the model need?       │
│  "The user is a beginner. The codebase      │
│   uses FastAPI and PostgreSQL..."           │
├─────────────────────────────────────────────┤
│  TASK (always required)                     │
│  What do you want it to do?                 │
│  "Review this function and suggest 3        │
│   specific improvements."                   │
├─────────────────────────────────────────────┤
│  FORMAT (often very helpful)                │
│  How should the output be structured?       │
│  "Respond as a numbered list. Each item:    │
│   issue, why it matters, fix."              │
└─────────────────────────────────────────────┘
```

Most weak prompts are missing **context** or **format**. Most good prompts nail all four.

---

## 3. The 10 Core Techniques

### Technique 1 — Be Specific About the Output Format

**The problem:** Vague prompts produce vague outputs.

**The fix:** Tell the model exactly what structure, length, and format you want.

```python
# ❌ Vague
prompt = "Give me some ideas for a mobile app."

# ✅ Specific
prompt = """
Generate 5 mobile app ideas targeting college students in India.

For each idea, provide:
- App name (2-4 words)
- Problem it solves (1 sentence)
- Core feature (1 sentence)
- Monetization approach (1 sentence)

Format as a numbered list.
"""
```

---

### Technique 2 — Give the Model a Role

Assigning a role narrows the model's "personality" and biases it toward the expertise and tone you need.

```python
# ❌ No role
prompt = "What's wrong with this SQL query?\n\n" + query

# ✅ With role
prompt = f"""
You are a senior database engineer with 10 years of PostgreSQL experience.

Review the following SQL query for:
1. Performance issues
2. Security vulnerabilities (especially SQL injection)
3. Correctness

Be direct. If there are no issues, say so.

Query:
{query}
"""
```

> 💡 **Tip:** Roles work best when they're specific. "Expert" is vague. "Senior backend engineer who specializes in high-traffic Python APIs" is specific.

---

### Technique 3 — Use Delimiters to Separate Content from Instructions

When your prompt mixes instructions with user-provided content, the model can get confused about what it's supposed to do versus what it's supposed to process. Delimiters fix this.

Common delimiters: triple backticks ` ``` `, `<tags>`, `---`, `###`

```python
# ❌ Ambiguous — is the model supposed to follow these instructions, or summarize them?
prompt = "Summarize the following text. Return only in English. Text: Return only in English."

# ✅ Clear separation with delimiters
prompt = """
Summarize the text inside <article> tags in 2-3 sentences. Return only the summary.

<article>
{article_text}
</article>
"""
```

XML-style tags (`<document>`, `<code>`, `<context>`) are particularly effective with Claude. Triple backticks work well with GPT models.

---

### Technique 4 — Few-Shot Prompting

Show the model examples of what you want before asking it to do the task. This is one of the most reliable techniques for consistent output formatting.

```python
prompt = """
Classify the sentiment of customer reviews as POSITIVE, NEGATIVE, or NEUTRAL.

Examples:
Review: "The product arrived damaged and customer service was useless."
Sentiment: NEGATIVE

Review: "It works fine, nothing special."
Sentiment: NEUTRAL

Review: "Absolutely love it! Best purchase I've made this year."
Sentiment: POSITIVE

Now classify this review:
Review: "{user_review}"
Sentiment:"""
```

**How many examples?** Usually 2–5 is enough. More examples = more consistent output, but also more tokens (= more cost). Start with 3.

> 💡 **Key insight:** The examples don't just show the model the format — they also implicitly define your classification scheme. If your examples all treat mild praise as NEUTRAL, the model will learn that threshold.

---

### Technique 5 — Chain-of-Thought (CoT)

For tasks that require reasoning, ask the model to think step by step before giving the answer. This dramatically improves accuracy on math, logic, and multi-step tasks.

```python
# ❌ Direct answer — higher error rate on reasoning tasks
prompt = "A store sells apples for ₹12 each. If Priya buys 7 apples and pays with a ₹100 note, how much change does she get?"

# ✅ Chain-of-thought
prompt = """
A store sells apples for ₹12 each. If Priya buys 7 apples and pays with a ₹100 note, how much change does she get?

Think through this step by step before giving the final answer.
"""

# ✅ Zero-shot CoT (the magic phrase)
# Just appending "Let's think step by step." to almost any reasoning prompt improves results.
prompt = "... Let's think step by step."
```

**Why it works:** Generating intermediate steps forces the model to "commit" to a reasoning path rather than jumping straight to a likely-sounding (but potentially wrong) answer.

---

### Technique 6 — Output Anchoring

Start the model's response with the beginning of the answer you want. This steers generation powerfully.

```python
messages = [
    {
        "role": "user",
        "content": "What are the three main causes of the 2008 financial crisis?"
    },
    {
        "role": "assistant",
        "content": "The three main causes were:"  # ← anchor the output format
    }
]
```

This works because you're providing a "prefix" for the model to continue from. It's especially useful for:
- Forcing JSON output (`"assistant": "{"`)
- Forcing a specific list format
- Preventing unnecessary preamble ("Great question! Certainly, I'd be happy to...")

---

### Technique 7 — Ask for JSON Explicitly (Structured Outputs)

When you need to parse the output programmatically, always ask for JSON and validate it.

```python
import json
from openai import OpenAI

client = OpenAI(api_key="YOUR_API_KEY_HERE")

prompt = """
Extract the following information from the job posting below and return it as JSON.

Required fields:
- job_title (string)
- company_name (string)
- location (string)
- salary_range (string or null if not mentioned)
- required_skills (array of strings)
- is_remote (boolean)

Return ONLY valid JSON. No explanation, no markdown fences.

Job posting:
{job_posting_text}
"""

response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": prompt}],
    response_format={"type": "json_object"}  # enforces JSON output on supported models
)

data = json.loads(response.choices[0].message.content)
print(data["job_title"])
```

> 💡 OpenAI's `response_format={"type": "json_object"}` mode guarantees valid JSON. Anthropic has a similar `tool_use` pattern. For models that don't support this, see the error handling section below.

---

### Technique 8 — Negative Prompting

Tell the model what NOT to do. This is often more effective than only saying what to do.

```python
# ✅ With negative constraints
prompt = """
Write a product description for this wireless keyboard.

Guidelines:
- Focus on the typing experience and build quality
- Keep it under 80 words
- Do NOT use the words "revolutionary", "game-changing", or "innovative"
- Do NOT make claims about battery life without data
- Do NOT use exclamation marks

Product specs:
{specs}
"""
```

Negative prompting is particularly useful for:
- Removing filler phrases ("Certainly!", "Great question!")
- Preventing hallucination of specific facts
- Enforcing tone constraints ("Do not use casual language")
- Avoiding overused marketing language

---

### Technique 9 — Iterative Refinement in a Single Prompt

Instead of one-shot generation, ask the model to generate, then critique, then improve.

```python
prompt = """
Task: Write a cold email to a startup founder pitching our API product.

Step 1: Write a first draft of the email (max 150 words).
Step 2: List 3 weaknesses of this draft.
Step 3: Write an improved version that addresses those weaknesses.

Return all three steps clearly labeled.

Context about our product:
{product_context}
"""
```

This technique exploits the fact that models are often better critics than first-draft writers. By forcing a self-critique step, you consistently get better final output without multiple API calls.

---

### Technique 10 — Meta-Prompting (Ask the Model to Write the Prompt)

When you're not sure how to prompt for a task, ask the model itself.

```python
prompt = """
I need to extract structured data from unstructured customer support tickets.

Specifically, I want to extract:
- Customer name
- Product mentioned
- Issue category (billing / technical / shipping / other)
- Urgency level (low / medium / high)
- Requested action

Write me a high-quality prompt I can use for this extraction task. 
Include 2 examples in the prompt to make it few-shot.
"""
```

The model has seen millions of prompts and knows what makes them work. Use that knowledge.

---

## 4. The Prompt Iteration Framework

When a prompt isn't working, don't just rewrite it randomly. Diagnose first:

```
Is the output the wrong format?
  → Add explicit format instructions
  → Add a few-shot example
  → Use output anchoring

Is the output too vague / generic?
  → Add more context about your use case
  → Assign a specific expert role
  → Add negative constraints ("don't be generic")

Is the output factually wrong?
  → Add "If you're unsure, say so. Don't guess."
  → Add "Only include information from the provided text."
  → Consider RAG (Phase 3) to ground the model in real data

Is the output too long / too short?
  → Specify exact length ("respond in exactly 3 sentences")
  → Use "be concise" or "be thorough" explicitly

Is the output inconsistent across runs?
  → Lower the temperature (more deterministic)
  → Add more examples (few-shot)
  → Add more constraints (less room for variation)
```

---

## 5. A Complete Example — Putting It All Together

Here's a production-quality prompt that uses multiple techniques at once:

```python
SYSTEM_PROMPT = """
You are a senior technical writer at a software company.
Your job is to convert GitHub issue descriptions into well-structured bug reports.
You are precise, thorough, and concise. You never invent information not in the original issue.
"""

USER_PROMPT = """
Convert the GitHub issue below into a structured bug report.

Output format (JSON):
{
  "title": "Short, descriptive bug title (max 10 words)",
  "severity": "critical | high | medium | low",
  "affected_component": "The part of the system affected",
  "summary": "2-3 sentence plain English description",
  "steps_to_reproduce": ["step 1", "step 2", "..."],
  "expected_behavior": "What should happen",
  "actual_behavior": "What actually happens",
  "suggested_labels": ["array", "of", "relevant", "labels"]
}

Rules:
- Return ONLY valid JSON. No preamble, no markdown.
- If information is missing from the issue, use null for that field.
- Do NOT invent steps or details not mentioned in the issue.
- Severity guide: critical=data loss/security, high=broken feature, medium=degraded UX, low=cosmetic

GitHub Issue:
<issue>
{issue_text}
</issue>
"""

# Usage
import json
from openai import OpenAI

client = OpenAI(api_key="YOUR_API_KEY_HERE")

def parse_github_issue(issue_text: str) -> dict:
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[
            {"role": "system", "content": SYSTEM_PROMPT},
            {"role": "user", "content": USER_PROMPT.format(issue_text=issue_text)}
        ],
        response_format={"type": "json_object"},
        temperature=0.1  # low temperature for consistency
    )
    return json.loads(response.choices[0].message.content)
```

Techniques used: **role + context** (system prompt), **format specification** (JSON schema), **XML delimiters** (`<issue>`), **negative prompting** ("do NOT invent"), **output anchoring** (JSON object mode), **low temperature** for consistency.

---

## 6. Common Mistakes and How to Fix Them

| Mistake | Example | Fix |
|---------|---------|-----|
| **Asking multiple questions at once** | "What is X, how does it work, and when should I use it?" | One task per prompt, or explicitly number the sub-tasks |
| **Assuming the model knows your context** | "Fix the bug in my code" | Always include the code, the error, and what you expected |
| **Using vague quality words** | "Write a good summary" | Define what "good" means: length, audience, format, tone |
| **No negative constraints** | (prompt with no "do not" clauses) | Add at least one constraint about what to avoid |
| **Not testing edge cases** | Testing with one nice example | Test with short input, long input, missing data, unusual input |
| **Ignoring temperature** | Using default 1.0 for everything | Lower temp for factual/structured tasks, higher for creative |

---

## ✅ Key Takeaways

- A prompt has four components: Role, Context, Task, Format — most weak prompts are missing one
- Few-shot examples are the single most reliable way to control output format
- Chain-of-thought ("step by step") dramatically improves reasoning accuracy
- Negative constraints ("do NOT") are underused and highly effective
- For programmatic use, always request JSON and use `response_format` where supported
- When a prompt fails, diagnose before rewriting — the fix is usually targeted

---

## 🏗️ Exercise

Take any task you regularly do manually (writing emails, summarizing notes, classifying data, reviewing code) and write a production-quality prompt for it using at least 4 of the 10 techniques above. Test it on 5 different inputs and note where it fails.

---

## ➡️ What's Next

**[03 — Chain-of-Thought & Reasoning Prompts](./03-chain-of-thought.md)**
We go deeper on CoT — self-consistency, tree-of-thought, and reasoning strategies for complex multi-step tasks.

---

## 📚 Further Reading

- [Prompt Engineering Guide (DAIR.AI)](https://www.promptingguide.ai/) — the most comprehensive open reference
- [Anthropic's Prompt Engineering Docs](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview) — Claude-specific techniques
- [OpenAI Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering) — OpenAI's official reference
- [Large Language Models as Optimizers (OPRO paper)](https://arxiv.org/abs/2309.03409) — using LLMs to optimize prompts automatically
