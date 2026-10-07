# Privacy and Source

## Source code

The production source code for Research-Assistant-X is private and not included in this repository. The portfolio includes:

- High-level architecture and technical design documentation.
- Screenshots of the Streamlit UI with demo data (see below).
- Analysis notes on engineering decisions and trade-offs.

The original codebase remains on the production server and is not reproduced here.

## Demo data

All screenshots and demo output use **entirely fictional data:**

- Paper titles, authors, and abstracts are generated, not from real research.
- Citation networks are synthetic, created for demonstration only.
- Chatbot responses are pre-written examples, not live LLM output.
- The demo runs locally with in-memory stubs (no real vector search, no real Gemini API calls).

No real academic papers, researchers, or metadata are displayed in this portfolio.

## Third-party services

The original application integrates with several external services. None of the API credentials, configuration, or private keys are included:

- **arXiv API:** Metadata for papers (titles, abstracts, authors).
- **Semantic Scholar API:** Citation data and paper metadata.
- **Supabase (PostgreSQL):** Relational data storage (production key excluded).
- **Zilliz (Milvus):** Vector embeddings and similarity search (production cluster excluded).
- **Dgraph:** Citation graph database (production credentials excluded).
- **Google Gemini API:** LLM for paper summarization (API key excluded).
- **Groq/OpenRouter:** LLM for batch processing (API keys excluded).
- **Hugging Face Spaces:** Deployment platform (credentials excluded).

The demo mode does not call any external APIs. All responses are generated locally.

## Sensitive files excluded

The following files are not included to protect security and privacy:

- `.env` files (API keys, database passwords).
- Configuration files with server URLs and cluster identifiers.
- Database credentials and connection strings.
- LLM API keys (Gemini, Groq, OpenRouter).
- Supabase service role keys.
- Zilliz API credentials.

## Dependencies

Open-source libraries named here (sentence-transformers, Streamlit, Prefect, networkx, pandas, matplotlib) and the database engines retain their own licences; nothing from them is redistributed in this repository.

## Demo runtime

The demo in this portfolio runs locally with in-memory stand-ins; no data is sent to external services and all data is discarded when the session ends.
