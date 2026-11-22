---
industry: gaming
region: Global
updated: 2026-11-22
---

# Verticales de partida — Gaming AI

> Plataformas open source reales, customizables con una capa AI encima.
> Modelo: partir de algo funcional y robusto, añadir agentes encima.
> Última actualización: **2026-11-22 (cycle 12)** (passes previos: 2026-11-22 cycles 11/10, 2026-11-21 cycle 9, 2026-11-20, 2026-10-26, 2026-07-02).
>
> **Todas las licencias de esta página se releyeron el 2026-11-20 en dos canales**
> (`gh api search/repositories` para SPDX + `raw.../HEAD/` para el texto). El pass
> de octubre corrigió dos filas y **publicó mal otras cuatro**; esas cuatro se
> corrigen ahora y están marcadas 🔴. El detector que las encontró es el **checksum
> de bytes** del archivo de licencia, documentado en `agents/top.md`.

## 🆕 Capa vertical nueva (2026-11-22, cycle 13): **la plataforma de diálogo, voz y moderación** — la que convierte un LLM en algo que se puede enviar a certificación

Las capas que esta página ya cubría (engine, backend, orquestación de servidores,
pipeline de assets) terminan donde empieza el jugador. **Falta la plataforma de
lo que el jugador lee, escucha, escribe y entiende en su idioma.** No es un
repo: son cuatro subsistemas con una restricción de licencia distinta cada uno.

### El stack, con la licencia como criterio de selección (no como nota al pie)

