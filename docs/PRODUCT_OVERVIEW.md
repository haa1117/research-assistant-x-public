# Product Overview

Research-Assistant-X is a web-based research paper discovery platform. It ingests academic papers from arXiv and Semantic Scholar, embeds them with vector search, and offers three discovery modes: a trends dashboard, an AI-powered chatbot recommender, and an interactive citation graph explorer.

## What it solves

Researchers face information overload across multiple paper sources. This tool aggregates papers into a unified pipeline, extracts domain and keyword metadata via LLM processing, and surfaces relevant papers through semantic search and citation networks rather than keyword matching alone.

## Core features

**Paper ingestion.** Fetches papers from arXiv and Semantic Scholar, parses PDFs, extracts sections (abstract, introduction, methods, results), and stores metadata in a relational database.

**Semantic search.** Papers are embedded using a 1024-dimensional sentence transformer model (`gte-large`). Vector search finds semantically similar papers.

**Trends dashboard.** SQL analytics surface publication trends over time, average citation counts, top keywords, prolific authors, and research domains. A sidebar dropdown filters trends by keyword.

**RAG chatbot.** Users ask questions in natural language (e.g., "Show me papers about attention mechanisms in language models"). The chatbot retrieves candidate papers via vector search, filters them for relevance, and generates summaries with Gemini.

**Citation graph explorer.** A network visualization shows how papers cite each other. Nodes are papers; edges are citations. It is rendered as a static matplotlib image.

## Technology

**Backend pipeline:** Python 3.11, Prefect workflow orchestration (runs ingest every 4 hours), sentence-transformers for embeddings.

**Storage:** PostgreSQL (metadata + analytics), Milvus (vector index, 1024 dimensions), Dgraph (citation graph).

**Frontend:** Streamlit with three pages (dashboard, chatbot, citation graph).

**LLM integration:** Google Gemini 1.5 Flash for summaries; Groq/OpenRouter for paper processing; sentence-transformers for embeddings.

**Deployment:** Docker, configured for a Hugging Face Space.

## Demo mode

In demo mode the original UI scripts run unchanged and in-memory stubs are injected (keyword-based retrieval replacing vector search, canned summaries replacing real LLM calls, generated citation graphs with fictional papers and authors) so you can explore the UI and workflows without external credentials. Chatbot responses in demo are pre-written examples, not live LLM output.

Continue with the [technical case study](TECHNICAL_CASE_STUDY.md).
