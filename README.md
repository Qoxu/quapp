# Quapp

A native-speed WhatsApp desktop client with an Apple-grade interface — a ground-up
reimagining of [ZapFast](https://github.com/crmne/zapfast). Built with React,
Tailwind and Electron; designed to plug into a `whatsapp-rust` bridge daemon.

![stack](https://img.shields.io/badge/stack-React%2019%20·%20Vite%208%20·%20Tailwind%204%20·%20Electron-blue)

## Features

- **Instant UI** — virtualized chat & message lists, normalized state with
  per-row subscriptions, GPU-only spring motion. Nothing blocks.
- **Command palette** (`⌘K`) — jump to chats, open new conversations from
  contacts, search every message, run actions.
- **Apple-style themes** — light/dark/system plus 6 accent tints that recolor
  the whole app; 4 chat wallpapers. Settings persist.
- **Full messaging surface** — replies, reactions, edits, deletes-for-everyone,
  forwards, polls, voice notes with waveforms, docs, locations, stickers,
  link previews, selection mode.
- **Drafts** per chat (shown live as `Draft:` in the list), unread divider,
  jump-to-latest pill, starred messages, hover quick-actions.
- **Formatting shortcuts** — `⌘B` bold, `⌘I` italic, `⌘E` code, `⇧⌘X` strike.

## Architecture

```
UI (React)  →  zustand store  →  ClientAdapter  →  backend
                                    ├─ DemoAdapter   (local simulation, default)
                                    └─ WsAdapter     (?bridge=ws://… → quappd)
```

The UI never sees a protobuf — `src/bridge/types.ts` is the whole contract.
Swap the adapter and the same UI runs on the real protocol.

## Run it

```bash
pnpm install
pnpm dev            # web dev server
pnpm desktop        # build + open the Electron window
```

## Build the Windows app

```bash
pnpm dist           # produces release/Quapp-0.1.0-x64.exe (installer)
                    # and Quapp-0.1.0-portable.exe (no install needed)
```

## Connect a real backend

Point the app at a `quappd` bridge daemon over WebSocket:

```
quapp.exe --args --bridge=ws://127.0.0.1:8765
```

or in the browser: `http://localhost:5199/?bridge=ws://127.0.0.1:8765`.
The protocol is one JSON frame per message — see `src/bridge/ws.ts`.

## Privacy

Demo data is fully local and synthetic. No telemetry, no account system,
messages never leave the machine.
