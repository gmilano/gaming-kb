---
industry: gaming
region: Global
updated: 2026-11-22
---

# 🏗️ Repos fundacionales — Gaming

> Bases sobre las cuales construir. Licencia verificada primera mano.
> Última actualización: **2026-11-22 (cycle 12)** (passes previos: 2026-11-22 cycles 11/10, 2026-11-21 cycles 9/8/7/6/5, 2026-11-20, 2026-10-26, 2026-07-02).
>
> **Doble canal obligatorio en esta página.** El SPDX del API
> (`gh api search/repositories -f q='repo:<slug>'`) y la lectura del archivo en
> `raw.githubusercontent.com/<slug>/HEAD/<archivo>` **se usan juntos porque
> discrepan**: 6 de las filas de abajo devuelven `NOASSERTION` del API y tienen
> grant permisivo legible en el archivo. `NOASSERTION` es un **detector de
> licencia compuesta**, no una ausencia de licencia — y es lo que destapó las dos
> correcciones 🔴 de este pass.
>
> **★ de esta página: 100% releídas el 2026-11-20.** Ya no queda ninguna heredada.
>
> 🆕 **TERCER canal, agregado el 2026-11-21 (cycle 6):**
> `gh api search/code -f q='repo:<slug> filename:LICENSE'` **enumera** todos los
> archivos de licencia del repo. Los canales 1 y 2 sólo contestan lo que se les
> pregunta; **hay que saber el nombre del archivo para pedirlo**, así que un
> inventario hecho con sondas es tan completo como la lista de sondas de quien lo
> escribió. **Una sonda confirma lo que ya sospechás; sólo la enumeración te dice
> lo que te faltaba.** Pasado por los repos de esta página, cambió dos filas y
> confirmó el resto — ver la sección siguiente.
>
> ⚠️ **Trap nuevo en el checksum de bytes:** el cuerpo de un 404 de
> `raw.githubusercontent.com` son **exactamente 14 bytes** (`404: Not Found`), y
> `curl` sin `-f` sale con código 0. **Todo byte-count < 50 es un no-hallazgo.**
> Las sondas de este pass usan `curl -sS -w "|%{http_code}"` y descartan ≠ 200.

## 🔴 El hallazgo del cycle 12 (2026-11-22): **el modelo que NVIDIA te invita a correr dentro del juego no tiene licencia en su repo**, y la capa de SLM on-device faltaba entera

Once passes listaron engines, backends, frameworks de servidor y entornos de RL.
**Ninguno listó el modelo.** Y resulta que es la capa donde la regulación que
esta KB vende (Art. 50 EU, obligación federal de runtime en NA) se vuelve
tratable: un SLM on-device **no envía la conversación del jugador a ningún
lado**.

### 🆕 La capa nueva: SLM on-device para NPC

NVIDIA publicó que **ACE soporta Qwen3-8B como SLM on-device** para NPCs en
juegos de PC, además de su propia línea Nemotron (Nemotron 3 Nano 4B y, en
cobertura posterior, Nemotron 3.5 Bolt E4B sobre RTX Spark). Eso convierte al
repo de Qwen en fundacional para gaming, no en una curiosidad de NLP.

