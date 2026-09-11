# Operation Laaste Braai

Alex's bachelors invitation, disguised as someone else's phone. One HTML file,
no build step, no dependencies — it runs anywhere that can serve a static file.

**The passcode is `1912`** (19/12, the wedding day). Three wrong tries reveals a hint.

## Put it online (free, ~2 minutes)

1. Create a new **public** repo on GitHub.
2. Upload `index.html` to the root of it. That's the only file that matters.
3. Repo → **Settings** → **Pages** → under *Source* pick **Deploy from a branch**,
   branch `main`, folder `/ (root)` → **Save**.
4. Wait a minute. It goes live at `https://<your-username>.github.io/<repo-name>/`

GitHub Pages is free and doesn't expire, so the link keeps working after the wedding.

## How it runs

Lock screen → passcode → home screen. **Five seconds after unlocking** (not after
page load) a notification drops in. Tapping it opens the group chat, the messages
flood in, and he taps his replies — he never types.

## Make it yours

Everything is in one block at the top of the `<script>` tag. Search for `CONFIG`:

| Field | What it does |
|---|---|
| `groom`, `bride`, `bestMan` | Names used throughout |
| `group` | The group-chat name |
| `operation` | The title on the invite card |
| `dateISO` | Drives the countdown **and** the Add-to-calendar link |
| `dateLong`, `calMonth`, `calDay` | Dates as displayed |
| `depart`, `place`, `placeFull` | Logistics — the location is deliberately never revealed |
| `weather` | Detail used in the app jokes |
| `passcode` | Lock-screen PIN. Any length — the dots follow it |
| `passcodeHint` | Shown only after three wrong attempts |

The countdown in the Clock app is **live** — it recalculates from `dateISO` every
time the page loads, so it counts itself down without you touching it.

Just below `CONFIG`:

- **`CAST`** — the five people in the group chat. Each has a running gag, noted in
  the comment above it: Pierre runs it like a military op, Beukes nearly leaks the
  location every time, William is the deadpan fact-checker, Thomas is permanently
  one message behind, Jean communicates exclusively in thumbs.
- **`TREE`** — the conversation. Each node has `msgs` then either `choices`
  (branch) or `next` (carry on). A message is `[who, "text", {t: typingMs, d: pauseAfter}]`.
  Add `mono: true` for the dossier-style blocks, `big: true` for a giant emoji,
  `tap: "❤️"` for a tapback reaction.
- **`CHATS`** — the inbox. First entry is the live group chat; the rest are static.
  A message is `{t: "text"}`, or `{t: "...", me: true}` for Alex, `{who: "Melt"}`
  to name a sender in a group, `{gif: "🥺"}` for an attachment.
- **`APPS`** / **`DOCK`** — the home-screen icons. Everything except Messages just
  shows a joke when tapped; that's the `s` field.

### The one shared class to watch out for

`.app` styles the home-screen icon buttons (including `animation: appIn`). The
full-screen views use `.sheet`, deliberately — when they shared `.app`, the icon
animation replayed on the open view and the app appeared to open, close and
reopen. Don't put `app` back on a `.view`.

### Swapping in a real GIF

Thomas's reply is a placeholder — a bobbing emoji in a GIF-shaped card, because an
Artifact preview can't load external images. On GitHub Pages it can. To use a real
one, find the `o.gif` branch in `addLine` and swap the `<span class="face">` for
`<img src="https://..." alt="">` with `object-fit: cover`.

## Notes

- Sound is deliberately quiet and only starts after the first tap — browsers block
  audio before then, and a link that shouts at people is a bad link.
- Phone chrome stays English (Messages, Search, Today, iMessage) because that's how
  an English-language phone renders Afrikaans conversations. Only what people
  actually typed is in Afrikaans.
- It respects `prefers-reduced-motion`: animations collapse for anyone who's asked
  their system for less movement.
- Works on a phone (fills the screen) and desktop (a phone on a dark table).
