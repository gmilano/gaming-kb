---
industry: gaming
region: Global
updated: 2026-11-22
---

# Repos trending — Gaming AI

> **Archivo APPEND-ONLY.** Cada pass agrega una sección fechada arriba y conserva
> la historia completa debajo.

---

## 🆕 2026-11-22 — pass cycle 14

**El barrido cambió de eje por tercera vez: de *texto y voz* (cycle 13) a
*producción de assets*.** El motivo está medido: el eje histórico (engines, MCP,
NPC, backends) devolvió **8 de 8 candidatos ya listados** en la re-verificación
de este pass. Tasa de novedad del eje viejo: **0%**.

### 📏 El dato de tendencia que sólo se ve con la historia de este archivo

Cuatro de los repos nuevos de este pass **no cambiaron de licencia**: cambiaron
de **dónde** está la restricción. Puesto en la serie que este archivo conserva:

| Pass | Señal registrada | Qué canal la detectó |
|---|---|---|
| 2026-11-20 (cycle 4) | MonoGame es Ms-PL, no MIT | checksum de bytes |
| 2026-11-21 (cycle 8) | un `LICENSE` puede estar **roto** | byte-read + manifiesto |
| 2026-11-22 (cycle 10) | Tencent excluye **EU / UK / Corea** | byte-read de la 1ª línea |
| 2026-11-22 (cycle 13) | YSPL 1.0 y Fish Audio: umbral de **facturación** | enumeración |
| 🆕 **2026-11-22 (cycle 14)** | **TRELLIS: la restricción está en una dependencia nombrada sólo en el README** | **ninguno de los tres — hizo falta un canal 4** |

### 🔴 Movimiento nº1: `matteodante/AssetFurnace` — **el repo trending que no existe**