| Repo | ★ | Forks | Licencia — **tres canales** | Push |
|---|---|---|---|---|
| [QwenLM/Qwen3](https://github.com/QwenLM/Qwen3) | **27.758** | 2.113 | 🟡 Badge `NONE` · `raw` **9×404** (3 ramas × 3 nombres) · **enumeración `search/code` = 0 archivos** · 🟢 **grant declarado en `README.md` §*License Agreement*: *"All our open-weight models are licensed under Apache 2.0"*, archivos en Hugging Face** | 2026-01-09 |

> 📐 **Regla de método nº14 para esta página: enumeración = 0 NO significa "sin
> licencia".** Significa "el grant no vive en un archivo de este repo". El
> siguiente paso obligatorio es **leer el `README`** antes de escribir el
> rechazo. Esta página estuvo a un paso de descartar un fundacional de 27.758 ★
> por un falso negativo.

⛔ **Gap declarado:** `huggingface.co` **no resuelve en esta sesión** (dos sondas
independientes: `curl` → 000, WebFetch → `ENOTFOUND`). Por lo tanto **este pass
NO verificó el texto Apache-2.0 de los pesos**; verificó que el propietario lo
declara en su propio repo. Antes de embarcar: leer la *model card* del tamaño
exacto, porque esta KB ya documentó que **el grant se parte por variante**.

---

### 🆕 La organización Talo, completa — y la corrección a lo que esta página afirmaba

Esta página listaba `TaloDev/backend` y escribía *"MIT completo, sin tier
cerrado"*. Enumerada la organización, **eran 5 repos, no 1**, y uno de ellos no
tiene grant:

| Repo | ★ | Licencia (bytes) | Archivos de licencia (enumerados) | Push | Qué es |
|---|---|---|---|---|---|
| [TaloDev/godot](https://github.com/TaloDev/godot) | **251** | 🟢 **MIT** (1.074) | **4** — `LICENSE`, `addons/talo` (1.074), `addons/gdUnit4` (1.069, MIT), `addons/GDQuest_GDScript_formatter` (1.072, MIT) | 2026-11-16 | Plugin Godot: leaderboards, stats, auth, game saves. **Más estrellado que el backend (2,4×): es la puerta de entrada real** |
| [TaloDev/backend](https://github.com/TaloDev/backend) | 105 | 🟢 **MIT** (1.074) | 1 | 2026-11-21 | El servidor self-hosteable (ya listado abajo) |
| [TaloDev/unity](https://github.com/TaloDev/unity) | 43 | 🟢 **MIT** (1.074) | 2 | 2026-11-16 | Paquete Unity |
| [TaloDev/frontend](https://github.com/TaloDev/frontend) | 21 | 🟢 **MIT** (1.074) | 1 | 2026-11-17 | Dashboard de gestión |
| [TaloDev/hosting](https://github.com/TaloDev/hosting) | 36 | 🔴 **SIN GRANT** | **0** | 2026-10-14 | *"Self-hosting starter pack for Talo"* |

> 🔴 **Corrección publicada:** *"MIT completo, sin tier cerrado"* es cierto **del
> producto** y falso **de la organización**. `TaloDev/hosting` — el starter pack
> de self-hosting, es decir **la razón misma por la que un cliente elige Talo en
> vez de un backend gestionado** — no tiene ningún archivo de licencia. Cero por
> enumeración, no por sonda fallida.
>
> **Mitigación operativa:** el servidor (`TaloDev/backend`) **sí** es MIT y es lo
> que se despliega. Lo que no está otorgado es el *runbook*. Desplegar con IaC
> propia y **no copiar `hosting/`** al repo del cliente.

---

### 🟡 Framework de UI para agentes: el derivado es menos libre que el padre

| Repo | ★ | Licencia (bytes) | Archivos | Lectura |
|---|---|---|---|---|
| [LumioGames/LGUI](https://github.com/LumioGames/LGUI) | 148 | 🟡 **PolyForm Shield 1.0.0 + Additional Permission** (8.363) | **8** | Verde en un juego de cliente; **rojo dentro de un acelerador reutilizable** (la licencia nombra *"game development tool"* como producto competidor) |
| [fairygui/FairyGUI-unity](https://github.com/fairygui/FairyGUI-unity) | **2.978** | 🟢 **MIT** | — | **El padre vendorizado dentro de LGUI.** Sin condiciones. ⚠️ push 2026-02-03 (~9 meses) |
| [fairygui/FairyGUI-cocoscreator](https://github.com/fairygui/FairyGUI-cocoscreator) | 627 | 🟢 **MIT** | — | Variante Cocos Creator. ⚠️ push 2024-08-05 |
| [fairygui/FairyGUI-Editor](https://github.com/fairygui/FairyGUI-Editor) | 802 | 🔴 **`NONE`** | — | ⚠️ **El editor — la pieza que hace usable al framework — no tiene grant.** Push 2024-03-11 |

> 📐 **Regla de método nº15: la enumeración también encuentra ESCAPES, no sólo
> trampas.** Esta página usaba la provenance para *sospechar*. Acá sirve al
> revés: un grant condicional en la raíz puede tener adentro, vendorizado, al
> padre permisivo que de verdad se necesita. **El trade-off es mantenimiento:**
> el padre es más libre y está más dormido, y su editor no está licenciado.

---

### 📏 Tres valores nuevos para el checksum de bytes de esta KB

| Licencia | Bytes | Dónde se midió |
|---|---|---|
| **BSD 3-Clause** tras preámbulo corporativo de Tencent | **3.473** | `puerts-3.0.3` vendorizado en LGUI. El preámbulo *"Tencent is pleased to support the open source community"* **no altera** el grant: el cuerpo es BSD-3 literal. Mismo patrón que la KB de enterprise midió en Tencent/LoopForge |
| **PolyForm Shield 1.0.0 + Additional Permission** | **8.363** | `LumioGames/LGUI`. ⚠️ Cae **entre** MIT (~1.100) y Apache-2.0 (~11.350): **por tamaño no se distingue de nada conocido**, hay que leer la cabecera |
| **LGPL-3.0 (suplemento sin la GPL-3.0 base)** | **7.652** | `thedivergentai/Game-Dev-Agentic-Skills`. El cycle 11 lo vio por sonda; este pass lo **prueba por enumeración** (`search/code` = 1 archivo, y los 6 nombres de la GPL base dan 404) |

---

## 🔴 El hallazgo del cycle 11 (2026-11-22): **faltaba una CAPA entera, no un repo** — cinco frameworks de game server, 24.000 ★ sumadas, ninguno listado en 11 passes

El cycle 9 encontró un backend de 25.570 ★ que esta página nunca había mirado
(SpacetimeDB) y lo descartó por licencia. Este pass repitió el barrido **por
categoría en vez de por nombre** (`game server framework stars:>1500`) y el
resultado no fue un repo suelto: fue **una familia de cinco**, todos permisivos,
cuatro de ellos vivos, **ninguno** en esta KB.

La sección de backend de esta página listaba Nakama, Open-Match, Colyseus, Talo,
Agones y Supabase — es decir, **el stack que se googlea en inglés**. La familia
que faltaba es la de los *frameworks de servidor de juego* propiamente dichos,
y su centro de gravedad está en **APAC**.

| Repo | Licencia (dos canales) | Lenguaje / forma | ★ | Último push |
|---|---|---|---|---|
| 🆕 [cloudwu/skynet](https://github.com/cloudwu/skynet) | **MIT** (`LICENSE`, 1.085 B) · API `MIT` | Framework de actores en **C + Lua**. El más estrellado de la categoría por un margen grande | **14.199** | 2026-11-15 |
| 🆕 [lonng/nano](https://github.com/lonng/nano) | **MIT** (`LICENSE`, 1.073 B) · API `MIT` | **Go**, ligero, un solo binario. ⚠️ 9 meses sin push | 3.228 | 2026-02-26 |
| 🆕 [topfreegames/pitaya](https://github.com/topfreegames/pitaya) | **MIT** (`LICENSE`, 1.067 B) · API `MIT` | **Go** con *clustering*, y SDKs de cliente para **Unity, iOS y Android** vía el C SDK (`libpitaya`). 547 forks | 2.847 | 2026-11-17 |
| 🆕 [zfoo-project/zfoo](https://github.com/zfoo-project/zfoo) | **Apache-2.0** (`LICENSE`, **11.357 B** — checksum Apache~11.350 ✅) · API `Apache-2.0` | **Java**, RPC + servidor de juego. Única opción JVM de la categoría | 2.005 | 2026-11-19 |
| 🆕 [node-pinus/pinus](https://github.com/node-pinus/pinus) | **MIT** (`LICENSE`, 1.067 B) · API `MIT` | **Node.js / TypeScript**, distribuido | 1.933 | 2026-09-29 |

⛔ **Y el que NO se usa, que es la mitad del hallazgo:**
[NetEase/pomelo](https://github.com/NetEase/pomelo) — **11.855 ★, MIT,
y ARCHIVADO** desde 2023-08-10. Es el framework original de Node.js para juegos
de NetEase, sigue siendo el primer resultado por estrellas, y está muerto desde
hace más de tres años. **`node-pinus/pinus` es su continuación viva**, con una
sexta parte de las estrellas. Quien ordene por popularidad elige el cadáver.

🔵 **Un linaje que conviene saber antes de proponer dos de ellos como alternativas
independientes:** el `LICENSE` de **pitaya** dice literalmente
*"Copyright (c) 2018 TFG Co **and nano Authors**"*, y su README se declara
*"inspired by starx and pomelo and built on top of **nano's networking library**"*.
⇒ **pitaya y nano no son dos opciones separadas: son una rama y su tronco.**
Ponerlos en la misma tabla comparativa de un entregable sin decirlo es un error
de criterio técnico, no de licencia. (Nota: *pomelo → nano → pitaya* es la cadena,
y pomelo está archivado.)

⚠️ **Origen geográfico NO confirmado en este pass.** `topfreegames` es una
organización de GitHub cuya ficha **no se puede leer desde esta sesión**
(`gh api users/<org>` responde *"sessions are bound to their configured
repositories"*). Una búsqueda web no pudo confirmar la atribución a un estudio
brasileño —una fuente incluso advierte que las dos empresas que suelen asociarse
son distintas—. ⇒ **pitaya se lista por su licencia y su actividad, no como
"repo de origen LATAM"**, y esa casilla queda explícitamente vacía hasta que haya
evidencia. Ver `intel/market.md` → LATAM.

### 🆕 El motor web que faltaba en la tabla de engines — y la lección de la minúscula, por segunda vez

| Repo | Licencia (verificada) | Qué aporta | ★ | Último push |
|---|---|---|---|---|
| 🆕 [BabylonJS/Babylon.js](https://github.com/BabylonJS/Babylon.js) | **Apache-2.0** — `license.md` (**9.226 B**), **en minúscula** · API `Apache-2.0` | Motor de render y juego **web** completo, mantenido por Microsoft. La tabla de engines de esta página tenía Godot, o3de, MonoGame, Stride, LÖVE, Defold, Cocos, Panda3D, libGDX, Phaser, Flame, raylib y Bevy — y **ningún engine 3D de navegador de primera línea**. `godogen`, el pack de skills mejor puntuado de esta KB, **ya genera para Babylon.js**: la skill apuntaba a un engine que la KB no tenía | **26.419** | 2026-11-21 |

🔴 **Segunda instancia medida de la misma trampa.** El cycle 7 descubrió que
LÖVE guarda su grant en `license.txt` **en minúscula** y que sondear sólo
`LICENSE.txt` devuelve 404 sobre un repo que sí tiene licencia. Babylon.js repite
el patrón con `license.md`. Dos casos ya no son una anécdota:

> 🆕 **Regla de método nº19 — `raw.githubusercontent.com` distingue mayúsculas.**
> Una sonda que pruebe sólo nombres en mayúscula **fabrica falsos "sin licencia"**.
> La lista mínima de nombres es `{LICENSE, LICENSE.md, LICENSE.txt, license,
> license.md, license.txt, LICENCE, COPYING}` × `{HEAD, rama default}`. Y cuando
> el API dice `Apache-2.0` pero las sondas dan 404, **el que está mal es el juego
> de sondas, no el repo** — exactamente al revés del caso ViZDoom, donde fallan
> los dos canales. Ver `agents/top.md` → cycle 11.

### ⛔ El benchmark canónico de RL sobre juegos es **GPL-2.0** — medido, no supuesto

[Farama-Foundation/Arcade-Learning-Environment](https://github.com/Farama-Foundation/Arcade-Learning-Environment)
(2.463 ★, push 2026-11-15) es **GPL-2.0**, y
[Farama-Foundation/ViZDoom](https://github.com/Farama-Foundation/ViZDoom)
(2.110 ★, push 2026-11-22) **no expone ningún grant legible** (27 sondas en 404).
Los dos son vecinos de Gymnasium y PettingZoo —que esta página lista como MIT— en
la misma fundación. El desarrollo completo está en `agents/top.md` → cycle 11,
con la **regla nº18: la licencia no se hereda de la organización**.

**Consecuencia concreta para la Receta 2 (QA con agentes RL):** el stack
recomendado de esta KB (Gymnasium + stable-baselines3 + ml-agents + godot_rl_agents)
**se sostiene entero en MIT/Apache** y no necesita ni ALE ni ViZDoom. Decirlo por
escrito evita que alguien "complete" el stack con el benchmark de Atari y le
meta GPL-2.0 al entregable.

---

## 🆕 El hallazgo del cycle 10 (2026-11-22): **esta página tenía el navmesh en C++ y el world model con exclusión territorial** — las dos puertas de entrada que faltaban

Dos huecos de **tipo**, no de precisión: no era que una fila estuviera mal, era
que faltaba la variante que un cliente real usa.

### 🆕 Repos fundacionales nuevos — 6 filas, todas verificadas byte a byte

| Repo | Licencia (bytes) | Para qué sirve | ★ | Último push |
|---|---|---|---|---|
| [ikpil/DotRecast](https://github.com/ikpil/DotRecast) | **Zlib** (`LICENSE.txt`, 904) | Port a **C#** de Recast & Detour, el navmesh estándar de la industria. Esta página listaba sólo [recastnavigation/recastnavigation](https://github.com/recastnavigation/recastnavigation) (C++, 8.205 ★, **sin push desde 2026-02-27**) ⇒ **no había camino a Unity ni a ningún stack .NET.** Zlib es la licencia más permisiva del inventario entero | 968 | 2026-10-03 |
| [deeplethe/utopia](https://github.com/deeplethe/utopia) | **Apache-2.0** (`LICENSE`, 11.353) | World model abierto de escala grande, **sin cláusula territorial**. Creado 2026-08-07 | **8.745** | 2026-11-22 |
| [nv-tlabs/lyra](https://github.com/nv-tlabs/lyra) | **Apache-2.0** (`LICENSE`, 11.356) | World models 3D generativos (NVIDIA Toronto AI Lab). **Genera escena 3D, no video** ⇒ la salida entra a un engine. ⚠️ 4 meses sin push | 2.888 | 2026-07-20 |
| [galilai-group/stable-worldmodel](https://github.com/galilai-group/stable-worldmodel) | **MIT** (`LICENSE`, 1.070) | Plataforma de **evaluación reproducible** de world models. Convierte la elección entre utopia / Lyra / Matrix-Game en una medición | 2.315 | 2026-10-02 |
| [Kautenja/gym-super-mario-bros](https://github.com/Kautenja/gym-super-mario-bros) | **MIT** (`LICENSE`, 1.073) | Entornos Gymnasium para NES/Mario con metadata de tarea y recompensa instrumentada. Benchmark clásico **y vivo** | 910 | 2026-09-30 |
| [mindcraft-bots/mindcraft](https://github.com/mindcraft-bots/mindcraft) | **MIT** | Agentes LLM en Minecraft; benchmark MARL. **Slug corregido este pass** — esta KB citaba `kolbytn/mindcraft`, que ya es sólo un redirect | **6.039** | 2026-10-27 |

### 🔴 Por qué `DotRecast` no es "otro navmesh más"

`recastnavigation/recastnavigation` tiene 8.205 ★ y es la referencia — pero es
una librería **C++**, y el 100% de las herramientas de agentes que lista esta KB
para Unity trabajan en **C#**. El puente que cualquiera construye a mano
(P/Invoke sobre la librería nativa, un binario por plataforma de destino) es
precisamente el trabajo que DotRecast ya hizo, bajo **Zlib**, que no pide ni
atribución en binario. **Es el repo con el grant menos restrictivo de todo el
inventario**, y llevaba 10 passes sin aparecer.

### 🔴 La advertencia que acompaña al bloque de world models

Las tres releases abiertas de world models de **Tencent** que esta KB lista
(`HY-World-2.0`, `HY-WorldPlay`, `Hunyuan-GameCraft-1.0`) **no otorgan licencia
en la Unión Europea, el Reino Unido ni Corea del Sur** — verificado byte a byte
en el cycle 10, los tres archivos, misma cláusula en la primera línea. **Las
cuatro filas permisivas de arriba existen para que haya a dónde ir.** Ver
`agents/top.md` (hallazgo nº1) y la **Receta 25**.

### ⚠️ Fundacionales ya listados que están **dormidos** — re-medido este pass

No se quitan (son referencia), pero no se recomiendan como base viva:

| Repo | ★ | Último push | Antigüedad |
|---|---|---|---|
| [joonspk-research/generative_agents](https://github.com/joonspk-research/generative_agents) | 22.399 | 2024-08-05 | **28 meses** |
| [MineDojo/Voyager](https://github.com/MineDojo/Voyager) | 7.339 | 2024-04-03 | **32 meses** |
| [recastnavigation/recastnavigation](https://github.com/recastnavigation/recastnavigation) | 8.205 | 2026-02-27 | 9 meses |
| [MineDojo/MineDojo](https://github.com/MineDojo/MineDojo) | 2.285 | 2024-03-18 | **32 meses** |
| [opendilab/DI-engine](https://github.com/opendilab/DI-engine) | 3.645 | 2025-12-07 | 12 meses |
| [chongdashu/unreal-mcp](https://github.com/chongdashu/unreal-mcp) | 2.119 | 2025-04-22 | **19 meses** + licencia `null` |

> **El patrón, y vale para toda la familia de KBs: los repos fundacionales de
> esta página promedian más estrellas y más meses sin commit que los repos de
> agentes. Es esperable —una librería de navmesh terminada no necesita commits—
> pero significa que `pushed_at` NO se lee igual en las dos tablas.** En
> `agents/top.md`, 7 meses de silencio es un repo muerto. Acá puede ser uno
> terminado. **Lo que no cambia es que haya que medirlo y escribirlo.**

---
## 🔴 El hallazgo del cycle 9 (2026-11-21): **el agujero no era de precisión, era de COBERTURA** — un backend de 25.570 ★ que esta página nunca listó

Ocho passes de forensia de licencias sobre el catálogo existente, cada uno más fino
que el anterior. Y el repo más grande del nicho de backends de juego **no estaba en
ninguna fila de este archivo**: nadie lo había abierto.

### `clockworklabs/SpacetimeDB` — ⛔ **fuera de la ruta permisiva**, con motivo medido

| Campo | Valor medido hoy |
|---|---|
| Repo | [clockworklabs/SpacetimeDB](https://github.com/clockworklabs/SpacetimeDB) |
| ★ / forks | **25.570** / 1.121 |
| Creado / último push / `archived` | 2023-06-17 / **2026-11-21** / `false` |
| GitHub reporta | `NOASSERTION` |
| **Licencia real (byte-read)** | **BSL 1.1** — `LICENSE.txt`, **42.562 B** |
| Change Date → Change License | **2031-11-20** → **AGPL-3.0 con excepción de linking** |
| Additional Use Grant | *"no more than **one SpacetimeDB instance in production**"*, y prohibido ofrecer un *Database Service* |

**Qué es:** una base de datos relacional donde la lógica del servidor corre **dentro
de la base** (módulos Rust o C# compilados a WASM), eliminando la capa de servidor de
juego tradicional. Es el modelo que popularizó BitCraft. Técnicamente, el rival serio
de Nakama.

**Por qué queda afuera:** no por el copyleft de 2031 — para entonces hay excepción de
linking y no obligaría a abrir el código del juego. Queda afuera **por el tope de hoy**:
*una* instancia en producción. Un multijugador comercial que escale horizontalmente
sale del grant el día que levanta la segunda instancia. Para Globant eso es un riesgo
que aparece **en el éxito del proyecto**, no en su fracaso: el peor momento posible.

**Recomendación sin cambios:** [heroiclabs/nakama](https://github.com/heroiclabs/nakama)
(Apache-2.0) por defecto; [TaloDev/backend](https://github.com/TaloDev/backend) (MIT)
cuando el cliente no acepta Apache-2.0; [NamazuStudios/elements](https://github.com/NamazuStudios/elements)
(MPL-2.0) como tercera opción. **SpacetimeDB no entra en esa lista**, y a partir de
hoy está registrado como rechazo, no como ausencia.

#### 🆕 9º eje de partición de un grant: **por subdirectorio, con cláusula de conflicto autocontradictoria**

El preámbulo de 28 líneas de `LICENSE.txt` dice que un subdirectorio con su propio
`LICENSE` manda *"exclusively"* sobre su contenido — **y en el párrafo siguiente** que
ante conflicto *"the base license will govern"*. Las dos no pueden ser ciertas a la
vez. Consecuencia para auditoría: **el grant de un subdirectorio de este repo no se
puede resolver leyendo ese subdirectorio**, y la única lectura defendible ante un
cliente es la conservadora (BSL gobierna todo).

Dato complementario: `licenses/BSL.txt` contiene **el mismo cuerpo legal con 40.980 B**
— exactamente el archivo raíz menos ese preámbulo. **Dos byte-counts, un solo grant:
el checksum tríaga, el íncipit dicta** (regla del cycle 7, revalidada acá).

---

## 🆕 Repos fundacionales incorporados este pass (cycle 9)

Verificados por `gh api search/repositories` **y** byte-read de `LICENSE`.

| Repo | Licencia (byte-read) | ★ | Capa | Por qué es fundacional |
|---|---|---|---|---|
| [scenario-labs/skills](https://github.com/scenario-labs/skills) | **MIT** (`LICENSE`, 1.070 B) | 3.961 | Pipeline de assets | Skills para generar imagen, video, audio y **3D** desde cualquier agente, eligiendo modelo por tarea y **cotizando el costo antes de ejecutar**. Es la primera pieza de esta KB que trata el **presupuesto de generación** como parte del pipeline y no como sorpresa de fin de mes. 350 forks. |
| [CraftJarvis/MineStudio](https://github.com/CraftJarvis/MineStudio) | **MIT** (`LICENSE`, 1.068 B, rama `master`) | 423 | Entrenamiento de agentes | Datos + modelos + entrenamiento + inferencia para agentes de Minecraft en una API. Minecraft sigue siendo el banco de pruebas estándar de agentes encarnados; esta página ya listaba el lado *prompting* (Mindcraft) y le faltaba el lado *training*. |
| [tettethu/VibeGame](https://github.com/tettethu/VibeGame) | **Apache-2.0** (`LICENSE`, 14.096 B — incluye avisos de terceros embebidos) | 389 | Engine AI-native | Engine donde el bucle de construcción lo maneja un sistema multi-agente auto-evolutivo. Creado 2026-08-12. Categoría distinta a "MCP sobre un engine existente": acá el engine **nace** para el agente. |
| [typeonce-dev/ritmo](https://github.com/typeonce-dev/ritmo) | **MIT** (`LICENSE`, 1.072 B) | 170 | ECS *agent-first* | ECS + assets + efectos + sonido + música diseñados para que el consumidor sea un agente. El contrapunto chico y legible de VibeGame. |

### ⚠️ Nota de método sobre los 14.096 B de `VibeGame`

Apache-2.0 estándar ≈ 11.350 B. El excedente de **2.696 B** se leyó, no se asumió:
es una sección `Third-Party Notices` anexada **dentro del propio `LICENSE`**, después
del `APPENDIX`, con texto MIT embebido para dependencias vendorizadas. **No hay
términos adicionales.** El repo es permisivo.

> **Regla (nº15):** una desviación de byte-count **obliga a leer, no a rechazar**.
> El cycle 4 usó el checksum para cazar 4 licencias mal declaradas; acá el mismo
> checksum habría producido un **falso positivo** si se lo hubiera aplicado como
> veredicto. El inventario de terceros, que el cycle 6 encontró en un
> `THIRD_PARTY_NOTICES.md` aparte, también puede vivir **adentro del `LICENSE`**.

### 🔎 Señal de procedencia: el `LICENSE` de un derivado permisivo delata a sus padres

[WenyiWU0111/RSIGame](https://github.com/WenyiWU0111/RSIGame) (Apache-2.0, 11.400 B,
178 ★) **no usa la plantilla en blanco**: donde el template dice
`Copyright [yyyy] [name of copyright owner]`, el archivo dice `Copyright 2025 Google LLC`,
`Copyright 2025 Qwen`, `Copyright 2026 OpenGame contributors`. Apache-2.0 **obliga** a
conservar esos avisos, así que no es un defecto: es la declaración de que RSIGame es
trabajo derivado de código de Google y de Qwen, y de que **quien lo forkee hereda ese
deber de atribución**. Técnica reutilizable en cualquier auditoría.

### 🔴 Las sondas: cuál sirve y cuál no, medido en esta sesión

| Sonda | Repo real | Slug inventado | ¿Sirve? |
|---|---|---|---|
| `curl -sI https://github.com/<slug>` | 403 | **403** | ❌ **no discrimina** |
| `gh api repos/<slug>` | 403 (no habilitada en la sesión) | 403 | ❌ |
| `gh api search/repositories` | devuelve ★/licencia/`archived`/`pushed_at` | nada | ✅ |
| `raw.githubusercontent.com/.../LICENSE` | 200 + bytes reales | 404 + cuerpo de **14 B** | ✅ |

Todas las filas nuevas de arriba se verificaron con las dos últimas.
⛔ **Gap declarado:** `WebFetch` no resuelve hosts generales en esta sesión
(`github.blog`, `es.kotaku.com`, `www.pocketgamer.biz` → `ENOTFOUND`), así que el
editorial de GitHub sobre proyectos open source de gaming **no pudo leerse** y no se
usó como fuente.

---

## 🔴 El hallazgo del cycle 8 (2026-11-21): **un `LICENSE` puede ser un archivo DAÑADO**, y el manifiesto puede contradecirlo

Esta página verifica grants en bytes desde el cycle 3. Todos sus canales dan por
sentado algo que nunca se escribió: **que el archivo leído es una instancia bien
formada de la licencia que declara en la primera línea.**
`Farama-Foundation/Minigrid` (2.543 ★) es el contraejemplo.

| Canal | Veredicto | Evidencia (2026-11-21) |
|-------|-----------|------------------------|
| Badge SPDX | `NOASSERTION` | sin veredicto |
| `LICENSE` (raíz) | dice **"Apache License, Version 2.0"** | **2.507 B / 40 líneas.** Sólo §§ **1** y **9**. **Faltan las §§ 2-8: el grant de copyright (§ 2), el de patente (§ 3) y la redistribución (§ 4).** Línea 18: **`@@ -154,49 +194,23 @@`** — un **hunk de diff** commiteado dentro del archivo |
| `pyproject.toml` | **MIT** | `license = { text = "MIT License" }` + classifier OSI MIT |
| **`docs/LICENSE`** (sólo por enumeración) | 🟢 **MIT + Apache-2.0, composición completa** | **12.201 B**, con los dos textos íntegros y la atribución a la Farama Foundation |

🟢 **Minigrid es usable y permisivo. El archivo a citar es `docs/LICENSE`.**

**Tres reglas de esta página quedan tocadas:**

1. **El checksum de bytes clasifica mal este caso en vez de cazarlo.** 2.507 B con
   íncipit `Apache License` cae en la categoría *"Apache sin apéndice"* que esta
   misma página usa para `libgdx` (10.174 B). **El tamaño distingue "recortado" de
   "canónico"; no distingue "recortado" de "roto".**
2. **Manifiesto y `LICENSE` se contradicen en la FAMILIA de licencia** (MIT vs
   Apache-2.0) y **ninguno es un anuncio** — los dos están versionados en el
   árbol. ⇒ **Regla nueva: manifiesto ≠ `LICENSE` en familia no es un veredicto,
   es una ambigüedad abierta.** Se resuelve buscando el tercer documento, no
   eligiendo entre los dos.
3. **La sonda no omitió el grant real: lo sustituyó**, con `HTTP 200` y un tamaño
   que no dispara la regla del no-hallazgo (< 50 B). Peor que el caso del cycle 6.

**Los hermanos del org están sanos** — `Gymnasium` 1.104 B MIT y `PettingZoo`
1.073 B MIT, **0 artefactos de diff en ambos**. El daño es del repo, no de la
Farama Foundation.

**Chequeo de integridad, mecánico (Receta 21):**

```bash
grep -cE '^\s*(@@|<<<<<<<|=======|>>>>>>>|\+\+\+|---)' LICENSE   # 0 esperado
grep -cE '^ *[0-9]+\. ' LICENSE                                  # Apache-2.0 → 9
```

## 🔴 El censo de inventario del cycle 7 (2026-11-21): **8 engines que esta página no tenía, 158.000 ★ en total**

El cycle 6 cerró con la regla *"una sonda confirma lo que ya sospechabas; sólo la
enumeración te dice lo que te faltaba"* — y la aplicó a los **archivos dentro de
cada repo**. Este pass la aplicó a **la lista de repos**, con una sola consulta en
forma de censo (*"los engines open source más usados"*, sin forma de problema).
Resultado:

| Engine añadido | ★ | Por qué se escapó 6 passes |
|----------------|---|----------------------------|
| `bevyengine/bevy` | **49.914** | Un engine de propósito general **no responde a ninguna consulta con forma de problema**. Sólo aparecía como la palabra *"paridad Bevy"* dentro de la descripción de un pack de skills ajeno |
| `phaserjs/phaser` | **40.756** | Idem. Aparecía como **ítem de una lista dentro de la celda de otra fila** (`awesome-gamedev-agent-skills`), nunca como fila |
| `raysan5/raylib` | **35.758** | Nunca mencionado |
| `libgdx/libgdx` | **25.553** | Nunca mencionado |
| `flame-engine/flame` | 10.821 | Nunca mencionado |
| `cocos/cocos-engine` | 9.941 | Nunca mencionado; badge `NOASSERTION` lo habría descartado igual |
| `defold/defold` | 6.476 | Nunca mencionado; y es el hallazgo de licencia del pass (ver su fila) |
| `panda3d/panda3d` | 5.280 | Nunca mencionado; badge `NOASSERTION` |

Más, en RL y entornos: **`google-deepmind/open_spiel`** (5.603 ★) y
**`google-deepmind/mujoco`** (18.369 ★).

> 📐 **Regla nº14 para toda la familia de KBs:** a partir del 3er pass, **una
> consulta por pass** debe ser un **censo plano contra los incumbentes obvios**,
> formulada como categoría y no como problema. La KB de enterprise descubrió lo
> mismo de forma independiente (le faltaba `google/adk-python`, 22.295 ★). El
> punto ciego de un vocabulario de problemas **tiene la forma exacta de un
> producto**.

🟢 **Y el efecto práctico inmediato:** la pregunta *"¿cuál es la alternativa a
O3DE si el cliente exige MIT y no acepta el Qt/LGPL del editor?"* **ahora tiene
respuesta** (`bevy`, MIT *o* Apache a elección, 49.914 ★) y hasta hoy esta KB no
la tenía.

## 🆕 Enumeración aplicada a esta página (2026-11-21 cycle 6): **lo que una sonda no podía ver**

| Repo de esta página | Archivos de licencia **enumerados** | Qué cambia |
|---------------------|-------------------------------------|-----------|
| 🔴 [limbonaut/limboai](https://github.com/limbonaut/limboai) | `LICENSE.md` (1103, **MIT**) · `LOGO_LICENSE.md` (212, **CC BY 4.0**) · `demo/LICENSE_ASSETS.md` (280, **CC BY 4.0**) | **Esta KB lo publicaba como "MIT" plano.** Es **MIT el código y CC BY 4.0 el logo y los PNG/SVG del proyecto demo**. Forkear el demo como base de proyecto **hereda atribución**. Corregido en `agents/top.md`. |
| 🔴 [SkyworkAI/Matrix-Game](https://github.com/SkyworkAI/Matrix-Game) | **7 archivos** (raíz MIT; `Matrix-Game-1/3/4` Apache-2.0; `licenses/MG4CF-LICENSE` Apache-2.0; `licenses/TAEHV-LICENSE` MIT; `web/mediamtx-reader.LICENSE` MIT) | El pass anterior publicó **2** y declaró que no podía saber si había más. Hay 7, y 3 de las rutas **no eran adivinables**. |
| 🟡 [recastnavigation/recastnavigation](https://github.com/recastnavigation/recastnavigation) | `License.txt` (**zlib**) · `Docs/doxygen-awesome-css/LICENSE` · `RecastDemo/Contrib/implot/LICENSE` · `RecastDemo/Contrib/imgui/LICENSE.txt` | La librería sigue siendo **zlib puro**; el `RecastDemo` **no**: vendoriza imgui/implot (MIT). Embeberlo requiere propagar esos avisos. |
| 🟡 [agones-dev/agones](https://github.com/agones-dev/agones) | `LICENSE` (Apache-2.0) · `proto/grpc-gateway/LICENSE.txt` · `sdks/rust/proto/grpc-gateway/LICENSE.txt` | grpc-gateway vendorizado por duplicado. Benigno; hay que propagar el aviso. |
| 🟢 [heroiclabs/nakama](https://github.com/heroiclabs/nakama) | `LICENSE` (Apache-2.0) · `internal/gopher-lua/LICENSE` | gopher-lua vendorizado. Benigno. **Confirma** la fila. |
| 🟢 [CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp), [hi-godot/godot-ai](https://github.com/hi-godot/godot-ai) | duplicados **byte-idénticos** al de la raíz (+ un Unity Companion License confinado a `TestProjects/` en unity-mcp) | **Falsas alarmas.** Ver el discriminador abajo. |

> **Discriminador de byte-identidad — nuevo y gratis.** Un segundo archivo de
> licencia cuyos **bytes coinciden** con el de la raíz es una **copia de
> packaging** (un subpaquete que embarca su propio LICENSE), **no** una partición
> del grant. Si los bytes **difieren**, hay que leerlo. En este pass separó
> **2 falsas alarmas de 2 hallazgos reales** sin una sola lectura extra: el
> byte-count que esta página ya anota hace todo el trabajo.
>
> ⚠️ **`filename:LICENSE` matchea código, no sólo licencias.** La enumeración
> devolvió `tools/ci_unity_license.py` y `build/extract-licenses.sh`. Filtrar por
> ruta/extensión antes de contar, o el `total_count` infla.

## 🆕 El inventario de dependencias vendorizadas: `THIRD_PARTY_NOTICES.md`

El hallazgo de método con más alcance de este pass. `Matrix-Game-4` publica un
archivo de notices —**cuya existencia sólo se descubre enumerando**— que abre
el **7º eje de partición de un grant: por dependencia vendorizada.** Encabeza
con: *"The project-level Apache-2.0 license does not replace the licenses
below"*, lista 11 componentes y cierra con dos frases que resuelven el gap más
viejo de esta KB: *"**Model and asset distributions carry their own accompanying
license statements**"* y *"**Base-model and codec weight licenses accompany
their upstream distributions**"*.

Entre los 11 componentes aparecen **dos repos que esta misma página recomienda
por separado** — `recastnavigation` (zlib) y Jolt Physics (MIT) — más
[madebyollin/taehv](https://github.com/madebyollin/taehv) (589★, MIT) y, lo que
muerde: **audio de racing bajo CC0 y CC BY 3.0 con obligación de retener
`audio/CREDITS.md`**. ⇒ *"Matrix-Game 4.0 es Apache-2.0"* es cierto para el
código del proyecto y **falso como entrada de inventario**.

> **Regla para toda la familia de KBs: antes de declarar la licencia de un repo
> grande, buscar `THIRD_PARTY_NOTICES`, `NOTICE` y `CREDITS`.** Es el único eje
> de partición que **no** se puede descubrir sondeando nombres de licencia.

## 🟢 El gap de las licencias de PESOS: primer caso verificado en la historia de esta KB

| Modelo | Grant de los **pesos** | Evidencia leída en bytes (2026-11-21) |
|--------|------------------------|---------------------------------------|
| 🆕 [THUSI-Lab/GameScaling](https://github.com/THUSI-Lab/GameScaling) (Apache-2.0, 604★) | 🟢 **Apache-2.0** | El `README.md` dice literal: *"The models are continued pre-trained from **Qwen3.5**, which is also **Apache 2.0**"*. Pesos `bfloat16` desde los backbones Qwen3.5-0.8B/2B/9B/27B; `config.json` declara arquitectura Qwen3.5. ⚠️ **sin código de entrenamiento**; 8 checkpoints, cada uno su propio repo de HF. 🔵 el paper acredita trabajo *"while interning at miHoYo"*. |

> **Regla nueva, mecánica: el grant de los pesos de un modelo derivado está
> acotado por el grant de su modelo base, y el modelo base casi siempre está
> nombrado en el README del repo de código.** Un modelo *continued-pretrained* o
> *fine-tuned* no puede ser más permisivo que su backbone. Convierte una
> pregunta que requería Hugging Face —**host denegado por política del proxy,
> no un fallo de DNS: ver `agents/top.md`**— en una que se contesta con
> `raw.githubusercontent.com` + `grep -i "base model\|pre-trained from"`.
> No sirve para un modelo entrenado desde cero; sirve para la mayoría.

## 🔴 Las dos correcciones del pass 2026-11-20, y por qué el checksum de bytes las caza

Ver `agents/top.md` para la tabla de bandas canónicas. Resumen aplicable aquí:
una fila "MIT" con **3882 B** y una fila "MIT" con **1563 B** no pueden ser MIT
(texto Expat = 1.050–1.150 B). Las dos estaban mal.

| Repo | Decía esta página | Es, leído completo | Por qué importa |
|------|-------------------|--------------------|-----------------|
| `MonoGame/MonoGame` | **MIT** (`LICENSE.txt`, 3882) | 🔴 **Ms-PL** (Microsoft Public License) | §3(D): cualquier porción distribuida **en fuente** sólo puede ir **bajo Ms-PL**. Para binarios da igual; para un **entregable de código a cliente, no se puede relicenciar** MIT/Apache. |
| `PostHog/posthog` | **MIT** (`LICENSE`, 1563) | 🔴 **MIT Expat + carve-out `ee/`** | Idéntico al patrón de Onyx (KB enterprise): *"All content … under the `ee/` directory … licensed under `ee/LICENSE`"* (PostHog Enterprise License, 2176 B). Self-host sigue siendo viable; **"PostHog es MIT" es falso como inventario**. |

## Game Engines

| Repo | Licencia (archivo, bytes) | Descripción | ★ |
|------|--------------------------|-------------|---|
| [godotengine/godot](https://github.com/godotengine/godot) | **MIT** (`LICENSE.txt`, 1149) | Motor completo 2D/3D. Ecosistema AI más rico: LimboAI, Beehave, godot_rl_agents, godot-ai, godot-mcp. ⚠️ default `master`. **97k ★ anotadas en oct → 120.483 reales**: la fila más desactualizada de la KB. | **120.483** |
| [o3de/o3de](https://github.com/o3de/o3de) | 🟡 **Apache-2.0 *o* MIT, a elección** (`LICENSE.txt`, 1475 — es un **puntero**, no el texto) | Open 3D Engine (ex-Amazon Lumberyard). AAA-grade, sin royalties. ⚠️ default **`development`**. Ver advertencia Qt abajo. | 9.894 |
| [MonoGame/MonoGame](https://github.com/MonoGame/MonoGame) | 🔴 **Ms-PL** (`LICENSE.txt`, 3882) — **no MIT**, 🆕 **confirmado 2026-11-21 leyendo la cabecera**: *"Microsoft Public License (Ms-PL)"* (el checksum de ayer acertó) | Framework C# cross-platform. Para equipos .NET añadiendo AI. ⚠️ default **`develop`**. | 14.663 |
| [stride3d/stride](https://github.com/stride3d/stride) | **MIT** (`LICENSE.md`, 1304) | Engine C# cross-platform (ex-Xenko). Alternativa .NET madura a Unity. ⚠️ default `master`. Commit de hoy. | 7.957 |
| [love2d/love](https://github.com/love2d/love) | **zlib** para LÖVE — ⚠️ `license.txt` (74927) es **agregado** · 🆕 **confirmado por lectura directa 2026-11-21**: la línea 8 del archivo dice `License: zlib` para LOVE, y debajo vienen ENet (MIT/Expat), FreeType, GLAD (MIT), glslang (BSD-3), SPIRV-Cross (Apache-2.0 o MIT) y más | LÖVE 2D: Lua, ligero, ideal para prototipos. El archivo agrega ENet (MIT), FreeType, GLAD y texto LGPL/GPL de dependencias: el grant de LÖVE es zlib, el del binario depende de qué se enlace. **Minúsculas: `license.txt`, no `LICENSE.txt`** (404). | 8.981 |
| 🆕 🟢 [bevyengine/bevy](https://github.com/bevyengine/bevy) | 🟢 **MIT *o* Apache-2.0, a elección** — `LICENSE-MIT` (1036) **y** `LICENSE-APACHE` (10173, sin apéndice). 🔴 **El badge dice sólo `Apache-2.0` y sub-declara el grant.** Canal 3 enumera **164 archivos de licencia**: el doble grant está replicado **en cada crate** (`crates/bevy_ecs/LICENSE-MIT`, `crates/bevy_pbr/…`), no sólo en la raíz | **La omisión más grande en la historia de esta KB: 6 passes sin listarlo.** Engine ECS en Rust, data-driven, 2D/3D, el 2º engine open source más estrellado después de Godot. Releases **0.19 (19-jun-2026)** y **0.20 (8-oct-2026, rendering Solari + sistema de escenas BSN)**. Para un cliente que exige MIT es **la alternativa AAA-grade a O3DE sin el problema de Qt/LGPL**. ⚠️ Rust: el equipo del cliente lo tiene o no lo tiene. | **49.914** |
| 🆕 [phaserjs/phaser](https://github.com/phaserjs/phaser) | **MIT** (`LICENSE.md`, **1100** — coincide **exacto** con el checksum canónico MIT) | Engine 2D HTML5/WebGL: el estándar de facto de juegos web y el engine que **`OpenGame` usa como target de generación**. Camino más corto a un juego jugable en navegador, sin instalación. ⚠️ default `master`. push 2026-08-21. | **40.756** |
| 🆕 [raysan5/raylib](https://github.com/raysan5/raylib) | **Zlib** (`LICENSE`, 877) | Librería C para juegos, sin dependencias, extremadamente simple. **Zlib es permisiva y sin cláusula de atribución en binario** ⇒ la más limpia del inventario para firmware/embedded y para entregables donde el cliente no quiere avisos. Commit de hoy. | **35.758** |
| 🆕 [libgdx/libgdx](https://github.com/libgdx/libgdx) | **Apache-2.0** (`LICENSE`, **10174 — sin apéndice**; ver regla nº15 en `agents/top.md`) | Framework Java/JVM cross-platform (desktop, Android, iOS, web). **La opción JVM**: si el cliente es una casa Java —común en LATAM y en enterprise— es el engine que su equipo ya puede mantener. push 2026-11-19. | **25.553** |
| 🆕 [cocos/cocos-engine](https://github.com/cocos/cocos-engine) | 🟢 **MIT** (`LICENSE.md`, 1164) — **badge `NOASSERTION`, permisivo detrás del badge**. Los 1164 B (vs 1100 canónico) son **dos líneas de copyright** (Chukong 2013-2016 + Xiamen Yaji 2017-2023), no cláusulas añadidas | Cocos Creator: engine 2D/3D muy fuerte en **mobile de APAC** (China en particular). Relevante por región más que por estrellas. push 2026-10-28. | 9.941 |
| 🆕 [flame-engine/flame](https://github.com/flame-engine/flame) | **MIT** (`LICENSE`, 1067) | Engine de juegos sobre **Flutter/Dart**: la vía cuando el cliente ya tiene una app Flutter y quiere gamificar dentro de ella, en vez de incrustar un engine ajeno. Commit de hoy. | 10.821 |
| 🆕 [panda3d/panda3d](https://github.com/panda3d/panda3d) | 🟢 **BSD-3-Clause** (`LICENSE`, 1569) — **badge `NOASSERTION`, permisivo detrás del badge.** El clasificador no lo ve porque el archivo **no es la plantilla BSD verbatim**: no tiene cabecera SPDX y **se auto-declara en prosa al final** (*"(This is the Modified BSD License.)"*) | Engine 3D en C++ con API Python, origen Disney/CMU. **La opción Python**: equipos de data science / ML que quieren un engine sin salir de Python. push 2026-07-28. | 5.280 |
| 🆕 🔴 [defold/defold](https://github.com/defold/defold) | 🔴 **`Defold License 1.0` (`LICENSE.txt`, 9977) — NO es open source.** Apache-2.0 verbatim + cláusula **4(a): prohibido comercializar la obra o derivados como *"Game Engine Product"*** (= software de desarrollo de videojuegos, autoría **y** runtime). Falla el punto 6 de la OSD ⇒ **source-available**. Canal 3: 37 archivos de licencia, incl. `COMPLYING_WITH_LICENSES.md` y `TOOLS_LICENSES.md` | Engine 2D/3D ligero (ex-King), muy usado en mobile. Commit de hoy, proyecto sanísimo. 🔴 **Pero la restricción apunta al entregable de Globant, no al del cliente:** hacer **un juego** con Defold está dentro del grant; vender un **pipeline de autoría asistido por AI** (Recetas 6 y 11) está fuera. | 6.476 |

### 🟡 O3DE: dos matices que esta página no decía

1. **No es "Apache-2.0" a secas: es Apache-2.0 *o* MIT a elección del usuario**, y
   las contribuciones deben hacerse **bajo ambas**. Es *más* permisivo de lo que
   decía esta página, y para un cliente que exige MIT es una buena noticia
   que estábamos ocultando.
2. 🔴 **Pero arrastra copyleft de terceros, y el propio `LICENSE.txt` lo dice:**
   *"certain materials are subject to 'copyleft' licenses, which require
   distribution of source code, including: **Qt Toolkit … LGPL v3** (with certain
   exceptions)"*. Esta página vendía O3DE como "enterprise **sin royalties**" sin
   mencionar que el editor enlaza **Qt bajo LGPL-3.0**. Sin royalties ≠ sin
   obligaciones. Para distribuir el **editor** modificado hay que resolver Qt;
   para distribuir sólo el **runtime del juego**, normalmente no.

## 🆕 La fila que este pass vuelve **más permisiva**: C2PA es MIT *o* Apache-2.0

El stack de Content Credentials es la pieza central de la Receta 7 (marcado del
Art. 50(2) del EU AI Act). Esta KB lo publicaba como **Apache-2.0**. Son **dos
licencias a elección**, y los dos archivos están presentes en los dos repos:

| Repo | `LICENSE-MIT` | `LICENSE-APACHE` | Grant real |
|------|---------------|------------------|-----------|
| [contentauth/c2pa-rs](https://github.com/contentauth/c2pa-rs) | 200, **1083 B** | 200, **11335 B** | **MIT *o* Apache-2.0** |
| [contentauth/c2pa-python](https://github.com/contentauth/c2pa-python) | 200, **1083 B** | 200, **11335 B** | **MIT *o* Apache-2.0** |

Importa para engagement: un cliente con **política de sólo-MIT** —que existe, y
es la razón por la que esta KB mantiene `Unity-MCP` de Murzak como alternativa
Apache— **puede tomar C2PA bajo MIT** y cumplir el Art. 50(2) sin excepción
legal. Lo estábamos ocultando, igual que con O3DE. **Es el tercer caso de la KB
en que el repo era *más* libre de lo publicado** (O3DE, `elements`, C2PA), contra
cinco en que era menos. El sesgo del error no es uniforme: **leer mal una
licencia perjudica al cliente en las dos direcciones, y la dirección "más
permisivo de lo que creíamos" nunca dispara una alarma**, porque nadie se queja
de una restricción que no existe.

### 🆕 Por qué `UnrealMLAgents` figuraba sin licencia resoluble

`AlanLaboratory/UnrealMLAgents` (port de ML-Agents a Unreal, 91★) devuelve
`NOASSERTION`. Su `LICENSE` son **609 B** y dicen, en este orden:

```
Copyright © 2025 Stephane Capponi and individual contributors. All Rights Reserved.
Licensed under the Apache License, Version 2.0 (the "License"); …
```

Es **Apache-2.0** (fila correcta en esta KB), y lo que rompe al clasificador es
el *"All Rights Reserved."* **arriba** del grant. Es el mismo mecanismo que la KB
de enterprise documentó en `Tencent/LoopForge` (MIT detrás de un preámbulo
corporativo): **un preámbulo propietario sobre un grant permisivo produce
`NOASSERTION`, y leer sólo el badge convierte un repo Apache-2.0 en "sin
licencia"**. 609 B está **por debajo** de la banda Apache (~11.350) ⇒ archivo
puntero, hay que seguir el puntero — la regla asimétrica de ayer, aplicada.

## Multiplayer Backends

| Repo | Licencia (archivo, bytes) | Descripción | ★ |
|------|--------------------------|-------------|---|
| [heroiclabs/nakama](https://github.com/heroiclabs/nakama) | **Apache-2.0** (`LICENSE`, 11358) | Backend open source más completo. SDK oficial Godot/Unity/Unreal. Extensible en Lua/TS/Go: ahí se inyecta la capa AI. ⚠️ default `master`. | 13.757 |
| [heroiclabs/nakama-godot](https://github.com/heroiclabs/nakama-godot) | **Apache-2.0** (`LICENSE`, 11359) | SDK oficial de Nakama para Godot. | — |
| [googleforgames/open-match](https://github.com/googleforgames/open-match) | **Apache-2.0** (`LICENSE`, 11345) | Framework de matchmaking de Google. Algoritmos plug-in: entra un modelo propio. ⚠️ 4 meses sin commits (2026-07-12). | 3.427 |
| [colyseus/colyseus](https://github.com/colyseus/colyseus) | **MIT** (`LICENSE`, 1075) | Server multijugador Node.js/TypeScript. Ideal para web games. ⚠️ default `master`. | 7.565 |
| [TaloDev/backend](https://github.com/TaloDev/backend) | **MIT** (`LICENSE`, 1074) | Backend self-hostable: auth, stats, leaderboards, saves, event tracking, exports CSV, canales realtime, live config, presencia, relaciones. Steamworks + Google Play Games + Apple Game Center. Clientes Godot y Unity. **MIT completo, sin tier cerrado.** ⚠️ default **`develop`**. Commit de hoy. | 105 |
| 🆕 [TaloDev/godot](https://github.com/TaloDev/godot) | **MIT** (`LICENSE`, 1074) — **4 archivos enumerados, 4 MIT** (`addons/talo` 1.074, `addons/gdUnit4` 1.069, `addons/GDQuest_GDScript_formatter` 1.072) | Cliente **Godot** de Talo. 🔴 **Tiene 2,4× las ★ del backend: es la puerta de entrada real del producto, y esta página no lo listaba.** 16 forks. | **251** |
| 🆕 [TaloDev/unity](https://github.com/TaloDev/unity) | **MIT** (`LICENSE`, 1074) — 2 archivos | Paquete **Unity** de Talo. Se distribuye por el Unity Asset Store (`TaloDev/unity-package` quedó **archivado** 2025-04-10). | 43 |
| 🆕 [TaloDev/frontend](https://github.com/TaloDev/frontend) | **MIT** (`LICENSE`, 1074) | Dashboard de gestión de Talo. Completa el stack self-hosteable: servidor + clientes + consola, todo MIT. | 21 |
| 🆕 ⛔ [TaloDev/hosting](https://github.com/TaloDev/hosting) | 🔴 **SIN GRANT — enumeración = 0 archivos de licencia** | *"Self-hosting starter pack for Talo"*. 🔴 **El producto es MIT; el camino de despliegue que el propio proyecto documenta NO está otorgado.** Para un cliente que elige Talo *por* self-hosting, el gate está en el runbook, no en el servidor. **Mitigación: desplegar `TaloDev/backend` con IaC propia y no copiar `hosting/`.** | 36 |
| 🟢 [NamazuStudios/elements](https://github.com/NamazuStudios/elements) | **MPL-2.0** (`LICENSE.txt`, 16726) — **no AGPL-3.0** | **Re-admitido este pass.** Esta KB lo había descartado por "copyleft de red"; es MPL-2.0, copyleft **por archivo**: hostearlo **no** obliga a liberar tu código. Backend self-hosted con REST/WebSockets y OAuth2/OIDC. **Chico (22★)**: alternativa a considerar, no default. Commit de hoy. | 22 |
| [supabase/supabase](https://github.com/supabase/supabase) | **Apache-2.0** (`LICENSE`, 11338) | BaaS para juegos asíncronos: profiles, inventarios, leaderboards, UGC. `pgvector` para RAG sin servicio externo. **80k ★ anotadas → 115.201 reales.** | **115.201** |

## 🆕 Orquestación de game servers — la capa que faltaba

Esta KB tenía `pterodactyl` (panel, bare-metal/Docker) y **nada para Kubernetes**.
Agones es el estándar de facto del nicho y **estaba ausente desde el primer pass**.

| Repo | Licencia | Descripción | ★ |
|------|----------|-------------|---|
| 🆕 [agones-dev/agones](https://github.com/agones-dev/agones) | **Apache-2.0** | Hosting, scaling y orquestación de **game servers dedicados sobre Kubernetes**: `Fleet`, `GameServer` y `FleetAutoscaler` como CRDs. Proyecto CNCF-adyacente, usado en producción para multiplayer de sesión. **Commit de hoy.** Es la pieza que convierte "tengo un binario de servidor" en "tengo capacidad elástica". | 7.106 |

🔴 **Rename:** el slug `googleforgames/agones` que aparece en toda la documentación
y en los blogs **ya no es el canónico**; `repo:googleforgames/agones` →
`NOT_FOUND`, mientras `raw` del slug viejo sigue devolviendo 200 (redirect, no
borrado). El proyecto salió de la org de Google a **`agones-dev`**. Misma firma
que el rename de `casbin` en la KB de enterprise.

### 🔴 2026-11-21 (cycle 6): **`rivet-gg/opengb` — "Open Game Backend" — ya no existe como producto de gaming**

El *Open Game Backend* figura en la prensa de backends de juego como la
alternativa Apache-2.0 a Nakama. Esta página **nunca lo listó**; conviene saber
por qué no hay que agregarlo. Trazado hoy, es una **cadena de renames de 3
saltos**:

| Paso | Slug | `search/repositories` | `raw` | Qué dice |
|------|------|----------------------|-------|----------|
| 1 | `rivet-gg/opengb` | 422 | **200** | `README.md`: *"# Open Game Backend — **Moved to https://github.com/rivet-gg/toolchain**"* |
| 2 | `rivet-gg/toolchain` | 422 | **200** | *"# **Archived Repository** — ⚠️ archived and no longer maintained. **It has been moved to `rivet-gg/rivet`**"* |
| 3 | `rivet-gg/rivet` | 422 | **200** | redirect: la **organización** pasó de `rivet-gg` a `rivet-dev` |
| 4 | ✅ [rivet-dev/rivet](https://github.com/rivet-dev/rivet) | **6.970★, Apache-2.0 (`LICENSE`, 11350), `pushed_at` 2026-11-21** | 200 | slug canónico |

**Y lo que encontramos al llegar es más importante que el rename.**
`rivet-dev/rivet` ya no se describe como backend de juego: hoy es *"the
orchestrator for agentic workloads"* (*"4,99 ms p99 cold starts, 72 KB RAM per
Actor"*, *"create one for each **agent**, user, session, or tenant"*). La misma
org publica [rivet-dev/agentos](https://github.com/rivet-dev/agentos) (**4.912★**,
Apache-2.0, sandboxes WASM) y [rivet-dev/agents](https://github.com/rivet-dev/agents)
(1.652★, Apache-2.0). **La empresa que construyó el Open Game Backend abandonó
el nicho de gaming y revendió su tecnología de orquestación de game servers
como infraestructura de agentes AI.** Ver `intel/trends.md` nº 18.

⇒ **Consecuencia operativa: la alternativa Apache-2.0 a Nakama para orquestación
sigue siendo `agones-dev/agones`.** OpenGB no está "sin mantenimiento": dejó de
existir como producto de gaming. 🔵 Dos lecciones de método: la firma de rename
**hay que iterarla** (seguir el README del repo viejo te deja en otro 404), y un
rename de **organización** invalida *todos* sus punteros a la vez — es el modo
de fallo de rename con mayor radio de daño.

## RL y entornos de entrenamiento

| Repo | Licencia (archivo, bytes) | Descripción | ★ |
|------|--------------------------|-------------|---|
| [Farama-Foundation/Gymnasium](https://github.com/Farama-Foundation/Gymnasium) | **MIT** (`LICENSE`, 1104) | API estándar de entornos RL. Sucesor mantenido de `openai/gym`. | 12.843 |
| [Farama-Foundation/PettingZoo](https://github.com/Farama-Foundation/PettingZoo) | **MIT** (`LICENSE`, 1073) | Equivalente multi-agente de Gymnasium. Para NPCs que compiten o cooperan. | 3.556 |
| [DLR-RM/stable-baselines3](https://github.com/DLR-RM/stable-baselines3) | **MIT** (`LICENSE`, 1075) | PPO, SAC, A2C, DQN en PyTorch. El algoritmo por defecto para QA bots. ⚠️ default `master`. | 14.038 |
| [Unity-Technologies/ml-agents](https://github.com/Unity-Technologies/ml-agents) | **Apache-2.0** — 🔴 **corregido 2026-11-21 (cycle 7): el grant está partido por RAMA.** `LICENSE.md` = **572 B en `develop`** (rama default: es el **aviso corto que Apache recomienda**, grant válido por referencia) y **11.324 B en `main`** (texto completo). La KB publicaba *"cabecera-puntero, no el texto completo"*, que es cierto de la rama default y **falso del repo** | Toolkit RL oficial de Unity. ⚠️ default **`develop`**. Commit de hoy. Badge `NOASSERTION`. | **19.890** |
| [edbeeching/godot_rl_agents](https://github.com/edbeeching/godot_rl_agents) | **MIT** (`LICENSE`, 1071) | Puente Godot ↔ Python para 4 frameworks RL. **900+ → 1.639 (+82%)**: mayor salto relativo de la KB. | 1.639 |
| 🆕 🟢 [google-deepmind/open_spiel](https://github.com/google-deepmind/open_spiel) | **Apache-2.0** (`LICENSE`, 11358 — canónico) | **El framework de referencia de RL y search/planning en juegos, y faltaba.** Juegos en C++ con wrapper Python; n-jugadores, suma cero, cooperativos y suma general; CFR, Q-learning, SARSA, regret matching, DQN, PPO, A2C. **Es la base correcta para información imperfecta** (póker, Guandan, negociación) donde PPO sobre Gymnasium no alcanza. v1.6.11 publicada 5-ene-2026, push 2026-11-14. ⚠️ default `master`. | 5.603 |
| 🆕 [google-deepmind/mujoco](https://github.com/google-deepmind/mujoco) | **Apache-2.0** (`LICENSE`, 11358) | Motor de física para control y robótica, el estándar de los benchmarks de locomoción. En gaming sirve para **animación procedural y ragdoll/control físico aprendido**, no para gameplay. Commit de hoy. | 18.369 |

| 🆕 🟢 [opendilab/LightZero](https://github.com/opendilab/LightZero) | **Apache-2.0** (`LICENSE`, 11357 — canónico) | **MCTS + familia AlphaZero/MuZero** (incl. sampled y stochastic) en un toolkit unificado. **La capa que faltaba:** PPO/SAC sobre Gymnasium dan políticas **reactivas**; esto hace **búsqueda con modelo aprendido** ⇒ base correcta para **juego de tablero/cartas, oponentes que planifican y dificultad calibrada**. Push 2026-11-19. | 1.663 |
| 🆕 🟢 [opendilab/DI-engine](https://github.com/opendilab/DI-engine) | **Apache-2.0** (`LICENSE`, 11342) | Plataforma RL general del mismo laboratorio; el catálogo de algoritmos más amplio del inventario, con entornos de juego integrados. **LightZero es su módulo de búsqueda.** ⚠️ **push 2025-12-07 (~11,5 meses)**: vivo pero lento, verificar antes de ponerlo en un roadmap. | 3.645 |
| 🆕 🟢 [Farama-Foundation/Minigrid](https://github.com/Farama-Foundation/Minigrid) | 🟡 **MIT + Apache-2.0 (composición)** — leer **`docs/LICENSE`** (12.201 B); **`LICENSE` (2.507 B) está DAÑADO**, ver el hallazgo de arriba | Entornos de grilla con llaves, puertas, objetivos y **misiones en lenguaje natural**. El camino barato: **probar la lógica del agente sin pagar un engine 3D**, y migrarlo después por el mismo API Gymnasium. Push 2026-11-14. | 2.543 |
| 🆕 [Farama-Foundation/MicroRTS-Py](https://github.com/Farama-Foundation/MicroRTS-Py) | **MIT** (`LICENSE`, 1117) | RTS en miniatura para RL: **estrategia en tiempo real** (economía, build order, micro) a escala de laboratorio. ⚠️ push 2025-08-11 ⇒ señal, no base de producción. | 295 |
| 🆕 ⚠️ [MineDojo/MineDojo](https://github.com/MineDojo/MineDojo) | **MIT** (`LICENSE`, 1070) | La suite de entornos y el benchmark de Minecraft sobre los que se construyó `Voyager`. Hueco histórico: la KB tenía el agente y no el entorno. 🔴 **push 2024-03-18** ⇒ benchmark de lectura, no base viva. | 2.285 |
| 🆕 ⛔ [openai/gym](https://github.com/openai/gym) | — | 🔴 **ARCHIVADO.** Sucesor: `Farama-Foundation/Gymnasium` (arriba). Se lista **para que nadie lo proponga**: con 37 k ★ encabeza cualquier búsqueda de *"RL environments"*. | 37.237 |


## 🆕 Foundation models de agente de juego (nuevo en esta página, 2026-11-21 cycle 6)

Categoría que esta página no tenía: **no un LLM con tools, un modelo de acción**.
Las dos filas son código de investigación (23 y 35 forks), no producto — se
listan porque son los únicos puntos de partida permisivos del nicho.

| Repo | Licencia (archivo, bytes) | Descripción | ★ |
|------|--------------------------|-------------|---|
| 🆕 [RainBowLuoCS/Gamer](https://github.com/RainBowLuoCS/Gamer) | **Apache-2.0** (`LICENSE`, 11358) | *Generalist **Vision-Language-Action** foundation model* para agentes de juego de **horizonte largo y tiempo real**. Primera fila VLA de esta KB. 23 forks. | 1.189 |
| 🆕 [THUSI-Lab/GameScaling](https://github.com/THUSI-Lab/GameScaling) | **Apache-2.0** (11358) — 🟢 **y los pesos también** | *Continued pre-training* para agentes embodied en mundos virtuales diversos: 8 checkpoints 0,8B–27B desde Qwen3.5, evaluación en 4 benchmarks de juego. **El único modelo de esta KB con grant de pesos verificado.** ⚠️ sin código de entrenamiento. 35 forks. | 604 |
| 🆕 [Calix-L/DanKS](https://github.com/Calix-L/DanKS) | **Apache-2.0** (11348) + `NOTICE` (453) | Agente de **Guandan** (掼蛋) por RL a escala chica. Patrón útil: **información imperfecta + juego por equipos**, el caso que los agentes LLM todavía no resuelven. 🔵 APAC. | 833 |
| 🆕 [Station-Sciences/bot-crossing](https://github.com/Station-Sciences/bot-crossing) | **MIT** (`LICENSE`, 1069) | *"A video game for AI agents"*: **el juego como entorno de evaluación**, no el agente. 286 forks. La pieza viva que `lmgame-org/GamingAgent` (congelado desde 2025-11-16) dejó de ser. | 906 |
| 🆕 [QwenLM/Qwen3](https://github.com/QwenLM/Qwen3) | 🟡 **badge `NONE` · enumeración = 0 archivos** → 🟢 **Apache-2.0 declarado en `README.md` §*License Agreement*** (*"All our open-weight models are licensed under Apache 2.0"*; archivos en Hugging Face, host que **no resuelve en esta sesión**) | 🆕 **La capa de SLM que esta página no tenía.** NVIDIA publicó soporte de **Qwen3-8B como SLM on-device en ACE** para NPCs de PC: es el modelo que corre **en la máquina del jugador**, no en una nube. Por eso es fundacional para gaming y no sólo para NLP — y por eso desarma de raíz la obligación de *runtime* que esta KB documenta en NA. 2.113 forks. ⚠️ Leer la *model card* del tamaño exacto antes de embarcar: esta KB ya midió que **el grant se parte por variante de modelo**. | **27.758** |

| 🆕 ⛔ [MineDojo/NitroGen](https://github.com/MineDojo/NitroGen) | ⛔ **`NVIDIA License`** (`LICENSE`, **4729**) — **uso NO COMERCIAL (§ 3.3) y VIRAL (§ 3.2)** | Foundation model de agente **generalista** de NVIDIA: behaviour cloning sobre **40.000 h de gameplay en +1.000 juegos**, wrapper Gymnasium, pesos en HF (`nvidia/NitroGen`), **oral en CVPR 2026** (arXiv 2601.02427). § 3.3: *"only may be used or intended for use **non-commercially**… non-commercial research purposes only"*; § 3.2 obliga a arrastrar esa limitación a todo derivado; § 3.7 arrastra el **Meta OPT-IML 175B License Agreement**; § 3.4 termina el grant ante un reclamo de patente. ⛔ **Fuera para todo entregable, incluido un PoC pago.** 🔴 El paper dice *"we open-source the dataset, simulator, and pre-trained weights"*: **"open-source" en un abstract de CVPR no es un grant.** | 2.251 |


## Arquitecturas de agente de referencia

| Repo | Licencia (archivo, bytes) | Descripción | ★ |
|------|--------------------------|-------------|---|
| [joonspk-research/generative_agents](https://github.com/joonspk-research/generative_agents) | **Apache-2.0** (`LICENSE`, 11335) | El paper de Stanford convertido en código. Memory stream + reflection + planning. ⚠️ **`pushed_at` 2024-08-05: 27 meses sin commits.** Esta página decía *"sigue siendo la arquitectura de referencia"* — es verdad del **patrón**, y engañoso del **código**: es un artefacto de paper congelado, no una base mantenida. | 22.399 |
| 🆕 [a16z-infra/ai-town](https://github.com/a16z-infra/ai-town) | **MIT** | La contraparte **viva y desplegable** del anterior: starter kit MIT de un pueblo de agentes que conversan y socializan, pensado para hacerle fork. **Commit de hoy.** Si el patrón `generative_agents` es el destino, éste es el vehículo. | 10.835 |
| [google-deepmind/concordia](https://github.com/google-deepmind/concordia) | **Apache-2.0** (`LICENSE`, 11359) | Simulación social generativa de DeepMind. Para **poblaciones** de NPC, no un NPC. Commit de ayer. | 1.911 |
| [lmgame-org/GamingAgent](https://github.com/lmgame-org/GamingAgent) | **MIT** (`LICENSE`, 1065) | ICLR 2026. Harness + benchmark de agentes que juegan. ⚠️ **un año exacto sin commits** (2025-11-16): instrumento válido, proyecto congelado. | 1.006 |

## Pathfinding

| Repo | Licencia (archivo, bytes) | Descripción | ★ |
|------|--------------------------|-------------|---|
| [recastnavigation/recastnavigation](https://github.com/recastnavigation/recastnavigation) | **zlib** (`License.txt` — ojo la capitalización) | Estándar de industria navmesh. Ya embebido en Godot, Unity, UE. | 8.203 |
| [Mugen87/yuka](https://github.com/Mugen87/yuka) | **MIT** (`LICENSE`, 1072) | Game AI primitives JS: steering, pathfinding, FSM. ⚠️ default `master`. | 1.466 |

## Analytics, observabilidad y ops

| Repo | Licencia (archivo, bytes) | Descripción | ★ |
|------|--------------------------|-------------|---|
| [PostHog/posthog](https://github.com/PostHog/posthog) | 🔴 **MIT Expat + carve-out `ee/`** (`LICENSE`, 1563 + `ee/LICENSE`, 2176) — **no "MIT"** | Product analytics self-hosted. Base de player behavior y churn. El core sigue siendo utilizable y self-hostable; lo que cambia es **la entrada de inventario** y la obligación de no prometer "todo MIT". **Corrección de este pass.** | **40.990** |
| [pterodactyl/panel](https://github.com/pterodactyl/panel) | **MIT** (`LICENSE.md`, 1137 — verificado, el API dice `NOASSERTION` porque el texto va dentro de un *code fence* de Markdown) | El panel de game servers open source más usado. ⚠️ default **`1.0-develop`**. | 9.533 |
| [grafana/grafana](https://github.com/grafana/grafana) | 🔴 **AGPL-3.0** (`LICENSE`, 34523) — **no Apache-2.0** | Dashboards de player analytics. Corrección de octubre **revalidada por el 2º canal** (SPDX = `AGPL-3.0`). Copyleft de red: exponer el dashboard a terceros puede obligar a liberar modificaciones. Alternativas: Apache Superset (Apache-2.0), Metabase (AGPL-3.0 core / comercial). | 78.014 |

> **Por qué `pterodactyl` explica el mecanismo de `NOASSERTION`.** Su `LICENSE.md`
> es MIT **envuelto en un bloque de código Markdown**, y la línea de copyright
> dice *"Pterodactyl® Copyright © Dane Everitt"*. El detector de GitHub no
> matchea, devuelve `NOASSERTION`, y un pass que confiara sólo en el API habría
> publicado "sin licencia" un repo MIT limpio de 9.536★. **Es el falso positivo
> simétrico de las 4 correcciones** — y la razón de que los dos canales sean
> obligatorios y ninguno suficiente.
>
> 🆕 **2026-11-21 — releído en bytes y medido en escala:** de los **16** repos de
> esta KB con badge `NOASSERTION`/`null`, **7 son permisivos**, 8 restrictivos y
> 3 no tienen grant. ⇒ `NOASSERTION` **no es señal útil en ninguna dirección**
> (~44% permisivo); es sólo *"el clasificador no pudo decidir"*. Censo completo
> en `repos/trending.md` (2026-11-21).
> ⚠️ **El `®` tiene consecuencia propia:** MIT concede derechos de **copyright**,
> no de **marca**. Un panel re-brandeado para un cliente puede usar el código y
> **no** el nombre "Pterodactyl" ni su logo.

## 🔴 Filas retiradas

| Repo | Motivo |
|------|--------|
| [pelican-dev/panel](https://github.com/pelican-dev/panel) | **Agravado: el repo ya no existe.** En octubre era "vivo pero sin archivo de licencia"; hoy `repo:` → `NOT_FOUND` y `raw` → 404 en `main`. Usar `pterodactyl/panel`. |

---
## Cómo leer la columna de estrellas

**Todas las ★ de esta página se releyeron el 2026-11-20** vía
`gh api search/repositories`. Sin asteriscos pendientes. Lo que cambió más desde
las cifras heredadas de julio, que el pass de octubre no pudo refrescar porque
creía el canal bloqueado:

| Repo | Anotado antes | Real 2026-11-20 | Δ |
|------|---------------|-----------------|---|
| `godotengine/godot` | 97k* | **120.483** | +24% |
| `supabase/supabase` | 80k* | **115.201** | +44% |
| `PostHog/posthog` | 23k* | **40.990** | +78% |
| `grafana/grafana` | 67k* | **78.014** | +16% |
| `MonoGame/MonoGame` | 11k* | **14.663** | +33% |
| `godot_rl_agents` | 900+* | **1.639** | +82% |
| `o3de/o3de` | 7.8k* | **9.894** | +27% |
| `love2d/love` | 4.5k* | **8.981** | +100% |

**Lección de instrumento, no de contenido:** un canal que se registra como
"bloqueado" congela una columna entera de la KB durante meses. El pass de octubre
dejó ~20 filas con `*` por esa creencia. **Cuando un canal se declara bloqueado,
el próximo pass debe reintentarlo, no heredar la conclusión.**

---
*Pass 2026-11-21 (cycle 6): 3er canal agregado (`gh api search/code` → enumeración
de archivos de licencia); 2 filas corregidas por enumeración (LimboAI → MIT +
CC BY 4.0; Matrix-Game → 7 archivos, no 2); 7º eje de partición de grant (por
dependencia vendorizada, vía `THIRD_PARTY_NOTICES.md`); **1er grant de pesos
verificado de la KB** (GameScaling ← Qwen3.5) con la regla del modelo base;
trap de los 14 bytes documentado; discriminador de byte-identidad (2 falsas
alarmas separadas de 2 hallazgos reales); categoría nueva de **foundation models
de agente de juego** con 4 filas; cadena de renames de 3 saltos en
`rivet-gg/opengb` + el pivot de Rivet a infraestructura de agentes. 2026-11-21.*

*Ver también: `verticals/solutions.md` para plataformas verticales completas y
`agents/top.md` para el método de verificación, el checksum de bytes y los grants
bloqueantes.*
