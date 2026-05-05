# SOP Web Recorder

Chrome extension that records your browser actions (clicks, typing, scrolling, navigation) plus a screen video. Output is a folder you can hand off to a teammate or automation pipeline.

## Install

1. Download the latest `sop-recorder-vX.Y.Z.zip` from [Releases](../../releases/latest).
2. Unzip it and keep the folder somewhere safe (Chrome reads files from this location).
3. Open `chrome://extensions`, turn on **Developer mode**, click **Load unpacked**, pick the folder.
4. Pin the extension icon from the toolbar's puzzle-piece menu.

## Record

1. Click the toolbar icon → name your task → **Start recording**.
2. Pick what to share (entire screen / window / tab).
3. Do your workflow → **Stop recording**.

A folder lands in Downloads:

```
recording_<task>_<timestamp>/
├── recording.webm   video
├── events.json      clicks, keys, scrolls, navigation
└── manifest.json    session metadata
```

## Privacy

- Nothing leaves your machine — no upload code in the extension.
- Passwords, OTP codes, and credit card fields are redacted at the source. Only the length is kept.


## Update

Download the new zip, overwrite the folder, then hit reload ⟳ on `chrome://extensions`.

