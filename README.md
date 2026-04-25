<p align="center">
  <img src="https://img.shields.io/badge/python-3.10+-3776ab?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" />
  <img src="https://img.shields.io/badge/Groq-Whisper%20%2B%20Llama%203.3-f97316?style=for-the-badge" />
  <img src="https://img.shields.io/badge/FAISS-Vector%20Search-7c3aed?style=for-the-badge" />
  <img src="https://img.shields.io/badge/license-MIT-22c55e?style=for-the-badge" />
</p>

<h1 align="center">⚡ EchoQuery</h1>
<h3 align="center">Multi-Modal RAG Platform — Ask Questions About Your Videos, PDFs & Documents</h3>

<p align="center">
  Upload a YouTube video, research paper, or any text file.<br>
  AI transcribes, indexes, and makes it searchable — then answers your questions<br>
  with <strong>cited sources</strong>, <strong>timestamps</strong>, and <strong>page references</strong>.
</p>

---

## 🎯 What is EchoQuery?

**EchoQuery** is an AI-powered Retrieval-Augmented Generation (RAG) platform that transforms unstructured content into a searchable knowledge base. Unlike generic chatbots, every answer EchoQuery gives is **grounded in your uploaded content** — with exact citations so you can verify.

### The Problem
You have a 45-minute lecture video, a 60-page research paper, and meeting notes scattered across files. Finding specific information means scrubbing through timelines or skimming pages.

### The Solution
Upload everything into EchoQuery. Ask *"What methodology was used in the experiment?"* and get a precise answer pointing to **Page 12** of the paper and **timestamp 23:45** of the lecture video — in seconds.

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🎬 **YouTube Video Processing** | Paste any YouTube URL — audio is downloaded, transcribed with Groq Whisper, and indexed with timestamps |
| 📕 **PDF Document Analysis** | Upload research papers, reports, or manuals — text is extracted page-by-page using PyMuPDF |
| 📝 **Text File Support** | TXT, Markdown, CSV, JSON, HTML, Python, and more — any text-based file can be indexed |
| 💬 **Natural Language Q&A** | Ask questions in plain English and get AI-generated answers from Llama 3.3 70B |
| 📍 **Source Citations** | Every answer includes the exact source — video timestamps, PDF page numbers, or document sections |
| ⚡ **Real-Time Pipeline** | Watch the processing happen step-by-step with a live progress tracker |
| 📚 **Content Library** | Browse all your indexed content, generate summaries, or jump straight to asking questions |
| 🔍 **Semantic Search** | Powered by FAISS vector search with sentence-transformer embeddings for meaning-based retrieval |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        FRONTEND                             │
│  HTML/CSS/JS  •  Glassmorphism UI  •  SSE Live Updates      │
└───────────────────────┬─────────────────────────────────────┘
                        │ REST API
┌───────────────────────▼─────────────────────────────────────┐
│                      FASTAPI BACKEND                        │
│                                                             │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────────┐  │
│  │ Ingestion│  │Transcribe│  │ Chunking │  │ Embedding  │  │
│  │ yt-dlp   │  │Groq      │  │ 30s wins │  │ MiniLM-L6  │  │
│  └────┬─────┘  │Whisper   │  │ / pages  │  │ → FAISS    │  │
│       │        └────┬─────┘  └────┬─────┘  └─────┬──────┘  │
│       │             │             │               │         │
│  ┌────▼─────────────▼─────────────▼───────────────▼──────┐  │
│  │                   VECTOR STORE (FAISS)                 │  │
│  │           + SQLite Metadata (SQLAlchemy)               │  │
│  └────────────────────────┬──────────────────────────────┘  │
│                           │                                 │
│  ┌────────────────────────▼──────────────────────────────┐  │
│  │            RETRIEVAL + GENERATION                     │  │
│  │     Semantic Search → Context Assembly → Llama 3.3    │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 Quick Start

### Prerequisites