El resultado nº1 de la búsqueda de assets 3D de este pass, repetido por **cinco
dueños distintos**, apunta a un slug que devuelve **HTTP 422** en
`search/repositories` y **HTTP 404 (14 bytes)** en `raw.githubusercontent.com`.
El proyecto es real —se llama **AssetFurnace**— pero el repo se llama
[`Bingeljell/image-to-3dlab`](https://github.com/Bingeljell/image-to-3dlab)
(**1.244 ★**, Apache-2.0, push 2026-11-20) y los otros cuatro resultados eran
**forks** (`0 items` con HTTP 200, que es la firma de un fork: la búsqueda de
GitHub los excluye).

> **Para la serie de este archivo:** es el tercer caso registrado de *slug
> fantasma* (cycle 4: `pelican-dev/panel`; cycle 10: 4 slugs con 422; hoy). La
> diferencia es que **esta vez el slug fantasma era el resultado nº1 del
> buscador**, no una fila heredada. El trending de terceros propaga nombres de
> proyecto, no identificadores.

### 🔴 Movimiento nº2: el copyleft bajó de los packs de skills al **modelo**

El cycle 6 registró que *"el copyleft se mudó hacia arriba del stack — ahora
vive en los packs de skills"*. Este pass lo encuentra **abajo**, dentro del peso
del modelo: [`VAST-AI-Research/UniRig`](https://github.com/VAST-AI-Research/UniRig)
(2.189 ★) lleva badge **MIT** y un subárbol **GPL-3.0 de 35.148 B**
(`src/model/michelangelo/`) importado **sin condición** por el encoder que
construye sus dos modelos. El checksum de bytes de la casa (GPL-3.0 ≈ 35.200 B)
lo detectó sin que nadie leyera el archivo.

### 🟢 Movimiento nº3: un proyecto que hace bien **exactamente lo que esta KB mide**

[`Bingeljell/image-to-3dlab`](https://github.com/Bingeljell/image-to-3dlab)
(AssetFurnace) describe su propia salida como *"con la licencia de cada asset
escrita"*. Es el primer repo que esta KB encuentra en 14 passes que trata la
**procedencia de licencia del asset generado** como una característica del
producto y no como una nota al pie. Vale como señal de hacia dónde va la capa:
cuando el output es un asset comercializable, **la licencia deja de ser
metadato del repo y pasa a ser metadato del artefacto**.

### ⏱️ Sobre los deltas de ★

~1 hora desde el cycle 13. **No se reportan variaciones.** Las ★ de este pass
son primeras lecturas, no deltas.

---

## 🆕 2026-11-22 — pass cycle 13

**El barrido de este pass cambió de eje: de *engines y agentes* a la capa de
TEXTO Y VOZ que consume el jugador.** El motivo está medido arriba en
`agents/trending.md`: el eje de engines devolvió 13 de 15 candidatos ya listados.

### 📏 El dato de tendencia que sólo se puede ver con la historia de este archivo

Tres de los proyectos más grandes de la capa nueva **cambiaron de licencia o de
repo en los últimos 15 meses**, y los tres cambios van en la misma dirección:

| Proyecto | Antes | Ahora | Fecha del cambio |
|----------|-------|-------|------------------|
| **Piper** (TTS) | `rhasspy/piper` — **MIT** | `OHF-Voice/piper1-gpl` — **GPL-3.0** | repo viejo archivado 2025-08-26 |
| **fish-speech** (TTS) | permisiva en releases previas | **Fish Audio Community License 1.0** (umbral US$ 500k, evaluado sobre el cliente) | **13-oct-2026** |
| **Yarn Spinner** (diálogo) | MIT en todos los ports | **YSPL 1.0** en los ports Godot-GDScript y Unreal | `NOASSERTION` vigente hoy |

⇒ **La tendencia no es "más proyectos": es menos permisividad en la capa más
cercana al jugador.** Las licencias nuevas de 2026 no prohíben el uso: lo
**condicionan por modelo de negocio** (facturación del cliente, producto
competidor, hosting, entrenamiento de AI). Son más difíciles de auditar
automáticamente que una GPL, porque un badge SPDX las reporta como
`NOASSERTION` o directamente como `MIT`.

### 🆕 Repos fundacionales nuevos de este pass

| Repo | ★ | Licencia (byte-read) | Para qué entra |
|------|---|----------------------|----------------|
| [openai/whisper](https://github.com/openai/whisper) | 113.247 | MIT | referencia de STT |
| [ggml-org/whisper.cpp](https://github.com/ggml-org/whisper.cpp) | 55.409 | MIT | STT en el cliente del juego |
| [SYSTRAN/faster-whisper](https://github.com/SYSTRAN/faster-whisper) | 26.929 | MIT | STT en servidor (moderación de voz) |
| [SWivid/F5-TTS](https://github.com/SWivid/F5-TTS) | 15.633 | MIT | TTS permisivo — reemplazo de fish-speech |
| [i18next/i18next](https://github.com/i18next/i18next) | 8.666 | MIT | runtime de idioma |
| [guardrails-ai/guardrails](https://github.com/guardrails-ai/guardrails) | 7.635 | Apache-2.0 | validación de salida de LLM |
| [NVIDIA-NeMo/Guardrails](https://github.com/NVIDIA-NeMo/Guardrails) | 7.544 | Apache-2.0 (SPDX short-form) | rieles conversacionales |
| [inkle/ink](https://github.com/inkle/ink) | 5.029 | MIT | lenguaje de narrativa |
| [YarnSpinnerTool/YarnSpinner](https://github.com/YarnSpinnerTool/YarnSpinner) | 2.876 | MIT | lenguaje de diálogo |
| [unitaryai/detoxify](https://github.com/unitaryai/detoxify) | 1.320 | Apache-2.0 | clasificador de toxicidad local |

### 🔄 Rename nuevo de este pass

**`NVIDIA/NeMo-Guardrails` → `NVIDIA-NeMo/Guardrails`.** Firma medida: el slug
viejo da **HTTP 422** en `search/repositories` (*"resources do not exist or you do
not have permission"*), el nuevo da `total_count: 1`. Suma a la lista histórica de
renames de esta KB (`justinpbarnett`→`CoplayDev`, `googleforgames`→`agones-dev`,
`minetest`→`luanti-org`, `princeton-nlp/SWE-agent`→`SWE-agent/SWE-agent`,
`rivet-gg/opengb`→cadena de 3 saltos).

### 🔧 Defecto de instrumento medido este pass: la rama por defecto

`YarnSpinnerTool/YarnSpinner-Godot` **no tiene rama `main`**. Sondear
`raw.githubusercontent.com/.../main/LICENSE` devolvió 404 en los 8 nombres de
archivo probados ⇒ *"repo sin licencia"*. En `master` el archivo está y es **MIT
(1.104 B)**. El cycle 7 había medido que `raw` aliasea `master`→`main` en
`Unity-Technologies/ml-agents`; **este repo es el contraejemplo**. ⇒ el aliasing
es propiedad del repo, no del canal: **sondear siempre las dos ramas antes de
declarar un gap.**

---

## 🆕 2026-11-22 — pass cycle 12

**El barrido de este pass cambió de eje otra vez: por ORGANIZACIÓN y por
VENDORIZACIÓN, no por repo.** Dos de los tres hallazgos no eran repos faltantes:
eran **repos hermanos** del que ya estaba listado, con licencia distinta.

| Repo | Licencia (bytes + enumeración) | Qué es | ★ | Push |
|---|---|---|---|---|
| [QwenLM/Qwen3](https://github.com/QwenLM/Qwen3) | 🟡 badge `NONE` · **enumeración = 0** → 🟢 **Apache-2.0 declarado en `README.md`**, archivos en Hugging Face | 🆕 **La capa de SLM que esta KB no tenía.** NVIDIA publicó soporte de **Qwen3-8B on-device en ACE** para NPCs de PC. 2.113 forks | **27.758** | 2026-01-09 |
| [TaloDev/godot](https://github.com/TaloDev/godot) | 🟢 **MIT** (1.074) — **4 archivos, 4 MIT** | Cliente Godot de Talo. **Más estrellado que el backend que esta KB sí listaba** | **251** | 2026-11-16 |
| [fairygui/FairyGUI-unity](https://github.com/fairygui/FairyGUI-unity) | 🟢 **MIT** | El padre permisivo **vendorizado dentro de LGUI**. Framework de UI para Unity. ⚠️ ~9 meses sin push | **2.978** | 2026-02-03 |
| [LumioGames/LGUI](https://github.com/LumioGames/LGUI) | 🟡 **PolyForm Shield 1.0.0** (8.363) — **8 archivos** | UI de juego para agentes. Condicional por **tipo de entregable** | 148 | 2026-11-19 |
| [TaloDev/unity](https://github.com/TaloDev/unity) | 🟢 **MIT** (1.074) — 2 archivos | Paquete Unity de Talo | 43 | 2026-11-16 |
| ⛔ [TaloDev/hosting](https://github.com/TaloDev/hosting) | 🔴 **SIN GRANT — enumeración = 0** | *"Self-hosting starter pack"* dentro de un producto MIT. **El runbook no está otorgado** | 36 | 2026-10-14 |
| ⛔ [anthill-platform/anthill](https://github.com/anthill-platform/anthill) | MIT real | Un blog de 2026 lo llama *"actively maintained"*. **Último commit: 2018-10-05** | **4** | 2018-10-05 |

### 📏 Tres valores nuevos para el checksum de bytes

| Licencia | Bytes medidos | Dónde |
|---|---|---|
| **BSD 3-Clause** tras preámbulo Tencent | **3.473** | `puerts-3.0.3` vendorizado en LGUI. El preámbulo corporativo **no altera** el grant permisivo |
| **PolyForm Shield 1.0.0 + Additional Permission** | **8.363** | `LumioGames/LGUI`. ⚠️ Cae **entre** MIT (~1.100) y Apache-2.0 (~11.350): por tamaño **no se distingue de nada conocido** |
| **LGPL-3.0 (suplemento sin su GPL-3.0 base)** | **7.652** | `thedivergentai/Game-Dev-Agentic-Skills`. El cycle 11 lo vio por sonda; este pass lo **prueba por enumeración** |

### 🔧 Canales de esta sesión

✅ `search/repositories` · ✅ `raw.githubusercontent.com` · ✅ `search/code`
(controles positivos: `TaloDev/backend`→1, `godotengine/godot`→3,
`filename:LICENSE godot`→5.632).
⛔ `gh api repos/<o>/<r>` → **403** · ⛔ `curl -sI github.com` → `200 Connection
Established` **del proxy** · ⛔ **`huggingface.co` no resuelve** (`curl` 000 +
WebFetch `ENOTFOUND`).

---

## 🆕 2026-11-22 — pass cycle 11

**El barrido de este pass cambió de eje: por CATEGORÍA, no por nombre.** Buscar
`game server framework stars:>1500` devolvió una familia de cinco frameworks
permisivos, con **24.000 ★ sumadas**, que once passes de esta KB no habían
listado. No era un repo faltante: era una capa.

| Repo | Licencia (dos canales) | Qué es | ★ | Push |
|---|---|---|---|---|
| [cloudwu/skynet](https://github.com/cloudwu/skynet) | **MIT** (1.085) | Framework de actores **C + Lua**. El más estrellado de la categoría | **14.199** | 2026-11-15 |
| [topfreegames/pitaya](https://github.com/topfreegames/pitaya) | **MIT** (1.067) | **Go** con clustering + SDKs **Unity / iOS / Android** (`libpitaya`). 547 forks | 2.847 | 2026-11-17 |
| [lonng/nano](https://github.com/lonng/nano) | **MIT** (1.073) | **Go** ligero. ⚠️ 9 meses sin push. **Es el tronco del que sale pitaya** | 3.228 | 2026-02-26 |
| [zfoo-project/zfoo](https://github.com/zfoo-project/zfoo) | **Apache-2.0** (11.357 ✅) | **Java/JVM**, RPC + game server. Única opción JVM de la categoría | 2.005 | 2026-11-19 |
| [node-pinus/pinus](https://github.com/node-pinus/pinus) | **MIT** (1.067) | **Node.js / TypeScript** distribuido | 1.933 | 2026-09-29 |
| [BabylonJS/Babylon.js](https://github.com/BabylonJS/Babylon.js) | **Apache-2.0** (`license.md`, 9.226 — **minúscula**) | Motor 3D **web** completo. La tabla de engines no tenía ninguno de navegador de primera línea, y `godogen` ya generaba para él | **26.419** | 2026-11-21 |
| [4ian/GDevelop](https://github.com/4ian/GDevelop) | **NOASSERTION → MIT × 4** (ver `verticals/solutions.md`) | Engine **no-code** 2D/3D/multijugador. Badge opaco, grant permisivo | **29.166** | 2026-11-21 |

⛔ **El cadáver más estrellado de la categoría:**
[NetEase/pomelo](https://github.com/NetEase/pomelo) — **11.855 ★, MIT,
ARCHIVADO** desde 2023-08-10. Ordenar por popularidad lo pone primero;
[node-pinus/pinus](https://github.com/node-pinus/pinus) es su continuación viva
con **1/6 de las estrellas**. La relación popularidad↔vigencia vuelve a salir
invertida, como con `0ad/0ad` (cycle 4) y `SerpentAI` (este pass).

🔴 **Cadena de linaje que hay que declarar antes de comparar:**
**pomelo (archivado) → nano → pitaya**. El `LICENSE` de pitaya dice
*"© 2018 TFG Co **and nano Authors**"*. No son tres alternativas independientes.

---

## 🆕 2026-11-22 — pass cycle 10

> Sin deltas de ★ (primer pass de un día nuevo). Lo que cambia este pass es
> **cobertura del lado de los world models**, que era el hueco más caro del
> inventario: la KB listaba la línea de Tencent —que **no tiene licencia en
> EMEA**— y casi nada permisivo al lado.

### 🆕 World models permisivos — el bloque que faltaba

| Repo | ★ | Licencia (byte-read) | Último push | Qué aporta |
|---|---|---|---|---|
| [deeplethe/utopia](https://github.com/deeplethe/utopia) | **8.745** | **Apache-2.0** (11.353 B) | 2026-11-22 | El world model abierto más grande del inventario. Creado 2026-08-07: **8.745 ★ en 3,5 meses** |
| [nv-tlabs/lyra](https://github.com/nv-tlabs/lyra) | **2.888** | **Apache-2.0** (11.356 B) | ⚠️ 2026-07-20 | NVIDIA Toronto AI Lab. **Salida 3D**, no video — el equivalente funcional directo de HY-World 2.0, con grant limpio |
| [SkyworkAI/Matrix-Game](https://github.com/SkyworkAI/Matrix-Game) | 2.951 | **MIT** raíz + **Apache-2.0** en `Matrix-Game-4/` | 2026-10-29 | Ya en la KB; se re-verifica porque es **el sustituto recomendado** de la línea Tencent |
| [galilai-group/stable-worldmodel](https://github.com/galilai-group/stable-worldmodel) | **2.315** | **MIT** (1.070 B) | 2026-10-02 | **Evaluación reproducible.** La pieza que convierte la elección entre los otros tres en una medición |

🔴 **El contraste que justifica el bloque entero:** las **tres** releases
abiertas de world models de Tencent (`HY-World-2.0` 2.901 ★,
`HY-WorldPlay` 1.630 ★, `Hunyuan-GameCraft-1.0` 765 ★) llevan, en la **primera
línea** de su archivo de licencia, *"THIS LICENSE AGREEMENT DOES NOT APPLY IN
THE EUROPEAN UNION, UNITED KINGDOM AND SOUTH KOREA"*. **≈5.300 ★ de world
models que un estudio europeo no puede usar**, contra ≈14.000 ★ permisivos que
ahora sí están listados.

### 🆕 Navegación y entornos RL — dos clásicos que faltaban

| Repo | ★ | Licencia | Último push | Qué aporta |
|---|---|---|---|---|
| [ikpil/DotRecast](https://github.com/ikpil/DotRecast) | 968 | **Zlib** (904 B) | 2026-10-03 | Port a **C#** de Recast & Detour. La KB tenía sólo el original C++ ⇒ **no tenía vía de entrada a Unity/.NET** para navmesh |
| [Kautenja/gym-super-mario-bros](https://github.com/Kautenja/gym-super-mario-bros) | 910 | **MIT** (1.073 B) | 2026-09-30 | Entornos Gymnasium NES/Mario, **vivos**, compatibles con `Farama-Foundation/Gymnasium` |

### 🆕 Herramientas de engine — 5 filas, todas permisivas

| Repo | ★ | Licencia | Último push | Engine |
|---|---|---|---|---|
| [kevinpbuckley/VibeUE](https://github.com/kevinpbuckley/VibeUE) | 845 | **MIT** (1.091 B) | 2026-11-20 | **Unreal** |
| [hatayama/unity-cli-loop](https://github.com/hatayama/unity-cli-loop) | 602 | **MIT** (`LICENSE.md`) | 2026-11-21 | Unity (**hasta Play Mode**) |
| [tugcantopaloglu/godot-mcp](https://github.com/tugcantopaloglu/godot-mcp) | 521 | **MIT** (1.169 B) | 2026-10-09 | Godot (157 tools) |
| [IvanMurzak/Godot-MCP](https://github.com/IvanMurzak/Godot-MCP) | 376 | **Apache-2.0** | 2026-11-21 | Godot |
| [FunplayAI/funplay-unity-mcp](https://github.com/FunplayAI/funplay-unity-mcp) | 330 | **MIT** (1.064 B) | 2026-11-19 | Unity (`execute_code`) |

🟡 **El lado Unreal estaba peor que el de Godot y nadie lo había dicho:** el repo
Unreal más estrellado del inventario,
[chongdashu/unreal-mcp](https://github.com/chongdashu/unreal-mcp), tiene
**2.119 ★, licencia `null` y 19 meses sin push**. `VibeUE` (MIT, 845 ★, push
hace 2 días) es la primera alternativa Unreal viva **y con grant** que entra a
esta KB.

### 📉 Estado de salud del inventario (159 repos, medido este pass)

| Señal | Nº | Comentario |
|---|---|---|
| Archivados | 2 | `0ad/0ad`, `openai/gym` — ya anotados |
| Renombrados (422 + `raw` 200) | 3 | `minetest/minetest`, `rivet-gg/toolchain`, 🆕 `kolbytn/mindcraft` |
| Borrados (422 + `raw` 404) | 1 | `pelican-dev/panel` |
| Dormidos > 6 meses | **26** | El más estrellado vivo-pero-dormido: `joonspk-research/generative_agents` (22.399 ★, **sin push desde 2024-08-05**) |
| `NOASSERTION` o `null` | 31 | Cada uno necesita byte-read; **el badge no es un veredicto en ninguna dirección** |

---
## 🆕 2026-11-21 — pass cycle 9

> Sin deltas de ★ (quinto pass del día). Lo que cambia este pass **es el tamaño del
> universo medido**: 11 repos nuevos y un rechazo de 25.570 ★ que llevaba 8 passes
> sin ser abierto. Los tres passes anteriores subieron la precisión sobre el catálogo
> existente; éste descubre que el catálogo **tenía un agujero**.

### 🔴 El repo del pass: `SpacetimeDB`, 25.570 ★, **BSL 1.1 — no open source**

| Campo | Valor | Cómo se obtuvo |
|---|---|---|
| [clockworklabs/SpacetimeDB](https://github.com/clockworklabs/SpacetimeDB) | 25.570 ★, 1.121 forks | `gh api search/repositories` |
| Creado / push | 2023-06-17 / 2026-11-21 | idem |
| GitHub dice | `NOASSERTION` | idem |
| Realidad | **BSL 1.1**, `LICENSE.txt` **42.562 B** | byte-read `raw` |
| Change Date | **2031-11-20** → AGPL-3.0 c/ excepción de linking | byte-read |
| Tope de uso | **1 (una) instancia en producción** | byte-read |

Base de datos relacional donde la lógica del servidor vive **dentro** de la base
(módulos en Rust o C# a WASM): el modelo que popularizó BitCraft. Técnicamente es el
competidor serio de Nakama. **Legalmente está fuera de la ruta permisiva** y el
motivo no es la conversión a AGPL de 2031 sino el tope de **una instancia** hoy.

⚠️ **Trampa de comparativo:** los blogs de 2026 que comparan *"SpacetimeDB vs PlayFab
vs Nakama"* lo ponen en la misma tabla que Nakama (Apache-2.0) **sin una columna de
licencia que marque BSL**. Si el cliente llegó con esa tabla, llegó con un error.

### 🆕 Repos nuevos incorporados al universo medido (11)

Todos verificados hoy por `search/repositories` **y** byte-read de su `LICENSE`.
Detalle completo en [`agents/top.md`](../agents/top.md).

| Repo | Licencia (bytes) | ★ | Forks | Creado | Último push |
|---|---|---|---|---|---|
| [scenario-labs/skills](https://github.com/scenario-labs/skills) | MIT (1.070) | 3.961 | 350 | 2026-08-12 | 2026-11-21 |
| [CraftJarvis/MineStudio](https://github.com/CraftJarvis/MineStudio) | MIT (1.068) | 423 | 36 | 2024-11-07 | 2026-09-28 |
| [ukanwat/overtime](https://github.com/ukanwat/overtime) | MIT (1.078) | 403 | 75 | 2026-07-31 | 2026-10-18 |
| [tettethu/VibeGame](https://github.com/tettethu/VibeGame) | Apache-2.0 (14.096) | 389 | 40 | 2026-08-12 | 2026-11-21 |
| [isuzu-shiranui/UnityMCP](https://github.com/isuzu-shiranui/UnityMCP) | MIT (1.066) | 344 | 27 | 2025-04-16 | 2026-11-20 |
| [wellingfeng/UltraGameStudio](https://github.com/wellingfeng/UltraGameStudio) | MIT (1.068) | 344 | 37 | 2026-05-30 | 2026-11-21 |
| [TabooHarmony/roblox-brain](https://github.com/TabooHarmony/roblox-brain) | MIT (1.069) | 199 | 20 | 2026-05-27 | 2026-10-15 |
| [satelliteoflove/godot-mcp](https://github.com/satelliteoflove/godot-mcp) | MIT (1.056) | 188 | 23 | 2025-12-21 | 2026-11-16 |
| [WenyiWU0111/RSIGame](https://github.com/WenyiWU0111/RSIGame) | Apache-2.0 (11.400) | 178 | 16 | 2026-09-28 | 2026-10-30 |
| [typeonce-dev/ritmo](https://github.com/typeonce-dev/ritmo) | MIT (1.072) | 170 | 7 | 2026-03-28 | 2026-11-21 |
| [quazaai/UnityMCPIntegration](https://github.com/quazaai/UnityMCPIntegration) | MIT (1.068) | 155 | 34 | 2025-03-17 | **2025-04-15 ⚠️ dormido** |

**Lectura de cohorte:** **7 de los 11 fueron creados en 2026** (`scenario-labs/skills`,
`VibeGame`, `UltraGameStudio`, `roblox-brain`, `RSIGame`, `ritmo` y `overtime`) y
6 de los 11 tienen push **de hoy o de ayer**. La categoría *agente que construye el
juego* se está poblando ahora mismo; la categoría *agente que juega* (Mindcraft,
GamingAgent, MineStudio) es más vieja y se mueve más lento.

### ⚠️ Un dormido entre los nuevos, marcado a propósito

`quazaai/UnityMCPIntegration` tiene 155 ★ y **no recibe un commit desde 2025-04-15**
(19 meses). Entra a esta página **marcado**, no recomendado: es exactamente el perfil
que una búsqueda ordenada por estrellas devuelve arriba y que un equipo apurado toma
como base viva. Regla heredada del cycle 4 (*runner-up promotion*), aplicada al revés:
acá el riesgo no es coronar al runner-up, es **coronar a un muerto por inercia de ★**.

### ⛔ Rechazado: `databrickslabs/ontobricks` — homónimo de `Unity`

485 ★, push de hoy, pero expone **Unity Catalog** (metastore de Databricks) por MCP.
No es el engine. Entró por la búsqueda `unity ai agent mcp`. **Segundo homónimo
medido de esta KB.**

---

## 🆕 2026-11-21 — pass cycle 8

> Sin deltas de ★ (cuarto pass del día; una hora no mueve una estrella). Lo que
> cambia este pass **no es el tamaño del universo medido sino la confiabilidad
> del dato de licencia sobre el universo ya medido**.

### 🔴 El evento del pass: un `LICENSE` que es **un fragmento de diff**

`Farama-Foundation/Minigrid` (2.543 ★, push 2026-11-14) — benchmark RL estándar,
no un repo marginal:

- `LICENSE` raíz: **2.507 B**, íncipit *"Apache License, Version 2.0"*, **sólo
  §§ 1 y 9**, **faltan §§ 2-8 (grant de copyright, de patente, redistribución)**,
  y un **`@@ -154,49 +194,23 @@`** literal en la línea 18.
- `pyproject.toml`: **MIT**.
- Badge: `NOASSERTION` — **acertado, y por dos causas a la vez** (archivo dañado
  **y** repo compuesto). Primer caso así en esta KB+energy+enterprise.
- **`docs/LICENSE` (12.201 B): MIT + Apache-2.0 completos** ⇒ 🟢 **permisivo**.

**Lo que esto le hace al checksum de bytes, la técnica estrella de la casa:**
2.507 B con íncipit Apache **encaja en la categoría *"Apache sin apéndice"*** que
esta misma página usa para `libgdx` (10.174 B). El checksum sabe decir *"no es
canónico"*; **no sabe decir *"no es un documento"***. Un recorte deliberado y un
archivo roto son **indistinguibles por tamaño**. Chequeo nuevo (Receta 21):

```bash
grep -cE '^\s*(@@|<<<<<<<|=======|>>>>>>>|\+\+\+|---)' LICENSE   # 0 esperado
grep -cE '^ *[0-9]+\. ' LICENSE                                  # Apache-2.0 → 9
```

Hermanos **sanos**: `Gymnasium` 1.104 B, `PettingZoo` 1.073 B, 0 artefactos.

### 🔴 El rechazo del pass: `MineDojo/NitroGen` (2.251 ★, CVPR 2026)

**NVIDIA License, 4.729 B**: § 3.3 **no comercial** (*"or intended for use"*),
§ 3.2 **viral**, § 3.7 arrastra el **Meta OPT-IML 175B License Agreement**,
§ 3.4 terminación por patente. ⛔ **Inusable en entregables, PoC pago incluido.**
El abstract dice *"we open-source the dataset, simulator, and pre-trained
weights"*. **"Open-source" en un paper de CVPR no es un grant.**

### 🆕 Trending real, filtrado por licencia (todo byte-read)

| Repo | ★ | Licencia (bytes) | Por qué entra |
|------|---|------------------|---------------|
| [opendilab/LightZero](https://github.com/opendilab/LightZero) | 1.663 | **Apache-2.0** (11357) | **MCTS + AlphaZero/MuZero**: la capa de **planificación** que faltaba junto a PPO. Push 2026-11-19 |
| [opendilab/DI-engine](https://github.com/opendilab/DI-engine) | 3.645 | **Apache-2.0** (11342) | Plataforma RL amplia; LightZero es su módulo de búsqueda. ⚠️ push 2025-12-07 |
| [Farama-Foundation/Minigrid](https://github.com/Farama-Foundation/Minigrid) | 2.543 | 🟡 **MIT + Apache-2.0** (`docs/LICENSE`, 12201) | Prototipado de agentes sin engine 3D; mismo API que Gymnasium |
| [Farama-Foundation/MicroRTS-Py](https://github.com/Farama-Foundation/MicroRTS-Py) | 295 | **MIT** (1117) | El caso **RTS** a escala de laboratorio. Señal, no base |
| [MineDojo/MineDojo](https://github.com/MineDojo/MineDojo) | 2.285 | **MIT** (1070) | El entorno detrás de `Voyager`. 🔴 push 2024-03-18 |
| ⛔ [MineDojo/NitroGen](https://github.com/MineDojo/NitroGen) | 2.251 | ⛔ **NVIDIA License** (4729) | **Rechazado**: no comercial y viral |
| ⛔ [openai/gym](https://github.com/openai/gym) | 37.237 | — | 🔴 **ARCHIVADO**. Se lista para que **nadie lo proponga** |

### Censo de salud del inventario (2026-11-21, cycle 8)

**139 slugs** re-verificados: **134 resuelven**, **5 no** — y **los 5 ya estaban
documentados** (rename, fuera de índice, o muerto). 🟢 **0 filas vivas falsas.**
Archivados en el inventario: **`0ad/0ad`** (ya anotado) y **`openai/gym`**
(se incorpora ya marcado). Sin push desde antes de mayo-2026: **19 repos** — los
más citados siguen siendo `MineDojo/Voyager` (2024-04-03),
`joonspk-research/generative_agents` (2024-08-05) y `lmgame-org/GamingAgent`
(2025-11-16), **los tres recomendados por roundups de 2026**.

---

## 🆕 2026-11-21 — pass cycle 7

> Sin deltas de ★ por la misma razón que en los cycles 5 y 6 (una hora no mueve
> una estrella). Lo que cambia este pass es **el tamaño del universo medido**.

### 🔴 El evento del pass: **8 engines y 2 frameworks de RL que esta página nunca listó**

Un censo plano contra los incumbentes obvios —una consulta formulada como
**categoría** y no como problema— devolvió repos que seis passes de verificación
no habían tocado porque **nunca los habían buscado**:

| Repo | Licencia (archivo, bytes) | ★ | `pushed_at` |
|------|---------------------------|---|-------------|
| [bevyengine/bevy](https://github.com/bevyengine/bevy) | **MIT** (`LICENSE-MIT`, 1036) **o** **Apache-2.0** (`LICENSE-APACHE`, 10173) | **49.914** | 2026-11-20 |
| [phaserjs/phaser](https://github.com/phaserjs/phaser) | **MIT** (`LICENSE.md`, 1100) | **40.756** | 2026-08-21 |
| [raysan5/raylib](https://github.com/raysan5/raylib) | **Zlib** (`LICENSE`, 877) | **35.758** | 2026-11-21 |
| [libgdx/libgdx](https://github.com/libgdx/libgdx) | **Apache-2.0** (`LICENSE`, 10174) | **25.553** | 2026-11-19 |
| [google-deepmind/mujoco](https://github.com/google-deepmind/mujoco) | **Apache-2.0** (`LICENSE`, 11358) | 18.369 | 2026-11-21 |
| [flame-engine/flame](https://github.com/flame-engine/flame) | **MIT** (`LICENSE`, 1067) | 10.821 | 2026-11-21 |
| [cocos/cocos-engine](https://github.com/cocos/cocos-engine) | **MIT** (`LICENSE.md`, 1164) · badge `NOASSERTION` | 9.941 | 2026-10-28 |
| [defold/defold](https://github.com/defold/defold) | 🔴 **`Defold License 1.0`** (`LICENSE.txt`, 9977) · badge `NOASSERTION` | 6.476 | 2026-11-21 |
| [google-deepmind/open_spiel](https://github.com/google-deepmind/open_spiel) | **Apache-2.0** (`LICENSE`, 11358) | 5.603 | 2026-11-14 |
| [panda3d/panda3d](https://github.com/panda3d/panda3d) | **BSD-3-Clause** (`LICENSE`, 1569) · badge `NOASSERTION` | 5.280 | 2026-07-28 |

**158.000 ★ acumuladas, 4 de ellas por encima de las 25.000.** Ninguna había sido
sondeada nunca: no es un fallo de verificación, es un fallo de **vocabulario de
búsqueda**. Ver la regla nº14 en `repos/foundations.md`.

### 🆕 Trending verificado este pass (nuevo y vivo)

| Repo | Licencia (bytes) | Por qué está acá | ★ |
|------|------------------|------------------|---|
| [ramenvr/autopilot](https://github.com/ramenvr/autopilot) | **Apache-2.0** (11358) | Agente de playtesting OSS multi-engine (UE 5.4–5.8, Unity 6, Godot 4, Roblox Studio) + MCP. Commit de hoy | 881 |
| [htdt/godogen](https://github.com/htdt/godogen) | **MIT** (1077) | Pack de skills de game dev más estrellado que existe. Commit de hoy | **7.378** |
| [vercel-labs/skills](https://github.com/vercel-labs/skills) | **MIT** (`LICENSE`, 1069) | 🆕 **La capa de distribución de los packs de skills**, que esta KB no tenía: `npx skills add <owner>/<repo>` instala en `.agents/skills/` para Codex y Antigravity con enlaces desde `.claude/skills/`. Es el **punto de control natural del gate de la Receta 16** | **40.334** |
| [EricSun0218/OpenGameAgent](https://github.com/EricSun0218/OpenGameAgent) | **MIT** (1096) | Runtime TS para personajes AI-native. push 2026-11-17 | 61 |

### 🔴 Las tres correcciones al **checksum de bytes** de esta casa

El checksum heredado (`MIT~1100 / Apache~11350 / AGPL~34500`) se validó en 2 de 3
y **falló en Apache**, en dos direcciones:

1. **Apache-2.0 tiene dos valores legítimos**: **11358** (canónico, con
   `APPENDIX: How to apply`) y **10173/10174** (el texto termina en `END OF TERMS
   AND CONDITIONS`). **Δ = 1.185 B = el bloque del apéndice.** Con el checksum
   heredado, `bevy` y `libgdx` se habrían marcado **mal** como "no Apache".
2. **Un conteo por encima del canónico es un hallazgo, no ruido:**
   `NVIDIA/Audio2Face-3D-training-framework` = **15057 B** = Apache-2.0 + un
   inventario `PORTIONS LICENSED AS FOLLOWS` de **3 dependencias vendorizadas
   MIT con su ruta**, dentro del mismo archivo. 🔴 **La enumeración de la Receta
   17 no lo caza**: encuentra **un** archivo de licencia y concluye un grant.
3. **El checksum no puede distinguir un grant hecho a medida:** la
   `Defold License 1.0` son **9977 B**, a **196 bytes** del Apache sin apéndice,
   y es Apache verbatim con una **restricción de campo de uso** añadida.

🟢 **`AGPL~34500` confirmado** por `SillyTavern` (34523 B). **`MIT~1100`
confirmado exacto** por `phaser` (1100 B), con la variación esperada por número
de líneas de copyright (`cocos-engine` 1164 B = dos titulares).

### 🧪 Defecto de instrumento nuevo: `raw` **aliasea `master` → `main`**

`Unity-Technologies/ml-agents` **no tiene** `refs/heads/master` (sus ramas son
`develop`, `main`, `hotfix-bump-version-master`, `master-limitation-docs`,
`master-project-upgrade`, `master-reward-providers`, `master-tracking`). Y aun
así:

```
raw/.../ml-agents/master/LICENSE.md       → 200, 11.324 B, idéntico a main
raw/.../ml-agents/zzz-no-such-branch/...  → 404   (control negativo)
```

⇒ **"lo encontré en `master`" no prueba que `master` exista.** Un bucle de sondas
`main`+`master` **cuenta dos veces el mismo archivo**. Deduplicar por hash de
contenido, no por nombre de rama.

### 📋 Censo de verificación de este pass

- **24 repos nuevos** pasados por los 3 canales; **24/24 vivos** vía
  `git ls-remote`; **0 archivados**.
- **23/24 licencias leídas en bytes.** La excepción es
  `NVIDIA/Audio2Face-3D`, que **no tiene ninguna** (y eso *es* el hallazgo).
- **2 repos fuera del índice** de `search/repositories` (`kolbytn/mindcraft`,
  y `htdt/godogen_assets` sólo por `search/code`): **no se les inventó ★**.
- **4 badges `NOASSERTION` resueltos por byte-read en las dos direcciones**:
  🟢 permisivos → `cocos-engine` (MIT), `panda3d` (BSD-3-Clause);
  🔴 restrictivos → `defold` (field-of-use), `minerl` (CC-BY-NC-SA).
- **1 corrección a una fila publicada**: `ml-agents` (grant partido por rama).
- **1 gap declarado sin fila**: GameReverie (sin slug resoluble).

---

## 🆕 2026-11-21 — pass cycle 6 (≈1 h después del cycle 5)

> ⏱️ **Una hora no es un intervalo de medición. Este pass no reporta un solo
> delta de ★** y no reescribe ninguna cifra heredada. Se anota explícitamente
> porque un archivo append-only tiene la tentación de inventar movimiento para
> tener algo que escribir. **No hubo nada que medir. Hubo algo que descubrir.**

**Canal nuevo, y es el cambio real:**
`gh api search/code -f q='repo:<slug> filename:LICENSE'` → **enumera** los
archivos de licencia de cualquier repo de terceros. Gap declarado *"CONFIRMADO
sin canal"* en los dos passes anteriores, que probaron
`gh api repos/<o>/<r>/contents` (bloqueado) y no probaron `search/code`.

### Repos nuevos en el radar (★ leídas hoy, sin delta)

| Repo | Licencia (bytes, status HTTP verificado) | ★ | Forks | `pushed_at` | Rama default |
|------|------------------------------------------|---|-------|-------------|--------------|
| [gamedev-skills/awesome-gamedev-agent-skills](https://github.com/gamedev-skills/awesome-gamedev-agent-skills) | **Apache-2.0** (11395) + `NOTICE` (883) | **9.929** | 1.013 | 2026-11-12 | `main` |
| [rivet-dev/rivet](https://github.com/rivet-dev/rivet) | **Apache-2.0** (11350) | **6.970** | 328 | **2026-11-21** | `main` |
| [rivet-dev/agentos](https://github.com/rivet-dev/agentos) | **Apache-2.0** | **4.912** | 281 | **2026-11-21** | `main` |
| [cranebeaker/coop-anything](https://github.com/cranebeaker/coop-anything) | **MIT** (1085) | 2.185 | 298 | 2026-11-18 | `main` |
| [rivet-dev/agents](https://github.com/rivet-dev/agents) | **Apache-2.0** | 1.652 | — | 2026-11-20 | `main` |
| [RainBowLuoCS/Gamer](https://github.com/RainBowLuoCS/Gamer) | **Apache-2.0** (11358) | 1.189 | 23 | 2026-11-16 | `main` |
| [shinkuan/Akagi](https://github.com/shinkuan/Akagi) | **Apache-2.0** | 1.151 | — | 2026-11-18 | ⚠️ **`v3`** |
| [thedivergentai/Game-Dev-Agentic-Skills](https://github.com/thedivergentai/Game-Dev-Agentic-Skills) | 🔴 **LGPL-3.0** (7652) | 983 | 62 | 2026-10-16 | `main` |
| [Station-Sciences/bot-crossing](https://github.com/Station-Sciences/bot-crossing) | **MIT** (1069) | 906 | 286 | 2026-10-30 | `main` |
| [Calix-L/DanKS](https://github.com/Calix-L/DanKS) | **Apache-2.0** (11348) | 833 | 36 | 2026-10-31 | `main` |
| [jaylfc/taOS](https://github.com/jaylfc/taOS) | 🔴 **AGPL-3.0** (34523) | 609 | 48 | 2026-11-20 | ⚠️ `master` |
| [THUSI-Lab/GameScaling](https://github.com/THUSI-Lab/GameScaling) | **Apache-2.0** — 🟢 **pesos Qwen3.5** | 604 | 35 | 2026-11-19 | `main` |
| [openfw-game/OpenLiberty](https://github.com/openfw-game/OpenLiberty) | **MIT** en ⚠️ **`LICENCE`** (1066) | 493 | 46 | 2026-10-13 | `main` |
| [op7418/guizang-game-skill](https://github.com/op7418/guizang-game-skill) | 🔴 **AGPL-3.0** (34524) | 431 | 71 | 2026-11-11 | `main` |
| [Eurekaleo/awesome-ai-for-games](https://github.com/Eurekaleo/awesome-ai-for-games) | **MIT** (1082) | 391 | 17 | 2026-11-19 | `main` |
| [AIGeeksGroup/Code2Games](https://github.com/AIGeeksGroup/Code2Games) | 🔴 **ninguno** (11 sondas → 404) | 89 | — | 2026-10-17 | `main` |

### 🔴 El evento del pass: `rivet-gg/opengb` ("Open Game Backend") dejó de existir como producto de gaming

Cadena de renames de **3 saltos**, y los tres slugs obsoletos devuelven **200 en
`raw`** mientras ninguno es visible para el API — es decir, **todos parecen
vivos**:
`rivet-gg/opengb` → su README dice *"Moved to `rivet-gg/toolchain`"* → ese repo
está **archivado** y su README apunta a `rivet-gg/rivet` → **ése** es el redirect
de **organización** (`rivet-gg` → `rivet-dev`) → el canónico es
[rivet-dev/rivet](https://github.com/rivet-dev/rivet) (6.970★, Apache-2.0,
`LICENSE` 11350 B, commit de hoy).

**Y lo que hay al llegar no es un backend de juego.** `rivet-dev/rivet` hoy es
*"the orchestrator for agentic workloads"*; sus hermanos son `agentos` (4.912★,
sandboxes WASM) y `agents` (1.652★). **La empresa del Open Game Backend
abandonó gaming y revendió su orquestación de game servers como infraestructura
de agentes AI.** Es el tercer repo de esta KB que sale del nicho hacia arriba
(los otros dos: `agency-agents` 169k★ genérico con gaming como 1 de 18
divisiones, y `Claude-Code-Game-Studios`). Ver `intel/trends.md` nº 18.

🔵 **Dos lecciones de método, y una es nueva:** la firma de rename (`repo:` 422 +
`raw` del slug viejo 200) **hay que iterarla** —hacen falta **3** iteraciones: `opengb` apunta a `toolchain`, que está
**archivado** y apunta a `rivet-gg/rivet`, que es el redirect de organización— y
un rename de **organización** invalida *todos* sus punteros de golpe: es el modo de fallo de rename con mayor radio de daño y el
único que puede romper una página entera de esta KB en un solo evento.

### ⚠️ Trap nuevo en el método de esta página: **14 bytes**

El cuerpo de un 404 de `raw.githubusercontent.com` son **exactamente 14 bytes**
(la cadena `404: Not Found`), y `curl` sin `-f` sale con código 0 y lo escribe en
stdout. Un script que mida bytes sin leer el status anota **"`LICENSE.md`, 14 B"**
como un archivo que existe. **Todo byte-count < 50 es un no-hallazgo.** Peor: la
regla heredada *"bytes muy por debajo de la banda ⇒ archivo puntero, seguir el
puntero"* manda, aplicada a 14 B, a seguir un puntero inexistente. Las sondas de
este pass usan `curl -sS -w "|%{http_code}"` y descartan ≠ 200: **0 falsos
positivos en 11 sondas × 13 repos.**

---

## 2026-11-21 — pass cycle 5

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
