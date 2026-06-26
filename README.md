# 🚀 Day 21 — Deployed Full Stack App (Railway + Streamlit Cloud)

Part of my **30-Day AI Engineering Bootcamp** (Day 21/30) — building one AI project a day to become job-ready.

## 🤔 Why I Built This

Days 1-20 only ran on my laptop. A project that only works on `localhost` doesn't exist to anyone else — no recruiter, no user, no demo link. Today I took my JWT-authenticated, rate-limited FastAPI backend (Day 18-20) and put it on the actual internet, with a separate live frontend talking to it across two different cloud platforms.

## 🧠 Thought Process

1. Split deployment correctly: FastAPI backend → Railway, Streamlit frontend → Streamlit Community Cloud
2. Add CORS middleware so the two different domains are allowed to talk to each other
3. Replace hardcoded `127.0.0.1` with an environment-variable-based URL so the same code works locally and in production
4. Configure Railway's start command to bind to `0.0.0.0` and the dynamic `$PORT` Railway assigns
5. Debug real cloud-only errors that never show up locally: missing `python-multipart`, incompatible `bcrypt` wheels, and a `Pillow` build failure caused by an outdated `streamlit` pin on a newer Python version

## ⚙️ What It Does

- 🌐 Backend live on Railway — JWT login, rate-limited `/chat`, `/usage` tracking
- 🖥️ Frontend live on Streamlit Community Cloud — talks to the Railway backend over HTTPS
- 🔐 Same JWT auth and 5-requests/minute rate limiting from Day 20, now working across the public internet
- 📊 Usage stats (tokens, cost, history) still logged to SQLite on the backend

## 🛠️ Tech Stack

- **FastAPI** — backend API (deployed on Railway)
- **Streamlit** — frontend UI (deployed on Streamlit Community Cloud)
- **Groq API (LLaMA 3.3 70B)** — the AI model
- **SQLite** — usage logging
- **JWT (python-jose)** — authentication
- **Railway** — backend hosting
- **Streamlit Community Cloud** — frontend hosting

## 🚀 How to Run

**Locally:**
```bash
git clone <your-repo-url>
cd Day21-Deployment
python -m venv venv
.\venv\Scripts\activate
pip install -r requirements.txt
# Add GROQ_API_KEY and SECRET_KEY to .env
uvicorn backend:app --reload          # Terminal 1
streamlit run frontend.py             # Terminal 2
```

**Live demo:**
- Backend docs: `https://web-production-79c36.up.railway.app/`
- Frontend: `https://prashik-full-stack-app-rt.streamlit.app/`

## 💡 What I Learned

- `127.0.0.1` only ever means "this same machine" — once frontend and backend live on different servers, they need real public URLs to find each other
- CORS exists because browsers block cross-domain requests by default unless the backend explicitly allows it
- Cloud platforms often run newer Python versions than your local machine — tightly pinned dependency versions (`==`) can fail in the cloud even though they worked perfectly locally, because no precompiled wheel exists for that combination
- Deploying isn't "click and done" — most of the real learning happened in reading error logs and fixing one dependency issue at a time

## 🔮 What's Next

**Day 22 — AI with Calculator Tool**, my first step into function/tool calling — teaching the AI to call real functions instead of just generating text.

## 👤 About Me

I'm Prashik, building one AI project a day for 120 days to become a job-ready AI Engineer. Follow my journey on [LinkedIn](https://www.linkedin.com/in/prashik-sawant-ds) 
