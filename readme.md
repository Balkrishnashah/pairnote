# Pairnote

**Move text between your devices without a URL, an account, or an app.**

Pairnote is a tiny clipboard relay. Open it on one device, get a 6-character code. Open it on another device, type that code, and whatever you write shows up there too — live, both ways.

```
You ───▶  K7QX2M  ───▶  Your other laptop
```

No login. No file to email yourself. No copy-pasting through Slack just to get a snippet from your desktop to your phone.

---

## Why this exists

Most "share text between devices" tools hand you a URL to send to yourself, which means a notification, a tab, a link you have to click. Pairnote flips that: you carry a short code in your head (or your clipboard) instead, and typing it back in is the entire interface. It's built for one person syncing their own stuff across their own machines — not for sharing with others.

## How it works

1. Open the page on Device A. It generates a code automatically — something like `K7QX2M`.
2. Paste or type whatever you need to move: a snippet, a link, a full article.
3. Open the same page on Device B. Type that code into **enter code**, hit **Open**.
4. Your text is there. Keep editing on either side and it stays in sync while both tabs are open.

The code is remembered in your browser's local storage, so reopening Pairnote later reconnects automatically. You only type a code in when pairing a *new* device for the first time.

## Under the hood

| Piece | Role |
|---|---|
| `CodeGen` | Generates and validates the 6-character code. Swap the alphabet or length here. |
| `Storage` | The only module that talks to the backend (Firebase Realtime Database). Replace this block to switch providers — nothing else changes. |
| `UI` | All DOM reads and writes, isolated into small named functions. |
| `App` | Wiring: app state, event listeners, save-debounce logic. |

Everything lives in a single `index.html` so it deploys to GitHub Pages with zero build step, but the internals are split cleanly enough to debug or extend one piece at a time.

## Setup

Full step-by-step instructions, including Firebase configuration and database rules, are in [`SETUP.md`](./SETUP.md). Short version:

1. Create a free Firebase project, enable Realtime Database.
2. Drop your config into `FIREBASE_CONFIG` at the top of the script in `index.html`.
3. Set the database rules (provided in `SETUP.md`) so only the `notes/` path is writable.
4. Push to GitHub, enable Pages in repo settings.

You're live at `https://<your-username>.github.io/pairnote/`.

## A note on security

There's no login by design — the code itself is the key. That's fine for personal use: a 6-character code from a 31-character alphabet has roughly 887 billion combinations, well beyond casual guessing. It is **not** meant for sensitive secrets (passwords, financial data) since there's no encryption layer beyond what Firebase provides at rest. For day-to-day text, links, and snippets between your own devices, it's a comfortable trade-off between convenience and safety.

## Roadmap ideas

- Optional passphrase appended to a code for an extra layer on top of the random key
- Auto-expiring notes after a set number of days (TTL field already exists in storage, cleanup job not yet wired up)
- Plain-text file drop, not just typed/pasted content

---

Built for one person, on one bad Wi-Fi connection, trying to get a paragraph from a laptop to a phone without opening five apps to do it.