- **Python 3.10+**
- **Groq API Key** — [Get one free at groq.com](https://console.groq.com/)
- **FFmpeg** — Required by `yt-dlp` for audio extraction

### 1. Clone the Repository

```bash
git clone https://github.com/navajyothb/EchoQuery-Multimodal-Search-Discovery-Agent.git
cd echoquery
```

### 2. Create a Virtual Environment

```bash
python -m venv venv
source venv/bin/activate        # macOS/Linux
# venv\Scripts\activate         # Windows
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Environment

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key_here
```

### 5. Run the Server

```bash
uvicorn app.main:app --reload
```

Open **http://127.0.0.1:8000** in your browser. That's it! 🎉

---

## 📖 Usage Guide

### Processing a YouTube Video

1. Click **"Add Content"** in the navigation bar
2. Select the **"YouTube Video"** tab
3. Paste a YouTube URL (e.g., `https://www.youtube.com/watch?v=...`)
4. Click **"🚀 Process Video"**
5. Watch the live pipeline: Download → Transcribe → Chunk → Embed → Done

### Uploading a PDF or Text File

1. Click **"Add Content"** → select **"PDF / Document"** tab
2. Drag & drop your file into the upload zone (or click to browse)
3. Supported formats: `PDF`, `TXT`, `MD`, `CSV`, `JSON`, `HTML`, `PY`, `JS`
4. Click **"🚀 Process Document"**

### Asking Questions

1. Click **"Ask AI"** in the navigation bar
2. Type your question in plain English
3. AI searches across **all** your indexed content (videos + documents)
4. Get answers with **source citations** — timestamps for videos, page numbers for PDFs

### Generating Summaries

1. Click **"Library"** to see all processed content
2. Click **"📝 Summary"** on any item
3. AI generates a comprehensive summary of the entire content

---

## 📁 Project Structure

```
echoquery/
├── app/
│   ├── main.py                 # FastAPI app entry point
│   ├── config.py               # Environment & configuration
│   ├── api/
│   │   └── routes.py           # REST API endpoints
│   ├── db/
│   │   ├── database.py         # SQLAlchemy engine & session
│   │   ├── models.py           # Video & Chunk ORM models
│   │   └── crud.py             # Database operations
│   └── services/
│       ├── ingestion.py        # YouTube audio download (yt-dlp)
│       ├── transcription.py    # Groq Whisper transcription
│       ├── document.py         # PDF & text file extraction
│       ├── chunking.py         # Transcript/document chunking
│       ├── embedding.py        # Sentence-transformer embeddings
│       ├── vector_store.py     # FAISS index management
│       ├── retrieval.py        # Semantic search
│       └── generator.py        # Llama 3.3 answer generation
├── frontend/
│   ├── index.html              # Single-page application
│   ├── style.css               # Dark-mode glassmorphism design
│   └── app.js                  # Client-side logic
├── data/
│   ├── audio/                  # Downloaded audio files (gitignored)
│   ├── faiss/                  # FAISS index + metadata (gitignored)
│   └── uploads/                # Uploaded documents
├── requirements.txt
├── .env                        # API keys (gitignored)
└── .gitignore
```

---

## 🔌 API Reference

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/process/start?url={youtube_url}` | Start processing a YouTube video |
| `POST` | `/api/upload/start` | Upload and process a document (multipart/form-data) |
| `GET` | `/api/process/status/{job_id}` | Get current job status |
| `GET` | `/api/process/stream/{job_id}` | SSE stream of job progress |
| `GET` | `/api/query?q={question}` | Ask a question across all content |
| `GET` | `/api/summarize/{content_id}` | Generate an AI summary |
| `GET` | `/api/content` | List all processed content |

---

## 🧠 Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Backend** | FastAPI | Async REST API with background jobs |
| **Transcription** | Groq Whisper Large v3 | State-of-the-art speech-to-text |
| **LLM** | Groq Llama 3.3 70B | Answer generation from retrieved context |
| **Embeddings** | all-MiniLM-L6-v2 | 384-dim sentence embeddings (runs locally) |
| **Vector Search** | FAISS | Facebook's similarity search library |
| **PDF Parsing** | PyMuPDF (fitz) | Page-level text extraction from PDFs |
| **Video Download** | yt-dlp | YouTube audio extraction |
| **Database** | SQLite + SQLAlchemy | Metadata storage for videos & chunks |
| **Frontend** | HTML/CSS/JS | Single-page app with dark-mode UI |

---

## 🛠️ How It Works (Under the Hood)

### 1. Ingestion
- **Videos**: `yt-dlp` downloads audio as MP3, sanitizing filenames for special characters (emojis, pipes)
- **Documents**: PyMuPDF extracts text page-by-page from PDFs; text files are split by paragraphs

### 2. Transcription (Videos Only)
- Audio is sent to **Groq Whisper Large v3** API
- Returns timestamped segments (`start`, `end`, `text`)

### 3. Chunking
- Video transcripts are grouped into **30-second windows** with overlap
- Documents are chunked at ~500 words with 50-word overlap for context continuity

### 4. Embedding & Indexing
- Each chunk is embedded using **all-MiniLM-L6-v2** (384 dimensions, runs on CPU)
- Vectors are stored in a **FAISS index** for sub-millisecond search
- Metadata (source title, timestamps, page numbers) is pickled alongside

### 5. Retrieval & Generation
- User queries are embedded with the same model
- FAISS returns the **top 8** most semantically similar chunks
- Chunks + metadata are assembled into a context prompt
- **Llama 3.3 70B** generates an answer constrained to the provided context

---

## ⚙️ Configuration

All configuration is via environment variables (`.env` file):

| Variable | Default | Description |
|----------|---------|-------------|
| `GROQ_API_KEY` | — | **Required.** Your Groq API key |
| `DATABASE_URL` | `sqlite:///./echoquery.db` | Database connection string |
| `FAISS_INDEX_PATH` | `data/faiss/index.bin` | Path to FAISS index file |
| `FAISS_META_PATH` | `data/faiss/meta.pkl` | Path to metadata pickle |
| `EMBEDDING_MODEL` | `all-MiniLM-L6-v2` | Sentence-transformer model name |

---

## 🤝 Contributing

Contributions are welcome! Here are some ideas:

- [ ] **Scoped retrieval** — Filter search results by specific content ID
- [ ] **Multi-language support** — Whisper supports 100+ languages
- [ ] **Batch upload** — Process multiple files at once
- [ ] **Export answers** — Download Q&A history as Markdown
- [ ] **User authentication** — Multi-user support with separate libraries
- [ ] **Cloud deployment** — Docker + Railway/Render deployment guide

```bash
# Fork the repo, create a branch, make changes, then:
git checkout -b feature/your-feature
git commit -m "Add your feature"
git push origin feature/your-feature
# Open a Pull Request
```

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

<p align="center">
  Built with ❤️ using <strong>FastAPI</strong> · <strong>Groq</strong> · <strong>FAISS</strong> · <strong>Llama 3.3</strong>
</p>
