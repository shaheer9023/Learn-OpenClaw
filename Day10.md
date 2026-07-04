# Day 10 — OpenClaw WhatsApp Integration + Group Mentions

## Kya kiya aaj

Aaj ka din OpenClaw ko poori tarah se fresh install karke WhatsApp ke saath live connect karne mein gaya — including group mention replies test karna.

---

## Fresh Install Struggle

Purane installs (npm global + system-wide `/usr/lib/node_modules/openclaw`) conflict kar rahe the. Root-owned system install delete karne ke liye sudo lagi:

```bash
sudo npm uninstall -g openclaw
sudo rm -rf /usr/lib/node_modules/openclaw
```

Uske baad clean install:

```bash
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard
```

Version confirm:
```bash
openclaw --version
# OpenClaw 2026.6.11 (e085fa1)
```

---

## Gemini API Key Setup

```bash
export GEMINI_API_KEY="your-key-here"
openclaw config set agents.defaults.model.primary "google/gemini-2.5-flash"
```

Model set ho gaya: `google/gemini-2.5-flash`

---

## WhatsApp Channel Setup

WhatsApp login ke liye fresh terminal chahiye hota hai (QR code TTY requirement ki wajah se):

```bash
openclaw channels login --channel whatsapp
```

ClawHub se WhatsApp skill install ki:
```bash
openclaw skills install whatsapp-messaging
```

QR scan karne ke baad number pair ho gaya: **+923127511011**

---

## Gateway Run

```bash
openclaw gateway run --force
```

Gateway successfully start hua, WhatsApp provider bhi load ho gaya:
```
[whatsapp] [default] starting provider (+923127511011)
[gateway] ready
```

---

## Group Mention Test — Live Working

WhatsApp group banaya, AI ko mention karke test kiya:

```
@Shair hi
→ Shair: Hi there!

@Shair who are you?
→ Shair: I am the OpenClaw Assistant. I am an AI, and my vibe is helpful, resourceful, and calm 🤖
```

Group @mention reply successfully kaam kar raha hai.

---

## Policy Configuration — DM vs Group

### Group Policy (Open — mentions allowed)
```bash
openclaw config set channels.whatsapp.groupPolicy "open"
```

### DM Policy (kept on Pairing — no open DMs)
Faisla kiya ke abhi koi bhi random DM na kar sake, sirf approval ke baad chalu ho:
```bash
openclaw config set channels.whatsapp.dmPolicy "pairing"
```

### Self-chat enable kiya (khud ko DM karke test karne ke liye)
```bash
openclaw config set channels.whatsapp.selfChatMode true --strict-json
```

---

## Current Working Status

| Feature | Status |
|---|---|
| OpenClaw installed | v2026.6.11 |
| Model | google/gemini-2.5-flash |
| WhatsApp paired | +923127511011 |
| Group mentions | Working (tested live) |
| Random DMs from others | Blocked (pairing required) |
| Self-chat (DM to self) | Enabled |

---

## Doctor Warnings — Pending Fixes (Not Urgent)

1. **Command owner not set** — privileged commands ke liye:
   ```bash
   openclaw config set commands.ownerAllowFrom '["whatsapp:923127511011"]'
   ```

2. **Plaintext secret in config** — `gateway.auth.token` abhi plain text mein hai:
   ```bash
   openclaw secrets configure
   openclaw secrets audit --check
   ```

3. **WSL2 systemd not enabled** — daemon install ke liye future mein zaroorat pad sakti hai:
   ```
   Edit /etc/wsl.conf → [boot]\nsystemd=true
   wsl --shutdown (PowerShell se)
   ```

---

## Key Learnings

- System-wide root-owned OpenClaw install cleanup ke liye sudo zaroori hai
- WhatsApp QR login ek real interactive terminal mangta hai, bash tool se hang ho jata hai
- `dmPolicy "open"` set karne se pehle `allowFrom` list mein `"*"` add karna zaroori hai, warna saare DMs silently drop ho jate hain
- Group aur DM policies independent hain — ek open rakh sakte ho, dusra pairing pe

---

## Environment

- OpenClaw: v2026.6.11
- Model: google/gemini-2.5-flash
- WhatsApp Number: +923127511011
- OS: WSL Ubuntu on Windows
