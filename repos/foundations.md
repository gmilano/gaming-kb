---
industry: gaming
region: Global
updated: 2026-10-26
---

# 🏗️ Repos fundacionales — Gaming

> Bases sobre las cuales construir. Licencia verificada primera mano.
> Última actualización: **2026-10-26** (pass previo: 2026-07-02).
>
> Toda licencia de esta página se leyó el 2026-10-26 desde
> `raw.githubusercontent.com/<slug>/HEAD/<archivo>`, con el nombre de archivo y
> los bytes anotados. Donde el archivo es **agregado** (cubre código y assets de
> terceros en un solo texto) se separan las dos capas, porque en gaming casi
> nunca coinciden.

## Game Engines

| Repo | Licencia (archivo, bytes) | Descripción | Stars |
|------|--------------------------|-------------|-------|
| [godotengine/godot](https://github.com/godotengine/godot) | **MIT** (`LICENSE.txt`, 1149) | Motor completo 2D/3D. Ecosistema AI más rico: LimboAI, Beehave, godot_rl_agents, godot-ai, godot-mcp. | 97k* |
| [o3de/o3de](https://github.com/o3de/o3de) | **Apache-2.0** (`LICENSE.txt`, 1475) | Open 3D Engine (ex-Amazon Lumberyard). AAA-grade. Opción enterprise, sin royalties. | 7.8k* |
| [MonoGame/MonoGame](https://github.com/MonoGame/MonoGame) | **MIT** (`LICENSE.txt`, 3882) | Framework C# cross-platform. Para equipos .NET añadiendo AI. | 11k* |
| [stride3d/stride](https://github.com/stride3d/stride) | **MIT** (`LICENSE.md`, 1304) | Engine C# cross-platform (ex-Xenko). Alternativa .NET madura a Unity. | 7.7k* |
| [love2d/love](https://github.com/love2d/love) | **zlib** para LÖVE — ⚠️ `license.txt` (74927) es **agregado** | LÖVE 2D: Lua, ligero, ideal para prototipos rápidos con AI. El archivo agrega ENet (MIT), FreeType, GLAD y texto LGPL/GPL de dependencias: el grant de LÖVE es zlib, el del binario distribuido depende de qué se enlace. | 4.5k* |

## Multiplayer Backends

| Repo | Licencia (archivo, bytes) | Descripción | Stars |
|------|--------------------------|-------------|-------|
| [heroiclabs/nakama](https://github.com/heroiclabs/nakama) | **Apache-2.0** (`LICENSE`, 11358) | Backend open source más completo. SDK oficial Godot/Unity/Unreal. Extensible en Lua/TS/Go: ahí se inyecta la capa AI. Social, matchmaking, leaderboards. | **13.6k** |
| [heroiclabs/nakama-godot](https://github.com/heroiclabs/nakama-godot) | **Apache-2.0** (`LICENSE`, 11359) | SDK oficial de Nakama para Godot. | — |
| [googleforgames/open-match](https://github.com/googleforgames/open-match) | **Apache-2.0** (`LICENSE`, 11345) | Framework de matchmaking de Google. Los algoritmos son plug-in: entra un modelo propio. | — |
| [colyseus/colyseus](https://github.com/colyseus/colyseus) | **MIT** (`LICENSE`, 1075) | Server multijugador Node.js/TypeScript. Ideal para web games. | 6.2k* |
| 🆕 [TaloDev/backend](https://github.com/TaloDev/backend) | **MIT** (`LICENSE`, 1074) | Backend self-hostable con auth, stats, leaderboards, saves, event tracking, exports CSV, canales realtime, live config, presencia y relaciones de jugador. Integra Steamworks, Google Play Games y Apple Game Center. Paquetes cliente Godot y Unity. **MIT completo, sin tier cerrado** — la diferencia con Nakama es licencia más permisiva y alcance menor. | 104 |
| [supabase/supabase](https://github.com/supabase/supabase) | **Apache-2.0** (`LICENSE`, 11338) | BaaS para juegos asíncronos: profiles, inventarios, leaderboards, UGC. `pgvector` para RAG sin servicio externo. | 80k* |

## RL y entornos de entrenamiento

| Repo | Licencia (archivo, bytes) | Descripción | Stars |
|------|--------------------------|-------------|-------|
| [Farama-Foundation/Gymnasium](https://github.com/Farama-Foundation/Gymnasium) | **MIT** (`LICENSE`, 1104) | API estándar de entornos RL. Sucesor mantenido de `openai/gym`. | 12.1k* |
| [Farama-Foundation/PettingZoo](https://github.com/Farama-Foundation/PettingZoo) | **MIT** (`LICENSE`, 1073) | Equivalente multi-agente de Gymnasium. Para NPCs que compiten o cooperan. | 3.5k* |
| [DLR-RM/stable-baselines3](https://github.com/DLR-RM/stable-baselines3) | **MIT** (`LICENSE`, 1075) | PPO, SAC, A2C, DQN en PyTorch. El algoritmo por defecto para QA bots. | 13.5k* |
| [Unity-Technologies/ml-agents](https://github.com/Unity-Technologies/ml-agents) | **Apache-2.0** (`LICENSE.md`, 572) | Toolkit RL oficial de Unity. | 19.5k* |
| [edbeeching/godot_rl_agents](https://github.com/edbeeching/godot_rl_agents) | **MIT** (`LICENSE`, 1071) | Puente Godot ↔ Python para 4 frameworks RL. | 900+* |

## Arquitecturas de agente de referencia

| Repo | Licencia (archivo, bytes) | Descripción | Stars |
|------|--------------------------|-------------|-------|
| [joonspk-research/generative_agents](https://github.com/joonspk-research/generative_agents) | **Apache-2.0** (`LICENSE`, 11335) | El paper de Stanford convertido en código. Memory stream + reflection + planning: sigue siendo la arquitectura de referencia para NPC con memoria. | 21.7k* |
| [google-deepmind/concordia](https://github.com/google-deepmind/concordia) | **Apache-2.0** (`LICENSE`, 11359) | Simulación social generativa de DeepMind. Para poblaciones de NPC, no un NPC. | 1.5k* |
| 🆕 [lmgame-org/GamingAgent](https://github.com/lmgame-org/GamingAgent) | **MIT** (`LICENSE`, 1065) | ICLR 2026. Harness + benchmark de agentes que juegan. Instrumento de medición, no sólo librería. | 998 |

## Pathfinding

| Repo | Licencia (archivo, bytes) | Descripción | Stars |
|------|--------------------------|-------------|-------|
| [recastnavigation/recastnavigation](https://github.com/recastnavigation/recastnavigation) | **zlib** (`License.txt` — ojo la capitalización) | Estándar de industria navmesh. Ya embebido en Godot, Unity, UE. Combinable con agentes RL. | 7.8k* |
| [Mugen87/yuka](https://github.com/Mugen87/yuka) | **MIT** (`LICENSE`, 1072) | Game AI primitives JS: steering, pathfinding, FSM. | 1.4k* |

## Analytics, observabilidad y ops

| Repo | Licencia (archivo, bytes) | Descripción | Stars |
|------|--------------------------|-------------|-------|
| [PostHog/posthog](https://github.com/PostHog/posthog) | **MIT** (`LICENSE`, 1563) | Product analytics self-hosted. Base de player behavior analysis y churn. | 23k* |
| 🆕 [pterodactyl/panel](https://github.com/pterodactyl/panel) | **MIT** (`LICENSE.md`, 1137) | El panel de game servers open source más usado. Capa de ops que faltaba en este KB: el backend de juego no se opera a mano. | — |
| [grafana/grafana](https://github.com/grafana/grafana) | 🔴 **AGPL-3.0** (`LICENSE`, 34523) — **no Apache-2.0** | Dashboards de player analytics. **Corrección de este pass:** este archivo publicaba Apache-2.0. AGPL-3.0 es copyleft de red: exponer el dashboard a terceros puede obligar a liberar modificaciones. Para un cliente que no quiere esa obligación: Apache Superset (Apache-2.0) o Metabase (AGPL-3.0 core / licencia comercial). | 67k* |

## 🔴 Fila retirada de esta página

| Repo | Motivo |
|------|--------|
| [pelican-dev/panel](https://github.com/pelican-dev/panel) | Fork de Pterodactyl que aparece en comparativas 2026 como alternativa. **Vivo pero sin ningún archivo de licencia en HEAD** (`LICENSE`, `LICENSE.md`, `LICENSE.txt`, `COPYING` → 404). No se recomienda; usar `pterodactyl/panel`. |
| [namazustudios/elements](https://github.com/namazustudios/elements) | Evaluado como backend alternativo. Es **AGPL-3.0** (`LICENSE.txt`, 16726): copyleft de red sobre el servidor de juego. Fuera de la ruta permisiva. |

---
## Cómo leer la columna de estrellas

Sin asterisco = releído el 2026-10-26. Con `*` = heredado del pass 2026-07-02 y
**no reverificado**. Los releídos que cambiaron materialmente: `nakama`
12.8k → 13.6k. El descubrimiento por `api.github.com/search` está bloqueado en
este entorno, así que no hay forma de refrescar todas las ★ en un pass.

*Ver también: `verticals/solutions.md` para plataformas verticales completas y
`agents/top.md` para los grants que bloquean uso comercial.*
