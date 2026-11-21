---
industry: gaming
region: Global
updated: 2026-11-21
---

# Verticales de partida — Gaming AI

> Plataformas open source reales, customizables con una capa AI encima.
> Modelo: partir de algo funcional y robusto, añadir agentes encima.
> Última actualización: **2026-11-20** (passes previos: 2026-10-26, 2026-07-02).
>
> **Todas las licencias de esta página se releyeron el 2026-11-20 en dos canales**
> (`gh api search/repositories` para SPDX + `raw.../HEAD/` para el texto). El pass
> de octubre corrigió dos filas y **publicó mal otras cuatro**; esas cuatro se
> corrigen ahora y están marcadas 🔴. El detector que las encontró es el **checksum
> de bytes** del archivo de licencia, documentado en `agents/top.md`.

## Stack recomendado: Godot + Nakama + MCP

La combinación más potente disponible en open source hoy para juegos online, con
la capa MCP que no existía en el pass anterior:

```
Godot (MIT) — motor del juego
    + LimboAI (MIT) / Beehave (MIT)      — behavior trees de NPC
    + godot_rl_agents (MIT)              — entrenamiento RL / QA bots
    + godot-ai (MIT, 3.627★)             — MCP server: 46 tools, 120+ ops
    ↕ nakama-godot SDK (Apache-2.0)
Nakama (Apache-2.0, 13.757★) — backend multiplayer
    + hooks Go/TypeScript/Lua            — lógica server-side con AI
    + Open Match (Apache-2.0)            — matchmaking enchufable
    + PostHog (MIT Expat + carve-out ee/) — analytics de jugador  🔴 ver nota
    + Pterodactyl (MIT)                  — panel de ops (bare-metal/Docker)
    + Agones (Apache-2.0, 7.106★)        — 🆕 ops sobre Kubernetes
```

🔴 **Corrección sobre el diagrama: PostHog no es "MIT" a secas.** Su `LICENSE`
(1563 B) dice *"All content that resides under the `ee/` directory … is licensed
under the license defined in `ee/LICENSE`"* (PostHog Enterprise License, 2176 B);
el resto es MIT Expat. El self-host del core sigue siendo viable y la receta no
cambia, pero **la entrada de inventario sí**, y es el mismo patrón literal que
Onyx en la KB de enterprise. Si el cliente exige "todo MIT" por política, hay que
decir que PostHog es *mixto, ver archivo*, no MIT.

