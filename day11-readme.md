# Day 11 — Brain Files Customization + WhatsApp Policy Config

**Series:** 100 Days of AI | OpenClaw by Panaversity
**Stack:** OpenClaw 2026.6.11 · Gemini 2.5 Flash · WSL Ubuntu 24.04 · OpenCode

---

## Aaj Kya Seekha?

Aaj ka session do major cheezoon pe focused tha:
1. **OpenClaw Brain Files** — agent ki identity aur memory system
2. **WhatsApp Policies** — kaun DM kar sakta hai, groups mein kaise behave kare

---

## Part 1 — Gateway Diagnosis & Fix (OpenCode se)

Session shuru hone pe gateway band tha. OpenCode ne **Plan Mode** mein diagnosis kiya:

### Kya hua tha?
- Gateway **SIGINT (Ctrl+C)** se manually stop hua tha — crash nahi tha
- PID 902 stuck tha port 18789 pe
- `openclaw gateway start` fail hua kyunki WSL mein `systemd` nahi hota

### Fix:
```bash
# Stuck process kill karo
kill -9 902

# Fresh gateway start karo
openclaw gateway --port 18789

# WhatsApp status verify karo
openclaw channels status --probe
```

### Final Status:
```
✅ Gateway: Running on port 18789
✅ WhatsApp: enabled, linked, connected, health:healthy
✅ 9 Bundled Plugins: Active
```

---

## Part 2 — Brain Files System

OpenClaw ke `~/.openclaw/workspace/` mein yeh core files hoti hain:

| File | Kaam |
|---|---|
| `SOUL.md` | Agent ki values, behavior rules, sochne ka tarika |
| `IDENTITY.md` | Agent ka naam, personality, emoji, vibe |
| `USER.md` | User ki info — naam, timezone, preferences |
| `MEMORY.md` | Long-term memory — hamesha yaad rakhne wali cheezein |
| `HEARTBEAT.md` | Scheduled/daily tasks — agent khud automatically kare |

### Default Values Jo Mile:
- **SOUL.md:** Genuinely helpful, strong opinions, resourceful before asking, calm & concise
- **IDENTITY.md:** Name: OpenClaw Assistant, Creature: AI, Emoji: 🤖
- **USER.md:** SHAHEER AHMAD, Asia/Karachi timezone

### HEARTBEAT.md — Sabse Powerful File:
```yaml
tasks:
  - name: daily-summary
    interval: 24h
    prompt: "Send me a daily summary on WhatsApp"
```
Yahan tum define kar sakte ho koi bhi repeating task jo agent **automatically** kare.

---

## Part 3 — WhatsApp Group Behavior Test

### Jo Observe Kiya:
| Action | Expected | Actual |
|---|---|---|
| Abusive message ("aby Ullu k patthayyy") | Reply | ❌ No reply |
| Name poocha | Reply | ✅ "I am Jarvis" |

### Kyun?
- Yeh hardcoded filter **nahi** hai
- Agent ki **SOUL.md** values ki wajah se — abusive language pe respond karna uski values ke against hai
- Yeh agent ki **judgment** hai, rule nahi

---

## Part 4 — WhatsApp Policy Configuration

### Command Used:
```bash
openclaw config patch --file /tmp/whatsapp-patch.json5
```

### Final Settings:
```json
{
  "channels": {
    "whatsapp": {
      "enabled": true,
      "dmPolicy": "pairing",
      "allowFrom": ["+923127511011"],
      "groupPolicy": "open",
      "groupAllowFrom": ["*"]
    }
  }
}
```

### Behavior Table:
| Scenario | Behavior |
|---|---|
| Tumhara number (+923127511011) DM kare | ✅ Direct reply — no pairing needed |
| Koi stranger DM kare | 🔐 Pairing code bheja jayega → approve hoga → tab reply |
| Group mein @mention karo | ✅ Reply milega |
| Group mein bina mention ke message | ❌ No reply |

### Troubleshooting Jo Ayi:
```
ConfigMutationConflictError: config changed since last load
```
**Fix:** Multiple commands ek saath chalane se conflict hota hai — `config patch` se ek saath apply karo

> **Note:** Yeh changes gateway restart ke baad apply honge

---

## Key Learnings — Day 11

1. **Gateway "crash" aur "manually stopped" alag hain** — logs se pata chalta hai
2. **WSL mein systemd nahi hota** — `openclaw gateway` directly run karna padta hai
3. **Brain Files agent ko "tumhara" banati hain** — SOUL, IDENTITY, USER, MEMORY, HEARTBEAT
4. **Agent ki responses SOUL.md se driven hain** — abusive language pe reply nahi kiya judgment se, filter se nahi
5. **HEARTBEAT.md** = agent ka daily schedule — powerful automation tool
6. **`config patch`** = multiple settings ek saath safely apply karne ka tarika

---

## Useful Commands — Quick Reference

```bash
# Gateway
openclaw gateway --port 18789        # Gateway start karo
openclaw channels status --probe     # WhatsApp health check

# Config
openclaw config get channels.whatsapp          # Current settings dekho
openclaw config patch --file patch.json5       # Multiple settings apply karo
openclaw config set channels.whatsapp.dmPolicy pairing  # Single setting

# Diagnosis
openclaw doctor                      # Full health check
kill -9 <PID>                        # Stuck process kill karo
ps aux | grep openclaw               # Running processes dekho
```

---

## Environment

- **OpenClaw:** v2026.6.11
- **Model:** google/gemini-2.5-flash
- **OS:** WSL Ubuntu 24.04 on Windows 10
- **Gateway Port:** 18789
- **WhatsApp:** +923127511011 (personal number, allowlisted)

---

*Part of #100DaysOfAI challenge | Following Panaversity AI Agent Factory Book*
*GitHub: [@shaheer9023](https://github.com/shaheer9023)*
