<img width="1915" height="899" alt="image" src="https://github.com/user-attachments/assets/21a9f3ee-5c7b-4444-a4d1-118bf8296af7" />

# 📬 Portfolio Contact Form Notifier

An AI-powered automation workflow built with **n8n** that triggers whenever someone submits a contact form on a portfolio website. It instantly notifies the owner via Gmail and Telegram, then uses **Google Gemini** to generate and send a personalized auto-reply to the sender — all without writing a single backend route.

---

## ✨ Features

- **Webhook Trigger** — receives POST requests from any contact form
- **Gmail Notification** — sends a styled HTML email to the portfolio owner instantly
- **Telegram Notification** — pushes a real-time alert to the owner's Telegram
- **AI Auto-Reply** — Google Gemini generates a warm, personalized reply and sends it to the sender automatically
- **Fully Free** — runs on n8n Community Edition (self-hosted via Docker) with no paid subscriptions
- **Zero Downtime** — runs 24/7 in the background, no manual intervention needed

---

## 🏗️ Workflow Architecture

```
Contact Form (POST)
        │
        ▼
┌─────────────────────┐
│  Receive Contact    │  ← Webhook Node
│       Form          │
└─────────────────────┘
        │
        ├─────────────────────────────────┐
        ▼                                 ▼
┌──────────────────┐           ┌──────────────────────┐
│  Send Gmail      │           │  Send Telegram        │
│  Notification    │           │  Notification         │
└──────────────────┘           └──────────────────────┘
                                          │
                                          ▼
                               ┌──────────────────────┐
                               │  AI Auto-Reply        │
                               │  Generator            │
                               │  (Google Gemini)      │
                               └──────────────────────┘
                                          │
                                          ▼
                               ┌──────────────────────┐
                               │  Auto-Reply to        │
                               │  Sender (SMTP)        │
                               └──────────────────────┘
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| [n8n](https://n8n.io) | Workflow automation engine |
| Docker | Self-hosting n8n locally |
| Google Gemini API | AI-generated personalized replies |
| Gmail SMTP | Sending emails |
| Telegram Bot API | Real-time push notifications |

---

## 📋 Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop) installed
- A Gmail account with 2FA enabled
- A Telegram account
- A Google AI Studio account (free Gemini API key)

---

## 🚀 Setup Guide

### 1. Run n8n with Docker

```powershell
docker run -d --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n n8nio/n8n
```

Open `http://localhost:5678` in your browser and create your account.

### 2. Import the Workflow

1. Download `workflow.json` from this repo
2. In n8n, click **"..."** menu → **"Import from file"**
3. Select `workflow.json`

### 3. Configure Credentials

You need to set up 3 credentials in n8n (Settings → Credentials):

**Gmail SMTP:**
| Field | Value |
|-------|-------|
| Host | `smtp.gmail.com` |
| Port | `465` |
| SSL | On |
| User | your Gmail address |
| Password | [Gmail App Password](https://myaccount.google.com/apppasswords) |

**Telegram Bot:**
1. Open Telegram → message [@BotFather](https://t.me/BotFather)
2. Send `/newbot` and follow the steps
3. Copy the bot token into n8n
4. Get your Chat ID from [@RawDataBot](https://t.me/RawDataBot)

**Google Gemini:**
1. Go to [Google AI Studio](https://aistudio.google.com)
2. Create a free API key
3. Paste it into the Google Gemini credential in n8n

### 4. Update the Workflow

After importing, open each node and replace placeholders:
- `YOUR_EMAIL@gmail.com` → your real Gmail
- `YOUR_TELEGRAM_CHAT_ID` → your real Chat ID

### 5. Activate

Click **"Publish"** in n8n. Your production webhook URL will be:
```
http://localhost:5678/webhook/portfolio-contact
```

---

## 🌐 Connect Your Portfolio Website

In your contact form's submit handler, POST to the webhook URL:

```javascript
const handleSubmit = async (formData) => {
  await fetch("http://localhost:5678/webhook/portfolio-contact", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      name: formData.name,
      email: formData.email,
      message: formData.message
    })
  });
};
```

> **Note:** For production use, expose n8n to the internet using [ngrok](https://ngrok.com) or deploy to a cloud server, then replace `localhost:5678` with your public URL.

---

## 📁 Project Structure

```
portfolio-contact-notifier/
├── workflow.json       # n8n workflow export (credentials removed)
└── README.md           # This file
```

---

## 🔒 Security Notes

- Never commit real API keys or credentials to GitHub
- All credentials in `workflow.json` are replaced with placeholders
- Use Gmail App Passwords instead of your main Gmail password
- Rotate your Telegram bot token if accidentally exposed

---

## 📸 Workflow Preview

> Import `workflow.json` into your n8n instance to see the full visual workflow.

---

## 🔮 Planned Improvements

- [ ] Deploy n8n to a free cloud server (Render.com) for 24/7 availability without keeping PC on
- [ ] Add Google Sheets logging — store every contact form submission
- [ ] Add spam filter node — ignore submissions with suspicious content
- [ ] WhatsApp notification support

---

## 👤 Author

**MD Ashraful Hossain Rahat**
- GitHub: [@MAH-Rahat](https://github.com/MAH-Rahat)
- Built with n8n Community Edition · Self-hosted via Docker

---

## 📄 License

MIT — free to use, modify, and distribute.
