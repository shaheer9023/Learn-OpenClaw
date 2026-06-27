# Day 9 — Agent-Driven OpenClaw Setup with Gemini Free Tier

**Series:** Learning OpenClaw by Panaversity  
**Topic:** OpenCode se OpenClaw ko Gemini Free Tier ke saath setup karna  
**Stack:** OpenClaw 2026.6.8 · OpenCode 1.17.11 · Gemini 2.5 Flash · WSL Ubuntu 24.04

---

## Aaj kya seekha?

Aaj ka session **3-Actor Model** ka live demonstration tha:

- **Actor 1 (You):** Plain English mein instructions deta hai
- **Actor 2 (OpenCode):** Coding agent jo sab commands run karta hai
- **Actor 3 (OpenClaw):** AI Employee jo WhatsApp pe reply karta hai

OpenCode ne poora OpenClaw setup akele kiya — install check, gateway start, Gemini configure, WhatsApp connect — sab kuch bina ek bhi command manually type kiye.

---

## Key Commands (OpenCode ne khud run kiye)

### 1. OpenClaw version check
```bash
openclaw --version
# Output: OpenClaw 2026.6.8 (844f405)
```

### 2. Gateway mode check
```bash
openclaw config get gateway.mode
# Output: local
```

### 3. System health check
```bash
openclaw doctor
```

### 4. Gateway start karna
```bash
nohup openclaw gateway run --port 18789 > /tmp/openclaw/gateway.log 2>&1 &
```

### 5. Gemini free tier set karna
```bash
openclaw config set agents.defaults.model.primary "google/gemini-2.5-flash"
```

### 6. Gateway status check
```bash
openclaw gateway status
```

### 7. WhatsApp connect karna
```bash
openclaw channels login --channel whatsapp
# QR code scan karo WhatsApp se:
# Settings → Linked Devices → Link a Device
```

### 8. WhatsApp status verify karna
```bash
openclaw channels status --probe
# Output: WhatsApp default: enabled, linked, connected, health:healthy
```

### 9. Gateway band karna
```bash
openclaw gateway stop
# Agar process still running ho:
kill $(pgrep -f "openclaw")
```

---

## Final Status Jo Mila

```
✅ OpenClaw 2026.6.8 installed
✅ Gateway running on 127.0.0.1:18789
✅ Primary model: google/gemini-2.5-flash (free tier)
✅ WhatsApp: enabled, linked, connected, healthy
✅ DM Allowlist: 923xxxxxxxxx
✅ Gateway successfully stopped
```

---

## Troubleshooting

### Gateway start nahi ho raha?
```bash
# Wrong command (kaam nahi karta):
openclaw gateway run --local 18789   # ❌

# Sahi command:
openclaw gateway run --port 18789    # ✅
```

### Gateway stop ke baad bhi process chal raha ho?
```bash
ps aux | grep openclaw | grep -v grep
kill <PID>
```

### Model check karna
```bash
openclaw config get agents.defaults.model.primary
# Should show: google/gemini-2.5-flash
```

### OpenCode mein model change karna (VS Code mein)
```bash
# Ctrl+P mat dabao (VS Code use kar leta hai)
# Terminal mein yeh type karo:
/model
```

---

## Important Notes

- **OpenCode session:** Ctrl+C dabane se conversation history permanently delete ho jaati hai — files safe rehti hain, sirf chat history jaati hai
- **Context restore karna:** `"We were setting up OpenClaw with Gemini free tier. Continue from where we left off."` — OpenCode folder dekh ke khud samajh jaata hai
- **Gateway port:** Default 18789 (loopback only — sirf local access)
- **Dashboard URL:** http://127.0.0.1:18789/
- **Gateway logs:** /tmp/openclaw/openclaw-2026-06-28.log

---

## Resources

- Panaversity AI Agent Factory Book: https://agentfactory.panaversity.org
- OpenClaw Docs: https://docs.openclaw.ai
- Google AI Studio (free Gemini API key): https://aistudio.google.com
- GitHub: [@shaheer9023](https://github.com/shaheer9023)
