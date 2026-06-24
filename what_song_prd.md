# 🎵 What Song Is Your Life Right Now?
### Product Requirements Document · v1.0 · June 2026

---

## 1. Overview

### 1.1 Product Summary

A single-page web app where users describe their current mood, situation, or day in a few sentences. An AI persona — a dramatic late-night radio DJ — responds with one perfectly chosen song that soundtracks their life in that moment, delivered with a poetic 3–4 sentence radio monologue. The result is instantly shareable and endlessly replayable.

### 1.2 The Problem

People experience moments, emotions, and situations they want to narrate — but static playlists don't feel personal and mood-based apps feel clinical. There's no tool that creates a genuinely dramatic, personalized, story-driven music moment from a few words of input.

### 1.3 The Solution

A frictionless one-screen experience: type your vibe, hit a button, and receive a cinematic song recommendation from an AI DJ character — complete with a Spotify and YouTube link to play the song instantly. Zero sign-up. Zero complexity. Maximum drama.

---

## 2. Goals & Success Metrics

### 2.1 Goals

- Deliver a working, shareable demo in under 1 hour of vibe coding
- Create a moment users want to screenshot and share in group chats
- Demonstrate the Claude API in an engaging, non-technical way
- Work entirely client-side with no backend or auth required

### 2.2 Success Metrics

| Metric | Target |
|---|---|
| Share rate | 1 in 3 users copies or shares the result |
| Engagement | Average user runs 2+ sessions |
| Load time | First meaningful paint under 1 second |
| Accuracy | Song titles are real, findable songs 95%+ of the time |

---

## 3. User Personas

### 3.1 Primary — The Social Sharer

| | |
|---|---|
| Age | 18–30 |
| Context | Hanging out with friends, at a party, on their phone |
| Goal | Find something fun to share in the group chat right now |
| Pain point | Mood-based music apps feel boring and generic |

### 3.2 Secondary — The Developer / Tinkerer

| | |
|---|---|
| Age | 20–35 |
| Context | Building their own vibe-coded project, looking for inspiration |
| Goal | See a working example of the Claude API done simply |
| Pain point | Most AI demos are either too complex or too boring |

---

## 4. Feature Requirements

### 4.1 Priority Matrix

| Feature | Priority | Effort | Notes |
|---|---|---|---|
| Mood text input + submit button | P0 | 10 min | Core interaction |
| Claude API DJ persona call | P0 | 15 min | Heart of the product |
| Animated vinyl record while loading | P0 | 10 min | Sets the vibe |
| Song title parsed + displayed large | P0 | 10 min | The payoff moment |
| YouTube search link | P1 | 5 min | No API key needed |
| Spotify search link | P1 | 5 min | No API key needed |
| Copy result to clipboard | P1 | 5 min | Enables sharing |
| Re-roll button (new song, same vibe) | P2 | 10 min | Replayability |
| Streaming text animation | P2 | 15 min | Extra drama |

### 4.2 Core User Flow

1. User lands on the page — immediately sees the input box and prompt text
2. User types their current situation (2–5 sentences)
3. User clicks the "Ask the DJ" button
4. Vinyl record animation plays while the API call runs (1–2 seconds)
5. DJ monologue appears, with the song title highlighted prominently
6. "Play on YouTube" and "Find on Spotify" buttons appear below
7. User shares, replays, or re-rolls for a different song

---

## 5. Technical Specification

### 5.1 Stack

| | |
|---|---|
| Frontend | Single HTML file — HTML, CSS, JavaScript (vanilla) |
| AI | Anthropic Claude API — `claude-sonnet-4-6` via `fetch()` |
| Streaming | `stream: true` with EventSource for typing effect (P2) |
| Links | YouTube and Spotify search URLs — constructed client-side |
| Deployment | GitHub Pages, Vercel, or direct `.html` file share |
| Dependencies | None — zero npm, zero build step |

### 5.2 AI Persona — System Prompt

```
You are a legendary late-night radio DJ with a gift for finding the perfect song
for any human moment. When someone describes their situation, you dramatically
announce ONE specific song — real artist, real title — with a 3-4 sentence poetic
radio monologue about why it is their soundtrack right now. You speak in a warm,
cinematic, slightly theatrical voice.

End every response on a new line with exactly:
🎵 [Artist] — [Song Title]
```

