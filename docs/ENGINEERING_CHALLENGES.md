# Engineering Challenges

Design considerations behind the system, based on the structure of the codebase. This is not a benchmark report; no performance or scale figures are claimed.

## 1. Polyglot persistence

Paper metadata, vector embeddings and the citation graph live in three different stores (PostgreSQL via Supabase, Milvus on Zilliz Cloud, Dgraph). Each fits its data shape, at the cost of three services to configure and keep in step. A Prefect flow orchestrates ingestion and storage so the stores are populated by one pipeline rather than by separate scripts.

## 2. Keeping LLM work off the read path

Keyword and domain extraction is done during ingestion (via Groq/OpenRouter) and stored, so dashboard analytics are plain SQL. At query time the only LLM call is the Gemini step that writes relevance summaries for the retrieved papers.

## 3. Relevance of retrieved papers

The chatbot embeds the query, searches Milvus for similar chunks, filters candidates for relevance, and only then asks Gemini to explain why each paper matches. Filtering before generation limits summaries of weakly related papers.

## 4. Analytics inside the database

Trend charts (publications over time, average citations, top keywords, authors and domains) are served by SQL RPC functions in PostgreSQL, so the Streamlit app receives small aggregate results instead of raw rows.

## 5. Scheduled ingestion

A Prefect flow ingests and stores papers every four hours, so the corpus refreshes without manual runs.

## 6. Running the real UI without live services

The original code needs Supabase, Zilliz, Dgraph and LLM credentials. For this portfolio the real Streamlit UI scripts run unchanged against in-memory stand-ins that return fictional data: fake SQL RPC results, keyword-matched retrieval with canned summaries in place of Milvus and Gemini, and a generated citation graph. This keeps credentials out of the portfolio and the demo reproducible.

## Known limitations

- The FastAPI `/recommend` endpoint is a stub.
- The citation graph is a static matplotlib/networkx image, not an interactive view.
- `requirements.txt` is UTF-16 encoded and very heavy.
- In demo mode, chatbot replies are canned keyword matches, not live model output.
