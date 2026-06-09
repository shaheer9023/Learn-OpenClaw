# Day 4 — OpenClaw Memory System + WhatsApp Re-linking

## What I Explored Today

### OpenClaw Memory System
- OpenClaw agents can **save information to memory files** automatically during a conversation
- Memory is stored as **date-based markdown files**: `memory/YYYY-MM-DD.md` inside the workspace
- When you tell the agent something worth remembering (e.g. "reply in bullet points, max 5 lines"), it writes that instruction to the memory file using the filesystem write tool
- The memory is then available in future sessions as context

### Memory File Location
```
~/.openclaw/workspace/memory/YYYY-MM-DD.md
```

### MEMORY.md Brain File
- `MEMORY.md` is one of the 9 core Brain files
- If it shows as **"Missing"** in the Control UI, the agent still creates daily memory notes — but MEMORY.md controls *how* memory injection works
- Without MEMORY.md, the agent falls back to default memory behavior

### Memory Flow
```
User tells agent something → Agent writes to memory/YYYY-MM-DD.md → Next session loads that file → Agent remembers
```

---

## WhatsApp Re-linking Troubleshooting

### Problem
Needed to link a different WhatsApp number. Channel config delete kept re-linking the old session.

### Root Cause
WhatsApp credentials are stored in **two separate locations**:
1. `~/.openclaw/workspace/.claw/channels/whatsapp/` — session data
2. `~/.openclaw/credentials/whatsapp/default/creds.json` — actual credentials ← **this is the real one**

Deleting channel config via CLI only removes the config entry, not the credentials.

### Fix
```bash
# Delete actual credentials
rm -rf ~/.openclaw/credentials/whatsapp/

# Delete session data
rm -rf ~/.openclaw/workspace/.claw/channels/
rm -rf ~/.openclaw/workspace/.claw/sessions/

# Reconfigure fresh
openclaw configure --section channels
```

---

## Key Learnings

- Memory in OpenClaw = filesystem-based, date-organized markdown files
- Agent uses `filesystem write` tool to persist memory mid-conversation
- WhatsApp credentials live at `~/.openclaw/credentials/whatsapp/` — not in `openclaw.json`
- `MEMORY.md` missing ≠ memory broken; agent still writes daily notes

---

## Stack
- OS: Windows 10 + WSL Ubuntu
- OpenClaw: v2026.6.1
- Model: Google Gemini 2.5 Flash
- Learning Source: Panaversity (YouTube)