### 5.3 Song Parsing

Extract the song title and artist from the final line using a regex matching the 🎵 emoji prefix. Construct search URLs as:

```
YouTube:  https://www.youtube.com/results?search_query={artist}+{title}
Spotify:  https://open.spotify.com/search/{artist}%20{title}
```

### 5.4 API Call Structure

```javascript
const response = await fetch("https://api.anthropic.com/v1/messages", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    model: "claude-sonnet-4-6",
    max_tokens: 1000,
    system: DJ_SYSTEM_PROMPT,
    messages: [{ role: "user", content: userInput }]
  })
});
```

---

## 6. Design Direction

### 6.1 Visual Aesthetic

Dark, moody, cinematic — like a jazz club at 2am crossed with a late-night radio broadcast. The design should immediately communicate atmosphere, not utility.

| | |
|---|---|
| Background | Near-black `#0D0D0D` or deep purple-black |
| Accent | Purple `#7C3AED` for interactive elements and song title |
| Typography | Monospace or slab-serif for the DJ voice output |
| Animation | CSS keyframe rotation on a vinyl SVG while loading |
| Song reveal | Large glowing song title, artist in muted text below |

### 6.2 Layout — Single Screen

```
┌─────────────────────────────────────┐
│  🎵 What Song Is Your Life Right Now? │
│  ─────────────────────────────────── │
│                                       │
│  [ Tell the DJ how life is going...  ]│
│  [ multiline textarea               ]│
│                                       │
│         [ Ask the DJ 🎙️ ]            │
│                                       │
│         🎶 (vinyl spinning)           │
│                                       │
│  ┌───────────────────────────────┐   │
│  │ DJ monologue in italic text…  │   │
│  │                               │   │
│  │  🎵 Artist — Song Title       │   │
│  └───────────────────────────────┘   │
│                                       │
│  [ ▶ YouTube ]  [ Spotify ]  [ Copy ]│
└─────────────────────────────────────┘
```

---

## 7. One-Hour Build Plan

| Time | Duration | Task |
|---|---|---|
| 0:00 – 0:10 | 10 min | HTML scaffold — dark layout, textarea, button, result area |
| 0:10 – 0:25 | 15 min | Wire Claude API call with DJ system prompt, handle response |
| 0:25 – 0:40 | 15 min | Parse song title, build YouTube + Spotify links, CSS vinyl animation |
| 0:40 – 0:50 | 10 min | Polish — re-roll button, copy button, mobile responsiveness |
| 0:50 – 1:00 | 10 min | Deploy to GitHub Pages or Vercel, share the link |

---

## 8. Out of Scope (v1)

- User accounts or saved history
- Real Spotify / YouTube API integration (search links are sufficient)
- Mobile app or PWA
- Social features — likes, comments, leaderboards
- Multiple AI DJ personalities (save for v2)
- Backend or database of any kind

---

## 9. Risks & Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| API key exposed client-side | 🔴 High for public deploy | Share as local file for demos; add a proxy for public use |
| AI suggests obscure/non-existent songs | 🟡 Medium | Prompt instructs: real, well-known songs only |
| API latency breaks the vibe | 🟢 Low | Spinning vinyl animation covers the wait elegantly |

---

## 10. Appendix — Kickoff Prompt

Drop this into Claude Code or any AI coding assistant to start the build:

```
Build me a single HTML file app called "What Song Is Your Life Right Now?".

The user types their current mood/situation into a textarea. On submit, call the
Anthropic Claude API (claude-sonnet-4-6) with a system prompt where Claude plays
a dramatic late-night radio DJ who picks ONE real song and delivers a 3-4 sentence
poetic monologue ending with:

🎵 [Artist] — [Song Title]

Parse that final line to auto-build a YouTube search link and Spotify search link.
Show a spinning vinyl record SVG animation during loading.
Dark purple aesthetic (#0D0D0D background, #7C3AED accent).
No frameworks, no build step — vanilla HTML/CSS/JS only, single file.
```

---

*Confidential · v1.0 · June 2026*
