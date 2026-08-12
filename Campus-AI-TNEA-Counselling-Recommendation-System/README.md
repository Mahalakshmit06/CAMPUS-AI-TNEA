# Campus AI — TNEA Counselling Recommendation System

A professional full-stack counselling recommendation application built with React + Vite, FastAPI, Pandas and SQLite.

## Core experience

### Campus AI chatbot
The assistant behaves as a guided recommendation system:

1. Greets the user when they say Hi/Hello/Hey.
2. Asks the user's name.
3. Asks the cutoff mark.
4. Asks the community.
5. Asks the district, with an **all districts** option.
6. Asks the branch, with an **all branches** option.
7. Searches the supplied dataset using the complete profile.
8. Displays matching college + branch records in a professional table.
9. Continues answering follow-up questions about cutoff, community, district, branch, specific colleges and counselling preparation.
10. Understands common branch and district abbreviations such as CSE, ECE, EEE, IT, AI & DS, AI & ML, Mech, Civil, Kovai, Nellai, Trichy and similar terms.
11. Stores conversation messages in SQLite using the user's recommendation profile.

### Cutoff calculator
Uses:

`Mathematics + Physics/2 + Chemistry/2`

### College Finder
Filters by:

- Name
- Cutoff
- Community
- District
- Branch

## Project structure

```text
Campus-AI-TNEA-Counselling-Recommendation-System/
├── backend/
│   ├── app/
│   │   ├── __init__.py
│   │   └── main.py
│   ├── data/
│   │   └── Final_TNEA_dataset.csv
│   ├── campus_ai.db
│   └── requirements.txt
├── frontend/
│   ├── src/
│   │   ├── main.jsx
│   │   └── styles.css
│   ├── index.html
│   ├── package.json
│   ├── vite.config.js
│   └── vercel.json
├── render.yaml
└── README.md
```

## Run locally in VS Code

### Terminal 1 — backend

```powershell
cd backend
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```

If PowerShell blocks activation, run:

```powershell
.\.venv\Scripts\activate.bat
```

Test:

- http://127.0.0.1:8000/api/health
- http://127.0.0.1:8000/docs

### Terminal 2 — frontend

```powershell
cd frontend
npm install
npm run dev
```

Open the Vite address shown in the terminal.

## Chatbot test sequence

Start a new conversation and send:

```text
Hi
```

Then answer each question one at a time, for example:

```text
Mahalakshmi
180
BC
Chennai
CSE
```

The assistant should then return the matching records.

Try follow-up questions:

```text
Which colleges can I get with my cutoff?
Which colleges offer CSE?
Suggest colleges in Chennai.
Which colleges are in Coimbatore?
Does this college offer ECE?
What branches can I choose with my cutoff?
What documents are needed for counselling?
How does counselling work?
Which colleges are safer choices for my cutoff?
```

## GitHub

From the project root:

```powershell
git init
git add .
git commit -m "Build Campus AI TNEA recommendation system"
git branch -M main
git remote add origin YOUR_GITHUB_REPOSITORY_URL
git push -u origin main
```

If Git reports that `origin` already exists:

```powershell
git remote set-url origin YOUR_GITHUB_REPOSITORY_URL
git push -u origin main
```

Do not commit `.venv`, `node_modules`, cache files or secrets.

## Vercel frontend deployment

1. Push the project to GitHub.
2. Open Vercel and import the GitHub repository.
3. Set **Root Directory** to `frontend`.
4. Build command: `npm run build`.
5. Output directory: `dist`.
6. Add environment variable:

```text
VITE_API_URL=https://YOUR-RENDER-BACKEND.onrender.com/api
```

7. Deploy.
8. Copy the final Vercel URL.

## Render backend deployment

### Option A — Blueprint

The repository includes `render.yaml`.

1. Create a new Render Blueprint from the GitHub repository.
2. Render detects `render.yaml`.
3. Set the `CORS_ORIGINS` environment variable to the final Vercel frontend URL.
4. Deploy.

### Option B — Web Service

Use these values:

```text
Root Directory: backend
Build Command: pip install -r requirements.txt
Start Command: uvicorn app.main:app --host 0.0.0.0 --port $PORT
```

Set:

```text
CORS_ORIGINS=https://YOUR-VERCEL-DOMAIN.vercel.app
```

After deployment, test:

```text
https://YOUR-RENDER-BACKEND.onrender.com/api/health
```

Then set the Vercel `VITE_API_URL` to:

```text
https://YOUR-RENDER-BACKEND.onrender.com/api
```

Redeploy the frontend after changing the environment variable.

## Database

SQLite is included as `backend/campus_ai.db`. It stores chat history and profile information.

For local development this is a persistent local database file. If the backend hosting service uses an ephemeral filesystem, database files can be reset when the service is recreated. For production-grade persistent chat history, use a managed PostgreSQL database and migrate the same `chat_history` schema.

## Deployment separation

```text
Vercel
  └── React + Vite frontend
          │
          │ HTTPS API requests
          ▼
Render
  └── FastAPI backend
          │
          ▼
      SQLite database
          │
          ▼
  Supplied TNEA dataset
```

## Accuracy and scope

College recommendations are generated from the supplied dataset. The application does not fabricate college, branch or cutoff records. Information outside the supplied dataset, such as fees, hostel facilities and placement statistics, is not invented.

For counselling requirements, use the official TNEA instructions for the applicable counselling cycle.
