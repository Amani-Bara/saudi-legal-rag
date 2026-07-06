# المساعد القانوني السعودي 🏛️
### Saudi Legal AI Assistant — RAG-Powered Legal Research

![Python](https://img.shields.io/badge/Python-3.11-blue)
![FastAPI](https://img.shields.io/badge/FastAPI-latest-green)
![Claude](https://img.shields.io/badge/Claude-Sonnet-orange)
![Pinecone](https://img.shields.io/badge/Pinecone-Vector_DB-purple)
![Railway](https://img.shields.io/badge/Deployed-Railway-success)

> 🔗 **Live Demo:** https://legal-ai-production-7001.up.railway.app

---

## 📌 Overview

An AI-powered legal research assistant specialized in Saudi Arabian court decisions. The system enables lawyers and legal professionals to search and analyze thousands of judicial rulings using natural language queries in Arabic.

---

## 🏗️ Architecture

User Query (Arabic)
↓
Cohere Multilingual Embeddings
↓
Pinecone Vector Search (4 namespaces in parallel)
├── Board of Grievances (ديوان المظالم)
├── Ministry of Justice (وزارة العدل)
├── CRSD — Securities Disputes (لجنة الأوراق المالية)
└── CRSD — Legal Principles (المبادئ القضائية)
↓
Cohere Reranker (multilingual)
↓
Claude Sonnet — Legal Analysis & Answer Generation
↓
Streaming Response to User


---

## 📊 Data Scale

| Source | Cases | Chunks |
|--------|-------|--------|
| Board of Grievances (ديوان المظالم) | 10,567 | 151,355 |
| Ministry of Justice (وزارة العدل) | — | — |
| CRSD Cases (لجنة الأوراق المالية) | — | — |
| CRSD Principles (المبادئ القضائية) | 300 | 589 |

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| **Embeddings** | Cohere `embed-multilingual-v3.0` |
| **Vector DB** | Pinecone (multi-namespace) |
| **Reranking** | Cohere `rerank-multilingual-v3.0` |
| **Generation** | Anthropic Claude Sonnet |
| **Backend** | FastAPI (Python) |
| **Frontend** | Vanilla JS + Arabic RTL UI |
| **Scraping** | Playwright + PyMuPDF |
| **Deployment** | Railway (auto-deploy from GitHub) |

---

## ✨ Key Features

- 🔍 **Multi-source parallel search** across 4 legal databases simultaneously
- 🌊 **Streaming responses** — answers appear word by word in real time
- 📜 **Structured legal analysis** with sections: وقائع، مسائل قانونية، نصوص نظامية، تطبيقات قضائية، خلاصة
- 🔗 **Source citations** with links to original court documents
- 🗣️ **Fully Arabic** interface with RTL support
- 💬 **Conversation history** for follow-up questions
- ⚖️ **Legal principles** from CRSD appeals committee integrated into search

---

## 🖥️ Screenshots



---

## 🔄 RAG Pipeline

```python
# Simplified concept

# 1. Embed the query
query_vector = cohere.embed(query, model="embed-multilingual-v3.0")

# 2. Search all namespaces in parallel
with ThreadPoolExecutor(max_workers=4) as executor:
    results = [
        executor.submit(pinecone.query, namespace=ns, vector=query_vector)
        for ns in ["bog", "__default__", "crsd", "crsd-principles"]
    ]

# 3. Rerank combined results
reranked = cohere.rerank(query=query, documents=all_chunks, top_n=5)

# 4. Generate structured legal answer
answer = claude.stream(
    system=LEGAL_SYSTEM_PROMPT,
    context=build_context(reranked)
)
```

---

## 📁 Data Pipeline

Web Scraping (Playwright)
↓
PDF Extraction (PyMuPDF)
↓
Arabic Quality Filtering (arabic_ratio >= 0.4)
↓
Text Chunking (1200 chars / 180 overlap)
↓
Cohere Embeddings
↓
Pinecone Upsert

---

## ⚠️ Disclaimer

This tool is for legal research assistance only and does not constitute legal advice. Always consult a licensed legal professional.



