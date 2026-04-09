
# 🦞 OpenClaw: Complete A-Z Installation Guide (2026 Edition)

This guide covers the full setup of your Personal AI Employee, including WSL2 installation, configuration, and real-world troubleshooting based on live deployment errors.

---

## 🏗️ Step 1: Install WSL2 & Ubuntu (The Foundation)
OpenClaw runs best in a Linux environment. If you are on Windows, start here:

1. Open **PowerShell as Administrator** and run:
   ```bash
   wsl --install -d Ubuntu
   ```
2. **Reboot** your computer when prompted.
3. After reboot, Ubuntu will open. Set your **Username** and **Password** (Remember this password for `sudo` commands).
4. Verify the install:
   ```bash
   wsl -l -v
   ```

---

## 🛠️ Step 2: Install OpenClaw
Inside your **Ubuntu terminal**, run the following:

1. **Auto-Installer:**
   ```bash
   curl -fsSL [https://openclaw.ai/install.sh](https://openclaw.ai/install.sh) | bash
   ```
2. **Fix PATH (If `openclaw` command is not found):**
   ```bash
   echo 'export PATH="$HOME/.openclaw/bin:$PATH"' >> ~/.bashrc
   source ~/.bashrc
   ```
3. **Verify:**
   ```bash
   openclaw --version
   ```

---

## ⚙️ Step 3: Configuration & Intelligence
We skip the standard wizard for better stability. Run these commands:

1. **Set API Key (Google Gemini):**
   ```bash
   openclaw config set providers.google.apiKey YOUR_API_KEY_HERE
   ```
2. **Set the "2026 Power Model" (Gemini 2.5 Flash):**
   ```bash
   openclaw config set agent.model google/gemini-2.5-flash
   ```
3. **Set Gateway Mode:**
   ```bash
   openclaw config set gateway.mode local
   ```

---

## 📱 Step 4: WhatsApp Connection
1. **Login:**
   ```bash
   openclaw channels login --channel whatsapp
   ```
2. **Scan:** A QR code will appear. Open WhatsApp on your phone > Linked Devices > Scan QR.
3. **Run Gateway:**
   ```bash
   openclaw gateway run
   ```

---

## ❌ Step 5: Master Troubleshooting (Shaheer's Fixes)

If you see these errors, follow these exact solutions:

| Error / Issue | Solution |
| :--- | :--- |
| **`Unrecognized key: "agent"`** | OpenClaw version mismatch. Edit the file manually: `nano ~/.openclaw/openclaw.json`. Find `"model": ""` and type `"google/gemini-2.5-flash"`. |
| **`503 Service Overloaded`** | Free tier limit hit. Wait 60 seconds. If it persists, restart with `openclaw gateway restart`. |
| **`404 Model Not Found`** | You are likely using an old model (like 1.5). Update to `google/gemini-2.5-flash`. |
| **`ETIMEDOUT` (WhatsApp)** | Your session is stuck. Run: `openclaw channels logout --channel whatsapp` then login again. |
| **Windows File Not Found** | Linux (WSL) uses a different path. To access Windows Desktop, use: `/mnt/c/Users/[Windows_User]/Desktop/`. |
| **The "Crash Loop"** | If gateway restarts 100 times, run: `openclaw config set gateway.mode local`. |

---

## 🚀 Step 6: Test Your AI Employee
Once connected, send these to your bot on WhatsApp:

1. **Memory Test:** "My name is Shaheer. Remember this." (Wait 1 min) "What is my name?"
2. **System Test:** "Check my system uptime and free RAM."
3. **File Test:** "Create a folder named 'Agent_Lab' in my home directory."

---

## 💡 Important Commands to Remember
* **Check Health:** `openclaw doctor`
* **See Logs:** `tail -f ~/.openclaw/logs/gateway.log` (The most important tool for debugging!)
* **Open Dashboard:** `openclaw dashboard`

