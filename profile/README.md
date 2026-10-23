<img width="800" height="250" alt="logo-classic" src="https://github.com/user-attachments/assets/03a16a04-6c4d-4eda-bb9b-14166802ae78" />


# Relics of Return

Relics of Return is a **RuneScape Classic** private server playable in the web browser or through a desktop launcher. The project comprises the game client, the game server, a website providing accounts and hiscores, a wiki, the World and Model Editors, and the tooling required to operate the service. It is built on the open-source [2003Scape](https://github.com/2003scape) RuneScape Classic projects.

The project preserves the look and feel of Classic while adding the elements of Old School RuneScape that players missed:

- **19 skills**, including Construction, with player-owned houses.
- **Old School interfaces**: a bank with tabs, placeholders and notes; prayer and spell grids; a character summary; chatbox dialogues; and travel maps for the ships.
- **Hiscores with records**: the first players to reach level 99 and 200M experience in each skill, together with a collection log of every collectible item.
- **Animated skill actions**: woodcutting, mining, fishing and firemaking each have a dedicated animation, and every skill has a cape.
- **Events and content**: the Evil Tree, achievement diaries, ironman accounts, and reworked towns (Varrock, Falador, Draynor, Al Kharid and Yanille).
- **An optional HD renderer**: dynamic lighting and shadows, photographic textures, water and reflections, day/night cycles, seasons, weather, clouds and mist. The classic graphics can be restored with a single keypress.
- **In-client plugins**, several of which are ported from RuneLite's Plugin Hub.

## Architecture

The following diagram illustrates how the components of the project interconnect:

```text
   browser/launcher                     website
 ┌──────────────────┐             ┌──────────────────┐
 │ rsc-client       │             │ rsc-web (Next.js)│
 │ rsc-tauri-client │             └────────┬─────────┘        
 └────────┬─────────┘                      │ HTTP            ┌──────────────────────────┐
          │ WebSocket                ┌─────▼─────┐           │ rsc-manager              │
   ┌──────▼──────┐   JSON / TCP      │  rsc-www  │           │ starts, watches and      │
   │ rsc-server  ├──────────────┐    └─────┬─────┘           │ stops all of it          │
   │ (a world)   │              │          │ JSON / TCP      └──────────────────────────┘
   └──────┬──────┘        ┌─────▼──────────▼─────┐           ┌──────────────────────────┐
          │ reads         │   rsc-data-server    │           │ rsc-editor               │
   ┌──────▼──────┐        │ accounts, SQLite     │           │ edits the world and its  │
   │  rsc-data   │        └──────────────────────┘           │ models                   │
   └─────────────┘                                           └──────────────────────────┘
```

## Repositories

Each part of the game is released through three stages, **dev → beta → live**, and each stage is maintained in its own repository. For example, `rsc-server-dev` is where code is written, `rsc-server-beta` is what testers play, and `rsc-server` is the production server. The same arrangement applies to the client, the game data and the data server.

### Game

| Repository | Description |
| --- | --- |
| **rsc-client** | RuneScape Classic client, ported from Java to JavaScript. It runs in the browser over WebSockets and provides fixed and resizable layouts, the HD renderer, in-client plugins and Discord Rich Presence. |
| **rsc-server** | Game server: combat, skills, quests, NPCs, trading, banking, the Grand Exchange, Construction, events, staff tools, and the scripting API for the botting world. It runs as one or more worlds. |
| **rsc-data** | Game data in JSON format: items, NPCs, objects, drop tables, spawns, shops, skills, quests, the collection log, and the landscape. |
| **rsc-data-server** | Login and persistence server (Jagex's "loginserver"). It stores accounts and players in SQLite and manages friends, hiscores, records and the collection log for each world. |

### Website

| Repository | Description |
| --- | --- |
| **rsc-web** | Website, built with Next.js, TypeScript and Tailwind: news; hiscores with records and the collection log; the account page; the play page with each world's ping; the roadmap; and staff tools. |
| **rsc-www** | Backend for the website: accounts, world status, hiscores, news, launcher downloads, and the Discord integration that admits beta testers. |
| **rsc-wiki** | Wiki, built with Next.js: every item, monster, drop table, shop, skill, quest, spell and prayer, generated from the game data, together with guides written by the team. |

### Tools and clients

| Repository | Description |
| --- | --- |
| **rsc-tauri-client** | Desktop launcher, built with Tauri. It updates itself and the game, stores accounts in the system keychain, displays Discord Rich Presence, and provides a "Join the beta" switch for testers. |
| **rsc-editor** | World and Model Editors: paint the ground, walls, roofs, scenery, spawns and lighting of the world, and edit scenery models, drawn live by the game's own renderer. |
| **rsc-manager** | Starts, monitors and stops every service, restarts any that crash, and serves a dashboard for logs, players online, broadcasts, backups and scheduled restarts. |

## Game Worlds

| World | Purpose |
| --- | --- |
| World 1 | Primary |
| World 2 | Botting (scripting API; separate from the primary world) |
| World 3 | Beta (invitation only) |

An account is valid on every world, and each world maintains its own character.

## Acknowledgements and License

- **[2003Scape](https://github.com/2003scape)**: client, server and data projects on which this project is built.
- **[mudclient204](https://github.com/2003scape/mudclient204)**: original client revision of which rsc-client is a JavaScript port.
- **[117HD](https://github.com/117HD/RLHD)**: its ideas inspired client's HD renderer.
- **[RuneLite](https://runelite.net/)**: client's interface, layout and several of its plugins are modelled on RuneLite and its Plugin Hub.
- **[OSRS World](https://osrs.world/)**: interface of the World Editor is based on that project.
- **[RSCPlus](https://github.com/RSCPlus/rscplus)**: client's overlays (hit, prayer and fatigue bars) follow its style.
- **[OpenRSC](https://gitlab.com/openrsc/openrsc)**: a reference for authentic behaviour when writing quests and scripts.
- **[RSCGo](https://github.com/spkaeros/RSCGo)**: another RuneScape Classic server with which the client is designed to work.

Several in-client plugins are ports or adaptations of RuneLite plugin projects:

- **[osrs-tcg](https://github.com/Azderi/osrs-tcg)**: card-collecting plugin on which the client's TCG is based.
- **[resource-packs](https://github.com/melkypie/resource-packs)**: interface packs that restyle the prayers, spells, quests tab and frame.
- **[compact-orbs](https://github.com/its-cue/compact-orbs)**: compact orb arrangements offered by the interface layout.
- **[storage-interactions](https://github.com/Moltenrain/storage-interactions)**: the quantity and note overlay for storage interfaces.
- **[Expanded-bank](https://github.com/VolkezXO/Expanded-bank)**: expanded bank interface.
- **[spirit-tree-map](https://github.com/MJHylkema/spirit-tree-map)**: interactive travel maps for the ships.
- **[vicious-bars](https://github.com/viciousliqx/vicious-bars)**: status bars that replace the orbs.

photographic textures given to scenery, roofs and walls in the optional HD renderer come from **[Poly Haven](https://polyhaven.com/)**, a library of CC0 HDRIs, textures and 3D models.

RuneScape is a trademark of Jagex Ltd. This project is not affiliated with or endorsed by Jagex.

code is licensed under AGPL-3.0-or-later. See each repository for details.
