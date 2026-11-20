# Smart Campus Complaint Management System

Full-stack college project:
- Backend: Java 17, Spring Boot 3, Spring Security, Spring Data JPA
- Frontend: React + Vite (Node.js/npm)
- Database: PostgreSQL 16
- AI: Local Ollama integration (optional) with a built-in rule-based fallback

## Features
- Role-based login: STUDENT, ADMIN, MAINTENANCE
- Student complaint submission and personal complaint tracking
- AI-assisted category and priority suggestions
- Admin complaint overview, assignment and status management
- Maintenance staff assigned-work queue
- Status history
- Dashboard summary
- PostgreSQL persistence
- JWT authentication

## Quick start (Windows 10/11)

### Requirements
1. JDK 17
2. Node.js 20 LTS (includes npm)
3. Maven 3.9+ (or use the included Maven wrapper if you add one)
4. Docker Desktop (recommended) OR PostgreSQL 16 installed locally

Check:
```powershell
java -version
node -v
npm -v
mvn -v
docker --version
```

### 1. Start PostgreSQL
From the project root:
```powershell
docker compose up -d db
```
Database: `campus_complaints`, user: `campus`, password: `campus123`, port: `5432`.
The schema is created automatically by Spring Boot (`ddl-auto=update`).

### 2. Start backend
Open a terminal:
```powershell
cd backend
mvn spring-boot:run
```
Backend: http://localhost:9090
Health check: http://localhost:8080/api/health

On first startup, demo users are created:
| Role | Email | Password |
|---|---|---|
| Admin | admin@campus.edu | Admin@123 |
| Student | student@campus.edu | Student@123 |
| Maintenance | maintenance@campus.edu | Maint@123 |

Change these passwords before any real deployment.

### 3. Start frontend
Open another terminal:
```powershell
cd frontend
npm install
npm run dev
```
Open http://localhost:5173

### 4. Optional AI with Ollama
Install Ollama from https://ollama.com, then:
```powershell
ollama pull llama3.2:3b
ollama serve
```
Set environment variables before starting backend:
```powershell
$env:AI_PROVIDER="ollama"
$env:OLLAMA_URL="http://localhost:11434"
$env:OLLAMA_MODEL="llama3.2:3b"
mvn spring-boot:run
```
If Ollama is unavailable or `AI_PROVIDER` is not `ollama`, the app uses its deterministic rule-based classifier. This fallback is an AI-assist baseline, not a trained machine-learning model.

## Build checks
Backend:
```powershell
cd backend
mvn clean package
```
Frontend:
```powershell
cd frontend
npm install
npm run build
```

## API documentation
See `docs/API.md`. All `/api/**` endpoints except `/api/auth/**`, `/api/health` and `/api/ai/classify` require:
`Authorization: Bearer <JWT>`

## Important production notes
This is an educational starter project. Before production use:
- replace the demo JWT secret with a long random secret
- change demo passwords
- use HTTPS and managed secrets
- configure CORS for your real frontend domain
- add rate limiting, audit logging, backups and file-scanning for uploads
- review privacy, retention and campus escalation policies
- do not treat AI priority suggestions as final decisions; staff should verify them

## Project structure
```text
smart-campus-complaint-system/
├── backend/       Spring Boot REST API
├── frontend/      React + Vite application
├── database/      PostgreSQL helper SQL
├── docs/          API documentation
└── docker-compose.yml
```
