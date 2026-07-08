<p align="center">
  <h1 align="center">🧠 DocuMind AI</h1>
  <p align="center">
    <strong>Chat with your PDFs using Agentic RAG — powered by LLaMA 3.3, Gemini Embeddings & Pinecone</strong>
  </p>
  <p align="center">
    <a href="#-features">Features</a> •
    <a href="#%EF%B8%8F-architecture">Architecture</a> •
    <a href="#-getting-started">Getting Started</a> •
    <a href="#-deployment">Deployment</a> •
    <a href="#-api-reference">API Reference</a>
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

**DocuMind AI** is a full-stack **MERN** Retrieval-Augmented Generation (RAG) platform that lets users upload PDF documents and have AI-powered conversations with them. It combines **Google Gemini embeddings**, **Groq's ultra-fast LLaMA 3.3 70B inference**, and **Pinecone cloud vector storage** to deliver accurate, sub-second answers grounded in your documents — complete with a built-in hallucination auditor.

> Upload a PDF → Ask questions in natural language → Get cited, evaluated answers instantly.

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🔐 **JWT Authentication** | Secure register & login with bcrypt password hashing |
| 📄 **PDF Upload** | Drag-and-drop PDF upload with real-time processing status |
| ⚡ **Real-Time Processing** | Visual pipeline indicators: *Uploading → Extracting → Chunking → Vectorizing* |
| 🤖 **Agentic Query Routing** | LLM classifier routes queries to Greet / Summary / QA intents |
| 🔍 **Hybrid Search Retrieval** | Dense vector + sparse keyword fusion (70/30 weighted) |
| 🚀 **Ultra-Fast Inference** | Sub-second responses via Groq LPU hardware |
| 🛡️ **Hallucination Auditor** | LLM-as-a-Judge scores groundedness (0–100%) against source context |
| 📋 **RAG Auditor Panel** | Inspect matched source chunks and evaluation scores |
| 🌙 **Premium Dark UI** | Glassmorphism effects with a custom dark-theme design system |
| 💾 **Persistent Storage** | Chat history in MongoDB, vectors in Pinecone Cloud |

---

## 🛠️ Tech Stack

<table>
  <tr>
    <th>Layer</th>
    <th>Technology</th>
  </tr>
  <tr>
    <td><strong>Frontend</strong></td>
    <td>React 19 (Vite), Axios, Lucide React Icons, Custom Dark-Theme CSS</td>
  </tr>
  <tr>
    <td><strong>Backend</strong></td>
    <td>Node.js, Express, MongoDB Atlas (Mongoose), JWT Auth (bcryptjs), Multer</td>
  </tr>
  <tr>
    <td><strong>AI / ML Pipeline</strong></td>
    <td>LangChain.js, Google Gemini (<code>gemini-embedding-001</code>, 3072-dim), Groq (<code>llama-3.3-70b-versatile</code>)</td>
  </tr>
  <tr>
    <td><strong>Vector Store</strong></td>
    <td>Pinecone Cloud (primary), MemoryVectorStore (fallback)</td>
  </tr>
</table>

---

## ⚙️ Architecture

```
┌────────────────────────────────────────────────────────────────────┐
│                        DocuMind AI Architecture                    │
├────────────────────────────────────────────────────────────────────┤
│                                                                    │
│  ┌──────────┐     ┌──────────────┐     ┌────────────────────────┐ │
│  │  React    │────▶│  Express API │────▶│   RAG Service          │ │
│  │  Frontend │◀────│  (JWT Auth)  │◀────│                        │ │
│  └──────────┘     └──────┬───────┘     │  ┌──────────────────┐  │ │
│                          │             │  │ 1. Query Router   │  │ │
│                          │             │  │    (Intent LLM)   │  │ │
│                   ┌──────▼───────┐     │  ├──────────────────┤  │ │
│                   │   MongoDB    │     │  │ 2. Hybrid Search  │  │ │
│                   │   Atlas      │     │  │   Dense + Sparse  │  │ │
│                   │  ┌─────────┐ │     │  ├──────────────────┤  │ │
│                   │  │ Users   │ │     │  │ 3. LLM Generator  │  │ │
│                   │  │ Docs    │ │     │  │   (Groq LLaMA)    │  │ │
│                   │  │ Chats   │ │     │  ├──────────────────┤  │ │
│                   │  └─────────┘ │     │  │ 4. LLM Auditor    │  │ │
│                   └──────────────┘     │  │   (Groundedness)  │  │ │
│                                        │  └──────────────────┘  │ │
│                   ┌──────────────┐     │           │             │ │
│                   │   Pinecone   │◀────┼───────────┘             │ │
│                   │  (Vectors)   │     │                        │ │
│                   └──────────────┘     └────────────────────────┘ │
│                                                                    │
│                   ┌──────────────┐     ┌────────────────────────┐ │
│                   │ Google Gemini│     │      Groq LPU          │ │
│                   │ (Embeddings) │     │  (LLaMA 3.3 70B)       │ │
│                   └──────────────┘     └────────────────────────┘ │
└────────────────────────────────────────────────────────────────────┘
```

