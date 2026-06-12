# 05 — Building a RAG Pipeline End-to-End

> RAG (Retrieval-Augmented Generation) is how you give an LLM access to your own data — documents, databases, wikis, anything. This module walks you through building a complete, working RAG pipeline from scratch.

✍️ Part of [Phase 3: RAG](./README.md) · 🟡 Intermediate · ⏱️ ~2 hr read + build

---

## 🎯 What You'll Learn

- How all the pieces of a RAG pipeline fit together
- How to load, clean, and chunk documents
- How to create and store embeddings in a vector database
- How to retrieve relevant context and inject it into a prompt
- How to evaluate whether your RAG pipeline is working
- A complete, runnable implementation you can adapt immediately

## ⏱️ Prerequisites

- [01 — What Is RAG and Why It Matters](./01-what-is-rag.md)
- [02 — Vector Databases Explained](./02-vector-databases.md)
- [03 — Chunking Strategies](./03-chunking.md)
- [04 — Embeddings Deep Dive](./04-embeddings-deep-dive.md)
- Basic Python. Familiarity with the OpenAI API.

---

## 1. The Full Picture

Before writing any code, understand what you're building:

```
                        ┌─────────────────────────────────────┐
                        │         INDEXING PIPELINE           │
                        │         (run once, offline)         │
                        │                                     │
  Your Documents ──────►│  Load → Clean → Chunk → Embed ─────►│ Vector DB
  (PDF, MD, TXT, etc.)  │                                     │
                        └─────────────────────────────────────┘

                        ┌─────────────────────────────────────┐
                        │         QUERY PIPELINE              │
                        │         (run on every question)     │
                        │                                     │
  User Question ────────►│  Embed Question                    │
                        │       │                             │
                        │       ▼                             │
                        │  Search Vector DB ─► Top-K Chunks   │
                        │       │                             │
                        │       ▼                             │
                        │  Build Prompt (question + chunks)   │
                        │       │                             │
                        │       ▼                             │
                        │  LLM generates answer ─────────────►│ Answer
                        └─────────────────────────────────────┘
```

Two pipelines. Two distinct phases. The indexing pipeline runs once (or whenever your data changes). The query pipeline runs every time a user asks a question.

---

## 2. Setup

```bash
pip install openai chromadb tiktoken pypdf langchain-text-splitters python-dotenv
```

```python
# .env
OPENAI_API_KEY=sk-...
```

```python
# config.py
import os
from dotenv import load_dotenv

load_dotenv()

OPENAI_API_KEY  = os.getenv("OPENAI_API_KEY")
EMBED_MODEL     = "text-embedding-3-small"   # cheap, fast, good
CHAT_MODEL      = "gpt-4o-mini"              # cheap, fast, good
CHUNK_SIZE      = 512                         # tokens per chunk
CHUNK_OVERLAP   = 64                          # overlap between chunks
TOP_K           = 5                           # chunks to retrieve per query
COLLECTION_NAME = "my_documents"
```

---

## 3. Step 1 — Load Documents

```python
# loader.py
import os
from pathlib import Path
from pypdf import PdfReader

def load_text_file(path: str) -> str:
    """Load a plain text or markdown file."""
    with open(path, "r", encoding="utf-8") as f:
        return f.read()

def load_pdf(path: str) -> str:
    """Extract text from a PDF."""
    reader = PdfReader(path)
    pages = []
    for page in reader.pages:
        text = page.extract_text()
        if text:  # some pages are images — skip them
            pages.append(text)
    return "\n\n".join(pages)

def load_document(path: str) -> dict:
    """
    Load a single document. Returns a dict with:
    - content: the raw text
    - source: the file path (used for citations later)
    - type: file extension
    """
    path = Path(path)
    ext = path.suffix.lower()

    if ext in [".txt", ".md"]:
        content = load_text_file(path)
    elif ext == ".pdf":
        content = load_pdf(path)
    else:
        raise ValueError(f"Unsupported file type: {ext}")

    return {
        "content": content,
        "source": str(path),
        "type": ext.lstrip(".")
    }

def load_directory(directory: str) -> list[dict]:
    """Load all supported documents from a directory."""
    docs = []
    for path in Path(directory).rglob("*"):
        if path.suffix.lower() in [".txt", ".md", ".pdf"]:
            try:
                docs.append(load_document(str(path)))
                print(f"  ✓ Loaded: {path.name}")
            except Exception as e:
                print(f"  ✗ Failed: {path.name} — {e}")
    return docs
```

---

## 4. Step 2 — Clean and Chunk

Raw documents are too long for an LLM's context window and too noisy for good retrieval. We split them into clean, overlapping chunks.

