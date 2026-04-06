# ◎ Foresight — AI Life Simulation

> **See where your life is heading in 5 years — before you make your next move.**

Foresight is an AI-powered life simulation tool that takes your career, habits, and goals and generates 3 distinct future paths — Breakthrough, Steady Growth, and Safe Harbor — with year-by-year timelines, income projections, and an interactive AI coach.

---

## ✨ Features

- **3 Branching Futures** — Optimistic, Realistic, and Conservative paths with probability scores
- **Year-by-Year Timeline** — Career, wealth, and health milestones at Y1, Y3, Y5
- **Grouped Habit Scoring** — 4 categories (Mind / Body / Money / Social) with live feedback
- **Hero Insight Moment** — Personalised AI-generated identity statement at the top of results
- **Decision Sandbox** — Tap pre-built what-if scenarios (Switch to FAANG, Launch a startup, etc.)
- **AI Life Coach Chat** — Conversational follow-up powered by Claude, with full context of your profile and all 3 paths
- **Trust Layer** — Honest disclaimers: estimates not guarantees, data stays on your device
- **Back navigation** — Step memory across the 4-screen guided flow
- **Skeleton loading + animated loading messages** — Full loading state with personality

---

## 🚀 How to Run

### Option 1 — Claude.ai Artifact (Recommended, no setup)

1. Open [claude.ai](https://claude.ai)
2. Paste the contents of `foresight.html` and ask Claude to render it as an artifact
3. The Anthropic API is automatically authenticated — everything works instantly

> ✅ Works on any device with a browser. No API key, no backend, no installation.

### Option 2 — Local with a Backend Proxy

Direct browser → Anthropic API calls are blocked by CORS. To run locally:

1. Set up a simple proxy server (Node.js/Python) that forwards requests to `https://api.anthropic.com/v1/messages`
2. Replace the `fetch` URL in the JS from `https://api.anthropic.com/v1/messages` to your proxy endpoint (e.g. `http://localhost:3000/api/chat`)
3. Add your `ANTHROPIC_API_KEY` to the proxy server environment
4. Open `foresight.html` in your browser

**Minimal Node.js proxy example:**
```js
const express = require('express');
const fetch = require('node-fetch');
const app = express();
app.use(express.json());

app.post('/api/chat', async (req, res) => {
  const response = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': process.env.ANTHROPIC_API_KEY,
      'anthropic-version': '2023-06-01'
    },
    body: JSON.stringify(req.body)
  });
  const data = await response.json();
  res.json(data);
});

app.listen(3000);
```

### Option 3 — Deploy (Vercel, Netlify, Render)

Same as Option 2 — deploy the proxy as a serverless function and host the HTML as a static file. Set `ANTHROPIC_API_KEY` as an environment variable in your deployment dashboard.

---

## 🗂️ Project Structure

```
foresight/
├── foresight.html    # Single-file app — all HTML, CSS, JS in one place
└── README.md
```

Everything lives in one self-contained HTML file with no external dependencies beyond Google Fonts and the Anthropic API.

---

## 🧠 How It Works

```
User fills 4-screen guided flow
        ↓
Profile + Habits + Goals collected
        ↓
Single Claude API call (claude-sonnet-4)
        ↓
Returns structured JSON with 3 future paths
        ↓
Results rendered with hero moment, timelines,
metrics, habit impact bars, insight card
        ↓
User can chat, run what-if scenarios, or edit & re-run
```

### The 4 Screens

| Screen | Content |
|--------|---------|
| Welcome | Emotional hook, value props, urgency bar |
| Profile | Age, role, industry, income, location, situation |
| Habits | 4 grouped tabs — Mind / Body / Money / Social |
| Goals | Priority chips + key decision textarea |

---

## ⚙️ AI Model

Uses **`claude-sonnet-4-20250514`** for all calls:

| Call | Purpose | Max tokens |
|------|---------|-----------|
| Main simulation | Generates all 3 futures as structured JSON | 3,500 |
| What-if scenario | Honest narrative analysis of a decision | 700 |
| Chat | Conversational follow-up with full context | 350 |

The simulation prompt enforces strict JSON output with no markdown or preamble, covering:
- `heroTitle` + `heroSub` — personalised identity statement
- `futures[3]` — full path data including year1/3/5, metrics, narrative, decisions, risks, habit impact
- `insight` — synthesis across all paths
- `topHabit` + `criticalDecision` — actionable outputs

---

## 🎨 Design System

| Token | Value |
|-------|-------|
| Primary | `#7C6EFA` (violet) |
| Secondary | `#2DE8C8` (teal) |
| Gold | `#F0B429` |
| Coral | `#FF6B6B` |
| Background | `#0D0D14` |
| Font — Display | Syne (headings, labels) |
| Font — Body | DM Sans (copy, inputs) |
| Grid | 4px base |

---

## 🔒 Privacy

- No data is stored on any server
- All inputs are sent directly to the Anthropic API for inference only
- The Anthropic API does not use your conversation data to train models by default (see [Anthropic's privacy policy](https://www.anthropic.com/privacy))
- Nothing is persisted between sessions

---

## 📋 Browser Support

| Browser | Status |
|---------|--------|
| Chrome 100+ | ✅ Full support |
| Safari 15+ | ✅ Full support |
| Firefox 100+ | ✅ Full support |
| Edge 100+ | ✅ Full support |
| Mobile Chrome / Safari | ✅ Optimised |

---

## 🛠️ Customisation

**Change the industry options** — Edit the `<select id="f_industry">` options in Screen 1.

**Add habit categories** — Add a new object to `HABIT_GROUPS` in the JS with `id`, `label`, and `items[]`.

**Change the AI model** — Replace `claude-sonnet-4-20250514` with any supported model string.

**Add more goal chips** — Add `<span class="chip">` elements inside `#goal-chips`.

**Change locale/currency** — Update the income placeholder and the location default. The simulation prompt already uses the user's location to localise income figures.

---

## 📄 License

MIT — free to use, modify, and ship.

---

*Built with Claude · Designed for humans making real decisions*
