# LingoPulse - Universal AI Translator & Cloud Sync

**LingoPulse** is a modern, responsive web application for real-time multilingual text translation, voice dictation, multi-language speech audio playback, and cloud-synced translation history using Firebase Firestore.

---

## 🌟 Key Features

* 🌐 **35+ Supported Languages**: Complete with native script titles (e.g., தமிழ், हिन्दी, Español) and BCP-47 locale audio tags.
* 🔍 **Searchable Language Picker Modal**: Quick filter modal to search languages by English name, native script, or country code.
* ⚡ **Dual Engine Translation Pipeline**:
  * **Primary**: MyMemory API for quick contextual translations.
  * **Fallback**: Google GTX translation engine to ensure zero downtime.
* 🗣️ **Universal Text-to-Speech (TTS)**: Dual audio engine that combines the browser's native Web Speech API with a Google Audio stream fallback, enabling speech output for all languages (Tamil, Hindi, French, Spanish, etc.).
* 🎙️️ **Speech-to-Text (STT) Voice Input**: Microphone dictation mapped dynamically to the chosen source language.
* ☁️ **Real-Time Cloud Synchronization**: Instant sync with Firebase Firestore (`artifacts/{appId}/public/data/translations`).
* ⭐ **Favorites & Searchable History**: Star important translations, filter by starred items, search through saved records, or delete history items.
* 🌙 **Dark & Light Mode**: Seamless dark mode toggling using Tailwind CSS.

---

## 🛠️ Tech Stack

* **Frontend**: HTML5, Tailwind CSS (via CDN), FontAwesome 6 Icons.
* **JavaScript**: ES6+ Modules, Web Speech API (`SpeechSynthesis` & `webkitSpeechRecognition`).
* **Backend & Database**: Firebase Auth (Anonymous/Custom Token) & Firebase Firestore.
* **Translation APIs**: MyMemory Translation API & Google GTX Endpoint.

---

## 📋 Prerequisites & Firebase Config

The application is pre-configured with the following Firebase Credentials:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyDhNSBvaFyPe0Tdp3vae7CfeoxhqxZ4vgU",
  authDomain: "translate-8a509.firebaseapp.com",
  projectId: "translate-8a509",
  storageBucket: "translate-8a509.firebasestorage.app",
  messagingSenderId: "268681835872",
  appId: "1:268681835872:web:4afa2b8528d5baf7b65a05",
  measurementId: "G-4F4N3TR8EF"
};
```

---

## 🔐 Firebase Firestore Security Rules

To ensure your app can write history, toggle stars/favorites, and delete items without permission errors, publish these rules in your **Firebase Console** $\rightarrow$ **Firestore Database** $\rightarrow$ **Rules**:

```javascript
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write, update, delete: if true;
    }
  }
}
```

---

## 🚀 How to Run the Project

1. **Clone or Download** the project repository.
2. Ensure you have the generated `index.html` file in your directory.
3. Open `index.html` in any web browser (Google Chrome, Microsoft Edge, or Brave recommended for speech recognition).
4. Alternatively, serve using VS Code **Live Server**:
   * Right-click `index.html` $\rightarrow$ Click **Open with Live Server**.

---

## 📁 File Structure

```text
├── index.html     # Single-file complete web application
└── README.md      # Project documentation & setup guide
```

---

## 💡 Usage Guide

1. **Translating Text**:
   * Type or paste text into the source text area.
   * Auto-translate updates the result as you type (or click **Translate Now**).
2. **Voice Dictation**:
   * Click the **Microphone** button and grant browser microphone permissions.
   * Speak in the language configured as the **Source Language**.
3. **Listening to Translations**:
   * Click the **Speaker** icon under the source or target box to listen to the audio playback.
4. **Starring / Favoriting**:
   * Click the **Star** icon under the translation output to save it to your Starred tab.
5. **Managing History**:
   * Use the right sidebar to search through past translations, load previous items, listen to them, or clear your history.