🆕 **La capa de ops estaba incompleta.** `Pterodactyl` es un panel para servidores
en bare-metal/Docker. Para capacidad elástica, lo que el mercado usa es
**[agones-dev/agones](https://github.com/agones-dev/agones)** (Apache-2.0,
7.106★): `Fleet`, `GameServer` y `FleetAutoscaler` como CRDs de Kubernetes.
**Faltaba desde el primer pass de esta KB.** ⚠️ Y su slug cambió: el
`googleforgames/agones` de toda la documentación **ya no es el canónico**.

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
| **Open 3D Engine** | 🔴 **Apache-2.0 *o* MIT a elección** (`LICENSE.txt`, 1475 B — es un **puntero**, no el texto de Apache) | [o3de/o3de](https://github.com/o3de/o3de) | C++ | Engine AAA, sponsors AWS/Intel. **Más permisivo de lo que decíamos** (el cliente puede elegir MIT), **pero el propio `LICENSE.txt` advierte componentes copyleft, incluido Qt bajo LGPL-3.0**: "sin royalties" ≠ "sin obligaciones". Distribuir el **editor** modificado exige resolver Qt; distribuir sólo el **runtime**, normalmente no. ⚠️ rama default `development`. 9.894★ |
| **Nakama** | Apache-2.0 (`LICENSE`, 11358 B) | [heroiclabs/nakama](https://github.com/heroiclabs/nakama) | Go + SDKs | Backend de juego completo: matchmaking, leaderboards, chat, social. 13.763★. Heroic Cloud managed desde ~$600/mes. 🆕 **Límite del open core, verificado 2026-11-21:** el **servidor** es Apache-2.0, pero **Satori (live-ops) y el clustering enterprise son productos cerrados**. ⇒ un stack de live-ops **completamente abierto no existe en gaming**: hay que construir la capa de experimentación/segmentación o aceptar un componente propietario. Decirlo en la propuesta, no al descubrirlo en la integración. |
| 🆕 **Talo** | MIT (`LICENSE`, 1074 B) | [TaloDev/backend](https://github.com/TaloDev/backend) | Node/TS + Docker | Backend self-hostable MIT con imágenes Docker públicas de front y back. La opción cuando el cliente no acepta Apache-2.0 ni un tier managed. |
| **Colyseus** | MIT (`LICENSE`, 1075 B) | [colyseus/colyseus](https://github.com/colyseus/colyseus) | Node.js/TypeScript | Servidor multiplayer web. Browser games y webapps. |
| **MonoGame** | 🔴 **Ms-PL** (`LICENSE.txt`, 3882 B) — **no MIT** | [MonoGame/MonoGame](https://github.com/MonoGame/MonoGame) | C# | Framework C# cross-platform para equipos .NET. **Corrección de este pass.** Ms-PL §3(D): cualquier porción distribuida **en código fuente sólo puede ir bajo Ms-PL** — un entregable de fuente a cliente **no se puede relicenciar** a MIT/Apache. Para un juego compilado es indistinto. ⚠️ rama default `develop`. 14.663★ |
| **Stride** | MIT (`LICENSE.md`, 1304 B) | [stride3d/stride](https://github.com/stride3d/stride) | C# | Engine .NET maduro, alternativa a Unity. |
| **Supabase** | Apache-2.0 (`LICENSE`, 11338 B) | [supabase/supabase](https://github.com/supabase/supabase) | PostgreSQL + APIs | BaaS para juegos asíncronos: profiles, inventarios, leaderboards, UGC. `pgvector` para RAG sin servicio externo. 115.201★ |
| 🟢 **Elements** (Namazu) | **MPL-2.0** (`LICENSE.txt`, 16726 B) — **no AGPL-3.0** | [NamazuStudios/elements](https://github.com/NamazuStudios/elements) | Java + REST/WebSockets | **Re-admitido este pass: estaba descartado por error.** Backend self-hosted con OAuth2/OIDC y Custom Elements. MPL-2.0 es copyleft **por archivo**: hostearlo **no** obliga a liberar tu código, sólo las modificaciones a archivos MPL. **Muy chico (22★)**: opción a considerar si el cliente rechaza Apache-2.0 y Talo le queda corto, nunca default frente a Nakama. |
| **Pterodactyl** | MIT (`LICENSE.md`, 1137 B — verificado leyendo; el API dice `NOASSERTION` porque el texto va dentro de un *code fence* de Markdown) | [pterodactyl/panel](https://github.com/pterodactyl/panel) | PHP/Laravel + Docker | Panel de game servers más usado. Deploy, consola, recursos, usuarios por servidor. Bare-metal/Docker. ⚠️ rama default `1.0-develop`. 9.533★ |
| 🆕 **Agones** | **Apache-2.0** | [agones-dev/agones](https://github.com/agones-dev/agones) | Go + Kubernetes CRDs | **La pieza que faltaba.** Orquestación y autoescalado de game servers dedicados sobre K8s (`Fleet`/`GameServer`/`FleetAutoscaler`). Complementa a Pterodactyl, no lo reemplaza: panel para operar hosts, Agones para capacidad elástica. 🔴 renombrado desde `googleforgames/agones`. 7.106★ |

### 🔴 Plataformas evaluadas y descartadas, con motivo

| Plataforma | Problema verificado |
|------------|--------------------|
| 🟢 ~~`namazustudios/elements`~~ | **FILA RETIRADA DE ESTA TABLA — el descarte era incorrecto.** Decía "AGPL-3.0 (`LICENSE.txt`, 16726 B), copyleft de red, fuera de la ruta permisiva". El archivo, con **esos mismos 16726 B**, dice *"Mozilla Public License Version 2.0"*. **AGPL-3.0 son ~34.500 B** — y este mismo KB anotaba 34523 B para la AGPL de Grafana, así que **el dato que refutaba la etiqueta ya estaba escrito al lado**. Movido a la tabla de plataformas base. Nuevo slug: `NamazuStudios/elements`. |
| [pelican-dev/panel](https://github.com/pelican-dev/panel) | 🔴 **Agravado: el repo ya no existe.** En octubre era "vivo pero sin archivo de licencia"; hoy `repo:` → `NOT_FOUND` y `raw` → 404. Usar `pterodactyl/panel`. |
| [grafana/grafana](https://github.com/grafana/grafana) | 🔴 **AGPL-3.0** (`LICENSE`, 34523 B), **no Apache-2.0** como publicaba este KB. Sigue siendo utilizable, pero como servicio expuesto a terceros arrastra obligaciones. Alternativas: Apache Superset (Apache-2.0), Metabase (AGPL-3.0 core + comercial). |

---

## Juegos open source para fork + AI

⚠️ **En gaming, un solo `LICENSE` casi nunca cubre código *y* assets.** Las tres
filas de abajo tienen grants distintos por capa, verificados leyendo la cabecera de
cada archivo. Forkear el repo y quedarse con los assets es el error caro.

| Juego | Licencia del **código** | Licencia de los **assets** | Repo | Por qué útil |
|-------|------------------------|---------------------------|------|--------------|
| **Luanti** (ex-Minetest) | **LGPL-2.1-or-later** (`doc/lgpl-2.1.txt`, 26406 B) | **CC-BY-SA 3.0** (`LICENSE.txt`, 8793 B — cubre sólo texturas y sonidos) | [luanti-org/luanti](https://github.com/luanti-org/luanti) | Mundo voxel completo. Base ideal para PCG con AI y NPCs agénticos. 🔴 **Corrección 2026-11-20:** el pass anterior decía que `minetest/minetest` *"sigue resolviendo"*. Resuelve **por redirect**: `repo:minetest/minetest` → `NOT_FOUND` en la API, y `raw` del slug viejo da 200 en `HEAD`/`master` y **404 en `main`**. El único slug canónico es `luanti-org/luanti` (**13.882★, commit de hoy** — la más activa de las tres). |
| **0 A.D.** | **GPL-2.0-or-later** (🆕 precisado 2026-11-21: `LICENSE.txt` dice *"GPL version 2 (or later)"*, y agrega subdirectorios **MIT** y **Apache-2.0** vendorizados) | **CC-BY-SA** (`LICENSE.txt`, 3049 B, agregado por directorio) | 🔴 [0ad/0ad](https://github.com/0ad/0ad) — **ARCHIVADO** | RTS completo, AI de unidades mejorable con ML, **pero el repo de GitHub está `archived: true` y su propia descripción dice `[DEPRECATED] Git mirror … migrated to gitea.wildfiregames.com on August 20th 2024`.** Último commit 2024-08-17. **El juego está vivo; su presencia en GitHub está muerta.** Para forkear hay que ir al Gitea propio del proyecto, fuera de GitHub. 2.827★ |
| **SuperTuxKart** | **GPL-3.0-or-later** (`COPYING`, 35245 B) | Mezcla **GPL / CC-BY / CC-BY-SA / dominio público** | [supertuxkart/stk-code](https://github.com/supertuxkart/stk-code) | Juego de carreras completo. AI de oponentes mejorable con RL. |

Las tres son copyleft en el código: sirven como **banco de pruebas y demo**, no
como base de un producto propietario. Para eso, Godot + assets propios.

🆕 **Trampa de lectura encontrada el 2026-11-21, y es genérica.** El
`LICENSE.txt` de Luanti es **un solo archivo con tres grants en orden**, y el
orden es el problema:

| Bytes del archivo | Sección | Grant |
|-------------------|---------|-------|
| desde el byte 0 | *"License of Luanti textures and sounds"* | **CC-BY-SA 3.0** |
| línea 114 en adelante | *"License of Luanti source code"* | **LGPL-2.1-or-later** |
| línea ~190 | Lua embebido | **MIT** |

Una sonda que lea los **primeros 200 bytes** del archivo —la forma estándar de
identificar una licencia— concluye **"CC-BY-SA 3.0"** y publica que *el código de
Luanti es una licencia de contenido*. Es exactamente el error que esta KB ya
tiene documentado en `local-llm-npc` (CC-BY-4.0 aplicada a software), **pero al
revés**: ahí la licencia de contenido era real; acá sería un artefacto de leer la
cabecera de un archivo compuesto.

> **Discriminador mecánico, derivado del checksum de ayer:** 8793 B **no cae en
> ninguna banda canónica** (MIT ~1.100 · Ms-PL ~3.900 · Apache ~11.350 · MPL
> ~16.700 · CC-BY ~18.650 · LGPL-2.1 ~26.400 · AGPL ~34.500 · GPL-3.0 ~35.200).
> **Bytes fuera de toda banda ⇒ archivo compuesto ⇒ hay que buscar los
> *encabezados de sección*, no leer la cabecera del archivo.** Con eso, el
> checksum de bytes deja de ser sólo un detector de familia equivocada y pasa a
> ser también un **detector de archivos multi-grant**.

🆕 **Estado medido de las tres (2026-11-20, revalidado 2026-11-21):** `luanti-org/luanti` **13.882★,
commit de hoy** — la única de las tres con desarrollo activo en GitHub y por tanto
la mejor base de prueba; `supertuxkart/stk-code` 5.490★, commit del 3-nov-2026,
activa; `0ad/0ad` **archivada** (arriba). ⚠️ **Lección de método que vale para
toda la KB:** un repo puede estar muerto **mientras el proyecto está vivo**
—migró de hosting— y ésa es la inversa del chequeo habitual. `archived` es un
booleano que ni las ★ ni `pushed_at` revelan: 0 A.D. tiene 2.827★ y está
archivado. Hay que pedir los tres datos.

---

## 🆕 Plataforma vertical nueva: el **mod de un juego existente** como camino a producción

Esta página tenía dos caminos: engine propio (Godot) o juego open source para
forkear. Falta el tercero, que es el que tiene **jugadores reales hoy**: poner la
AI **dentro de un juego comercial que ya tiene comunidad de modding**.

| Plataforma | Licencia (leída 2026-11-21) | Repo | Qué habilita | Límite duro |
|------------|------------------------------|------|--------------|-------------|
| **SkyrimNet** (Skyrim, SKSE) | 🔴 **propia, no OSI** (`LICENSE`, 3745 B) — ⚠️ **con `Source/Scripts/SkyrimNetApi.psc` bajo MIT** | [MinLL/SkyrimNet-GamePlugin](https://github.com/MinLL/SkyrimNet-GamePlugin) (386★, commit de hoy) | NPCs que **hablan, recuerdan, reaccionan y actúan**, con voz en tiempo real, en un juego con millones de jugadores. Es la prueba de campo de NPC-LLM con más horas reales de esta KB. | **Integrar sí, forkear no.** §2 prohíbe redistribuir el mod fuera de sus páginas oficiales; §4 prohíbe reverse engineering del `.dll`; §6 prohíbe usar los nombres/logos para promocionar; `FatelessAiEngine.dll` **se licencia aparte**. Los *content packs* (prompts/templates bajo `plugins/skyrimnet/base`) **sí** se pueden modificar y distribuir (§3). |
| **universal-modder** | **MIT** | [rehan-remade/universal-modder](https://github.com/rehan-remade/universal-modder) (9.825★, commit de hoy) | Modding asistido por agente sobre juegos de PC que el usuario ya posee, con testing in-game. La capa **genérica** del mismo camino. | El grant cubre la herramienta, **no** los juegos que modifica: el EULA de cada juego sigue mandando. |

**Por qué el carve-out de SkyrimNet es un patrón reutilizable, no una curiosidad
legal.** Es el **inverso exacto de PostHog**: PostHog es un repo permisivo con un
directorio cerrado (`ee/`); SkyrimNet es un repo **cerrado con un archivo
abierto** (su API Papyrus, MIT, *"you may compile against it, copy it and modify
it freely, in any mod, open or closed source"*). Para un estudio eso define la
superficie contratable con precisión quirúrgica: **se puede construir y vender un
mod que hable con SkyrimNet; no se puede empaquetar SkyrimNet.** Cuando un
proyecto quiere ecosistema sin regalar el producto, **ésta es la forma de
licenciamiento que lo consigue**, y conviene reconocerla cuando aparece del lado
del cliente.

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
🔴 **Corrección de este pass sobre la elección de MCP.** Esta página decía que
para Unity el dominante era `IvanMurzak/Unity-MCP` (Apache-2.0, 4.672★). **No lo
es:** el líder real es
**[CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp) (MIT, 16.087★)**,
3,4× más grande **y con licencia más permisiva**. Estaba invisible para esta KB
porque **fue renombrado** (`justinpbarnett/unity-mcp` → `CoplayDev/unity-mcp`) y
las búsquedas seguían apuntando al slug viejo. ⚠️ Su rama default es **`beta`**:
usar `HEAD` en `raw`, no `main`.

- **Unity →** `CoplayDev/unity-mcp` (MIT, 16.087★). Si el cliente exige
  Apache-2.0 por política, `IvanMurzak/Unity-MCP` sigue siendo la opción válida.
- **Godot →** aquí hay un trade-off real que **hay que decirle al cliente**:
  `Coding-Solo/godot-mcp` tiene la adopción (**6.713★, MIT**, 15× el
  `tomyud1/godot-mcp` que esta KB listaba) pero **7 meses sin commits**;
  `hi-godot/godot-ai` tiene la mitad (**3.627★**) y **commits de hoy**. Para
  trabajo de cliente con soporte: **`godot-ai`**.
- **Unreal →** `prajwalshettydev/UnrealGenAISupport` (MIT, 686★) es el único con
  licencia limpia y volumen. `chongdashu/unreal-mcp` tiene 2.120★ **y ningún
  archivo de licencia**: no usar.

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
| AI-assisted game dev (Unity) | Unity + **CoplayDev/unity-mcp** (MIT) | Claude Code vía MCP | 1 día de setup |
| 🆕 **Pipeline de estudio con agentes por rol** | cualquier engine | **Claude-Code-Game-Studios** (MIT, 40.377★): 49 agentes + 72 skills | **1-2 días de setup, semanas de calibración** |
| 🆕 **Capacidad elástica de game servers** | Nakama/Talo sobre Kubernetes | **Agones** (Apache-2.0) + `FleetAutoscaler` | 2-3 semanas |
| 🔴 **Marcado de contenido AI (UE, Art. 50)** | catálogo existente | pipeline de metadata + C2PA | **3-5 semanas — YA NO CABE ANTES DEL 2-DIC-2026 (quedan 12 días).** Ver `compose/patterns.md` → Receta 7, reencuadrada: inventario de exposición primero. |
| Juego AAA con AI cloud | O3DE | AWS Bedrock / GameLift | meses (enterprise) |
| **Prototipado con world model** | Godot/Unity + Matrix-Game 4.0 | rendering neural | 4-6 semanas |

---
*Licencias verificadas el 2026-11-20 en **dos canales** (`gh api
search/repositories` para SPDX/★/`archived`/`pushed_at`/rama default + lectura del
grant en `raw.githubusercontent.com/<slug>/HEAD/<archivo>` con bytes anotados),
con control negativo en ambos sentidos. **Correcciones de este pass:** `MonoGame`
(MIT → **Ms-PL**), `PostHog` (MIT → **MIT Expat + carve-out `ee/`**), `o3de`
(Apache-2.0 → **Apache-2.0 o MIT, con Qt LGPL-3.0**), `NamazuStudios/elements`
(AGPL-3.0 → **MPL-2.0**, descarte revertido), `0ad/0ad` (**archivado**),
`pelican-dev/panel` (**ya no existe**), y la elección de MCP para Unity y Godot.
Revalidadas de octubre: `local-llm-npc` (CC-BY-4.0) y `grafana` (AGPL-3.0), ambas
ahora confirmadas por el segundo canal. Ver `agents/top.md` para el método, el
checksum de bytes y los grants bloqueantes.*
