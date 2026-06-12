# 01 — OpenAI API Quickstart

> Go from zero to calling GPT-4o from your own Python code. By the end of this module you'll have a working chat function, understand how the API is structured, and know what every parameter does.

✍️ Part of [Phase 2: APIs & Integrations](./README.md) · 🟢 Beginner · ⏱️ ~1 hr read + build

---

## 🎯 What You'll Learn

- How the OpenAI API is structured (models, messages, roles)
- How to make your first API call in Python
- What every key parameter does (temperature, max_tokens, top_p, etc.)
- How to handle multi-turn conversations with message history
- How to stream responses token by token
- How to count tokens and estimate costs before you run anything
- Common errors and exactly how to fix them

## ⏱️ Prerequisites

- [Phase 0: Foundations](../00-foundations/) — know what an LLM is
- [Phase 1: Prompting](../01-prompting/) — know what a prompt is
- Python installed. Comfortable running scripts.

---

## 1. Setup

### Install the SDK

```bash
pip install openai python-dotenv
```

### Get an API Key

1. Go to [platform.openai.com](https://platform.openai.com)
2. Sign up or log in → click your profile → **API Keys**
3. Click **Create new secret key** → copy it immediately (you won't see it again)
4. New accounts get free credits — enough to complete this entire module

### Store the Key Safely

Never hardcode API keys in your code. Use a `.env` file:

```bash
# .env  ← create this file in your project root
OPENAI_API_KEY=sk-...your-key-here...
```

```bash
# .gitignore  ← make sure .env is in here
.env
```

```python
# load_env.py — run this at the top of every script
from dotenv import load_dotenv
import os

load_dotenv()
api_key = os.getenv("OPENAI_API_KEY")
```

> ⚠️ **Warning:** If you accidentally commit an API key to a public GitHub repo, OpenAI will automatically revoke it within minutes. But your billing account may still be charged for any calls made in that window. Always use `.gitignore`.

---

## 2. Your First API Call

```python
from openai import OpenAI
from dotenv import load_dotenv

load_dotenv()
client = OpenAI()  # automatically reads OPENAI_API_KEY from environment

response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {"role": "user", "content": "What is the capital of France?"}
    ]
)

print(response.choices[0].message.content)
# → Paris
```

That's the core of every OpenAI API call. Everything else is built on top of this.

---

## 3. Understanding the Response Object

The full response contains more than just the text:

```python
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": "Hello!"}]
)

# The text you care about
print(response.choices[0].message.content)

# Why there might be multiple choices (see n parameter later)
print(len(response.choices))           # → 1

# Why the model stopped generating
print(response.choices[0].finish_reason)
# → "stop"     (model finished naturally)
# → "length"   (hit max_tokens limit)
# → "tool_calls" (model wants to call a function)

# Token usage — important for cost tracking
print(response.usage.prompt_tokens)    # tokens in your input
print(response.usage.completion_tokens) # tokens in the output
print(response.usage.total_tokens)     # sum of both
```

---

## 4. The Messages Array and Roles

Every OpenAI API call takes a `messages` list. Each message has a `role` and `content`:

| Role | Who it's from | Purpose |
|------|--------------|---------|
| `system` | You (developer) | Sets the model's behavior, persona, and constraints. Processed before everything else. |
| `user` | The human | The human's message or question. |
| `assistant` | The model | A previous model response. Used to provide conversation history. |

```python
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        # System prompt: sets behavior
        {
            "role": "system",
            "content": "You are a concise assistant. Answer in one sentence maximum."
        },
        # User message: the question
        {
            "role": "user",
            "content": "Explain quantum entanglement."
        }
    ]
)
```

> 💡 **System prompts are powerful.** They're the first thing the model reads and strongly bias everything that follows. A well-written system prompt can dramatically improve the quality and consistency of responses. → See [Phase 1: Prompting](../01-prompting/)

---

## 5. Key Parameters

```python
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[...],

    # --- Generation control ---
    temperature=0.7,      # Randomness. 0 = deterministic, 2 = chaotic. Default: 1.0
                          # Use 0-0.3 for factual tasks, 0.7-1.0 for creative tasks.

    max_tokens=500,       # Maximum tokens to generate. If hit, finish_reason = "length".
                          # Does NOT include prompt tokens. Default: model max.

    top_p=1.0,            # Nucleus sampling. Consider tokens summing to top_p probability.
                          # Use either temperature OR top_p, not both. Default: 1.0.

    # --- Multiple outputs ---
    n=1,                  # How many completions to generate. n=3 gives 3 different answers.
                          # Costs n× more tokens. Usually leave at 1.

    # --- Repetition control ---
    frequency_penalty=0,  # Penalise tokens based on how often they've appeared so far.
                          # Range: -2.0 to 2.0. Positive = less repetition. Default: 0.

    presence_penalty=0,   # Penalise tokens that have appeared at all (regardless of count).
                          # Range: -2.0 to 2.0. Positive = more topic diversity. Default: 0.

    # --- Reproducibility ---
    seed=42,              # Set for deterministic outputs (best effort — not guaranteed).

    # --- Output format ---
    response_format={"type": "json_object"},  # Force valid JSON output.
                                               # Omit for regular text.
)
```

### Parameter Quick Reference

| Task | Temperature | Notes |
|------|------------|-------|
| Data extraction / classification | 0.0 – 0.1 | Maximum consistency |
| Q&A over documents | 0.1 – 0.3 | Factual, minimal variance |
| Summarization | 0.3 – 0.5 | Balanced |
| Chatbot / conversation | 0.6 – 0.8 | Natural, slightly varied |
| Creative writing / brainstorming | 0.8 – 1.2 | Diverse, surprising |
| Poetry / experimental | 1.2 – 2.0 | Very unpredictable |

---

## 6. Multi-Turn Conversations

The OpenAI API is **stateless** — it has no memory between calls. To have a conversation, you must send the full message history with every request.

```python
from openai import OpenAI
from dotenv import load_dotenv

load_dotenv()
client = OpenAI()

def chat(history: list, user_message: str, system_prompt: str = None) -> tuple[str, list]:
    """
    Send a message and get a response, maintaining conversation history.

    Args:
        history: List of previous messages (can be empty for first turn)
        user_message: The new user message
        system_prompt: Optional system instructions

    Returns:
        (assistant_reply, updated_history)
    """
    # Build the messages list
    messages = []

    if system_prompt:
        messages.append({"role": "system", "content": system_prompt})

    messages.extend(history)  # all previous turns
    messages.append({"role": "user", "content": user_message})  # new message

    # Call the API
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=messages,
        temperature=0.7
    )

    reply = response.choices[0].message.content

    # Add both the user message and assistant reply to history
    updated_history = history + [
        {"role": "user",      "content": user_message},
        {"role": "assistant", "content": reply}
    ]

    return reply, updated_history


# Example: multi-turn conversation
system = "You are a helpful Python tutor. Keep explanations short."
history = []

reply, history = chat(history, "What is a list comprehension?", system)
print(f"Assistant: {reply}\n")

reply, history = chat(history, "Can you show me an example using numbers?", system)
print(f"Assistant: {reply}\n")

reply, history = chat(history, "Now make it filter only even numbers.", system)
print(f"Assistant: {reply}\n")

# The model remembers the full context of the conversation
```

> ⚠️ **Context window limit:** As conversation history grows, it approaches the model's context window limit. For long conversations you'll need a truncation or summarisation strategy. We cover this in [Phase 5: Agents — Memory](../05-agents/04-agent-memory.md).

---

## 7. Streaming Responses

By default, the API waits until the full response is generated before returning. Streaming sends tokens as they're generated — making your app feel much faster.

```python
from openai import OpenAI
from dotenv import load_dotenv

load_dotenv()
client = OpenAI()

# stream=True returns an iterator of chunks
stream = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": "Write a short poem about Python."}],
    stream=True
)

# Print tokens as they arrive
for chunk in stream:
    delta = chunk.choices[0].delta
    if delta.content:             # some chunks have no content (e.g. the final one)
        print(delta.content, end="", flush=True)

print()  # newline at the end
```

**Collecting the full streamed response:**

```python
full_response = ""
for chunk in stream:
    delta = chunk.choices[0].delta
    if delta.content:
        full_response += delta.content
        print(delta.content, end="", flush=True)

print()
# full_response now contains the complete text
```

---

## 8. Token Counting and Cost Estimation

Always estimate cost before running expensive batches.

```python
import tiktoken

def count_tokens(messages: list, model: str = "gpt-4o-mini") -> int:
    """
    Count the tokens in a messages list before sending to the API.
    Matches OpenAI's internal token counting closely (not exact — overhead varies by model).
    """
    enc = tiktoken.encoding_for_model(model)
    total = 0
    for message in messages:
        total += 4  # every message has ~4 tokens of overhead
        for key, value in message.items():
            total += len(enc.encode(str(value)))
    total += 2  # reply priming overhead
    return total

# Pricing as of early 2025 (always check platform.openai.com/docs/pricing for current rates)
PRICING = {
    "gpt-4o":        {"input": 2.50,  "output": 10.00},   # per 1M tokens
    "gpt-4o-mini":   {"input": 0.15,  "output": 0.60},
    "gpt-4-turbo":   {"input": 10.00, "output": 30.00},
}

def estimate_cost(prompt_tokens: int, completion_tokens: int, model: str = "gpt-4o-mini") -> float:
    """Estimate cost in USD."""
    if model not in PRICING:
        raise ValueError(f"Unknown model: {model}")
    p = PRICING[model]
    cost = (prompt_tokens / 1_000_000 * p["input"]) + \
           (completion_tokens / 1_000_000 * p["output"])
    return round(cost, 6)

# Example usage
messages = [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user",   "content": "Explain transformer attention in 3 sentences."}
]

input_tokens = count_tokens(messages)
expected_output = 150   # your estimate of output length

cost = estimate_cost(input_tokens, expected_output)
print(f"Estimated cost: ${cost:.6f}")
print(f"Input tokens: {input_tokens}")
```

---

## 9. Error Handling

```python
import openai
import time

def call_with_retry(messages: list, model: str = "gpt-4o-mini", max_retries: int = 3) -> str:
    """
    Call the OpenAI API with automatic retry on rate limit errors.
    """
    client = OpenAI()

    for attempt in range(max_retries):
        try:
            response = client.chat.completions.create(
                model=model,
                messages=messages
            )
            return response.choices[0].message.content

        except openai.RateLimitError:
            wait = 2 ** attempt   # exponential backoff: 1s, 2s, 4s
            print(f"Rate limited. Waiting {wait}s before retry {attempt + 1}/{max_retries}...")
            time.sleep(wait)

        except openai.AuthenticationError:
            raise ValueError("Invalid API key. Check your OPENAI_API_KEY environment variable.")

        except openai.BadRequestError as e:
            raise ValueError(f"Bad request — likely context too long or invalid input: {e}")

        except openai.APIConnectionError:
            print(f"Connection error. Retry {attempt + 1}/{max_retries}...")
            time.sleep(2)

    raise RuntimeError(f"Failed after {max_retries} retries.")
```

### Common Errors Cheat Sheet

| Error | Cause | Fix |
|-------|-------|-----|
| `AuthenticationError` | Invalid or missing API key | Check `OPENAI_API_KEY` in `.env` |
| `RateLimitError` | Too many requests or quota exceeded | Add exponential backoff; check usage limits |
| `BadRequestError: context_length_exceeded` | Messages exceed context window | Truncate history; use a larger context model |
| `BadRequestError: invalid_api_key` | Key was revoked or never existed | Generate a new key at platform.openai.com |
| `APIConnectionError` | Network issue | Retry with backoff |
| Empty `.content` on streamed chunk | Normal — some chunks are metadata only | Always check `if delta.content` before printing |

---

## 10. Choosing the Right Model

| Model | Best For | Cost (relative) |
|-------|---------|-----------------|
| `gpt-4o-mini` | Learning, prototyping, high-volume tasks | 💲 Cheapest |
| `gpt-4o` | Production apps needing high quality | 💲💲💲 |
| `gpt-4-turbo` | Long documents (128K context) | 💲💲💲 |
| `o1-mini` | Coding and reasoning tasks | 💲💲 |
| `o3-mini` | Advanced reasoning, math, science | 💲💲💲 |

> 💡 **Start with `gpt-4o-mini` for everything.** It's 15–20x cheaper than GPT-4o and good enough for 80% of tasks. Only upgrade if quality is genuinely insufficient.

---

## 💻 Putting It All Together — A Reusable Chat Client

```python
# chat_client.py — a clean, reusable wrapper around the OpenAI API
from openai import OpenAI
from dotenv import load_dotenv
import os

load_dotenv()

class ChatClient:
    def __init__(
        self,
        model: str = "gpt-4o-mini",
        system_prompt: str = "You are a helpful assistant.",
        temperature: float = 0.7,
        max_tokens: int = 1000,
    ):
        self.client      = OpenAI()
        self.model       = model
        self.temperature = temperature
        self.max_tokens  = max_tokens
        self.history     = []
        self.system      = system_prompt

    def chat(self, user_message: str) -> str:
        self.history.append({"role": "user", "content": user_message})

        messages = [{"role": "system", "content": self.system}] + self.history

        response = self.client.chat.completions.create(
            model=self.model,
            messages=messages,
            temperature=self.temperature,
            max_tokens=self.max_tokens
        )

        reply = response.choices[0].message.content
        self.history.append({"role": "assistant", "content": reply})
        return reply

    def reset(self):
        """Clear conversation history."""
        self.history = []

    @property
    def token_count(self) -> int:
        """Approximate tokens used so far in this conversation."""
        import tiktoken
        enc = tiktoken.encoding_for_model(self.model)
        return sum(len(enc.encode(m["content"])) for m in self.history)


# Usage
if __name__ == "__main__":
    bot = ChatClient(
        system_prompt="You are a Python tutor. Be concise.",
        temperature=0.5
    )

    while True:
        user_input = input("You: ").strip()
        if user_input.lower() in ["quit", "exit"]:
            break
        reply = bot.chat(user_input)
        print(f"Bot: {reply}\n")
        print(f"(~{bot.token_count} tokens used so far)")
```

---

## ✅ Key Takeaways

- Every API call needs: `model`, `messages` (with correct roles), and your API key in the environment
- The API is stateless — you must send full history for multi-turn conversations
- `temperature` is the single most impactful parameter — use low values for factual tasks
- Always stream for user-facing apps — it feels 3× faster even at the same speed
- Start with `gpt-4o-mini` — upgrade only when you've proven you need it
- Count tokens before batch jobs — surprises are expensive

---

## ➡️ What's Next

**[02 — Anthropic (Claude) API](./02-anthropic-api.md)**
The same patterns, a slightly different SDK, and some unique Claude-specific features worth knowing.

---

## 📚 Further Reading

- [OpenAI API Reference](https://platform.openai.com/docs/api-reference/chat) — the full parameter list
- [OpenAI Cookbook](https://github.com/openai/openai-cookbook) — official code examples for dozens of use cases
- [tiktoken on GitHub](https://github.com/openai/tiktoken) — OpenAI's tokenizer library
- [OpenAI Platform Usage Dashboard](https://platform.openai.com/usage) — monitor your costs in real time
