# 📖 LLM Glossary

> Plain-English definitions for every term you'll encounter in this curriculum — and in the real world. No unnecessary math. No assumed knowledge.

Alphabetically ordered. Use `Ctrl+F` / `Cmd+F` to search.

---

## A

**Agent**
An LLM-powered system that can take actions — calling tools, browsing the web, writing and running code, interacting with APIs — in order to complete a goal autonomously.

**Attention Mechanism**
The core innovation of the Transformer architecture. Allows a model to "focus" on the most relevant parts of the input when generating each output token.

**Autoregressive**
Describes how LLMs generate text: one token at a time, where each new token is conditioned on all previously generated tokens.

---

## B

**Base Model**
A model that has been pretrained on a large text corpus but has not been fine-tuned for any specific task or aligned with human preferences.

**Benchmark**
A standardized test used to evaluate and compare LLM performance. Common benchmarks: MMLU, HumanEval, GSM8K, HellaSwag.

**BPE (Byte Pair Encoding)**
The most common tokenization algorithm used by modern LLMs.

---

## C

**Chain-of-Thought (CoT)**
A prompting technique where you ask the model to reason step by step before giving a final answer.

**Chunking**
The process of splitting large documents into smaller pieces before indexing them in a vector database for RAG.

**Context Window**
The maximum amount of text (measured in tokens) that a model can process in a single call.

---

## F

**Fine-Tuning**
Continuing to train a pretrained model on a smaller, task-specific dataset → See [LLM Dev Curriculum](./README.md)

**Few-Shot Prompting**
Providing a small number of examples (typically 2–10) of the task in the prompt, before asking the model to do the same task on new input.

---

## H

**Hallucination**
When an LLM generates confident, plausible-sounding but factually incorrect information.

**HuggingFace**
The most important open-source platform in the LLM ecosystem. Hosts thousands of open-weight models, datasets, and demo apps.

---

## L

**LLM (Large Language Model)**
A neural network trained on large amounts of text to predict the next token in a sequence.

**LoRA (Low-Rank Adaptation)**
An efficient fine-tuning technique that adds small trainable matrices to a frozen pretrained model → See [LoRA and QLoRA](./04-finetuning_lora.md)

---

## P

**Prompt**
The input you send to an LLM. Can include instructions, context, examples, and the actual question or task → See [Prompt Engineering](./02-prompt_engineering.md)

---

## Q

**QLoRA (Quantized LoRA)**
LoRA applied to a quantized (4-bit) model → See [LoRA and QLoRA](./04-finetuning_lora.md)

---

## R

**RAG (Retrieval-Augmented Generation)**
A technique for grounding LLM responses in external documents → See [RAG Pipeline](./03-rag_pipeline.md)

---

## T

**Token**
The basic unit an LLM processes — not exactly a word, but a chunk of text.

**Transformer**
The neural network architecture introduced in "Attention Is All You Need" (2017) that underlies virtually every modern LLM.

---

## Z

**Zero-Shot Prompting**
Asking the model to perform a task with no examples — just instructions.

---

## Missing a term?

[Open an issue →](https://github.com/ruhaankumar2013-debug/Practical-AI-engineering/issues/new?title=glossary:+add+term+%5BYOUR+TERM%5D) and we'll add it.
