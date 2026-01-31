---
name: spotify-player
description: Terminal Spotify playback/search via spogo (preferred) or spotify_player.
homepage: https://www.spotify.com
metadata: {"openclaw":{"emoji":"🎵","requires":{"anyBins":["spogo","spotify_player"]},"install":[{"id":"brew","kind":"brew","formula":"spogo","tap":"steipete/tap","bins":["spogo"],"label":"Install spogo (brew)"},{"id":"brew","kind":"brew","formula":"spotify_player","bins":["spotify_player"],"label":"Install spotify_player (brew)"}]}}
---

# spogo / spotify_player

Use `spogo` **(preferred)** for Spotify playback/search. Fall back to `spotify_player` if needed.

Requirements
- Spotify Premium account.
- Either `spogo` or `spotify_player` installed.

spogo setup
- Import cookies: `spogo auth import --browser chrome`

Command discovery (important)
- Run `spogo --help` and `spogo <command> --help` to confirm subcommands/flags. Versions vary.
- For parsing, prefer structured/clean output if your version supports it: `--json`, `--plain`, `--no-color`.

Command reference (verify with `spogo --help`)
Playback & transport (if supported)
- `spogo play [<uri-or-id>] [--type track|album|artist|playlist|show|episode]`
- `spogo pause`, `spogo next`, `spogo prev`
- `spogo seek <seconds>` (some builds accept relative offsets like `+30`; verify)
- `spogo volume <level>` (0-100 or relative; verify)
- `spogo shuffle`, `spogo repeat`
Queue management (if supported)
- `spogo queue add <uri>`
- `spogo queue show`
Devices & output (if supported)
- `spogo device list`
- `spogo device set <name-or-id>`
- `spogo <command> --device <name-or-id>` (global flag if present)
Search (if supported)
- `spogo search <type> <query> [--limit N]`
- Types often include: track, album, artist, playlist, show, episode
Library & playlists (if supported)
- `spogo library tracks|albums|artists|playlists`
- `spogo playlist create <name>`
- `spogo playlist add <playlist-uri-or-name> <track-uri>`
- `spogo playlist remove <playlist> <uri>`
- `spogo playlist tracks <playlist>`
Metadata (if supported)
- `spogo track info <id>`, `spogo album info <id>`, `spogo artist info <id>`
Status & output (if supported)
- `spogo status [--json|--plain|--no-color]`
Global flags (if supported)
- `--engine connect|web|auto`
- `--market <country>`
- `--language <lang>`
- `--profile <name>`

Agent playbooks (chain 2-4 commands)
- "Add 5 chill songs after this one": `spogo status --json` -> `spogo search track "chill lo-fi" --limit 5` -> `spogo queue add <uri>` (repeat) -> `spogo queue show`
- "Play this artist on my TV": `spogo search artist "Artist Name" --limit 1` -> `spogo device list` -> `spogo device set "<tv>"` -> `spogo play <artist-uri> --type artist`
- "Volume to 42%, shuffle on, skip if over 3 minutes in": `spogo status --json` -> `spogo volume 42` -> `spogo shuffle` -> `spogo next` (if status shows progress > 180s)
- "What's similar to this track?": `spogo status --json` -> `spogo track info <id>` -> `spogo search track "<related artist/genre>" --limit 5`
- "Hand off to my phone when I leave the room": `spogo device list` -> match device name contains "phone" -> `spogo device set "<name|id>"` (or use `--device` on a follow-up command)
- "Add 5 upbeat tracks after this one for my run": `spogo status --json` -> `spogo track info <id>` -> `spogo search track "upbeat workout" --limit 5` -> `spogo queue add <uri>` (repeat)
- "Continue this vibe / more like this": `spogo status --json` -> `spogo track info <id>` -> `spogo search track "<artist|genre|album>" --limit 5` -> `spogo queue add <uri>` (repeat) -> `spogo queue show`

spotify_player commands (fallback)
- Search: `spotify_player search "query"`
- Playback: `spotify_player playback play|pause|next|previous`
- Connect device: `spotify_player connect`
- Like track: `spotify_player like`

Notes
- Config folder: `~/.config/spotify-player` (e.g., `app.toml`).
- For Spotify Connect integration, set a user `client_id` in config.
- TUI shortcuts are available via `?` in the app.
