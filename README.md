# Next Whistle Stream Deck

A Stream Deck profile and supporting shell scripts for running a live roller derby broadcast. This is the setup used at Texas Roller Girls (TXRG) bouts — it may work for you as-is, or serve as a starting point to customize for your own rig.

**Requires [Next Whistle Streaming Companion](https://github.com/autumnix/next-whistle-streaming-companion).** Every button on the deck calls an HTTP API exposed by the companion app running locally. The companion handles all the OBS control, PTZ cameras, and scoreboard integration — the Stream Deck is purely a trigger surface.

---

## Hardware

The profile is designed for the **Stream Deck XL** (32 buttons). It can be adapted for smaller Stream Deck models by dropping or reorganizing buttons, but the XL layout is the reference.

---

## What's in here

```
src/
├── profiles/
│   └── NWS Companion (Mac).streamDeckProfile   # importable Stream Deck profile
└── sh/
    ├── save-and-arm.sh          # save replay buffer and arm latest clip
    ├── jam-reset.sh             # cut to cam 1, recall PTZ presets
    ├── jam-reset-and-play.sh    # roll armed replay, then reset
    ├── go-cam1.sh               # cut to camera 1
    ├── go-cam2.sh               # cut to camera 2
    ├── cam1-preset-{1,2,3}.sh   # recall a PTZ preset on cam 1
    ├── cam2-preset-{1,2,3}.sh   # recall a PTZ preset on cam 2
    ├── show-jammer-stats.sh     # show jammer stat overlay (15s)
    ├── show-penalties.sh        # show penalty overlay
    ├── show-rosters.sh          # show roster overlay
    ├── hide-overlays.sh         # hide all overlays
    └── obs-ping.sh              # health check (useful as a title button)
```

Each shell script is a single `curl` call to `http://127.0.0.1:8787` — the default port the companion app listens on. If you change the companion's port, update the scripts to match.

---

## Getting started

1. **Set up and start the companion app** — see [next-whistle-streaming-companion](https://github.com/autumnix/next-whistle-streaming-companion) for install and config instructions. The companion must be running before the deck buttons do anything useful.
2. **Import the profile** — open the Stream Deck desktop app, go to Profiles in preferences, and import `src/profiles/NWS Companion (Mac).streamDeckProfile`. The `.streamDeckProfile` file is not usable directly; it must be imported through the app.
3. **Install the shell scripts** — copy the scripts from `src/sh/` to a stable location on your machine (e.g. `~/bin/nwsc/`), and update any Stream Deck "Open" or "System: Open" actions in the profile to point to those paths.
4. **Adjust for your setup** — scene names, overlay source names, and PTZ presets are baked into the companion's `config.yaml`, not the scripts. If your config differs from the defaults, update it there.

---

## Customizing

The profile is a starting point, not a fixed standard. Common things to change:

- **Fewer cameras** — remove or repurpose the cam 2 buttons and preset rows.
- **More cameras** — add a script per camera following the same `go-cam{N}.sh` and `cam{N}-preset-{N}.sh` pattern; the companion's PTZ API accepts any configured camera ID, so scaling up is straightforward.
- **Different overlays** — the overlay scripts call `/overlay/{group}/show?source={source}` — swap the group and source names to match your OBS scene structure.
- **Smaller deck** — move the highest-priority buttons (save-and-arm, jam-reset, jam-reset-and-play) to a condensed layout and put the rest in a folder.

---

## Status

Personal production setup for TXRG events. Works reliably in that context; adapt to taste for other leagues or rigs.
