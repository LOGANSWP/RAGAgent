# 🚀 Production-Ready RAG AI Agent

A full-stack, production-grade Retrieval-Augmented Generation (RAG) application. This project allows users to securely upload PDFs and query document-specific information using an AI assistant.

Unlike standard local RAG project, this application is built for the wild—featuring fault-tolerant asynchronous workflows, step-level observability, rate limiting, and automatic retries using Inngest.

## ✨ Key Features

* 📄 Asynchronous Document Ingestion: Built a robust pipeline utilizing LlamaIndex to parse PDFs into optimized 1,000-character chunks (with 200-character overlaps) to generate precise text embeddings.

* 🧠 High-Performance Vector Search: Utilizes a Dockerized Qdrant vector database to store embeddings, implementing cosine distance similarity search to instantly retrieve relevant document contexts.

* 🤖 Grounded LLM Orchestration: Integrates OpenAI's GPT-4o-mini via an Inngest AI adapter. The model is strictly grounded in the retrieved data and provides direct source citations for every answer.

* 🛡️ Production-Grade Reliability: Observable Workflows: Complex AI operations are broken into step-level, retryable workflows.

  * Fault Tolerance: Maintains stability during LLM provider outages or timeouts.

  * API Protection: Includes endpoint throttling and dynamic rate-limiting to prevent API abuse.

## 🛠️ Tech Stack

* Frontend: Streamlit

* Backend: Python, FastAPI

* Orchestration & Observability: Inngest

* Vector Database: Qdrant (via Docker)

* AI & Machine Learning: LlamaIndex, OpenAI API (GPT-4o-mini, text-embedding-3-large)

* Package Management: uv

## 🚀 Getting Started

### Prerequisites

* Python 3.10+

* Docker Desktop (for Qdrant)

* Node.js (required to run the Inngest local dev server)

* OpenAI API Key

### 1. Clone the repository
```
git clone https://github.com/LOGANSWP/RAGAgent.git
cd RAGAgent
```

### 2. Install dependencies

This project uses uv for lightning-fast Python package management.
```
uv init .
uv add fastapi inngest llama-index-core llama-index-readers-file python-dotenv qdrant-client uvicorn streamlit openai pydantic
```

### 3. Environment Variables

Create a .env file in the root of your project and add your OpenAI API key:
```
OPENAI_API_KEY=your_openai_api_key_here
```

### 4. Start the Vector Database (Qdrant)

Run Qdrant locally using Docker. First, ensure you have a qdrant_storage folder in your root directory.
```
# Mac/Linux:
docker run -d --name qdrant -p 6333:6333 -v $(pwd)/qdrant_storage:/qdrant/storage qdrant/qdrant

# Windows (PowerShell):
docker run -d --name qdrant -p 6333:6333 -v ${PWD}/qdrant_storage:/qdrant/storage qdrant/qdrant
```

### 5. Run the Application Services

You will need three separate terminal windows to run the Backend, the Orchestrator, and the Frontend.

Terminal 1: Start the FastAPI Backend
```
uv run uvicorn main:app
```

Terminal 2: Start the Inngest Dev Server
This server tracks your functions, provides the UI for observability, and handles retries.
```
npx inngest-cli@latest dev -u [http://127.0.0.1:8000/api/inngest](http://127.0.0.1:8000/api/inngest) --no-discovery
```

You can view the Inngest dashboard at http://localhost:8288.

Terminal 3: Start the Streamlit Frontend
```
uv run streamlit run streamlit_app.py
```

## 💡 Usage

1. Open the Streamlit web app in your browser (usually http://localhost:8501).

2. Use the sidebar to upload a PDF document.

3. Behind the scenes, the FastAPI server chunks the document, embeds it via OpenAI, and stores the vectors in Qdrant. You can watch this process step-by-step in the Inngest dashboard.

4. Type a question into the chat interface. The system will retrieve the most relevant chunks and generate a cited response!

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the issues page.

## 📝 License

This project is MIT licensed.

