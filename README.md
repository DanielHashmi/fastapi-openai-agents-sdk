# FastAPI + OpenAI Agents SDK

A minimal FastAPI server using OpenAI Agents SDK with Groq (OpenAI-compatible API).

## Table of Contents

- [Prerequisites](#prerequisites)
- [Configuration](#configuration)
- [Local Development](#local-development)
- [Docker Deployment](#docker-deployment)
- [Railway Deployment](#railway-deployment)
- [API Usage](#api-usage)
- [API Endpoints](#api-endpoints)

## Prerequisites

- Python 3.11+ (for local development)
- [uv](https://docs.astral.sh/uv/) package manager (for local development)
- Docker and Docker Compose (for containerized deployment)
- Groq API key from https://console.groq.com/keys

## Configuration

Create a `.env` file in the project root:

```bash
GROQ_API_KEY=your_groq_api_key_here
```

## Local Development

### Install Dependencies

```bash
uv sync
```

### Run the Server

```bash
uv run main.py
```

The server starts at http://localhost:8000

## Docker Deployment

### Quick Start

1. **Clone and navigate to the project:**
   ```bash
   cd fastapi-openai-agents-sdk
   ```

2. **Create your environment file:**
   ```bash
   cp .env.example .env
   ```

3. **Add your Groq API key to `.env`:**
   ```bash
   GROQ_API_KEY=your_actual_api_key_here
   ```

4. **Build and run with Docker Compose:**
   ```bash
   docker compose up --build
   ```

   > **Note:** The first build downloads base images and dependencies, which may take a few minutes. Subsequent builds use cached layers and are much faster.

   The API is now available at http://localhost:8000

### Docker Commands Reference

| Command | Description |
|---------|-------------|
| `docker compose up --build` | Build and start the container |
| `docker compose up -d` | Start in detached (background) mode |
| `docker compose down` | Stop and remove containers |
| `docker compose logs -f` | View logs in real-time |
| `docker compose restart` | Restart the service |
| `docker compose ps` | List running containers |

### Manual Docker Build (without Compose)

```bash
# Build the image
docker build -t fastapi-agents .

# Run the container
docker run -d \
  --name fastapi-agents \
  -p 8000:8000 \
  --env-file .env \
  --restart unless-stopped \
  fastapi-agents
```

### Verify Deployment

Check if the container is running:
```bash
docker compose ps
```

Check container health:
```bash
curl http://localhost:8000/health
```

Expected response:
```json
{"status": "ok", "model": "openai/gpt-oss-20b", "provider": "groq"}
```

### View Logs

```bash
# All logs
docker compose logs

# Follow logs in real-time
docker compose logs -f

# Last 100 lines
docker compose logs --tail 100
```

### Stop and Clean Up

```bash
# Stop containers
docker compose down

# Stop and remove volumes
docker compose down -v

# Remove built images
docker compose down --rmi local
```

## Railway Deployment

Deploy this project to [Railway](https://railway.app) in minutes. Railway automatically detects the Dockerfile and handles everything.

### Prerequisites

- Railway account (sign up at https://railway.app)
- Groq API key from https://console.groq.com/keys

### Deploy Steps

1. **Go to Railway Dashboard:**

   Visit https://railway.app/new

2. **Deploy from GitHub:**

   - Click **"Deploy from GitHub repo"**
   - Connect your GitHub account (if not already connected)
   - Select the `fastapi-openai-agents-sdk` repository
   - Click **"Deploy Now"**

3. **Add Environment Variable:**

   - Click on your newly created service
   - Go to the **"Variables"** tab
   - Click **"+ New Variable"**
   - Add:
     - **Name:** `GROQ_API_KEY`
     - **Value:** `your_groq_api_key_here`
   - Click **"Add"**

4. **Generate Public URL:**

   - Go to the **"Settings"** tab
   - Scroll to **"Networking"**
   - Click **"Generate Domain"**
   - Your app is now live at `https://your-app.up.railway.app`

### Verify Deployment

```bash
# Replace with your Railway URL
curl https://your-app.up.railway.app/health
```

Expected response:
```json
{"status": "ok", "model": "openai/gpt-oss-20b", "provider": "groq"}
```

### Test the API

```bash
curl -X POST https://your-app.up.railway.app/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "What is 25 * 4?"}'
```

### Railway Dashboard Features

| Feature | Location |
|---------|----------|
| View logs | **Deployments** → Select deployment → **View Logs** |
| Redeploy | **Deployments** → **Redeploy** |
| Environment variables | **Variables** tab |
| Custom domain | **Settings** → **Networking** → **Custom Domain** |
| Usage & billing | **Settings** → **Usage** |

### Troubleshooting Railway

**Build fails:**
- Check the build logs in the **Deployments** tab
- Ensure the Dockerfile is in the root directory

**App crashes after deploy:**
- Verify `GROQ_API_KEY` is set in **Variables**
- Check runtime logs for errors

**Cannot access the URL:**
- Ensure you generated a domain in **Settings** → **Networking**
- Wait a few seconds for DNS propagation

## API Usage

### Chat Endpoint

```bash
curl -X POST http://localhost:8000/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "What is 25 * 4?"}'
```

Example response:
```json
{"response": "The result of 25 × 4 is **100**."}
```

### Weather Tool

```bash
curl -X POST http://localhost:8000/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "What is the weather in Tokyo?"}'
```

Example response:
```json
{"response": "Tokyo is 68°F and clear."}
```

### Streaming Chat

```bash
curl -X POST http://localhost:8000/chat/stream \
  -H "Content-Type: application/json" \
  -d '{"message": "Tell me about the weather in Tokyo"}'
```

### Using PowerShell (Windows)

```powershell
Invoke-RestMethod -Uri "http://localhost:8000/chat" `
  -Method POST `
  -ContentType "application/json" `
  -Body '{"message": "What is 25 * 4?"}'
```

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | API information |
| GET | `/health` | Health check |
| GET | `/docs` | Swagger UI documentation |
| GET | `/redoc` | ReDoc documentation |
| POST | `/chat` | Send message, get response |
| POST | `/chat/stream` | Send message, stream response |

## Model

This project uses `openai/gpt-oss-20b` via Groq's OpenAI-compatible API endpoint.

## Architecture

```
                    +------------------+
                    |   Docker Host    |
                    |                  |
  HTTP :8000        |  +------------+  |
  ─────────────────►│  │  FastAPI   │  │
                    |  │  Container │  │
                    |  +-----┬------+  |
                    |        │         |
                    +--------|─────────+
                             │
                             ▼
                    +------------------+
                    │    Groq API      │
                    │ (OpenAI-compat)  │
                    +------------------+
```

## Troubleshooting

### Container won't start
- Verify `.env` file exists and contains `GROQ_API_KEY`
- Check logs: `docker compose logs`

### API returns 500 error
- Verify your Groq API key is valid
- Check if you have API credits remaining

### Port already in use
- Change the port mapping in `docker-compose.yml`:
  ```yaml
  ports:
    - "8001:8000"  # Use port 8001 instead
  ```

### Health check failing
- Wait for the container to fully start (10s start period)
- Check if the application is running: `docker compose logs`

### Windows-specific issues
- If using Git Bash, `curl` commands work as shown
- If using PowerShell, use `Invoke-RestMethod` (see examples above)
- If using CMD, ensure `curl` is available or use PowerShell

### Docker build fails
- Ensure Docker Desktop is running
- Check available disk space (build requires ~500MB)
- Try clearing Docker cache: `docker builder prune`
