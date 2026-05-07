# ADIP — Adaptive Document Intelligence Pipeline

*Mortgage document intelligence using Mixture-of-RAGs routing and contextual retrieval.*

> **Status:** Core architecture functional (Google Colab prototype). Accuracy and latency improvements in progress. Not yet production-ready — this is the working foundation.

---

## What It Does

ADIP processes mortgage document packages (loan applications, closing disclosures, paystubs, W2s) and answers queries with intelligent retrieval — extracting borrower details, verifying compliance, and checking document completeness.

The core insight: **one retrieval strategy doesn't fit all queries.** A question like "What is the borrower's name?" needs sparse keyword extraction. A question like "Are all FHA requirements met?" needs hybrid semantic + compliance lookup. ADIP routes accordingly.

---

## Architecture

```
User Query → [MoRAG Router] → Strategy Selection → Contextual Chunks → Answer
```

**MoRAG Router** — rule-based strategy selection across 4 modes:
- `SPARSE` — keyword matching for data extraction (names, amounts, dates)
- `FILTERED` — targeted search within specific document types
- `DENSE` — semantic search for conceptual queries
- `HYBRID` — compliance checking, completeness verification

**Contextual Retrieval** (Anthropic, September 2024) — each chunk carries a document context header:
```
[Loan Application | Page 1 | Borrower: Sarah Johnson] + original text
```
Reduces retrieval failures by 35% on ambiguous queries.

**Vector Indexing:**
- `sentence-transformers/all-MiniLM-L6-v2`
- FAISS `IndexFlatL2`, 384-dimensional vectors

**Built-in FHA Knowledge Base** — compliance rules, document checklists, and requirement verification baked in.

---

## Stack

![Python](https://img.shields.io/badge/Python-3.11-3776ab?style=flat-square&logo=python)
![Claude AI](https://img.shields.io/badge/Claude-Anthropic-8B5CF6?style=flat-square&logo=anthropic)
![FAISS](https://img.shields.io/badge/FAISS-Vector_Search-blue?style=flat-square)
![Gradio](https://img.shields.io/badge/Gradio-UI-orange?style=flat-square)

- **Retrieval:** FAISS (dense) + BM25 keyword (sparse) + metadata filtering
- **Embeddings:** sentence-transformers/all-MiniLM-L6-v2
- **Document Processing:** PyPDF2 + OCR support
- **LLM:** Claude (Anthropic)
- **UI:** Gradio
- **Deployment:** Google Colab (prototype)

---

## Current Limitations

- Accuracy targets not yet fully met — routing logic is sound, precision on edge cases needs work
- Local deployment not yet configured
- Not production-ready

---

## Why MoRAG

Most RAG systems apply one retrieval strategy uniformly. Mortgage documents require precision across very different query types — exact data extraction, semantic reasoning, and compliance checking often in the same session. MoRAG treats retrieval strategy as a first-class architectural decision.

---

*Part of an ongoing exploration into document intelligence for regulated industries.*
