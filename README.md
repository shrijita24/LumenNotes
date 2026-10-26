# LumenNotes 📚

**An AI-powered reading journal for non-fiction thinkers.**

LumenNotes helps you capture, organise, and revisit insights from the books you read. It is built end-to-end with Python, SQLite, and Streamlit, with an AI layer powered by Groq's free-tier LLM API.

> 🚀 **In progress:** upgrading the AI layer to a full RAG pipeline (embeddings + FAISS) for semantic search and retrieval across your entire reading history. See the [Roadmap](#-roadmap--rag-upgrade-in-progress).

## 🔗 [**Try it live →**](https://lumennotes.streamlit.app/)

## ✨ Features

- 📝 **Add notes** with tags; pick an existing book and the author auto-fills
- ⭐ **Star** your best insights and **delete** notes you no longer need
- 📖 **Notes grouped by book**, with starred notes shown first
- 📊 **Dashboard** with total notes, books logged, starred count, and top tags
- 🔍 **Search** by book title, tag, or keyword inside a note
- 🤖 **AI Insights**: ask questions about your notes (e.g. "What are the common themes across my books?") using Groq
- 🗄️ **Persistent storage** in SQLite
- ☁️ **Deployed on Streamlit Cloud**, so no local setup is needed to try it

---

## 🛠️ Tech Stack

| Layer      | Technology                                                                 |
| ---------- | -------------------------------------------------------------------------- |
| Frontend   | Streamlit                                                                  |
| Backend    | Python                                                                     |
| Database   | SQLite (single `notes` table: book, author, note, tags, starred)           |
| AI Layer   | Groq API (free tier), `openai/gpt-oss-20b`, called via the OpenAI-compatible SDK |
| Deployment | Streamlit Cloud                                                            |

**How the AI layer works today:** all saved notes are formatted into a single context block and sent to the model together with your question. This works well for small collections. It is the reason for the RAG upgrade below.

---

## 🗂️ Project Structure

```
lumennotes/
├── app.py              # Main Streamlit application (UI, database, AI calls)
├── check_db.py         # Database inspection and schema utilities
├── requirements.txt    # Dependencies
├── .gitignore
└── README.md
```

---

## 🚀 Run Locally

```bash
git clone https://github.com/shrijita24/LumenNotes
cd LumenNotes
pip install -r requirements.txt
```

Get a free Groq API key (no card required) at [console.groq.com](https://console.groq.com), then provide it in **one** of these ways:

**Option A: Streamlit secrets**

```bash
mkdir -p .streamlit
echo 'GROQ_API_KEY = "gsk_your_key_here"' > .streamlit/secrets.toml
```

**Option B: environment variable**

```bash
export GROQ_API_KEY=gsk_your_key_here
```

Then start the app:

```bash
streamlit run app.py
```

The app works without a key. Only the AI Insights page needs it. On first run it creates `journal.db` and seeds a few sample notes.

---

## ⚠️ Known Limitations

- **Shared demo data:** the live demo uses a single SQLite file for all visitors (there are no user accounts), and it may reset when the app restarts. Run it locally for a private journal.
- **Context size:** the AI layer sends all notes with each question, so it will not scale to very large collections.
- **Keyword search only:** search matches exact text, so related ideas phrased differently will not be found.

---

## 🔭 Roadmap: RAG Upgrade (In Progress)

The next version moves from keyword search and full-context prompting to **semantic retrieval**:

| Component    | Technology                          |
| ------------ | ----------------------------------- |
| LLM Backend  | Groq                                |
| Embeddings   | HuggingFace sentence-transformers   |
| Vector Store | FAISS                               |

**What this unlocks:**

- Ask questions across your entire reading history ("What have I read about decision-making?")
- Semantic similarity search that finds related notes even without exact keyword matches
- Responses grounded in the notes retrieved for each question, instead of every note in the database

**Also planned:** per-user storage so each person gets a private journal, and a normalised schema with separate books, notes, and tags tables.

---

## 👩‍💻 Author

**Shrijita Bhattacharyya**
B.Tech CS, IEM Kolkata | LLM Post-Training Intern (Ethara AI) | AI/ML Engineer

[LinkedIn](https://www.linkedin.com/in/shrijita-bhattacharyya/) · [GitHub](https://github.com/shrijita24) · [Portfolio](https://shrijitabhattacharyya-portfolio.netlify.app/)
