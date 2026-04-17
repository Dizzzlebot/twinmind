# twinmind
TwinMind is a single-file browser app providing real-time conversation support. It transcribes audio via Groq Whisper in 30s chunks, using an LLM to surface talking points and fact-checks. A streaming chat allows deep-dives into the full transcript context. No backend or install needed—just a Groq API key for instant AI-driven insights.


# TwinMind — Live Suggestions

> A single-file, browser-based AI copilot that listens to your conversations in real time and surfaces relevant suggestions, fact-checks, and answers as you speak.

---

## What It Does

TwinMind listens through your microphone, transcribes what is being said in 30-second rolling chunks, and uses an LLM to generate a fresh batch of contextual suggestions after each chunk. A built-in chat panel lets you drill into any suggestion — or ask your own questions — with the full transcript as context. Everything runs in the browser with no server required.

---

## Features

| Feature | Detail |
|---|---|
| **Live transcription** | Uses Groq's Whisper API to transcribe audio every 30 seconds |
| **Auto suggestions** | After each transcription chunk, generates 3 contextual suggestions via an LLM |
| **Suggestion types** | `QUESTION` · `TALKING POINT` · `ANSWER` · `FACT CHECK` · `CLARIFY` |
| **Streaming chat** | Click any suggestion or type freely — answers stream token-by-token |
| **Manual refresh** | Force a new suggestion batch at any time without waiting for the next cycle |
| **Session export** | Download the full transcript, all suggestion batches, and chat history as JSON |
| **Configurable prompts** | Both the suggestion prompt and chat prompt are fully editable in Settings |
| **Local persistence** | Settings are saved to `localStorage` — no account or backend needed |

---

## How It Works

```
Microphone
    │
    ▼ (30s audio chunk)
Groq Whisper API  →  Transcript line appended to Column 1
    │
    ▼
Groq LLM (Kimi K2 / configurable)
    │
    ├──▶  3 suggestion cards → Column 2 (newest batch at top, older batches faded)
    │
    └──▶  Click a card or type in chat → streaming LLM answer → Column 3
```

The UI is a fixed three-column layout:
1. **Mic & Transcript** — start/stop recording; timestamped transcript lines appear here
2. **Live Suggestions** — auto-refreshing suggestion batches, newest first
3. **Chat** — streaming detailed answers grounded in the full transcript

---

## Requirements

- A modern browser with microphone access (Chrome or Edge recommended)
- A [Groq API key](https://console.groq.com/) — free tier is sufficient for most sessions

---

## Getting Started

1. Open `twinmind.html` in your browser (no build step, no install)
2. On first load, the Settings modal opens automatically
3. Paste your Groq API key and click **Save**
4. Click the microphone button to begin — suggestions appear within ~30 seconds

---

## Settings Reference

| Setting | Default | Description |
|---|---|---|
| `Groq API Key` | *(required)* | Your Groq API key (`gsk_…`) |
| `LLM Model` | `moonshotai/kimi-k2-instruct` | Any Groq-hosted chat model |
| `Whisper Model` | `whisper-large-v3` | Groq's Whisper transcription model |
| `Suggestion context` | `12 lines` | How many recent transcript lines are sent for suggestion generation |
| `Chat context` | `60 lines` | How many transcript lines are included in chat answers |
| `Suggestion Prompt` | *(see defaults)* | Fully editable — controls suggestion type selection logic |
| `Chat Prompt` | *(see defaults)* | Fully editable — controls how detailed answers are structured |

---

## Export Format

Clicking **Export** downloads a JSON file with three keys:

```json
{
  "exportedAt": "2025-01-01T12:00:00.000Z",
  "transcript": [
    { "time": "...", "text": "..." }
  ],
  "suggestionBatches": [
    {
      "time": "...",
      "suggestions": [
        { "type": "ANSWER", "preview": "..." }
      ]
    }
  ],
  "chatHistory": [
    { "role": "you", "content": "...", "time": "..." },
    { "role": "assistant", "content": "...", "time": "..." }
  ]
}
```

---

## Tech Stack

- **Vanilla HTML/CSS/JS** — zero dependencies, zero build tooling
- **Groq API** — Whisper for transcription, LLM for suggestions and chat
- **Web Speech / MediaRecorder API** — native browser audio capture
- **`localStorage`** — settings persistence across sessions

---

## Privacy Note

Audio is sent directly from your browser to the Groq API for transcription. No audio or transcript data is stored by TwinMind itself — everything lives in memory for the duration of the session and is cleared on page close (unless you export first).
