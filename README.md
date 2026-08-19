# Customer Feedback Analyzer (Gen AI)

The Customer Feedback Analyzer is a full-stack web application designed to automatically process and summarize large volumes of customer reviews for restaurant owners. The application combines a **FastAPI backend** powered by **Google Gemini** with an interactive **Streamlit frontend dashboard**, leveraging an **SQLite database** to store results.

**What you will build:**

- A **backend** (FastAPI) that uses Gemini to analyze one review.
- A **frontend** (Streamlit dashboard) that the owner actually clicks on. It sends every review to your backend and shows a summary.

---

## Technical Architecture & Component Breakdown

| Component      | Technology             | Primary Function                                                          |
| -------------- | ---------------------- | ------------------------------------------------------------------------- |
| Backend API    | FastAPI + uv           | Hosts the service that analyzes individual reviews.                       |
| AI Integration | Gemini + Pydantic      | Generates structured analysis (labels, scores, and themes).               |
| Frontend UI    | Streamlit              | Provides the dashboard interface for pasting reviews and viewing metrics. |
| Data Storage   | SQLite (`feedback.db`) | Saves every processed review and its metadata for long-term tracking.     |
| Core Logic     | Python Fundamentals    | Handles loops, list parsing, and robust error management.                 |
| Integration    | HTTP Requests          | Facilitates frontend-to-backend API communication.                        |

---

## Step 1: Set up the project

```bash
uv init customer_feedback_analyzer
cd customer_feedback_analyzer
uv add "fastapi[standard]" google-genai python-dotenv pydantic streamlit requests
```

Put your Gemini key in a `.env` file:

```
GOOGLE_API_KEY=your_key_here
```

---

## Step 2: The backend (`api.py`)

The full code is in `api.py`.

Run the backend in its **own terminal** and leave it running:

```bash
uv run fastapi dev api.py
```

Quick test: open **http://127.0.0.1:8000/docs**, try `/analyze` with a review, and confirm you get back a label, score, and theme.

---

## Step 3: The frontend (`app.py`)

Open a **second terminal** (keep the backend running in the first one) and run:

```bash
uv run streamlit run app.py
```

Paste a few reviews (one per line) and click **Analyze**. You can copy test reviews from `sample_reviews.txt`.

The key idea in the frontend is this loop. For each review, we call our own backend and collect the answer:

Small summary: how many reviews, the average score, the percent that are positive, and the single theme people mention most. Finally, a **Save to database** button writes every review into a SQLite table (`feedback.db`), and a **Saved history** section reads them all back.

---

## Step 4: Keep code tidy (`database.py`)

When a file gets long, we split it. All the database code (create table, save, load) lives in its own file, **`database.py`**. Then `app.py` simply imports it at the top:

This is the same idea as `import requests` or `import streamlit` — except now we are importing **our own** file. The app file stays short and is about the screen; the database file is about the data. Each file has one clear job.

---

## How to run the whole thing (two terminals)

| Terminal 1 (backend)        | Terminal 2 (frontend)         |
| --------------------------- | ----------------------------- |
| `uv run fastapi dev api.py` | `uv run streamlit run app.py` |
| stays running               | opens in your browser         |

If the frontend shows "error" rows, the most common reason is that the backend is not running. Start it first.

---

# Screenshots

[Screenshot1.png]
[Screenshot1.png]
