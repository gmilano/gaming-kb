---
industry: gaming
region: Global
updated: 2026-10-26
---

# 🎮 Agentes AI — Gaming

> Agentes y herramientas AI open source. Foco: MIT / Apache 2.0 / BSD / Zlib.
> Última actualización: **2026-10-26** (pass previo: 2026-07-02).
>
> **Método de verificación de este pass:** cada URL se probó en dos etapas —
> `git ls-remote` (existencia) + lectura del archivo de licencia en
> `raw.githubusercontent.com/<slug>/HEAD/<LICENSE>` (grant real, con bytes).
> `curl -sI https://github.com/...` devuelve **403 para todo repo**, incluido uno
> fabricado, por lo que no distingue repos vivos de muertos y no se usó.
> Control negativo (`gmilano/this-repo-does-not-exist-control-xyz`) correctamente
> UNREACHABLE. Las ★ se leyeron de la página del repo el 2026-10-26.

## 🆕 Tooling MCP para engines — la categoría que más se movió desde julio

Esta tier no existía en este archivo. Es hoy la capa de AI con más adopción real
en gaming: no NPCs, sino **agentes conectados al editor**.

| Nombre | Repo | Licencia | Descripción | Stars |
|--------|------|----------|-------------|-------|
| Unity-MCP | [IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP) | Apache-2.0 | AI Skills + MCP tools + CLI para Unity. Cualquier método C# se convierte en tool con una línea. Loop completo develop+test. Claude Code, Gemini, Copilot, Cursor. | 4.5k |
| godot-ai | [hi-godot/godot-ai](https://github.com/hi-godot/godot-ai) | MIT | MCP server para Godot: **46 tools y 120+ operaciones**. Scene building, scripts, signals, animations desde el agente. | 3.2k |
| godot-mcp | [tomyud1/godot-mcp](https://github.com/tomyud1/godot-mcp) | MIT | MCP server + plugin Godot alternativo para desarrollo asistido. Segunda opción real del ecosistema Godot. | 444 |
| UnrealGenAISupport | [prajwalshettydev/UnrealGenAISupport](https://github.com/prajwalshettydev/UnrealGenAISupport) | MIT | Plugin Unreal para LLM/GenAI + MCP server UE5. GPT, Gemini, Claude. | 619* |
| Unreal-MCP | [IvanMurzak/Unreal-MCP](https://github.com/IvanMurzak/Unreal-MCP) | Apache-2.0 | Plugin C++ de editor + bridge .NET + `unreal-mcp-cli`. Mismo autor que Unity-MCP. **Temprano.** | 51 |
| GameDev-MCP-Server | [IvanMurzak/GameDev-MCP-Server](https://github.com/IvanMurzak/GameDev-MCP-Server) | Apache-2.0 | Backend MCP engine-agnostic compartido por Unity-MCP, Godot-MCP y Unreal-MCP. **Infra, no producto; adopción mínima aún.** | 16 |

**Lectura para engagement:** la asimetría 4.5k / 51 / 16 dentro del *mismo autor*
dice que la adopción está en el plugin de Unity, no en el backend compartido.
Elegir `Unity-MCP` o `godot-ai` directamente; `GameDev-MCP-Server` es apuesta
arquitectónica, no base estable.

## Ecosistema Godot — NPC behaviour (MIT, base recomendada)

| Nombre | Repo | Licencia | Descripción | Stars |
|--------|------|----------|-------------|-------|
| LimboAI | [limbonaut/limboai](https://github.com/limbonaut/limboai) | MIT | Plugin C++ para Godot: Behavior Trees + Hierarchical State Machines. `BTState` combina ambos para IA reactiva. | 3.1k |
| Beehave | [bitbrain/beehave](https://github.com/bitbrain/beehave) | MIT | Behavior trees componibles en el scene tree de Godot, para NPC y boss AI. | 3.2k* |
| godot_rl_agents | [edbeeching/godot_rl_agents](https://github.com/edbeeching/godot_rl_agents) | MIT | Wrappers para 4 frameworks RL: StableBaselines3, Sample Factory, Ray RLLib, CleanRL. | 900+* |

## NPC AI con LLMs

| Nombre | Repo | Licencia | Descripción | Stars |
|--------|------|----------|-------------|-------|
| LLMUnity | [undreamai/LLMUnity](https://github.com/undreamai/LLMUnity) | Apache-2.0 | Personajes Unity con LLMs locales o cloud. | 1.7k |
| npcpy | [NPC-Worldwide/npcpy](https://github.com/NPC-Worldwide/npcpy) | MIT | Librería Python para NPC con LLMs multimodales. | 1.4k* |
| Interactive LLM NPCs | [AkshitIreddy/Interactive-LLM-Powered-NPCs](https://github.com/AkshitIreddy/Interactive-LLM-Powered-NPCs) | MIT | Diálogo LLM para Cyberpunk 2077, GTA 5, AC. Vector stores para memoria + `pre-conversation.json` por personaje. | 716* |

## 🆕 Agentes que *juegan* — evaluación y QA

| Nombre | Repo | Licencia | Descripción | Stars |
|--------|------|----------|-------------|-------|
| GamingAgent / LMGame Bench | [lmgame-org/GamingAgent](https://github.com/lmgame-org/GamingAgent) | MIT | **ICLR 2026.** Agentes LLM/VLM que juegan en entornos estandarizados + harness de gaming + leaderboard. Despliega computer-use agents (CUA) para juegos en PC. | 998 |

Esta fila cierra el hueco que el pass de julio declaraba sin resolver: había
patrones de "QA con RL" pero ningún **benchmark publicado** para medir si el
agente juega bien. `LMGame Bench` es el instrumento, y es MIT.

## 🆕 World models — juego generado, no assets generados

| Nombre | Repo | Licencia | Descripción | Stars |
|--------|------|----------|-------------|-------|
| Matrix-Game 4.0 | [SkyworkAI/Matrix-Game](https://github.com/SkyworkAI/Matrix-Game) | ⚠️ **grant partido** (ver abajo) | **Release 15-oct-2026.** Engine ejecutable (lógica, física, input) + modelo causal de video que convierte frames low-poly en visuales. 67 FPS en RTX 5080 a 864×480 con SageAttention. Código de inferencia, demo browser y 3 juegos nativos (Medieval Castle, Ancient Egypt, City Racing). | 2.8k |
| MineWorld | [microsoft/MineWorld](https://github.com/microsoft/MineWorld) | MIT | World model interactivo en tiempo real sobre Minecraft. Transformer autoregresivo visual-acción. Checkpoints 300M / 700M / 1200M, 16–32 frames de contexto. 4–7 fps con Diagonal Decoding. | 493 |

⚠️ **Matrix-Game — el grant depende del directorio, y el badge de GitHub engaña.**
Leído primera mano el 2026-10-26:

| Ruta | Archivo | Bytes | Grant real |
|------|---------|-------|-----------|
| raíz del repo | `LICENSE` | 1118 | **MIT** |
| `Matrix-Game-4/` | `Matrix-Game-4/LICENSE` | 11357 | **Apache-2.0** |

El sidebar de GitHub muestra "MIT license" para todo el repo. La versión que un
estudio usaría (v4) es **Apache-2.0**: ambos son permisivos y comercialmente
usables, pero Apache-2.0 obliga a preservar `NOTICE` y trae grant de patente que
MIT no tiene. Un equipo que lee sólo el badge publica el aviso equivocado.
**Los pesos están en Hugging Face y su licencia NO se pudo verificar** —
`huggingface.co` no resuelve DNS en este entorno (gap declarado abajo).
Precedente directo: en la KB de automotive, Alpamayo 1.5 tiene código Apache-2.0
y **pesos OpenMDW-1.1**. No asumir que el grant del código cubre los pesos.

## Pathfinding y AI de movimiento

| Nombre | Repo | Licencia | Descripción | Stars |
|--------|------|----------|-------------|-------|
| recastnavigation | [recastnavigation/recastnavigation](https://github.com/recastnavigation/recastnavigation) | Zlib | Estándar de industria para navmesh. Usado en Unity, Godot, UE. C++. | 7.8k* |
| yuka | [Mugen87/yuka](https://github.com/Mugen87/yuka) | MIT | Game AI primitives en JavaScript: steering behaviors, pathfinding, FSM. | 1.4k* |
| aitoolkit | [linkdd/aitoolkit](https://github.com/linkdd/aitoolkit) | MIT | Cerebro para NPCs, header-only C++. | 524* |

El grant de `recastnavigation` está en **`License.txt`** (mayúscula L, minúscula t).
`raw.githubusercontent.com` es **case-sensitive**: una sonda que pida `LICENSE.txt`
devuelve 404 y la fila se publica como "sin licencia". Verificado: zlib, con las
tres cláusulas intactas.

## Generación procedural

| Nombre | Repo | Licencia | Descripción | Stars |
|--------|------|----------|-------------|-------|
| WorldX | [YGYOOO/WorldX](https://github.com/YGYOOO/WorldX) | MIT | Generación procedural de mundos con AI (TypeScript). | 1.1k* |
| OpenGame | [leigest519/OpenGame](https://github.com/leigest519/OpenGame) | Apache-2.0 | Framework agéntico para generar juegos web end-to-end con GameCoder-27B. ⚠️ **Última novedad en README: 2026-04-21** — seis meses sin anuncios. | 3.0k |

## ⚠️ Licencias y grants que bloquean uso comercial

Todo lo de esta tabla se leyó primera mano el 2026-10-26. **Ninguna de estas
filas es recomendable como base sin revisión legal.**

| Nombre | Repo | Grant declarado | Grant real verificado | Problema |
|--------|------|-----------------|----------------------|----------|
| unreal-mcp | [chongdashu/unreal-mcp](https://github.com/chongdashu/unreal-mcp) | Badge "License: MIT" en el README | **Ningún archivo de licencia en HEAD** (`LICENSE`, `LICENSE.md`, `LICENSE.txt`, `COPYING` → todos 404) | Un badge no es un grant. El repo se autodeclara `Status: Experimental`. Sin archivo de licencia, nadie puede otorgar permiso: legalmente es "all rights reserved". |
| local-llm-npc | [code-forge-temple/local-llm-npc](https://github.com/code-forge-temple/local-llm-npc) | **MIT** (así figuraba en `verticals/solutions.md` de este KB) | **CC-BY-4.0** (`LICENSE`, 18655 B, "Attribution 4.0 International") | Licencia de *contenido* aplicada a software: sin grant de patente, sin distinción fuente/objeto. **Corrección de este pass a una fila propia.** |
| Hunyuan-GameCraft | [Tencent-Hunyuan/Hunyuan-GameCraft-1.0](https://github.com/Tencent-Hunyuan/Hunyuan-GameCraft-1.0) | "open source" en prensa | Licencia propia Tencent (`LICENSE`, 16386 B), **no OSI** | Restricciones de uso/territorio típicas de las community licenses de Tencent. No es Apache/MIT. |
| Elements | [namazustudios/elements](https://github.com/namazustudios/elements) | "self-hosted game backend" | **AGPL-3.0** (`LICENSE.txt`, 16726 B) | Copyleft de red: hostear el backend para jugadores puede obligar a liberar el servidor. |
| Grafana | [grafana/grafana](https://github.com/grafana/grafana) | **Apache-2.0** (así figuraba en `repos/foundations.md` de este KB) | **AGPL-3.0** (`LICENSE`, 34523 B) | Idem. **Corrección de este pass a una fila propia.** |
| NobodyWho | — (Godot AssetLib) | EUPL-1.2 | EUPL-1.2 | Copyleft: derivados deben ser open source. |
| panel (Pelican) | [pelican-dev/panel](https://github.com/pelican-dev/panel) | — | **Sin payload de licencia en HEAD** | Fork de Pterodactyl sin grant verificable. Usar `pterodactyl/panel` (MIT, verificado). |
| Aivill | [SKYHUBDev/Aivill](https://github.com/SKYHUBDev/Aivill) | — | **Sin licencia y sin README en HEAD** | Repo vivo pero vacío. Estaba en `agents/trending.md` y en el radar de `intel/trends.md` como "concepto a observar": no hay nada que observar. |

## Candidatos evaluados y **rechazados** este pass

Se registran para que un pass futuro no vuelva a gastar la misma búsqueda.

| Candidato | Licencia | Por qué se rechaza |
|-----------|----------|--------------------|
| [balaraj74/openNPC](https://github.com/balaraj74/openNPC) | MIT (1077 B, verificado) | Apareció en búsqueda como "framework open source para NPCs autónomos". Primera mano: **5 ★, 4 commits, 1 fork**, el README se autodescribe "production-ready foundation" pero su comando de clonado sigue diciendo `yourusername`, y Actions no muestra tests. El grant es real; el proyecto no. |
| [lschiweck/LLM-NPC-Agents](https://github.com/lschiweck/LLM-NPC-Agents) | Apache-2.0 (11319 B, verificado) | Único candidato Apache-2.0 de NPC con voz en tiempo real, y **es un proyecto real** (55 commits, backend + cliente Unity, módulos STT/TTS/LLM, cita un paper). Pero 11 ★ y se autodeclara "Early Release": va a `agents/trending.md` como señal, **no** a la ruta recomendada. |
| `awesome-*` listas (3) | CC-BY-family | [lappemic/awesome-ai-built-games](https://github.com/lappemic/awesome-ai-built-games), [SubmitGame/awesome-ai-game-dev-tools](https://github.com/SubmitGame/awesome-ai-game-dev-tools), [JonTelep/awesome-ai-game-generation](https://github.com/JonTelep/awesome-ai-game-generation). Vivas y útiles como *canal de descubrimiento*, pero son listas bajo licencia de contenido: no son código sobre el cual construir. |

## Gaps declarados — canales que no se pudieron leer

Una ausencia medida vale más que un silencio. Este pass no pudo verificar:

- **`huggingface.co` → `getaddrinfo ENOTFOUND`.** DNS no resuelve en este entorno.
  Consecuencia concreta: la licencia de los **pesos** de Matrix-Game 4.0 y de los
  checkpoints de MineWorld queda **sin verificar**. Son la mitad del entregable de
  un world model.
- **`api.github.com/search/*` → rechazado** ("sessions are bound to their
  configured repositories"). No hay búsqueda por topic/estrellas desde aquí; el
  descubrimiento dependió de búsqueda web + lectura de páginas de repo, que
  sesga hacia lo que ya está indexado y escrito en inglés.
- **Star counts marcados con `*`** son los que este pass **no** releyó: vienen del
  pass 2026-07-02 y pueden estar desactualizados. Los sin asterisco se leyeron el
  2026-10-26. Tres de los releídos habían cambiado mucho: `godot-ai` 805 → **3.2k**,
  `limboai` 2.8k → **3.1k**, `nakama` 12.8k → **13.6k**.

## Defecto de instrumento encontrado en este pass

Se registra en lugar de corregirse en silencio, porque habría publicado datos
falsos: mi primer detector de licencias hacía `grep` de familias sobre **todo** el
cuerpo del archivo, con precedencia copyleft-primero. Eso **misgradúa los archivos
de licencia agregados**, que son comunes en engines y juegos:

| Repo | Veredicto del grep | Grant real (leído de la cabecera) |
|------|--------------------|----------------------------------|
| `love2d/love` | Apache-2.0 | **zlib** para LÖVE; `license.txt` (74927 B) agrega ENet/FreeType/GLAD y texto LGPL/GPL de terceros |
| `supertuxkart/stk-code` | AGPL-3.0 | **GPL-3.0-or-later** para el código; data en mezcla GPL / CC-BY / CC-BY-SA / dominio público |
| `0ad/0ad` | CC-family | **GPL-2.0** para código, **CC-BY-SA** para data |
| `luanti-org/luanti` | Apache-2.0 | **LGPL-2.1** para el engine (`doc/lgpl-2.1.txt`, 26406 B); `LICENSE.txt` es sólo texturas y sonidos (CC-BY-SA 3.0/4.0) |

Cuatro filas se habrían publicado con la licencia equivocada. Se detectó leyendo
las primeras líneas de cada archivo a mano. **Regla derivada:** en gaming, un
único `LICENSE` casi nunca cubre código *y* assets; hay que leer la cabecera y
separar las dos capas.

---
*8 filas nuevas, 4 correcciones de licencia sobre filas existentes, 8 grants
bloqueantes documentados, 3 candidatos rechazados con motivo, 3 gaps declarados.
Verificación: `git ls-remote` + lectura de licencia en HEAD, con control negativo
en ambos sentidos. 2026-10-26.*