| Subsistema | Elección recomendada | Licencia (byte-read) | La alternativa grande que se descarta, y por qué |
|------------|----------------------|----------------------|--------------------------------------------------|
| **Autoría de diálogo** | [inkle/ink](https://github.com/inkle/ink) (5.029 ★) o [YarnSpinner](https://github.com/YarnSpinnerTool/YarnSpinner) (2.876 ★) | **MIT** (1.067 / 1.138 B) | ninguna: las dos son MIT en el núcleo. ⚠️ pero **los ports de Yarn Spinner para Godot-GDScript y Unreal son YSPL 1.0**, no MIT. |
| **STT (voz del jugador)** | [ggml-org/whisper.cpp](https://github.com/ggml-org/whisper.cpp) (55.409 ★) en cliente; [faster-whisper](https://github.com/SYSTRAN/faster-whisper) (26.929 ★) en servidor | **MIT** | — |
| **TTS (voz del NPC)** | [SWivid/F5-TTS](https://github.com/SWivid/F5-TTS) (15.633 ★) | **MIT** (1.068 B) | `fish-speech` (33.825 ★) por umbral de facturación **del cliente**; `piper` MIT por **archivado**; `piper1-gpl` por **GPL-3.0**; `coqui-ai/TTS` por **27 meses sin commits**. |
| **Moderación** | [unitaryai/detoxify](https://github.com/unitaryai/detoxify) + [NVIDIA-NeMo/Guardrails](https://github.com/NVIDIA-NeMo/Guardrails) + `PurpleLlama/LlamaFirewall/` | **Apache-2.0 / MIT** | los **pesos** `Llama-Guard*` (Llama 2/3.2/4 Community): no open source. Se usa el framework, no el modelo de Meta. |
| **Localización** | [i18next](https://github.com/i18next/i18next) (runtime, **MIT**) + [Weblate](https://github.com/WeblateOrg/weblate) (plataforma, **GPL-3.0 desplegada aparte**) | mixta, con el límite del binario como criterio | `LunaTranslator` (14.258 ★, GPL-3.0): ✅ como tool del equipo, ⛔ embebido. |

### 🔴 Por qué esta capa es *vertical* y no *horizontal*: la regulación la toca directo

Las otras capas de esta página (engine, servidores, assets) son indiferentes a la
jurisdicción. **Ésta no.** Lo que el jugador lee, dice y escucha es exactamente
el objeto de los tres instrumentos que vencen o rigen ahora mismo:

- **EU AI Act Art. 50** — transparencia sobre contenido generado por AI, con
  **2-dic-2026** para sistemas ya en mercado: **faltan 10 días**. Afecta al
  diálogo generado y a la voz sintética, no al navmesh.
- **Texas SB 2420 (App Store Accountability Act)** — vigente desde el 1-ene-2026
  y **exigible desde el 4-jun-2026**: la tienda le pasa al desarrollador la
  **categoría de edad del usuario**, y el desarrollador debe **borrarla** después
  de verificar. Afecta a qué chat y qué NPC se le habilita a quién.
- **Roblox** exige desde ene-2026 **verificación de edad para acceder al chat, a
  escala global**. Es el precedente de producto, no sólo el legal.

⇒ **Una plataforma de diálogo/voz/moderación sin un parámetro de edad en runtime
no es incompleta: es no-entregable** en NA y, a partir del 2-dic, en EMEA. Ver
`compose/patterns.md`, Receta 21.

### 🟢 La pieza de plataforma que NO existe, medida

No hay ninguna plataforma vertical open source de **moderación de chat de juego**
—el equivalente a lo que Nakama es para backend o Agones para servidores—.
Búsquedas por `toxicity detection chat moderation` y `content moderation LLM
guardrails`, ordenadas por ★: el máximo es **8 ★**. Lo que hay son **componentes
genéricos de LLM** (detoxify, Guardrails, NeMo, LlamaFirewall) que hay que
ensamblar y a los que hay que agregarles lo específico de juego: jerga,
ofuscación (`l33t`), contexto de partida, voz además de texto, y multi-idioma con
el perfil de LATAM y APAC.

> **Para posicionamiento:** es la única capa de este KB donde el entregable de
> Globant **no compite con un producto open source consolidado**, porque no
> existe. Las otras capas son integración; ésta es construcción sobre primitivas.

---

## 🆕 Movimiento de plataforma (2026-11-22, cycle 12): **Talo no era un repo, era una organización de cinco — y el que faltaba es el que no tiene licencia**

Esta página recomienda Talo desde varios passes con la frase *"MIT completo, sin
tier cerrado"*, listando **un** repo. Enumerada la organización:

| Repo | ★ | Licencia (bytes) | Archivos de licencia (enumerados) | Push | Rol |
|---|---|---|---|---|---|
| [TaloDev/godot](https://github.com/TaloDev/godot) | **251** | 🟢 **MIT** (1.074) | **4** (todos MIT) | 2026-11-16 | 🆕 **Cliente Godot. 2,4× las ★ del backend: es la puerta de entrada real del producto** |
| [TaloDev/backend](https://github.com/TaloDev/backend) | 105 | 🟢 **MIT** (1.074) | 1 | 2026-11-21 | Servidor self-hosteable (el único que esta página listaba) |
| [TaloDev/unity](https://github.com/TaloDev/unity) | 43 | 🟢 **MIT** (1.074) | 2 | 2026-11-16 | 🆕 Cliente Unity |
| [TaloDev/frontend](https://github.com/TaloDev/frontend) | 21 | 🟢 **MIT** (1.074) | 1 | 2026-11-17 | 🆕 Consola de gestión |
| ⛔ [TaloDev/hosting](https://github.com/TaloDev/hosting) | 36 | 🔴 **SIN GRANT** | **0** | 2026-10-14 | 🆕 *"Self-hosting starter pack"* — **el camino de despliegue no está otorgado** |

> 🔴 **La corrección, dicha sin rodeos:** *"MIT completo, sin tier cerrado"* es
> cierto **del producto** y **falso de la organización**. El repo sin licencia
> es precisamente el *starter pack de self-hosting* — es decir, **la razón por
> la que un cliente elige Talo en lugar de un backend gestionado**, y lo primero
> que un equipo clonaría.
>
> ✅ **Mitigación, y es simple:** desplegar `TaloDev/backend` (MIT, cubre el
> servidor) con IaC propia. **No copiar `hosting/`** al repo del cliente.
> Ver `compose/patterns.md`, Receta 31.

**Comparación de la capa, re-medida este pass:**

| Plataforma | Repo | Licencia | ★ | Push | Cuándo elegirla |
|---|---|---|---|---|---|
| **Nakama** | [heroiclabs/nakama](https://github.com/heroiclabs/nakama) | 🟢 **Apache-2.0** | **13.763** | 2026-11-18 | Matchmaking, realtime authoritative, chat social a escala. 1.541 forks |
| **Talo** | [TaloDev/backend](https://github.com/TaloDev/backend) + clientes | 🟢 **MIT** | 105 (+251 cliente Godot) | 2026-11-21 | Metadata del jugador: auth, saves, leaderboards, stats, analytics. **Licencia más permisiva, superficie menor** |
| ⛔ **Anthill** | [anthill-platform/anthill](https://github.com/anthill-platform/anthill) | MIT | **4** | **2018-10-05** | 🔴 **Descartada.** Ver abajo |

---

### ⛔ Plataforma evaluada y **descartada** este pass: Anthill — *"actively maintained"* según un blog, **8 años sin un commit** según la medición

Una guía de backends de juego de 2026 presenta `anthill-platform/anthill` como
una alternativa MIT *"actively maintained"* a Nakama. Medido:

| Señal | Valor |
|---|---|
| Licencia | 🟢 MIT (real, 1 archivo enumerado) |
| ★ | **4** |
| Forks | 2 |
| Issues abiertos | 0 |
| `created_at` | 2016-08-12 |
| **`pushed_at`** | 🔴 **2018-10-05** |

> 🔴 **Octavo caso de *recomendación-por-blog vs medición* en esta KB, y el
> patrón vuelve a ser el mismo: la licencia estaba bien y la VITALIDAD era
> falsa.** Un texto que dice *"actively maintained"* es una afirmación sobre el
> presente escrita en un pasado indeterminado. `pushed_at` no es una opinión.
>
> **La sonda que lo caza cuesta una llamada:**
> `gh api -X GET search/repositories -f q='repo:<slug>' --jq '.items[].pushed_at'`

---

### 🆕 Framework de UI de juego: la plataforma cuya licencia depende de **qué estás construyendo**

Capa que esta página no tenía: **UI de juego manejable por agentes**.

| Plataforma | Repo | Licencia | ★ | Veredicto |
|---|---|---|---|---|
| **LGUI** | [LumioGames/LGUI](https://github.com/LumioGames/LGUI) | 🟡 **PolyForm Shield 1.0.0 + Additional Permission** (8.363 B; **8 archivos de licencia**) | 148 | 🟡 **Condicional: verde en un juego de cliente, ROJO dentro de un acelerador reutilizable de Globant** |
| **FairyGUI (el padre, vendorizado adentro)** | [fairygui/FairyGUI-unity](https://github.com/fairygui/FairyGUI-unity) | 🟢 **MIT** | **2.978** | 🟢 **Sin condiciones.** ⚠️ push 2026-02-03 (~9 meses) |
| — su editor | [fairygui/FairyGUI-Editor](https://github.com/fairygui/FairyGUI-Editor) | 🔴 **`NONE`** | 802 | 🔴 **La herramienta que hace usable al framework no tiene grant.** Push 2024-03-11 |

**La cláusula, citada:** *"…as long as the product itself is not a UI framework,
UI toolkit, UI editor **or game development tool**."*

> 🔴 **Eje nuevo para esta página: la licencia segmenta por TIPO DE ENTREGABLE.**
> Esta KB ya tenía licencias que segmentan por uso, por región, por rama, por
> subdirectorio y por variante de modelo. Ésta segmenta por **qué tipo de cosa
> construís** — y es el eje que peor le sienta a una consultora, porque el
> entregable de cuenta (permitido) y el acelerador reutilizable (prohibido) son
> **el mismo código en dos momentos del ciclo de vida**. Ver `compose/patterns.md`,
> Receta 32.

---

### 🆕 La capa de modelo: lo que corre **dentro** del juego, no detrás

| Pieza | Repo / producto | Licencia | Nota |
|---|---|---|---|
| **Qwen3** (SLM on-device) | [QwenLM/Qwen3](https://github.com/QwenLM/Qwen3) | 🟡 badge `NONE`, **enumeración = 0** → 🟢 Apache-2.0 **declarado en el README**, archivos en Hugging Face | **27.758 ★.** NVIDIA publicó soporte de **Qwen3-8B en ACE** para despliegue on-device. ⛔ `huggingface.co` **no resuelve en esta sesión**: el texto de los pesos **no fue verificado** |
| **NVIDIA ACE** | SDK propietario + componentes abiertos | mixta — grants partidos por componente | 🟡 **Títulos embarcados: sólo *Mecha BREAK* corroborado** este pass (Nemotron-4 4B on-device). PUBG e inZOI se degradan a *"Krafton usa AI"*; NARAKA y MIR5 **sin corroborar** |

> 🟢 **Por qué entra en una página de *verticales*:** un SLM on-device no es una
> optimización de costo. Es la **única configuración** del catálogo en la que la
> conversación del jugador **no sale de su equipo** — y por lo tanto la
> obligación estadounidense de topología de datos que esta KB documenta **no
> tiene hecho que regular**. ⚠️ El Art. 50(2) europeo **sí sigue aplicando**: es
> transparencia, no datos.

---

## 🆕 Plataforma vertical nueva (2026-11-22, cycle 11): **GDevelop** — 29.166 ★, badge opaco, grant MIT en las cuatro carpetas

Esta página existe para listar *sistemas reales y funcionales sobre los que se
pone una capa AI encima* — el papel que Odoo juega en ERP o Moodle en educación.
En gaming, el equivalente más cercano que faltaba es el **engine no-code**:
la plataforma donde el cliente ya tiene contenido hecho por gente que no programa.

| Plataforma | Licencia **verificada** | ★ | Push | Por qué entra |
|---|---|---|---|---|
| 🆕 [4ian/GDevelop](https://github.com/4ian/GDevelop) | 🟢 **MIT en todo el código** — ver desglose abajo | **29.166** | 2026-11-21 | Engine open source **2D / 3D / multijugador**, *no-code*, multiplataforma. Es la plataforma de esta página con la base de usuarios **menos técnica**, y por eso la de mayor apalancamiento para una capa de agentes: el usuario ya describe lógica en lenguaje casi natural (eventos), que es exactamente la superficie donde un agente aporta |

### 🔴 El badge decía `NOASSERTION`. La lectura byte a byte dice MIT cuatro veces.

El API de GitHub devuelve `license.spdx_id = NOASSERTION` para GDevelop, que en
la regla del cycle 7 es **la ausencia de un veredicto, no un veredicto negativo**,
y obliga a resolver por lectura directa. La resolución:

| Archivo leído | Bytes | Contenido |
|---|---|---|
| `LICENSE.md` (raíz) | **305** | 🔵 **Es un PUNTERO, no un grant.** Dice: *"The Core library, the game engine, the new IDE, and all extensions (respectively Core, GDJS, new IDE and Extensions folders) are under the **MIT license**"* + *"The name, GDevelop, and its logo are the exclusive property of Florian Rival"* |
| `Core/LICENSE.md` | 1.274 | **The MIT License (MIT)** — GDevelop Core (GDCore) |
| `GDJS/LICENSE.md` | 1.296 | **The MIT License (MIT)** — GDevelop JS Platform |
| `newIDE/LICENSE.md` | 1.107 | **The MIT License (MIT)** — GDevelop IDE |
| `Extensions/LICENSE.md` | 1.295 | **The MIT License (MIT)** — GDevelop extensions |

⇒ **Tercer caso medido de `NOASSERTION` que resuelve a PERMISIVO** (tras los 4 del
cycle 7 y los de o3de/love). La causa no es ambigüedad legal: es que el detector
automático de GitHub **no sabe leer un archivo raíz de 305 bytes que delega en
cuatro subcarpetas**.

⚠️ **El límite real, y no es la licencia del código:** *"The name, GDevelop, and
its logo are the exclusive property of Florian Rival"*. El MIT cubre **el código**;
**la marca no**. Un entregable que forkee GDevelop para un cliente **puede
redistribuir el software y no puede llamarlo GDevelop** ni usar su logo. Es el
mismo patrón de reserva de marca que la mayoría de los proyectos grandes, y es la
única cláusula de esta ficha que hay que poner por escrito en una propuesta.

### 🆕 La capa de servidor de juego que esta página tampoco tenía

`repos/foundations.md` (cycle 11) incorpora **cinco frameworks de game server**
permisivos que la KB no listaba —
[skynet](https://github.com/cloudwu/skynet) (MIT, 14.199 ★),
[pitaya](https://github.com/topfreegames/pitaya) (MIT, 2.847 ★, SDKs Unity/iOS/Android),
[nano](https://github.com/lonng/nano) (MIT, 3.228 ★),
[zfoo](https://github.com/zfoo-project/zfoo) (Apache-2.0, 2.005 ★, JVM) y
[pinus](https://github.com/node-pinus/pinus) (MIT, 1.933 ★, Node/TS).

**Dónde encajan en esta página, que es lo que decide si sirven:** no compiten con
Nakama ni con Talo. Nakama es un **backend terminado** (auth, leaderboards, chat,
storage) que se extiende; estos cinco son **frameworks**, es decir, el cliente
**escribe la lógica** y gana control del protocolo y del sharding a cambio de
trabajo. Regla práctica para elegir en una propuesta:

| Si el cliente... | Va a | Por qué |
|---|---|---|
| Quiere features de backend ya hechas y una capa AI encima | **Nakama** / **Talo** | Lo que se customiza es la extensión (Lua/TS/Go), no el servidor |
| Ya tiene un protocolo propio y un servidor que no escala | **skynet** / **pitaya** / **zfoo** | Framework, no producto: se adapta al protocolo existente |
| Necesita escalar *instancias de partida* en Kubernetes | **Agones** | Es ortogonal a los cinco: orquesta procesos, no define lógica |
| Es un juego web / asíncrono | **Colyseus** / **pinus** / **Supabase** | Node/TS de punta a punta |

⛔ **Y el descarte explícito:** [NetEase/pomelo](https://github.com/NetEase/pomelo),
**11.855 ★, MIT, ARCHIVADO desde 2023-08-10**. Es el primer resultado por
estrellas de toda la categoría. **No se propone.** Su continuación viva es
`node-pinus/pinus`.

---

## 🆕 Movimientos de plataforma (2026-11-22, cycle 10): **Agones pasó a gobernanza CNCF, y el clúster de herramientas de Godot se fragmentó en siete**

### 🟢 `agones-dev/agones` → **gobernanza CNCF desde marzo de 2026**

| Campo | Valor (medido este pass) |
|---|---|
| Repo | [agones-dev/agones](https://github.com/agones-dev/agones) |
| Licencia | **Apache-2.0** |
| ★ / último push | **7.107** / 2026-11-20 |
| Gobernanza | 🆕 **CNCF, desde marzo de 2026** (creado por Google y Ubisoft) |
| Qué es | Orquestación de **servidores de juego dedicados sobre Kubernetes**. Infraestructura, **no** un backend: matchmaking, identidad, economía y datos van por fuera |

**Por qué el cambio de gobernanza importa comercialmente y no es trivia.** En
una conversación de arquitectura con un cliente grande, *"proyecto de una
empresa"* y *"proyecto de la CNCF"* se evalúan con criterios distintos: el
segundo trae un modelo de gobernanza público, política de releases y una
expectativa de continuidad que no depende de la hoja de ruta de un proveedor.
**Es el mismo argumento que sostiene a Kubernetes en esa sala**, y ahora aplica
a la capa de servidores de juego.

### 🟢 Backends open source — estado re-verificado este pass

| Plataforma | Repo | Licencia | ★ | Último push | Capa |
|---|---|---|---|---|---|
| **Nakama** | [heroiclabs/nakama](https://github.com/heroiclabs/nakama) | **Apache-2.0** | **13.763** | 2026-11-18 | Backend completo: multiplayer, matchmaking, leaderboards, chat, social |
| **Agones** | [agones-dev/agones](https://github.com/agones-dev/agones) | **Apache-2.0** | 7.107 | 2026-11-20 | Orquestación de servidores sobre K8s |
| **Open Match** | [googleforgames/open-match](https://github.com/googleforgames/open-match) | **Apache-2.0** | 3.427 | ⚠️ 2026-07-12 | Matchmaking; se usa **junto con** Agones. **4 meses sin push** — el único de los cuatro que no está activo |
| **Talo** | [TaloDev/backend](https://github.com/TaloDev/backend) | **MIT** | 105 | 2026-11-21 | Backend indie: players, cloud saves, analytics, leaderboards |

⚠️ **Dato con fuentes en conflicto, se declara sin resolver:** las releases de
Nakama. Una comparativa fecha **v3.41.0 el 18-sep-2026**; la página de
desarrollador de Nakama lista **v3.41.0 el 5-oct-2026** y **v3.42.0 el
3-nov-2026**. **Ninguna pudo verificarse contra el repo en esta sesión** (el
canal `releases` de la API de GitHub no está habilitado acá). Si la versión
importa para un diseño, hay que mirarla en el repo, no en esta tabla.

⚠️ **Y lo que las páginas de vendor no dicen:** Heroic Labs posiciona **Hiro**
(metagame) y **Satori** (LiveOps: eventos, audiencias, feature flags,
experimentos) encima de Nakama. **Ninguna de las fuentes consultadas afirma que
Hiro o Satori sean open source.** El Apache-2.0 verificado cubre el **servidor
Nakama y sus librerías cliente**. Para un cliente que compra "LiveOps open
source", la frontera cae exactamente ahí, y conviene decirlo antes de la firma.

### 🟡 Herramientas de engine como vertical: **el clúster de Godot se fragmentó en siete, y el líder lleva 7 meses sin un commit**

Para una práctica que elige stack, esto es una decisión de plataforma, no de
librería:

| Repo | ★ | Licencia (byte-read) | Último push | Veredicto |
|---|---|---|---|---|
| [Coding-Solo/godot-mcp](https://github.com/Coding-Solo/godot-mcp) | **6.732** | MIT | 2026-04-16 | ⛔ **muerto** (7 meses) |
| [youichi-uda/godot-mcp-pro](https://github.com/youichi-uda/godot-mcp-pro) | 649 | ⛔ MIT **sólo del plugin**; servidor **de pago** | 2026-09-24 | ⛔ open-core |
| [tugcantopaloglu/godot-mcp](https://github.com/tugcantopaloglu/godot-mcp) | 521 | ✅ MIT | 2026-10-09 | 🟢 viable (157 tools) |
| [tomyud1/godot-mcp](https://github.com/tomyud1/godot-mcp) | 454 | ✅ MIT | 2026-10-17 | 🟢 viable |
| [IvanMurzak/Godot-MCP](https://github.com/IvanMurzak/Godot-MCP) | 376 | ✅ **Apache-2.0** | **2026-11-21** | 🟢 **el más activo** + grant de patentes |
| [HaD0Yun/Doyunha-Gopeak](https://github.com/HaD0Yun/Doyunha-Gopeak) | 274 | ✅ MIT | 2026-11-13 | 🟢 viable (95+ tools) |
| [satelliteoflove/godot-mcp](https://github.com/satelliteoflove/godot-mcp) | 188 | ✅ MIT | 2026-11-16 | 🟢 **playtesting determinista** (Receta 24) |

**Seis de los siete nacieron entre dic-2025 y jun-2026.** Es un mercado abierto
hace once meses, sin consolidar, donde **el único que superó las 700 ★ es el que
dejó de publicar**. Para una práctica eso significa: **no casarse todavía**, y
elegir por `pushed_at` + licencia, no por estrellas.

> Contraste útil: en **Unity** el clúster sí consolidó —
> [CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp) (**MIT, 16.104 ★,
> push 2026-11-17**) está un orden de magnitud por encima del segundo. **Mismo
> problema, dos engines, dos estructuras de mercado opuestas**, y eso cambia qué
> se le recomienda a un cliente según con qué engine llegue.

---
## 🆕 Plataforma vertical nueva (2026-11-21, cycle 9): **Roblox** — la plataforma de desarrollo que esta página no tenía

Esta KB mencionaba Roblox de pasada (seguridad infantil, coalición con la Attorney
General Alliance en NA) pero **nunca como plataforma sobre la cual construir**. Es una
omisión grande: Roblox es, por volumen de creadores, la plataforma de desarrollo de
juegos más usada del mundo, y su lenguaje (Luau) y su editor (Roblox Studio) son un
vertical en sí mismos.

| Pieza | Licencia (byte-read) | Repo | Qué aporta |
|---|---|---|---|
| **roblox-brain** | **MIT** (`LICENSE`, 1.069 B) | [TabooHarmony/roblox-brain](https://github.com/TabooHarmony/roblox-brain) | Librería de skills con el conocimiento de **Roblox Studio** que a un agente de código le falta: APIs, convenciones y patrones del editor. 199 ★, 20 forks, push 2026-10-15. |

**Cómo encaja en un engagement:** Roblox **no** es open source y su plataforma no se
self-hostea — es el caso opuesto a Godot. Lo que es open source y permisivo es **la
capa de conocimiento que vuelve productivo a un agente adentro de Roblox Studio**. El
patrón de entrega cambia en consecuencia:

- ✅ **Sirve para:** estudios que ya publican en Roblox y quieren acelerar con agentes;
  marcas que buscan presencia en la plataforma; prototipado social rápido.
- ⛔ **No sirve para:** cualquier cliente que necesite portabilidad del runtime, control
  del backend o publicar fuera de Roblox. Ahí la respuesta sigue siendo **Godot + Nakama**.
- ⚠️ **Dependencia de plataforma:** todo lo construido vive bajo los términos de Roblox,
  no bajo MIT. **Lo MIT es el pack de skills, no el juego.** Es exactamente la ruta de
  contaminación inversa a la del cycle 6 (donde el copyleft entraba por el pack de
  skills): acá el pack es permisivo y **la plataforma es la que impone condiciones**.

---

## ⛔ Plataforma evaluada y **descartada** este pass: SpacetimeDB

| Plataforma | Licencia real | Repo | Por qué se descarta |
|---|---|---|---|
| **SpacetimeDB** (Clockwork Labs) | **BSL 1.1** (`LICENSE.txt`, 42.562 B) — **no open source**; Change Date **2031-11-20** → AGPL-3.0 c/ excepción de linking | [clockworklabs/SpacetimeDB](https://github.com/clockworklabs/SpacetimeDB) | 25.570 ★ y push de hoy, así que está **vivo y es serio**: base relacional con la lógica del servidor adentro (módulos Rust/C# a WASM), el modelo de BitCraft. Pero el Additional Use Grant permite **una sola instancia en producción** y prohíbe ofrecer un *Database Service*. Un multijugador comercial sale del grant **al escalar**, es decir, cuando el proyecto va bien. |

⚠️ **Advertencia de venta:** si el cliente llega con un comparativo de blog tipo
*"SpacetimeDB vs PlayFab vs Nakama 2026"*, llega con una tabla que **no tiene columna
de licencia**. Mostrarle el `Additional Use Grant` leído —no la insignia de GitHub,
que dice `NOASSERTION`— es la forma más rápida de reencuadrar la conversación hacia
Nakama (Apache-2.0) o Talo (MIT).

---

## 🆕 Pipeline de assets con control de gasto — la capa que faltaba en el stack recomendado

El stack de esta página (Godot + Nakama + MCP) cubre engine, backend y tooling, pero
**no decía nada sobre cuánto cuesta generar los assets**, que en un proyecto con IA
generativa es una línea de costo real y variable.

| Pieza | Licencia (byte-read) | Repo | Qué aporta |
|---|---|---|---|
| **Scenario Skills** | **MIT** (`LICENSE`, 1.070 B) | [scenario-labs/skills](https://github.com/scenario-labs/skills) | Skills que generan imagen, video, audio y **3D** desde cualquier agente, **eligen el modelo adecuado por tarea**, **cotizan el costo antes de gastar** y mantienen consistencia de personaje entre generaciones. 3.961 ★, 350 forks, push 2026-11-21. |

Las dos propiedades que lo hacen entregable a un cliente: **cotizar antes de ejecutar**
(convierte un costo variable opaco en una estimación previa) y **consistencia de
personaje** (el defecto que más retrabajo genera en arte generado). Se enchufa encima
del stack existente sin reemplazar nada.

## 🆕 🔴 Frontera declarada (2026-11-21, cycle 8): esta página es de **VIDEOJUEGOS**; *"gaming"* en inglés también significa **casino**

**Hallazgo de ingesta que afecta a esta página más que a ninguna otra**, porque
es la que lista plataformas por vertical. Las búsquedas de adopción y regulación
de este pass devolvieron, **primero y en las cuatro regiones**, el informe
**"The State of AI in Gaming 2026"** (UNLV International Gaming Institute +
KPMG): *AI Maturity Index* **≤ 45/100**, gobernanza rezagada, y **divergencia
medida regulador-industria** sobre la gravedad del daño al jugador.

🔴 **Es del rubro apuestas/casino.** En inglés regulatorio y de mercado,
*"gaming"* = **juego de azar**. Las plataformas de este archivo —Luanti, Godot,
Nakama, Colyseus, Agones, los paneles de game server— son de **videojuegos**, y
**ninguna cifra de iGaming se mezcla con ellas**.

**Por qué se escribe como frontera y no se borra el hallazgo:** iGaming es un
vertical **real, grande y adyacente**, con fuerza en **NA** (UNLV, reguladores
estatales) y en **LATAM** (Brasil abrió mercado licenciado el **1-ene-2025**; la
**SPA** consultó por su agenda regulatoria **2026-2027**; México con reforma
federal pendiente). Globant puede atenderlo — **pero con otros compradores, otro
marco regulatorio y otra lista de plataformas que esta página no releva.**

⇒ **Regla de uso de este archivo:** si el cliente es de **azar/apuestas**,
**ninguna fila de abajo aplica tal cual** y hace falta un relevamiento propio del
vertical. Si es de **videojuegos**, **ninguna cifra del informe UNLV/KPMG
aplica**. **La confusión es cara en las dos direcciones**, y es fácil porque el
informe del vertical ajeno es bueno: cuantitativo, reciente e institucional.

🔵 **Nota de método para la flota:** clasificar la fuente en `videojuegos` o
`azar/iGaming` **antes** de citar la cifra, y declararlo en la misma línea. Es la
Receta 14 (ejes de alcance de un grant) aplicada al **alcance de un número**.

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
| 🆕 **Agones** | **Apache-2.0** — 🆕 **+ grpc-gateway vendorizado** (`proto/grpc-gateway/LICENSE.txt` y `sdks/rust/proto/grpc-gateway/LICENSE.txt`, enumerados 2026-11-21 cycle 6: hay que propagar el aviso) | [agones-dev/agones](https://github.com/agones-dev/agones) | Go + Kubernetes CRDs | **La pieza que faltaba.** Orquestación y autoescalado de game servers dedicados sobre K8s (`Fleet`/`GameServer`/`FleetAutoscaler`). Complementa a Pterodactyl, no lo reemplaza: panel para operar hosts, Agones para capacidad elástica. 🔴 renombrado desde `googleforgames/agones`. 7.106★ |
| 🆕 🟢 **Bevy** | 🟢 **MIT *o* Apache-2.0 a elección** (`LICENSE-MIT` 1036 B / `LICENSE-APACHE` 10173 B; el doble grant está replicado en **cada crate** — 164 archivos de licencia enumerados) | [bevyengine/bevy](https://github.com/bevyengine/bevy) | Rust (ECS) | 🔴 **Faltaba desde el pass 1 y es 49.914★.** Engine 2D/3D data-driven. **La respuesta a "¿qué uso si el cliente exige MIT y el Qt/LGPL del editor de O3DE lo bloquea?"** — pregunta que esta KB no podía contestar hasta hoy. Releases 0.19 (19-jun-2026) y 0.20 (8-oct-2026: rendering Solari, escenas BSN). ⚠️ **Rust**: la restricción real es el equipo del cliente, no la licencia. |
| 🆕 **Phaser** | MIT (`LICENSE.md`, 1100 B) | [phaserjs/phaser](https://github.com/phaserjs/phaser) | TypeScript/JS (WebGL) | 40.756★. Engine 2D web: **el camino más corto a un jugable sin instalación**, ideal para pilotos y demos de cliente. Es además el target que usa `OpenGame` para generar juegos. |
| 🆕 **libGDX** | Apache-2.0 (`LICENSE`, 10174 B — sin apéndice) | [libgdx/libgdx](https://github.com/libgdx/libgdx) | Java/JVM | 25.553★. Desktop + Android + iOS + web desde una base Java. **La opción para un cliente que ya es una casa JVM** y va a mantener el código él mismo. |
| 🆕 **Flame** | MIT (`LICENSE`, 1067 B) | [flame-engine/flame](https://github.com/flame-engine/flame) | Dart/Flutter | 10.821★. **Gamificar dentro de una app Flutter existente** sin incrustar un engine ajeno. Commit de hoy. |
| 🆕 **Cocos Creator** | MIT (`LICENSE.md`, 1164 B) — badge `NOASSERTION` | [cocos/cocos-engine](https://github.com/cocos/cocos-engine) | TypeScript/C++ | 9.941★. Muy fuerte en **mobile de APAC**, China en particular. 🔵 **Se elige por región y por talento disponible**, no por estrellas. |
| 🆕 **raylib** | Zlib (`LICENSE`, 877 B) | [raysan5/raylib](https://github.com/raysan5/raylib) | C | 35.758★. Sin dependencias, muy simple. **Zlib no exige aviso de atribución en el binario** ⇒ la licencia más limpia del inventario para entregables embebidos. Commit de hoy. |
| 🆕 **Panda3D** | BSD-3-Clause (`LICENSE`, 1569 B) — badge `NOASSERTION` | [panda3d/panda3d](https://github.com/panda3d/panda3d) | C++ con API Python | 5.280★. Origen Disney/CMU. **La opción Python**: equipos de ML/data science que no quieren salir de Python para construir el entorno. |

### 🔴 Plataformas evaluadas y descartadas, con motivo

| Plataforma | Problema verificado |
|------------|--------------------|
| 🟢 ~~`namazustudios/elements`~~ | **FILA RETIRADA DE ESTA TABLA — el descarte era incorrecto.** Decía "AGPL-3.0 (`LICENSE.txt`, 16726 B), copyleft de red, fuera de la ruta permisiva". El archivo, con **esos mismos 16726 B**, dice *"Mozilla Public License Version 2.0"*. **AGPL-3.0 son ~34.500 B** — y este mismo KB anotaba 34523 B para la AGPL de Grafana, así que **el dato que refutaba la etiqueta ya estaba escrito al lado**. Movido a la tabla de plataformas base. Nuevo slug: `NamazuStudios/elements`. |
| 🆕 **Open Game Backend** (`rivet-gg/opengb`) | 🔴 **No se agrega a la tabla de arriba porque dejó de existir como producto de gaming** (verificado 2026-11-21 cycle 6). Figura en la prensa de backends como la alternativa Apache-2.0 a Nakama. Es una **cadena de renames de 3 saltos**, cada eslabón invisible para el API y servido por `raw`: su README dice *"Moved to `rivet-gg/toolchain`"* → ese repo está **archivado** y apunta a `rivet-gg/rivet` → ése es el redirect de **organización** (`rivet-gg` → `rivet-dev`) y el canónico es [rivet-dev/rivet](https://github.com/rivet-dev/rivet) (6.970★, Apache-2.0, `LICENSE` 11350 B, commit del 2026-11-21). **Y lo que hay al llegar ya no es un backend de juego:** es *"the orchestrator for agentic workloads"*, con hermanos [agentos](https://github.com/rivet-dev/agentos) (4.912★, sandboxes WASM) y [agents](https://github.com/rivet-dev/agents) (1.652★). ⇒ **la alternativa Apache-2.0 a Nakama en orquestación sigue siendo Agones.** Ver `intel/trends.md` nº 18. |
| 🆕 🔴 **Defold** ([defold/defold](https://github.com/defold/defold)) | 🔴 **No es open source, y la restricción apunta a Globant.** Badge `NOASSERTION`; el byte-read da `Defold License 1.0` (`LICENSE.txt`, 9977 B) = **Apache-2.0 verbatim** + la cláusula **4(a): *"You do not sell or otherwise commercialise the Work or Derivative Works as a Game Engine Product"***, donde *Game Engine Product* se define como software de desarrollo de videojuegos **incluida la herramienta de autoría**. Es una **restricción de campo de uso** ⇒ falla el punto 6 de la OSD. 🟢 **Matiz que hay que decir bien:** hacer **un juego** con Defold está dentro del grant (el engine es excelente y tiene commit de hoy, 6.476★); lo que queda fuera es **vender un pipeline de autoría asistido por AI** — exactamente las Recetas 6 y 11. **Única licencia del inventario cuyo límite es el modelo de negocio de la consultora, no el del cliente.** |
| [pelican-dev/panel](https://github.com/pelican-dev/panel) | 🔴 **Agravado: el repo ya no existe.** En octubre era "vivo pero sin archivo de licencia"; hoy `repo:` → `NOT_FOUND` y `raw` → 404. Usar `pterodactyl/panel`. |
| [grafana/grafana](https://github.com/grafana/grafana) | 🔴 **AGPL-3.0** (`LICENSE`, 34523 B), **no Apache-2.0** como publicaba este KB. Sigue siendo utilizable, pero como servicio expuesto a terceros arrastra obligaciones. Alternativas: Apache Superset (Apache-2.0), Metabase (AGPL-3.0 core + comercial). |

---

## Juegos open source para fork + AI

⚠️ **En gaming, un solo `LICENSE` casi nunca cubre código *y* assets.** Las filas
de abajo tienen grants distintos por capa, verificados leyendo la cabecera de cada
archivo. Forkear el repo y quedarse con los assets es el error caro.

🆕 **2026-11-21 (cycle 6) — y la advertencia de arriba no era sólo para juegos
completos: aplica a los *plugins* que esta página recomienda.** Enumerando los
archivos de licencia (canal nuevo: `gh api search/code -f q='repo:<slug>
filename:LICENSE'`), **[limbonaut/limboai](https://github.com/limbonaut/limboai)
—el plugin de behavior trees que la ruta recomendada de abajo usa— tiene TRES
grants, no uno**: `LICENSE.md` (1103 B, **MIT**, el código),
`LOGO_LICENSE.md` (212 B, **CC BY 4.0**) y `demo/LICENSE_ASSETS.md` (280 B,
**CC BY 4.0**, los PNG/SVG de `assets/`). Esta KB lo publicaba como "MIT" plano.
⇒ **el código se puede relicenciar; forkear el proyecto demo como punto de
partida arrastra atribución CC BY 4.0** — y el demo es justo lo que un equipo
forkea el primer día. Mismo patrón que Luanti, pero en una dependencia de 3.137★
que llevábamos un mes recomendando sin la nota.

🆕 **La lección de nombre de archivo, con testigo nuevo.** `OpenLiberty` publica
su grant **sólo en `LICENCE`** (ortografía británica, sin `S`): una sonda que
pida `LICENSE` recibe 404 y la fila se publica como "sin licencia". Con este
caso, los repos reales de esta KB cubren **seis** de las 11 variantes de nombre
de la lista de sondas: `License.txt` (HY-World, recastnavigation), `license.txt`
(LÖVE), `COPYING` (SuperTuxKart), `LICENSE.md` (pterodactyl, stride, ml-agents,
limboai), `LICENSE-MIT`+`LICENSE-APACHE` (c2pa) y ahora **`LICENCE`**.

| Juego | Licencia del **código** | Licencia de los **assets** | Repo | Por qué útil |
|-------|------------------------|---------------------------|------|--------------|
| **Luanti** (ex-Minetest) | **LGPL-2.1-or-later** (`doc/lgpl-2.1.txt`, 26406 B) | **CC-BY-SA 3.0** (`LICENSE.txt`, 8793 B — cubre sólo texturas y sonidos) | [luanti-org/luanti](https://github.com/luanti-org/luanti) | Mundo voxel completo. Base ideal para PCG con AI y NPCs agénticos. 🔴 **Corrección 2026-11-20:** el pass anterior decía que `minetest/minetest` *"sigue resolviendo"*. Resuelve **por redirect**: `repo:minetest/minetest` → `NOT_FOUND` en la API, y `raw` del slug viejo da 200 en `HEAD`/`master` y **404 en `main`**. El único slug canónico es `luanti-org/luanti` (**13.882★, commit de hoy** — la más activa de las tres). |
| **0 A.D.** | **GPL-2.0-or-later** (🆕 precisado 2026-11-21: `LICENSE.txt` dice *"GPL version 2 (or later)"*, y agrega subdirectorios **MIT** y **Apache-2.0** vendorizados) | **CC-BY-SA** (`LICENSE.txt`, 3049 B, agregado por directorio) | 🔴 [0ad/0ad](https://github.com/0ad/0ad) — **ARCHIVADO** | RTS completo, AI de unidades mejorable con ML, **pero el repo de GitHub está `archived: true` y su propia descripción dice `[DEPRECATED] Git mirror … migrated to gitea.wildfiregames.com on August 20th 2024`.** Último commit 2024-08-17. **El juego está vivo; su presencia en GitHub está muerta.** Para forkear hay que ir al Gitea propio del proyecto, fuera de GitHub. 2.827★ |
| **SuperTuxKart** | **GPL-3.0-or-later** (`COPYING`, 35245 B) | Mezcla **GPL / CC-BY / CC-BY-SA / dominio público** | [supertuxkart/stk-code](https://github.com/supertuxkart/stk-code) | Juego de carreras completo. AI de oponentes mejorable con RL. |
| 🆕 **OpenLiberty** (2026-11-21 cycle 6) | **MIT** — ⚠️ el grant está **sólo en `LICENCE`** (1066 B, ortografía británica); `LICENSE` devuelve 404 | 🔴 **ninguno: el juego no trae assets, los carga** | [openfw-game/OpenLiberty](https://github.com/openfw-game/OpenLiberty) (493★, GDScript) | Mundo abierto en **Godot** que carga modelos y texturas de la era 3D de **GTA**. Es la única base de esta tabla con un engine **MIT** y mundo abierto completo ⇒ la mejor para PCG y NPCs agénticos **sin arrastrar copyleft**. 🔴 **El riesgo no es de licencia, es de contenido:** el grant cubre el *motor de carga*; los assets de GTA son de Take-Two y **no vienen en el repo**. Usable con assets propios; **inviable** como entregable que dependa de los originales. |

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
LimboAI (MIT el código — ⚠️ logo y assets del demo bajo CC BY 4.0) — behavior tree, decisión determinista
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

---
*Pass 2026-11-21 (cycle 6): canal de verificación nuevo
(`gh api search/code -f q='repo:<slug> filename:LICENSE'` → **enumeración** de
archivos de licencia, gap declarado "sin canal" en los dos passes anteriores).
**Correcciones de este pass en esta página:** `limbonaut/limboai` deja de ser
"MIT" plano y pasa a **MIT el código + CC BY 4.0 el logo y los assets del
demo**; `agones-dev/agones` gana la nota de grpc-gateway vendorizado;
`heroiclabs/nakama` confirmado (vendoriza gopher-lua, benigno);
`recastnavigation` confirmado zlib **en la librería** pero no en `RecastDemo`
(vendoriza imgui/implot). **Filas nuevas:** `openfw-game/OpenLiberty` (MIT en
`LICENCE`, mundo abierto Godot) en juegos para fork, y **Open Game Backend
explícitamente descartado** con la cadena de renames trazada y el pivot de Rivet
a infraestructura de agentes documentado. **Trap nuevo del método:** el cuerpo
de un 404 de `raw` son **14 bytes** — todo byte-count < 50 es un no-hallazgo, no
una licencia. Ver `agents/top.md` para el método completo y
`compose/patterns.md` Receta 17 para el procedimiento.*
