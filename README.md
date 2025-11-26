# Web Scraping Solution

Full-stack scraper and search UI: Django + DRF backend, PostgreSQL storage, Next.js + React frontend, all runnable locally or via Docker Compose.

## Stack
- Backend: Django 5, Django REST Framework, django-filter, requests, BeautifulSoup, psycopg2.
- Frontend: Next.js 14, React 18, Bootstrap / React-Bootstrap.
- Infra: PostgreSQL, Docker & docker-compose.

## Project Structure
```
backend/      Django project (apps: scraper, search, user_app, common)
frontend/     Next.js app (UI for scraping control, auth, search)
docker-compose.yml  Orchestrates postgres, backend, frontend
```

## Prerequisites
- Docker + docker-compose (for the simplest start)
- or: Python 3.12+, Node.js 18+, PostgreSQL 14+ reachable locally

## Quick Start (Docker)
```bash
docker-compose up --build
```
Services:
- Frontend: http://localhost:3000
- Backend API: http://localhost:8000
- Postgres: localhost:5432 (db `meta_scrapping_solution`, user `meta_scrapping`, password `meta_scrapping`)

Stop with Ctrl+C; remove containers/volumes if needed: `docker-compose down -v`.

## Local Development (without Docker)
### Backend
```bash
cd backend
python -m venv .venv && source .venv/bin/activate  # Windows: .venv\\Scripts\\activate
pip install -r requirements.txt
export DJANGO_DEBUG=True DB_HOST=127.0.0.1 DB_NAME=meta_scrapping_solution DB_USER=meta_scrapping DB_PASSWORD=meta_scrapping DB_PORT=5432
python manage.py makemigrations
python manage.py migrate
python manage.py runserver 0.0.0.0:8000
```

### Frontend
```bash
cd frontend
npm install
npm run dev      # serves on http://localhost:8080
# production build:
npm run build
npm run start    # serves on http://localhost:3000
```

## Environment Variables
| Variable | Default (docker-compose) | Purpose |
| --- | --- | --- |
| DJANGO_DEBUG | True | Toggle Django debug mode |
| DJANGO_ALLOWED_HOSTS | backend | Allowed host entry appended to defaults |
| DJANGO_CSRF_TRUSTED_ORIGINS | http://localhost:3000 | CSRF trusted origins |
| DB_NAME | meta_scrapping_solution | Postgres database name |
| DB_USER | meta_scrapping | Postgres user |
| DB_PASSWORD | meta_scrapping | Postgres password |
| DB_HOST | meta_database (Docker) / 127.0.0.1 (local) | Postgres host |
| DB_PORT | 5432 | Postgres port |
| BACKEND_URL | http://backend:8000 | Frontend runtime API base (Docker) |

## Useful Commands
- `docker-compose up --build` / `docker-compose down -v`
- Backend: `python manage.py makemigrations`, `python manage.py migrate`, `python manage.py runserver 0.0.0.0:8000`
- Frontend: `npm run dev`, `npm run build`, `npm run start`, `npm run lint`

## API & Data Notes
- Backend apps expose REST endpoints for scraping control, search, and user auth (see sample requests in `backend/scrap_controll.http`, `backend/search.http`, `backend/user_app.http`).
- Core models live in `common` app (products, categories, stores). PostgreSQL is required; credentials configurable via env vars.
- Backend logs to `backend/debug.log` by default (see logging config in `meta_scrape_hub/settings.py`).

## Troubleshooting
- Migrations failing: confirm Postgres is running and env vars match credentials.
- CORS/CSRF issues in dev: ensure `DJANGO_CSRF_TRUSTED_ORIGINS` includes your frontend origin (e.g., http://localhost:8080).
- Port conflicts: adjust `docker-compose.yml` or package.json scripts (`dev` uses port 8080, `start` uses 3000).
