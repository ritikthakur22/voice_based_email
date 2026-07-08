# Voice Controlled Mail System (FastAPI Demo)

An academic, highly interactive, and entirely voice-controlled modern webmail application. Built entirely with Python (FastAPI), Jinja2, and Vanilla JavaScript, this project mimics the look, feel, and functionality of a modern email client like Gmail, but relies solely on local JSON storage and local AI models for full voice accessibility.

## 🚀 What is this project?
This project is an experimental Software Engineering demonstration intended to bridge the gap between traditional UI navigation and full accessibility via Natural Language Processing (NLP). 
The primary feature is its **Hands-Free Interactive Voice Engine**, which allows users to navigate folders, read emails, search, and even compose complex emails entirely through conversational voice prompts without touching a keyboard or mouse.

## 🛠️ Technology Stack & Versions
- **Backend Framework:** FastAPI (Python 3.12+)
- **Templating:** Jinja2
- **Frontend:** Vanilla HTML5, CSS3, JavaScript (No Node.js or Webpack required)
- **Speech-to-Text (STT):** `faster-whisper` (Runs completely locally on CPU)
- **Text-to-Speech (TTS):** `edge-tts` (High-quality neural voices)
- **Database:** Local JSON File System (Abstracted for easy replacement)

## 🏗️ Architecture & Flow
```mermaid
graph TD;
    User((User Speaking)) -->|Audio via Mic| Browser(Browser VAD Engine);
    Browser -->|Uploads .webm| API[/FastAPI /api/transcribe/];
    API -->|Processes Audio| STT[Faster-Whisper STT];
    STT -->|Extracts Text| IntentParser{Regex Intent Parser};
    IntentParser -->|Routes Command| Controller(FastAPI Router);
    Controller -->|Triggers Action| Storage[(JSON Local Storage)];
    Controller -->|Returns Intent| Browser;
    Browser -->|Updates UI| UI[Jinja2 HTML Template];
    Browser -->|Requests TTS| TTS_API[/FastAPI /api/speak/];
    TTS_API -->|Generates Audio| TTS[Edge-TTS Engine];
    TTS -->|Plays Sound| AudioOut((Audio Output));
```

## 🎙️ Voice Actions & Triggers
The application defaults to **Auto Voice Mode**. It continuously listens and uses Voice Activity Detection (VAD) to know when you finish speaking. You can toggle to Manual mode by clicking the button above the microphone.

### Navigation Triggers
- `"Compose mail"`, `"Write mail"`, `"Compose compose"` -> Opens the compose editor.
- `"Open inbox"`, `"Show emails"`, `"Open mail"` -> Navigates to the Inbox.
- `"Open start"`, `"Open star"` -> Navigates to the Starred folder.
- `"Open send"`, `"Open sent"` -> Navigates to the Sent folder.
- `"Open drafts"` -> Navigates to Drafts.
- `"Open spam"`, `"Spam"` -> Navigates to Spam.
- `"Open trashcan"`, `"Open bin"`, `"Open trash"` -> Navigates to the Trash folder.
- `"Open history"` -> Navigates to History.
- `"Open settings"` -> Navigates to Settings.

### Action Triggers
- `"Search [query]"` -> Searches your emails for the specified query.
- `"Read latest email"` -> Reads aloud the contents of the most recent email.

### Interactive Voice Composer
Once in the Compose section, the system starts a conversational state machine:
1. It asks for the **Recipient** and waits for your voice.
2. It asks for the **Subject**.
3. It asks for the **Body**.
4. It asks whether you want to **Send, Save as Draft, or Discard**.
*(It confirms your input at every step and allows you to say "No" to retry).*

## 📂 Project Structure
```text
/
├── main.py                    # Main FastAPI server entrypoint
├── requirements.txt           # Python dependencies
├── seed_emails.py             # Script to generate demo emails
├── progress.md                # Development log
├── LICENSE                    # MIT License
├── README.md                  # This file
├── docs/                      # Documentation
│   ├── API.md
│   ├── Architecture.md
│   ├── Future_Gmail_Integration.md
│   └── Project_Report.md
├── storage/                   # Local database directory
├── tests/                     # Pytest Unit testing scripts
├── static/                    # Frontend assets
│   ├── css/style.css          # Gmail-like CSS
│   └── js/app.js              # State Machine & API bindings
├── templates/                 # Jinja2 HTML templates
└── app/                       # Python Backend
    ├── api/                   # FastAPI routers
    ├── intent/                # Natural Language Parser logic
    ├── services/              # Storage interface implementation
    └── speech/                # Audio processing wrappers
```

## 🌟 Pros & Advantages
- **Privacy First:** The Speech-To-Text transcription happens completely locally on your hardware using `faster-whisper`. No API keys needed.
- **Highly Accessible:** Ideal for visually impaired users or scenarios where hands-free computing is required.
- **Lightweight & Fast:** By avoiding heavy JS frameworks like React and compiling steps, the system boots instantly and consumes minimal memory.
- **Abstracted Data Layer:** The storage mechanism is abstracted under `StorageService`, meaning you can swap the JSON backend for PostgreSQL or an external API seamlessly.

## 🚀 Future Enhancements
- **Real Email Integration:** Implementing the Google Gmail API or Microsoft Graph API inside a new `GmailStorageService` class to fetch and send real emails.
- **Advanced Machine Learning:** Replacing the Regex-based Intent Parser with a localized LLM (like Llama 3) for contextual awareness and conversational understanding (e.g., "Reply to my boss and tell him I will be late").
- **Speaker Diarization:** Using AI to recognize *who* is speaking, ensuring the system only accepts commands from the authenticated owner.
