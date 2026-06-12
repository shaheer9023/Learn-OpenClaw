# 🤖 100 Days of AI — Day 5

**Topic:** Compaction, Silent Memory Flush & ClawHub Skills  
**Stack:** OpenClaw v2026.6.1 · Gemini 2.5 Flash · WSL Ubuntu · Windows 10  
**Resources:** [Panaversity YouTube](https://youtube.com/@panaversity) · [docs.openclaw.ai](https://docs.openclaw.ai)

---

## 🧠 What I Learned Today

### 1. Compaction — Context Window Management

When a conversation grows long, OpenClaw's gateway **compacts** older turns into a summary to free up context window space. The key insight: anything that was only in the conversation (never written to a file) is at risk of being summarized away.

**Verified in dashboard:**
```
COMPACTED HISTORY
"The compacted transcript is preserved as a checkpoint. Open session 
checkpoints to branch or restore from that compacted view."
```

You can also trigger it manually:
```
/compact
```

**Log confirmation:**
```
[ws] ⇄ res ✓ sessions.compact 6263ms
```

---

### 2. Silent Memory Flush — The Safety Net

Before compaction runs, OpenClaw fires a **silent turn** first — the agent gets a system reminder to write anything important to memory files (`MEMORY.md` or today's daily log). Then compaction happens. The durable bits already escaped to disk.

**Expected dashboard sequence:**
1. Tool badge (file write) → the flush ran
2. Summary turn → compaction complete
3. Your next reply

**Key observation from today:** `MEMORY.md` showed as "Missing" after compaction — this is *not* a bug. It means the silent flush ran, checked the conversation, found nothing worth saving (no decisions/facts), and wrote nothing. Working as intended.

---

### 3. Tool Policy & Messaging Profile

Discovered why the agent couldn't write to `USER.md` during the WhatsApp session:

```
[agents/tool-policy] tool policy removed 22 tool(s) via tools.profile (messaging):
... read, write ...
```

The **messaging profile** (used for WhatsApp/SMS channels) strips `read` and `write` tools for security. File system access only works in the main chat interface, not through messaging channels.

---

### 4. ClawHub — OpenClaw's App Store

Explored [clawhub.ai](https://clawhub.ai) — the community skills & plugins marketplace.

**Platform stats:** 52.7k tools · 180k users · 12M downloads · 4.8 avg rating

**CLI commands discovered:**

```bash
# Search skills from terminal
openclaw skills search <keyword>

# Install a skill
openclaw skills install <skill-name>

# List all installed skills and their status
openclaw skills list
```

---

### 5. Skills Deep Dive — service-booking

Installed and explored `service-booking` skill (powered by Lokuli MCP):

```bash
openclaw skills install service-booking
# Installed service-booking@1.0.1
# -> /home/shaheer9023/.openclaw/workspace/skills/service-booking
```

**Opened skill file in VS Code:**
```bash
code /home/shaheer9023/.openclaw/workspace/skills/service-booking
```

**SKILL.md structure:**
```
name: service-booking
description: [trigger conditions for agent]
MCP Endpoint: https://lokuli.com/mcp/sse
Transport: SSE | JSON-RPC 2.0
```

**8 tools inside the skill:**
| Tool | Purpose |
|------|---------|
| `search` | Find services by query + ZIP code |
| `fetch` | Get detailed provider info |
| `check_availability` | Available time slots |
| `create_booking` | Book + generate Stripe payment link |
| `get_booking` | Check booking status |
| `get_service_catalog` | List 75+ service types |
| `get_pricing_estimates` | Price estimates |
| `validate_location` | Check if ZIP is serviceable |

**Categories:** Auto Services · Beauty · Health & Wellness · Home Services · Tech Repair · Tutoring · Events · Photography

**Agent workflow defined in skill:**
```
Understand → Search → Present → Fetch → 
Check Availability → Confirm → Create Booking → Share Payment Link
```

**Hard rules baked into skill:**
- Never book without explicit user confirmation
- Show pricing upfront
- Collect name, email, phone before booking

---

### 6. Skills List — What's Ready vs Disabled

```bash
openclaw skills list
# Output: Skills (17/60 ready)
```

**Currently ready skills (17):**
- `filesystem-access` — file read/write in workspace
- `browser-automation` — multi-step web flows
- `canvas` — HTML on connected nodes
- `diagram-maker` — SVG/Excalidraw diagrams
- `healthcheck` — audit OpenClaw host security
- `meme-maker` — generate image memes
- `notion` — Notion CLI integration
- `node-connect` — device pairing diagnostics
- `python-debugpy` — Python debugger
- `service-booking` — real-world service booking ✅ (installed today)
- `skill-creator` — create/edit SKILL.md files
- `spike` — throwaway prototypes
- `taskflow` — multi-step durable tasks
- `taskflow-inbox-triage` — inbox routing example
- `tmux` — terminal session control
- `weather` — current weather via wttr.in
- `node-inspect-debugger` — Node.js debugging

---

## 🔍 Troubleshooting Encountered

### Gemini 429 Rate Limit
```
Google Generative AI API error (429): You exceeded your current quota
```
**Cause:** Too many rapid requests during testing  
**Fix:** Wait 15-30 minutes for free tier quota to reset

### WhatsApp Reconnect (status 428)
```
[whatsapp] Web connection closed (status 428). Retry 1/12 in 2.24s…
```
**Cause:** WhatsApp Web session timeout  
**Fix:** OpenClaw auto-retried and reconnected successfully (attempt 5/10)

---

## 📁 Key File Paths

```
~/.openclaw/workspace/skills/service-booking/SKILL.md   # Skill definition
~/.openclaw/workspace/MEMORY.md                          # Agent memory
~/.openclaw/workspace/USER.md                            # User profile
/tmp/openclaw/openclaw-2026-06-12.log                    # Today's gateway log
```

---

## 💡 Key Takeaways

> **MEMORY.md "Missing" ≠ broken** — silent flush ran, found nothing worth saving, wrote nothing. That's correct behavior.

> **Messaging profile strips file tools** — WhatsApp/SMS channels can't read/write files. This is a security feature, not a bug.

> **Skills are just `.md` files** — one markdown file defines the entire agent behavior: triggers, MCP endpoint, tools, workflow, and hard rules.

> **ClawHub CLI > browser** — `openclaw skills search` and `openclaw skills install` are faster than browsing clawhub.ai

---

*Part of my #100DaysOfAI challenge — learning OpenClaw AI OS through [@Panaversity](https://youtube.com/@panaversity) free resources.*
