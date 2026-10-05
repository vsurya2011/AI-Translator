# 🌐 LingoPulse — Universal Translator (Python + Vercel)

A modern translator with **46 languages**, auto language detection, transliteration, voice input, voice output, favourites and searchable history. The translation logic runs in a **Python (Flask)** backend and deploys to **Vercel** as a serverless function.

## ✨ Features

- 🔤 46 languages incl. Tamil, Hindi, Telugu, Malayalam, Kannada, Bengali, Arabic, Chinese, Japanese and more
- ✨ Auto-detect source language, swap languages, RTL support
- 🔁 **3 engines with automatic fallback:** Google Translate → MyMemory → Gemini AI (optional)
- 📏 Long text is split into safe chunks and translated in parallel (up to 5,000 characters)
- 🎤 Voice input (browser Speech Recognition) and 🔊 voice output (browser voices, with a Python `/api/tts` fallback)
- ⭐ History with search, favourites, delete and clear-all
- ☁️ **Cloud history via Firestore** (optional). Without it, history is stored in the browser automatically
- 🌙 Dark / light theme, responsive layout, keyboard shortcut `Ctrl + Enter`

## 📁 Project Structure

```
lingopulse-vercel/
├─ api/index.py          Flask backend (translate, tts, history)
├─ data/languages.json   Supported languages
├─ public/index.html     Frontend UI
├─ requirements.txt
├─ vercel.json
├─ .env.example
└─ README.md
```

## 🚀 Run Locally

Requires Python 3.9+.

```bash
pip install -r requirements.txt
python api/index.py          # open http://localhost:3000
```

It works with **no configuration**. Google and MyMemory need no API key. Set the optional variables below to enable cloud history or the Gemini backup.

## ☁️ Deploy to Vercel

1. Push this folder to a GitHub repository.
2. On [vercel.com](https://vercel.com): **Add New → Project**, import the repo, set **Framework Preset: Other**.
3. (Optional) add environment variables, then click **Deploy**.

| Variable | Required | Purpose |
|----------|----------|---------|
| `FIREBASE_SERVICE_ACCOUNT` | No | Service-account JSON (one line) to store history in Firestore |
| `GEMINI_API_KEY` | No | Enables Gemini as a 3rd backup translation engine |
| `GEMINI_MODEL` | No | Defaults to `gemini-2.5-flash` |

CLI alternative: `npm i -g vercel` then `vercel --prod`.

## 🔥 Optional: Firestore Cloud History

1. Firebase Console → **Build → Firestore Database → Create database**.
2. **Project settings → Service accounts → Generate new private key**.
3. Put the whole JSON on **one line** into the `FIREBASE_SERVICE_ACCOUNT` variable on Vercel and redeploy.
4. Lock the rules. The server uses admin access, so browsers never touch Firestore:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} { allow read, write: if false; }
  }
}
```

Each browser gets a random session id (stored in `localStorage`). History is saved in the `translations` collection under that id, and the server only lets a session read or change its own records. The top-right badge shows **Cloud Synced** or **Local Backup**.

## 🔌 API

| Method | Route | Description |
|--------|-------|-------------|
| GET | `/api/languages` | Supported languages |
| POST | `/api/translate` | Body `{text, source, target}`. Returns `{translatedText, translit, detectedLang, engine}` |
| GET | `/api/tts?tl=ta&q=text` | Speech audio (mp3), max 200 characters |
| GET | `/api/history` | History for this session (header `X-Session-Id`) |
| POST | `/api/history` | Save a translation |
| PATCH | `/api/history/<id>` | Body `{isFavorite}` |
| DELETE | `/api/history/<id>` | Delete one |
| DELETE | `/api/history` | Clear all |
| GET | `/api/health` | Status |

## 🛠️ Troubleshooting

| Problem | Fix |
|---------|-----|
| "Translation failed" on Vercel | Google/MyMemory may be rate-limiting server IPs. Add `GEMINI_API_KEY` for a reliable fallback, then redeploy |
| Badge shows "Local Backup" | `FIREBASE_SERVICE_ACCOUNT` is missing or invalid. Check the Vercel function logs |
| Voice input not working | Use Chrome, Edge or Safari over `https://` (or `localhost`) and allow the microphone |
| No voice for a language | Install that language's voice in your OS, or rely on the `/api/tts` fallback |
| `500` errors | Vercel → Deployments → Functions → Logs |

## ⚠️ Notes

- The Google Translate endpoint used here (`translate.googleapis.com … client=gtx`) is **unofficial and has no uptime guarantee**. For production, switch to the official Google Cloud Translation API or keep the Gemini backup enabled.
- Anyone can call `/api/translate`. Set a spending limit on your Gemini key and consider Vercel Firewall rate limiting.
- Voice input runs in the browser (Web Speech API). It cannot run in Python.

## 🧰 Tech Stack

Python · Flask · requests · google-genai · firebase-admin (Firestore) · Tailwind CSS · vanilla JavaScript · Vercel
