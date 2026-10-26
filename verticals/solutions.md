---
industry: gaming
region: Global
updated: 2026-10-26
---

# Verticales de partida — Gaming AI

> Plataformas open source reales, customizables con una capa AI encima.
> Modelo: partir de algo funcional y robusto, añadir agentes encima.
> Última actualización: **2026-10-26** (pass previo: 2026-07-02).
>
> **Todas las licencias de esta página se releyeron primera mano el 2026-10-26.**
> El pass anterior publicaba dos de ellas mal; las correcciones están marcadas 🔴.

## Stack recomendado: Godot + Nakama + MCP

La combinación más potente disponible en open source hoy para juegos online, con
la capa MCP que no existía en el pass anterior:

```
Godot (MIT) — motor del juego
    + LimboAI (MIT) / Beehave (MIT)      — behavior trees de NPC
    + godot_rl_agents (MIT)              — entrenamiento RL / QA bots
    + godot-ai (MIT, 3.2k★)              — MCP server: 46 tools, 120+ ops
    ↕ nakama-godot SDK (Apache-2.0)
Nakama (Apache-2.0, 13.6k★) — backend multiplayer
    + hooks Go/TypeScript/Lua            — lógica server-side con AI
    + Open Match (Apache-2.0)            — matchmaking enchufable
    + PostHog (MIT)                      — analytics de jugador
    + Pterodactyl (MIT)                  — panel de ops de los game servers
```

**Alternativa más permisiva y más chica:** reemplazar Nakama por **Talo**
(`TaloDev/backend`, MIT). Trae auth, stats, leaderboards, saves, event tracking,
exports CSV, canales realtime, live config, presencia y relaciones de jugador, con
integraciones a Steamworks / Google Play Games / Apple Game Center y paquetes
cliente de Godot y Unity. MIT completo, sin tier cerrado.

---

## Plataformas base completas

