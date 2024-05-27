# Langchain AI Assistant with Hybrid RAG and Memory

This application can be configured (see config.py) to create your own specialized AI assistant.

- AI Python framework: Langchain
- Web interface Python framework: Streamlit
- Vector DB: Chroma
- Hybrid RAG: bm25 keyword search and vector db semantic search (BM25Retriever + vector_db.as_retriever = EnsembleRetriever). Hybrid RAG improves greatly the efficiency of the RAG search.
- Chat history (use of predefined chains: history_aware_retriever, stuff_documents_chain, retrieval_chain)
- Streaming of the AI answer
- Logs sent to Langsmith
- AI Models: OpenAI GPT 4o, Google Gemini 1.5, Anthropic Claude 3, Ollama (Llama 3, etc.). Vector size: 3072.
- Admin interface (scrape web pages, upload PDF files, embed in vector DB)
- Files ingestion into the vector DB: JSON files (one JSON item / web page per chunk) and PDF files (one PDF page per chunk)
 
Frameworks and tools:

- Langchain: https://www.langchain.com (Python framework for AI applications)
- Langsmith: https://smith.langchain.com (logs and debug for LangChain applications)
- Streamlit: https://streamlit.io (Python framework for web interfaces for data / AI applications)
- Chroma: https://www.trychroma.com (Vector DB)
- OpenAI (GPT): https://platform.openai.com (LLM)
- Anthropic (Claude): https://console.anthropic.com/dashboard (LLM)

Installation:

```
$ git clone https://github.com/dodeeric/langchain-ai-assistant-with-hybrid-rag.git
$ cd langchain-ai-assistant-with-hybrid-rag
```

Add your API keys (only the OpenAI API key is mandatory):

```
$ nano .env
```

```
OPENAI_API_KEY = "sk-proj-xxx"       ==> Go to https://platform.openai.com/api-keys
ANTHROPIC_API_KEY = "sk-ant-xxx"     ==> Go to https://console.anthropic.com/settings/keys
LANGCHAIN_API_KEY = "ls__xxx"        ==> Go to https://smith.langchain.com (Langsmith)
LANGCHAIN_TRACING_V2 = "true"        ==> Set to false if you will not use Langsmith traces
ADMIN_PASSWORD = "xxx"               ==> You chose your password
GOOGLE_APPLICATION_CREDENTIALS = "./serviceaccountxxx.json"  ==> Path to the Service Account (with VertexAI role) JSON file
```

Install required libraries:

```
$ pip install -U -r requirements.txt
```

Launch the AI Assistant:

```
$ bash app.sh start
```

Go to: http://IP:8080

Go first to the admin interface (introduce the admin password), and scrape some web pages and/or upload some PDF files, then embed them to the vector DB.

Check the Chroma vector DB: (OPTIONAL)

```
$ cd chromadb
$ sqlite3 chroma.sqlite3
```
```
sqlite> .tables                            ===> List of the tables
sqlite> select * from collections;         ===> Name of the collection (bmae) & size of the vectors (3072)
sqlite> select count(*) from embeddings;   ===> Number of records in the DB
sqlite> select id, key, string_value from embedding_metadata LIMIT 10 OFFSET 0;       ===> Display JSON items and PDF pages
sqlite> PRAGMA table_info(embedding_metadata);                                        ===> Structure of the table   
sqlite> select * from embedding_metadata where string_value like '%Delper%';          ===> Display matching records
sqlite> select count(*) from embedding_metadata where string_value like '%Delper%';   ===> Display number of matching records
```

---

Running Ollama / Llama 3 (or another LLM) locally:

Install Ollama, then:

```
$ ollama pull llama3
$ ollama list
$ ollama serve
```

In a new window:

```
$ ollama run llama3
>>> What's the capital of France?
>>> /bye
```
