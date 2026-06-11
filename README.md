# YPH — AI Teaching Orchestrator

> AI teaching assistant built on **n8n + GPT-4o**: student onboarding, lesson generation, Vision-based essay grading, and progress analytics. Telegram-native, self-hosted.

**Try the live bot:** [@YPH_your_personal_helper_bot](https://t.me/YPH_your_personal_helper_bot)

---

## Problem

Personalized tutoring doesn't scale by hand. A single tutor juggles a mixed audience (school kids to adult learners), spends hours manually grading essays, loses context between lessons, and falls back on generic materials for lack of time.

## Solution

A single Telegram interface that automates the routine through AI analysis, preserves full context per student, and shifts the tutor's focus from grading to strategy.

---

## Features

| Module | What it does |
|--------|--------------|
| **Student management** | Profiles, goals, schedule |
| **Lesson generation** | Hyper-personalized to each student's interests and level |
| **Vision audit** | OCR + grading of handwritten essays (CEFR scoring, error breakdown) |
| **Analytics** | Progress tracking and growth-area detection |
| **Calendar** | Schedule synchronization |

---

## Architecture

A three-layer data-flow design:

- **Layer 1 — Frontend (Telegram API):** interaction point, raw data capture, inline buttons.
- **Layer 2 — Orchestrator (n8n, self-hosted):** central processor, 55+ nodes in one workflow, JavaScript validation and parsing, webhook handling, data routing.
- **Layer 3 — Memory & Intelligence (Backend):**
  - **OpenAI** — GPT-4o-mini & Vision API for content generation and OCR analysis
  - **Google Workspace** — Sheets & Calendar as a 4-sheet database (Students, Errors, Vocabulary, Dialog_State)

### Stateful onboarding in a stateless environment

n8n webhooks are stateless by default. Each onboarding step (name → age → level → goal → interests → schedule) is persisted to a `Dialog_State` sheet, and `callback_query` buttons are used for critical inputs to minimize user typos.

### Resilience

- **LLM response parsing** — a universal JS parser handles raw Chat Completions and Markdown wrappers, protecting the orchestrator chain from format breakage.
- **Input protection** — closed-list selections via `callback_query` for critical data.
- **Fallbacks** — graceful handling of missing data; the bot reroutes to valid commands instead of failing.

---

## Tech stack

`n8n (self-hosted)` · `Telegram Bot API` · `OpenAI GPT-4o-mini + Vision` · `Google Sheets` · `Google Calendar` · `JavaScript`

---

## Trade-offs & limitations

| Parameter | Limitation | Rationale |
|-----------|-----------|-----------|
| Latency | 20–60s for generation / Vision | Deliberate trade-off for GPT-4o context quality; interim "please wait" messages added |
| Cost | ~$0.5/month API (10 lessons + 20 checks) | Highly economical vs. hours of manual tutor work |
| OCR quality | Depends on lighting & handwriting | Small margin of error, sufficient for assessment |
| DB lookup | Exact name match | Will migrate to unique IDs when student names collide |

---

## Roadmap (v2.0)

- Lesson reminders (auto-notify 1h before class)
- Export to Google Docs
- Audio module (Whisper API) — transcription & pronunciation analysis
- Multi-tenant isolation (SaaS mode for multiple tutors)
- Spaced-repetition flashcard generation
- Payment integration (Kaspi / Stripe)

---

## License

MIT
