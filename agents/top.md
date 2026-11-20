---
industry: gaming
region: Global
updated: 2026-11-20
---

# 🎮 Agentes AI — Gaming

> Agentes y herramientas AI open source. Foco: MIT / Apache 2.0 / BSD / Zlib.
> Última actualización: **2026-11-20** (passes previos: 2026-10-26, 2026-07-02).
>
> **Método de este pass — dos canales, y el segundo es nuevo:**
> 1. `gh api search/repositories -f q='repo:<slug>'` → existencia + **SPDX** +
>    ★ + `archived` + `pushed_at` + **default_branch**, todo en una llamada.
>    El pass de octubre registró este canal como **bloqueado**; **no lo está**.
>    Esa corrección es la que hizo posible todo lo de abajo.
> 2. `raw.githubusercontent.com/<slug>/HEAD/<archivo>` → el **grant real**, leído
>    con bytes.
>
> Los dos canales **no son redundantes y discrepan sistemáticamente**: el API
> devuelve `NOASSERTION` para 8 repos de esta KB cuyo grant es permisivo y
> legible. `NOASSERTION` **no significa "sin licencia": significa "no matcheó un
> SPDX único"**, y eso es un **detector de licencias compuestas** — es el canal
> que destapó las 4 correcciones de este pass. `curl -sI https://github.com/...`
> sigue sin servir como prueba de existencia (403/200 del proxy, no del repo).
> Control negativo: `gmilano/this-repo-does-not-exist-control-xyz` → `NOT_FOUND`. ✅

## 🔴 Lo más importante de este pass: el *checksum de bytes* de una licencia

Este KB venía anotando los **bytes** de cada archivo de licencia. Nadie los usaba.
**Son un checksum**: los textos de licencia tienen longitud canónica, así que una
fila cuyos bytes no caen en la banda de su familia está **mal clasificada**.

| Familia | Bytes canónicos del texto completo |
|---------|-----------------------------------|
| MIT / Expat | **1.050 – 1.150** |
| Ms-PL | **~3.900** |
| Apache-2.0 | **11.300 – 11.400** |
| MPL-2.0 | **~16.700** |
| CC-BY-4.0 | **~18.650** |
| LGPL-2.1 | **~26.400** |
| AGPL-3.0 | **~34.500** |
| GPL-3.0 | **~35.200** |

Aplicado a las filas que este KB **ya tenía escritas**, sin una sola llamada de
red nueva, marca 4 y **las 4 estaban mal. Cero falsas alarmas.**

| Fila del KB | Bytes anotados | Decía | Es | Cómo lo delata el checksum |
|-------------|----------------|-------|-----|----------------------------|
| `MonoGame/MonoGame` | 3882 | MIT | 🔴 **Ms-PL** | 3882 no es MIT (~1.100). Es exactamente Ms-PL. |
| `PostHog/posthog` | 1563 | MIT | 🔴 **MIT Expat + carve-out `ee/`** | 1563 excede MIT en ~400 B: el excedente es el preámbulo del carve-out. |
| `o3de/o3de` | 1475 | Apache-2.0 | 🟡 **Apache-2.0 *o* MIT a elección** | 1475 ≪ 11.350: no es el texto de Apache, es un **puntero**. |
| `NamazuStudios/elements` | 16726 | AGPL-3.0 | 🔴 **MPL-2.0** | **El propio KB anota AGPL-3.0 = 34523 B en `grafana`.** 16726 ≠ 34523 → contradicción interna. |

La fila de `elements` es la lección entera en una línea: **el dato que refutaba la
etiqueta ya estaba en el archivo, al lado de la etiqueta**, hace casi un mes.

