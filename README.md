# Your Personal AI Teaching Assistant for Lecture Slides V2

> **Intelligent Lecture Q&A System with Advanced RAG, Multi-turn Conversation, and AI-Powered Study Tools**

---

## 🎯 Overview

Your Personal AI Teaching Assistant is a cutting-edge **Retrieval-Augmented Generation (RAG)** system designed to revolutionize how students interact with lecture materials. This Streamlit-powered web application combines semantic search, intelligent reranking, and large language models to deliver instant, accurate answers grounded in uploaded lecture PDFs.

Whether you're reviewing course materials, preparing for exams, or clarifying complex concepts, this system acts as your **24/7 personal tutor**—delivering contextual explanations, generating study notes, and creating adaptive quizzes—all without leaving your uploaded lectures.

---

## ⚡ Key Features

### 💬 **Intelligent Q&A System**
- Ask questions about lecture content in natural language
- Get contextual answers backed by precise source citations
- **Multi-turn conversation support** with semantic understanding of conversation history
- Retrieved chunks ranked by both dense embeddings and cross-encoder relevance scores

### 📝 **Smart Study Notes Generator**
- Automatically extract important concepts, definitions, and formulas from lectures
- Scope notes to specific lectures or generate from all uploads
- Export notes as Markdown for offline study
- Seamless manual note-taking integration

### 🎯 **Adaptive Quiz Engine**
- Generate multiple-choice quizzes with configurable difficulty (Easy/Medium/Hard)
- Customizable question count (3–12 per quiz)
- Instant grading with detailed explanations
- Quiz history tracking for progress monitoring
- Color-coded feedback (✅ correct, ❌ incorrect)

### 🔍 **Advanced Retrieval Pipeline**
- **Embedding Model**: BAAI/bge-large-en-v1.5 (1024-dim, optimized for semantic search)
- **Vector Database**: FAISS IndexFlatIP (exact cosine similarity, memory-efficient)
- **Reranker**: Cross-encoder ms-marco-MiniLM-L-6-v2 (precise relevance ranking)
- **Two-stage retrieval**: dense search + intelligent reranking

### 📚 **Seamless PDF Processing**
- Upload multiple lecture PDFs at once
- Automatic sentence-aware chunking with overlap (400 tokens/chunk, 50-token overlap)
- Metadata tracking (lecture name, page number, chunk index)
- One-time FAISS index construction (cached for performance)

### 🚀 **Production-Ready Architecture**
- Streamlit caching for models and indexes
- Error handling and graceful fallbacks
- Real-time progress indicators
- Professional, accessible UI with dark mode

---

## 🛠️ Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Frontend** | Streamlit | Interactive web interface |
| **Embedding** | BAAI/bge-large-en-v1.5 | Semantic text representation (1024-dim) |
| **Vector DB** | FAISS (IndexFlatIP) | Fast similarity search over embeddings |
| **Reranker** | CrossEncoder (ms-marco) | Precision relevance ranking |
| **LLM** | Llama-3.1-8b-instant (Groq) | Ultra-fast, cost-effective generation |
| **PDF Processing** | PyMuPDF (fitz) | High-fidelity text extraction |
| **Infrastructure** | Python 3.8+ | Lightweight, portable |

---

## 🚀 Quick Start

