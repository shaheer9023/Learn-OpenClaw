# Day 6 - OpenCode: Free Claude Code Alternative in Terminal

**AI Challenge** | By Shaheer

---

## Aaj Kya Seekha?

OpenCode ek open-source terminal-based AI coding agent hai — bilkul Claude Code jaisi feel, but **free mein**. Aaj humne isko WSL Ubuntu pe install kiya, multiple AI providers se connect kiya, aur actually files create/edit/delete karwai.

---

## Stack

- **OS:** Windows 10 + WSL Ubuntu 24.04
- **Tool:** OpenCode v1.17.7
- **Model:** Gemini 2.5 Flash-Lite (via Google API)
- **Learning Source:** Panaversity YouTube + Hands-on

---

## Complete Setup — Step by Step

### Step 1: OpenCode Install Karo

```bash
curl -fsSL https://opencode.ai/install | bash
```

Install hone ke baad PATH reload karo:

```bash
source ~/.bashrc
opencode --version
# Output: 1.17.7
```

### Step 2: API Key Set Karo

Gemini key ke liye (recommended — tool calling properly kaam karta hai):

```bash
export GOOGLE_GENERATIVE_AI_API_KEY="teri_api_key_yahan"
echo 'export GOOGLE_GENERATIVE_AI_API_KEY="teri_api_key_yahan"' >> ~/.bashrc
```

Groq key ke liye (free but rate limits strict hain):

```bash
export GROQ_API_KEY="teri_groq_key_yahan"
echo 'export GROQ_API_KEY="teri_groq_key_yahan"' >> ~/.bashrc
```

OpenRouter key ke liye (50+ models ek key se):

```bash
export OPENROUTER_API_KEY="teri_openrouter_key_yahan"
echo 'export OPENROUTER_API_KEY="teri_openrouter_key_yahan"' >> ~/.bashrc
```

### Step 3: Config File Banao (Context Limit Fix)

```bash
mkdir -p ~/.config/opencode
cat > ~/.config/opencode/opencode.json << 'EOF'
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "groq": {
      "models": {
        "llama-3.3-70b-versatile": {
          "limit": {
            "context": 100000,
            "output": 8000
          }
        }
      }
    }
  }
}
EOF
```

### Step 4: OpenCode Start Karo

```bash
cd ~
opencode
```

### Step 5: Model Select Karo

TUI open hone ke baad:

- `Ctrl+P` dabaao → model list khulegi
- Search karo **Gemini 2.5 Flash** ya **Gemini 2.5 Flash-Lite**
- Enter dabaao select karne ke liye

### Step 6: Mode Samjho

- **`Tab`** dabaao mode switch karne ke liye
- **Build mode** = actual files create/edit/delete karta hai
- **Plan mode** = sirf plan batata hai, koi change nahi karta

---

## Test — File Create Karwao

Build mode mein yeh type karo:

```
create a file called hello.py with content print("Hello, World!") at /mnt/c/Users/Hp/Desktop/hello.py
```

OpenCode khud file likhega, code add karega, aur verify bhi karega!

---

## Troubleshooting Jo Aaj Aai

### ❌ `opencode: command not found` after install

```bash
source ~/.bashrc
```

### ❌ Groq "Rate limit reached"

- Free tier pe Llama 3.3 70B bahut popular hai
- Fix: Model change karo ya Gemini use karo

### ❌ "Conversation history too large to compact"

- Groq models ka context OpenCode ke saath mismatch karta hai
- Fix: `~/.config/opencode/opencode.json` mein explicit context limits set karo (Step 3 dekho)

### ❌ OpenRouter free models hallucinate kar rahe hain

- Chote free models (Flash-Lite, Llama Scout) tool calling mein hallucinate karte hain
- Fix: `GOOGLE_GENERATIVE_AI_API_KEY` use karo — Gemini 2.5 Flash-Lite properly kaam karta hai

### ❌ `Google Generative AI API key is missing`

- `GOOGLE_API_KEY` kaam nahi karta — exact variable name chahiye:

```bash
export GOOGLE_GENERATIVE_AI_API_KEY="teri_key"
```

---

## Key Learnings

- OpenCode = Claude Code ka best free open-source alternative
- WSL Ubuntu pe perfectly kaam karta hai
- Sahi model choose karna zaroori hai — chote models hallucinate karte hain
- Gemini 2.5 Flash-Lite via Google API = best free option for actual file operations
- Build mode mein kaam karo, Plan mode sirf planning ke liye hai
- Windows files `/mnt/c/Users/Hp/` path se access hoti hain WSL mein

---

## Resources

- OpenCode Docs: https://opencode.ai/docs
- OpenRouter Free Models: https://openrouter.ai/collections/free-models
- Groq Console: https://console.groq.com
- Panaversity YouTube: Free AI tutorials
