<p align="center">
  <h1 align="center">⚡ PulseTrack</h1>
  <p align="center">
    <strong>AI-Powered Workout Planner & Sports Activity Tracker</strong>
  </p>
  <p align="center">
    <a href="#features">Features</a> •
    <a href="#tech-stack">Tech Stack</a> •
    <a href="#architecture">Architecture</a> •
    <a href="#getting-started">Getting Started</a> •
    <a href="#contributing">Contributing</a>
  </p>
</p>

---

## Overview

**PulseTrack** is a full-stack web application that helps users plan workout routines, track sports activities in real time, and receive AI-driven recommendations. Built with a modern decoupled architecture — a vanilla frontend communicating with a Python-based REST API — and backed by Supabase for data persistence and authentication.

> **Status:** 🟡 In Development — Initial setup & architecture phase.

---

## Features

| Module | Description |
|--------|-------------|
| **Landing Page** | Presentation page showcasing the platform and its core value proposition. |
| **Authentication** | Secure login and registration system powered by Supabase Auth. |
| **User Profile & Data** | Onboarding forms to capture user metrics (weight, height, age, goals, fitness level). |
| **Sports Tracker** | Real-time activity tracker supporting outdoor (GPS) and indoor (timer-based) sessions. |
| **AI Coach (Chat)** | Conversational assistant powered by OpenAI GPT Mini for personalized workout and nutrition recommendations. |

---

## Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Frontend | HTML5 · CSS3 · JavaScript | UI structure, styling, and client-side logic |
| Backend | Python · [FastAPI](https://fastapi.tiangolo.com/) | RESTful API, business logic, AI integration |
| Database | [Supabase](https://supabase.com/) (PostgreSQL) | User data, sessions, metrics storage & auth |
| AI | OpenAI API (GPT Mini) | Intelligent workout recommendations chatbot |

---

## Architecture

### Sports Tracker — Implementation Approach

The tracker module captures activity data on the client side and sends it to the backend for processing and storage.

**Outdoor Mode** (Running, Cycling)
- Uses the HTML5 Geolocation API (`navigator.geolocation.watchPosition()`) for real-time GPS coordinates.
- Distance is calculated between consecutive points using the Haversine formula.
- Displays live metrics: distance, average speed, elapsed time.

**Indoor Mode** (Gym, Strength Training)
- JavaScript-driven stopwatch/timer.
- Dynamic forms for logging exercises, sets, reps, and weight.
- Support for pre-built routine templates.

**Data Flow:**
```
User starts session → JS captures data (GPS / form input)
    → Data buffered in localStorage (crash safety)
    → User ends session → JSON payload sent via fetch() POST
    → FastAPI validates & enriches data (e.g., calorie estimation)
    → Stored in Supabase → Summary displayed to user
```

### API Endpoints (Planned)

```
POST   /api/tracker/sessions         Create a new workout session
GET    /api/tracker/sessions         Retrieve session history for the authenticated user
GET    /api/tracker/sessions/{id}    Retrieve a specific session detail
GET    /api/tracker/stats            Aggregated stats (weekly, monthly)
POST   /api/chat/message             Send a message to the AI coach
POST   /api/auth/login               User login
POST   /api/auth/register            User registration
GET    /api/users/profile            Retrieve user profile data
PUT    /api/users/profile            Update user profile data
```

### Database Schema (Planned)

**`sessions` table:**

| Column | Type | Description |
|--------|------|-------------|
| `id` | UUID | Primary key |
| `user_id` | UUID | FK → Supabase Auth users |
| `activity_type` | VARCHAR | `running`, `cycling`, `weightlifting`, etc. |
| `started_at` | TIMESTAMPTZ | Session start time |
| `ended_at` | TIMESTAMPTZ | Session end time |
| `duration_minutes` | INTEGER | Total duration |
| `calories_estimated` | FLOAT | Estimated calories burned |
| `metrics` | JSONB | Flexible payload (GPS route, sets/reps, etc.) |

---

## Project Structure

```
pulsetrack/
│
├── frontend/                     # Client-side application
│   ├── index.html                # Landing page
│   ├── login.html                # Authentication page
│   ├── dashboard.html            # User dashboard
│   ├── tracker.html              # Sports tracker interface
│   ├── chat.html                 # AI coach chat interface
│   ├── css/
│   │   └── style.css             # Global styles
│   ├── js/
│   │   ├── app.js                # Core application logic
│   │   ├── auth.js               # Supabase auth integration
│   │   ├── tracker.js            # Tracker logic (GPS + timer)
│   │   ├── chat.js               # AI chatbot connection
│   │   └── api.js                # API service layer (fetch wrapper)
│   ├── .env                      # Frontend environment variables
│   └── .gitignore
│
├── backend/                      # Server-side application
│   ├── main.py                   # FastAPI entry point
│   ├── requirements.txt          # Python dependencies
│   ├── .env                      # Backend environment variables (secrets)
│   ├── .gitignore
│   └── routers/                  # Modular API route handlers
│       ├── auth.py               # Authentication routes
│       ├── tracker.py            # Tracker session routes
│       ├── chat.py               # AI chat routes
│       └── users.py              # User profile routes
│
└── README.md
```

---

## Getting Started

### Prerequisites

- Python 3.10+
- pip
- A [Supabase](https://supabase.com/) project (free tier works)
- An [OpenAI API key](https://platform.openai.com/)

### Backend Setup

```bash
cd backend
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate
pip install -r requirements.txt
# Configure .env with your Supabase URL, key, and OpenAI API key
uvicorn main:app --reload
```

### Frontend Setup

Open `frontend/index.html` in your browser, or serve it with any static file server:

```bash
cd frontend
python -m http.server 5500
```

---

## Contributing

This is a collaborative project. To get started as a contributor:

1. Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/pulsetrack.git
   cd pulsetrack
   ```
2. Create a feature branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. Commit your changes using [Conventional Commits](https://www.conventionalcommits.org/):
   ```bash
   git commit -m "feat: add tracker GPS module"
   ```
4. Push and open a Pull Request:
   ```bash
   git push origin feature/your-feature-name
   ```

### Branch Strategy

| Branch | Purpose |
|--------|---------|
| `main` | Stable, production-ready code |
| `dev` | Active development and integration |
| `feature/*` | Individual feature branches |

---

## License

This project is for educational and personal use.

---

<p align="center">
  Built with 💪 by the PulseTrack team
</p>
