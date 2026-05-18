# LLM RAG Chatbot

An AI-powered conversational system built on Retrieval-Augmented Generation (RAG) — combining semantic vector search, LangGraph agentic orchestration, and a FastAPI backend deployed on GCP Cloud Run.

This project serves as the architectural foundation for production health AI systems, demonstrating how to build reliable, cost-efficient, and context-aware LLM applications.

---

## System Architecture

```
User Query
    │
    ▼
FastAPI Backend          ← Request handling, session routing
    │
    ▼
Embedding Model          ← Sentence Transformers, query vectorization
    │
    ▼
Qdrant Vector Search ←── Knowledge Base (WHO, NHS, Mayo Clinic)
    │
    ▼
LLM Generation           ← Context-aware response, LangGraph routing
    │           └──────── Firestore (conversation memory)
    ▼
Response to User
```

---

## Features

- **Retrieval-Augmented Generation** — semantic search over a domain-specific knowledge base before generation
- **LangGraph orchestration** — agentic state machine for multi-turn conversation routing
- **Qdrant vector search** — fast, scalable similarity search with Sentence Transformer embeddings
- **Conversation memory** — persistent multi-turn context via Firestore
- **FastAPI backend** — async REST API with clean endpoint structure
- **GCP Cloud Run deployment** — containerised, serverless, auto-scaling
- **Tiered LLM architecture** — lightweight model for simple queries, powerful model for complex reasoning (~$0.0003/interaction)

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Python 3.11 |
| API framework | FastAPI |
| Orchestration | LangGraph |
| Vector DB | Qdrant Cloud |
| Embeddings | Sentence Transformers |
| LLM APIs | Gemini Flash-Lite · Gemini 2.5 Flash |
| Memory | Firestore |
| Deployment | GCP Cloud Run · Docker |

---

## Project Structure

```
llm-rag-chatbot/
├── app/
│   ├── main.py              # FastAPI app entry point
│   ├── routes/              # API endpoint handlers
│   ├── agents/              # LangGraph state machine & nodes
│   ├── retrieval/           # Qdrant search & embedding logic
│   └── memory/              # Firestore conversation storage
├── knowledge_base/          # Document ingestion & indexing scripts
├── Dockerfile
├── requirements.txt
└── README.md
```

---

## How It Works

1. **User sends a query** via the REST API
2. **Query is embedded** using Sentence Transformers into a dense vector
3. **Qdrant retrieves** the top-k semantically relevant context chunks from the knowledge base
4. **LangGraph routes** the query — simple questions go to a lightweight model, complex ones to a more capable model
5. **LLM generates** a response grounded in the retrieved context
6. **Firestore stores** the conversation turn for multi-turn memory
7. **Response returned** to the user via the API

---

## Key Design Decisions

**Why LangGraph over a simple chain?**
LangGraph's state machine allows conditional routing — different query types (factual lookup, reasoning, out-of-scope) follow different execution paths, which improves accuracy and reduces unnecessary token usage.

**Why Qdrant?**
Qdrant offers a hosted cloud tier with fast approximate nearest-neighbor search, making it easy to iterate on knowledge base updates without managing infrastructure.

**Tiered LLM architecture**
Simple, factual queries are handled by a lightweight model (fast, cheap). Complex multi-step reasoning is routed to a more capable model. This keeps cost per interaction well below $0.001 while maintaining quality where it matters.

---

## Status

This repository documents the reference architecture. The production implementation runs as a deployed health AI assistant with real users.

Active development areas:
- Multi-agent orchestration for parallel retrieval paths
- Re-ranking retrieved chunks before generation
- Streaming response support
- Evaluation pipeline for RAG quality metrics

---

## Author

**Fatima Tuz Zehra** — AI Engineer  
[LinkedIn](https://www.linkedin.com/in/fatima-tuz-zehra) · [GitHub](https://github.com/zehra99hassan) · zehra99hassan@gmail.com
