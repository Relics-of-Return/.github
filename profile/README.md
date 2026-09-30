<p align="center">
  <img src="https://raw.githubusercontent.com/Relics-of-Return/rsc-manager/main/public/logo.png" alt="Relics of Return" width="96">
</p>

# Relics of Return

A browser-based **RuneScape Classic** private server: a game client that runs in any browser, a game server, a website with hiscores and accounts, and the tools to run it all. It builds on the open-source [2003Scape](https://github.com/2003scape) RSC projects.

Play in the browser with multiplayer, 18 skills, NPC and player combat, free-to-play quests, trading, a Grand Exchange, and a botting world for scripts.

## How the pieces fit

```
   browser / launcher                     website
 ┌──────────────────┐             ┌──────────────────┐
 │ rsc-client       │             │ rsc-web (Next.js)│
 │ rsc-tauri-client │             └────────┬─────────┘
 └────────┬─────────┘                      │ HTTP
          │ WebSocket                ┌─────▼─────┐
   ┌──────▼──────┐   JSON / TCP      │  rsc-www  │
   │ rsc-server  ├──────────────┐    └─────┬─────┘
   │ (a world)   │              │          │ JSON / TCP
   └──────┬──────┘        ┌─────▼──────────▼─────┐
          │ reads         │   rsc-data-server    │
   ┌──────▼──────┐        │ accounts, SQLite     │
   │  rsc-data   │        └──────────────────────┘
   └─────────────┘
              rsc-manager starts, watches and stops all of it
```

## Repositories

### Game

| Repository | What it is |
| --- | --- |
| **rsc-client** | The RuneScape Classic client ported from Java to JavaScript. Runs in the browser over WebSockets, with fixed, resizable and classic layouts, interface skins, seasonal login themes and an equipment tab. |
| **rsc-server** | The game server: packet handling, combat, skills, quests, NPCs, trading, banking, the Grand Exchange, staff tools and the scripting API for the botting world. Runs as one or more worlds. |
| **rsc-data** | Shared game data as JSON: drop tables, spawns, shops, skill data, quests and landscape. Read by the server. |
| **rsc-data-server** | The login and persistence server (Jagex's "loginserver"). Stores accounts and players in SQLite and coordinates worlds, friends and hiscores over JSON on TCP. |

### Website

| Repository | What it is |
| --- | --- |
| **rsc-web** | The website, built with Next.js, TypeScript and Tailwind: news, hiscores, account management, the play page, a public status page, and staff-only tools such as the report dashboard and the world map editor. |
| **rsc-www** | The backend the website talks to: account registration, world status, hiscores and news, served over a small HTTP API on top of the data server. |

### Tools and clients

| Repository | What it is |
| --- | --- |
| **rsc-manager** | Starts, watches and stops every service, restarts crashes, and serves a dashboard for logs, players online, broadcasts and scheduled restarts. |
| **rsc-tauri-client** | A desktop launcher and client, built with Tauri, that runs the game without a browser. It has self-updating, keychain-stored accounts, Discord Rich Presence and sandboxed plugins. |

### Design and docs

| Folder | What it holds |
| --- | --- |
| **branding** | The logo, emblem and Discord art (Aseprite sources and exports). |
| **documentation** | Design notes and guides: commands, staff ranks, client interface, quest research, the standalone launcher and more. |

## Running it

Everything is started from the project root with one command on Windows:

```
manager.cmd
```

This starts the data servers, the game worlds, the client files, the website API and the front end, then opens the manager dashboard. The website is at `http://localhost:3000`.

Each repository's own README has its setup details.

## Worlds

| World | Purpose |
| --- | --- |
| World 1 | The main world |
| World 2 | A botting world with a scripting API, kept apart from the main world |

## Credits and license

- **[2003Scape](https://github.com/2003scape)**: the client, server and data projects this is built on.
- **[117HD](https://github.com/117HD/RLHD)**: their idea inspired the HD engine rework of the client, with dynamic lighting, shadows, water and reflections, materials and post-processing.
- **[OSRS World](https://osrs.world/)**: the world map editor's interface is based on theirs.
- **[mudclient204](https://github.com/2003scape/mudclient204)**: the original client revision that rsc-client is a JavaScript port of.
- **[RSCGo](https://github.com/spkaeros/RSCGo)**: another RuneScape Classic server the client is built to work with.
- **[RSCPlus](https://github.com/RSCPlus/rscplus)**: the client's overlays (hit, prayer and fatigue bars) follow its style.
- **[OpenRSC](https://gitlab.com/openrsc/openrsc)**: a reference for authentic behaviour when writing quests and scripts.
- **[RuneLite](https://runelite.net/)**: the client's interface and layout take their cues from it.

RuneScape is a trademark of Jagex Ltd; this project is not affiliated with or endorsed by Jagex.

Code is licensed under AGPL-3.0-or-later. See each repository for details.
