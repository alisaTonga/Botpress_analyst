# 🤖 Botpress Monitor

> A zero-dependency, single-file dashboard for monitoring and QA-analyzing a **Botpress Cloud** support chatbot — it pulls live conversations, classifies every chat, and surfaces the problems that actually hurt the user experience, with an optional AI review pass.

![Vanilla JS](https://img.shields.io/badge/stack-vanilla%20JS-f7df1e)
![No build step](https://img.shields.io/badge/build-none-brightgreen)
![Single file](https://img.shields.io/badge/deploy-single%20HTML%20file-blue)
![Dependencies](https://img.shields.io/badge/dependencies-0-success)

---

## Why

Botpress Cloud shows you individual conversations, but not *"which chats went wrong, and why?"* This dashboard answers that: it separates **real bot mistakes** (leaked template text, wrong answers, missing buttons) from **expected behaviour** (correctly declining an out-of-scope request), so you know exactly what to fix — instead of scrolling through hundreds of transcripts.

Everything runs in the browser from one `index.html`. No server, no build, no npm install.

## ✨ Features

| Tab | What it does |
|-----|--------------|
| 💬 **Conversations** | Searchable list + full transcript viewer. Bot button menus render as chips, so a missing menu is visually obvious. Full-text search across every message. |
| 📊 **Analytics** | Headline KPIs (self-service rate, ticket rate, health score) and trends over the selected window. |
| 🔧 **Error Analysis** | Technical errors and unanswered questions, grouped by root cause with suggested fixes. |
| 🚨 **Highlights** | The real problems — template/variable leaks, missing buttons at the start of a chat, wrong answers, poor UX — detected by deterministic rules **and** an AI pass. Filter by issue type and severity; click any card to jump to the conversation. |
| 💡 **Topics & Insights** | Outcome breakdown per topic — *resolved / correct escalation / investigate / fix now / dropped* — with drill-downs into each problem ticket and the AI's reasoning. |
| 📋 **Follow-up Needed** | Users the bot left without a resolution (a ticket was offered but never confirmed). |

### How problems are detected

The dashboard uses a **hybrid** approach for reliability:

- **Deterministic rules** (run on every conversation, no cost, stable across loads) catch unambiguous bugs — e.g. raw template leaks (`${...}` / `{{...}}` sent to a user) and missing button menus at known selection moments.
- **An optional LLM pass** (via Groq) reviews escalated tickets and adds context — was the escalation correct, or did the bot have the answer and fail to give it? The model *extracts structured facts* and the verdict is computed in code, which keeps results explainable and consistent.

## 🚀 Getting started

Because the app makes live API calls, serve it over `http://localhost` rather than opening the file directly — a `file://` origin can be rejected by the APIs (CORS).

```bash
# from the project folder
npx serve -p 3456 -s .
#   – or –
python -m http.server 8000
```

Then open the served URL (e.g. `http://localhost:3456`) in your browser.

## ⚙️ Configuration

Click **⚙ Settings** and enter:

| Field | Required | Notes |
|-------|----------|-------|
| **Botpress PAT** | ✅ | Personal Access Token for the Botpress Cloud API |
| **Bot ID** | ✅ | The bot you want to monitor |
| **Groq API key** | optional | Enables the AI review pass (`gsk_…`) |
| **Fetch depth** | – | How far back to load (24h / 7d / 14d / 30d) |

You need API access to **your own** Botpress bot for the dashboard to show data.

## 🔒 Privacy & security

- Credentials are stored **only in your browser's `localStorage`** — they are **never** written into the source and **no keys are committed** to this repository.
- Conversation data is fetched live at runtime and held in memory; nothing is persisted to disk by the app.
- The demo mode (button on the connect screen) uses entirely synthetic data.

## 🛠️ Tech

- Vanilla **HTML / CSS / JS** — a single self-contained file, zero dependencies, no build.
- **Botpress Cloud REST API** for conversations and messages.
- **Groq** (OpenAI-compatible) API, model `llama-3.3-70b-versatile`, for the optional QA pass.

## 📋 Notes & limitations

- Built and tested against a single German-language support bot; some keyword/intent heuristics are German-first.
- The AI "Fix Now" verdict reflects the model's per-run judgment and can vary slightly between loads; the deterministic Highlights are the stable signal.
- The 2,000-conversation fetch cap keeps loads responsive.

## 📄 License

No license yet