**Refinamiento necesario** (un no-error que la regla marca): `ml-agents` anota
`LICENSE.md`, 572 B para Apache-2.0. Está **por debajo** de la banda y es
correcto: es una cabecera-puntero ("Licensed under the Apache License, Version
2.0"), no el texto. Por eso la regla es **asimétrica**: bytes *por encima* de la
banda ⇒ licencia compuesta o familia equivocada; bytes muy *por debajo* ⇒ archivo
puntero, hay que seguir el puntero. Nunca "sin licencia".

## 🔴 Dos renames, y uno de ellos le costó al KB el líder del mercado

| Slug antiguo | Slug canónico hoy | Evidencia |
|--------------|-------------------|-----------|
| `justinpbarnett/unity-mcp` | **`CoplayDev/unity-mcp`** (16.087★) | `repo:` antiguo → `NOT_FOUND`; `raw` antiguo → **200** en `main` y `master` ⇒ redirect, no borrado. |
| `googleforgames/agones` | **`agones-dev/agones`** (7.106★) | idéntica firma: search `NOT_FOUND`, `raw` antiguo 200. |
| `minetest/minetest` | **`luanti-org/luanti`** (13.882★) | `repo:` → `NOT_FOUND`; `raw` antiguo **200 en `HEAD` y `master`, 404 en `main`**. 🔴 El pass de octubre escribió que este slug *"sigue resolviendo"*: resuelve **por redirect**, no porque el repo exista. |
| `JonTelep/awesome-ai-game-generation` | **`Anil-matcha/awesome-ai-game-generation`** (58★) | misma firma. Detectado **sólo** porque este pass verificó las URLs de las tablas heredadas, no sólo las nuevas. |

**Cuatro renames en 83 URLs** ⇒ ~1 cada 21 repos, el mismo orden de magnitud que
el ~1/10 que midió la KB de enterprise. **No es una anomalía: es una tasa**, y una
KB que no revalida slugs acumula punteros muertos a ese ritmo. La firma es siempre
la misma y es barata de probar: **`repo:` devuelve `NOT_FOUND` mientras `raw` del
slug viejo devuelve 200.** Si sólo se mira `raw`, el rename es invisible; si sólo
se mira la API, el repo parece borrado. Hacen falta los dos.

**Cómo un rename produce un error de recomendación.** Este KB afirmaba que
`IvanMurzak/Unity-MCP` (Apache-2.0, 4.672★) era "el MCP de engine **dominante**".
No lo es:

| Repo | ★ | Licencia | `pushed_at` | rama default |
|------|---|----------|-------------|--------------|
| **`CoplayDev/unity-mcp`** | **16.087** | **MIT** | 2026-11-17 | ⚠️ **`beta`** |
| `IvanMurzak/Unity-MCP` | 4.672 | Apache-2.0 | 2026-11-19 | `main` |
| `CoderGamester/mcp-unity` | 2.018 | MIT | 2026-10-11 | `main` |
| `AnkleBreaker-Studio/unity-mcp-server` | 549 | NOASSERTION | 2026-10-23 | `main` |
| `jackwrichards/UnityMCP` | 528 | NOASSERTION | 2025-03-18 | `main` |
| `isuzu-shiranui/UnityMCP` | 344 | MIT | 2026-11-20 | `main` |
| `FunplayAI/funplay-unity-mcp` | 330 | MIT | 2026-11-19 | `main` |

El líder tiene **3,4×** las estrellas del que el KB recomendaba, y es **MIT**
(más permisivo que Apache-2.0). Se perdió porque **fue renombrado**: las búsquedas
y los blogs seguían indexando `justinpbarnett/unity-mcp`, que ya no resuelve, y el
segundo quedó coronado por defecto.

> **Modo de fallo nuevo, nombrado para la rotación: *el ascenso del subcampeón*.**
> Un rename vuelve **invisible al líder** en toda búsqueda por nombre, y el nº2
> hereda el título de "dominante" sin que nadie lo compare. Es el **inverso** del
> cluster de colisión de nombres que documentó la KB de enterprise: ahí los 7
> resultados eran falsos; **aquí los 7 son reales** y el error está en *cuál* se
> elige. Discriminador: **comparar siempre ≥3 candidatos por ★ antes de escribir
> "dominante"**, y si el nº1 de una categoría no aparece en la búsqueda, sospechar
> un rename y probar `raw` del slug viejo.
>
> ⚠️ `CoplayDev/unity-mcp` tiene rama default **`beta`**. Una sonda que asuma
> `main`/`master` falla. Ésta es la tercera KB de la familia que tropieza con lo
> mismo (casbin en enterprise, `beehave` → `godot-4.x` aquí abajo): **`HEAD` en
> `raw` resuelve la rama default sola; usarlo siempre en vez de hardcodear.**

## 🆕 Orquestación de agentes para game dev — la categoría que no existía aquí

El pass de octubre abrió la tier "tooling MCP para engines". En 3,5 semanas la
capa de arriba se volvió más grande que ella: **no un agente conectado al editor,
sino una flota de agentes con roles de estudio.**

| Nombre | Repo | Licencia | Descripción | ★ |
|--------|------|----------|-------------|---|
| Claude-Code-Game-Studios | [Donchitos/Claude-Code-Game-Studios](https://github.com/Donchitos/Claude-Code-Game-Studios) | **MIT** (`LICENSE`, 1066) | Convierte Claude Code en un estudio: **49 agentes** con rol + **72 skills** de workflow. Fue el **repo nº1 de game-development del trending de GitHub** a principios de noviembre. | **40.377** |
| ohmygame | [WhiteTowerAI/ohmygame](https://github.com/WhiteTowerAI/ohmygame) | **Apache-2.0** (`LICENSE`, 11357) | "Estudio de un solo desarrollador": canvas de juego + BYO key. **+107% desde el 2026-10-26** (1,2k → 2.486). Commit de hoy. | 2.486 |
| blender-skills | [EL4CTEO/blender-skills](https://github.com/EL4CTEO/blender-skills) | **MIT** | Workflows de Blender empaquetados **como skills de agente**. Cubre el hueco de *arte 3D* que el tooling de engine no toca. | 2.529 |
| genex-desktop | [genex-games/genex-desktop](https://github.com/genex-games/genex-desktop) | **MIT** | App de escritorio para hacer juegos con AI sobre **Unreal, Unity, Godot o browser**. ⚠️ rama default **`dev`**. | 900 |

**Lectura para engagement.** 40.377★ en un repo de **prompts y definiciones de
agente** —sin engine, sin runtime, sin modelo— contra 16.087★ del plugin de
editor más usado. La demanda que el mercado está expresando no es "un agente en el
editor", es **un pipeline de estudio con roles**. Dicho con la honestidad que
corresponde: las estrellas miden **interés**, no producción, y un repo de prompts
es barato de marcar como favorito. Pero el orden de magnitud es el dato.

## Tooling MCP para engines

| Nombre | Repo | Licencia | Descripción | ★ |
|--------|------|----------|-------------|---|
| 🆕 **unity-mcp (Coplay)** | [CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp) | **MIT** (`LICENSE`, 1066) | **El líder real del ecosistema Unity.** Bridge MCP editor↔agente. ⚠️ rama default **`beta`**. | **16.087** |
| 🆕 **godot-mcp (Coding-Solo)** | [Coding-Solo/godot-mcp](https://github.com/Coding-Solo/godot-mcp) | **MIT** | **15× el que este KB listaba como "segunda opción".** ⚠️ **`pushed_at` 2026-04-16: 7 meses sin commits** — líder por adopción, dormido por mantenimiento. | **6.713** |
| godot-ai | [hi-godot/godot-ai](https://github.com/hi-godot/godot-ai) | MIT | MCP server para Godot: 46 tools, 120+ operaciones. Scene building, scripts, signals, animations. **Commit de hoy** — el Godot MCP *mantenido*. | 3.627 |
| Unity-MCP (Murzak) | [IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP) | Apache-2.0 | Cualquier método C# → tool con una línea. **Nº2 real, no nº1.** Sigue siendo la opción Apache-2.0 si el cliente rechaza MIT por política. | 4.672 |
| mcp-unity | [CoderGamester/mcp-unity](https://github.com/CoderGamester/mcp-unity) | MIT | Tercera opción Unity con tracción propia. | 2.018 |
| UnrealGenAISupport | [prajwalshettydev/UnrealGenAISupport](https://github.com/prajwalshettydev/UnrealGenAISupport) | MIT | Plugin Unreal LLM/GenAI + MCP server UE5. **Único Unreal con licencia limpia y volumen.** 7 meses sin commits. | 686 |
| Unreal-MCP (Murzak) | [IvanMurzak/Unreal-MCP](https://github.com/IvanMurzak/Unreal-MCP) | Apache-2.0 | Plugin C++ + bridge .NET. **Temprano** (51 → 64 en 3,5 semanas). | 64 |
| GameDev-MCP-Server | [IvanMurzak/GameDev-MCP-Server](https://github.com/IvanMurzak/GameDev-MCP-Server) | Apache-2.0 | Backend MCP engine-agnostic. **Infra, no producto** (16 → 21). | 21 |

**Elección práctica.** Unity → `CoplayDev/unity-mcp` (MIT, 16k). Godot → el
trade-off es real y hay que decirlo al cliente: `Coding-Solo` tiene la adopción
(6.713★) pero **7 meses parado**; `hi-godot/godot-ai` tiene la mitad (3.627★) y
**commits de hoy**. Para trabajo de cliente con soporte, `godot-ai`. Unreal →
`UnrealGenAISupport` es el único con licencia limpia y volumen; el resto es
temprano o no tiene grant (ver tabla de bloqueantes).

## Ecosistema Godot — NPC behaviour (MIT, base recomendada)

| Nombre | Repo | Licencia | Descripción | ★ |
|--------|------|----------|-------------|---|
| LimboAI | [limbonaut/limboai](https://github.com/limbonaut/limboai) | MIT | Plugin C++ Godot: Behavior Trees + HSM. `BTState` combina ambos. ⚠️ rama default **`master`**. | 3.137 |
| Beehave | [bitbrain/beehave](https://github.com/bitbrain/beehave) | MIT | Behavior trees componibles en el scene tree. ⚠️ rama default **`godot-4.x`** — ni `main` ni `master`. | 3.329 |
| godot_rl_agents | [edbeeching/godot_rl_agents](https://github.com/edbeeching/godot_rl_agents) | MIT | Wrappers para 4 frameworks RL: SB3, Sample Factory, Ray RLLib, CleanRL. **900+ → 1.639: +82%**, el mayor salto relativo de la KB. | 1.639 |

## NPC AI con LLMs

| Nombre | Repo | Licencia | Descripción | ★ |
|--------|------|----------|-------------|---|
| LLMUnity | [undreamai/LLMUnity](https://github.com/undreamai/LLMUnity) | Apache-2.0 | Personajes Unity con LLMs locales o cloud. 7 meses sin commits (2026-04-29). | 1.739 |
| npcpy | [NPC-Worldwide/npcpy](https://github.com/NPC-Worldwide/npcpy) | MIT | Librería Python para NPC con LLMs multimodales. **Commit de hoy.** | 1.529 |
| Interactive LLM NPCs | [AkshitIreddy/Interactive-LLM-Powered-NPCs](https://github.com/AkshitIreddy/Interactive-LLM-Powered-NPCs) | MIT | Diálogo LLM para Cyberpunk 2077, GTA 5, AC. Vector stores + `pre-conversation.json` por personaje. | 739 |
| 🆕 ai-town | [a16z-infra/ai-town](https://github.com/a16z-infra/ai-town) | **MIT** | Starter kit **desplegable** de un pueblo de agentes que conversan y socializan. **Faltaba por completo** en esta KB, que sí tenía el paper de Stanford pero no la versión que se despliega. **Commit de hoy.** | **10.835** |

## Agentes que *juegan* — evaluación y QA

| Nombre | Repo | Licencia | Descripción | ★ |
|--------|------|----------|-------------|---|
| GamingAgent / LMGame Bench | [lmgame-org/GamingAgent](https://github.com/lmgame-org/GamingAgent) | MIT (`LICENSE`, 1065) | **ICLR 2026.** Agentes LLM/VLM en entornos estandarizados + harness + leaderboard. ⚠️ **`pushed_at` 2025-11-16: un año exacto sin commits.** Sigue siendo el único benchmark publicado del nicho, pero es un **instrumento congelado**, no un proyecto activo. | 1.006 |

## World models — juego generado, no assets generados

| Nombre | Repo | Licencia | Descripción | ★ |
|--------|------|----------|-------------|---|
| Matrix-Game 4.0 | [SkyworkAI/Matrix-Game](https://github.com/SkyworkAI/Matrix-Game) | ⚠️ **grant partido** (ver abajo) | Release 15-oct-2026. Engine ejecutable (lógica, física, input) + modelo causal de video que convierte frames low-poly en visuales. 67 FPS en RTX 5080 @864×480 con SageAttention. | 2.944 |
| MineWorld | [microsoft/mineworld](https://github.com/microsoft/MineWorld) | MIT (`LICENSE`, 1141) | World model interactivo sobre Minecraft. Transformer autoregresivo visual-acción. Checkpoints 300M/700M/1200M. 4–7 fps con Diagonal Decoding. 6 meses sin commits. | 497 |

⚠️ **Matrix-Game — el grant depende del directorio, y el badge de GitHub engaña.**
Verificado el 2026-10-26, revalidado hoy (el API devuelve `MIT` para todo el repo,
lo cual es precisamente el error):

| Ruta | Archivo | Bytes | Grant real |
|------|---------|-------|-----------|
| raíz | `LICENSE` | 1118 | **MIT** |
| `Matrix-Game-4/` | `Matrix-Game-4/LICENSE` | 11357 | **Apache-2.0** |

La versión que un estudio usaría (v4) es **Apache-2.0**: ambos permisivos, pero
Apache-2.0 obliga a preservar `NOTICE` y trae grant de patente. **Los pesos están
en Hugging Face y su licencia sigue sin verificarse** (`huggingface.co` no resuelve
DNS aquí — gap declarado, 3er pass consecutivo). Precedente: en la KB de
automotive, Alpamayo 1.5 tiene código Apache-2.0 y **pesos OpenMDW-1.1**.

## Pathfinding y AI de movimiento

| Nombre | Repo | Licencia | Descripción | ★ |
|--------|------|----------|-------------|---|
| recastnavigation | [recastnavigation/recastnavigation](https://github.com/recastnavigation/recastnavigation) | **Zlib** (confirmado por ambos canales) | Estándar de industria navmesh. Embebido en Unity, Godot, UE. | 8.203 |
| yuka | [Mugen87/yuka](https://github.com/Mugen87/yuka) | MIT | Game AI primitives JS: steering, pathfinding, FSM. ⚠️ default `master`. | 1.466 |
| aitoolkit | [linkdd/aitoolkit](https://github.com/linkdd/aitoolkit) | MIT | Cerebro para NPCs, header-only C++. ⚠️ **`pushed_at` 2024-05-08: 2,5 años. Sin mantenimiento.** | 533 |

El grant de `recastnavigation` está en **`License.txt`** (mayúscula L, minúscula
t). `raw.githubusercontent.com` es **case-sensitive**: una sonda que pida
`LICENSE.txt` devuelve 404 y la fila se publica como "sin licencia".

## Generación procedural y modding

| Nombre | Repo | Licencia | Descripción | ★ |
|--------|------|----------|-------------|---|
| universal-modder | [rehan-remade/universal-modder](https://github.com/rehan-remade/universal-modder) | **MIT** (verificado hoy; antes ★ sin verificar) | Modding asistido por AI. **Commit de hoy.** La categoría con más volumen propio fuera del tooling de engine. | **9.809** |
| WorldX | [YGYOOO/WorldX](https://github.com/YGYOOO/WorldX) | MIT | Generación procedural de mundos con AI (TypeScript). 1,1k → 1.637. | 1.637 |
| OpenGame | [leigest519/OpenGame](https://github.com/leigest519/OpenGame) | Apache-2.0 | Framework agéntico para generar juegos web end-to-end con GameCoder-27B. ⚠️ 2,5 meses sin commits (2026-09-03); el README no anuncia nada desde abr-2026. | 3.077 |

## ⚠️ Licencias y grants que bloquean o condicionan uso comercial

Revalidado entero el 2026-11-20. **Ninguna fila es base recomendable sin revisión
legal.**

| Nombre | Repo | Grant declarado | Grant real verificado | Problema |
|--------|------|-----------------|----------------------|----------|
| 🆕 **horizon** | [horizon-rust/horizon](https://github.com/horizon-rust/horizon) | aparece en trending de game-dev | **LGPL-3.0** (SPDX del API) | Reescritura de Minecraft Java en Rust, **11.515★** y subiendo: va a aparecer en toda lista de trending. LGPL-3.0 exige poder relinkear: incompatible con un binario de juego cerrado sin cuidado de arquitectura. |
| 🆕 **HY-World 2.0** | [Tencent-Hunyuan/HY-World-2.0](https://github.com/Tencent-Hunyuan/HY-World-2.0) | "open-source SOTA world model" en el README | **Ningún archivo de licencia en HEAD** (`LICENSE`, `.txt`, `.md` → 404) | 2.891★ y se autodenomina open source **sin publicar grant**. Sin archivo no hay permiso. Mismo patrón que `unreal-mcp`. |
| 🆕 **MonoGame** | [MonoGame/MonoGame](https://github.com/MonoGame/MonoGame) | **MIT** (así lo publicaba este KB en 2 archivos) | **Ms-PL** (`LICENSE.txt`, 3882 B, leído completo) | Ms-PL es OSI y permisivo **para binarios**, pero §3(D): si distribuís **cualquier porción en código fuente, sólo podés hacerlo bajo Ms-PL**. Un entregable de *fuente* a cliente no se puede relicenciar MIT/Apache. §3(B) termina tu licencia de patente si demandás a un contribuidor. **Corrección de este pass.** |
| 🆕 **PostHog** | [PostHog/posthog](https://github.com/PostHog/posthog) | **MIT** (así lo publicaba este KB en 3 lugares) | **MIT Expat fuera de `ee/`; `ee/` bajo PostHog Enterprise License** (`ee/LICENSE`, 2176 B) | El texto es **palabra por palabra el mismo patrón que Onyx** en la KB de enterprise: *"All content that resides under the `ee/` directory … is licensed under the license defined in `ee/LICENSE`"*. No bloquea self-host, pero "PostHog es MIT" es **falso como entrada de inventario**. |
| unreal-mcp | [chongdashu/unreal-mcp](https://github.com/chongdashu/unreal-mcp) | Badge "License: MIT" en el README | **Ningún archivo de licencia**; SPDX del API = `NONE` | Un badge no es un grant. **2.120★** y **sin commits desde 2025-04-22**: el repo sin licencia más popular del nicho Unreal. Legalmente "all rights reserved". |
| local-llm-npc | [code-forge-temple/local-llm-npc](https://github.com/code-forge-temple/local-llm-npc) | MIT (así figuraba en este KB) | **CC-BY-4.0** (`LICENSE`, 18655 B) — **ahora confirmado por los dos canales** | Licencia de *contenido* aplicada a software. La corrección de octubre queda **doblemente verificada**: SPDX del API = `CC-BY-4.0`. 54★. |
| Hunyuan-GameCraft | [Tencent-Hunyuan/Hunyuan-GameCraft-1.0](https://github.com/Tencent-Hunyuan/Hunyuan-GameCraft-1.0) | "open source" en prensa | Licencia propia Tencent, **no OSI**; SPDX = `NOASSERTION` | Restricciones de uso/territorio típicas de las community licenses de Tencent. Sin commits desde 2025-11-28. |
| Grafana | [grafana/grafana](https://github.com/grafana/grafana) | Apache-2.0 (así figuraba en este KB) | **AGPL-3.0** (`LICENSE`, 34523 B) | Copyleft de red. Corrección de octubre, **revalidada**: SPDX del API = `AGPL-3.0`. 78.014★. |
| NobodyWho | — (Godot AssetLib) | EUPL-1.2 | EUPL-1.2 | Copyleft: derivados deben ser open source. |
| Aivill | [SKYHUBDev/Aivill](https://github.com/SKYHUBDev/Aivill) | — | **Sin licencia y sin README**; **0★** | Repo vivo y vacío. Confirmado por segunda vez: no hay nada que observar. |

### 🟢 Fila que este pass **re-admite** — una rechazada por error

| Nombre | Repo | Qué decía el KB | Qué es | Consecuencia |
|--------|------|-----------------|--------|--------------|
| **elements** | [NamazuStudios/elements](https://github.com/NamazuStudios/elements) | "**AGPL-3.0** (`LICENSE.txt`, 16726 B) — copyleft de red sobre el servidor de juego. **Fuera de la ruta permisiva.**" | **MPL-2.0** (`LICENSE.txt`, **los mismos 16726 B**, cabecera leída: *"Mozilla Public License Version 2.0"*) | MPL-2.0 es copyleft **a nivel de archivo**: hostear el backend **no** obliga a liberar tu código; sólo las modificaciones a archivos MPL. **El rechazo era incorrecto.** Sigue siendo chico (22★) y no desplaza a Nakama ni Talo, pero **no estaba descalificado y lo dimos por descalificado.** |

> **Regla de la rotación, nueva: un rechazo caduca igual que una recomendación.**
> Este KB revalidaba lo que recomendaba y **nunca** lo que había descartado. Un
> descarte mal fundado es **peor** que una recomendación floja, porque se saca el
> candidato del campo de visión y **nadie vuelve a mirarlo**. Nótese que el slug
> además cambió de capitalización (`namazustudios` → `NamazuStudios`).

## Candidatos evaluados y **rechazados** (revalidado 2026-11-20)

| Candidato | Licencia | Por qué se rechaza |
|-----------|----------|--------------------|
| [balaraj74/openNPC](https://github.com/balaraj74/openNPC) | MIT (verificado) | "Framework open source para NPCs autónomos". Primera mano: **6★**, el README se autodescribe "production-ready" y su comando de clonado sigue diciendo `yourusername`. Sin commits desde 2026-05-21. El grant es real; el proyecto no. |
| [lschiweck/LLM-NPC-Agents](https://github.com/lschiweck/LLM-NPC-Agents) | Apache-2.0 (verificado) | Proyecto real (backend + cliente Unity, STT/TTS/LLM, cita un paper) pero **11★** y "Early Release". Señal, no ruta recomendada. |
| [pelican-dev/panel](https://github.com/pelican-dev/panel) | — | 🔴 **Estado agravado: el repo ya no existe.** En octubre era "vivo pero sin archivo de licencia"; hoy `repo:` → `NOT_FOUND` y `raw` → **404**. Usar `pterodactyl/panel` (MIT, verificado). |
| [raiscui/gh-trends](https://github.com/raiscui/gh-trends) | MIT | **0★.** Útil como *feed* de trending (releases diarios, commit de hoy) y es la fuente que este KB usa para fechar señales, pero **citarlo no es avalarlo**: es un scraper personal, no una autoridad. |
| `awesome-*` listas | CC-BY-family | `lappemic/awesome-ai-built-games`, `SubmitGame/awesome-ai-game-dev-tools`, `Anil-matcha/awesome-ai-game-generation` (🔴 renombrado desde `JonTelep/`), `NeuraLiying/Awesome-World-Models` (340+ papers). Buen **canal de descubrimiento**, pero licencia de contenido: no son código. |

## Gaps declarados — lo que no se pudo leer

Una ausencia medida vale más que un silencio.

- **`huggingface.co` → no resuelve DNS. Tercer pass consecutivo.** Consecuencia
  concreta e inmutable desde julio: **ninguna licencia de pesos ha sido verificada
  nunca por esta KB** — ni Matrix-Game 4.0, ni los checkpoints de MineWorld, ni
  `tencent/HY-World-2.0`. En un world model, los pesos son la mitad del entregable.
  Es el gap más viejo y más caro de la KB. **Vía a probar el próximo pass:** la KB
  de enterprise leyó una spec bloqueada vía `raw.githubusercontent.com`; buscar si
  el model card está espejado en un repo de GitHub.
- **Listado de directorios de un repo:** sigue sin canal. Por eso el grant partido
  de Matrix-Game se verifica por sonda de rutas conocidas y no por enumeración, y
  por eso no puedo afirmar que `Matrix-Game-4/` sea el único subdirectorio con
  licencia propia.
- **★ de este pass: 100% releídas.** No queda ninguna fila con `*` heredado. El
  canal `gh api search/repositories` lo hizo posible; el pass de octubre lo creía
  bloqueado y tuvo que dejar 20 filas sin verificar.

---
*Este pass: 4 correcciones de licencia (2 nuevas, 2 revalidadas por 2º canal),
1 rechazo revertido, 2 renames documentados, 1 repo desaparecido, 6 filas nuevas,
7 repos con rama default ≠ `main`/`master` señalados, 1 regla de checksum derivada
y validada 4/4 sobre datos preexistentes, 38 repos verificados en dos canales con
control negativo en ambos sentidos. 2026-11-20.*