```python
# chunker.py
import re
from langchain_text_splitters import RecursiveCharacterTextSplitter
import tiktoken

def count_tokens(text: str, model: str = "gpt-4o-mini") -> int:
    """Count tokens in a string using tiktoken."""
    enc = tiktoken.encoding_for_model(model)
    return len(enc.encode(text))

def clean_text(text: str) -> str:
    """
    Basic text cleaning:
    - Collapse multiple blank lines into one
    - Strip leading/trailing whitespace per line
    - Remove null bytes
    """
    text = text.replace("\x00", "")                   # null bytes
    text = re.sub(r"\n{3,}", "\n\n", text)            # collapse blank lines
    lines = [line.strip() for line in text.splitlines()]
    text = "\n".join(lines)
    return text.strip()

def chunk_document(doc: dict, chunk_size: int = 512, chunk_overlap: int = 64) -> list[dict]:
    """
    Split a document into overlapping chunks.
    Returns a list of chunk dicts, each with content + metadata.
    """
    splitter = RecursiveCharacterTextSplitter(
        chunk_size=chunk_size,
        chunk_overlap=chunk_overlap,
        length_function=count_tokens,       # measure in tokens, not chars
        separators=["\n\n", "\n", ". ", " ", ""]  # try to split on paragraphs first
    )

    clean = clean_text(doc["content"])
    splits = splitter.split_text(clean)

    chunks = []
    for i, text in enumerate(splits):
        chunks.append({
            "content": text,
            "metadata": {
                "source": doc["source"],
                "type": doc["type"],
                "chunk_index": i,
                "total_chunks": len(splits),
            }
        })

    return chunks

def chunk_documents(docs: list[dict], chunk_size: int = 512, chunk_overlap: int = 64) -> list[dict]:
    """Chunk all documents."""
    all_chunks = []
    for doc in docs:
        chunks = chunk_document(doc, chunk_size, chunk_overlap)
        all_chunks.extend(chunks)
        print(f"  ✓ {doc['source']}: {len(chunks)} chunks")
    return all_chunks
```

> 💡 **Why `RecursiveCharacterTextSplitter`?** It tries to split on natural boundaries (paragraphs, then sentences, then words) before splitting mid-word. This preserves semantic coherence in each chunk.

---

## 5. Step 3 — Embed and Store

```python
# vectorstore.py
import chromadb
from openai import OpenAI
from config import OPENAI_API_KEY, EMBED_MODEL, COLLECTION_NAME

client = OpenAI(api_key=OPENAI_API_KEY)
chroma = chromadb.PersistentClient(path="./chroma_db")  # persists to disk

def get_or_create_collection():
    return chroma.get_or_create_collection(
        name=COLLECTION_NAME,
        metadata={"hnsw:space": "cosine"}  # cosine similarity for text
    )

def embed_texts(texts: list[str]) -> list[list[float]]:
    """
    Embed a list of strings using OpenAI's embedding model.
    Batches automatically — OpenAI allows up to 2048 inputs per call.
    """
    response = client.embeddings.create(
        model=EMBED_MODEL,
        input=texts
    )
    return [item.embedding for item in response.data]

def index_chunks(chunks: list[dict], batch_size: int = 100):
    """
    Embed chunks and store them in ChromaDB.
    Processes in batches to avoid API rate limits.
    """
    collection = get_or_create_collection()

    # Skip chunks already in the collection (for incremental updates)
    existing_ids = set(collection.get()["ids"])

    to_index = []
    for i, chunk in enumerate(chunks):
        chunk_id = f"{chunk['metadata']['source']}::chunk_{chunk['metadata']['chunk_index']}"
        if chunk_id not in existing_ids:
            to_index.append((chunk_id, chunk))

    if not to_index:
        print("  ✓ All chunks already indexed — nothing to do.")
        return

    print(f"  Indexing {len(to_index)} new chunks...")

    for i in range(0, len(to_index), batch_size):
        batch = to_index[i : i + batch_size]
        ids      = [item[0] for item in batch]
        texts    = [item[1]["content"] for item in batch]
        metadatas = [item[1]["metadata"] for item in batch]

        embeddings = embed_texts(texts)

        collection.add(
            ids=ids,
            documents=texts,
            embeddings=embeddings,
            metadatas=metadatas
        )
        print(f"    Indexed batch {i // batch_size + 1} / {-(-len(to_index) // batch_size)}")

    print(f"  ✓ Done. Collection size: {collection.count()} chunks")

def retrieve(query: str, top_k: int = 5) -> list[dict]:
    """
    Embed a query and return the top-k most similar chunks.
    """
    collection = get_or_create_collection()
    query_embedding = embed_texts([query])[0]

    results = collection.query(
        query_embeddings=[query_embedding],
        n_results=top_k,
        include=["documents", "metadatas", "distances"]
    )

    chunks = []
    for doc, meta, dist in zip(
        results["documents"][0],
        results["metadatas"][0],
        results["distances"][0]
    ):
        chunks.append({
            "content": doc,
            "source": meta.get("source", "unknown"),
            "similarity": round(1 - dist, 4)   # convert distance to similarity score
        })

    return chunks
```

