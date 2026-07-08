<p align="center">
  <h1 align="center">🧠 DocuMind AI – Agentic RAG PDF Assistant</h1>
  <p align="center">
    <strong>Chat with your PDFs using Agentic RAG — powered by LLaMA 3.3, Gemini Embeddings & Pinecone</strong>
  </p>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Node.js-18+-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js" />
  <img src="https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=black" alt="React" />
  <img src="https://img.shields.io/badge/MongoDB-Atlas-47A248?style=for-the-badge&logo=mongodb&logoColor=white" alt="MongoDB" />
  <img src="https://img.shields.io/badge/Express-4.x-000000?style=for-the-badge&logo=express&logoColor=white" alt="Express" />
  <img src="https://img.shields.io/badge/Vite-5-646CFF?style=for-the-badge&logo=vite&logoColor=white" alt="Vite" />
  <img src="https://img.shields.io/badge/LangChain-JS-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white" alt="LangChain" />
  <img src="https://img.shields.io/badge/Pinecone-Vector_DB-000000?style=for-the-badge&logo=pinecone&logoColor=white" alt="Pinecone" />
  <img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="License" />
</p>

---

## 📖 Overview

**DocuMind AI** is a full-stack **MERN** Retrieval-Augmented Generation (RAG) platform that lets users upload PDF documents and have AI-powered conversations with them. It combines **Google Gemini embeddings**, **Groq's ultra-fast LLaMA 3.3 70B inference**, and **Pinecone cloud vector storage** to deliver accurate, sub-second answers grounded in your documents — complete with a built-in groundedness evaluator.

> Upload a PDF → Ask questions in natural language → Get cited, evaluated answers instantly.

---

## 🌐 Live Demo

> *https://documind-ai-hazel.vercel.app/*

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🔐 **JWT Authentication** | Secure user registration and login with JWT and bcrypt password hashing |
| 📄 **PDF Upload** | Drag-and-drop PDF upload with document processing |
| ⚡ **RAG Pipeline** | Automatic text extraction, chunking, embedding generation, and vector indexing |
| 🤖 **Agentic Query Routing** | Routes queries to Greeting, Summary, or QA workflows using an LLM |
| 🚀 **LLM-Powered Responses** | Context-aware answers generated using Groq's LLaMA 3.3 model |
| 🛡️ **Groundedness Evaluation** | Evaluates response groundedness against retrieved document context |
| 📋 **Source Inspector** | Displays retrieved source chunks and evaluation details for transparency |
| 🌙 **Responsive Dark UI** | Modern React interface with drag-and-drop document upload |
| 💾 **Persistent Storage** | Stores users and chat history in MongoDB, vector embeddings in Pinecone |

---

## 🛠️ Tech Stack

<table>
  <tr>
    <th>Layer</th>
    <th>Technologies</th>
  </tr>
  <tr>
    <td><strong>Frontend</strong></td>
    <td>React, Vite, Axios, Lucide React, CSS</td>
  </tr>
  <tr>
    <td><strong>Backend</strong></td>
    <td>Node.js, Express.js, MongoDB Atlas, Mongoose, JWT, bcryptjs, Multer</td>
  </tr>
  <tr>
    <td><strong>AI / RAG</strong></td>
    <td>LangChain.js, Google Gemini Embeddings (<code>gemini-embedding-001</code>), Groq (<code>llama-3.3-70b-versatile</code>)</td>
  </tr>
  <tr>
    <td><strong>Vector Database</strong></td>
    <td>Pinecone, MemoryVectorStore (fallback)</td>
  </tr>
  <tr>
    <td><strong>Deployment</strong></td>
    <td>Vercel, Render</td>
  </tr>
</table>

---

## ⚙️ Architecture

```text
                        DocuMind AI Architecture

                    ┌──────────────────────────┐
                    │     React Frontend       │
                    │  Authentication & Chat   │
                    └────────────┬─────────────┘
                                 │
                          HTTP / REST API
                                 │
                    ┌────────────▼─────────────┐
                    │     Express Backend      │
                    │ JWT Auth & Document APIs │
                    └───────┬─────────┬────────┘
                            │         │
          ┌─────────────────▼───┐     │
          │   MongoDB Atlas      │     │
          │ Users │ Docs │ Chats │     │
          └──────────────────────┘     │
                                       ▼
                            ┌──────────────────┐
                            │    RAG Service   │
                            ├──────────────────┤
                            │ Intent Routing   │
                            │ Document Parsing │
                            │ Embeddings       │
                            │ Retrieval        │
                            │ Re-ranking       │
                            │ Answer Generation│
                            │ Groundedness Eval│
                            └───────┬──────────┘
                                    │
          ┌───────────────┬─────────┴──────────┬──────────────┐
          ▼               ▼                    ▼              ▼
 Google Gemini      Pinecone Vector DB    Groq LLaMA     Memory Store
  Embeddings       (Semantic Retrieval)   3.3 70B        (Fallback)
```

### 🔄 RAG Pipeline

```text
                    PDF Upload
                        │
                        ▼
                Extract PDF Text
                        │
                        ▼
      Recursive Text Chunking (1000 / 200)
                        │
                        ▼
      Generate Embeddings (Gemini)
                        │
                        ▼
 Store Embeddings (Pinecone)
        │
        └──► Fallback: MemoryVectorStore
────────────────────────────────────────────────

                   User Question
                        │
                        ▼
        Intent Classification (LLM)
        ├── Greeting
        ├── Summary
        └── Question Answering
                        │
                        ▼
     Semantic Similarity Search (Pinecone)
                        │
                        ▼
 Keyword-based Re-ranking (70% Semantic +
       30% Keyword Relevance)
                        │
                        ▼
      Build RAG Prompt + Chat History
                        │
                        ▼
     Generate Answer (Groq LLaMA 3.3)
                        │
                        ▼
      Groundedness Evaluation (LLM)
                        │
                        ▼
 Return Answer + Source Chunks + Audit Score
```

## 📋 Prerequisites

- Node.js 18+
- MongoDB Atlas
- Google AI Studio API Key
- Groq API Key
- Pinecone Account & Index

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/kanav19jain/Documind-AI.git
cd Documind-AI
```

### 2. Backend

```bash
cd backend
npm install
cp .env.example .env
```

Configure the `.env` file with your MongoDB, Gemini, Groq, and Pinecone credentials, then start the server:

```bash
npm run dev
```

### 3. Frontend

```bash
cd frontend
npm install
cp .env.example .env
```

Set:

```env
VITE_API_URL=http://localhost:5000
```

Start the frontend:

```bash
npm run dev
```

Open **http://localhost:5173**.

---

## 🌐 Deployment

- **Backend:** Deploy `backend/` to Render and configure the environment variables from `.env.example`.
- **Frontend:** Deploy `frontend/` to Vercel and set `VITE_API_URL` to your Render backend URL.

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
