# Claude-Code

A collection of static HTML sites for **Digital Culture IT Solutions** built and maintained with AI assistance via the Claude API (GitHub API approach).

---

## Repository Contents

| File | Description |
|------|-------------|
| `index.html` | Main Digital Culture IT Solutions website — full-featured, dark cyber aesthetic |
| `digitalculture-smb.html` | SMB-focused variant with a modern, minimal dark design |

---

## What This Repo Demonstrates

### Connecting Claude to GitHub Without a Plugin

This repo was created and edited entirely through **Claude.ai** using the **GitHub REST API** — no MCP connector, no local git, no IDE.

The workflow:
1. User provides a GitHub **Personal Access Token (PAT)** with `repo` scope
2. Claude uses `bash_tool` to make `curl` calls to the GitHub API
3. Files are read (`GET /contents/{path}`), created, or updated via `PUT /contents/{path}` with base64-encoded content

This proves that Claude can act as a remote code editor for any GitHub repo as long as it has a valid PAT — useful for quick edits, documentation drops, and file creation without leaving the chat.

---

## How Claude Edited Files via the GitHub API

### Reading a file
```bash
curl -s -H "Authorization: token <PAT>" \
  "https://api.github.com/repos/<owner>/<repo>/contents/<file>" \
  | python3 -c "import sys,json,base64; d=json.load(sys.stdin); print(base64.b64decode(d['content']).decode())"
```

### Creating a new file
```bash
CONTENT=$(python3 -c "import base64; print(base64.b64encode(open('file.html','rb').read()).decode())")
curl -s -X PUT -H "Authorization: token <PAT>" \
  -H "Content-Type: application/json" \
  -d "{\"message\":\"Add file\",\"content\":\"$CONTENT\"}" \
  "https://api.github.com/repos/<owner>/<repo>/contents/<file>"
```

### Updating an existing file (requires SHA)
```bash
SHA=$(curl -s -H "Authorization: token <PAT>" \
  "https://api.github.com/repos/<owner>/<repo>/contents/<file>" \
  | python3 -c "import sys,json; print(json.load(sys.stdin)['sha'])")

CONTENT=$(python3 -c "import base64; print(base64.b64encode(open('file.html','rb').read()).decode())")
curl -s -X PUT -H "Authorization: token <PAT>" \
  -H "Content-Type: application/json" \
  -d "{\"message\":\"Update file\",\"sha\":\"$SHA\",\"content\":\"$CONTENT\"}" \
  "https://api.github.com/repos/<owner>/<repo>/contents/<file>"
```

---

## Why We Did This

| Goal | Outcome |
|------|---------|
| Manage GitHub files without leaving Claude | Fully working via REST API + PAT |
| Avoid needing a GitHub MCP connector | Native bash_tool curl calls suffice |
| Demonstrate AI-assisted web development | Full HTML sites written and pushed by Claude |
| Build a cloud portfolio artifact | Repo lives at github.com/estarobi50/Claude-Code |

This approach is also a useful fallback for any environment where MCP connectors are unavailable or the user does not want to install additional tooling.

---

## Sites Overview

### index.html — Digital Culture IT Solutions (Main)
- Dark cyber aesthetic with Orbitron + Rajdhani fonts
- Animated background grid + scroll progress bar
- Services, Pricing, Testimonials, Contact sections
- Typewriter headline, animated stat counters
- Formspree-powered contact form
- Fully responsive

### digitalculture-smb.html — SMB Variant
- Minimal dark design with DM Sans + Syne + DM Mono fonts
- Animated canvas background
- Marquee tech-stack ticker
- Pricing calculator widget
- Optimized for small/medium business audience

---

## Author

**Eric Starobinsky** — IT Infrastructure Engineer & Cloud Consultant  
Digital Culture IT Solutions · Bergen County, NJ  
GitHub: [@estarobi50](https://github.com/estarobi50)

---

## Security Note

> **Never commit PATs to a repository.** The token used during this session was provided in chat only and should be rotated after use. Always use environment variables or GitHub Secrets for automation.
