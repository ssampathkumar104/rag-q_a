# Retrieval-Augmented Generation (RAG) - Q & A
Context based q & a from PDF documents

---

## Introduction

rag-q_a is a lightweight Retrieval-Augmented Generation (RAG) demo that turns a collection of PDF research papers into a context-aware question-and-answer assistant. It provides a Streamlit UI for ingesting PDFs, building vector embeddings (FAISS), and querying a Groq-hosted LLM (ChatGroq using the llama-3.1-8b-instant model). The app returns answers grounded in the document snippets it retrieves so responses are traceable back to the source PDFs.

This project is useful for researchers, students, or engineers who want a quick way to ask natural-language questions over a folder of PDF documents without building an index and search pipeline from scratch.

## Key features

- Simple Streamlit web UI for uploading/placing PDFs and asking questions.
- PDF ingestion using a PyPDF-based loader and chunking with RecursiveCharacterTextSplitter.
- Embeddings generated via OpenAI embeddings (configurable) and stored in an in-memory FAISS vector store.
- Retrieval chain built with LangChain-style components and a Groq LLM for generation.
- Shows the supporting document snippets used to produce each answer.

## Stack / Notable libraries

- Language: Python
- Web app: Streamlit
- Vector store: FAISS (faiss-cpu)
- Document loader: langchain_community.document_loaders.PyPDFDirectoryLoader
- Text splitter: langchain_text_splitters.RecursiveCharacterTextSplitter
- Embeddings: OpenAIEmbeddings (configurable to other embeddings)
- LLM: ChatGroq (groq) using llama-3.1-8b-instant
- Orchestration: langchain, langchain_classic, langchain_core

See `requirements.txt` for the full dependency list.

## How it works (quick overview)

1. Place PDF files into the `research_papers/` directory.
2. Click "Document Embedding" in the Streamlit UI to load PDFs, split into chunks, compute embeddings, and build a FAISS index.
3. Enter a natural-language question. The app retrieves the most similar document chunks and passes them (as context) to the LLM prompt to generate an answer.
4. The UI displays both the generated answer and the retrieved document snippets used as evidence.

## Getting started (shortest path)

1. Clone the repository:

```bash
git clone https://github.com/ssampathkumar104/rag-q_a.git
cd rag-q_a
```

2. Set up a Python virtual environment and install dependencies:

```bash
python -m venv venv
source venv/bin/activate  # macOS / Linux
venv\Scripts\activate     # Windows (PowerShell)

pip install -r requirements.txt
```

3. Configure API keys. Create a `.env` file (already present as a template) or export the env vars:

```env
OPENAI_API_KEY="your_api_key"
GROQ_API_KEY="your_api_key"
```

4. Add your PDF files to `research_papers/`.

5. Run the Streamlit app:

```bash
streamlit run app.py
```

6. In the app: click "Document Embedding" to build the vector index, then type queries into the input box to get answers.

## Environment variables

- OPENAI_API_KEY: API key for OpenAI embeddings (used via langchain_openai.OpenAIEmbeddings).
- GROQ_API_KEY: API key for Groq Chat endpoint (used by ChatGroq).

Make sure both are set before creating embeddings or invoking the LLM.

## Notes and limitations

- The current app uses an in-memory FAISS index that is rebuilt each time you click "Document Embedding". For larger corpora or persistence across runs, swap to a persisted store or serialize FAISS to disk.
- The text splitter currently uses chunk_size=1000 and chunk_overlap=200 and only splits the first 50 loaded documents (see `app.py`). Adjust these parameters for your dataset.
- Responses are only as good as the retrieved context; always inspect the provided document snippets for accuracy.

## Where to look in the code

- `app.py` — main Streamlit app and RAG pipeline wiring (loader, splitter, embeddings, FAISS, retrieval chain, prompt template).
- `requirements.txt` — dependencies to install.
- `research_papers/` — place your PDFs here for ingestion.

---

*README updated: added detailed introduction, quickstart, and usage guidance.*
