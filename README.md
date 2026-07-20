# Codium: Integrated DSA and SQL Learning Ecosystem

Codium is an interactive, full-stack learning platform designed to bridge the gap between static algorithmic study and live technical interview preparation. It combines a professional browser-based code editor with a sub-second Generative AI engine to act as a Socratic tutor — analyzing your specific code and providing contextual logical nudges rather than giving away direct solutions.

---

## Core Features

**Interactive Coding Workspace**
Write and execute code directly inside a rich browser-based IDE powered by CodeMirror 6. The editor supports syntax highlighting, auto-completion, and a split-screen layout showing the problem statement alongside your code.

**Contextual AI Mentor**
When stuck on a problem, the hint interface bundles your current code and the problem description and sends it to Groq (Llama 3.1), which returns 2 to 3 precise logic hints tailored to your specific mistakes — without revealing the solution.

**Adaptive Quiz System**
Take structured conceptual quizzes across core Data Structures topics including Arrays, Strings, Linked Lists, Stacks, Queues, Trees, and Graphs, as well as database design fundamentals.

**Custom AI Quiz Generator**
Upload your own study material as a PDF to dynamically generate custom multiple-choice technical quizzes using AI-powered content parsing.

**Performance Dashboard**
Track your submission history, verdicts (AC, WA, TLE, RE, CE), points earned, and overall problem-solving progress — all persisted to your user profile.

---

## System Architecture

Codium uses a decoupled client-server architecture where the frontend and backend communicate over a stateless REST API.

### Frontend

- **React.js** — Single Page Application that delivers a desktop-like experience using client-side routing and Virtual DOM reconciliation, avoiding full page refreshes.
- **CodeMirror 6** — Extensible code editor with language support for Python, Java, and C.
- **Fetch API** — Handles all asynchronous data fetching and UI state transitions in the background.

### Backend

- **Django** — Handles all API endpoints, database migrations, user authentication, and the sandboxed code execution engine.
- **Django REST Framework** — Provides structured API views and serialization.
- **Django CORS Headers** — Configured to allow cross-origin requests between the frontend (localhost:3000) and backend (localhost:8000) while protecting session state.
- **Session-based Authentication** — User login state is maintained server-side using Django sessions with secure cookie handling.

### AI and Infrastructure

- **Groq Cloud (Llama 3.1)** — Uses ultra-low latency Language Processing Units (LPUs) to deliver AI-generated hints and quiz questions in under 500ms.
- **SQLite** — Default relational database storing user profiles, problems, test cases, submissions, quiz questions, and topic content.
- **PyMuPDF** — Handles server-side PDF parsing for the custom quiz upload feature.

---

## Tech Stack Summary

| Layer      | Technology                              |
|------------|-----------------------------------------|
| Frontend   | React.js, CodeMirror 6                  |
| Backend    | Django, Django REST Framework           |
| Database   | SQLite                                  |
| AI         | Groq API (Llama 3.1)                    |
| Auth       | Django Session Authentication           |
| PDF Parser | PyMuPDF                                 |

---

## Installation and Local Setup

### Prerequisites

- Node.js v18 or higher
- Python v3.10 or higher
- A Groq API key (free at https://console.groq.com) — required only for AI hint and quiz features

### Backend Setup

```bash
cd codium_backend
pip install -r requirements.txt
```

Create a `.env` file inside `codium_backend/`:

```
GROQ_API_KEY=your_groq_api_key_here
```

Run database migrations and seed problems:

```bash
python manage.py migrate
python manage.py createsuperuser
python manage.py seed_problems
python manage.py runserver
```

The backend will be available at `http://localhost:8000`.

### Frontend Setup

Open a second terminal:

```bash
cd codium_frontend
npm install
npm start
```

The frontend will open at `http://localhost:3000`.

---

## API Endpoints

| Method | Endpoint                  | Description                        |
|--------|---------------------------|------------------------------------|
| POST   | /api/signup/              | Register a new user                |
| POST   | /api/login/               | Log in an existing user            |
| GET    | /api/problems/            | Fetch all DSA problems             |
| POST   | /api/code/submit/         | Submit code for evaluation         |
| GET    | /api/user/stats/          | Get logged-in user profile stats   |
| GET    | /api/quiz/questions/      | Fetch quiz questions by topic      |
| POST   | /api/generate-quiz/       | Generate quiz from uploaded PDF    |
| GET    | /api/learn/dsa/           | Fetch DSA topic learning content   |

---

## Project Structure

```
codium_backend/
    backend/          Django project settings and URL configuration
    core/             Main app — models, views, API logic, code executor
    manage.py

codium_frontend/
    src/
        components/   Navbar and Sidebar
        pages/        All page-level React components
        styles/       CSS stylesheets
    public/
    package.json
```

---

## Notes

- The `.env` file is excluded from version control via `.gitignore`. Never commit your API key.
- The code execution engine runs submitted code in a sandboxed subprocess with a 2-second timeout.
- The admin panel is available at `http://localhost:8000/admin` using your superuser credentials.