| Plataforma | Licencia (verificada 2026-10-26) | URL | Stack | Caso de uso |
|------------|----------------------------------|-----|-------|-------------|
| **Godot Engine** | MIT (`LICENSE.txt`, 1149 B) | [godotengine/godot](https://github.com/godotengine/godot) | C++/GDScript/C# | Engine 2D/3D completo. El ecosistema AI open source más rico: LimboAI, Beehave, godot_rl_agents, godot-ai, godot-mcp. |
| **Open 3D Engine** | Apache-2.0 (`LICENSE.txt`, 1475 B) | [o3de/o3de](https://github.com/o3de/o3de) | C++ | Engine AAA, sponsors AWS/Intel. Enterprise sin royalties de engine. |
| **Nakama** | Apache-2.0 (`LICENSE`, 11358 B) | [heroiclabs/nakama](https://github.com/heroiclabs/nakama) | Go + SDKs | Backend de juego completo: matchmaking, leaderboards, chat, social. 13.6k★. Heroic Cloud managed desde ~$600/mes. |
| 🆕 **Talo** | MIT (`LICENSE`, 1074 B) | [TaloDev/backend](https://github.com/TaloDev/backend) | Node/TS + Docker | Backend self-hostable MIT con imágenes Docker públicas de front y back. La opción cuando el cliente no acepta Apache-2.0 ni un tier managed. |
| **Colyseus** | MIT (`LICENSE`, 1075 B) | [colyseus/colyseus](https://github.com/colyseus/colyseus) | Node.js/TypeScript | Servidor multiplayer web. Browser games y webapps. |
| **MonoGame** | MIT (`LICENSE.txt`, 3882 B) | [MonoGame/MonoGame](https://github.com/MonoGame/MonoGame) | C# | Framework C# cross-platform para equipos .NET. |
| **Stride** | MIT (`LICENSE.md`, 1304 B) | [stride3d/stride](https://github.com/stride3d/stride) | C# | Engine .NET maduro, alternativa a Unity. |
| **Supabase** | Apache-2.0 (`LICENSE`, 11338 B) | [supabase/supabase](https://github.com/supabase/supabase) | PostgreSQL + APIs | BaaS para juegos asíncronos: profiles, inventarios, leaderboards, UGC. `pgvector` para RAG sin servicio externo. |
| 🆕 **Pterodactyl** | MIT (`LICENSE.md`, 1137 B) | [pterodactyl/panel](https://github.com/pterodactyl/panel) | PHP/Laravel + Docker | El panel de game servers open source más usado. Capa de operación: deploy, consola, recursos, usuarios por servidor. |

### 🔴 Plataformas evaluadas y descartadas, con motivo

| Plataforma | Problema verificado |
|------------|--------------------|
| [namazustudios/elements](https://github.com/namazustudios/elements) | **AGPL-3.0** (`LICENSE.txt`, 16726 B). Copyleft de red sobre el servidor de juego: hostearlo para jugadores puede arrastrar la obligación de liberar modificaciones. Fuera de la ruta permisiva. |
| [pelican-dev/panel](https://github.com/pelican-dev/panel) | Fork de Pterodactyl, aparece en comparativas 2026. **Sin ningún archivo de licencia en HEAD** (`LICENSE`, `LICENSE.md`, `LICENSE.txt`, `COPYING` → 404). Sin grant no hay permiso. Usar `pterodactyl/panel`. |
| [grafana/grafana](https://github.com/grafana/grafana) | 🔴 **AGPL-3.0** (`LICENSE`, 34523 B), **no Apache-2.0** como publicaba este KB. Sigue siendo utilizable, pero como servicio expuesto a terceros arrastra obligaciones. Alternativas: Apache Superset (Apache-2.0), Metabase (AGPL-3.0 core + comercial). |

---

## Juegos open source para fork + AI

⚠️ **En gaming, un solo `LICENSE` casi nunca cubre código *y* assets.** Las tres
filas de abajo tienen grants distintos por capa, verificados leyendo la cabecera de
cada archivo. Forkear el repo y quedarse con los assets es el error caro.

| Juego | Licencia del **código** | Licencia de los **assets** | Repo | Por qué útil |
|-------|------------------------|---------------------------|------|--------------|
| **Luanti** (ex-Minetest) | **LGPL-2.1** (`doc/lgpl-2.1.txt`, 26406 B) | **CC-BY-SA 3.0 / 4.0** (`LICENSE.txt`, 8793 B — cubre sólo texturas y sonidos) | [luanti-org/luanti](https://github.com/luanti-org/luanti) | Mundo voxel completo. Base ideal para PCG con AI y NPCs agénticos. El slug canónico cambió: `minetest/minetest` sigue resolviendo pero el proyecto se renombró. |
| **0 A.D.** | **GPL-2.0** | **CC-BY-SA** (`LICENSE.txt`, 3049 B, agregado por directorio) | [0ad/0ad](https://github.com/0ad/0ad) | RTS completo. AI de unidades mejorable con ML. |
| **SuperTuxKart** | **GPL-3.0-or-later** (`COPYING`, 35245 B) | Mezcla **GPL / CC-BY / CC-BY-SA / dominio público** | [supertuxkart/stk-code](https://github.com/supertuxkart/stk-code) | Juego de carreras completo. AI de oponentes mejorable con RL. |

Las tres son copyleft en el código: sirven como **banco de pruebas y demo**, no
como base de un producto propietario. Para eso, Godot + assets propios.

---

## Cómo customizar cada plataforma con AI

### Godot + AI (ruta recomendada para la mayoría de proyectos)

**Opción A — NPC local sin API externa (indie / privacy-first)**
```
Godot Engine (MIT)
    ↓ plugin
LimboAI (MIT) — behavior tree, decisión determinista
    ↓ BTAction con HTTP local
Ollama con Llama 3.1 8B / Gemma 3n
    ↓
NPC con diálogo dinámico, sin latencia de red, sin costo de API
```
🔴 **Corrección de licencia de este pass:** este archivo recomendaba
[code-forge-temple/local-llm-npc](https://github.com/code-forge-temple/local-llm-npc)
**como MIT**. Verificado el 2026-10-26: su `LICENSE` (18655 B) es
**CC-BY-4.0 — "Attribution 4.0 International"**, una licencia de *contenido*
aplicada a software: sin grant de patente y sin distinción fuente/objeto. Sirve
como **referencia de implementación**; no es base para código de cliente. La ruta
construible es LimboAI (MIT) + Ollama directo.
⚠️ **NobodyWho** (Godot AssetLib) es **EUPL-1.2**: copyleft, exige que los
derivados sean open source.

**Opción B — RAG sobre el lore del juego (narrativa coherente)**
```
Godot Engine (MIT)
    ↓ HTTP a FastAPI
LlamaIndex (MIT) + Qdrant (Apache-2.0) o pgvector
    ↓ retrieval de lore, quests, personajes
LLM (Claude Haiku / GPT-4o-mini / modelo local)
    ↓
NPC que "conoce" el universo del juego y no inventa lore
```

**Opción C — MCP en el editor + RL para QA (workflow de estudio)**
```
Godot Engine (MIT)
    ├── godot-ai (MIT, 3.2k★) ──→ Claude Code / Cursor conectado al editor
    │       46 tools, 120+ operaciones: escenas, scripts, signals, animaciones
    └── godot_rl_agents (MIT) ──→ Stable-Baselines3 (MIT) → QA bots
```
Para Unity el equivalente dominante es **[IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP)**
(Apache-2.0, 4.5k★): cualquier método C# se convierte en tool con una línea.

---

### Nakama + AI (backend inteligente)

Nakama expone hooks server-side en Go, TypeScript y Lua. Ahí se interceptan
eventos y se llama a modelos:

```
Nakama server (Apache-2.0)
    ├── Hook: after_match_create
    │   └── modelo ONNX (PyTorch exportado) → scoring de matchmaking
    ├── Hook: after_authenticate
    │   └── clasificador ML → cuenta nueva vs botfarm
    ├── Stream de eventos en partida
    │   └── anomaly detection → anti-cheat conductual
    └── Cron job de Nakama
        └── churn prediction → trigger de re-engagement
    ↓ operación
Pterodactyl (MIT) — panel de los game servers
    ↓ observabilidad
PostHog (MIT) → dashboards  |  evitar Grafana si el dashboard es público (AGPL-3.0)
```

---

### Supabase para juegos asíncronos / persistentes

Ideal para RPGs, idle games, juegos de turnos, social games y plataformas UGC.

```
Supabase (Apache-2.0: PostgreSQL + realtime + auth + storage + pgvector)
    ├── Edge Functions (Deno/TS) → evento de juego dispara llamada al LLM
    ├── pgvector → embeddings de lore/personajes, RAG sin ChromaDB externo
    └── Realtime subscriptions → estado de juego en vivo
```
Ventaja sobre Firebase para gaming: SQL nativo, `pgvector` sin servicio externo,
self-host real y Row Level Security.

---

### O3DE + AWS AI (enterprise / AAA)

```
Open 3D Engine (Apache-2.0)
    ↓ AWS Gem
AWS Bedrock        ← LLM para NPCs y PCG
AWS GameLift       ← multiplayer servers managed
AWS Comprehend     ← toxicidad en chat
AWS Rekognition    ← moderación de UGC
```
Perfil: estudio mid-size/AAA que escala con AWS sin royalties de engine. O3DE es
Apache-2.0: la migración de cloud queda abierta.

---

### 🆕 World model sobre un engine existente (prototipado visual)

La arquitectura de Matrix-Game 4.0 **no reemplaza el engine**: lo mantiene.

```
Engine ejecutable (Godot / Unity)  ← lógica, física, input: sigue siendo tuyo
    ↓ frames low-poly RGB + prompt de texto
Matrix-Game 4.0 (Apache-2.0 en Matrix-Game-4/)
    ↓ streaming neural rendering, 67 FPS en RTX 5080 @864×480
Salida visual de alta fidelidad sin pipeline de arte
```
⚠️ El grant de los **pesos** (Hugging Face) **no se pudo verificar** en este pass:
`huggingface.co` no resuelve DNS aquí. Revisar antes de cualquier uso comercial.

---

## Tabla fit por caso de uso

| Caso de uso | Plataforma base | Capa AI | Esfuerzo |
|-------------|----------------|---------|----------|
| NPC con LLM local | Godot | LimboAI + Ollama | 2-3 semanas |
| NPC con memoria persistente | Godot | patrón generative_agents + LLM | 3-4 semanas |
| Backend multiplayer inteligente | Nakama o Talo | hooks ONNX + PostHog | 3-4 semanas |
| Backend social/persistente + AI | Supabase | Edge Functions + pgvector + LLM | 2-3 semanas |
| QA automatizado con RL | Godot + godot_rl_agents | SB3 + PPO | 2-4 semanas |
| **Evaluación de agentes que juegan** | cualquier engine | **GamingAgent / LMGame Bench (MIT)** | **1-2 semanas** |
| AI-assisted game dev (Godot) | Godot + godot-ai | Claude Code vía MCP | 1 día de setup |
| AI-assisted game dev (Unity) | Unity + Unity-MCP | Claude Code vía MCP | 1 día de setup |
| **Marcado de contenido AI (UE, Art. 50)** | catálogo existente | pipeline de metadata + C2PA | **3-5 semanas** |
| Juego AAA con AI cloud | O3DE | AWS Bedrock / GameLift | meses (enterprise) |
| **Prototipado con world model** | Godot/Unity + Matrix-Game 4.0 | rendering neural | 4-6 semanas |

---
*Licencias verificadas el 2026-10-26 leyendo cada archivo de grant en
`raw.githubusercontent.com/<slug>/HEAD/<archivo>`, con bytes anotados y control
negativo. Correcciones de este pass: `local-llm-npc` (MIT → CC-BY-4.0) y
`grafana` (Apache-2.0 → AGPL-3.0). Ver `agents/top.md` para el detalle del método
y de los grants bloqueantes.*
