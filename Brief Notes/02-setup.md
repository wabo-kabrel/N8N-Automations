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
   - Download nvm for Windows: https://github.com/coreybutler/nvm-windows/releases
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
- This starts n8n locally at http://localhost:5678
- Every time you stop PowerShell, n8n stops (good for testing)

## Step 3: Optional - Global Install (npm)