---

## 6. Step 4 — Retrieve and Generate

```python
# rag.py
from openai import OpenAI
from vectorstore import retrieve
from config import OPENAI_API_KEY, CHAT_MODEL, TOP_K

client = OpenAI(api_key=OPENAI_API_KEY)

SYSTEM_PROMPT = """
You are a helpful assistant that answers questions based on provided context.

Rules:
- Answer ONLY using information from the context provided.
- If the context does not contain enough information to answer, say:
  "I don't have enough information in my knowledge base to answer that."
- Never make up facts or add information beyond what's in the context.
- Cite your sources by referring to the document name at the end of your answer.
- Be concise and direct.
"""

def build_context_block(chunks: list[dict]) -> str:
    """Format retrieved chunks into a readable context block."""
    parts = []
    for i, chunk in enumerate(chunks, 1):
        source = chunk["source"].split("/")[-1]   # just the filename
        parts.append(
            f"[Source {i}: {source} (relevance: {chunk['similarity']:.0%})]\n"
            f"{chunk['content']}"
        )
    return "\n\n---\n\n".join(parts)

def answer(question: str, top_k: int = TOP_K, verbose: bool = False) -> dict:
    """
    Full RAG pipeline: retrieve relevant chunks, then generate an answer.

    Returns:
        - answer: the generated response
        - sources: list of source documents used
        - chunks: the raw retrieved chunks (for debugging)
    """
    # 1. Retrieve
    chunks = retrieve(question, top_k=top_k)

    if verbose:
        print(f"\n📚 Retrieved {len(chunks)} chunks:")
        for c in chunks:
            print(f"  [{c['similarity']:.0%}] {c['source'].split('/')[-1]}: {c['content'][:80]}...")

    # 2. Build prompt
    context = build_context_block(chunks)
    user_message = f"""Context:
{context}

---

Question: {question}"""

    # 3. Generate
    response = client.chat.completions.create(
        model=CHAT_MODEL,
        messages=[
            {"role": "system", "content": SYSTEM_PROMPT},
            {"role": "user",   "content": user_message}
        ],
        temperature=0.1   # low temperature for factual Q&A
    )

    answer_text = response.choices[0].message.content

    return {
        "answer": answer_text,
        "sources": list({c["source"] for c in chunks}),
        "chunks": chunks,
        "tokens_used": response.usage.total_tokens
    }
```

---

## 7. Step 5 — Wire It All Together

```python
# main.py
from loader import load_directory
from chunker import chunk_documents
from vectorstore import index_chunks
from rag import answer
from config import CHUNK_SIZE, CHUNK_OVERLAP

def build_index(docs_dir: str = "./docs"):
    """Indexing pipeline — run this once to build the vector store."""
    print("📂 Loading documents...")
    docs = load_directory(docs_dir)
    print(f"   Loaded {len(docs)} documents\n")

    print("✂️  Chunking documents...")
    chunks = chunk_documents(docs, CHUNK_SIZE, CHUNK_OVERLAP)
    print(f"   Created {len(chunks)} chunks\n")

    print("🔢 Embedding and indexing...")
    index_chunks(chunks)
    print()

def chat():
    """Simple interactive Q&A loop."""
    print("💬 RAG Chat — type 'quit' to exit\n")
    while True:
        question = input("You: ").strip()
        if question.lower() in ["quit", "exit", "q"]:
            break
        if not question:
            continue

        result = answer(question, verbose=True)
        print(f"\n🤖 Answer:\n{result['answer']}")
        print(f"\n📎 Sources: {', '.join(s.split('/')[-1] for s in result['sources'])}")
        print(f"🔢 Tokens used: {result['tokens_used']}\n")
        print("-" * 60 + "\n")

if __name__ == "__main__":
    import sys

    if len(sys.argv) > 1 and sys.argv[1] == "index":
        docs_dir = sys.argv[2] if len(sys.argv) > 2 else "./docs"
        build_index(docs_dir)
    else:
        chat()
```

**Run it:**
```bash
# Step 1: Index your documents
python main.py index ./docs

# Step 2: Chat with them
python main.py
```

---

## 8. Evaluating Your Pipeline

A pipeline that runs isn't necessarily a pipeline that works. Here's how to measure quality:

### 8a. Retrieval Quality
Check if the right chunks are being retrieved:

```python
def evaluate_retrieval(test_cases: list[dict], top_k: int = 5) -> dict:
    """
    test_cases: list of {"question": ..., "expected_source": ...}
    Returns hit rate — how often the expected source is in the top-k results.
    """
    hits = 0
    for case in test_cases:
        chunks = retrieve(case["question"], top_k=top_k)
        sources = [c["source"] for c in chunks]
        if any(case["expected_source"] in s for s in sources):
            hits += 1

    hit_rate = hits / len(test_cases)
    print(f"Retrieval hit rate: {hit_rate:.0%} ({hits}/{len(test_cases)})")
    return {"hit_rate": hit_rate, "hits": hits, "total": len(test_cases)}

# Example usage
test_cases = [
    {"question": "What is the refund policy?", "expected_source": "refund-policy.pdf"},
    {"question": "How do I reset my password?",  "expected_source": "user-guide.md"},
]
evaluate_retrieval(test_cases)
```

### 8b. Answer Quality (LLM-as-Judge)

```python
def evaluate_answer(question: str, answer: str, context: str) -> dict:
    """
    Use GPT-4o to evaluate whether an answer is grounded in the context.
    Returns a score and reasoning.
    """
    eval_prompt = f"""
You are evaluating whether a RAG system's answer is grounded in the provided context.

Score the answer on two dimensions (1-5 each):
1. Groundedness: Is every claim in the answer supported by the context?
   (1 = answer invents facts, 5 = every claim is directly from context)
2. Completeness: Does the answer address the question using available context?
   (1 = misses key info that's in context, 5 = thorough use of available info)

Return JSON: {{"groundedness": int, "completeness": int, "reasoning": "string"}}

Question: {question}

Context:
{context}

Answer:
{answer}
"""
    response = client.chat.completions.create(
        model="gpt-4o",
        messages=[{"role": "user", "content": eval_prompt}],
        response_format={"type": "json_object"}
    )
    import json
    return json.loads(response.choices[0].message.content)
```

---

## 9. Common Failure Modes and Fixes

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Model says "I don't know" when it should know | Retrieval failing — wrong chunks coming back | Lower chunk size, check embeddings, try query rewriting |
| Model answers but cites wrong source | Chunks from multiple docs blending together | Add doc title to each chunk's text, not just metadata |
| Slow on first query | Embedding model cold start | Warm up the embedding model on startup |
| Model ignores the context and uses general knowledge | System prompt not strict enough | Add "Use ONLY the provided context. Ignore your training knowledge for this task." |
| Good retrieval but bad answers | Context too noisy — irrelevant chunks diluting the good ones | Reduce `top_k`, add a reranker (see Advanced RAG module) |
| Chunks cut mid-sentence | Chunk size too small or splitter not respecting sentence boundaries | Increase chunk size, use sentence-aware splitter |
| Costs too high | Embedding every query with expensive model | Use `text-embedding-3-small` for queries; cache frequent queries |

---

## ✅ Key Takeaways

- RAG has two pipelines: **indexing** (offline) and **query** (real-time) — keep them separate
- Chunk quality is the single biggest factor in RAG performance — bad chunks = bad answers
- Measure in tokens, not characters — LLMs care about tokens
- Low temperature (0.1) is best for factual Q&A tasks
- Always evaluate retrieval quality before blaming the LLM for bad answers
- The system prompt's grounding instruction ("answer only from context") is what prevents hallucination

---

## 🏗️ Capstone Project

Build the full **[Chat with Your Documents](./projects/doc-chatbot/)** project:
- Load a set of your own documents (PDFs, notes, anything)
- Index them with this pipeline
- Add a simple web UI (Streamlit or Gradio)
- Add conversation history (multi-turn Q&A)
- Deploy it

---

## ➡️ What's Next

**[06 — Advanced RAG: Reranking, HyDE & Query Expansion](./06-advanced-rag.md)**
The techniques that take retrieval accuracy from good to great.

---

## 📚 Further Reading

- [LangChain RAG Tutorial](https://python.langchain.com/docs/tutorials/rag/) — official LangChain walkthrough
- [ChromaDB Docs](https://docs.trychroma.com/) — the vector DB used in this module
- [OpenAI Embeddings Guide](https://platform.openai.com/docs/guides/embeddings) — model comparison and best practices
- [RAGAS — RAG Evaluation Framework](https://github.com/explodinggradients/ragas) — automated RAG evaluation library
- [Pinecone's RAG Guide](https://www.pinecone.io/learn/retrieval-augmented-generation/) — excellent deep dive with diagrams
