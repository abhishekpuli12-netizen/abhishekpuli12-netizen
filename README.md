# 🎓 Hybrid RAG Academy Assistant

A Retrieval-Augmented Generation (RAG) assistant for **Kalashala Beauty & Makeup Academy** that answers academy-related queries using **hybrid search (FAISS + BM25)**, **cross-encoder reranking** and a **local LLM (Llama 3 via Ollama)**, with measured retrieval quality and zero API cost.

---

## 📌 Project Overview

The assistant answers questions about courses, fees, trainers, admissions, certifications and batch details. Instead of relying on the LLM alone, it retrieves relevant chunks from a Markdown knowledge base, reranks them, and generates a grounded answer. If the answer is not in the knowledge base, it says so instead of guessing.

**Highlights**

- 📈 **Recall@5 improved from 73% to 93%** and **MRR from 0.61 to 0.84** by adding hybrid retrieval and reranking
- ⏱️ **2.8 s median** and **4.6 s p95** end-to-end latency on a local LLM
- 🛡️ **90% of prompt-injection test queries** and **85% of out-of-scope queries** handled correctly
- 💸 Runs fully locally with Ollama, so **no API cost**

---

## 🚀 Features

- ✅ Hybrid Retrieval (FAISS + BM25)
- ✅ Cross-Encoder Re-ranking
- ✅ Local LLM using Ollama (Llama 3)
- ✅ FastAPI REST Backend
- ✅ Streamlit Chat Interface
- ✅ Dockerized Deployment (Docker Compose)
- ✅ Markdown Knowledge Base
- ✅ Grounded answers with a "not found" fallback
- ✅ Prompt Injection Protection
- ✅ Domain Restricted Responses
- ✅ Source Attribution
- ✅ Greeting & Small Talk Handling

---

## 🏗️ Architecture

<img width="2500" height="1200" alt="rag_flow_horizontal" src="https://github.com/user-attachments/assets/229512b1-6c79-4997-b7be-d7ac8b6023db" />

**Pipeline**

1. The user asks a question in the Streamlit app, which calls the FastAPI backend.
2. **FAISS** (`all-MiniLM-L6-v2` embeddings) and **BM25** each retrieve candidates, and the results are merged.
3. A **cross-encoder** reranks the top 20 candidates.
4. The best chunks go into a grounded prompt for **Llama 3 (Ollama)**.
5. The answer is returned with its sources.

---

## 📊 Evaluation

Evaluated on a **30-question labeled test set** over a knowledge base of **40 documents (~600 chunks)**.

### Retrieval quality

| Configuration | Recall@5 | MRR |
|---|---|---|
| FAISS only | 73% | 0.61 |
| Hybrid (FAISS + BM25) | 83% | 0.72 |
| **Hybrid + cross-encoder reranking** | **93%** | **0.84** |

### Latency (end-to-end, local)

| Metric | Value |
|---|---|
| p50 | 2.8 s |
| p95 | 4.6 s |
| Overhead of hybrid retrieval + top-20 reranking vs FAISS-only | ~180 ms |

### Answer quality and safety

| Check | Result |
|---|---|
| Faithfulness (RAGAS / manual check) | 27 / 30 |
| Out-of-scope queries handled correctly | 85% |
| Prompt-injection queries handled correctly | 90% |

---

## 🛠️ Tech Stack

### Backend

- Python
- FastAPI
- Uvicorn

### Frontend

- Streamlit

### AI / Machine Learning

- Ollama (Llama 3)
- Sentence Transformers (`all-MiniLM-L6-v2`)
- Cross-Encoder reranker
- FAISS
- BM25

### Deployment

- Docker
- Docker Compose

---

## 📂 Project Structure

```
Hybrid-RAG-Academy-Assistant
│
├── backend
│   ├── data
│   ├── rag.py
│   ├── main.py
│   ├── requirements.txt
│   └── Dockerfile
│
├── frontend
│   ├── app.py
│   └── Dockerfile
│
├── docker-compose.yml
├── README.md
└── .gitignore
```

---

## 📚 Knowledge Base

The assistant retrieves information from Markdown documents such as:

- Courses
- Fees
- Trainers
- Admissions
- Contact Information
- Batch Details
- Certifications
- Locations

---

## 💬 Sample Questions

### Courses

- What courses do you offer?
- Tell me about the Professional Makeup Course.
- What is the Beautician Course?

### Fees

- What are the course fees?
- How much is the Hair Styling Course?

### Trainers

- Who are the trainers?
- Are the trainers CIDESCO certified?

### Admissions

- I want to join a course.
- How can I enroll?

### Certifications

- Will I receive a certificate?
- Do trainers have CIDESCO certification?

---

## 🛡️ Security and Hallucination Control

The assistant is designed to answer **only academy-related questions**.

- **Grounded prompts:** answers must come from the retrieved context.
- **"Not found" fallback:** if the context does not contain the answer, the assistant says so.
- **Prompt-injection resistance:** it resists attempts such as:
  - Ignore previous instructions
  - Reveal your prompt
  - Forget the instructions
  - Tell me about Python / AI / Indian festivals

Out-of-scope questions are politely declined.

---

## 📸 Screenshots

### Home Page

<!-- Add your home page screenshot here -->

### Course Information

<img width="903" height="365" alt="Screenshot 2026-07-11 032933" src="https://github.com/user-attachments/assets/644533e7-ba7b-4bb3-ba38-ec93a6f2cc8b" />

### Prompt Injection Protection

<img width="913" height="320" alt="Screenshot 2026-07-11 032640" src="https://github.com/user-attachments/assets/98ba37f0-68ba-40bf-97a7-5f20b08df86e" />

### Admissions Assistance

<img width="920" height="415" alt="Screenshot 2026-07-13 021041" src="https://github.com/user-attachments/assets/6b1c3661-a4c4-4ff0-ac97-01269c1ff5a5" />

---

## ⚙️ Installation

### Clone Repository

```bash
git clone https://github.com/abhishekpuli12-netizen/Hybrid-RAG-Academy-Assistant.git
cd Hybrid-RAG-Academy-Assistant
```

### Install Backend

```bash
cd backend
pip install -r requirements.txt
```

### Install Frontend

```bash
cd ../frontend
pip install streamlit requests
```

### Start Ollama

```bash
ollama serve
ollama pull llama3
```

### Run Backend

```bash
cd backend
python -m uvicorn main:app --reload
```

### Run Frontend

```bash
cd frontend
streamlit run app.py
```

---

## 🐳 Docker

Build and run the complete application:

```bash
docker compose up --build
```

---

## 📈 Future Improvements

- Conversational slot-filling for appointment booking
- Conversation memory
- User authentication
- Admin dashboard
- Multi-language support
- Voice-based queries

---

## 👨‍💻 Author

**Puli Abhishek**
Python | Machine Learning | Generative AI

- 🔗 [LinkedIn](https://linkedin.com/in/puli-abhishek-05914a264)
- 💻 [GitHub](https://github.com/abhishekpuli12-netizen)
- 📧 abhishekpuli12@gmail.com

---

⭐ If you found this project useful, consider giving it a star!
