# SOP Web Recorder

A Chrome extension that records what you do in your browser — clicks, typing, scrolling, page navigations — alongside a video of your screen. The output is a small folder you can hand off to anyone (a teammate, an automation system, or a future-you trying to remember how a workflow went).

> ✨ **Designed for non-technical users.** No command line needed to record. Install once, click Start, do your task, click Stop. A folder lands in your Downloads.

---

## Install in 60 seconds

1. **Download the latest release:**
   👉 [Releases page](../../releases/latest) — grab the `sop-recorder-vX.Y.Z.zip` file at the bottom.

2. **Unzip it.** You should see files like `manifest.json`, `recorder.js`, `icons/` etc. Move the unzipped folder somewhere safe (e.g. `~/Documents/sop-recorder/`). Don't delete it — Chrome reads the files directly from this location.

3. **Open Chrome and go to:** `chrome://extensions`

4. **Turn on Developer mode** (toggle in the top-right corner).

5. **Click "Load unpacked"** and select the unzipped folder.

6. The **SOP Web Recorder** icon now appears in your Chrome toolbar (puzzle-piece icon → pin it for easy access).

That's it. You're ready to record.

---

## How to record a workflow

1. Click the **SOP Web Recorder** icon in your toolbar. A small recorder window opens.
2. Type a short name for the task (e.g. `submit_expense_report`).
3. Click **Start recording**.
4. Chrome's screen-share picker pops up. Pick what to share:
   - **Entire screen** — captures everything you see, follows tab and window switches.
   - **Window** — captures one app window.
   - **Tab** — captures one specific tab only.
5. Do your workflow normally. The recorder window shows duration + event count.
6. Click **Stop recording**.

A folder named `recording_<task>_<timestamp>/` lands in your Downloads. Inside:

```
recording_submit_expense_report_2026-05-04-10-30-15/
├── recording.webm     ← video of your screen
├── events.json        ← every click, keystroke, scroll, navigation
└── manifest.json      ← session metadata (start time, tabs used, etc.)
```

---

## What gets recorded — and what doesn't

**Recorded:**
- Mouse clicks (with the element you clicked, its visible text, position)
- Text you typed into form fields
- Special keys (Enter, Tab, Esc, arrow keys, Cmd/Ctrl combos)
- Page scrolls (resting position after scrolling stops)
- URLs you navigated to and tab switches
- A continuous video of your screen

**NOT recorded — automatically redacted at the source:**
- 🔒 **Passwords** — input fields with `type="password"` or autocomplete `current-password` / `new-password`
- 🔒 **One-time codes (OTP)** — fields with autocomplete `one-time-code`
- 🔒 **Credit card numbers, CVV, expiry, cardholder name** — fields with autocomplete `cc-*`
- 🔒 Anything where the field name, id, or aria-label matches `password`, `secret`, `token`, `otp`, `cvv`, `ssn`, `credit card` (case-insensitive)

Sensitive values **never leave the page** — the content script redacts them before they reach the recorder. Only the *length* of what you typed is kept (so the trace knows you typed 8 characters into a password field, but not which characters).

**Where does the data go?** Nowhere automatic. The bundle is saved to your local Downloads folder. Nothing is uploaded to any server. Sharing it with someone else is your choice.

---

## Updating the extension

1. Download the new `.zip` from [Releases](../../releases/latest).
2. Unzip it on top of the existing folder (overwrite when prompted).
3. Go to `chrome://extensions`, find "SOP Web Recorder", click the reload icon ⟳.

That's it — your install location stays the same, just the files inside refresh.

---

## Troubleshooting

**The extension icon doesn't appear in my toolbar.**
Click the puzzle-piece icon (🧩) at the right of the address bar, find "SOP Web Recorder" in the list, and click the pin to keep it visible.

**Clicking "Start" does nothing / picker doesn't appear.**
- Make sure you're on Chrome 116 or newer (`chrome://version` to check).
- Try fully reloading the extension at `chrome://extensions`.

**My click on a button wasn't captured.**
Some pages render certain UI inside special embedded contexts (PDF viewer, sandboxed iframes) that browsers don't allow extensions to read. Bookmark-bar clicks and Chrome's own UI also can't be captured. The video still records visually — only the structured event data is missing for those.

**The screenshot from a click is "late" / shows after the click.**
Web video has gaps when the screen isn't changing. Most clicks are fine; occasionally one lands in a gap and the closest available frame is just before or just after the click. The video itself always has the full visual.

**I want to use the recordings with the SOP automation pipeline.**
This extension is designed to feed into the [SOP project](https://github.com/ridnex/sop-agent) (separate repo). Drop the downloaded folder into that pipeline's import step and it will generate executable Standard Operating Procedures from your recording.

---

## Privacy

- **All recording stays on your machine.** The extension does not have any "send to server" code. Open `recorder.js` and search for `fetch(` — there isn't one.
- **You control sharing.** The bundle is in your Downloads folder. To share it, you copy/upload it deliberately.
- **Sensitive fields are redacted.** See "What gets recorded" above.
- **Source code is open.** Every line that runs in your browser is in this repo. Audit before installing.

---

## For developers

- `manifest.json` — Manifest V3, requires Chrome 116+
- `background.js` — service worker; opens the recorder window and pings content scripts
- `recorder.html` / `recorder.css` / `recorder.js` — the small popup window that owns the screen-capture stream and the event buffer
- `content_script.js` — runs on every page; captures clicks, inputs, keys, scrolls
- `icons/` — toolbar icons (16/48/128 px)

To work on the extension locally instead of a release zip:
```bash
git clone <this-repo>
# then in chrome://extensions → Load unpacked → pick the cloned folder
```

Bug reports and PRs welcome.

---

## License

MIT — see [LICENSE](LICENSE).
