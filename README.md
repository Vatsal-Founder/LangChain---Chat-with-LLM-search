# LangChain Chat with LLM Search — Agent + Memory

A conversational search app that uses **Wikipedia tools/agents** to find information and keeps **chat memory** for coherent back‑and‑forth dialogue.

* 🔍 Tool‑augmented search with **Wikipedia** (and optional web tools)
* 🧠 **Conversation memory** per session
* ⚡ LLM backends: **OpenAI** or **Groq**
* 🧩 Built on **LangChain** with optional **LangSmith** tracing

> Repo: `Vatsal-Founder/LangChain---Chat-with-LLM-search`
* Link: https://huggingface.co/spaces/vatsal18/LLM_Search
---

## Features

* **Ask anything** → the agent searches Wikipedia, reads summaries/pages, and composes answers.
* **Follow‑ups work** → short‑term memory preserves context within a session.
* **Pluggable LLM** → switch between OpenAI and Groq with env vars.
* **Lightweight UI** → run as Streamlit or serve an API (if your `app.py` exposes FastAPI).

---

## Architecture

```
[User]
  ↓
[Chat UI / API]
  ↓
[LangChain Agent]
  ├─ Tools:
  │    • WikipediaSearchTool (search + page read)
  │    • (optional) Web search/fallbacks
  ├─ Memory (Buffer/Summary for session)
  └─ LLM (OpenAI or Groq)
  ↓
[Answer + cited snippets]
```

---

## Project Structure (typical)

```
.
├── app.py               # entrypoint (Streamlit or FastAPI)
├── requirements.txt
├── README.md            # this file
└── .env.example         # sample environment (optional)
```

---

## Requirements

* Python **3.10+**
* One LLM key: **OpenAI** or **Groq**

---

## Setup

```bash
git clone https://github.com/Vatsal-Founder/LangChain---Chat-with-LLM-search.git
cd LangChain---Chat-with-LLM-search
python -m venv .venv && source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env  # create if missing
```

### Environment variables (minimum)

```ini
# Choose your LLM backend: openai | groq
LLM_BACKEND=openai

# OpenAI
OPENAI_API_KEY=your_openai_key
MODEL_NAME=gpt-4o-mini          # example, adjust to your code

# Groq
# GROQ_API_KEY=your_groq_key
# MODEL_NAME=llama-3.1-8b



# LangSmith (optional)
LANGCHAIN_TRACING_V2=true
LANGCHAIN_API_KEY=your_langsmith_key
LANGCHAIN_PROJECT=llm-search
```

---

## Run locally

Pick the entrypoint matching your `app.py`.

**Streamlit UI**

```bash
streamlit run app.py
```

Open [http://localhost:8501](http://localhost:8501) and chat.


---

## Using the App

* Start broad: *"Give a concise overview of diffusion models. Cite Wikipedia sections you used."*
* Follow up: *"Now compare diffusion vs. GANs; list key differences in a table."*
* Ask for sources: *"Which pages were consulted? Give links and section headings."*

> The agent will call Wikipedia tools as needed and weave results into the reply, using chat memory to keep context.

---

## Deployment

### Docker

**Dockerfile (example)**

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
# Streamlit default
CMD ["streamlit", "run", "app.py", "--server.port", "8501", "--server.address", "0.0.0.0"]
# For FastAPI instead:
# CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
```

**Build & run**

```bash
docker build -t llm-search .
docker run -p 8501:8501 --env-file .env llm-search
```


---


