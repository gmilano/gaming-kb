---
industry: gaming
region: Global
updated: 2026-11-21
---

# Agentes trending — Gaming AI

> **Archivo APPEND-ONLY.** Cada pass agrega una sección fechada arriba y conserva
> la historia completa debajo. La historia es el dato: sin ella no hay tendencia,
> sólo una foto.

---

## 🆕 2026-11-21 — pass cycle 6 (≈1 h después del cycle 5)

> ⏱️ **Advertencia de instrumento, y es la entrada más importante de esta
> sección: una hora no es un intervalo de medición.** Ninguna ★ se mueve de
> forma interpretable en 60 minutos. **Este pass no reporta un solo delta**, y
> no reescribe ninguna cifra heredada. Un archivo append-only tiene una tentación
> propia —anotar algo cada vez que corre— y la forma de resistirla es anotar
> explícitamente **que no hubo nada que medir**. Lo que sí cambió en una hora es
> el **instrumento**: hay un canal de verificación nuevo.

**Canal nuevo (el cambio real del pass):**
`gh api search/code -f q='repo:<slug> filename:LICENSE'` **enumera** los archivos
de licencia de un repo de terceros. Los passes del 2026-11-20 y del 2026-11-21
cycle 5 declararon este gap como *"CONFIRMADO sin canal"* dos veces seguidas,
porque los dos probaron `gh api repos/<o>/<r>/contents` (bloqueado) y ninguno
probó `search/code`. **Regla: "sin canal" significa "sin los canales que
probamos", y hay que escribir *cuáles*.**

### Agentes y packs nuevos, verificados hoy (★ de hoy, sin delta por lo dicho arriba)

