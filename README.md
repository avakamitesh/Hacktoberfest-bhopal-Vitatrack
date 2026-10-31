# VitaTrack

A wellness tracker with an AI assistant: track steps, heart rate, sleep, nutrition, hydration and workouts, keep a personal health profile, and chat with Gemini about your own data.

| Layer | Tech |
| --- | --- |
| Frontend | React + Vite + TypeScript, Tailwind CSS v4, Lucide icons, Recharts, Inter font |
| Backend | Python, FastAPI, Pydantic v2 |
| Database | SQLite via SQLAlchemy 2.0 |
| AI | Official Google Gen AI SDK (`google-genai`) with the Gemini API |
| API | REST (JSON), JWT bearer auth |

## Run it

### 1. Backend

```bash
cd backend
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env        # then edit: add GEMINI_API_KEY and a random JWT_SECRET
uvicorn app.main:app --reload --port 8000
```

Get a Gemini key at <https://aistudio.google.com/apikey>. The app works without one — only the AI Assistant and the AI weekly summary need it (they show a clear message until it is set).

API docs: <http://localhost:8000/docs>

### 2. Frontend

```bash
cd frontend
cp .env.example .env        # VITE_API_URL=http://localhost:8000
npm install
npm run dev                 # http://localhost:5173
```

### Demo account

With `SEED_DEMO=true` (default) the first start creates a demo user with 30 days of sample data:

- **Username:** `alex` (or `demo@vitatrack.app`)
- **Password:** `demo1234`

Set `SEED_DEMO=false` and delete `backend/vitatrack.db` for a clean database. New accounts start empty.

## Pages

- **Login / Sign up** — email-or-username login
- **Dashboard** — steps ring, heart rate, active calories, sleep score, 8-day activity trends (toggle series), nutrition, hydration (+/−), last workout
- **Activity** — 14-day steps chart with goal line, manual logging
- **Sleep** — duration/score history and logging
- **Nutrition** — daily food log, calories and macros
- **Workouts** — history, weekly totals, logging
- **Reports** — 7/30-day averages, trend vs previous period, AI weekly summary
- **AI Assistant** — Gemini chat that is given a summary of your recent data and health profile
- **Settings** — profile, daily goals, and a health profile (conditions, allergies, medications, notes)

## Project layout

```
backend/app/
  main.py  config.py  database.py  models.py  schemas.py  security.py  deps.py  seed.py
  routers/   auth.py  health.py  chat.py
  services/  gemini.py  summary.py
frontend/src/
  pages/  components/  context/AuthContext.tsx  lib/api.ts
```

## Notes

- Data is entered manually (or seeded); there is no wearable/device sync. Heart-rate values are daily figures, not live readings.
- Password reset is not implemented — the "Forgot Password?" link says so.
- The assistant gives general wellness guidance only and is instructed not to diagnose or advise on prescriptions.
- Set a strong `JWT_SECRET` before deploying, and restrict `CORS_ORIGINS` to your real frontend origin.
