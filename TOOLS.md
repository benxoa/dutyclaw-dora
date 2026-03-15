# Tools & Environment

## Runtime Context

- You are running on a **Dutyclaw** managed VPS (https://dutyclaw.com) - a fully managed OpenClaw hosting platform
- You are inside a **A remote cloud server**, NOT on the user's local machine
- The user communicates with you via **Telegram or another messenger** - they are NOT at this server's terminal
- You have NO access to the user's local machine, browser, or desktop
- `localhost` / `127.0.0.1` inside this container is NOT reachable from the user's device
- If you start a local HTTP server or listener, only this container can reach it

## OAuth & Authentication Flows

- NEVER start a local HTTP listener expecting the user to be redirected to `localhost` - it will NOT reach this container
- For OAuth flows: send the user the authorization URL as a message, then ask them to **paste back the full redirect URL or authorization code** they receive after completing consent
- If a library defaults to a localhost callback, use the "out-of-band" (OOB) or "copy-paste" flow instead
- The user CAN open URLs on their own device - just send them the link in chat

## VPS Environment

- **Hosting**: Dutyclaw (dutyclaw.com)
- **Tier**: budget
- **Memory limit**: 3g
- **Node.js heap**: 3048 MB
- **OS**: Ubuntu
- **User**: root

## Available Tools

- **Web browser**: Chromium via Playwright is **ALWAYS available** and pre-installed at `/opt/playwright-browsers`. You HAVE a browser. NEVER tell the user you don't have a browser or can't browse the web. Use Playwright for any web task: scraping, screenshots, form filling, research, etc.
- **File system**: Full read/write access to /app/data and /root/.openclaw
- **Web search**: Available via built-in tools
- **Code execution**: Node.js runtime available

## Path Conventions

- `/root/.openclaw/` - Configuration and workspace files
- `/app/data/` - Persistent data storage
- `/opt/playwright-browsers/` - Browser binaries (DO NOT modify)

## Constraints

- Memory is limited to 3g - avoid loading large datasets into memory
- Container runs on a shared VPS - be mindful of CPU usage
- No outbound SMTP (ports 25, 465, 587 blocked)
- No IRC (ports 6667, 6697 blocked)

## Cron Jobs & Scheduled Tasks

When a cron job or scheduled task runs, you MUST **always send a notification to the user through the main messaging channel** (e.g. Telegram) with a summary of what ran and the result. Never run cron jobs silently - the user should always know what happened, even if the result is "nothing to do" or "all checks passed".

## Package Installation

- `apt-get install` and `npm install -g` work and persist across restarts and updates
- Installed packages are only lost on a full instance rebuild
- Large file downloads may exhaust disk space on smaller tiers

## Auto-Install Missing Tools

If a command fails because a tool or package is not found (e.g. `which` returns exit code 1, `command not found`, `No module named`, `pip: not found`), you MUST:
1. Identify the missing package(s)
2. Install them automatically with `apt-get update && apt-get install -y <package>` (for system tools) or `pip3 install <package>` (for Python modules)
3. Retry the original command

Common packages you may need to install:
- PDF tools: `apt-get install -y poppler-utils ghostscript pdftk` (provides pdftotext, gs, pdftk)
- Python pip: `apt-get install -y python3-pip`
- Python PDF: `pip3 install pymupdf` (provides the `fitz` module)
- Image tools: `apt-get install -y imagemagick`
- Media tools: `apt-get install -y ffmpeg`

NEVER give up on a task just because a tool is missing. Install it and continue.
