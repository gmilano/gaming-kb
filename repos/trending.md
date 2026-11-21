---
industry: gaming
region: Global
updated: 2026-11-21
---

# Repos trending — Gaming AI

> **Archivo APPEND-ONLY.** Cada pass agrega una sección fechada arriba y conserva
> la historia completa debajo.

---

## 🆕 2026-11-21

24 h desde el pass anterior: **pass de resolución de licencias y descubrimiento**,
no de medición de crecimiento. Canal: `gh api search/repositories` + lectura de
bytes por `raw.githubusercontent.com/<slug>/HEAD/<archivo>` con una lista de
**11 nombres de archivo** (ampliada hoy, ver abajo por qué).

### 🟢 Trending verificado este pass

| Repo | Licencia (archivo, bytes) | ★ | Qué es |
|------|---------------------------|---|--------|
| [msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents) | **MIT** | **169.646** (27.561 forks) | ~230 agentes por rol en 18 divisiones; **Game Development** es una de ellas, con Unity/Unreal/Godot/Blender/Roblox. El techo real de la categoría de orquestación. |
| [zenstory-ai/novel-to-game](https://github.com/zenstory-ai/novel-to-game) | **MIT** (`LICENSE`, 1081) | 1.175 | Skill-pack novela→juego: despiece del libro, diseño de mecánicas y arte, build, *blind-play*. **Targets de distribución APAC** (WeChat/Douyin/TapTap). |
| [MinLL/SkyrimNet-GamePlugin](https://github.com/MinLL/SkyrimNet-GamePlugin) | 🔴 **propia, no OSI** (`LICENSE`, 3745), **con `SkyrimNetApi.psc` MIT** | 386 | NPCs AI para Skyrim con voz en tiempo real, memoria y acción. Commit de hoy. |
| [fennaraOfficial/fennara-godot-ai](https://github.com/fennaraOfficial/fennara-godot-ai) | **MIT** (`LICENSE.md`, 1064) | 321 | Chat AI nativo en el editor de Godot + MCP. Commit de hoy. |
| [christos-antono/NPCTalk](https://github.com/christos-antono/NPCTalk) | **MIT** (`LICENSE`, 1078) | 232 | Voz para NPCs de cualquier juego: skills + servidor Fish Audio self-hosted. |
| [zoyluoblue/mc_aiplayer](https://github.com/zoyluoblue/mc_aiplayer) | **MIT** (`LICENSE`, 1063) | 141 | Agente autónomo que juega Minecraft (mod Fabric server-side, MC 1.21.3). |
| [Sagesheep/NarrativeEngine-P](https://github.com/Sagesheep/NarrativeEngine-P) | **MIT** (`LICENSE`, 1065) | 131 | Dungeon Master AI self-hosted; memoria de campaña larga. |
| [SummerEngine/summer](https://github.com/SummerEngine/summer) | **MIT** (`LICENSE`, 1070 + `NOTICE`, 1652) | 110 | Engine **construido para agentes**: MCP server + CLI + skills de build/verify. |
| [aigengame/godot-agent](https://github.com/aigengame/godot-agent) | **MIT** (`LICENSE`, 1066) | 58 | Build+verify de proyectos Godot por agente. |

### 📐 Los 16 badges `NOASSERTION`/`null` de esta KB, resueltos

Regla aplicada: la de la KB de **enterprise** (pase 16, T60) — *`NOASSERTION` es
la ausencia de un veredicto, no un veredicto; resolver leyendo bytes **en las dos
direcciones***. Hasta hoy esta KB lo trataba como *"detector de licencias
compuestas"*, que es la mitad correcta.

| Repo | Badge del API | Grant real leído en bytes | Dirección |
|------|---------------|---------------------------|-----------|
| [pterodactyl/panel](https://github.com/pterodactyl/panel) | NOASSERTION | **MIT** (`LICENSE.md`, 1137) — el `®` de *"Pterodactyl®"* en la línea de copyright rompe el match | 🟢 permisivo |
| [love2d/love](https://github.com/love2d/love) | NOASSERTION | **zlib** para LÖVE (`license.txt` **minúsculas**, 74.927 B: es un agregado de ~20 dependencias) | 🟢 permisivo |
| [AlanLaboratory/UnrealMLAgents](https://github.com/AlanLaboratory/UnrealMLAgents) | NOASSERTION | **Apache-2.0** (`LICENSE`, 609 B) — ⚠️ con *"All Rights Reserved."* **arriba** del grant: eso es lo que rompe al clasificador | 🟢 permisivo |
| [contentauth/c2pa-rs](https://github.com/contentauth/c2pa-rs) | NOASSERTION | 🆕 **MIT *o* Apache-2.0** (`LICENSE-MIT` 1083 + `LICENSE-APACHE` 11335, los dos presentes) | 🟢 **más** permisivo de lo publicado |
| [contentauth/c2pa-python](https://github.com/contentauth/c2pa-python) | Apache-2.0 | 🆕 **MIT *o* Apache-2.0** (idem, 1083 + 11335) | 🟢 **más** permisivo de lo publicado |
| [o3de/o3de](https://github.com/o3de/o3de) | NOASSERTION | Apache-2.0 *o* MIT (ya corregido ayer) | 🟢 permisivo |
| [Unity-Technologies/ml-agents](https://github.com/Unity-Technologies/ml-agents) | NOASSERTION | Apache-2.0 (`LICENSE.md`, 572 B puntero) | 🟢 permisivo |
| [supertuxkart/stk-code](https://github.com/supertuxkart/stk-code) | NOASSERTION | **GPL-3.0-or-later** (`COPYING`, **35.245 B** — cae en la banda GPL-3.0 ~35.200 del checksum de ayer) | 🔴 copyleft |
| [luanti-org/luanti](https://github.com/luanti-org/luanti) | NOASSERTION | **código LGPL-2.1+**, **texturas/sonidos CC-BY-SA 3.0**, Lua MIT — los tres en `LICENSE.txt` (8793 B) | 🔴 copyleft (código) |
| [0ad/0ad](https://github.com/0ad/0ad) | NOASSERTION | **GPL-2.0-or-later** + MIT/Apache-2.0 por subdirectorio (`LICENSE.txt`, 3049 B, agregado) · **ARCHIVADO** | 🔴 copyleft |
| [Tencent-Hunyuan/HY-World-2.0](https://github.com/Tencent-Hunyuan/HY-World-2.0) | NOASSERTION | 🔴 **Tencent Community License** (`License.txt`, 16.858 B) **que no se aplica en EU / UK / Corea del Sur** | 🔴 no OSI + territorial |
| [Tencent-Hunyuan/Hunyuan-GameCraft-1.0](https://github.com/Tencent-Hunyuan/Hunyuan-GameCraft-1.0) | NOASSERTION | Licencia comunitaria Tencent, no OSI (sin commits desde 2025-11-28) | 🔴 no OSI |
| [electricsheephq/WorldOS](https://github.com/electricsheephq/WorldOS) | NOASSERTION | **"WorldOS Source-Available Commercial EULA v1.0"** (`LICENSE`, 10.598 B), que **se autodeclara** *"not OSI open source"* con `SPDX-License-Identifier: LicenseRef-WorldOS-Source-Available-Commercial-1.0` | 🔴 source-available |
| [AnkleBreaker-Studio/unity-mcp-server](https://github.com/AnkleBreaker-Studio/unity-mcp-server) · [plugin](https://github.com/AnkleBreaker-Studio/unity-mcp-plugin) | NOASSERTION | **"AnkleBreaker Open License v1.0"** (3068 B): atribución obligatoria en el producto + **prohibición de reventa** | 🔴 restricción de campo de uso |
| [MonoGame/MonoGame](https://github.com/MonoGame/MonoGame) | NOASSERTION | **Ms-PL** (`LICENSE.txt`, 3882 B, cabecera leída: *"Microsoft Public License (Ms-PL)"*) — confirma el hallazgo de ayer **por lectura directa**, no sólo por checksum | 🟡 compuesto binario/fuente |
| [PostHog/posthog](https://github.com/PostHog/posthog) | NOASSERTION | MIT Expat + carve-out `ee/` (confirmado) | 🟡 carve-out |
| [chongdashu/unreal-mcp](https://github.com/chongdashu/unreal-mcp) · [SKYHUBDev/Aivill](https://github.com/SKYHUBDev/Aivill) · [mannychen0225/Agent-Hive-Workbench](https://github.com/mannychen0225/Agent-Hive-Workbench) | `null` | **ningún archivo de licencia en ninguna de las 11 variantes** | ⚫ sin grant |

**Conteo: 7 permisivos, 8 restrictivos, 3 sin grant.** Un `NOASSERTION` de esta
KB tiene ~44% de probabilidad de ser permisivo. ⇒ **no es evidencia**, en ninguna
dirección, y no puede usarse como filtro.

### 🧪 Defecto de instrumento encontrado dentro de este pass

La primera sonda de licencia de hoy pedía 9 nombres de archivo y **falló en
`Tencent-Hunyuan/HY-World-2.0`**, cuyo grant está en `License.txt`. El pass
anterior había cometido **el mismo fallo** y publicó *"ningún archivo de
licencia"* en 4 archivos de esta KB — a pesar de que esta misma KB **ya había
documentado el trap** para `recastnavigation/License.txt`.

> **La lección no estaba en el lugar donde se usa.** Estaba en prosa, en una
> sección sobre pathfinding, y la lista de sondas no la incorporó. Corregido: la
> lista es ahora
> `LICENSE LICENSE.txt LICENSE.md License.txt license.txt LICENCE COPYING COPYING.txt LICENSE-MIT LICENSE-APACHE NOTICE`
> y **5 de esas variantes aparecen realmente** en los repos de esta KB
> (`License.txt`, `license.txt`, `COPYING`, `LICENSE.md`, `LICENSE-MIT`+`LICENSE-APACHE`).
> Una sonda de un solo nombre falla en **5 de 16** repos.

### 📋 Censo de verificación de este pass

- **83 URLs heredadas** re-consultadas en `repo:`: 82 resuelven, 1 `NOT_FOUND`
  esperado (`JonTelep/awesome-ai-game-generation`, rename ya documentado ayer).
  **0 renames nuevos, 0 repos archivados nuevos, 0 desapariciones nuevas.**
- **14 candidatos nuevos** evaluados: 9 admitidos, 4 rechazados con motivo
  (`Agent-Hive-Workbench` sin grant, `lore-weave` AGPL-3.0, `PtcgDeckAgent`
  alcance estrecho, `Awesome-agent-cases` es lista), 1 promovido a tabla de
  bloqueantes (`SkyrimNet`, admitido **como integración**, no como fork).
- **16 badges** `NOASSERTION`/`null` resueltos por bytes (tabla arriba).
- Control negativo del canal: `repo:gmilano/control-does-not-exist-xyz-999` →
  HTTP 422. ✅

---

## 🆕 2026-11-20

25 días desde el pass anterior. **Canal recuperado:** el pass de octubre registró
`api.github.com/search/*` como bloqueado ("sessions are bound to their configured
repositories"); **no lo está** con `gh api -X GET search/repositories -f
q='repo:<slug>'`. Eso da ★ + SPDX + `archived` + `pushed_at` + rama default en una
llamada, y es lo que permitió releer **el 100% de las ★ de esta KB** (el pass
anterior tuvo que dejar ~20 filas con `*` heredado de julio).

### 🟢 Trending verificado este pass

| Repo | Licencia | ★ | Señal fechada |
|------|----------|---|---------------|
| [Donchitos/Claude-Code-Game-Studios](https://github.com/Donchitos/Claude-Code-Game-Studios) | **MIT** (`LICENSE`, 1066) | **40.377** | **Repo nº1 de game-development del trending de GitHub** a principios de nov-2026. 49 agentes con rol + 72 skills de workflow. Trayectoria medida: 34,7k (3-nov) → 40,2k (lista mensual) → **40.377 (hoy)**. |
| [horizon-rust/horizon](https://github.com/horizon-rust/horizon) | 🔴 **LGPL-3.0** | **11.515** | Reescritura de Minecraft Java Edition en Rust. Commit de hoy. **Está en las listas de trending de game-dev y NO es permisivo** — registrado aquí precisamente para que un pass futuro no lo adopte por aparecer en una lista. |
| [a16z-infra/ai-town](https://github.com/a16z-infra/ai-town) | **MIT** | **10.835** | Starter kit desplegable de pueblo de agentes. Commit de hoy. **Hueco histórico de esta KB**, que tenía el paper de Stanford (parado 27 meses) pero no la versión que se despliega. |
| [rehan-remade/universal-modder](https://github.com/rehan-remade/universal-modder) | **MIT** | **9.809** | ★ **ahora verificada** (en octubre era `~8,9k*` sin verificar). Commit de hoy. |
| [agones-dev/agones](https://github.com/agones-dev/agones) | **Apache-2.0** | **7.106** | Orquestación de game servers dedicados sobre **Kubernetes** (`Fleet`/`GameServer`/`FleetAutoscaler` como CRDs). Commit de hoy. **Ausente de esta KB desde el primer pass**, que sólo tenía panel bare-metal. 🔴 **Renombrado:** `googleforgames/agones` ya no es canónico. |
| [Coding-Solo/godot-mcp](https://github.com/Coding-Solo/godot-mcp) | **MIT** | **6.713** | **15× el godot-mcp que esta KB listaba.** ⚠️ **7 meses sin commits** (2026-04-16): líder por adopción, dormido por mantenimiento. |
| [EL4CTEO/blender-skills](https://github.com/EL4CTEO/blender-skills) | **MIT** | 2.529 | Workflows de Blender empaquetados como **skills de agente**. Cubre el hueco de arte 3D. |
| [WhiteTowerAI/ohmygame](https://github.com/WhiteTowerAI/ohmygame) | **Apache-2.0** (`LICENSE`, 11357) | **2.486** | **+107% en 25 días** (1,2k → 2.486), el mayor crecimiento relativo del pass. Commit de hoy. |
| [Tencent-Hunyuan/HY-World-2.0](https://github.com/Tencent-Hunyuan/HY-World-2.0) | 🔴 **ningún archivo de licencia** | 2.891 | World model 3D multimodal de Tencent. Se autodenomina "open-source SOTA" y **no publica grant** (`LICENSE`/`.txt`/`.md` → 404). Sin commits desde 2026-08-12. |
| [genex-games/genex-desktop](https://github.com/genex-games/genex-desktop) | **MIT** | 900 | Desktop app para hacer juegos con AI sobre Unreal/Unity/Godot/browser. ⚠️ rama default **`dev`**. Commit de hoy. |

### 🔴 Correcciones de licencia sobre repos fundacionales ya listados

El checksum de bytes (ver `agents/top.md`) marcó 4 filas de esta KB y **las 4
estaban mal, sin una sola falsa alarma.** Dos son fundacionales:

| Repo | Decía la KB | Es | ★ |
|------|-------------|-----|---|
| [MonoGame/MonoGame](https://github.com/MonoGame/MonoGame) | MIT (`LICENSE.txt`, 3882) | **Ms-PL** — §3(D) obliga a que **la distribución en fuente vaya bajo Ms-PL**; no relicenciable a MIT/Apache | 14.663 |
| [PostHog/posthog](https://github.com/PostHog/posthog) | MIT (`LICENSE`, 1563) | **MIT Expat + carve-out `ee/`** (PostHog Enterprise License, 2176 B) — mismo patrón literal que Onyx en la KB de enterprise | 40.990 |
| [o3de/o3de](https://github.com/o3de/o3de) | Apache-2.0 (`LICENSE.txt`, 1475) | **Apache-2.0 *o* MIT a elección** (más permisivo de lo que decíamos) **pero con Qt bajo LGPL-3.0** entre sus componentes, que no mencionábamos | 9.894 |
| [NamazuStudios/elements](https://github.com/NamazuStudios/elements) | AGPL-3.0 (`LICENSE.txt`, 16726) → **rechazado** | **MPL-2.0** — copyleft por archivo. **El rechazo era incorrecto.** | 22 |

### 📈 Las ★ que el canal recuperado destrabó

Filas congeladas con `*` desde julio, ahora medidas:

| Repo | Anotado | Real 2026-11-20 | Δ |
|------|---------|-----------------|---|
| [godotengine/godot](https://github.com/godotengine/godot) | 97k* | **120.483** | +24% |
| [supabase/supabase](https://github.com/supabase/supabase) | 80k* | **115.201** | +44% |
| [PostHog/posthog](https://github.com/PostHog/posthog) | 23k* | **40.990** | +78% |
| [grafana/grafana](https://github.com/grafana/grafana) | 67k* | **78.014** | +16% |
| [MonoGame/MonoGame](https://github.com/MonoGame/MonoGame) | 11k* | **14.663** | +33% |
| [love2d/love](https://github.com/love2d/love) | 4,5k* | **8.981** | +100% |
| [edbeeching/godot_rl_agents](https://github.com/edbeeching/godot_rl_agents) | 900+* | **1.639** | +82% |
| [o3de/o3de](https://github.com/o3de/o3de) | 7,8k* | **9.894** | +27% |
| [recastnavigation/recastnavigation](https://github.com/recastnavigation/recastnavigation) | 7,8k* | **8.203** | +5% |
| [luanti-org/luanti](https://github.com/luanti-org/luanti) | — | **13.882** | commit de hoy |

### 🔴 Repos que murieron o se congelaron — medido, no supuesto

`pushed_at` es el dato que esta KB no estaba usando y que cambia recomendaciones:

| Repo | ★ | Último commit | Lectura |
|------|---|---------------|---------|
| [0ad/0ad](https://github.com/0ad/0ad) | 2.827 | 2024-08-17 | 🔴 **`archived: true`.** Descripción propia: *"[DEPRECATED] Git mirror of the 0 A.D. SVN source code (migrated to gitea.wildfiregames.com on August 20th 2024)"*. Esta KB lo listaba como base para fork+AI. **El juego está vivo; su presencia en GitHub está muerta.** |
| [joonspk-research/generative_agents](https://github.com/joonspk-research/generative_agents) | 22.399 | 2024-08-05 | 27 meses. Artefacto de paper, no base mantenida. |
| [lmgame-org/GamingAgent](https://github.com/lmgame-org/GamingAgent) | 1.006 | 2025-11-16 | **Un año exacto.** Único benchmark publicado del nicho, pero congelado. |
| [linkdd/aitoolkit](https://github.com/linkdd/aitoolkit) | 533 | 2024-05-08 | 2,5 años. Sin mantenimiento. |
| [chongdashu/unreal-mcp](https://github.com/chongdashu/unreal-mcp) | 2.120 | 2025-04-22 | 19 meses **y sin archivo de licencia**. |
| [Coding-Solo/godot-mcp](https://github.com/Coding-Solo/godot-mcp) | 6.713 | 2026-04-16 | 7 meses. Líder de adopción, dormido. |
| [pelican-dev/panel](https://github.com/pelican-dev/panel) | — | — | **Ya no existe**: `NOT_FOUND` + `raw` 404. |

> **Regla derivada para la rotación:** una fila de KB necesita **tres** datos, no
> uno. ★ mide interés acumulado (es un **stock**, nunca baja y no caduca),
> `pushed_at` mide si el proyecto está vivo (es un **flujo**), y `archived` es un
> booleano que los dos anteriores no revelan: **`0ad/0ad` tiene 2.827★ y está
> archivado**. Recomendar por ★ sin `pushed_at` es recomendar el pasado.

### 🟢 Infra de descubrimiento

| Repo | Licencia | ★ | Nota |
|------|----------|---|------|
| [raiscui/gh-trends](https://github.com/raiscui/gh-trends) | MIT | **0** | El feed de trending que esta KB usa para fechar señales (releases diarios, commit de hoy). **Citarlo no es avalarlo:** 0★, es un scraper personal. Útil como instrumento, no como autoridad. |
| [NeuraLiying/Awesome-World-Models](https://github.com/NeuraLiying/Awesome-World-Models) | CC-BY-family | — | 340+ papers de world models. Canal de descubrimiento para la tier de world models; licencia de contenido, no código. |

---

## 2026-10-26

Primer pass desde el 2026-07-02. Verificación: `git ls-remote` + lectura del
archivo de grant en `raw.githubusercontent.com/<slug>/HEAD/`, con control negativo
en ambos sentidos. El `curl -sI` que prescribía el método anterior **devuelve 403
para todo repo de GitHub, incluido uno fabricado**, así que no distingue vivos de
muertos y no se usó.

### 🟢 Trending verificado esta semana

| Repo | Licencia (archivo, bytes) | ★ | Señal fechada |
|------|--------------------------|---|---------------|
| [SkyworkAI/Matrix-Game](https://github.com/SkyworkAI/Matrix-Game) | raíz **MIT** (1118) / `Matrix-Game-4/` **Apache-2.0** (11357) | 2.8k | **v4.0 liberado el 15-oct-2026**; technical report el 16-oct (arXiv 2610.27999); inferencia optimizada para RTX 5080 / 5070 Ti el 16-oct. |
| [WhiteTowerAI/ohmygame](https://github.com/WhiteTowerAI/ohmygame) | Apache-2.0 (11357) | 1.2k | **En el trending de GitHub del 2026-10-26** (release `daily-2026-10-26` de `raiscui/gh-trends`). Estudio de un solo dev con canvas de juego y BYO key. |
| [lmgame-org/GamingAgent](https://github.com/lmgame-org/GamingAgent) | MIT (1065) | 998 | **ICLR 2026.** Benchmark + harness de agentes que juegan, con leaderboard público. |
| [IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP) | Apache-2.0 (10197) | **4.5k** | El MCP de engine dominante. Faltaba por completo en esta KB. |
| [microsoft/MineWorld](https://github.com/microsoft/MineWorld) | MIT (1141) | 493 | World model Minecraft en tiempo real, checkpoints 300M/700M/1200M. |
| [tomyud1/godot-mcp](https://github.com/tomyud1/godot-mcp) | MIT (1071) | 444 | Segundo MCP real del ecosistema Godot. |
| [rehan-remade/universal-modder](https://github.com/rehan-remade/universal-modder) | MIT (1096) | ~8.9k* | Modding asistido por AI. |

### 🆕 Repos de infraestructura que faltaban en esta KB

No son "trending" por estrellas; son piezas de stack que el pass anterior no tenía.

| Repo | Licencia (archivo, bytes) | ★ | Qué resuelve |
|------|--------------------------|---|--------------|
| [TaloDev/backend](https://github.com/TaloDev/backend) | MIT (`LICENSE`, 1074) | 104 | Backend de juego self-hostable **MIT completo, sin tier cerrado**: auth, stats, leaderboards, saves, event tracking, exports CSV, canales realtime, live config, presencia, relaciones. Integra Steamworks / Google Play Games / Apple Game Center. Paquetes Godot y Unity. |
| [pterodactyl/panel](https://github.com/pterodactyl/panel) | MIT (`LICENSE.md`, 1137) | — | El panel de game servers open source más usado. La capa de **ops** que no existía en esta KB. |
| [contentauth/c2pa-python](https://github.com/contentauth/c2pa-python) | Apache-2.0 (`LICENSE-APACHE`, 11335) | — | Content Credentials en Python: es la pieza concreta para el marcado **machine-readable** que el Art. 50(2) del EU AI Act exige para el **2-dic-2026**. |
| [contentauth/c2pa-rs](https://github.com/contentauth/c2pa-rs) | Apache-2.0 (`LICENSE-APACHE`, 11335) | — | SDK de referencia del mismo estándar. |
| [Farama-Foundation/PettingZoo](https://github.com/Farama-Foundation/PettingZoo) | MIT (`LICENSE`, 1073) | 3.5k* | Equivalente multi-agente de Gymnasium: NPCs que compiten o cooperan. |

### 🔴 Correcciones de licencia sobre filas que esta KB ya publicaba

Cuatro grants estaban mal en los archivos de este repositorio. Todos releídos
primera mano el 2026-10-26:

| Repo | Esta KB publicaba | Grant real verificado | Consecuencia |
|------|-------------------|----------------------|--------------|
| [grafana/grafana](https://github.com/grafana/grafana) | **Apache-2.0** (`repos/foundations.md`) | **AGPL-3.0** (`LICENSE`, 34523 B) | Copyleft de red: un dashboard expuesto a terceros arrastra obligaciones. Alternativas: Apache Superset (Apache-2.0), Metabase. |
| [code-forge-temple/local-llm-npc](https://github.com/code-forge-temple/local-llm-npc) | **MIT** (`verticals/solutions.md`, en la ruta recomendada) | **CC-BY-4.0** (`LICENSE`, 18655 B, "Attribution 4.0 International") | Licencia de contenido sobre software: sin grant de patente, sin distinción fuente/objeto. Baja de ruta recomendada a referencia de implementación. |
| [SKYHUBDev/Aivill](https://github.com/SKYHUBDev/Aivill) | licencia `—`, "concepto a observar" | **Sin licencia y sin README en HEAD** | Repo vivo y vacío. Retirado del radar. |
| [minetest/minetest](https://github.com/minetest/minetest) → [luanti-org/luanti](https://github.com/luanti-org/luanti) | LGPL-2.1, sin matices | **LGPL-2.1 el engine** (`doc/lgpl-2.1.txt`, 26406 B) + **CC-BY-SA 3.0/4.0 las texturas y sonidos** (`LICENSE.txt`, 8793 B) | El slug canónico cambió y el `LICENSE.txt` del repo **no** es el del código. |

### 🧪 Defecto de instrumento encontrado dentro de este pass

Se registra en lugar de corregirse en silencio, porque habría publicado cuatro
filas con la licencia equivocada. Mi primer detector hacía `grep` de familias de
licencia sobre **todo** el cuerpo del archivo con precedencia copyleft-primero. Eso
misgradúa los **archivos de licencia agregados**, que en gaming son la norma porque
el código y los assets casi nunca comparten grant:

| Repo | Veredicto del grep | Grant real (leído de la cabecera) |
|------|--------------------|----------------------------------|
| `love2d/love` | Apache-2.0 | **zlib** para LÖVE; `license.txt` (74927 B) agrega ENet/FreeType/GLAD y texto LGPL/GPL de terceros |
| `supertuxkart/stk-code` | AGPL-3.0 | **GPL-3.0-or-later** el código; data en mezcla GPL / CC-BY / CC-BY-SA / dominio público |
| `0ad/0ad` | CC-family | **GPL-2.0** el código, **CC-BY-SA** la data |
| `luanti-org/luanti` | Apache-2.0 | **LGPL-2.1** el engine; `LICENSE.txt` es sólo texturas y sonidos |

Se detectó leyendo las primeras líneas de cada archivo a mano. **Regla derivada y
reutilizable:** en gaming, antes de publicar una licencia hay que leer la cabecera
del archivo y separar la capa de código de la capa de assets.

Un segundo detalle del mismo tipo: `raw.githubusercontent.com` es
**case-sensitive**. El grant de `recastnavigation` vive en **`License.txt`** (L
mayúscula, t minúscula); una sonda que pida `LICENSE.txt` recibe 404 y publica la
fila como "sin licencia".

### ⛔ Descartados de la ruta construible

| Repo | Motivo verificado |
|------|------------------|
| [pelican-dev/panel](https://github.com/pelican-dev/panel) | Fork de Pterodactyl citado en comparativas 2026. **Sin ningún archivo de licencia en HEAD.** Usar `pterodactyl/panel` (MIT). |
| [namazustudios/elements](https://github.com/namazustudios/elements) | **AGPL-3.0** (`LICENSE.txt`, 16726 B): copyleft de red sobre el servidor de juego. |
| [chongdashu/unreal-mcp](https://github.com/chongdashu/unreal-mcp) | Badge MIT en el README, **cero archivos de licencia en HEAD**, `Status: Experimental`. |
| [Tencent-Hunyuan/Hunyuan-GameCraft-1.0](https://github.com/Tencent-Hunyuan/Hunyuan-GameCraft-1.0) | Licencia community de Tencent (16386 B), **no OSI**. |
| `awesome-*` (3 listas) | [lappemic/awesome-ai-built-games](https://github.com/lappemic/awesome-ai-built-games), [SubmitGame/awesome-ai-game-dev-tools](https://github.com/SubmitGame/awesome-ai-game-dev-tools), [JonTelep/awesome-ai-game-generation](https://github.com/JonTelep/awesome-ai-game-generation). Vivas y útiles como **canal de descubrimiento**, pero bajo licencia de contenido (CC-BY family): no son código sobre el cual construir. |

### 📋 Censo de verificación de este pass

- **54 repos probados** con el procedimiento de dos etapas.
- **52 vivos**, 2 inalcanzables (`Skywork-AI/Matrix-Game` — el slug correcto es
  `SkyworkAI/Matrix-Game`; `TaloDev/talo` — el slug correcto es `TaloDev/backend`).
- **1 control negativo fabricado**, correctamente inalcanzable.
- **4 repos vivos sin ningún payload de licencia** en HEAD: `chongdashu/unreal-mcp`,
  `pelican-dev/panel`, `SKYHUBDev/Aivill`, y `recastnavigation` sólo hasta corregir
  la capitalización del nombre de archivo.
- **Canales bloqueados:** `api.github.com/search/*` (rechazado por scoping de sesión)
  y `huggingface.co` (DNS no resuelve).

---

# Historial

Todo lo que sigue es el contenido de los passes anteriores, sin modificar.

---

# Repos trending — Gaming AI

> Señales automáticas del pipeline + análisis manual. Última actualización: 2026-07-02

## Trending con tracción real (verificado manualmente)

| Repo | Licencia | Descripción | Stars | Señal |
|------|----------|-------------|-------|-------|
| [hi-godot/godot-ai](https://github.com/hi-godot/godot-ai) | MIT | MCP server para Godot. 120+ ops, 41 tools. Conecta Claude Code/Cursor al editor Godot en vivo. | 805 | Lanzado abr 2026, adopción masiva |
| [leigest519/OpenGame](https://github.com/leigest519/OpenGame) | MIT | Framework agentico end-to-end para generar juegos web. Usa GameCoder-27B (LLM RL-entrenado). | ~500 | Primer framework de su tipo, abr 2026 |
| [AlanLaboratory/UnrealMLAgents](https://github.com/AlanLaboratory/UnrealMLAgents) | Apache-2.0 | Port de Unity ML-Agents para Unreal Engine. Deep RL para juegos UE. v1.0 en 2025. | — | Comunidad Unreal adoptando RL |
| [pamirtuna/gamestudio-subagents](https://github.com/pamirtuna/gamestudio-subagents) | MIT | Equipo de sub-agentes AI en terminal: dev, QA, artist, game designer. | — | Patrón "AI dev team" aplicado a gaming |
| [Yuan-ManX/ai-game-devtools](https://github.com/Yuan-ManX/ai-game-devtools) | MIT | Hub curado: LLMs, world models, textures, shaders, 3D, audio, analytics para game dev. | — | Lista de referencia más completa del ecosistema |
| [undreamai/LLMUnity](https://github.com/undreamai/LLMUnity) | Apache-2.0 | Personajes Unity con LLMs locales o cloud. | 1.7k | Creciendo en Unity ecosystem |
| [NPC-Worldwide/npcpy](https://github.com/NPC-Worldwide/npcpy) | MIT | Librería Python para NPCs multimodales con LLMs. | 1.4k | Tracción en comunidad Python/AI |
| [Yuan-ManX/AI-Native-Game](https://github.com/Yuan-ManX/AI-Native-Game) | MIT | Tracking de juegos donde la AI ES el juego (world models, generative environments). | — | Categoría emergente: AI-native games |
| [stride3d/stride](https://github.com/stride3d/stride) | MIT | Engine C# cross-platform (ex-Xenko). Alternativa a Unity para devs .NET. | 7.7k | Engine maduro, alternativa Unity sin drama |
| [SKYHUBDev/Aivill](https://github.com/SKYHUBDev/Aivill) | — | Villanos adaptativos que aprenden el comportamiento del jugador y evolucionan estrategias. | 0 | Concept muy relevante, observar |

## Engines en radar

| Repo | Licencia | Descripción | Stars | Por qué |
|------|----------|-------------|-------|---------|
| [oxylusengine/Oxylus](https://github.com/oxylusengine/Oxylus) | Apache-2.0 | Game engine C++ data-driven con Vulkan. | 263 | Moderno, base sólida para experimentación |
| [electricsheephq/WorldOS](https://github.com/electricsheephq/WorldOS) | Custom | Simulación de mundos + evolución de juegos. | 2 | Concepto world model temprano |

## Frameworks agenticos con adopción en gaming

| Repo | URL | Licencia | Stars | Aplicación gaming |
|------|-----|----------|-------|-------------------|
| **LangChain** | [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | MIT | ~100k | Orquestar LLMs con tools, memoria y RAG. Base para NPCs con contexto y chatbots de soporte. |
| **LlamaIndex** | [run-llama/llama_index](https://github.com/run-llama/llama_index) | MIT | ~40k | RAG sobre datos propios. Para NPCs con conocimiento del lore del juego. |
| **CrewAI** | [crewAIInc/crewAI](https://github.com/crewAIInc/crewAI) | MIT | ~30k | Multi-agente con roles. Para QA pipelines, PCG con agentes especializados. |

## Señales de mercado a seguir

- **Roblox OpenGameEval** (dic 2025): benchmark OSS para evaluar AI en juegos. 47 escenarios: code gen, tool use, 3D reasoning, multiplayer. Adoptado como estándar de evaluación.
- **Unity AI Inference Engine**: permite correr LLMs/modelos ML directamente en el runtime Unity sin servidor. Tendencia hacia AI on-device.
- **NVIDIA ACE SDK**: SDK para Unreal/Unity. NPCs con ASR + NLP + TTS + animación facial en tiempo real. Tracción en demos AAA.
- **Gemma 3n on-device**: demostrado en Godot (code-forge-temple/local-llm-npc), posibilita NPCs offline-first.

---
*Pipeline automático actualiza señales; análisis manual verifica relevancia. 2026-07-02*