### 🔄 RAG Pipeline — Step by Step

```
User Query
    │
    ▼
┌─────────────────────────────────────────────────────────┐
│  Step 1: Agentic Query Routing                          │
│  LLM classifies intent → GREET | SUMMARY | QA          │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│  Step 2: Hybrid Retrieval                               │
│  Dense similarity (Pinecone) ──┐                        │
│  Sparse keyword (BM25-style) ──┼──▶ Weighted Fusion     │
│                        70%     │     30%                 │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│  Step 3: Context-Aware Generation                       │
│  Retrieved chunks → RAG prompt → Groq LLaMA 3.3 70B    │
│  Sub-second latency via Groq LPU inference              │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│  Step 4: LLM-as-a-Judge Evaluation                      │
│  Secondary auditor LLM scores groundedness (0–100%)     │
│  Hallucination detection against source context          │
└─────────────────────────────────────────────────────────┘
```

---

## 📂 Project Structure

```
documind-ai/
├── README.md
├── .gitignore
├── backend/
│   ├── server.js                  # Express server, MongoDB connection, CORS
│   ├── package.json
│   ├── .env.example               # Environment variable template
│   ├── middleware/
│   │   └── auth.js                # JWT verification middleware
│   ├── models/
│   │   ├── User.js                # User schema with bcrypt pre-save hook
│   │   ├── Document.js            # Document metadata schema
│   │   └── Chat.js                # Chat history schema with sources & eval
│   ├── routes/
│   │   ├── auth.js                # Register & Login endpoints
│   │   ├── documents.js           # PDF upload & document listing
│   │   └── chats.js               # Query endpoint & chat history
│   └── services/
│       └── ragService.js          # Core RAG pipeline (chunk, embed, retrieve, generate, evaluate)
└── frontend/
    ├── index.html
    ├── package.json
    ├── vite.config.js
    ├── .env.example                # Frontend env template
    └── src/
        ├── main.jsx
        ├── App.jsx                 # Root component with auth routing & dashboard layout
        ├── index.css               # Complete dark-theme design system
        ├── config/
        │   └── auth.js             # JWT auth client (Axios-based)
        └── components/
            ├── Login.jsx           # Login form
            ├── Register.jsx        # Registration form
            ├── DocumentManager.jsx # PDF upload & document selector
            ├── ChatWindow.jsx      # Chat interface with message history
            └── SourceInspector.jsx  # RAG auditor panel (sources + evaluation)
```

---

## 📋 Prerequisites

Before you begin, ensure you have the following:

