[中文](README.md) | **English**

# HF Quest

One HTML file, one route from signing up to publishing a model, one progress record shared across machines.

**Live**: https://logic77lhl.github.io/hf-quest/

A purely static single page: no build step, no dependencies, no backend, no analytics. Progress lives in your browser by default; connect a private Gist when you need it on more than one device (your own token, stored locally).

---

## What problem this solves

Hugging Face's documentation is thorough, but the path "sign up → publish your own model" is scattered. The usual state:

- a pile of bookmarks and no idea what to do next
- a lot read, nothing produced
- two days in, the streak breaks, and you can't say where you got to

So this page does exactly three things:

1. **Splits the path into 42 verifiable actions** — every lesson says what to do and how you know it's done, not "read the docs".
2. **Makes consistency visible** — streaks, a 12-week heatmap, XP, 18 badges.
3. **Keeps progress with you** — change machine, don't start over.

It is a **route map plus a progress ledger**, not a course. The course content is the official docs, linked next to each lesson.

## Who it's for

- you've used Hugging Face once or twice but never gone end to end
- you want "what do I do today", not another link collection
- you can give it 20–40 minutes a day

Not for absolute beginners (L1 onward runs code) or people looking for video courses.

## Start in 30 seconds

1. Open https://logic77lhl.github.io/hf-quest/
2. Hit "check in today" — the streak starts counting
3. Start ticking L0 in the learning path; official docs are on the right
4. Only look at cloud sync when you need a second device

No signup, no install. For offline use, save the page as `index.html` and open it (sync needs network).

## What's inside

| Stage | Topic | Lessons | After it you can |
|---|---|---|---|
| L0 | Orientation | 6 | name the Hub's six entry points, judge whether a model is commercially usable |
| L1 | Run it locally | 8 | produce a pipeline output on your own machine |
| L2 | Data and tokenizers | 6 | read a tokenizer, handle large datasets without filling the disk |
| L3 | Training and fine-tuning | 7 | run Trainer / LoRA / QLoRA / DPO |
| L4 | Inference and deployment | 7 | quantize, serve, deploy a Space, estimate cost |
| L5 | Production and contribution | 8 | upload a model, write a model card, open a PR |

**42 lessons** in total, plus:

- **12 exploration tasks** — output-oriented, e.g. compare three small Chinese models, reproduce a Daily Paper
- **12 micro-actions** — five-minute things for days you don't want to start
- **Chinese guides** for all 42 lessons, collapsible: one sentence + key points + common traps + pass criteria
- **Extended reading** — 10 advanced pieces attached to the relevant lessons
- **18 badges** — 3 / 7 / 21 / 60-day streaks, stage completion, all 42 lessons, note count, XP
- **12-week heatmap** — actions per day, darker means more
- **Live board** — Hugging Face trending top 20, with a built-in snapshot as fallback

XP: lesson +10, exploration +25, stage complete +50, daily check-in +5.

## Six tabs

| Tab | What it does |
|---|---|
| Today | check-in, streak, heatmap, today's micro-task, what to learn next |
| Path | 42 lessons, searchable, collapsible by stage, checkboxes, one-line notes |
| Explore | 12 tasks + 12 micro-actions |
| Live | trending models, click through to the model page |
| Sync | Gist connection, preview, conflict resolution, version rollback |
| Progress | overview, badges, export / import, clear, mirror switch |

Tip: press `/` to jump to lesson search.

## Cross-device sync (once per machine)

> Why this exists: the page is static and `localStorage` doesn't travel. A **private Gist** acts as the cloud save; no server needed.

### Three steps

1. **Create a token** — GitHub → Settings → Developer settings → Personal access tokens. Classic: check only `gist`. Fine-grained: Gists read and write. Set an expiry (90 days is reasonable).
2. **Create the archive** — open the Sync tab, paste the token, save, click "create new archive"; the Gist ID fills itself in.
3. **Second machine** — open the same URL, paste the same token + Gist ID, click "sync now".

Once connected: pull on load, push 4 seconds after a change. One status pill in the header:

| Status | Meaning |
|---|---|
| ✅ synced | clean |
| ⏳ pending | local changes waiting to upload |
| ⚠️ conflict | stop; your decision needed |
| ⛔ failed | the panel explains why |

### Merge rules

| Data | Rule |
|---|---|
| Lesson ticks, notes | last write wins (so un-ticking syncs too) |
| Check-in dates | union |
| Daily action counts | max |

Two cases **never write automatically** — they always show a preview first:

- both sides edited the same item since the last sync → choose local or remote per item
- the remote would un-complete something locally → list the lesson names, confirm, then apply

Principle: automatic sync only does safe things. Any merge that could lose local work waits for a human.

### Rolling back

Sync → versions → the last 12 are listed → pick one. Restoring is **non-destructive**: that version is written as a new version; history is never deleted.

## Data and privacy

| Stored | Where |
|---|---|
| progress, notes, check-ins | `localStorage["hf-quest-v1"]` |
| token, Gist ID, sync state | `localStorage["hf-sync-v1"]` |

- **The token never enters the repo or the export file.** Exports contain progress only.
- The token is still **plaintext** in that browser. Don't save it on a shared machine; use "clear" when done.
- The Gist is private, but the payload is plain JSON (GitHub doesn't encrypt it).
- The repo contains no tokens, keys, or analytics. No third-party scripts; all user input is escaped before rendering.

## Known limits

- **Sync is not atomic.** The Gist API has no conditional write (`If-Match` returns 400), so there is a millisecond window between the re-check and the write. In the worst case one write is overwritten — but every version carries a `parent`, the Gist keeps full history, and you can find and roll back. It's "detectable and recoverable", not "impossible".
- Sync needs `api.github.com` to be reachable.
- Links open `huggingface.co` by default. If that's unreachable, the Progress tab can switch to `hf-mirror.com`; the mirror lacks signup, token, and Spaces pages, so those links stay official (marked "official" in the UI).
- The live board pulls from `hf-mirror.com`'s API (CORS works, fast in mainland China) and falls back to a snapshot from 2026-10-28.
- Not a PWA, no service worker. Offline use means saving the HTML by hand.
- The data is plain JSON — don't put anything sensitive in it.

## Repo layout

```
.
├── index.html              # the whole app (HTML + CSS + JS); this is what Pages serves
├── README.md / README.en.md
└── HOW-IT-WAS-BUILT.md     # design trade-offs, sync protocol, deployment and test notes
```

No `package.json`, no build step. Edit `index.html`, refresh.

## Deploy your own

1. Fork or create a repo and put `index.html` at the root
2. Settings → Pages → Source: `main` / `/ (root)`
3. Wait a minute, then open `https://<user>.github.io/<repo>/`

Sync is optional; without a Gist it works standalone.

## License

[MIT](LICENSE)
