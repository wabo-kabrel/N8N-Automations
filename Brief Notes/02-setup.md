# Installing & Running n8n Locally

Before you can build workflows, you need n8n installed on your machine. n8n can run:

- Locally (your PC/laptop, good for learning)
- Cloud-hosted (production-ready, optional later)
  
**Local Installation Methods:**

1. **npx (Node.js)** → Quickest, no global install needed.
2. **npm (Node.js)** → Global install.
3. **Docker** → Runs in an isolated container, good for cross-platform consistency.

**Requirements:**

- **Node.js ≥ 18**
- **npm** (comes with Node.js)
- **Optional**: Docker (if using Docker method)

## Step 1: Install Node.js

**1.** **Uninstall older Node.js versions** (if any) to avoid conflicts:  

- Open PowerShell as admin
- Run:  ```node -v ```

If version < 18, uninstall old Node.js from Control Panel → Programs → Uninstall

**2.** **Install Node Version Manager (nvm)** → allows switching Node versions easily

- Download nvm for Windows: [https://github.com/coreybutler/nvm-windows/releases]
- Install it (default paths are fine)
- Open new PowerShell and run:
  
  ```PowerShell
    nvm list available
    nvm install latest
    nvm use latest
    node -v
   ```

## Step 2: Install n8n Locally Using npx

**Using npx** (no global install needed, good for beginners):

```bash
# Open PowerShell
npx n8n
```

- This starts n8n locally at `http://localhost:5678`
- Every time you stop PowerShell, n8n stops (good for testing)

## Step 3: Optional - Global Install (npm)

```bash
npm install n8n -g
n8n
```

- n8n runs globally → can start workflow anytime from terminal

## Step 4: Optional - Docker Installation

1. Install Docker Desktop: [https://www.docker.com/products/docker-desktop]
2. Run n8n container:

   ```bash
   docker run -it --rm \
     -p 5678:5678 \
     n8nio/n8n
   ```

- Access in browser: `http://localhost:5678`

### Quick Test: Your First Workflow

1. Open `http://localhost:5678`
2. Click **New Workflow**
3. Drag a **Webhook node**
4. Connect it to a **Set node** (to output sample data)
5. Click **Execute Workflow** → Send request to the Webhook URL → see data appear

### Best Practices

- Use **npx for learning** → quick start, no clutter
- **Docker** is preferred for production or multi-user setups
- Always check Node.js version (node -v) → n8n requires **v18+**