| Requirement | Details |
|-------------|---------|
| **Node.js** | v18 or higher ([download](https://nodejs.org)) |
| **MongoDB Atlas** | Free tier account → [mongodb.com/atlas](https://www.mongodb.com/atlas) |
| **Google AI Studio** | Free API key for Gemini embeddings → [aistudio.google.com](https://aistudio.google.com) |
| **Groq** | Free tier API key for LLM inference → [console.groq.com](https://console.groq.com) |
| **Pinecone** | Free tier account for vector storage → [app.pinecone.io](https://app.pinecone.io) |

---

## 🚀 Getting Started

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/your-username/documind-ai.git
cd documind-ai
```

### 2️⃣ Backend Setup

```bash
cd backend
npm install
cp .env.example .env
```

Edit `.env` with your actual credentials:

```env
PORT=5000
NODE_ENV=development
MONGODB_URI=mongodb+srv://<user>:<pass>@cluster.mongodb.net/documind
JWT_SECRET=your_super_secret_jwt_key

# AI Services
GEMINI_API_KEY=your_google_ai_studio_key
GROQ_API_KEY=your_groq_api_key

# Pinecone
USE_PINECONE=true
PINECONE_API_KEY=your_pinecone_api_key
PINECONE_INDEX_NAME=your_index_name

# CORS
FRONTEND_URL=http://localhost:5173
```

Start the backend:

```bash
npm run dev
```

### 3️⃣ Pinecone Index Setup

1. Go to [app.pinecone.io](https://app.pinecone.io)
2. Create a **new index** with:
   - **Dimensions:** `3072`
   - **Metric:** `cosine`
3. Copy the index name into your backend `.env` file

### 4️⃣ Frontend Setup

```bash
cd frontend
npm install
```

Create the frontend environment file:

```env
VITE_API_URL=http://localhost:5000
```

Start the development server:

```bash
npm run dev
```

Open [http://localhost:5173](http://localhost:5173) in your browser 🎉

---

## 🌐 Deployment

### Backend → Render

1. Create a new **Web Service** on [render.com](https://render.com)
2. Connect your GitHub repository
3. Configure the service:

   | Setting | Value |
   |---------|-------|
   | **Root Directory** | `backend` |
   | **Build Command** | `npm install` |
   | **Start Command** | `npm start` |

4. Add all environment variables from `.env.example`
5. Set `FRONTEND_URL` to your Vercel frontend URL

### Frontend → Vercel

1. Import your GitHub repo on [vercel.com](https://vercel.com)
2. Configure the project:

   | Setting | Value |
   |---------|-------|
   | **Root Directory** | `frontend` |
   | **Framework Preset** | `Vite` |

3. Add environment variable:
   - `VITE_API_URL` = your Render backend URL
4. Deploy 🚀

---

## 📡 API Reference

All endpoints prefixed with `/api`. Protected routes require a `Bearer` token in the `Authorization` header.

| Method | Endpoint | Description | Auth Required |
|:------:|----------|-------------|:-------------:|
| `POST` | `/api/auth/register` | Register a new user | ❌ |
| `POST` | `/api/auth/login` | Login & receive JWT token | ❌ |
| `POST` | `/api/upload` | Upload a PDF document | ✅ |
| `GET`  | `/api/documents` | List user's uploaded documents | ✅ |
| `POST` | `/api/query` | Query a document using RAG | ✅ |
| `GET`  | `/api/chats/:documentId` | Retrieve chat history for a document | ✅ |

### Example: Query a Document

```bash
curl -X POST http://localhost:5000/api/query \
  -H "Authorization: Bearer <your_jwt_token>" \
  -H "Content-Type: application/json" \
  -d '{
    "documentId": "664f1a2b3c...",
    "query": "What are the main conclusions of this paper?"
  }'
```

**Response:**

```json
{
  "answer": "The paper concludes that...",
  "sources": [
    {
      "content": "...relevant chunk text...",
      "score": 0.92
    }
  ],
  "intent": "QA",
  "evaluation": {
    "groundedness": 87,
    "explanation": "The answer is well-supported by the source material..."
  }
}
```

---

## 🔐 Environment Variables

### Backend (`backend/.env`)

| Variable | Description |
|----------|-------------|
| `PORT` | Server port (default: `5000`) |
| `NODE_ENV` | Environment (`development` / `production`) |
| `MONGODB_URI` | MongoDB Atlas connection string |
| `JWT_SECRET` | Secret key for JWT token signing |
| `GEMINI_API_KEY` | Google AI Studio API key (Gemini embeddings) |
| `GROQ_API_KEY` | Groq API key (LLaMA 3.3 inference) |
| `USE_PINECONE` | Enable Pinecone vector store (`true` / `false`) |
| `PINECONE_API_KEY` | Pinecone API key |
| `PINECONE_INDEX_NAME` | Name of your Pinecone index |
| `FRONTEND_URL` | Frontend URL for CORS (e.g., `http://localhost:5173`) |

### Frontend (`frontend/.env`)

| Variable | Description |
|----------|-------------|
| `VITE_API_URL` | Backend API base URL (e.g., `http://localhost:5000`) |

---

## 🤝 Contributing

Contributions are welcome! Here's how to get started:

1. **Fork** the repository
2. **Create** a feature branch: `git checkout -b feature/amazing-feature`
3. **Commit** your changes: `git commit -m "Add amazing feature"`
4. **Push** to the branch: `git push origin feature/amazing-feature`
5. **Open** a Pull Request

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2025 DocuMind AI

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

<p align="center">
  Built with ❤️ using the MERN Stack, LangChain, and Groq
</p>
