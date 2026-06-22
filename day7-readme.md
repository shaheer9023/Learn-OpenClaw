# Day 7 — Agent-Driven OpenClaw Setup with OpenCode

**100 Days of AI** | Series: OpenClaw by Panaversity  
**Reference:** [AI Agent Factory Book — Chapter 56, Lesson 2](https://agentfactory.panaversity.org/docs/Building-OpenClaw-Apps/meet-your-personal-ai-employee/install-and-connect)

---

## Aaj Kya Seekha (What We Learned Today)

Aaj humne **OpenCode** ko use karke OpenClaw ko agent-driven tarike se setup kiya. Matlab — ek AI ne dusre AI ka setup kiya! Skills install kiye ClawHub se aur unhe OpenClaw dashboard mein active kiya.

---

## Universal Setup Pattern

Har agent framework yeh 5 steps follow karta hai:

```
1. Install     → runtime install karo
2. Configure   → LLM provider set karo (Google Gemini free tier)
3. Connect I/O → messaging channel connect karo (WhatsApp)
4. Verify      → end-to-end test karo
5. Secure      → localhost binding, pairing mode
```

---

## Chapter Folder Structure

```
openclaw-with-general-agents/
├── AGENTS.md       ← OpenCode ke liye instructions
├── CLAUDE.md       ← Claude Code ke liye instructions
└── .agents/
    └── skills/
        ├── sales-enablement/
        └── marketing-psychology/
```

---

## Skills Install Karna (ClawHub se)

```bash
# Kisi bhi GitHub skills repo se install karo
npx skills add https://github.com/coreyhaines31/marketingskills --skill sales-enablement

# Global install (sab agents ke liye)
npx skills add https://github.com/coreyhaines31/marketingskills --skill marketing-psychology -y --global

# find-skills (agent khud skills dhundh sake)
npx skills add https://github.com/vercel-labs/skills --skill find-skills
```

Skills install hone ke baad:
```bash
openclaw gateway restart
```

Dashboard mein check karo: `http://127.0.0.1:18789/skills`

---

## OpenCode ko Agent-Driven Setup ke liye Use Karna

```bash
# Chapter folder mein jao
cd /mnt/c/Users/Hp/Desktop/openclaw-with-general-agents

# OpenCode start karo — AGENTS.md automatically load hoga
opencode
```

OpenCode teen rounds mein kaam karta hai:

| Round | Kaam |
|-------|------|
| Round 1 | OpenClaw install + dashboard verify |
| Round 2 | WhatsApp connect (QR scan) |
| Round 3 | Groups enable (optional) |

---

## WhatsApp Relink Command

```bash
openclaw channels login --channel whatsapp
```

QR code terminal mein aayega. WhatsApp → Settings → Linked Devices → Link a Device → Scan!

---

## Troubleshooting

### Gateway Start Nahi Ho Raha
```bash
# Auth cache delete karo
rm ~/.openclaw/agents/main/agent/auth-profiles.json

# Doctor se fix karo
openclaw doctor --fix

# Gateway restart
openclaw gateway restart
```

### Crash Loop Fix
```bash
openclaw config set gateway.mode local
openclaw gateway restart
```

### Gateway Log Check Karna
```bash
openclaw logs --follow
# File location: /tmp/openclaw/openclaw-YYYY-MM-DD.log
```

### openclaw doctor
```bash
openclaw doctor
```
Yeh command Node.js version, network, config paths, aur service status sab check karta hai.

---

## Skills kya hoti hain?

Skills **markdown-defined** hoti hain — SKILL.md files jisme MCP endpoints, tools, workflows, aur agent rules hote hain.

```
~/.agents/skills/
├── sales-enablement/   ← Sales collateral, pitch decks banata hai
├── marketing-psychology/ ← Psychological principles apply karta hai
└── find-skills/        ← Nayi skills dhundta hai
```

---

## Free Model Limits (Google Gemini)

| Model | Req/min | Req/day |
|-------|---------|---------|
| `gemini-3.1-flash-lite-preview` | 15 | 500 |
| `gemini-2.5-flash` | 10 | 250 |
| `gemini-2.5-pro` | 5 | 50 |

Model change karne ke liye:
```bash
openclaw configure --section model
```

---

## Key Learnings

- **OpenCode + AGENTS.md** = AI agent jo khud setup karta hai
- **Skills are markdown files** — simple `.md` files jo agent ko naye capabilities deti hain
- **`npx skills add`** = ClawHub se skill install karne ka tarika
- **Auth cache** hamesha env vars pe priority leta hai — issue hone pe delete karo
- **`openclaw doctor`** = pehla troubleshooting command

---

*Part of #100DaysOfAI challenge | Following Panaversity AI Agent Factory Book*  
*GitHub: [@shaheer9023](https://github.com/shaheer9023)*
