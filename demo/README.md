# Agent Ping demo video

54-second HyperFrames composition showing the real Agent Ping contract: shell blocks, you reply on the banner, stdout unblocks the agent, agent continues in chat.

## Preview

```bash
cd demo
npm run dev
```

## Render

```bash
cd demo
npm run render
```

Output: `demo/renders/demo_<timestamp>.mp4` (gitignored).

## Assets

`demo/assets/logo.svg` ships with the repo. The background music (Mixkit) and PP
Telegraf fonts are licensed for use but not for redistribution, so they are
gitignored. To render with audio and the intended type, drop your own copies at
`demo/assets/bg-music.mp3` and `demo/assets/fonts/telegraf/PPTelegraf-Regular.woff2`
(plus `-UltraBold`).

## Storyboard (accurate)

Each beat follows the actual CLI flow:

1. **Agent** announces what it will run (chat tool call)
2. **Terminal** types the command; blocking verbs show `⏳ blocked — waiting...`
3. **Banner** slides in; camera **zooms in** for interaction, **zooms out** before dismiss
4. **You** type/click on the notification (pointer is in camera space, aligned to buttons)
5. **Terminal** unblocks with stdout / exit code (`→ stdout: dev`, `→ approved (exit 0)`)
6. **Agent** responds in chat with the next step (not fake user chat bubbles)

| Verb | Blocks? | Terminal result | Agent chat after |
|------|---------|-----------------|------------------|
| notify | No | `→ delivered (exit 0)` | "Banner sent. Agent keeps going." |
| ask | Yes | `→ stdout: dev` | "Got dev from stdout. Merging..." |
| choose | Yes | `→ chose ship (exit 1)` | "Ship selected. Last gate..." |
| confirm | Yes | `→ approved (exit 0)` | "Approved. Running migrate.sh." |

## Motion fixes

- Separate `zoomIn()` / `zoomOut()` with `overwrite: "auto"` — no stuck zoom
- `resetCamera()` at end of confirm beat
- Pointer inside `#camera` so clicks land on notification controls during zoom
- Status pill shows "waiting for you" while shell is blocked
