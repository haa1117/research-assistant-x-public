# Technical Case Study: Research-Assistant-X

An engineering write-up of an AI research paper discovery app. Based on analysis of the production codebase. No production source is reproduced; descriptions are deliberately high level.

## 1. Product context

The app ingests papers from arXiv and Semantic Scholar and offers three ways to explore them: a trends dashboard, a RAG chatbot that recommends papers, and a citation graph explorer. The engineering problem is combining relational, vector and graph storage with LLM processing behind a small Streamlit front end.

## 2. Architecture

Ingestion pipeline (Python + Prefect) -> storage (PostgreSQL via Supabase, Milvus on Zilliz Cloud, Dgraph) -> Streamlit UI. See [ARCHITECTURE.md](ARCHITECTURE.md).

- **Pipeline.** Fetch papers, parse PDFs and extract sections, enrich with LLM keywords and domains, chunk and embed with `gte-large`, store. Prefect schedules the flow every 4 hours.
- **Recommendation.** Query embedding, Milvus similarity search, relevance filtering, then Gemini summaries explaining why each paper matches.
- **Analytics.** SQL RPC functions on PostgreSQL return publication trends, average citations over time, and top keywords, authors and domains.

## 3. Storage

- **PostgreSQL (Supabase):** paper metadata, JSON in object storage, and the SQL RPC analytics functions.
- **Milvus (Zilliz Cloud):** 1024-dimension embeddings and similarity search.
- **Dgraph:** citation relationships, read by the citation explorer.

## 4. LLM integration

- **Ingest time:** LLM keyword and domain extraction (Groq/OpenRouter), stored for analytics and filtering.
- **Query time:** Gemini 1.5 Flash writes the per-paper relevance summaries shown in the chatbot.

## 5. Streamlit UI

- **Dashboard:** keyword selector, publication and citation trend charts, top keywords, authors and domains.
- **Chatbot:** text input, retrieval, and formatted paper recommendations with summaries.
- **Citation graph:** networkx layout rendered by matplotlib as an image.

## 6. Demo mode

The demo runs the original Streamlit scripts unchanged and injects in-memory stand-ins with fictional data. Retrieval is keyword-based and summaries are canned; no external service is called.

## 7. Trade-offs and constraints

- **Strengths:** clear separation of pipeline, storage and UI; each store matches its data type; LLM extraction is done once at ingest.
- **Weaknesses:** three data services to operate; the FastAPI `/recommend` endpoint is a stub; the citation graph is a static image.
- **Caveats:** the original requires Supabase, Zilliz, Dgraph and Gemini/Groq credentials, which are not part of this portfolio.

## 8. Possible next steps

- Implement the `/recommend` API endpoint.
- Make the citation graph interactive.
- Add filtering by author or date range.
