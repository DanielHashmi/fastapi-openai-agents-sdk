---
title: FastAPI OpenAI Agents SDK
emoji: 🤖
colorFrom: blue
colorTo: green
sdk: docker
pinned: false
---

# FastAPI + OpenAI Agents SDK (Hugging Face Edition)

A production-ready FastAPI server using OpenAI Agents SDK, optimized for **Hugging Face Spaces**.

**Goal:** Get a running AI API in < 5 minutes.

---

## 🚀 Step-by-Step Deployment Guide

Follow these steps exactly. You have two options to deploy: **Option A (CLI - Recommended)** or **Option B (Web UI)**.

### Prerequisites (Do this first)

1.  **Create a Hugging Face Account:** [huggingface.co/join](https://huggingface.co/join)
2.  **Get an API Key:** You need a Groq API key from [console.groq.com](https://console.groq.com/keys).
3.  **Create an Access Token (Crucial for Option A):**
    *   Go to **Settings** > **[Access Tokens](https://huggingface.co/settings/tokens)**.
    *   Click **"Create new token"**.
    *   **Name:** `spaces-deploy` (or anything).
    *   **Permissions:** Select **"Write"** (This is required to push code).
    *   Click **"Create token"** and **COPY IT**. You will need it as your password.

---

### Step 1: Create the Space

1.  Go to [huggingface.co/new-space](https://huggingface.co/new-space).
2.  **Space Name:** `fastapi-agents` (or your choice).
3.  **License:** `MIT`.
4.  **SDK:** Select **Docker** (This is important!).
5.  **Space Hardware:** `CPU Basic` (Free) is fine.
6.  Click **"Create Space"**.

---

### Step 2: Push Your Code (Choose A or B)

#### Option A: Command Line (Best for Developers)

Run these commands in your terminal inside this project folder:

1.  **Initialize Git (if not already done):**
    ```bash
    git init
    git checkout -b main
    git add .
    git commit -m "Initial deploy"
    ```

2.  **Add the Remote:**
    *   Look at your empty Space page. You will see a clone command like `git clone https://huggingface.co/spaces/YOUR_USERNAME/YOUR_SPACE_NAME`.
    *   Run the following (replace the URL with YOUR Space's URL):
    ```bash
    git remote add space https://huggingface.co/spaces/YOUR_USERNAME/YOUR_SPACE_NAME
    ```

3.  **Push to Deploy:**
    ```bash
    git push --force space main
    ```
    *   **Username:** Your Hugging Face username.
    *   **Password:** Paste the **Access Token** you created in the "Prerequisites" step. (Do NOT use your login password).

#### Option B: Web UI (Easiest / No Git)

1.  Go to the **"Files"** tab of your newly created Space.
2.  Click **"Add file"** > **"Upload files"**.
3.  Drag and drop **ALL** files from this folder into the browser window (including `Dockerfile`, `main.py`, `pyproject.toml`, etc.).
    *   *Note: You don't need to upload `.env`, `.git`, or `__pycache__`.*
4.  In "Commit changes", type "Initial deploy" and click **"Commit changes to main"**.

---

### Step 3: Configure Environment Variables

Your app needs the API key to work. It will fail to build/run until you do this.

1.  Go to the **"Settings"** tab of your Space.
2.  Scroll down to the **"Variables and secrets"** section.
3.  Click **"New secret"** (top right of that section).
4.  **Name:** `GROQ_API_KEY`
5.  **Value:** Paste your actual Groq API key (starts with `gsk_...`).
6.  Click **"Save"**.

*Your Space will automatically restart and rebuild.*

---

### Step 4: Verify It Works

1.  Click the **"App"** tab.
2.  You should see a JSON response: `{"name":"FastAPI + OpenAI Agents SDK", ...}`.
3.  If you see "Building" or "Running", wait a moment.
4.  **Troubleshooting:** If it fails, click the **"Logs"** button to see why.

---

## 📡 API Usage

Once running, your API is available at:
`https://YOUR_USERNAME-YOUR_SPACE_NAME.hf.space`

### Chat Example
```bash
curl -X POST https://YOUR_USERNAME-YOUR_SPACE_NAME.hf.space/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "What is 25 * 4?"}'
```

### Swagger UI
Visit `https://YOUR_USERNAME-YOUR_SPACE_NAME.hf.space/docs` to test endpoints interactively.

---

## 🛠️ Local Development (Optional)

If you want to run this on your own machine first:

1.  **Install `uv`:** `curl -LsSf https://astral.sh/uv/install.sh | sh`
2.  **Install Dependencies:** `uv sync`
3.  **Setup Env:** `cp .env.example .env` (then edit `.env`)
4.  **Run:** `uv run main.py`
5.  **Open:** http://localhost:7860