| Nombre | Repo | Licencia (bytes) | ★ | Forks |
|--------|------|------------------|---|-------|
| **awesome-gamedev-agent-skills** | [gamedev-skills/awesome-gamedev-agent-skills](https://github.com/gamedev-skills/awesome-gamedev-agent-skills) | **Apache-2.0** (11395) + `NOTICE` (883) | **9.929** | 1.013 |
| **coop-anything** | [cranebeaker/coop-anything](https://github.com/cranebeaker/coop-anything) | **MIT** (1085) | 2.185 | 298 |
| **Gamer** (VLA) | [RainBowLuoCS/Gamer](https://github.com/RainBowLuoCS/Gamer) | **Apache-2.0** (11358) | 1.189 | 23 |
| **Game-Dev-Agentic-Skills** | [thedivergentai/Game-Dev-Agentic-Skills](https://github.com/thedivergentai/Game-Dev-Agentic-Skills) | 🔴 **LGPL-3.0** (7652) | 983 | 62 |
| **bot-crossing** | [Station-Sciences/bot-crossing](https://github.com/Station-Sciences/bot-crossing) | **MIT** (1069) | 906 | 286 |
| **DanKS** | [Calix-L/DanKS](https://github.com/Calix-L/DanKS) | **Apache-2.0** (11348) | 833 | 36 |
| **GameScaling** | [THUSI-Lab/GameScaling](https://github.com/THUSI-Lab/GameScaling) | **Apache-2.0** — 🟢 **pesos también (Qwen3.5)** | 604 | 35 |
| **OpenLiberty** | [openfw-game/OpenLiberty](https://github.com/openfw-game/OpenLiberty) | **MIT** en **`LICENCE`** (1066) | 493 | 46 |
| **guizang-game-skill** | [op7418/guizang-game-skill](https://github.com/op7418/guizang-game-skill) | 🔴 **AGPL-3.0** (34524) | 431 | 71 |
| **awesome-ai-for-games** | [Eurekaleo/awesome-ai-for-games](https://github.com/Eurekaleo/awesome-ai-for-games) | **MIT** (1082) | 391 | 17 |

### 🔴 La tendencia de licencia que esta página venía contando al revés

Los passes del 20 y del 21 abrieron la tier de orquestación y la celebraron como
*"catálogos de agentes MIT, gratis, con 27k forks"*. Medido sobre los packs
**nuevos** de la misma tier con >400★:

| Grant | Repos | ★ sumadas |
|-------|-------|-----------|
| 🟢 Permisivo (MIT/Apache-2.0) | 3 | 13.020 |
| 🔴 **Copyleft (LGPL-3.0 / AGPL-3.0 ×2)** | **3** | **2.023** |

**3 de 6.** La tier sigue estando dominada en volumen por lo permisivo, pero
**la mitad de los repos nuevos no lo es**, y eso es lo contrario de lo que esta
página comunicaba. El activo viral de gaming ahora es el **pack de skills** —
markdown y prompts, lo más fácil de copiar accidentalmente dentro del repo de un
cliente. Un `.md` de instrucciones bajo AGPL-3.0 pegado en un entregable arrastra
la obligación de red sobre **lo que lo ejecute**. Ver Receta 16.

### 🟢 Señal de procedencia: un `NOTICE` como activo comercial

`awesome-gamedev-agent-skills` (9.929★) trae un `NOTICE` de 883 B que declara que
**todas las skills son obra original escrita desde documentación primaria** y que
*"no text or code was copied"* de otras colecciones, más un disclaimer de marcas
de los 11 engines que nombra. En una industria con la ansiedad de IP que tiene
esta —ver `intel/trends.md` nº 1 y nº 12— **una atestación de procedencia escrita
por el autor es algo que se le puede mostrar al cliente**. Apache-2.0 §4(d)
obliga a propagarla, así que llevarla no es un costo: es el entregable.

### ⚫ Rechazados hoy, con motivo

| Candidato | Medido | Motivo |
|-----------|--------|--------|
| [AIGeeksGroup/Code2Games](https://github.com/AIGeeksGroup/Code2Games) | 89★, SPDX `NONE`, 11 sondas → 404 | **Sin grant.** Apareció en snippets como trending de generación de mundos. |
| [AlterLab-IEU/AlterLab_GameForge](https://github.com/AlterLab-IEU/AlterLab_GameForge) | 57★, MIT, 15 días sin commits | **"Rank #1 de un día" en un tracker no es tracción.** Señal, no ruta. |
| `jaylfc/taOS` | 609★, AGPL-3.0 | Menciona *gaming PC* sólo como hardware de cluster. Falso positivo de query; contó sólo en el reparto de licencias. |
| `shinkuan/Akagi` | 1.151★, Apache-2.0, rama default **`v3`** | Asistente de Mahjong online a un jugador humano, no game dev. Se anota la 4ª rama default no-`main`/`master` de la KB. |

---

## 2026-11-21 — pass cycle 5

Un día desde el pass anterior, así que **este pass no mide crecimiento: resuelve
licencias y descubre.** Las ★ de abajo se releyeron hoy (`repo:` sobre las 83
URLs de la KB + 14 candidatos nuevos); los deltas de 24 h se reportan sólo donde
son grandes en términos relativos.

### 🔴 La categoría de orquestación tiene un techo 4× más alto del que publicamos ayer

Ayer esta KB abrió la tier *"orquestación de agentes para game dev"* y la coronó
con `Donchitos/Claude-Code-Game-Studios` (40.485★, MIT, 5.305 forks), leyéndolo
como *"la demanda es un pipeline de estudio con roles"*. La lectura era correcta;
**el techo estaba mal medido**.

| Repo | ★ | Forks | Licencia | Alcance de gaming |
|------|---|-------|----------|-------------------|
| 🆕 [msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents) | **169.646** | **27.561** | MIT | **1 división de 18** (Game Development: cross-engine + Unity + Unreal + Godot + Blender + Roblox) |
| [Donchitos/Claude-Code-Game-Studios](https://github.com/Donchitos/Claude-Code-Game-Studios) | 40.485 | 5.305 | MIT | 100% gaming (49 agentes + 72–75 skills) |
| [WhiteTowerAI/ohmygame](https://github.com/WhiteTowerAI/ohmygame) | 2.571 (+85 en 24 h, **+3,4%/día**) | 313 | Apache-2.0 | 100% gaming |
| [EL4CTEO/blender-skills](https://github.com/EL4CTEO/blender-skills) | 2.596 (+67 en 24 h) | 250 | MIT | arte 3D |
| 🆕 [zenstory-ai/novel-to-game](https://github.com/zenstory-ai/novel-to-game) | 1.175 | — | MIT | 100% gaming, **pipeline novela→juego**, targets WeChat/Douyin/TapTap |
| [genex-games/genex-desktop](https://github.com/genex-games/genex-desktop) | 902 | 110 | MIT | 100% gaming |
| 🆕 [SummerEngine/summer](https://github.com/SummerEngine/summer) | 110 | 10 | MIT | engine **diseñado** para agentes |
| 🆕 [aigengame/godot-agent](https://github.com/aigengame/godot-agent) | 58 | — | MIT | build+verify de Godot por agente |

**El repo de agentes por rol más grande del mundo es genérico y trae gaming
adentro**: 169.646★ y 27.561 forks, contra 40.485★ del especializado. El patrón
que el mercado premia es **catálogo horizontal con división vertical**, no
catálogo vertical. Para AI Studios: el inventario de agentes de gaming **ya es
commodity MIT**; el diferencial defendible es el **harness, la evaluación y la
integración con el engine del cliente**.

⚠️ **Y el nº1 de gaming está quieto:** `Claude-Code-Game-Studios` tiene
`pushed_at` = **2026-11-09**, 12 días sin commits. La fila de ayer no lo decía.

### 🟢 Agentes nuevos con licencia leída en bytes (9)

| Repo | ★ | Licencia (archivo, bytes) | Qué aporta que la KB no tenía |
|------|---|---------------------------|-------------------------------|
| [msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents) | 169.646 | MIT | El catálogo horizontal (~230 agentes) con división de gaming |
| [zenstory-ai/novel-to-game](https://github.com/zenstory-ai/novel-to-game) | 1.175 | MIT (`LICENSE`, 1081) | **IP existente → juego**: despiece de novela, diseño, build, *blind-play*. Único repo de la KB que apunta a **mini-juegos de WeChat/Douyin/TapTap** |
| [MinLL/SkyrimNet-GamePlugin](https://github.com/MinLL/SkyrimNet-GamePlugin) | 386 | 🔴 propia (`LICENSE`, 3745) **con `SkyrimNetApi.psc` MIT** | NPCs que hablan, recuerdan **y actúan**, con voz en tiempo real, en un juego con base de jugadores real |
| [christos-antono/NPCTalk](https://github.com/christos-antono/NPCTalk) | 232 | MIT (`LICENSE`, 1078) | La capa **TTS self-hosted** que faltaba junto a `npcpy`/LLMUnity |
| [fennaraOfficial/fennara-godot-ai](https://github.com/fennaraOfficial/fennara-godot-ai) | 321 | MIT (`LICENSE.md`, 1064) | Chat AI **embebido en el editor** de Godot, no sólo MCP |
| [Sagesheep/NarrativeEngine-P](https://github.com/Sagesheep/NarrativeEngine-P) | 131 | MIT (`LICENSE`, 1065) | **Memoria de campaña larga** con NPCs persistentes (TTRPG) |
| [zoyluoblue/mc_aiplayer](https://github.com/zoyluoblue/mc_aiplayer) | 141 | MIT (`LICENSE`, 1063) | Agente que **juega** Minecraft como mod Fabric server-side |
| [SummerEngine/summer](https://github.com/SummerEngine/summer) | 110 | MIT (`LICENSE`, 1070 + `NOTICE`, 1652) | Primer engine **diseñado para agentes**, no adaptado |
| [aigengame/godot-agent](https://github.com/aigengame/godot-agent) | 58 | MIT (`LICENSE`, 1066) | Loop build→verify de Godot por CLI/skill/MCP |

### 🔴 Dos licencias destapadas dentro de tablas que esta KB ya publicaba

1. **`AnkleBreaker-Studio/unity-mcp-server` (549★) y `unity-mcp-plugin` (258★)**
   figuraban como `NOASSERTION` en la tabla de líderes de Unity. Son
   **"AnkleBreaker Open License v1.0"** (3068 B, idéntica en los dos): íncipit
   literal de MIT, y después **atribución obligatoria en el producto**
   (*"Made with AnkleBreaker MCP"* + logo) y **prohibición de reventa** del
   software o de cualquier derivado como herramienta/producto/servicio.
   Permitido explícitamente: uso interno comercial, integración como componente
   **gratuito**, y *"consulting, support, or training services around the
   Software"*. **"Open License" no es open source** — es una licencia con
   restricción de campo de uso.
2. **`Tencent-Hunyuan/HY-World-2.0` (2.900★)**: ayer escribimos "no publica
   ningún archivo de licencia". **Sí lo publica: `License.txt`** (mayúscula L,
   minúscula t), 16.858 B, licencia comunitaria de Tencent que **no se aplica en
   la Unión Europea, Reino Unido ni Corea del Sur**. Corrección completa en
   `agents/top.md`.

### 📐 `NOASSERTION` medido en los 16 repos de la KB: 7 permisivos / 8 restrictivos / 3 sin grant

Aplicando la regla T60 que llegó de la KB de enterprise. El resultado importa
porque **es una moneda al aire**: `NOASSERTION` no sirve ni para descartar ni
para priorizar. Nuevos permisivos destapados: `love2d/love` (zlib),
`AlanLaboratory/UnrealMLAgents` (Apache-2.0 con *"All Rights Reserved."* arriba
del grant, que es lo que rompe al clasificador), **`contentauth/c2pa-rs` y
`c2pa-python` = MIT *o* Apache-2.0** (los dos archivos presentes, 1083 y 11335 B)
— más permisivos de lo que la Receta 7 asumía.

### ⛔ Candidatos nuevos rechazados (para no re-gastar la búsqueda)

| Candidato | Licencia leída | Motivo |
|-----------|----------------|--------|
| [mannychen0225/Agent-Hive-Workbench](https://github.com/mannychen0225/Agent-Hive-Workbench) | **ninguna** (`LICENSE` 404 en todas las variantes) | 119★, commit de hoy, "Open Source Office Agent & Game Platform Beta 2026". Sin archivo no hay permiso. |
| [letuhao/lore-weave](https://github.com/letuhao/lore-weave) | **AGPL-3.0** | Co-autor AI para world-building con RAG de canon. Útil conceptualmente; copyleft de red lo saca de la ruta permisiva. |
| [beralee/PtcgDeckAgent](https://github.com/beralee/PtcgDeckAgent) | Apache-2.0 | Grant limpio, pero 61★ y alcance muy estrecho (simulación de reglas de PTCG en Godot 4.6). Señal, no ruta. |
| [parkerluxu/Awesome-agent-cases](https://github.com/parkerluxu/Awesome-agent-cases) | MIT | 26★. Canal de descubrimiento (tiene una colección de casos de agentes en game dev), no código. |

### Método y control negativo de este pass

- `gh api search/repositories -f q='repo:<slug>'` sobre **las 83 URLs heredadas**
  → 1 `NOT_FOUND` esperado (`JonTelep/awesome-ai-game-generation`, el rename ya
  documentado) y 0 renames nuevos. **La tasa de ~1/21 de ayer no se repitió en
  24 h**, que es lo que corresponde: los renames son un fenómeno de meses.
- Control negativo: `repo:gmilano/control-does-not-exist-xyz-999` → HTTP 422
  *"the listed users and repositories cannot be searched"*. ✅
- Lista de sondas de licencia ampliada a 11 nombres de archivo tras el fallo de
  `License.txt`. **Cinco** variantes distintas aparecen realmente en esta KB.

---

## 🆕 2026-11-20

25 días desde el pass anterior. **Este pass recuperó un canal que el anterior
daba por bloqueado** (`gh api search/repositories -f q='repo:<slug>'`), y con él
★ + SPDX + `archived` + `pushed_at` + rama default en una sola llamada. Todas las
★ de abajo son de hoy, no heredadas.

### 🔴 La corrección que manda: el líder de MCP para Unity no era el que decíamos

El pass del 2026-10-26 escribió que `IvanMurzak/Unity-MCP` era "el MCP de engine
**más adoptado**". Medido hoy, es el **nº2 por un factor de 3,4**.

| Repo | ★ 2026-11-20 | Licencia | Señal |
|------|--------------|----------|-------|
| **[CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp)** | **16.087** | **MIT** (`LICENSE`, 1066) | **El líder real.** Invisible para este KB porque **fue renombrado**: `justinpbarnett/unity-mcp` → `CoplayDev/unity-mcp`. ⚠️ rama default **`beta`**. |
| [IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP) | 4.672 | Apache-2.0 | Nº2. Sigue siendo la opción si el cliente exige Apache-2.0. |
| [CoderGamester/mcp-unity](https://github.com/CoderGamester/mcp-unity) | 2.018 | MIT | Nº3 con tracción propia. |

Y en Godot el mismo error: este KB listaba `tomyud1/godot-mcp` (455★) como "la
segunda opción real del ecosistema" sin haber visto a
**[Coding-Solo/godot-mcp](https://github.com/Coding-Solo/godot-mcp) (6.713★, MIT)**,
que es **15×** más grande. Con un matiz que cambia la recomendación:
`Coding-Solo` **no tiene commits desde 2026-04-16** (7 meses), mientras
`hi-godot/godot-ai` (3.627★) comiteó **hoy**.

> **Modo de fallo nombrado: *el ascenso del subcampeón*.** Un rename vuelve
> invisible al líder en toda búsqueda por nombre, y el nº2 hereda el título de
> "dominante" sin comparación. Inverso del *name-collision cluster* de la KB de
> enterprise: allí los resultados eran falsos, **aquí todos son reales** y el
> error está en cuál se elige. Discriminador: comparar **≥3 candidatos por ★**
> antes de escribir "dominante"; si el nº1 esperado no aparece, probar `raw` del
> slug viejo para detectar el redirect.

### 🟢 Categoría nueva: orquestación de agentes para game dev

Más grande, en estrellas, que toda la tier de tooling MCP que abrimos en octubre.

| Repo | Licencia | ★ | Señal fechada |
|------|----------|---|---------------|
| [Donchitos/Claude-Code-Game-Studios](https://github.com/Donchitos/Claude-Code-Game-Studios) | **MIT** (`LICENSE`, 1066) | **40.377** | **49 agentes con rol + 72 skills de workflow.** Repo nº1 de game-development en el trending de GitHub a principios de nov-2026 (34,7k el 3-nov → 40,2k en la lista mensual → **40.377 hoy**). |
| [EL4CTEO/blender-skills](https://github.com/EL4CTEO/blender-skills) | **MIT** | 2.529 | Workflows de Blender **como skills de agente**. Cubre *arte 3D*, que el tooling de engine no toca. |
| [genex-games/genex-desktop](https://github.com/genex-games/genex-desktop) | **MIT** | 900 | Desktop app para hacer juegos con AI sobre Unreal/Unity/Godot/browser. ⚠️ rama default **`dev`**. Commit de hoy. |

**Lectura honesta:** 40.377★ en un repo de **prompts y definiciones de agente**
—sin engine, sin runtime, sin modelo— contra 16.087★ del plugin de editor más
usado. Las estrellas miden **interés**, no producción, y un repo de prompts es
barato de marcar. Pero el orden de magnitud dice qué está pidiendo el mercado:
**un pipeline de estudio con roles**, no un agente en el editor.

### 🟢 Hueco histórico cerrado: la contraparte viva de `generative_agents`

| Repo | Licencia | ★ | Señal |
|------|----------|---|-------|
| [a16z-infra/ai-town](https://github.com/a16z-infra/ai-town) | **MIT** | **10.835** | Starter kit **desplegable** de un pueblo de agentes. **Faltaba desde el primer pass.** Esta KB tenía `joonspk-research/generative_agents` (22.399★, Apache-2.0) como "la arquitectura de referencia" sin decir que **no tiene commits desde 2024-08-05 (27 meses)**. `ai-town` comiteó **hoy**: el patrón es el destino, éste es el vehículo. |

### 📈 Crecimiento medido sobre filas ya existentes (25 días)

| Repo | ★ 2026-10-26 | ★ 2026-11-20 | Δ |
|------|--------------|--------------|---|
| [WhiteTowerAI/ohmygame](https://github.com/WhiteTowerAI/ohmygame) | 1,2k | **2.486** | **+107%** — el mayor crecimiento relativo del pass. Commit de hoy. |
| [rehan-remade/universal-modder](https://github.com/rehan-remade/universal-modder) | ~8,9k* (sin verificar) | **9.809** | ★ ahora **verificada**; MIT confirmado. Commit de hoy. |
| [SkyworkAI/Matrix-Game](https://github.com/SkyworkAI/Matrix-Game) | 2,8k | 2.944 | +5%. El pico del release del 15-oct ya pasó. |
| [hi-godot/godot-ai](https://github.com/hi-godot/godot-ai) | 3,2k | 3.627 | +13%. Commit de hoy. |
| [IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP) | 4,5k | 4.672 | +4%. |
| [lmgame-org/GamingAgent](https://github.com/lmgame-org/GamingAgent) | 998 | 1.006 | +1%. ⚠️ **un año exacto sin commits** (2025-11-16). |
| [IvanMurzak/Unreal-MCP](https://github.com/IvanMurzak/Unreal-MCP) | 51 | 64 | Sigue temprano. |
| [IvanMurzak/GameDev-MCP-Server](https://github.com/IvanMurzak/GameDev-MCP-Server) | 16 | 21 | Infra, adopción mínima. Confirma la lectura de octubre: la adopción está en el plugin, no en el backend compartido. |

### 🔴 Señales negativas de esta semana

| Repo | Qué cambió |
|------|-----------|
| [pelican-dev/panel](https://github.com/pelican-dev/panel) | **Desapareció.** En octubre: "vivo, sin archivo de licencia". Hoy: `repo:` → `NOT_FOUND`, `raw` → 404. Un repo sin grant **también puede dejar de existir**; el rechazo de octubre se confirma por la vía más dura. |
| [Tencent-Hunyuan/HY-World-2.0](https://github.com/Tencent-Hunyuan/HY-World-2.0) | 2.891★, se autodenomina "open-source SOTA world model" y **no publica ningún archivo de licencia** (`LICENSE`, `.txt`, `.md` → 404). Mismo patrón que `chongdashu/unreal-mcp` (2.120★, sin grant, parado desde abr-2025). |
| [horizon-rust/horizon](https://github.com/horizon-rust/horizon) | 11.515★ y en listas de trending de game-dev: es **LGPL-3.0**. Reescritura de Minecraft Java en Rust. Aparecerá como "repo de gaming trending"; **no es permisivo**. |
| [SKYHUBDev/Aivill](https://github.com/SKYHUBDev/Aivill) | **0★**, sin licencia y sin README, segundo pass consecutivo. Cerrado definitivamente. |

### 🟢 Un rechazo revertido — los descartes también caducan

[NamazuStudios/elements](https://github.com/NamazuStudios/elements) fue descartado
en octubre como **AGPL-3.0** ("copyleft de red, fuera de la ruta permisiva"). Es
**MPL-2.0**: copyleft **por archivo**, hostearlo **no** obliga a liberar tu
código. El archivo tiene **los mismos 16726 B** que anotó el pass anterior, y
AGPL-3.0 son ~34.500 B — **el dato que refutaba la etiqueta estaba al lado de la
etiqueta**. Sigue siendo chico (22★) y no desplaza a Nakama ni Talo, pero no
estaba descalificado. **Regla nueva: revalidar los rechazos, no sólo las
recomendaciones.** Un descarte mal fundado es peor que una recomendación floja,
porque saca al candidato del campo de visión y nadie vuelve a mirarlo.

### Método y control negativo

38 repos verificados en **dos canales** (`gh api search/repositories` con
qualifier `repo:` exacto + lectura del grant en `raw.../HEAD/`). Control negativo
`gmilano/this-repo-does-not-exist-control-xyz` → `NOT_FOUND` ✅. **7 repos con
rama default ≠ `main`/`master`** detectados (`beta`, `dev`, `develop`,
`development`, `godot-4.x`, `1.0-develop`, `master`): usar `HEAD` en `raw`, que
resuelve la rama default solo.

---

## 2026-10-26

Primer pass sobre esta KB desde el 2026-07-02 (116 días). Todo lo de abajo se
verificó primera mano: `git ls-remote` para existencia y lectura del archivo de
licencia en `raw.githubusercontent.com/<slug>/HEAD/`, con control negativo
(`gmilano/this-repo-does-not-exist-control-xyz` → correctamente UNREACHABLE). Las
★ se leyeron de la página del repo el 2026-10-26.

### 🟢 Lo nuevo con tracción real

| Repo | Licencia (archivo, bytes) | ★ 2026-10-26 | Señal |
|------|--------------------------|--------------|-------|
| [IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP) | Apache-2.0 (`LICENSE`, 10197) | **4.5k** | El MCP de engine más adoptado del ecosistema, y **no estaba en esta KB**. Cualquier método C# se convierte en tool con una línea. |
| [SkyworkAI/Matrix-Game](https://github.com/SkyworkAI/Matrix-Game) | ⚠️ partida: raíz MIT (1118) / `Matrix-Game-4/` Apache-2.0 (11357) | **2.8k** | **Matrix-Game 4.0 liberado el 15-oct-2026.** Engine ejecutable + rendering neural causal. **67 FPS en RTX 5080** a 864×480. Tres juegos nativos publicados. |
| [lmgame-org/GamingAgent](https://github.com/lmgame-org/GamingAgent) | MIT (`LICENSE`, 1065) | **998** | **ICLR 2026.** LMGame Bench + harness + leaderboard. El benchmark de agentes que juegan que esta KB declaraba faltante en julio. |
| [microsoft/MineWorld](https://github.com/microsoft/MineWorld) | MIT (`LICENSE`, 1141) | 493 | World model Minecraft en tiempo real. Checkpoints 300M/700M/1200M, 4-7 fps con Diagonal Decoding. Contrapeso no-APAC a Matrix-Game. |
| [tomyud1/godot-mcp](https://github.com/tomyud1/godot-mcp) | MIT (`LICENSE`, 1071) | 444 | Segunda opción MCP real del ecosistema Godot. |
| [WhiteTowerAI/ohmygame](https://github.com/WhiteTowerAI/ohmygame) | Apache-2.0 (`LICENSE`, 11357) | **1.2k** | "Estudio de un solo desarrollador": canvas de juego + BYO key. **En el trending de GitHub del 2026-10-26** (release `daily-2026-10-26` de `raiscui/gh-trends`). |
| [IvanMurzak/Unreal-MCP](https://github.com/IvanMurzak/Unreal-MCP) | Apache-2.0 (`LICENSE`, 11341) | 51 | Plugin C++ de editor + bridge .NET. Temprano. |
| [IvanMurzak/GameDev-MCP-Server](https://github.com/IvanMurzak/GameDev-MCP-Server) | Apache-2.0 (`LICENSE`, 10197) | 16 | Backend MCP engine-agnostic de los tres anteriores. |
| [rehan-remade/universal-modder](https://github.com/rehan-remade/universal-modder) | MIT (`LICENSE`, 1096) | ~8.9k* | Modding asistido por AI: categoría con volumen propio que esta KB no cubría. |

### 📈 Crecimiento medido sobre filas ya existentes

Lo que cambió en 116 días, releído uno por uno:

| Repo | julio 2026 | **2026-10-26** | Δ |
|------|-----------|----------------|---|
| `hi-godot/godot-ai` | 805 ★, "~41 tools" | **3.2k ★, 46 tools, 120+ ops** | **×4** |
| `limbonaut/limboai` | 2.8k ★ | **3.1k ★** | +11% |
| `heroiclabs/nakama` | 12.8k ★ | **13.6k ★** | +6% |
| `undreamai/LLMUnity` | 1.7k ★ | **1.7k ★** | plano |

**La tendencia, en una línea:** el crecimiento se concentró en **tooling MCP de
editor**, no en NPCs. La tier de NPC con LLM está plana; la tier de agente
conectado al editor se multiplicó por cuatro en el líder.

### 🔴 Retirado del radar con motivo

| Repo | Qué decía esta KB en julio | Qué se verificó el 2026-10-26 |
|------|---------------------------|-------------------------------|
| [SKYHUBDev/Aivill](https://github.com/SKYHUBDev/Aivill) | "Villanos adaptativos que aprenden del comportamiento del jugador. Concepto muy relevante, observar." Listado en *Proyectos a seguir* y en el radar de `intel/trends.md`. | Repo **vivo pero vacío**: `README.md` → 404, ningún archivo de licencia en HEAD, 0 ★. **No hay proyecto que observar.** Se observó durante cuatro meses una carpeta vacía. |
| [leigest519/OpenGame](https://github.com/leigest519/OpenGame) | "Primer framework agentico OSS para generación de juegos web, abr 2026" | Sigue vivo y creció (3.0k ★, Apache-2.0 verificado), pero su **última novedad en README es del 2026-04-21**: seis meses sin anuncios. Se mantiene *con* advertencia de estancamiento. |

### ⛔ Candidatos rechazados (para no re-gastar la búsqueda)

| Candidato | Licencia | Por qué no entra |
|-----------|----------|------------------|
| [balaraj74/openNPC](https://github.com/balaraj74/openNPC) | MIT (1077 B, real) | Se presenta como "framework open source para NPCs autónomos". Primera mano: **5 ★, 4 commits, 1 fork**, se autodescribe "production-ready foundation" pero el comando de clonado de su README sigue diciendo `yourusername`, y Actions no muestra tests. El grant es real; el proyecto no. |
| [lschiweck/LLM-NPC-Agents](https://github.com/lschiweck/LLM-NPC-Agents) | Apache-2.0 (11319 B) | **Proyecto real** (55 commits, backend + cliente Unity, módulos STT/TTS/LLM, cita un paper) y el único candidato Apache-2.0 de NPC con voz en tiempo real. Pero **11 ★** y se autodeclara "Early Release": queda como señal a seguir, no en la ruta recomendada. |
| [chongdashu/unreal-mcp](https://github.com/chongdashu/unreal-mcp) | **ninguna** | Citado en guías 2026 como "MIT, ~70 tools". El README muestra un **badge** `License: MIT`, pero **no hay archivo de licencia en HEAD** (`LICENSE`, `LICENSE.md`, `LICENSE.txt`, `COPYING` → todos 404) y el repo se autodeclara `Status: Experimental`. **Un badge no es un grant.** |
| [Tencent-Hunyuan/Hunyuan-GameCraft-1.0](https://github.com/Tencent-Hunyuan/Hunyuan-GameCraft-1.0) | Tencent community (16386 B), **no OSI** | Aparece en listas como "open source". No es Apache/MIT; trae restricciones de uso. |

### 🧭 Señal de ecosistema de este pass

- **MCP ganó en gaming, y el premio se lo llevó el plugin de engine.** La asimetría
  **4.5k / 51 / 16** dentro del *mismo autor* (Unity / Unreal / backend compartido)
  localiza la adopción con precisión: está en Unity, no en la abstracción. Se reporta
  además un plugin MCP **first-party experimental en UE 5.8**; si Epic lo estabiliza,
  la tier community de Unreal se consolida de golpe.
- **Los world models jugables dejaron de ser "a observar".** Matrix-Game 4.0 corre en
  una **RTX 5080**, y su arquitectura **conserva el engine** en lugar de reemplazarlo:
  por eso es adoptable por etapas. La línea **Genie de DeepMind no es abierta** (reports
  y blog posts, sin código): todo lo jugable abierto es de **Skywork, Microsoft o
  Tencent**, o sea mayoritariamente I+D de APAC.
- **El sentimiento se dio vuelta y eso cambia el pitch.** GDC 2026 (2.300+
  respondentes): **52%** de los devs dice que la gen AI está dañando la industria y
  sólo **7%** ve impacto positivo; uso declarado **36%** global, **30%** en estudio
  contra **58%** en publishing/marketing. El "87% de estudios usa AI agents" que esta
  KB venía repitiendo mide otra cosa y **no se puede usar como apertura**.

### 🔍 Gaps declarados de este pass

- **`huggingface.co` no resuelve DNS** aquí (`getaddrinfo ENOTFOUND`): la licencia de
  los **pesos** de Matrix-Game 4.0 y de los checkpoints de MineWorld **queda sin
  verificar**. En un world model los pesos son la mitad del entregable.
- **`api.github.com/search/*` rechazado** ("sessions are bound to their configured
  repositories"): cero descubrimiento por topic o por estrellas. Todo salió de
  búsqueda web + lectura de páginas de repo, lo que sesga hacia lo indexado en inglés.
- **Oferta OSS de origen LATAM: ausencia medida.** Se buscó en español apuntando a
  estudios brasileños y argentinos. **Cero repos de gaming AI de origen LATAM.** Lo más
  cercano es **Argentum Online** (primer MMORPG open source argentino, liberado 2018,
  **GPL-3.0-or-later, sin AI de NPC**). No es fallo de búsqueda: es la estructura del
  mercado.
- **★ marcadas con `*`** no se releyeron en este pass.

---

# Historial

Todo lo que sigue es el contenido de los passes anteriores, sin modificar.

---

# Agentes trending — Gaming AI

> Tendencias activas en AI gaming. Última actualización: 2026-07-02
> Investigación manual curada — complementa el pipeline automático de top.md

## Tendencias principales en 2026

### 1. NPCs con LLM + Memoria persistente — en producción
La adopción de LLMs para NPCs pasó de prototipos a producción en 2026. El patrón dominante combina:
- **LLM base** (GPT-4o, Claude 3.5, Llama 3.1 local) para generación de diálogo
- **Vector store** (ChromaDB, Qdrant) para memoria episódica del personaje
- **TTS/voz** (ElevenLabs, Kokoro TTS open source) para output de audio
- **Reconocimiento de emoción** del jugador vía análisis de texto o cámara

Impacto medido: NPCs AI aumentaron immersion scores 40% y session times 28% en RPGs.
Segmento NPCs + Digital Humans: 28.6% del mercado total AI gaming en 2026.

### 2. NVIDIA ACE — Digital Humans en tiempo real
NVIDIA ACE (Avatar Cloud Engine): ASR + NLP + TTS + animación facial para NPCs en tiempo real.
Demo "Covert Protocol" (Inworld + ACE, GDC 2025): jugadores interrogan NPCs adaptativos.
SDK disponible para Unreal Engine y Unity. Tendencia: NPCs con voz generada on-the-fly sin latencia perceptible.
NVIDIA también lanzó un fork de Godot con path tracing en GDC 2026 (MIT).

### 3. Generative Agents — de paper a producción
El paper de Stanford "Generative Agents: Interactive Simulacra of Human Behavior" (2023, 21.7k stars) se convirtió en el estándar de arquitectura NPC. Las 3 capas:
- **Memory stream**: log de eventos con timestamp + relevancia + recencia
- **Reflection**: síntesis de recuerdos en insights de alto nivel
- **Planning**: planes horarios basados en reflexiones

Repos derivados activos: Concordia (Google DeepMind, Apache 2.0), implementaciones para RPGs y MMOs.

### 4. AI integrado en engines — Unity 6.2, Godot MCP, Unreal Aura
- **Unity AI** (v6.2, mid-2025): suite en editor sin suscripción separada. Módulos: Assistant (docs/code), Generators (texturas/sprites con difusión), Inference Engine (ML on-device).
- **Godot MCP** ([hi-godot/godot-ai](https://github.com/hi-godot/godot-ai), abr 2026, MIT, 805 stars): 120+ operaciones, 41 tools MCP. Conecta Godot a Claude Code, Cursor, Codex.
- **Unreal Engine — Aura** (Ramen VR, ene 2026): agente con modos editor y coding. Caso Sinn Studio: lanzó *Zombonks* en 5 meses (~mitad del tiempo normal).
- **Roblox AI Studio** (dic 2025): MCP client integrado + OpenGameEval OSS (47 escenarios de benchmark).

### 5. PCG con LLMs, Diffusion y World Models
- **OpenGame** ([leigest519/OpenGame](https://github.com/leigest519/OpenGame), MIT): framework agentico OSS para generar juegos web end-to-end con GameCoder-27B (RL-entrenado para ejecución de código de juego). Abr 2026.
- **Roblox CUBE 3D**: modelo 1.8B params para generación de objetos 3D on-platform desde texto (mar 2025).
- **PANGeA** (paper 2024): Procedural Artificial Narrative using Generative AI para juegos de turnos.
- World models (Genie 2, etc.): modelos que generan entornos de juego completos e interactivos desde texto/imagen.

### 6. Behavior Trees + LLM — el stack ganador para NPCs
El ecosistema Godot consolidó el patrón Behavior Tree + LLM:
- **Beehave** (MIT, 3.2k stars): BTs componibles en el scene tree de Godot. Lógica reactiva estructurada.
- **LimboAI** (MIT, 2.8k stars): BTs + HSMs (Hierarchical State Machines) combinados.
- **godot_rl_agents** (MIT, 900+ stars): RL sobre juegos Godot con SB3, Sample Factory, RLLib, CleanRL.

Patrón emergente: BT como esqueleto de control → LLM como módulo de diálogo/decisión contextual.

### 7. AI QA automatizado — testing con agentes
- Bots RL que juegan el juego para detectar bugs en casos edge-case (especialmente level design)
- **GameStudio Subagents** ([pamirtuna/gamestudio-subagents](https://github.com/pamirtuna/gamestudio-subagents)): equipo de sub-agentes IA (dev, QA, artist, game designer) en terminal
- LLMs + Playwright + CrewAI para testing semántico de flujos de UI
- Roblox OpenGameEval: benchmark que incluye escenarios de testing AI automatizado

### 8. Player analytics con GNNs — churn prediction
- Graph Neural Networks sobre comportamiento + red social del jugador
- 75.83 AUROC con GNN (PyTorch Geometric) vs 62.44 de LightGBM flat-table (benchmark RelBench)
- Predicción de churn 14 días en adelante con señales: amigos que se van, difficulty walls, session length declining
- F2P mobile en LATAM: driver crítico (retención > adquisición en costo)

## Proyectos a seguir

| Proyecto | URL | Señal |
|----------|-----|-------|
| **godot_rl_agents** | [edbeeching/godot_rl_agents](https://github.com/edbeeching/godot_rl_agents) | RL sobre Godot con 4 frameworks, comunidad activa |
| **LLMUnity** | [undreamai/LLMUnity](https://github.com/undreamai/LLMUnity) | LLMs locales en Unity, 1.7k stars, creciendo |
| **npcpy** | [NPC-Worldwide/npcpy](https://github.com/NPC-Worldwide/npcpy) | Python para NPCs multimodales, 1.4k stars |
| **OpenGame** | [leigest519/OpenGame](https://github.com/leigest519/OpenGame) | Primer framework agentico OSS para generación de juegos web |
| **Aivill** | [SKYHUBDev/Aivill](https://github.com/SKYHUBDev/Aivill) | Villanos adaptativos que aprenden del comportamiento del jugador |

## Señal de mercado — adopción

- 87% de estudios usa AI agents en workflows (Google Cloud survey, 615 devs, jun-jul 2025)
- 50%+ de estudios AAA usa AI en alguna capacidad
- Generative AI in Gaming: $1.79B en 2026, CAGR 23.2%
- NPCs y Digital Humans: mayor segmento (28.6% del total AI gaming)

---
*Fuentes: agentmarketcap.ai, solidaitech.com, aivexify.com, wanderfolk.ai, GitHub (verificado 2026-07-02)*
