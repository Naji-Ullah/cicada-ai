# Cicada AI

AI chat application (Cicada AI) with a Django REST backend and a React + TypeScript frontend that integrates Google Gemini (Generative AI).

This repository contains a full-stack example and includes convenience scripts and a dockerized setup.

Contents
- backend/ — Django project
- frontend/ — React + TypeScript frontend (Vite)
- docker-compose.yml — local multi-container setup (backend + frontend)
- README.md — this file

Key features
- User authentication (register / login)
- Persistent chat history
- Integration with Google Gemini (requires API key)
- Simple REST API built with Django REST Framework
- React frontend with Tailwind CSS and Vite

Tech stack
- Backend: Django 5.2.x, Django REST Framework
- Frontend: React 18, TypeScript, Vite, Tailwind CSS
- Database: SQLite (default, changeable to PostgreSQL/MySQL for production)

Prerequisites
- Docker & Docker Compose (recommended for quick start)
- OR Python 3.8+, Node.js 16+, npm/yarn for local development

Quickstart — Docker (recommended)
1. Copy the repository and create an `.env` file in the repository root (see "Environment variables" below).
2. Build and start the services:

   docker-compose up --build

3. The services started by docker-compose in this repository are:
   - backend: mapped to port 8000 (http://localhost:8000)
   - frontend: mapped to port 3000 (http://localhost:3000) — note: the dev server (npm run dev) typically runs on 5173; the compose setup maps the container's configured frontend server to 3000.

4. Open the frontend in your browser and register/login to start chatting.

Running locally without Docker

Backend (Django)

1. Create and activate a virtual environment:

   python3 -m venv venv
   source venv/bin/activate

2. Install Python dependencies:

   pip install -r backend/requirements.txt

3. Create a `.env` file in the repository root (see example below).
4. Run migrations and create a superuser (optional):

   cd backend
   python manage.py migrate
   python manage.py createsuperuser

5. Start the Django development server:

   python manage.py runserver

The backend will be served at http://localhost:8000 by default.

Frontend (React + Vite)

1. Install Node dependencies:

   cd frontend
   npm install

2. Make sure `VITE_API_BASE_URL` in your `.env` points to the backend API (for local dev: http://localhost:8000/api).
3. Start the frontend dev server:

   npm run dev

The frontend dev server typically runs on http://localhost:5173 (check console output).

Environment variables (.env example)
Create a top-level `.env` file before running the app (Docker Compose uses this file):

SECRET_KEY=your-secret-key-here
DEBUG=True
GEMINI_API_KEY=your-google-gemini-api-key
VITE_API_BASE_URL=http://localhost:8000/api

Notes about environment variables and security
- Keep `SECRET_KEY` and `GEMINI_API_KEY` secret. Do not commit `.env` to version control.
- `DEBUG` should be set to `False` in production.

Docker notes
- docker-compose.yml builds the backend from `./backend/Dockerfile` and the frontend from `./frontend/Dockerfile` (if present) and injects environment variables from the root `.env` file.
- The Django settings allow the container host `backend` which is used by Docker networking.
- The frontend in the repository includes Vite-based development configuration; running inside a container may use a different server or ports depending on the Dockerfile.

Useful scripts
- frontend/setup.sh — installs frontend deps and creates an example `.env` stub
- frontend/start.sh — convenience script to start both backend and frontend locally (expects a local virtualenv and .env)

Project structure (high level)

project/
├─ backend/                 # Django project
│  ├─ backend/              # Django settings
│  ├─ api/                  # Django app with models, views, serializers
│  └─ manage.py
├─ frontend/                # React + TypeScript app (Vite)
│  ├─ src/
│  └─ package.json
├─ docker-compose.yml

Development hints
- The frontend uses `VITE_API_BASE_URL` to point to the Django API. When running the frontend locally with `npm run dev`, set this to `http://localhost:8000/api`.
- If you use Docker Compose the frontend container depends on the backend and the `.env` file is passed into the containers.
- If you plan to use a production database, update the Django `DATABASES` setting in `backend/backend/settings.py` and configure secrets through environment variables.

Contributing
- Fork the repository, create a feature branch, add tests where appropriate, and open a pull request.

License
- MIT

Contact
- Repo owner: @Naji-Ullah

--

This README was generated based on repository contents (frontend README and docker-compose.yml) and a quick scan of backend Django settings and frontend scripts. Please review and edit any environment values, ports, or instructions to match your intended deployment or local development workflow.