### Prerequisites
- Python 3.8 or higher
- A free Groq API key (get one at [console.groq.com](https://console.groq.com))
- At least one lecture PDF file

### Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/AhmedYaSSerUNKN/thinkinh-about-it.git
   cd thinkinh-about-it
   ```

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the application**:
   ```bash
   streamlit run "Machine Learning for Text Mining.py"
   ```

4. **Access the web interface**:
   Open your browser to `http://localhost:8501`

### First Steps
1. Paste your **Groq API key** in the sidebar configuration panel
2. Upload one or more lecture PDFs using the file uploader
3. Click **Initialize System** to build the FAISS index
4. Start asking questions in the **Q&A tab**
5. Generate study notes or quizzes as needed

---

## 📖 How It Works

### RAG (Retrieval-Augmented Generation) Pipeline

```
┌─────────────────────────────────────────────────────────────┐
│ 1. INGESTION: Extract & Chunk PDFs                          │
│    └─ Sentence-aware chunking (400 tokens, 50 overlap)      │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│ 2. EMBEDDING: Encode Chunks                                │
│    └─ BAAI/bge-large-en-v1.5 → 1024-dim vectors            │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│ 3. INDEXING: Store in FAISS                                │
│    └─ IndexFlatIP for exact cosine similarity              │
└────────────────────┬────────────────────────────────────────┘
                     │
              [Query Arrives]
                     │
┌────────────────────▼────────────────────────────────────────┐
│ 4. RETRIEVAL: Dense Search                                 │
│    └─ Fetch top-12 candidates via cosine similarity        │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│ 5. RERANKING: Precision Ranking                            │
│    └─ CrossEncoder scores top-12 → select top-4            │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│ 6. PROMPT CONSTRUCTION: Inject Context & History           │
│    └─ Last 3 turns + top-4 chunks + student question       │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│ 7. GENERATION: Query LLM                                   │
│    └─ Llama-3.1-8b-instant generates grounded answer       │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│ 8. RESPONSE: Return with Sources                           │
│    └─ Answer + metadata (file, page, chunk, scores)        │
└─────────────────────────────────────────────────────────────┘
```

### Key Innovations

- **Two-Stage Retrieval**: Dense embeddings find topical relevance; cross-encoders ensure precision
- **Sentence-Aware Chunking**: Preserves context boundaries unlike naive token chunking
- **Multi-Turn History**: Last 3 Q&A turns injected into prompts for conversational continuity
- **Citation Metadata**: Every answer includes exact source (file, page, chunk)
- **Cached Models**: Models and indexes reused across sessions for speed

---

## 📊 Performance Metrics

| Metric | Value |
|--------|-------|
| **Embedding Dimension** | 1024 |
| **Chunk Size** | 400 tokens (approx.) |
| **Chunk Overlap** | 50 tokens |
| **Dense Retrieval** | Top 12 (FAISS exact search) |
| **Reranking** | Top 4 (CrossEncoder) |
| **LLM Response Time** | < 3s per query (Groq) |
| **Memory Efficient** | IndexFlatIP (no quantization loss) |
| **Max Context** | 800 chars per chunk in prompt |

---

## 🎓 Use Cases

### For Students
- **Instant Clarification**: Ask questions about confusing concepts in real-time
- **Exam Prep**: Generate self-tests and track quiz performance
- **Note Synthesis**: Automatically extract key points from lengthy lectures
- **Review Assistant**: Maintain searchable notes organized by lecture

### For Educators
- **Lecture Analytics**: Understand which topics students find challenging
- **Study Material Creation**: Bulk-generate quizzes for different difficulty levels
- **Supplementary Content**: Provide 24/7 tutoring without scaling instructor time
- **Assessment**: Create randomized quizzes with consistent grading

### For Researchers
- **Knowledge Extraction**: Mine lecture PDFs for structured summaries
- **Semantic Analysis**: Explore relationships between concepts via embeddings
- **Baseline Comparisons**: Evaluate RAG systems on educational content

---

## 🔧 Configuration Guide

### System Parameters

Located in the code's `CONSTANTS` section:

```python
EMBEDDING_MODEL  = "BAAI/bge-large-en-v1.5"    # High-quality semantic embeddings
RERANKER_MODEL   = "cross-encoder/ms-marco-MiniLM-L-6-v2"  # Precision ranking
LLM_MODEL        = "llama-3.1-8b-instant"      # Fast, free-tier Groq LLM

CHUNK_SIZE       = 400      # Tokens per chunk
CHUNK_OVERLAP    = 50       # Token overlap between chunks
RETRIEVAL_TOP_K  = 12       # Dense candidates before reranking
RERANK_TOP_K     = 4        # Chunks sent to LLM
CONTEXT_CHAR_LIM = 800      # Characters per chunk in prompt
HISTORY_WINDOW   = 3        # Q&A turns for multi-turn context
```

**Tuning Tips**:
- **Increase `CHUNK_SIZE`** for longer-form content; decrease for fine-grained retrieval
- **Increase `RERANK_TOP_K`** for more comprehensive answers (higher latency)
- **Adjust `HISTORY_WINDOW`** to balance context vs. prompt length
- **Lower `CONTEXT_CHAR_LIM`** to prioritize brevity; raise for detail

---

## 🔐 Security & Privacy

- **API Key Management**: Groq keys entered via Streamlit's secure password input (never logged)
- **Local Processing**: PDFs processed locally; only queries sent to Groq API
- **No Data Retention**: Chat history stored in Streamlit session state (ephemeral)
- **Open Source**: Full code transparency for security audits

---

## 📋 Dependencies

```
streamlit>=1.28.0
sentence-transformers>=2.2.2
faiss-cpu>=1.7.4         # Use faiss-gpu for GPU acceleration
groq>=0.9.0
pymupdf>=1.23.0          # PDF text extraction
numpy>=1.24.0
```

Install all at once:
```bash
pip install streamlit sentence-transformers faiss-cpu groq pymupdf numpy
```

---

## 🚨 Troubleshooting

| Issue | Solution |
|-------|----------|
| **"No Groq API key provided"** | Paste your key in the sidebar configuration panel |
| **"Could not parse quiz (malformed JSON)"** | Regenerate the quiz; LLM occasionally returns invalid JSON |
| **FAISS index build takes 2+ minutes** | Normal for 100+ chunks; models are cached on subsequent runs |
| **Out of memory with many PDFs** | Reduce `CHUNK_SIZE` or use `faiss-gpu` |
| **LLM responses too generic** | Increase `RERANK_TOP_K` to provide more context |
| **Quiz answer index mismatch** | Clear browser cache; Streamlit session state may be stale |

---

## 📈 Future Enhancements

- [ ] **Streaming LLM Responses**: Real-time answer generation for better UX
- [ ] **Multi-Modal Support**: OCR for scanned PDFs and embedded images
- [ ] **Persistent Storage**: Database backend for chat history and notes
- [ ] **Fine-Tuned Embeddings**: Custom embeddings trained on course corpora
- [ ] **Voice Interface**: Speak questions; hear answers
- [ ] **Collaborative Spaces**: Share quizzes and notes with classmates
- [ ] **Analytics Dashboard**: Insights into learning patterns and weak areas
- [ ] **API Endpoints**: Enable third-party integrations

---

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -am 'Add feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a pull request

---

## 📄 License

This project is licensed under the **MIT License**. See the LICENSE file for details.

---

## 👨‍💻 Author

**Ahmed Yasser Belaih**  
GitHub: [@AhmedYaSSerUNKN](https://github.com/AhmedYaSSerUNKN)  
Repository: [thinkinh-about-it](https://github.com/AhmedYaSSerUNKN/thinkinh-about-it)

---

## 🙏 Acknowledgments

- **BAAI** for bge-large-en-v1.5 embeddings
- **Cross-Encoder** team for ms-marco reranker
- **Groq** for ultra-fast Llama-3.1 inference
- **Meta AI** for Llama-3.1 model architecture
- **Streamlit** for the web framework
- **FAISS** by Facebook AI for efficient similarity search

---

## 📞 Support & Feedback

Have questions or found a bug? Open an issue on GitHub or reach out to the author.

---

**Built with ❤️ to democratize access to intelligent tutoring.**
