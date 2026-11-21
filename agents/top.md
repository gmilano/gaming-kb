---
industry: gaming
region: Global
updated: 2026-11-21
---

# 🎮 Agentes AI — Gaming

> Agentes y herramientas AI open source. Foco: MIT / Apache 2.0 / BSD / Zlib.
> Última actualización: **2026-11-21 (pass cycle 6)**. Passes previos: 2026-11-21
> (cycle 5), 2026-11-20, 2026-10-26, 2026-07-02.
>
> ⏱️ **Este pass corre ~1 hora después del anterior. No mide crecimiento: no
> puede.** Ninguna ★ se movió de forma interpretable en 60 minutos, así que **no
> se reportan deltas** y no se reescribe ninguna cifra heredada que no haya
> cambiado de significado. Lo que sí se puede hacer en una hora es **abrir un
> canal de verificación nuevo y pasarlo por lo que ya estaba escrito** — y eso
> es lo que hace este pass.
>
> **Método — TRES canales, y el tercero es nuevo y cambia la naturaleza del
> trabajo:**
> 1. `gh api search/repositories -f q='repo:<slug>'` → existencia + **SPDX** + ★ +
>    `archived` + `pushed_at` + `default_branch`, en una llamada.
> 2. `raw.githubusercontent.com/<slug>/HEAD/<archivo>` → el **grant real**, leído
>    en bytes. ⚠️ Ver *el trap de los 14 bytes*, abajo.
> 3. 🆕 **`gh api search/code -f q='repo:<slug> filename:LICENSE'` → la LISTA
>    COMPLETA de archivos de licencia del repo.** Los passes del 2026-11-20 y
>    2026-11-21 declararon este gap como *"CONFIRMADO sin canal"* dos veces
>    seguidas, porque probaron `gh api repos/<o>/<r>/contents` (que sigue
>    bloqueado: el scope de GitHub de esta sesión cubre `search/*` y `raw`).
>    **`search/code` no estaba probado y funciona sobre repos de terceros.**
>
> **Por qué el canal 3 no es "uno más": cambia la pregunta.** Los canales 1 y 2
> sólo responden lo que se les pregunta — hay que *saber* el nombre del archivo
> para pedirlo. Toda la typología de 6 ejes del pass anterior se construyó
> **adivinando nombres de archivo**, y su propia conclusión fue que *"no puedo
> afirmar que `Matrix-Game-4/` sea el único subdirectorio con licencia propia"*.
> Con el canal 3 no hay que afirmarlo: se enumera.
>
> > **Regla de método, la central de este pass y para copiar a toda la familia de
> > KBs: una sonda sólo puede confirmar lo que ya sospechabas. Sólo la
> > enumeración te dice lo que te faltaba.** Un inventario construido con sondas
> > tiene un sesgo que no se ve desde dentro: es exactamente tan completo como la
> > imaginación de quien escribió la lista de sondas.
>
> `curl -sI https://github.com/...` sigue sin servir como prueba de existencia
> (403/200 del proxy, no del repo). Control negativo de `search/repositories`:
> `repo:gmilano/control-does-not-exist-xyz-999` → HTTP 422 *"resources do not
> exist or you do not have permission"*. ✅

## 🔴 Hallazgo nº1 del pass: el canal de enumeración existe, y lo primero que encuentra es que **Matrix-Game tiene 7 archivos de licencia, no 2**

El pass anterior publicó el grant de `SkyworkAI/Matrix-Game` como una tabla de
**dos filas** (raíz MIT / `Matrix-Game-4/` Apache-2.0) y declaró que no podía
saber si había más. Enumerado el 2026-11-21 con el canal 3 — **`total_count: 7`**:

| Ruta (enumerada, no adivinada) | Bytes | Grant leído |
|--------------------------------|-------|-------------|
| `LICENSE` | 1116 | **MIT** |
| `Matrix-Game-1/GameWorldScore/LICENSE` | 11356 | **Apache-2.0** |
| `Matrix-Game-3/LICENSE.txt` | 11357 | **Apache-2.0** |
| `Matrix-Game-4/LICENSE` | 11356 | **Apache-2.0** |
| `Matrix-Game-4/licenses/MG4CF-LICENSE` | 11356 | **Apache-2.0** (componente vendorizado) |
| `Matrix-Game-4/licenses/TAEHV-LICENSE` | 1072 | **MIT** — © Ollin Boer Bohan |
| `Matrix-Game-4/matrix_game/web/mediamtx-reader.LICENSE` | 1061 | **MIT** — © aler9 |

Tres de las siete rutas **no son adivinables**: nadie iba a sondear
`Matrix-Game-4/licenses/MG4CF-LICENSE`. La fila de dos entradas no estaba mal,
estaba **truncada**, y el pass anterior no tenía forma de saber cuánto.

## 🟢 Hallazgo nº2: el gap más viejo y más caro de esta KB — **la licencia de los pesos** — queda PARCIALMENTE CERRADO, y no por llegar a Hugging Face

Cuatro passes consecutivos declararon: *"ninguna licencia de pesos ha sido
verificada nunca por esta KB"*. **Ya no es cierto.** La vía no fue la red, fue
leer lo que el publicador declara en su propio repo.

**Primer grant de pesos verificado en la historia de esta KB:**

| Modelo | Pesos | Grant de los pesos | Evidencia (leída en bytes hoy) |
|--------|-------|--------------------|-------------------------------|
| **GameScaling** (`THUSI-Lab/GameScaling`, Apache-2.0, 604★) | 8 checkpoints, 0,8B / 2B / 9B / 27B | 🟢 **Apache-2.0** | El `README.md` dice, literal: *"The models are continued pre-trained from **Qwen3.5**, which is also **Apache 2.0**"*. Los pesos son `bfloat16` **desde los backbones Qwen3.5**, y `config.json` declara arquitectura Qwen3.5. |

> **Regla nueva, mecánica y portable a toda la familia de KBs: el grant de los
> pesos de un modelo derivado está acotado por el grant de su modelo base, y el
> modelo base casi siempre está nombrado en el README del repo de código.**
> Un modelo *continued-pretrained* o *fine-tuned* no puede ser más permisivo que
> su backbone. Eso convierte una pregunta que requería acceso a Hugging Face en
> una que se responde con `raw.githubusercontent.com` + `grep -i "base model"`.
> No sirve para un modelo entrenado desde cero; sirve para la mayoría.

Y en el caso donde **no** se puede cerrar, ahora el gap está **declarado por el
publicador**, que es mucho más fuerte que inferido por nosotros.
`Matrix-Game-4/THIRD_PARTY_NOTICES.md` (leído hoy, existencia descubierta por
enumeración) dice textualmente:

- *"The project-level Apache-2.0 license does not replace the licenses below."*
- *"**Model and asset distributions carry their own accompanying license
  statements.**"*
- *"**Base-model and codec weight licenses accompany their upstream
  distributions.**"*
- *"Game runtime assets are distributed separately by Skywork AI, with their own
  license/attribution documents."*

⇒ Antes decíamos *"la licencia de los pesos de Matrix-Game 4.0 no está
verificada"*. Ahora podemos decir algo accionable: **el publicador declara que
los pesos y los assets NO están cubiertos por el Apache-2.0 del repo, y que su
grant viaja con la distribución de Hugging Face.** Para una propuesta, esa
diferencia es la que separa "no sabemos" de "sabemos que hay que pedirlo".

