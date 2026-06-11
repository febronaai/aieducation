# AI Analogies — Generative Edition

A vault of hand-crafted and AI-generated analogies for understanding artificial intelligence concepts. Built for learners, educators, and anyone who wants to explain AI without jargon.

---

## What it does

- **Browse 12 curated analogies** covering AI fundamentals, LLMs, training, and safety — each with mapped terms, a key insight, and difficulty rating
- **Generate new analogies on demand** — type any AI concept and Claude creates a fresh analogy using a vivid, concrete metaphor
- **ELI5 mode** — on saved analogies, get a plain-language 3-sentence explanation aimed at a complete beginner
- **Review flow** — generated analogies appear in a pending state so you can Keep, Try Again, or Discard before saving
- **Persistent storage** — saved analogies survive page reloads
- **Clickable tags** — filter the vault by concept tag
- **Difficulty ratings** — 1.0 (common/beginner), 2.0 (rare/intermediate), 3.0 (legendary/advanced) shown in rarity colours
- **Copy to Markdown** — export any analogy as formatted markdown in one click
- **AI concept guard** — rejects non-AI topics with a friendly message

---

## Tech stack

| Layer | Tool |
|---|---|
| Frontend | Vanilla HTML + CSS + JS, single file |
| Hosting | GitHub Pages |
| AI | Anthropic Claude API (claude-sonnet-4-5) |
| Proxy | Cloudflare Workers (handles CORS + API key) |
| Storage | Artifact persistent storage (key-value) |
| Fonts | IBM Plex Mono + Press Start 2P (Google Fonts) |

---

## Design system

The app uses a custom retro terminal design system documented in `design-system.md`. Key rules:

- **Fonts:** Press Start 2P (titles/labels, 7–9px) + IBM Plex Mono (body, 14px)
- **Palette:** dark navy backgrounds, single purple accent `#6c5ce7`, warm flesh-tone pixel brain logo
- **Rules:** zero border-radius, no shadows, no gradients, no glow — retro feel from structure and typography only
- **Icons:** monochrome SVG on 28×28 grid, stroke-width 1.5, currentColor

---

## Project structure

```
/
├── index.html          # Main app (GitHub/Cloudflare version)
├── design-system.md    # Visual design reference and component specs
└── README.md           # This file
```

---

## Configuration

Two lines at the top of the script control the API target:

```js
const API_URL = 'https://your-worker.workers.dev';
const API_MODEL = 'claude-sonnet-4-5';
```

For local development or Claude artifact use, swap to:

```js
const API_URL = 'https://api.anthropic.com/v1/messages';
const API_MODEL = 'claude-sonnet-4-20250514';
```

---

## Cloudflare Worker setup

The app proxies all API requests through a Cloudflare Worker to keep the API key secret and handle CORS. Deploy this Worker code:

```js
const CORS_HEADERS = {
  'Access-Control-Allow-Origin': '*',
  'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
  'Access-Control-Allow-Headers': 'Content-Type, Authorization',
};

export default {
  async fetch(request, env) {
    if (request.method === 'OPTIONS') {
      return new Response(null, { status: 204, headers: CORS_HEADERS });
    }
    if (request.method !== 'POST') {
      return new Response('AI Analogies Proxy — OK', { status: 200, headers: CORS_HEADERS });
    }
    try {
      const body = await request.json();
      const response = await fetch('https://api.anthropic.com/v1/messages', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'x-api-key': env.ANTHROPIC_API_KEY,
          'anthropic-version': '2023-06-01'
        },
        body: JSON.stringify(body)
      });
      const data = await response.json();
      return new Response(JSON.stringify(data), {
        status: response.status,
        headers: { 'Content-Type': 'application/json', ...CORS_HEADERS }
      });
    } catch (err) {
      return new Response(JSON.stringify({ error: err.message }), {
        status: 500,
        headers: { 'Content-Type': 'application/json', ...CORS_HEADERS }
      });
    }
  }
}
```

Add your Anthropic API key as an encrypted environment variable named `ANTHROPIC_API_KEY` in the Worker settings.

---

## Live site

**[febronaai.github.io/aieducation](https://febronaai.github.io/aieducation/)**

---

## Built with

Claude Sonnet — design, code, analogies, and ELI5 explanations all generated and iterated through conversation.
