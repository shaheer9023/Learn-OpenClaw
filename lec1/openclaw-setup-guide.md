# OpenClaw AI Employee Builder — Setup Guide

---

## 1. Pre-Requisites — Pehle Yeh Check Karo

### Node Version Check

```bash
node --version    # v24.x.x hona chahiye
npm --version
```

### Agar Node purana ho toh NVM se upgrade karo

```bash
# NVM install karo
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# Terminal refresh karo
source ~/.bashrc

# Node 24 install karo
nvm install 24
nvm use 24
nvm alias default 24

# Verify karo
node --version   # v24.16.0+ hona chahiye
```

---

## 2. OpenClaw Install & Update

```bash
# Latest version install karo
npm install -g openclaw@latest

# Version check karo
openclaw --version
```

---

## 3. OpenClaw Onboard — First Time Setup

```bash
openclaw onboard
```

### Onboarding mein jo options choose kiye:

| Option              | Selection                   |
| ------------------- | --------------------------- |
| Setup mode          | QuickStart (recommended)    |
| Model/auth provider | Google                      |
| Google auth method  | Gemini API Key              |
| Default model       | `google/gemini-2.5-flash` |
| Channel             | WhatsApp (QR link)          |
| WhatsApp mode       | Personal phone number       |

### Gemini API Key kahan se milegi?

> **aistudio.google.com** — free mein milti hai, credit card nahi chahiye

---

## 4. WhatsApp Setup

### Onboarding ke dauran:

1. QR code scan karo WhatsApp → **Linked Devices** → **Link a Device**
2. Apna number enter karo (e.g. `+92xxxxxxxxx`)
3. dmPolicy automatically `allowlist` set ho jaayegi

### Self Chat Enable karo (apne aap ko message karne ke liye)

```bash
openclaw config set channels.whatsapp.selfChatMode true --strict-json
```

---

## 5. WhatsApp Plugin Install

```bash
openclaw plugins install @openclaw/whatsapp
```

> **Note:** Yeh zaroori hai warna gateway logs mein WhatsApp nahi dikhega

---

## 6. Gateway — Agent Start Karo

```bash
# Gateway start karo (yeh terminal band mat karna)
openclaw gateway run
```

### Gateway sahi chal raha hai agar yeh lines dikhen:

```
[gateway] ready
[whatsapp] Listening for WhatsApp inbound messages
```

### Gateway restart karna ho toh:

```bash
# Pehle Ctrl+C dabao gateway terminal mein
# Phir dobara:
openclaw gateway run
```

---

## 7. Dashboard Access

```bash
# Token ke saath URL copy karo
openclaw dashboard --no-open
```

> Jo URL clipboard mein aaye woh **poora** browser mein paste karo
> (token included hota hai URL mein — seedha `127.0.0.1:18789` mat kholo)

**Dashboard URL format:**

```
http://127.0.0.1:18789/#token=xxxxxxxxxxxxxxxx
```

---

## 8. Skills Management

### Skills list dekhna

```bash
openclaw skills list
```

### Skill install karna (ClawHub se)

```bash
openclaw skills install <skill-name>

# Example — filesystem access skill
npx clawhub@latest install filesystem-access
```

### Skills search karna

```bash
openclaw skills search <keyword>
```

> **ClawHub kya hai?**
> ClawHub = OpenClaw ka official skills marketplace — jaise npm packages ke liye hai, waise hi skills ke liye hai.
> 13,000+ community skills available hain.

---

## 9. Pairing — Agar Koi Naya Number Message Kare

```bash
# Pending pairing requests dekhna
openclaw pairing list whatsapp

# Approve karna
openclaw pairing approve whatsapp <code>
```

---

## 10. Windows Files Access (WSL se)

### Windows drives WSL mein yahan hoti hain:

| Windows                   | WSL Path                      |
| ------------------------- | ----------------------------- |
| `C:\`                   | `/mnt/c/`                   |
| `D:\`                   | `/mnt/d/`                   |
| `C:\Users\Hp\Downloads` | `/mnt/c/Users/Hp/Downloads` |

### Windows username dhundna

```bash
ls /mnt/c/Users/
# "Hp" folder = tumhara Windows username
```

### WSL files Windows Explorer mein dekhna

```
\\wsl$\Ubuntu\home\shaheer9023
```

---

## 11. Useful Commands — Quick Reference

| Kaam            | Command                               |
| --------------- | ------------------------------------- |
| Version check   | `openclaw --version`                |
| Gateway start   | `openclaw gateway run`              |
| Dashboard open  | `openclaw dashboard --no-open`      |
| Skills list     | `openclaw skills list`              |
| Skills search   | `openclaw skills search <name>`     |
| Plugin install  | `openclaw plugins install <name>`   |
| Config set      | `openclaw config set <key> <value>` |
| Pairing list    | `openclaw pairing list whatsapp`    |
| Security audit  | `openclaw security audit --deep`    |
| Update OpenClaw | `npm install -g openclaw@latest`    |

---

## 12. WhatsApp pe Agent se Baat Karna

Agent ko natural language mein bol do — koi exact command yaad nahi rakhni:

```
list all files in my Downloads folder
explore all my drives
what's the weather today?
```

---

## 13. WSL Mein Systemd Issue (Gateway Restart Error)

Agar `openclaw gateway restart` pe yeh error aaye:

```
WSL2 needs systemd enabled
```

Toh sirf yeh karo — manually band karke dobara chalao:

```bash
# Gateway terminal mein:
Ctrl+C

# Phir:
openclaw gateway run
```

---

## 14. Common Errors aur Fix

| Error                               | Fix                                                      |
| ----------------------------------- | -------------------------------------------------------- |
| `ERR_SSL_CIPHER_OPERATION_FAILED` | Node upgrade karo —`nvm install 24`                   |
| `Auth did not match` dashboard pe | `openclaw dashboard --no-open` se fresh URL lo         |
| WhatsApp reply nahi aa raha         | `openclaw plugins install @openclaw/whatsapp` run karo |
| Gateway not detected                | `openclaw gateway run` se manually start karo          |
| `device_token_mismatch`           | Fresh dashboard URL lo token ke saath                    |

---

## Environment Info

- **OS:** Windows 10 + WSL Ubuntu
- **Node:** v24.16.0
- **npm:** v11.13.0
- **OpenClaw:** 2026.6.1
- **Model:** google/gemini-2.5-flash
- **Channel:** WhatsApp

---