### 🆕 7º eje de partición de un grant: **por dependencia vendorizada**

El pass anterior derivó 6 ejes (directorio, tipo de medio, archivo, territorio,
campo de uso, binario vs fuente). `THIRD_PARTY_NOTICES.md` de Matrix-Game-4
expone el séptimo, con 11 componentes y su grant cada uno:

| Componente vendorizado | Ubicación | Grant |
|------------------------|-----------|-------|
| MG4CF (implementación del modelo) | `matrix_game/inference/wan` | Apache-2.0, extraído de MG4CF `d5b0307`, *"based on Wan and Causal Forcing"* |
| Código Wan (modelo/códec) | mismos módulos | notices originales de Alibaba/Wan retenidos |
| **TAEHV** | `matrix_game/inference/taehv.py` | **MIT** — [madebyollin/taehv](https://github.com/madebyollin/taehv) (589★, MIT), commit `011dfc2` |
| MediaMTX reader | `matrix_game/web/mediamtx-reader.js` | MIT (© aler9) |
| **Jolt Physics** | `game/third_party/JoltPhysics` | MIT |
| **Recast/Detour** | `game/third_party/recastnavigation` | **zlib** — ⚠️ es el **mismo repo** que esta KB ya recomendaba por separado |
| DirectXMath + SAL | `game/third_party/DirectXMath` | `LICENSE` + `SAL-LICENSE.txt` |
| stb | `game/third_party/stb` | texto dual-license |
| nlohmann/json | `game/third_party/json.hpp` | MIT embebido en el header |
| 🔴 **Audio de racing** (motor, neumáticos, impactos) | bundle `racing/assets/audio` | **CC0 y CC BY 3.0** — *"retain `audio/CREDITS.md`, including tire attribution"* ⇒ **obligación de atribución en el producto** |

**Lo que esto cambia en la práctica.** "Matrix-Game 4.0 es Apache-2.0" es
verdadero *para el código del proyecto* y **falso como entrada de inventario**:
el entregable arrastra zlib, MIT ×4, un dual-license, **CC BY 3.0 con
atribución obligatoria** y pesos bajo un grant que no está en el repo. Un eje de
partición más, y el único que **no se puede descubrir sondeando nombres de
licencia**: se descubre leyendo el archivo de notices, cuya existencia se
descubre enumerando.

## 🔴 Hallazgo nº3: el **trap de los 14 bytes** — una falla en la técnica estrella de esta KB

El *checksum de bytes* es el método que esta KB más usa. Tiene un modo de fallo
que no estaba documentado y que **fabrica datos en silencio**:

```
$ curl -sS https://raw.githubusercontent.com/rivet-dev/rivet/HEAD/LICENSE.md | wc -c
14
$ curl -sS -o /dev/null -w "%{http_code}" https://raw.githubusercontent.com/rivet-dev/rivet/HEAD/LICENSE.md
404
$ curl -sS https://raw.githubusercontent.com/rivet-dev/rivet/HEAD/LICENSE.md | od -c
0000000   4   0   4   :       N   o   t       F   o   u   n   d
```

**El cuerpo de un 404 de `raw.githubusercontent.com` son exactamente 14 bytes:
la cadena `404: Not Found`.** `curl` sin `-f` sale con código 0 y escribe ese
cuerpo en stdout. Un script que mida bytes sin mirar el status HTTP anota
**"`LICENSE.md`, 14 B"** como si fuera un archivo de licencia que existe.

> **Regla mecánica, nueva: todo byte-count por debajo de ~50 es un no-hallazgo,
> no una licencia. Hay que leer el status HTTP junto con el cuerpo, siempre.**
> Las sondas de este pass usan `curl -sS -w "|%{http_code}"` y descartan todo lo
> que no sea 200. Verificado: la lista de 11 sondas × 13 repos de este pass dio
> 0 falsos positivos de 14 B.
>
> Nótese la interacción venenosa con la regla heredada *"bytes muy por debajo de
> la banda ⇒ archivo puntero, seguir el puntero"*: esa regla, aplicada a 14 B,
> manda a seguir un puntero que no existe.

### 🔴 El mismo trap en el otro canal, y lo cazamos con las manos en la masa

El trap de los 14 bytes es *"la herramienta escribe el fallo en stdout y sale
con código 0"*. **El canal 1 tiene exactamente el mismo defecto**, y este pass
lo descubrió **fallando**: el script de verificación de URLs de este mismo pass
dio un **✅ falso** para `rivet-gg/toolchain`.

```bash
# ❌ MAL — `gh` escribe el cuerpo del error de la API en STDOUT
R=$(gh api -X GET search/repositories -f q="repo:$s" --jq '.items[0].full_name' 2>/dev/null)
[ -n "$R" ] && echo "existe"     # $R = '{"message":"Validation Failed",...}' ⇒ no vacío ⇒ ✅ FALSO

# ✅ BIEN — comparar contra el valor esperado, no contra "no vacío"
R=$(gh api -X GET search/repositories -f q="repo:$s" --jq '.items[0].full_name' 2>/dev/null)
[ "$R" = "$s" ] && echo "existe"
```

Corrida de nuevo con el test correcto, la verificación de las 26 URLs del diff
dio **25 existentes + 1 archivada-y-citada-a-propósito + 0 rotas**, y la fila de
`rivet-gg/toolchain` pasó de *"el destino del redirect tampoco existe"* (lo que
este pass escribió primero, **y era falso**) a lo que realmente dice su README:
*"Archived Repository … It has been moved to `rivet-gg/rivet`"*.

> **Regla de método, y es la generalización de las dos: en este entorno, una
> herramienta que falla puede escribir su fallo en stdout y salir con código 0.
> Un chequeo de existencia nunca puede ser "¿devolvió algo?" — tiene que ser
> "¿devolvió *lo que esperaba*?".** Vale para `curl` (status HTTP, no bytes) y
> para `gh` (valor esperado, no no-vacío). **Las dos verificaciones de esta KB
> compartían el mismo agujero y ninguna de las dos lo había visto en 4 passes.**

## 🔴 Hallazgo nº4: la enumeración pasada por las recomendaciones nº1 de esta KB — **tasa medida: 4 de 12**

Aplicado el canal 3 a los 12 repos que esta KB más recomienda. Resultado medido,
con el honesto incluido: **2 de los 4 "hallazgos" eran falsas alarmas**, y hay
un discriminador barato que los separa.

| Repo | Archivos de licencia enumerados | Veredicto |
|------|--------------------------------|-----------|
| 🔴 **`limbonaut/limboai`** (3.137★) | `LICENSE.md` (1103, **MIT**) · `LOGO_LICENSE.md` (212, **CC BY 4.0**) · `demo/LICENSE_ASSETS.md` (280, **CC BY 4.0**) | **GRANT PARTIDO REAL.** Esta KB lo publica como "MIT" plano en 2 archivos. Es **MIT para el código y CC BY 4.0 para el logo y para los assets gráficos del proyecto demo**. Un estudio que forkea el demo como punto de partida **hereda una obligación de atribución**. |
| 🟡 `CoplayDev/unity-mcp` (16.087★) | `LICENSE` (1066, MIT) · `Server/LICENSE` (1066, MIT) · `TestProjects/.../com.unity.asset-store-tools/LICENSE.md` (588, 🔴 **Unity Companion License**) | **1 grant no-OSI real, confinado a `TestProjects/`.** El `Server/LICENSE` es **byte-idéntico** a la raíz ⇒ copia de packaging, no partición. La recomendación MIT **se sostiene**; el Unity Companion License está en tooling de test que no se envía. |
| 🟡 `recastnavigation/recastnavigation` (8.203★) | `License.txt` (zlib) · `Docs/doxygen-awesome-css/LICENSE` · `RecastDemo/Contrib/implot/LICENSE` · `RecastDemo/Contrib/imgui/LICENSE.txt` | **3 grants vendorizados** en docs y demo (MIT). Benignos, pero el `RecastDemo` no es zlib puro. |
| 🟡 `agones-dev/agones` (7.106★) | `LICENSE` (Apache-2.0) · `proto/grpc-gateway/LICENSE.txt` · `sdks/rust/proto/grpc-gateway/LICENSE.txt` | grpc-gateway vendorizado. Benigno. Nota: `build/extract-licenses.sh` aparece en la enumeración y **no es una licencia** — ver falso positivo abajo. |
| 🟢 `hi-godot/godot-ai` (3.627★) | `LICENSE` (1078, MIT) · `plugin/addons/godot_ai/LICENSE` (1078, MIT) | **Falsa alarma: byte-idénticos.** El addon que se instala en el juego del cliente lleva el mismo MIT. ✅ |
| 🟢 `heroiclabs/nakama` (13.763★) | `LICENSE` (Apache-2.0) · `internal/gopher-lua/LICENSE` | gopher-lua vendorizado. Benigno. |
| 🟢 `gamedev-skills/awesome-gamedev-agent-skills`, `Donchitos/Claude-Code-Game-Studios`, `a16z-infra/ai-town`, `rivet-dev/rivet`, `thedivergentai/Game-Dev-Agentic-Skills`, `lmgame-org/GamingAgent` | 1 archivo cada uno | ✅ grant único, confirmado por enumeración y no sólo por sonda. |

**Dos reglas nuevas que salen de esto:**

> **1. Discriminador de byte-identidad.** Un segundo archivo de licencia cuyos
> **bytes coinciden con el de la raíz** es una copia de packaging (un
> subpaquete que embarca su propio LICENSE), **no** una partición del grant. Si
> los bytes difieren, hay que leerlo. Es gratis: el byte-count que esta KB ya
> anota hace todo el trabajo. **En este pass separó 2 falsas alarmas de 2
> hallazgos reales.**
>
> **2. `filename:LICENSE` matchea código, no sólo licencias.** La enumeración
> devolvió `tools/ci_unity_license.py`, `tools/tests/test_ci_unity_license.py` y
> `build/extract-licenses.sh`. Son scripts *sobre* licencias. Hay que filtrar
> por extensión/ruta antes de contar, o el `total_count` infla.

**Y una nota sobre el propio texto de LimboAI que vale como regla aparte:** su
`LICENSE.md` (1103 B) **no empieza con la cadena `MIT License`** — arranca con
la línea de copyright. Una sonda que clasifique por *primera línea* lo da por
desconocido; la que clasifica por **banda de bytes** (1.050–1.150) lo acierta.
Es el argumento más limpio a favor del checksum de bytes que ha producido esta
KB, justo en el mismo pass en que le encontramos el trap de los 14 B.

## 🔴 Hallazgo nº5: el copyleft se mudó hacia arriba del stack — ahora vive en los **packs de skills**

Los passes del 2026-11-20 y 2026-11-21 abrieron la tier de orquestación y la
contaron como la buena noticia: *catálogos de agentes MIT, gratis, con 27k
forks*. Este pass midió los repos **nuevos** de esa misma tier. El reparto es
otro:

| Repo | ★ | Grant leído en bytes | |
|------|---|---------------------|---|
| `gamedev-skills/awesome-gamedev-agent-skills` | **9.929** | **Apache-2.0** (11395) + **`NOTICE`** (883) | 🟡 obliga a propagar el NOTICE |
| `cranebeaker/coop-anything` | 2.185 | **MIT** (1085) | 🟢 |
| `thedivergentai/Game-Dev-Agentic-Skills` | 983 | 🔴 **LGPL-3.0** (7652) | copyleft |
| `Station-Sciences/bot-crossing` | 906 | **MIT** (1069) | 🟢 |
| `jaylfc/taOS` | 609 | 🔴 **AGPL-3.0** (34523) | copyleft de red |
| `op7418/guizang-game-skill` | 431 | 🔴 **AGPL-3.0** (34524) | copyleft de red |

**De los 6 packs de agentes/skills de gaming nuevos con >400★, 3 son copyleft
(1 LGPL-3.0, 2 AGPL-3.0).** No es una anécdota con ese tamaño de muestra, y es
el reparto opuesto al que esta KB venía comunicando para la tier.

> **Lectura para engagement, y es incómoda:** el activo que se volvió viral en
> gaming **no es el engine, es el pack de skills** — y un pack de skills es
> *markdown y prompts*, lo más fácil de copiar accidentalmente dentro de un
> entregable de cliente. Un `.md` con instrucciones de agente bajo AGPL-3.0,
> pegado dentro del repo de un cliente, arrastra la obligación de red sobre
> **lo que lo ejecute**. El gate de licencia de la Receta 15 miraba
> dependencias de build; **hay que extenderlo a `*.md` y a los directorios de
> skills**. Ver Receta 16.

### ⚠️ Y la LGPL-3.0 rompe el checksum de bytes — asimetría que hay que anotar

`thedivergentai/Game-Dev-Agentic-Skills` trae 7.652 B de licencia. La banda de
copyleft de esta KB arranca en ~26.400 (LGPL-2.1). ¿7.652 B es copyleft? Sí, y
la razón está en el byte 300 del propio archivo:

> *"This version of the GNU Lesser General Public License **incorporates the
> terms and conditions of version 3 of the GNU General Public License**,
> supplemented by the additional permissions listed below."*

**La LGPL-3.0 no es un texto completo: es un suplemento de ~7.650 B sobre la
GPL-3.0.** Y en este repo **la GPL-3.0 no está** — `repo:… filename:COPYING` →
`total_count: 0`. Es decir: **los ~35.200 B de términos que el repo realmente
impone no están en el repositorio**. Quien lea sólo el archivo presente ve
7.652 B y la palabra *"Lesser"*, y puede concluir "permisivo".

| Familia | Bytes | Nota |
|---------|-------|------|
| **LGPL-2.1** | **~26.400** | autocontenida |
| 🆕 **LGPL-3.0** | **~7.650** | **suplemento. Incorpora GPL-3.0 (~35.200 B) por referencia, y casi nunca la embarca** |

⇒ **Nueva banda en la tabla de checksum, y la única de la tabla donde los bytes
son inversamente proporcionales a la restricción.** Precedente en esta misma
KB: el `LICENSE.txt` de O3DE son 1.475 B y es un *puntero*; el de la LGPL-3.0
es un *puntero a un texto que no está*.

## 🆕 Agentes y repos nuevos verificados este pass

Enumerados con canal 3, grant leído en bytes con status HTTP, ★ y `pushed_at`
leídos con canal 1 el **2026-11-21**.

| Nombre | Repo | Licencia | Descripción | ★ |
|--------|------|----------|-------------|---|
| 🆕 **awesome-gamedev-agent-skills** | [gamedev-skills/awesome-gamedev-agent-skills](https://github.com/gamedev-skills/awesome-gamedev-agent-skills) | **Apache-2.0** (`LICENSE`, 11395) + **`NOTICE`** (883) | **79 skills de game-dev + un router**, para Godot, Unity, Unreal, Roblox, three.js, Phaser y 6 engines más — *"Checked on the real engines"*. **1.013 forks.** 🟢 **El `NOTICE` es un activo, no sólo una obligación:** declara que todas las skills son obra original escrita desde documentación primaria y que *"no text or code was copied"* de otras colecciones, más un disclaimer de marcas de los 11 engines. En una industria con ansiedad de IP, **es una atestación de procedencia que se le puede mostrar al cliente**. Apache-2.0 §4(d) obliga a propagarlo. | **9.929** |
| 🆕 **Gamer** | [RainBowLuoCS/Gamer](https://github.com/RainBowLuoCS/Gamer) | **Apache-2.0** (`LICENSE`, 11358) | *Generalist Vision-Language-Action foundation model* para **agentes de juego de horizonte largo y tiempo real**. Es la primera fila VLA de esta KB: no un LLM con tools, un modelo de acción. 23 forks ⇒ **código de investigación**, no producto. | 1.189 |
| 🆕 **GameScaling** | [THUSI-Lab/GameScaling](https://github.com/THUSI-Lab/GameScaling) | **Apache-2.0** — 🟢 **y sus PESOS también (Qwen3.5)** | *Continued pre-training* para agentes embodied en mundos virtuales diversos. 8 checkpoints (0,8B/2B/9B/27B), evaluación en 4 benchmarks de juego. ⚠️ **no hay código de entrenamiento**; cada checkpoint es un repo de HF aparte. 🟢 **El único modelo de esta KB con grant de pesos verificado.** 🔵 Señal APAC: el paper acredita trabajo hecho *"while interning at **miHoYo**"*. | 604 |
| 🆕 **coop-anything** | [cranebeaker/coop-anything](https://github.com/cranebeaker/coop-anything) | **MIT** (`LICENSE`, 1085) | Servidor MCP + playbook para que un agente **convierta un juego single-player en co-op** (online y splitscreen) por reverse engineering, y comparta lo aprendido. 298 forks. ⚠️ **El grant cubre la herramienta, no el juego:** el EULA de cada título manda, y el reverse engineering de un juego comercial es **exactamente** lo que la licencia de SkyrimNet §4 prohíbe. Herramienta legítima, uso que hay que encuadrar. | 2.185 |
| 🆕 **bot-crossing** | [Station-Sciences/bot-crossing](https://github.com/Station-Sciences/bot-crossing) | **MIT** (`LICENSE`, 1069) | *"A video game for AI agents"*. Categoría nueva en esta KB: **el juego como entorno de evaluación**, no el agente. 286 forks. Es la pieza viva que `lmgame-org/GamingAgent` (congelado hace un año) dejó de ser. | 906 |
| 🆕 **Game-Dev-Agentic-Skills** | [thedivergentai/Game-Dev-Agentic-Skills](https://github.com/thedivergentai/Game-Dev-Agentic-Skills) | 🔴 **LGPL-3.0** (`LICENSE`, 7652 — **suplemento; la GPL-3.0 que incorpora NO está en el repo**) | Memoria de largo plazo multi-engine para agentes de game dev: **99 skills de Godot 4.7+ y 27 blueprints de género**, paridad Bevy, orquestación por personas (Analyst/Auditor/Builder). Técnicamente el pack más profundo de Godot que existe. 🔴 **Fuera de la ruta permisiva**: copyleft sobre *skills*. | 983 |
| 🆕 **guizang-game-skill** | [op7418/guizang-game-skill](https://github.com/op7418/guizang-game-skill) | 🔴 **AGPL-3.0** (`LICENSE`, 34524) | 歸藏 Game Skill: skill para Claude Code / Codex que construye **un juego de acción 3D jugable en el browser** (three.js, HTML único) con assets generados por AI, con demo jugable *《雾锁山门》*. 🔵 **Origen APAC**, README bilingüe. 🔴 AGPL-3.0. | 431 |
| 🆕 **DanKS** | [Calix-L/DanKS](https://github.com/Calix-L/DanKS) | **Apache-2.0** (`LICENSE`, 11348) + `NOTICE` (453) | Agente de **Guandan** (掼蛋) competitivo por RL a escala chica. 🔵 APAC. Relevante como patrón: **RL de juego de cartas con imperfecta información y equipos**, el caso que los agentes LLM todavía no resuelven. | 833 |
| 🆕 **awesome-ai-for-games** | [Eurekaleo/awesome-ai-for-games](https://github.com/Eurekaleo/awesome-ai-for-games) | **MIT** (`LICENSE`, 1082) | Colección curada y **buscable** de investigación sobre AI y foundation models **a lo largo del ciclo de vida del juego** (no sólo NPCs). Escrita en **TeX** ⇒ es una survey, no una lista de links. Canal de descubrimiento. | 391 |
| 🆕 **OpenLiberty** | [openfw-game/OpenLiberty](https://github.com/openfw-game/OpenLiberty) | **MIT** — ⚠️ en **`LICENCE`** (1066), ortografía británica | Juego de mundo abierto en Godot que **carga modelos y texturas de la era 3D de GTA**. Útil como base de mundo abierto con AI. 🔴 **Riesgo propio, no de licencia:** el grant cubre el *motor de carga*; los assets de GTA son de Take-Two. | 493 |

### 🔴 `LICENCE` (ortografía británica) estrena testigo — la lista de sondas del pass anterior tenía un hueco medido

El pass anterior publicó la lista de 11 sondas y dijo que *"los casos reales de
esta KB cubren **cinco** de esas variantes"*. `OpenLiberty` la convierte en
**seis**: es el primer repo de esta KB cuyo grant está **sólo** en `LICENCE`
(sin S), y `LICENSE` devuelve 404 (14 B). La lista de sondas del pass anterior
**ya incluía `LICENCE`**, así que la sonda no falló — **la cobertura medida
subió porque apareció el testigo**. Vale anotarlo exactamente así: la lista era
correcta *antes* de tener la prueba, que es lo que una lista de sondas debe
lograr.

## 🔴 Hallazgo nº6: `rivet-gg/opengb` — una **cadena de renames de 3 saltos**, y el pivot de mercado que la explica

El *Open Game Backend* figuraba en la prensa de backends de juego como la
alternativa Apache-2.0 a Nakama. Trazado hoy:

| Paso | Slug | `search/repositories` | `raw` | Qué dice el `README.md` leído |
|------|------|----------------------|-------|-------------------------------|
| 1 | `rivet-gg/opengb` | 422 | **200** | *"# Open Game Backend — **Moved to https://github.com/rivet-gg/toolchain**"* |
| 2 | `rivet-gg/toolchain` | 422 | **200** | *"# **Archived Repository** — ⚠️ This repository is archived and no longer maintained. **It has been moved to `rivet-gg/rivet`**"* |
| 3 | `rivet-gg/rivet` | 422 | **200** | sin aviso: es el **redirect de organización**, `rivet-gg` → `rivet-dev` |
| 4 | **`rivet-dev/rivet`** | ✅ **6.970★, Apache-2.0 (`LICENSE`, 11350), `pushed_at` 2026-11-21** | 200 | el slug canónico |

**Dos lecciones.** (a) La firma de rename que esta KB documentó (`repo:` → no
visible + `raw` del slug viejo → 200) **se sostiene**, y aquí resolvió una
cadena de **3 saltos**. 🔴 **Precisión obligatoria, porque la primera lectura de
este pass fue incorrecta:** el eslabón intermedio **no está muerto** — está
**archivado y apunta hacia adelante**. Los tres slugs obsoletos devuelven **200
en `raw`**, así que *todos parecen vivos*, y ninguno es visible para el API. ⇒
hay que **iterar** la firma, no aplicarla una vez, y leer el `README` de cada
eslabón: es ahí donde está el puntero siguiente. (b) Un rename de **organización** renombra *todos* sus
repos a la vez: es el modo de fallo de rename con mayor radio de daño, y el
único que invalida una página entera de punteros de golpe.

**Y el dato de mercado, que es más grande que el de método.** `rivet-dev/rivet`
ya no se describe como backend de juego. Hoy es *"the orchestrator for agentic
workloads"*, con *"4,99 ms p99 cold starts, 72 KB RAM per Actor"* y la frase
*"you can create one for each **agent**, user, session, or tenant"*. La misma
org publica `rivet-dev/agentos` (**4.912★**, Apache-2.0, sandboxes WASM) y
`rivet-dev/agents` (1.652★, Apache-2.0). **La empresa que construyó el Open
Game Backend abandonó el nicho de gaming y revendió su tecnología de
orquestación de game servers como infraestructura de agentes AI.** Ver
`intel/trends.md` nº 18.

Consecuencia operativa inmediata: **la alternativa Apache-2.0 a Nakama en
orquestación de servers es `agones-dev/agones`, no OpenGB.** OpenGB no está
"sin mantenimiento": **ya no existe como producto de gaming.**

## ⚫ Candidatos nuevos evaluados y **rechazados** este pass

| Candidato | Medido | Por qué se rechaza |
|-----------|--------|--------------------|
| [AIGeeksGroup/Code2Games](https://github.com/AIGeeksGroup/Code2Games) | **89★**, **SPDX `NONE`**, `pushed_at` 2026-10-17 | Apareció en snippets de búsqueda como repo de trending de generación de mundos de juego. **Sin archivo de licencia** (las 11 sondas → 404) ⇒ *all rights reserved*. Mismo patrón que `chongdashu/unreal-mcp`, con 24× menos tracción. |
| [AlterLab-IEU/AlterLab_GameForge](https://github.com/AlterLab-IEU/AlterLab_GameForge) | 57★, MIT, `pushed_at` 2026-11-06 | Grant limpio, proyecto real, **pero 57★ y 15 días sin commits**. Apareció como "rank #1 de un día" en un tracker de trending: **el ranking diario de un tracker no es tracción.** Señal, no ruta. |
| `jaylfc/taOS` | 609★, **AGPL-3.0** | "AI agent OS" self-hosted que menciona *gaming PC* sólo como **hardware de cluster**. **No es un repo de gaming**: entró por la query y se va por el alcance. Se anota porque su AGPL-3.0 contó en el reparto de la tier de skills, y porque es el tipo de falso positivo que una búsqueda por palabra clave produce. |
| `shinkuan/Akagi` | 1.151★, Apache-2.0, ⚠️ rama default **`v3`** | Asistente de **Mahjong online** (riichi). Grant limpio y activo, pero es **asistencia a un jugador humano en un juego con dinero**, no game dev. Fuera de alcance; se anota la rama default `v3` porque es la **4ª rama no-`main`/`master`** que esta KB encuentra y refuerza usar `HEAD`. |

## Lo más importante del pass 2026-11-21 (cycle 5): **un grant sin su alcance no es un grant**

El pass del 2026-11-20 derivó el *checksum de bytes* y cazó 4 filas mal
clasificadas. Este pass aplica la regla que llegó de la KB de **enterprise**
(pase 16, regla T60) — ***`NOASSERTION` no es un veredicto, es la ausencia de
uno, y hay que resolverlo leyendo bytes en las DOS direcciones*** — a los **16
repos de esta KB** que el API marca `NOASSERTION` o `null`.

**Resultado medido, no supuesto: el reparto es ~50/50.**

| Resolución del badge | Repos | Conclusión |
|----------------------|-------|-----------|
| 🟢 **Permisivo detrás del badge** | `pterodactyl/panel` (MIT), `love2d/love` (zlib), `AlanLaboratory/UnrealMLAgents` (Apache-2.0), `contentauth/c2pa-rs` + `c2pa-python` (**MIT *o* Apache-2.0**), `o3de/o3de` (Apache-2.0 *o* MIT), `Unity-Technologies/ml-agents` (Apache-2.0) | 7 |
| 🔴 **Restrictivo o bloqueante detrás del badge** | `supertuxkart/stk-code` (GPL-3.0-or-later), `luanti-org/luanti` (código LGPL-2.1+), `0ad/0ad` (GPL-2.0-or-later), `Tencent-Hunyuan/HY-World-2.0` (licencia Tencent **con exclusión territorial**), `Tencent-Hunyuan/Hunyuan-GameCraft-1.0` (idem), `electricsheephq/WorldOS` (EULA source-available), `AnkleBreaker-Studio/unity-mcp-server` + `unity-mcp-plugin` (licencia propia, **prohibición de reventa**) | 8 |
| 🟡 **Compuesto / carve-out** | `MonoGame/MonoGame` (Ms-PL), `PostHog/posthog` (MIT + `ee/`) | 2 |
| ⚫ **Sin grant de ningún tipo** | `chongdashu/unreal-mcp`, `SKYHUBDev/Aivill`, `mannychen0225/Agent-Hive-Workbench` | 3 |

**La conclusión operativa es más fuerte que "verificar siempre".** Si
`NOASSERTION` fuera mala señal, serviría como filtro barato. **No lo es: es una
moneda al aire**, así que **no es evidencia en ninguna dirección** y no puede
usarse ni para descartar ni para priorizar. Es sólo un marcador de *"el
clasificador no pudo decidir"*, y lo que impide decidir es casi siempre una de
seis cosas — las **seis dimensiones por las que un grant se puede partir**, y
**esta KB tiene un ejemplo verificado de cada una**:

| Eje de partición | Repo testigo de esta KB | Qué cambia en la práctica |
|------------------|------------------------|---------------------------|
| **Por directorio** | `PostHog/posthog` (`ee/`), `SkyworkAI/Matrix-Game` (`Matrix-Game-4/` es Apache-2.0, la raíz MIT) | Hay que saber **qué subárbol** se va a usar antes de poder responder "¿qué licencia tiene?" |
| **Por tipo de medio** | `luanti-org/luanti`: **código LGPL-2.1+**, **texturas y sonidos CC-BY-SA 3.0**, Lua embebido MIT — **los tres en UN archivo** | Forkear y quedarse los assets es un grant distinto del de forkear el código |
| **Por archivo** | `MinLL/SkyrimNet-GamePlugin` (🆕 abajo): mod **propietario**, pero `SkyrimNetApi.psc` es **MIT** y se puede compilar contra él en cualquier mod, abierto o cerrado | El inverso de PostHog: **repo cerrado con un archivo abierto**. Integrarse es legal; forkear no |
| **Por territorio** | `Tencent-Hunyuan/HY-World-2.0`: *"THIS LICENSE AGREEMENT DOES NOT APPLY IN THE EUROPEAN UNION, UNITED KINGDOM AND SOUTH KOREA"* | **La licencia misma tiene región.** Un engagement EMEA no puede usarlo; NA, LATAM y APAC-ex-Corea sí |
| **Por campo de uso** | `AnkleBreaker-Studio/unity-mcp-server`: prohibida la **reventa** como herramienta/producto/servicio; permitido el uso interno comercial y el **consulting alrededor** | Afecta directamente al modelo de negocio de una consultora, no al código |
| **Por binario vs fuente** | `MonoGame/MonoGame` Ms-PL §3(D): el binario se distribuye libre; **cualquier porción en fuente sólo bajo Ms-PL** | El mismo repo es permisivo o viral según **el formato del entregable** |

> **Regla nueva de la rotación (para copiar a toda la familia de KBs):**
> **la pregunta "¿qué licencia tiene este repo?" está mal formulada.** La bien
> formulada es **"¿qué licencia tiene *esta parte*, en *este formato*, en *este
> territorio*, para *este uso*?"**. Un inventario que guarda un solo campo
> `license` por repo **no puede representar** 6 de los 16 repos `NOASSERTION` de
> esta KB. Mínimo: `license` + `scope` + `notes`.

## 🔴 La corrección de este pass: el trap de mayúsculas que esta KB ya había documentado, y le volvió a pasar

El pass anterior escribió, en 4 archivos, que `Tencent-Hunyuan/HY-World-2.0`
*"no publica ningún archivo de licencia (`LICENSE`, `.txt`, `.md` → 404)"* y lo
puso como caso de *"se autodenomina open source sin publicar grant"*.

**Falso. El archivo existe y se llama `License.txt`** — mayúscula L, minúscula t:

| Sonda | Resultado |
|-------|-----------|
| `LICENSE`, `LICENSE.txt`, `LICENSE.md` | 404 |
| **`License.txt`** | **200, 16.858 B** |

El contenido es la **TENCENT HY-WORLD 2.0 COMMUNITY LICENSE AGREEMENT**, release
date **15-abr-2026**, y su primera cláusula es territorial (ver el eje "por
territorio" arriba). Es decir: el repo **sí** tiene grant, **no** es OSI, y
además **excluye tres jurisdicciones**. Las dos afirmaciones del pass anterior
("no hay archivo") y la conclusión ("sin archivo no hay permiso") eran ambas
incorrectas, por la misma causa.

**Lo caro no es el error: es que la causa estaba documentada en esta misma
página.** La sección de pathfinding dice, textualmente, que el grant de
`recastnavigation` está en **`License.txt`** y que *"una sonda que pida
`LICENSE.txt` devuelve 404 y la fila se publica como 'sin licencia'"*. El pass
documentó el trap y, dos secciones más abajo, **publicó una fila exactamente
así**, porque la **lista de sondas** no se actualizó junto con la lección.

> **Regla de método, nueva y mecánica:** una lección sobre nombres de archivo
> **no vale escrita en prosa**; vale **dentro de la lista de sondas**. La lista
> de este pass, usada en los 16 repos, es:
> `LICENSE LICENSE.txt LICENSE.md License.txt license.txt LICENCE COPYING COPYING.txt LICENSE-MIT LICENSE-APACHE NOTICE`
> — y los casos reales de esta KB cubren **cinco** de esas variantes:
> `License.txt` (HY-World, recastnavigation), `license.txt` minúscula (LÖVE),
> `COPYING` (SuperTuxKart), `LICENSE.md` (pterodactyl, stride, ml-agents),
> `LICENSE-MIT`+`LICENSE-APACHE` (c2pa). **Un probe de un solo nombre falla en 5
> de 16 repos de esta página.**

## 🆕 Agentes nuevos verificados este pass

Todos con `repo:` + `raw` el **2026-11-21**. Licencia leída en bytes, no inferida
del badge.

| Nombre | Repo | Licencia | Descripción | ★ |
|--------|------|----------|-------------|---|
| 🆕 **agency-agents** | [msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents) | **MIT** | **El catálogo de agentes por rol más grande que existe: ~230 agentes** en ~18 divisiones, una de ellas **Game Development** con subsecciones Unity, Unreal, Godot, Blender y Roblox Studio. Incluye scripts de conversión a Claude Code, Cursor, Copilot y Gemini CLI. **27.561 forks.** | **169.646** |
| 🆕 **novel-to-game** | [zenstory-ai/novel-to-game](https://github.com/zenstory-ai/novel-to-game) | **MIT** (`LICENSE`, 1081) | Skill-pack de agentes que convierte **una novela en un juego**: lectura y despiece del libro → diseño de mecánicas y arte → build de código → *blind-play* de aceptación. Targets: web, película interactiva y **mini-juegos de WeChat / Douyin / TapTap**. 🟢 **Origen y distribución APAC** (ver `intel/market.md`). | 1.175 |
| 🆕 **SkyrimNet Game Plugin** | [MinLL/SkyrimNet-GamePlugin](https://github.com/MinLL/SkyrimNet-GamePlugin) | 🔴 **propia, no OSI** (`LICENSE`, 3745) — **pero con `SkyrimNetApi.psc` bajo MIT** | NPCs AI para Skyrim que hablan, recuerdan, reaccionan y **actúan**, con voz en tiempo real. Es la implementación de NPC-LLM **con más horas de jugador reales** de esta página. Commit de hoy. **Integrable, no forkeable** (§4 prohíbe reverse engineering; `FatelessAiEngine.dll` se licencia aparte). | 386 |
| 🆕 **NPCTalk** | [christos-antono/NPCTalk](https://github.com/christos-antono/NPCTalk) | **MIT** (`LICENSE`, 1078) | Hace hablar a **cualquier NPC de cualquier juego**: skills + servidor de voz self-hosted (Fish Audio). Cubre la capa TTS que `npcpy` y LLMUnity no traen. | 232 |
| 🆕 **fennara-godot-ai** | [fennaraOfficial/fennara-godot-ai](https://github.com/fennaraOfficial/fennara-godot-ai) | **MIT** (`LICENSE.md`, 1064) | Chat AI **nativo dentro del editor de Godot** + tooling de agente con MCP. Alternativa a `godot-ai` cuando se quiere la ventana de chat embebida. Commit de hoy. | 321 |
| 🆕 **NarrativeEngine-P** | [Sagesheep/NarrativeEngine-P](https://github.com/Sagesheep/NarrativeEngine-P) | **MIT** (`LICENSE`, 1065) | Dungeon Master AI self-hosted para campañas TTRPG largas: memoria con jerarquía y NPCs persistentes. El patrón de **memoria de campaña** reutilizable fuera del TTRPG. | 131 |
| 🆕 **mc_aiplayer** | [zoyluoblue/mc_aiplayer](https://github.com/zoyluoblue/mc_aiplayer) | **MIT** (`LICENSE`, 1063) | Agente autónomo que **juega** Minecraft como mod Fabric server-side (MC 1.21.3). La contraparte *dentro del juego* de los world models de Minecraft que esta página ya listaba. | 141 |
| 🆕 **summer** | [SummerEngine/summer](https://github.com/SummerEngine/summer) | **MIT** (`LICENSE`, 1070 + `NOTICE`, 1652) | *"The game engine built for AI"*: engine + MCP server + CLI + skills para que un agente **construya y verifique** proyectos. Muy temprano (110★) pero es la primera vez que aparece un engine **diseñado** para agentes y no adaptado. ⚠️ el `NOTICE` declara material de terceros en `library/s…`: leerlo antes de redistribuir. | 110 |
| 🆕 **godot-agent** | [aigengame/godot-agent](https://github.com/aigengame/godot-agent) | **MIT** (`LICENSE`, 1066) | Automatización de Godot para que un agente **construya y verifique** vía CLI, Agent Skill o MCP. Señal temprana del mismo patrón que `summer`. | 58 |

**Lectura para engagement — el dato más grande de este pass.** El repo de
orquestación que la KB coronó ayer, `Claude-Code-Game-Studios` (40.485★), **no
es el techo de la categoría: es el 24% de ella.** `agency-agents` tiene
**169.646★ y 27.561 forks** siendo *genérico*, con gaming como **una división
entre 18**. El mercado no está premiando catálogos de agentes **de gaming**:
está premiando catálogos de agentes **horizontales con una división de gaming**.
Para AI Studios eso cambia la propuesta: el diferencial defendible no es tener
"los agentes de gaming" —hay 230 gratis, MIT, con 27k forks— sino **el harness,
la evaluación y la integración con el engine del cliente**. Honestidad
obligatoria, igual que ayer: **las ★ miden interés, no producción**, y un repo
de Markdown es baratísimo de marcar.

⚠️ **`Claude-Code-Game-Studios`: `pushed_at` = 2026-11-09, 12 días sin commits.**
No es abandono, pero el repo nº1 de la categoría **no tuvo actividad esta
semana** y la fila de ayer no lo decía.

## 📐 Regla heredada y revalidada: el *checksum de bytes* de una licencia

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
| 🆕 **LGPL-3.0** | **~7.650** — ⚠️ **suplemento, no texto completo: incorpora la GPL-3.0 (~35.200 B) por referencia y casi nunca la embarca. La única banda de esta tabla donde menos bytes = MÁS restricción.** |
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
| `AnkleBreaker-Studio/unity-mcp-server` | 549 | 🔴 **AnkleBreaker Open License v1.0** (resuelto 2026-11-21 — ver abajo) | 2026-10-23 | `main` |
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
| LimboAI | [limbonaut/limboai](https://github.com/limbonaut/limboai) | 🔴 **MIT el código + CC BY 4.0 el logo y los assets del demo** (corregido 2026-11-21 cycle 6 por enumeración) | Plugin C++ Godot: Behavior Trees + HSM. `BTState` combina ambos. **Tres archivos de licencia, no uno:** `LICENSE.md` (1103, MIT — ⚠️ no empieza con la cadena `MIT License`), `LOGO_LICENSE.md` (212, **CC BY 4.0**), `demo/LICENSE_ASSETS.md` (280, **CC BY 4.0**, cubre los PNG/SVG de `assets/`). ⇒ el **código** se puede relicenciar; **forkear el proyecto demo como punto de partida arrastra atribución CC BY 4.0**. ⚠️ rama default **`master`**. | 3.137 |
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
| Matrix-Game 4.0 | [SkyworkAI/Matrix-Game](https://github.com/SkyworkAI/Matrix-Game) | ⚠️ **grant partido en 7 archivos** + 11 componentes vendorizados + pesos aparte (enumerado 2026-11-21 cycle 6 — ver arriba) | Release 15-oct-2026. Engine ejecutable (lógica, física, input) + modelo causal de video que convierte frames low-poly en visuales. 67 FPS en RTX 5080 @864×480 con SageAttention. | 2.944 |
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
| 🔴 **HY-World 2.0** — **corregido 2026-11-21** | [Tencent-Hunyuan/HY-World-2.0](https://github.com/Tencent-Hunyuan/HY-World-2.0) | "open-source SOTA world model" en el README; el pass anterior escribió **"ningún archivo de licencia"** | **TENCENT HY-WORLD 2.0 COMMUNITY LICENSE AGREEMENT** (`License.txt` — ⚠️ **mayúscula L, minúscula t**, 16.858 B, release 15-abr-2026) | **El archivo existe; la sonda no lo pedía.** No es OSI, y su **primera cláusula es territorial**: *"THIS LICENSE AGREEMENT DOES NOT APPLY IN THE EUROPEAN UNION, UNITED KINGDOM AND SOUTH KOREA"*. ⇒ **inusable en un engagement EMEA o coreano, usable en NA / LATAM / APAC-ex-Corea.** 2.900★. **No** es el mismo patrón que `unreal-mcp`: ahí no hay archivo; acá sí lo hay y dice cosas. |
| 🔴 **unity-mcp-server / unity-mcp-plugin** (AnkleBreaker) | [AnkleBreaker-Studio/unity-mcp-server](https://github.com/AnkleBreaker-Studio/unity-mcp-server) · [unity-mcp-plugin](https://github.com/AnkleBreaker-Studio/unity-mcp-plugin) | API devuelve `NOASSERTION`; el README se presenta como herramienta abierta de 268 tools | **"AnkleBreaker Open License v1.0"** (`LICENSE`, **3068 B en los dos repos**, leída completa) | 🔴 **"Open License" no es open source.** Arranca con el íncipit literal de MIT (*"Permission is hereby granted, free of charge…"*) y los 3068 B ya la delatan contra la banda MIT de ~1.100 — es el **impostor de íncipit MIT** que nombró la KB de enterprise, acá con dos restricciones que pegan en el modelo de negocio de una consultora: **§2 atribución obligatoria en el producto** (*"Made with AnkleBreaker MCP"* + logo, en about/splash/credits, también en uso comercial) y **§3 prohibición de reventa**: no se puede vender ni sublicenciar el software **ni un derivado** como herramienta, producto o servicio, ni envolverlo en *"a thin service layer"* y cobrarlo. ✅ **Sí** permite: uso interno comercial gratis, integrarlo como **componente gratuito** de un producto con valor propio sustancial, y **"consulting, support, or training services around the Software"**. ⇒ para un engagement, la línea es **quién es el valor principal del entregable**: asesorar sobre él, permitido; facturarlo como la herramienta, no. 549★ + 258★ |
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

- 🔴 **CORRECCIÓN DE DIAGNÓSTICO (2026-11-21 cycle 6): `huggingface.co` NO es un fallo de DNS.** Cuatro passes lo
  registraron como *"no resuelve DNS"*. Medido hoy: `curl` devuelve
  **`(56) CONNECT tunnel failed, response 403`** para `huggingface.co` **y** para
  `hf.co`. Según `/root/.ccr/README.md`, un 403 en el CONNECT significa que
  *"the destination host is not allowed for this session"*: es una **denegación
  de política del proxy de egreso**, no una resolución fallida. **La diferencia
  es operativa.** Un fallo de DNS podría ser transitorio y justifica reintentar
  cada pass; una denegación de política **no va a cambiar reintentando**.
  ⇒ **dejar de sondear Hugging Face.** La vía que sí funciona es la del hallazgo
  nº2: leer el **modelo base declarado en el README** del repo de código.
  Aplicada hoy dio el primer grant de pesos verificado de esta KB
  (GameScaling → Qwen3.5, Apache-2.0).
- **Historia del gap, conservada — `huggingface.co` inalcanzable.** Consecuencia
  concreta e inmutable desde julio: **ninguna licencia de pesos ha sido verificada
  nunca por esta KB** — ni Matrix-Game 4.0, ni los checkpoints de MineWorld, ni
  `tencent/HY-World-2.0`. En un world model, los pesos son la mitad del entregable.
  Es el gap más viejo y más caro de la KB. **Vía a probar el próximo pass:** la KB
  de enterprise leyó una spec bloqueada vía `raw.githubusercontent.com`; buscar si
  el model card está espejado en un repo de GitHub.
  🆕 **Vía probada y agotada este pass:** `WebFetch` tampoco resuelve
  `gamedevreports.substack.com`, `www.aiandgames.com`, `www.pcgamer.com` ni
  `www.gamingonlinux.com`. Los únicos hosts que resuelven son `github.com` y
  `raw.githubusercontent.com`. ⇒ **toda la evidencia de prensa y encuestas de
  esta KB viene de snippets de búsqueda, no de la fuente primaria**, y se
  cross-chequea corriendo búsquedas independientes sobre la misma cifra. Dicho
  explícitamente porque cambia el peso de las secciones de `intel/`: los repos
  están leídos en bytes, **los números de mercado no**.
- 🟢 **RESUELTO (2026-11-21 cycle 6): enumerar los archivos de licencia de un repo de terceros SÍ tiene canal — `gh api search/code -f q='repo:<slug> filename:LICENSE'`.** Ver hallazgo nº1: devolvió `total_count: 7` para `SkyworkAI/Matrix-Game` y resolvió los 12 repos principales de esta KB. **Sigue sin canal** la enumeración de un directorio *arbitrario* (`search/code` indexa por nombre de archivo, no lista rutas) y la API de contenidos. 🆕 **También medido bloqueado: el MCP de GitHub de esta sesión para repos de terceros** — `mcp__github__get_file_contents` sobre `SkyworkAI/Matrix-Game` devuelve *"repository is not configured for this session. Allowed repositories: gmilano/globant-kb"*: es un **scope de sesión**, no una falla del servidor. ⚠️ Y `filename:LICENSE` **matchea código también** (`tools/ci_unity_license.py`, `build/extract-licenses.sh`): hay que filtrar por ruta/extensión antes de contar.
- **Historia del gap, conservada — listado de directorios: CONFIRMADO sin canal, 2º pass (cycle 5).**
  `gh api repos/<owner>/<repo>/contents` devuelve
  *"GitHub access to this repository is not enabled for this session"* —
  el scope de GitHub de esta sesión cubre `search/*` y `raw`, no la API de
  contenidos. Probado hoy contra `SkyworkAI/Matrix-Game`. Por eso el grant partido
  de Matrix-Game se verifica por sonda de rutas conocidas y no por enumeración, y
  por eso no puedo afirmar que `Matrix-Game-4/` sea el único subdirectorio con
  licencia propia.
- **★ de este pass: 100% releídas.** No queda ninguna fila con `*` heredado. El
  canal `gh api search/repositories` lo hizo posible; el pass de octubre lo creía
  bloqueado y tuvo que dejar 20 filas sin verificar.

---
*Pass 2026-11-21 (cycle 6), ~1 h después del cycle 5, por lo que **no mide
crecimiento**: 1 **canal de verificación nuevo** (`gh api search/code` —
enumeración de archivos de licencia en repos de terceros, gap declarado
"sin canal" dos passes seguidos); Matrix-Game enumerado en **7** archivos de
licencia (no 2) + `THIRD_PARTY_NOTICES.md` con 11 componentes vendorizados ⇒
**7º eje de partición de grant**; **1er grant de pesos verificado en la historia
de esta KB** (GameScaling ← Qwen3.5, Apache-2.0) por la regla nueva *"el grant
de los pesos está acotado por el del modelo base, y el modelo base está en el
README"*; 1 corrección de diagnóstico (Hugging Face es **denegación de política
del proxy**, no fallo de DNS ⇒ dejar de reintentar); **trap de los 14 bytes**
(el cuerpo de un 404 de `raw` son 14 B y un byte-count sin status HTTP lo anota
como licencia); enumeración pasada por 12 repos recomendados, tasa medida
**4/12**, con **discriminador de byte-identidad** que separó 2 falsas alarmas de
2 hallazgos reales; **LimboAI corregido: MIT el código, CC BY 4.0 el logo y los
assets del demo**; **el copyleft se mudó a los packs de skills** (3 de 6 packs
nuevos >400★ son LGPL-3.0/AGPL-3.0) + **banda nueva LGPL-3.0 ~7.650 B** con la
asimetría explicada; cadena de renames de **3 saltos** en `rivet-gg/opengb` y el
pivot de Rivet de backend de juego a orquestador de agentes; 10 repos nuevos
verificados, 4 rechazados con motivo; `LICENCE` estrena testigo (6 de 11
variantes de sonda con caso real). 2026-11-21.*

---
*Pass 2026-11-21 (cycle 5): 16 badges `NOASSERTION`/`null` resueltos por lectura
de bytes con reparto medido 7 permisivos / 8 restrictivos / 3 sin grant;
1 corrección 🔴 (HY-World 2.0 **sí** tiene `License.txt`, con exclusión
territorial EU/UK/Corea); 1 licencia propia destapada en una tabla de líderes
(AnkleBreaker Open License v1.0: atribución en producto + prohibición de
reventa); 2 refinamientos a permisivo (`c2pa-rs` y `c2pa-python` son **MIT o
Apache-2.0**, no sólo Apache-2.0); 9 agentes nuevos verificados; typología de
**6 ejes de partición de grant** con un repo testigo de esta KB por eje;
83 filas heredadas revalidadas en `repo:`. 2026-11-21.*

---
*Pass 2026-11-20: 4 correcciones de licencia (2 nuevas, 2 revalidadas por 2º canal),
1 rechazo revertido, 2 renames documentados, 1 repo desaparecido, 6 filas nuevas,
7 repos con rama default ≠ `main`/`master` señalados, 1 regla de checksum derivada
y validada 4/4 sobre datos preexistentes, 38 repos verificados en dos canales con
control negativo en ambos sentidos. 2026-11-20.*
