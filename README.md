# Langchain AI Assistant with Hybrid RAG and Memory

This project allows you to deploy your own custom AI assistant using a hybrid RAG architecture, memory, and a Streamlit-based web interface.

---

## Key Features

- **Framework**: Langchain (AI logic) + Streamlit (UI)
- **Retrieval Augmented Generation (RAG)**: Combines BM25 keyword search and semantic vector search via `EnsembleRetriever`
- **Vector Store**: ChromaDB
- **LLM Support**: OpenAI GPT-4o, Google Gemini 1.5, Anthropic Claude 3, Ollama (LLaMA 3 and others)
- **Chat Memory**: Built-in chat history using Langchain's predefined chains
- **Streaming Output**: Real-time AI response updates
- **Admin Interface**: Web scraping, PDF upload, and document ingestion into ChromaDB
- **Document Support**:

  - JSON (one chunk per item/page)
  - PDF (one chunk per page)

- **Logging**: Integrated with Langsmith

---

## Supported Tools and Services

- **Langchain**: [langchain.com](https://www.langchain.com)
- **Langsmith**: [smith.langchain.com](https://smith.langchain.com)
- **Streamlit**: [streamlit.io](https://streamlit.io)
- **ChromaDB**: [trychroma.com](https://www.trychroma.com)
- **OpenAI (GPT)**: [platform.openai.com](https://platform.openai.com)
- **Anthropic (Claude)**: [console.anthropic.com](https://console.anthropic.com)
- **Google VertexAI (Gemini)**: [cloud.google.com/vertex-ai](https://cloud.google.com/vertex-ai)
- **Ollama (LLaMA, Mistral, etc.)**: [ollama.com](https://ollama.com)

---

## Installation Guide

### Prerequisites

- OS: Linux
- Python: 3.10
- Git

### Step 1: Clone the Repository

```bash
git clone https://github.com/dodeeric/LangChain-Powered-RAG-Agent.git
cd LangChain-Powered-RAG-Agent
```

### Step 2: Create Environment Configuration

Edit the `.env` file to include your API keys (OpenAI key is required):

```bash
nano .env
```

Example:

```env
OPENAI_API_KEY=sk-proj-xxx
ANTHROPIC_API_KEY=sk-ant-xxx
LANGCHAIN_API_KEY=ls__xxx
LANGCHAIN_TRACING_V2=true
ADMIN_PASSWORD=your_password
GOOGLE_APPLICATION_CREDENTIALS=./serviceaccountxxx.json
```

> Note: The Google credential file should be a valid service account with access to VertexAI.

### Step 3: Configure the Application

```bash
nano config/config.py
```

Adjust the settings as needed (e.g., model choice, retriever type, chunk size).

### Step 4: Install Dependencies

```bash
pip install -U -r requirements.txt
```

### Step 5: Launch the Application

```bash
bash app.sh start
```

If you encounter a `streamlit: command not found` error, log out and back in again to refresh your environment path.

---

## Accessing the Application

Once launched, access the assistant in your browser:

```
http://<your-server-ip>:8080
```

### First Time Setup

1. Go to the **Admin Interface**
2. Enter your admin password
3. Scrape websites or upload PDFs
4. Embed the content into ChromaDB

---

## Optional: Use a Reverse Proxy

For production deployments, consider using **Nginx** to forward traffic from port `80` or `443` to port `8080`.

---

## Inspecting the Chroma Vector Store

```bash
cd chromadb
sqlite3 chroma.sqlite3
```

Useful SQL commands:

```sql
.tables
SELECT * FROM collections;
SELECT COUNT(*) FROM embeddings;
SELECT id, key, string_value FROM embedding_metadata LIMIT 10;
SELECT * FROM embedding_metadata WHERE string_value LIKE '%keyword%';
```

---

## Running Ollama Locally

### Step 1: Install and Start Ollama

```bash
ollama pull llama3
ollama serve
```

### Step 2: Run an LLM

```bash
ollama run llama3
```

Example:

```
> What's the capital of France?
> /bye
```

---

## Running ChromaDB as a Standalone Server

### Option A: Using Docker

```bash
docker pull chromadb/chroma
docker run -p 8081:8081 chromadb/chroma
```

### Option B: Using Python CLI

```bash
chroma run --path /path/to/db
```

---

## Summary

This assistant provides a powerful hybrid search capability with integrated memory, multi-model LLM support, and easy document ingestion—all accessible via a Streamlit interface. It’s modular and extensible for both personal and production use.
