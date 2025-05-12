# 🌿 Prāna-fy — AI-Powered Wellness Micro-Interventions for Indian Youth

Prāna-fy (also known as **VitaSnap**) is a modern, mobile-first wellness app designed to deliver **5-minute daily micro-interventions** rooted in **Ayurveda**, **Indian culture**, and **evidence-based psychology**. Built for the Indian Gen Z, it combines AI, personalization, and simplicity to nudge users toward better mental and physical well-being — with zero fluff.

---

## 🚀 Features

- ✨ **AI-powered** personalized health nudges based on mood, symptoms, and habits.
- 📆 **Daily check-ins** for mood, energy, and mental clarity.
- 🧠 Smart suggestions using NLP + fallback local AI logic.
- 🪔 "Mom-Mode" — get culture-based wellness tips, as if from your desi mom.
- 🔥 5-minute **micro-habits** driven by cognitive science + Ayurveda.
- 🌐 Firebase-powered user authentication and real-time database.
- ⚡ Works offline with local fallback AI when OpenAI is unavailable.
- 💬 Optional WhatsApp-like intervention history log.

---

## 🧱 Tech Stack

| Layer | Tech |
| --- | --- |
| **Frontend** | Expo (React Native) |
| **Backend** | [Firebase](https://firebase.google.com/) (Auth, Firestore, Hosting) |
| **AI/NLP** | [OpenAI GPT-4](https://platform.openai.com/) + Local fallback logic |
| **Hosting** | Firebase Hosting (for web preview or admin tools, optional) |
| **Others** | AsyncStorage, Zustand, Tailwind (via NativeWind) |

---

## 📁 Folder Structure

```
bash
CopyEdit
pranaf/
├── assets/             # App assets: images, fonts, icons
├── components/         # Reusable UI components
├── screens/            # App screens (Home, CheckIn, Profile, etc.)
├── services/           # API wrappers (OpenAI, Firebase, etc.)
├── store/              # Global state (Zustand)
├── utils/              # AI fallback logic, helpers
├── constants/          # Colors, strings, prompts
├── App.js              # Entry point
└── README.md

```

---

## 🔧 Setup Instructions

### 1. Prerequisites

- Node.js v18+
- Expo CLI: `npm install -g expo-cli`
- Firebase account
- OpenAI API key

---

### 2. Clone & Install

```bash
bash
CopyEdit
git clone https://github.com/your-username/pranaf.git
cd pranaf
npm install

```

---

### 3. Firebase Setup

- Go to Firebase Console.
- Create a new project named `pranaf`.
- Enable:
    - Authentication (Email/Password)
    - Firestore Database
- Go to Project Settings → General → Add Android/iOS app and download `google-services.json` or `GoogleService-Info.plist` if needed.
- Copy your Firebase config to `services/firebase.js`.

```
js
CopyEdit
// services/firebase.js
import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";
import { getFirestore } from "firebase/firestore";

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "...",
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "..."
};

const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const db = getFirestore(app);

```

---

### 4. OpenAI Setup

- Get your API key from [OpenAI Dashboard](https://platform.openai.com/account/api-keys)
- Create a `.env` file:

```
ini
CopyEdit
OPENAI_API_KEY=sk-...

```

- Create wrapper function in `services/openai.js`:

```
js
CopyEdit
import { config } from "dotenv";
config();

export const fetchSuggestionFromOpenAI = async (prompt) => {
  try {
    const res = await fetch("https://api.openai.com/v1/chat/completions", {
      method: "POST",
      headers: {
        Authorization: `Bearer ${process.env.OPENAI_API_KEY}`,
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        model: "gpt-4",
        messages: [{ role: "user", content: prompt }],
      }),
    });
    const data = await res.json();
    return data.choices?.[0]?.message?.content ?? "Try a different input!";
  } catch (err) {
    console.error("OpenAI fetch error:", err);
    return null; // fallback will be triggered
  }
};

```

---

### 5. Fallback AI Logic

If OpenAI fails, we use offline AI:

```
js
CopyEdit
// utils/localAI.js
export const generateLocalSuggestion = ({ mood, symptom }) => {
  if (mood === "tired" && symptom === "headache") {
    return "Drink tulsi tea, rest your eyes, and take 5 deep breaths.";
  }
  return "Stretch for 2 minutes and listen to your favorite song!";
};

```

Use in your main suggestion engine:

```
js
CopyEdit
import { fetchSuggestionFromOpenAI } from "../services/openai";
import { generateLocalSuggestion } from "../utils/localAI";

export const getSmartSuggestion = async (input) => {
  const prompt = `Give a 5-minute Ayurvedic wellness tip for someone feeling ${input.mood} with ${input.symptom}.`;
  const openAIResponse = await fetchSuggestionFromOpenAI(prompt);
  return openAIResponse || generateLocalSuggestion(input);
};

```

---

### 6. Run the App

```bash
bash
CopyEdit
npx expo start

```

Scan QR with Expo Go or use iOS/Android simulator.

---

## 🛠️ Roadmap

- [x]  Daily check-in UI
- [x]  Firebase Auth
- [x]  Firestore integration
- [x]  OpenAI + fallback logic
- [ ]  Admin dashboard (for future coaches/moms)
- [ ]  Multilingual suggestions (Hindi + English)
- [ ]  Mood history charts

---

## 🧑‍💻 Contributing

1. Fork the repo
2. Create a new branch: `git checkout -b feature/new-feature`
3. Commit your changes
4. Push and open a PR!

---

## 📄 License

MIT © 2025 Prāna-fy Team

---

## 🫶 Made with love (and chaas) for India 🇮🇳
