---
industry: gaming
region: Global
updated: 2026-11-25
---

## 🔴 Lo más importante del pass 2026-11-25 (cycle 21): **el plano del ESPECTADOR** —cómo el juego llega a quien NO lo está jugando— y el hallazgo de que esta capa **se parte en el encoder**

Veinte passes auditaron lo que el jugador **ve y juega** (NPC, voz, assets, world
models), lo que el estudio **construye** (la fábrica), lo que el backend **decide**
(flags, experimentos, LTV) y **cómo entra la plata** (el camino del dinero, cycle
20). Ninguno auditó la capa que lleva **la imagen del juego a un tercero**:
captura → encode → transporte → servidor → espectador → clip → relato → archivo.

Medido antes de este pass con `grep -ric` sobre las 9 páginas: `spectator`,
`tournament`, `bracket`, `highlight`, `commentary`, `Sunshine`, `Moonlight`,
`selkies`, `pixel streaming`, `cloud-gaming`, `retroarch`, `miniaudio`, `OpenAL`
= **0 ocurrencias**. (`elo` da 380 y es **falso positivo de substring** dentro de
*"modelo"* y *"desarrollo"*; `matchmaking` da 24 y **sí** está cubierto desde el
cycle 6 — Open Match y openskill — así que el torneo y el rating **no** son el
hueco: el hueco es la **transmisión**.)

Importa porque es la capa que monetiza el juego **sin vender el juego**: UA por
clip, esports, cloud gaming y co-streaming. Y porque es la primera capa de esta KB
donde **la respuesta a "¿puede Globant construir sobre esto?" cambia según de qué
lado del encoder esté la pieza**.

### 🔴 Hallazgo nº1: la frontera de licencia cae **exactamente** donde el software deja de tocar píxeles y empieza a mover bytes

Es la primera capa de esta KB donde **el copyleft es el default, no la excepción**.
No es una impresión: son 33 repos medidos uno por uno hoy, y la partición es limpia.

| Lado | Qué hace la pieza | Licencias **medidas** (sin una sola excepción permisiva del lado píxel) | Veredicto |
|---|---|---|---|
| **Píxel** | capturar la pantalla, encodear, decodear, filtrar, reproducir | GPL-2.0 · GPL-3.0 · LGPL-2.0 · LGPL-2.1 · MPL-2.0 | ⚠️ **No existe el stack MIT/Apache de captura.** Hay que elegir *cómo* se toca el copyleft, no *si* se toca |
| **Byte** | señalizar, transportar, servir, empaquetar, repartir | MIT · Apache-2.0 · ISC · BSD · MPL-2.0 (+ **1 AGPL-3.0**, ver nº5) | ✅ Construible y relicenciable |

La consecuencia operativa, que es lo vendible: **la regla "sólo MIT/Apache" de
esta KB no se puede cumplir en esta capa**, y el pass no la fuerza a mentir. Lo
que se hace en su lugar está en la Receta 56: **separación por proceso**. OBS,
FFmpeg y Sunshine se consumen como **ejecutable invocado**, no como biblioteca
enlazada, y el código propietario del cliente vive del otro lado de un socket o
de una CLI. El copyleft viaja con el binario que ya es copyleft y no alcanza al
producto de Globant.

### 🔴 Hallazgo nº2: la licencia de FFmpeg **la decide un flag de compilación**, no el repositorio — 9º eje de partición de grant de esta KB

`FFmpeg/FFmpeg` (68.102 ★, push 2026-11-25) declara `NOASSERTION` en el canal de
metadatos. Leído el archivo (`LICENSE.md`, 4.352 B) y verificado el texto, el
grant **no es uno**: es una función del comando `./configure`.

| Flag pasado a `configure` | Licencia resultante del binario | Verificado en |
|---|---|---|
| *(ninguno)* | **LGPL-2.1-or-later** (`COPYING.LGPLv2.1`, 26.517 B) | `LICENSE.md`: *"In combination the LGPL v2.1+ applies to FFmpeg"* |
| `--enable-gpl` | **GPL-2.0-or-later** | *"In this case, FFmpeg's license changes to GPL v2+"* |
| `--enable-version3` | **(L)GPL-3.0** | *"the configure parameter `--enable-version3` will activate this licensing option"* |
| `--enable-nonfree` | 🔴 **NO REDISTRIBUIBLE** | *"This will cause the resulting binary to be unredistributable"* |

Tres consecuencias que ningún gate anterior de esta KB detecta:

1. **Auditar el repo de FFmpeg da la respuesta equivocada.** La pregunta correcta
   no es "¿qué licencia tiene FFmpeg?" sino **"¿con qué flags se compiló el binario
   que estamos embarcando?"**. En un cliente que usa un FFmpeg de distribución o
   de Docker Hub, eso **no lo decidió el cliente**: lo decidió el empaquetador.
2. **`--enable-nonfree` es un punto de no retorno**, no una advertencia de
   licencia: habilita Fraunhofer FDK AAC y OpenSSL y el binario resultante **no se
   puede entregar**. Un pipeline que encodea audio AAC es exactamente el caso que
   lo pisa.
3. **Un solo archivo arrastra GPL-3**: `libavfilter/vf_lensfun.c` está documentado
   como *"GPL version 3 or later"* dentro de una lista que por lo demás es GPL-2+.
   El eje no es sólo el flag: es el **filtro** que se invoca.

Los 8 ejes previos de esta KB eran por organización, repo, directorio, rama,
comando, tipo de entregable, región y versión. Este es el 9º: **por configuración
de build**. Y es el primero donde la licencia **no está en ningún archivo del
repo** — está en la línea de compilación, que vive en el Dockerfile del cliente.

### 🟢 Hallazgo nº3: Epic regala **MIT** en la mitad de pixel streaming que importa, mientras el engine sigue bajo EULA

`EpicGamesExt/PixelStreamingInfrastructure` — servidor de señalización + frontend
web de Pixel Streaming de Unreal. Licencia leída (`LICENSE.md`, **1.051 B**):
texto **MIT verbatim**, firmado *"Copyright Epic Games, Inc."*, sin cláusula
añadida. Lo verifiqué palabra por palabra porque el canal de metadatos no lo
clasifica (no dice "MIT" en el encabezado, sólo el grant).

Es el mismo patrón de **grant partido por componente** que esta KB ya documentó,
pero por una vez **a favor**: el engine es EULA de Epic, y la pieza que uno
realmente necesita forkear —señalización, matchmaker web, player— es MIT. Para un
cliente con un título en Unreal que quiere demo jugable en browser, **la capa que
Globant modifica es la permisiva**.

### 🔴 Hallazgo nº4: la capa de **relato automático (AI caster) no tiene open source adoptable** — medido, no supuesto

Era la apuesta obvia del pass: AI que narra la partida. Busqué y **medí los 6
candidatos** que devuelve el canal. El resultado es que esta tier **no existe
todavía como oferta**:

| Repo | ★ | Licencia **probada** | Último push | Por qué no se adopta |
|---|---|---|---|---|
| [`showlab/livecc`](https://github.com/showlab/livecc) | 485 | 🔴 **SIN LICENCIA** (probados `LICENSE`, `LICENSE.md`, `LICENSE.txt`, `COPYING` → 404 los cuatro) | ⚠️ **2025-10-29** (13 meses) | El líder de la tier es **todos los derechos reservados** + dormido |
| [`zicojiao/worldcupvoice`](https://github.com/zicojiao/worldcupvoice) | 111 | **MIT** | 2026-06-25 | El **único** con licencia usable, pero atado al SDK **propietario** de Agora RTC |
| [`gibinb/CASTER-Football-Commentary`](https://github.com/gibinb/CASTER-Football-Commentary) | **0** | 🔴 SIN LICENCIA (4 nombres probados) | 2026-11-21 | Proyecto de tesis, 0 ★ |
| [`aarshroongta1/live-soccer-commentary`](https://github.com/aarshroongta1/live-soccer-commentary) | **0** | 🔴 SIN LICENCIA (4 nombres probados) | 2026-11-24 | 0 ★ |
| [`Arsa-1109/sports-play-by-play-generator`](https://github.com/Arsa-1109/sports-play-by-play-generator) | **0** | 🔴 Sin licencia declarada | 2026-11-19 | GPT desde cero, no LLM |
| [`ChintanTrivedi/football_ai_commentary`](https://github.com/ChintanTrivedi/football_ai_commentary) | 79 | 🔴 Sin licencia declarada | ⚠️ **2019-06-24** | 7 años muerto |

**4 de 6 tienen 0 ★ y 3 no tienen ningún archivo de licencia** (y la ausencia está
*probada* con cuatro nombres de archivo cada uno, no inferida — regla T87 de esta
KB). Sin archivo de licencia **no hay grant**: es código mirable, no usable.

El dato vendible no es "falta un repo". Es: **el relato automático es la pieza
que un cliente de esports va a pedir en 2027 y hay que construirla**, no
adoptarla. Las piezas con las que se construye **sí** existen y son MIT — están
en la tabla de abajo (`whisper.cpp` para el audio del casteo, el parser de replay
para el evento, un LLM para el texto). Receta 59.

### 🔴 Hallazgo nº5: una sola pieza AGPL-3.0 en el lado permisivo, y está justo donde más duele

`AirenSoft/OvenMediaEngine` es el servidor de medios sub-segundo de referencia
para WebRTC a escala. Licencia leída: **AGPL-3.0** (`LICENSE`, **34.453 B** —
concuerda con el byte-count AGPL ~34.500 del catálogo de esta KB). Es el **único**
repo del lado byte que no es permisivo, y por ser **servidor** el AGPL es
precisamente el peor caso: la obligación se dispara **por uso en red**, sin
distribuir nada.

Sustitutos permisivos medidos y verificados para el mismo trabajo: `ossrs/srs`
(**MIT**, 29.454 ★), `bluenviron/mediamtx` (**MIT**, 20.766 ★),
`livekit/livekit` (**Apache-2.0**, 22.381 ★). **No hay motivo para tocar el AGPL
en esta capa**, y conviene decirlo antes de que un arquitecto lo elija por
benchmark de latencia.

### Agentes y piezas AI verificados este pass (plano del espectador)

Verificación: existencia y rama default real por `git ls-remote --symref`;
estrellas, `archived` y `pushed_at` por el canal `search/repositories`; licencia y
byte-count leídos con `curl` sobre `raw.githubusercontent.com` **en la rama
default real** (ver nº6: en esta sesión `github.com` por HTML da 403 y
`api.github.com` da 403, así que éstos son los únicos canales vivos).

| Agente / pieza | ★ | Licencia (bytes) | Último push | Qué hace en el plano del espectador |
|---|---|---|---|---|
| [`ggml-org/whisper.cpp`](https://github.com/ggml-org/whisper.cpp) | **55.496** | **MIT** | 2026-11-24 | 🥇 ASR on-device del **audio del casteo**: convierte lo que dice el comentarista en el *timestamp* del evento. Base de todo clipper que no quiera API paga |
| [`openai/whisper`](https://github.com/openai/whisper) | **113.439** | **MIT** | ⚠️ 2026-08-31 (3 meses) | El modelo de referencia. **Usar `whisper.cpp` para runtime**: mismo grant, mantenimiento vivo y sin Python en el servidor de encode |
| [`royshil/obs-backgroundremoval`](https://github.com/royshil/obs-backgroundremoval) | **4.720** | ⚠️ **GPL-3.0** (35.141 B) | 2026-11-24 | Segmentación de fondo **en vivo dentro de OBS** (ONNX). El caso canónico de AI del lado píxel: **el plugin hereda la GPL del host** |
| [`Aseiel/VideoHighlighter`](https://github.com/Aseiel/VideoHighlighter) | 507 | ⚠️ **AGPL-3.0** | 2026-11-24 | Detección de highlights local (escena + movimiento + audio + objeto + transcripción) sobre Ollama. **AGPL ⇒ no se ofrece como servicio** sin liberar |
| [`zicojiao/worldcupvoice`](https://github.com/zicojiao/worldcupvoice) | 111 | **MIT** | 2026-06-25 | Comentarista AI en vivo sobre un feed RTC. Único AI caster con grant usable; **el transporte es propietario (Agora)** y hay que reemplazarlo por `pion/webrtc` |
| [`bendawg2010/Auto-clipper`](https://github.com/bendawg2010/Auto-clipper) | 14 | **MIT** | ⚠️ 2026-04-19 | Clipper de VOD de Twitch/YouTube con YOLO + análisis de píxel. **14 ★**: se cita como referencia de diseño, no como dependencia |
| [`mackron/miniaudio`](https://github.com/mackron/miniaudio) | **7.700** | **Elección de licencias** (2.597 B) | 2026-11-24 | Captura y mezcla de audio en un solo header. El archivo **ofrece dos grants a elección** (`NOASSERTION` en metadatos): hay que **declarar cuál se eligió** en el aviso del producto |

### 📐 Hallazgo nº6 (método): los dos canales que esta KB usa para verificar **están caídos para GitHub en esta sesión**, y el canal que queda **miente por omisión**

El prompt de este trabajo pide verificar cada URL con `curl -sI`. **En esta sesión
eso no verifica nada de GitHub** y hay que escribirlo para que el próximo pass no
lo descubra de nuevo:

| Canal | Resultado medido hoy | Veredicto |
|---|---|---|
| `curl -sI https://github.com/<repo>` | **403** para **todos** los repos, incluidos los que existen con certeza | 🔴 Inservible: el 403 es del proxy, **no del repo**. Un `curl` a `github.com` **nunca** distingue vivo de muerto acá |
| `WebFetch` sobre `github.com/<repo>` | **503** en 4 de 4 | 🔴 Inservible |
| `gh api repos/<repo>` | **403** salvo repos atados a la sesión | 🔴 Inservible para censo |
| `gh api search/repositories` con `repo:` | ✅ Da ★, `archived`, `pushed_at`, SPDX | ⚠️ **Miente por omisión**, ver abajo |
| `git ls-remote --symref` | ✅ 20 de 20 correctos, **+ la rama default real** | 🥇 Canal de existencia |
| `curl raw.githubusercontent.com/<repo>/<rama>/LICENSE` | ✅ Devuelve el texto y el byte-count | 🥇 Canal de licencia |

**El defecto nuevo, y es grave para cualquier censo:** el canal `search` con
`repo:` **descarta en silencio** los repos que no puede ver cuando van en lote, y
sólo tira `422` cuando el repo problemático va **solo**. Medido: pedí 10 repos y
volvieron 8, sin ningún aviso de cuáles faltaban. Cuatro repos **vivos** son
invisibles para ese canal:

- `LizardByte/Sunshine` — vivo (`ls-remote` OK, rama `master`), GPL-3.0 leída
- `AirenSoft/OvenMediaEngine` — vivo (rama `master`), AGPL-3.0 leída
- `EpicGamesExt/PixelStreamingInfrastructure` — vivo (rama `master`), MIT leída
- `selkies-project/selkies-gstreamer` — vivo (rama `main`)

🔴 **Esto corrige la firma de rename heredada del cycle 4.** Esa regla decía:
*`repo:` NOT_FOUND + raw del slug viejo 200 ⇒ rename*. Falso. Acá hay cuatro casos
de **`repo:` NOT_FOUND + raw del slug ACTUAL 200**, que significa algo distinto:
**el repo está vivo y el canal es ciego**. La regla nueva:

> Una ausencia en `search/repositories` **no es evidencia de nada**. Hay que
> diffear *pedidos vs devueltos* en cada lote, y re-probar el hueco con
> `git ls-remote`. Sólo `ls-remote` fallando es evidencia de que el repo no está.

Y el costo medido de no hacerlo: **las ★ de esos cuatro repos no se pueden medir
en este pass** y quedan declaradas como huecos, no rellenadas con números de la
web (una fuente decía "41K ★" para Sunshine; no se escribe porque no se midió).

### ⚫ Conjetura propia que se probó y **falló**, escrita en vez de borrada

Hipótesis: *el canal `search` es ciego a esos repos porque GitHub excluye los
**forks** del índice de búsqueda* (Sunshine viene de un fork). Era verificable:
agregar `fork:true` a la query debería traerlos. **Resultado: `422` igual, los tres
repos.** La hipótesis es **falsa** y la causa real sigue sin medir. Se escribe
porque el próximo pass no debe gastar el intento de nuevo.


## 🔴 Lo más importante del pass 2026-11-25 (cycle 20): **el camino del DINERO** —de la tienda al derecho del jugador— y el **canal 8: la sonda de RELICENCIABILIDAD**

Diecinueve passes auditaron lo que el jugador **ve, oye y juega** (NPCs, voz,
assets, world models), lo que el estudio **construye** (la fábrica) y lo que el
backend **decide** (flags, experimentos, LTV). Ninguno auditó **cómo entra la
plata y cómo sale el derecho que compró el jugador**. Medido antes de este pass,
en las 8 páginas de esta KB había **cero** ocurrencias de `payment`, `wallet`,
`receipt`, `entitlement`, `patcher`, `butler`, `killbill`, `medusa`, `saleor`,
`hyperswitch`, `tigerbeetle`, `fdroid` y `iap` como capa propia (`openiap`
aparecía sólo como nota suelta del cycle 19, y `lago`/`formance` eran **falsos
positivos de substring** dentro de *"performance"* y *"Santiago"* — verificado con
`grep -oiE '.{50}lago.{50}'`, que devuelve 0 líneas reales).

No es una capa accesoria: **2026 es el año en que el camino del dinero se abrió
por ley en las cuatro regiones** (ver `intel/market.md`), y es la primera capa de
esta KB donde el entregable de Globant es **transaccional** — si falla, no se ve
mal, se pierde dinero o se entrega dos veces.

> 🔴 **El hallazgo que vale el pass entero:** el seam que falta en todas las demás
> capas, acá **existe y es Apache-2.0**. `heroiclabs/nakama` incorporó en 2026 un
> **registro de proveedores IAP enchufables** (`server/iap_provider_registry.go`,
> cabecera `Copyright 2026 The Nakama Authors`), con
> `RuntimeIAPProviderPurchaseFunction`, `…SubscriptionFunction` y
> `…NotificationFunction`. Traducido: **se puede validar una compra hecha en una
> tienda que no existía cuando el engine se compiló** — que es exactamente lo que
> son los *alternative marketplaces* que la DMA, la MSCA japonesa y el acuerdo
> CADE de Brasil acaban de legalizar. El cycle 19 cerró diciendo que lo que
> faltaba era *"el último metro"*; en esta capa el último metro **ya está
> cableado**.

### 🔴 Hallazgo nº1: `coinbase/x402` ya no es el repo de x402 — y **no está archivado**, así que el filtro obligatorio del cycle 19 (T74) NO lo atrapa

El cycle 19 estableció como regla que `archived` es un filtro obligatorio, porque
el predecesor archivado se queda con las estrellas. Este pass encuentra un caso
**peor y más silencioso**: dos repos **vivos**, con el **mismo byte-count de
licencia** (11.324 B, Apache-2.0) y **HEAD distinto**:

```
$ git ls-remote --symref https://github.com/coinbase/x402 HEAD
ref: refs/heads/main   HEAD
0e6e8c55...                                  tags=53
$ git ls-remote --symref https://github.com/x402-foundation/x402 HEAD
ref: refs/heads/main   HEAD
222f3541...                                  tags=1538
```

No es un redirect (SHAs distintos ⇒ son dos repos reales). Lo que los distingue
**no está en ningún metadato**: está en las tres primeras líneas del README de
Coinbase, y lo dice el propio repo:

> *"We've moved the x402 repo under the x402 Foundation repo. All issues and PRs
> were transferred here. Our repo (`coinbase/x402`) is now a **development
> fork**."*

🔴 **Corrección y extensión de T74:** `archived` es **necesario pero no
suficiente**. Un repo de vendor puede ser **degradado a fork sin ser archivado**,
conservando nombre, estrellas, tags y licencia, y **el único canal que lo detecta
es leer las primeras líneas del README**. Regla nueva (**T75**): antes de
recomendar un repo de vendor cuyo protocolo pasó a una fundación, leer
`README.md | head -5`. Los 1.538 tags contra 53 son el segundo indicador, no el
primero.

### Agentes y especificaciones verificados este pass (capa dinero)

Todos los renglones de abajo fueron probados uno por uno: existencia por
`git ls-remote --symref` (no por HTML ni por API — ambos dan 403 desde esta
sesión), y licencia leída por `raw.githubusercontent.com` **sobre la rama default
real**, con su byte-count.

| Agente / spec | Repo | Licencia (bytes medidos) | Rama default | Tags | Qué hace en el camino del dinero |
|---|---|---|---|---|---|
| **ACP** (Agentic Commerce Protocol) | [`agentic-commerce-protocol/agentic-commerce-protocol`](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol) | **Apache-2.0** (10.947 B) | `main` | **0** | El **checkout** del agente: RFCs + OpenAPI + JSON Schema. Creado por Stripe y OpenAI |
| **AP2** (Agent Payments Protocol) | [`google-agentic-commerce/AP2`](https://github.com/google-agentic-commerce/AP2) | **Apache-2.0** (11.359 B) | `main` | 2 | La **autorización** del pago iniciado por un agente. Donado por Google a la FIDO Alliance |
| **x402** (canónico) | [`x402-foundation/x402`](https://github.com/x402-foundation/x402) | **Apache-2.0** (11.324 B) | `main` | **1.538** | El **movimiento** del pago (HTTP 402 nativo). Hoy bajo la Linux Foundation |
| **x402** (fork de desarrollo) | [`coinbase/x402`](https://github.com/coinbase/x402) | **Apache-2.0** (11.324 B) | `main` | 53 | ⚠️ **Degradado a dev fork por su propio README** — es el que linkea casi toda la prensa |
| **LedgerForge** | [`devaccuracy/ledgerforge`](https://github.com/devaccuracy/ledgerforge) | **Apache-2.0** (10.749 B, en `LICENSE.md`) | `main` | 2 | Ledger doble entrada con **servidor MCP** y **límites de gasto y aprobaciones impuestos del lado del servidor para agentes AI** |
| **Nakama — registro IAP** | [`heroiclabs/nakama`](https://github.com/heroiclabs/nakama) | **Apache-2.0** (11.358 B) | `master` | 219 | El **seam enchufable**: registrar un validador de compra propio para una tienda arbitraria (último commit 2026-11-24) |

⚠️ **Límite declarado de los tres protocolos (ACP/AP2/x402):** son **capa de
especificación**, no producto. ACP tiene **0 tags** y se versiona **por carpeta**
(`2026-04-17`), no por release — así que **una dependencia no se puede pinear**, y
eso es un riesgo de build, no una opinión. AP2 tiene 2 tags. Ninguno de los tres
publica paquete Unity ni Unreal: acá sí sigue faltando el último metro.

⚠️ **No se midió** la adopción real de ninguno de los tres (sin API de GitHub
desde esta sesión no hay estrellas, forks ni dependientes verificables). Las
cifras que circulan —p. ej. *"165 millones de transacciones de agentes"* para
x402— vienen de fuentes secundarias y **no se publican acá como dato**.

### 🔴 Hallazgo nº2: el canal 8 —**la sonda de relicenciabilidad**— y sus tres defectos medidos

El canal 7 (cycle 19) mide la **deriva** de un carve-out: detecta que una licencia
empeoró **después** de que empeoró. Falta el **predictor**. Quién puede
relicenciar mañana depende de **quién es el titular del copyright**: una sola
empresa titular puede relicenciar unilateralmente (el patrón Unleash / Flipt /
Midaz); un colectivo de contribuyentes o una fundación, no.

Sonda aplicada a 20 repos de esta capa: `CONTRIBUTING.md` (+5 rutas alternativas),
`.github/workflows/{cla,dco}.yml`, y la **línea de copyright** de
`LICENSE`/`NOTICE`. Resultado honesto — **la sonda falla en 14 de 20**, y las
causas son el hallazgo:

| Familia de licencia | Dónde vive el titular | Qué devuelve la sonda |
|---|---|---|
| **MIT / BSD** | En la cabecera de la propia licencia | ✅ Funciona: 7 de 7 leídos |
| **Apache-2.0** | **No está en `LICENSE`** (el texto trae el placeholder `Copyright [yyyy] [name of copyright owner]`) ⇒ vive en `NOTICE` o en ningún lado | ⚠️ Sólo `hyperswitch` tiene `NOTICE` (*Juspay Technologies Private Limited*). 7 repos Apache **no declaran titular en ninguna ruta canónica** |
| **GPL / AGPL** | **La línea de copyright de `LICENSE` es la de la FSF sobre el texto de la licencia** (`Copyright (C) 2007 Free Software Foundation`) — **no la del proyecto** | 🔴 **Falso positivo en 5 de 5**: lago, fdroidserver, SideStore, Heroic y Obtainium "devuelven" FSF. El titular real está en las cabeceras **por archivo** |

🔴 **El defecto nº3 es el más caro y es propio de esta sonda:** en la familia
copyleft la sonda no sólo falla, **miente con formato correcto**. Un pipeline que
compile "titular = Free Software Foundation" para 5 repos produce exactamente el
tipo de dato inventado que esta KB existe para no producir. **Regla (T76): el
canal 8 sólo se corre sobre MIT/BSD; sobre Apache se corre contra `NOTICE`; sobre
GPL/AGPL NO se corre contra `LICENSE` nunca.**

Dónde sí vive el titular en copyleft y Apache, verificado a mano:

- `killbill/killbill` → en la cabecera de `pom.xml`, y es una **cadena de sucesión
  corporativa de 16 años**: `Ning, Inc. (2010-2014)` → `Groupon, Inc (2014-2020)`
  → `Equinix, Inc (2020)` → `The Billing Project, LLC (2014-2020)`. Un proyecto
  Apache-2.0 de **689 tags** cuyo titular cambió de dueño dos veces, y nada de eso
  se ve desde `LICENSE`.
- `heroiclabs/nakama` → cabecera por archivo: `Copyright 2026 The Nakama Authors`.
  Titular **colectivo**, no la empresa (Heroic Labs) ⇒ señal de durabilidad, con
  el límite obvio de que la empresa controla el repo.
- `velopack/velopack` → 🔴 **dos líneas de copyright en el mismo MIT**:
  `Copyright © 2021 Caelan Sayler` **y** `Copyright © 2024 Velopack Ltd.` Es la
  transición **individuo → empresa** capturada dentro del archivo de licencia. Por
  el canal 8 es **el predictor más limpio de toda la tabla**: a partir de 2024 hay
  una sociedad co-titular, y una sociedad co-titular es la que firma un cambio de
  licencia.

### ⚫ Conjetura propia que 404'eó y se escribe en vez de borrarse

`LerianStudio/midaz` — asumí que el ledger open source insignia de LATAM seguía
público porque toda la web lo cita así. **No es legible públicamente**:
`git ls-remote` pide credenciales (401, no 404) y `raw` da 404 en `LICENSE`,
`README.md` y `go.mod` en `develop` y en `main`. Ver `intel/market.md` §LATAM: es
el hallazgo regional del pass, y **corrige una regla del cycle 19**.

Host que se agrega a la lista de policy-dead desde esta sesión:
`beta.pkg.go.dev` (403 `connect_rejected` por el proxy de egress) ⇒ se cita en
prosa, no se linkea.

# 🎮 Agentes AI — Gaming

> Agentes y herramientas AI open source. Foco: MIT / Apache 2.0 / BSD / Zlib.
> Última actualización: **2026-11-22 (pass cycle 19)**. Passes previos: 2026-11-22
> (cycles 18, 17, 16, 15, 14, 13, 12, 11, 10), 2026-11-21 (cycles 9, 8, 7, 6, 5), 2026-11-20,
> 2026-10-26, 2026-07-02.
>
> 🔴 **Lo que el pass cycle 19 agrega a los seis canales:** el **canal 7 — el
> censo del carve-out y su DERIVA**. Los canales 1-6 contestan *qué dice* la
> licencia; ninguno contesta **qué proporción del repo** cubre el pedazo no libre
> ni **si esa proporción crece**. Dos comandos sobre el clon *blobless* que ya
> pide el canal 6. Primer resultado: el carve-out de GrowthBook pasó de
> **0,73 % a 4,55 %** del árbol en 20 meses **sin cambiar de licencia**, mientras
> el de PostHog creció en archivos (222 → 836) y **encogió** en proporción
> (7,35 % → 1,12 %). El conteo absoluto engaña; la proporción no. Ver los
> hallazgos del cycle 19.
>
> 🔴 **Lo que el pass cycle 18 agrega a los cinco canales:** el **canal 6 —
> `git ls-tree -r HEAD` sobre un clon `--filter=blob:none --no-checkout`**. Los
> canales 1-5 necesitan **saber la ruta** del archivo de licencia; el canal 6
> contesta *qué archivos existen*. Primer resultado: `SkyworkAI/Matrix-Game` tiene
> **28** archivos de licencia donde esta KB publicaba 7, y el nº 28 es
> **CC BY-NC 4.0** dentro del benchmark. Ver los hallazgos del cycle 18.
>
> 🔴 **Lo que el pass cycle 17 agrega a los cuatro canales de licencia:** el
> **canal 5 — `git log --follow -- LICENSE`**. Los canales 1-4 (badge, archivo,
> enumeración, prosa del README) **sólo ven el presente**, y un relicenciamiento no
> deja rastro en el archivo resultante: el `LICENSE` de `ynput/ayon-backend` leído
> hoy es un FSL impecable y hasta el 2025-01-27 fue Apache-2.0. **La licencia de un
> repo es una función del tiempo y del dueño.** Ver los hallazgos nº1 a nº3 del
> cycle 17.
>
> 🔴 **Lo que el pass cycle 14 cambia de raíz:** los trece passes anteriores
> asumieron que la licencia de un repo vive **dentro** del repo —badge, archivo,
> enumeración de archivos—. Este pass mide que **no**: los dos generadores 3D
> MIT más grandes del mundo (**27.514 ★ sumadas**) declaran en **prosa del
> README**, y en ningún archivo de licencia, que sus renderers dependen de
> paquetes **no comerciales**. La licencia de un proyecto es la licencia de su
> **clausura de dependencias**. Canal 4, nuevo y obligatorio desde hoy. Ver los
> hallazgos nº1 a nº5 del cycle 14.
>
> 🔴 **Lo que el pass cycle 13 cambia de raíz:** los doce passes anteriores
> trabajaron el mismo eje (engines, MCP, NPCs, world models, backends) y ese eje
> está **medido como saturado**: 13 de 15 candidatos ya estaban escritos. La capa
> que el jugador **lee y escucha** —diálogo autorado, moderación de chat, voz,
> localización— tenía **cero filas** y aporta **24 repos nuevos** y **tres
> familias de licencia que esta KB no tenía** (YSPL 1.0, Fish Audio Community
> 1.0, SPDX short-form). Ver los hallazgos nº1 a nº6 del cycle 13.
>
> 🔴 **Lo que el pass cycle 12 cambió de raíz:** los seis passes anteriores afinaron
> *cómo leer una licencia*. Este pass descubre que **la lista de repos sobre la
> que se aplicaba esa lectura tenía un agujero de 50.000 ★** — y que el checksum
> de bytes, la técnica estrella de la casa, **produce falsos negativos medidos**.
> Ver los hallazgos nº1, nº2 y nº3.
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

## 🔴 Lo más importante del pass 2026-11-22 (cycle 19): **el plano de DECISIÓN** —la capa que decide qué ve y qué paga cada jugador— y el **canal 7: el censo del carve-out y su deriva**

Dieciocho passes auditaron lo que el jugador **ve, oye y juega**: NPCs, world
models, voz, assets, accesibilidad, el plano de datos, la fábrica de build. Ninguno
auditó la capa que decide **qué** ve cada jugador y **a qué precio**: *remote
config*, *feature flags*, asignación de experimentos, personalización, LTV. En las
8 páginas de esta KB, antes de este pass, había **cero** ocurrencias de
`growthbook`, `unleash`, `flagsmith`, `openfeature`, `flipt`, `gorse`, `bandit`,
`uplift` y `remote config`.

No es una capa accesoria: es **la capa que la regulación de 2026 persigue por su
nombre** (ofertas personalizadas, precios individualizados, loot boxes a menores,
ver `intel/market.md`), y es la única capa de esta KB donde el entregable de
Globant *es* la decisión, no el contenido.

> 🟡 **Esto reencuadra —no contradice— el gap que el cycle 16 declaró** en
> `verticals/solutions.md`: *"no existe una plataforma de live-ops open source y
> permisiva para juegos"*. Medido hoy: **sí existen ocho plataformas permisivas y
> auto-hospedables** para flags/experimentos/recomendación. Lo que **no** existe es
> **el SDK de engine**: ninguna de las ocho publica paquete Unity o Unreal. El gap
> no era la plataforma, era **el último metro hasta el cliente de juego**, y eso
> cambia el presupuesto de un proyecto de "construir la plataforma" a "escribir dos
> SDKs delgados sobre un protocolo abierto".

### 🔴 Hallazgo nº1: el líder de la capa (**14.337 ★**) dejó de ser Apache-2.0 el **2026-05-26**, y casi toda la web lo sigue publicando como permisivo

`Unleash/unleash` aparece en todas las comparativas como *"open source, Apache
2.0"*. El **canal 5** (`git log --follow -- LICENSE`, incorporado en el cycle 17)
lo fecha al día:

```
$ git clone --filter=blob:none --no-checkout https://github.com/Unleash/unleash
$ git log --follow --format='%h %ad %s' --date=short -- LICENSE
34b5661eb 2026-05-26 task: Move to AGPL-v3 (#12086)
e6f6b9ac1 2020-05-12 fix: license year and company
f80d9a882 2014-09-29 Initial commit
```

| Medición | Valor verificado hoy |
|---|---|
| Commit del relicenciamiento | `34b5661eb631a80f4af3b0fa6dabe9969b6c2d4a`, 2026-05-26 10:15:30 +0200, Christopher Kolstad, PR #12086 |
| `LICENSE` **antes** | **11.316 B** = Apache-2.0 íntegro |
| `LICENSE` **después** | **34.523 B** = GNU AGPL-3.0 |
| Lo que instala npm hoy | `package.json` de HEAD: `"name": "unleash-server"`, `"version": "8.3.1"`, **`"license": "AGPL-3.0-or-later"`** |
| Último release permisivo recuperable | **v7.6.4** (2026-05-14); el beta hermano es `v7.6.5-beta.0`, misma fecha |
| Primera línea AGPL | la línea **v8** (el cambio se hizo efectivo con el salto de major) |

**Y la mitad que salva el proyecto:** el corte es **por componente**, igual que
AYON en el cycle 17. Los SDKs —lo que de verdad se enlaza dentro del juego o del
backend del cliente— **siguen siendo permisivos**, uno por uno, leídos por badge
hoy:

| SDK | ★ | Licencia | Último push |
|---|---|---|---|
| [Unleash/unleash-node-sdk](https://github.com/Unleash/unleash-node-sdk) | 226 | **Apache-2.0** | 2026-11-20 |
| [Unleash/unleash-go-sdk](https://github.com/Unleash/unleash-go-sdk) | 150 | **Apache-2.0** | 2026-08-20 |
| [Unleash/unleash-java-sdk](https://github.com/Unleash/unleash-java-sdk) | 141 | **Apache-2.0** | 2026-10-30 |
| [Unleash/unleash-python-sdk](https://github.com/Unleash/unleash-python-sdk) | 92 | **MIT** | 2026-11-19 |
| [Unleash/unleash-dotnet-sdk](https://github.com/Unleash/unleash-dotnet-sdk) | 87 | **Apache-2.0** | 2026-11-09 |
| [Unleash/unleash-php-sdk](https://github.com/Unleash/unleash-php-sdk) | 65 | **MIT** | 2026-11-12 |
| [Unleash/unleash-ruby-sdk](https://github.com/Unleash/unleash-ruby-sdk) | 63 | **Apache-2.0** | 2026-08-24 |
| [Unleash/unleash-edge](https://github.com/Unleash/unleash-edge) | 67 | **MIT salvo `crates/enterprise/`** (1.313 B; el propio archivo lo dice en su primera línea) | 2026-11-20 |

⚠️ **Los siete SDKs están RENOMBRADOS.** `Unleash/unleash-client-go` y
`Unleash/unleash-proxy-client-js` devuelven **HTTP 422** (la firma de
*inexistente o renombrado* de esta KB, cycle 4): el patrón nuevo es
`unleash-<lenguaje>-sdk`. Cualquier documentación interna, `go.mod` o README que
apunte a los slugs viejos está apuntando a un redirect, no a un repo.

### 🔴 Hallazgo nº2: `flipt-io/flipt` publica **TRES licencias en un repo**, y cuál te toca depende de la **rama** y del **subdirectorio**

El badge dice `NOASSERTION`. Una comparativa de un tercero (Statsig) dice *"100%
open source, sin ediciones de pago"*. Las dos cosas son falsas, y en direcciones
distintas:

| Lo que ejecutás | Qué resuelve | Licencia real (byte-read hoy) |
|---|---|---|
| `git clone` sin flags | rama por defecto = **`v2`** | **FCL-1.0-MIT** (*Fair Core License*), fuente disponible, **no** OSI |
| la rama `main` (v1, viva: último commit 2026-11-17) | v1 | **GPL-3.0** íntegro, **35.149 B** |
| `go get .../sdk/go`, `errors/`, `rpc/flipt/` | 3 subdirectorios del árbol v2 | **MIT**, 1.076 B cada uno |

Cláusulas de la FCL-1.0-MIT leídas del archivo, no de un resumen:

- **Permitted Purpose** = cualquier propósito **que no sea** *Competing Use*, y
  *Competing Use* se define como ofrecer el software a terceros en un producto o
  servicio comercial que **lo sustituya** o que ofrezca *"the same or substantially
  similar functionality"*.
- 🟢 **Y la cláusula que decide esto para una consultora:** los *Permitted Purposes*
  incluyen explícitamente *"in connection with **professional services that you
  provide to a licensee** using the Software"*. **Desplegar Flipt v2 dentro del
  cliente, como servicio profesional, está expresamente permitido**; empaquetarlo
  como *"la plataforma de flags de Globant"* es *Competing Use*. Es la primera
  licencia no-OSI de esta KB que **contempla el modelo de negocio de Globant por
  escrito**.
- **Limitations:** prohibido *"move, change, disable, or circumvent the license key
  functionality"*. El árbol lo confirma: `internal/coss/license/license.go`
  (*coss* = commercial open source software).
- **Grant of Future License:** licencia MIT adicional e irrevocable **efectiva en el
  segundo aniversario** de la fecha en que cada versión se publicó. El código de
  hoy es MIT en 2028, versión por versión.

### 🆕 Canal 7 · **el censo del carve-out y su DERIVA** — y el número accionable es la **proporción**, no el conteo

Los canales 1-6 contestan *qué dice la licencia*. Ninguno contesta **cuánto del
repo** cubre el pedazo no libre, ni **si ese pedazo está creciendo**. Con el clon
*blobless* del canal 6 ya descargado, son dos comandos:

```bash
# censo: qué parte del árbol está bajo el carve-out, hoy
git ls-tree -r --name-only HEAD | grep -cE '(^|/)enterprise/'
git ls-tree -r --name-only HEAD | wc -l

# deriva: lo mismo, año por año, sin cambiar de clon
for y in 2022 2023 2024 2025 2026; do
  c=$(git rev-list -1 --before=$y-11-22 HEAD)
  echo "$y $(git ls-tree -r --name-only $c | grep -cE '(^|/)ee/') / $(git ls-tree -r --name-only $c | wc -l)"
done
```

Aplicado a las **dos** plataformas líderes de esta capa, el resultado va en
**direcciones opuestas** — y ninguna de las dos cambió nunca de licencia:

| Fecha | GrowthBook · `enterprise/` | % del árbol | PostHog · `ee/` | % del árbol |
|---|---|---|---|---|
| 2022-11-22 | — | — | 222 | **7,35 %** |
| 2023-11-22 | 12 | **0,73 %** | 247 | — |
| 2024-11-22 | 15 | **0,65 %** | 339 | **4,26 %** |
| 2025-11-22 | 72 | **2,39 %** | 706 | — |
| 2026-11-22 | **317** | **4,55 %** | **836** | **1,12 %** |

- **GrowthBook: ×26 en archivos y ×6,2 en proporción** en 20 meses. La licencia no
  cambió: cambió **cuánto cubre**. El `LICENSE` raíz (1.716 B) es *MIT Expat* con
  tres directorios exceptuados (`packages/back-end/src/enterprise`,
  `packages/front-end/enterprise`, `packages/shared/src/enterprise`), y el canal 5
  fecha las dos mudanzas: `ae7a3862f` (2023-07-10, *"Move Enterprise code to separate
  package"*) y `3467bdcff` (2025-03-01, *"Split enterprise code into multiple
  directories"*).
- **PostHog: creció en archivos (222 → 836) y ENCOGIÓ en proporción (7,35 % → 1,12 %)**,
  porque el árbol libre creció más rápido (3.019 → 74.626 archivos). Un titular de
  *"el carve-out de PostHog casi se cuadruplicó"* sería cierto y **engañoso**.
- 🔴 **La regla:** el conteo absoluto no es un indicador; **la proporción sí**, y
  **la derivada de la proporción** es lo que hay que poner en un informe de riesgo.
  Dos productos de la misma capa, misma técnica de licencia, tendencias contrarias.

El texto del carve-out de GrowthBook (`packages/back-end/src/enterprise/LICENSE`,
2.333 B) agrega una cláusula que vale leer antes de contribuir: las modificaciones
y parches que escribas sobre ese código quedan en propiedad de GrowthBook —
*"You agree that GrowthBook and/or its licensors (as applicable) retain all right,
title and interest in and to all such modifications and/or patches"*— y sólo pueden
usarse con licencia Enterprise válida.

### 🆕 Reglas nuevas de método (T71-T76)

- **T71 · Un relicenciamiento no deja huella en el artefacto, sólo en la historia.**
  Hoy el badge de Unleash dice AGPL, pero cada comparativa, roundup y README de
  tercero sigue diciendo Apache-2.0 porque **el archivo no "cambió", fue
  reemplazado**. El canal 5 da la fecha **y** el último tag permisivo recuperable;
  sin él sólo hay opiniones con fecha desconocida.
- **T72 · 🔴 CORRECCIÓN a la T70 del cycle 18 — el checksum de bytes de Apache-2.0
  no tiene cuatro valores, tiene al menos SEIS.** Medidos en este pass, todos
  Apache-2.0 legítimos: **10.760** (`prebid/prebid-server`), **11.316**
  (Unleash pre-v8), **11.344** (`OpenAttribution/open-attribution`), **11.352**
  (`open-feature/spec`), **11.356** (`gorse-io/gorse`), **11.357**
  (`pymc-labs/pymc-marketing`). Con seis valores y 600 B de dispersión, para
  Apache-2.0 el checksum **sólo puede levantar la mano, nunca dictaminar**: hay que
  leer la primera línea y la presencia del apéndice. Para MIT (1.066-1.102 B) y
  GPL-3.0 (35.149 B) sigue sirviendo.
- **T73 · La partición del grant tiene un eje más: la RAMA POR DEFECTO.** El cycle 7
  lo vio en `Unity-Technologies/ml-agents`; flipt lo reproduce con **tres** grants
  simultáneos. Lo que decide cuál te toca **no es el repo, es el comando**:
  `git clone`, `docker pull :v1` y `go get .../sdk/go` traen licencias distintas
  del mismo slug.
- **T74 · El repo archivado se queda con las estrellas, y el buscador ordena por
  estrellas.** `hyochan/react-native-iap` tiene **3.219 ★** y `archived=true`; su
  sucesor declarado en la propia descripción, `hyodotdev/openiap`, tiene **171 ★**,
  MIT, vivo (2026-11-21). Cualquier ranking por tracción corona **el cadáver**. Es
  el *runner-up promotion* del cycle 4, pero por **archivo** en vez de por rename, y
  la señal también es distinta: acá el repo viejo responde 200 y se declara muerto
  en su propio texto.
- **T75 · No tomes un hecho de licencia o de ciclo de vida de la comparativa de un
  competidor.** La comparativa de GrowthBook sobre Unleash afirma que *"la versión
  open source se apaga el 31 de diciembre de 2026"*. Lo que tiene esa fecha de EOL
  es **Unleash Edge OSS**, no el servidor. Dos productos, un titular. ⚠️ **No se
  pudo verificar en la fuente primaria**: `getunleash.io` y `docs.getunleash.io`
  **no resuelven desde esta sesión** (ENOTFOUND). Se publica como *claim de
  tercero*, no como hecho.
- **T76 · `NOASSERTION` en esta capa corrió 4 de 5 veces hacia lo PERMISIVO.**
  Byte-read de los cinco: `uber/causalml` = Apache-2.0 (aviso corto de 561 B, por eso
  el clasificador no lo ve), `VowpalWabbit/vowpal_wabbit` = BSD-3-Clause (1.584 B,
  invisible por doble interlineado), `py-why/EconML` = MIT + apéndice BSD-3 de
  scikit-learn (2.978 B), `growthbook/growthbook` = MIT Expat + carve-out. El único
  que corrió hacia lo restringido fue `flipt-io/flipt`. Confirma la T60 del KB de
  enterprise: **el badge ausente no es un veredicto, es la ausencia de uno.**

### 🆕 Tabla nueva · **Plano de decisión: flags, remote config y experimentos**

Todo verificado el 2026-11-22: existencia y ★ por `gh api search/repositories`,
licencia por lectura en bytes de `raw.githubusercontent.com`.

| Agente / herramienta | ★ | Licencia (verificada) | Último push | Para qué sirve en un juego |
|---|---|---|---|---|
| [Unleash/unleash](https://github.com/Unleash/unleash) | 14.337 | ⛔ **AGPL-3.0-or-later** desde v8 (antes Apache-2.0, 2026-05-26) | 2026-11-20 | Gestión de flags madura. **El servidor ya no es adoptable sin análisis**; los SDKs sí. |
| [growthbook/growthbook](https://github.com/growthbook/growthbook) | 8.645 | 🟡 **MIT Expat** + 317 archivos bajo *Enterprise License* | 2026-11-21 | Flags **y** motor de estadística de experimentos (lo que Flagsmith no trae). |
| [Flagsmith/flagsmith](https://github.com/Flagsmith/flagsmith) | 8.533 | ✅ **BSD-3-Clause** (1.556 B, sin carve-out en el árbol) | 2026-11-21 | Flags + *remote config* + segmentos. La más limpia de licencia de las tres grandes. |
| [flipt-io/flipt](https://github.com/flipt-io/flipt) | 4.953 | 🟡 **FCL-1.0-MIT** (v2) · **GPL-3.0** (`main`) · **MIT** (`sdk/go`) | 2026-11-21 | Flags declarativos **versionados en Git**: el flag entra por PR, no por consola. |
| [thomaspoignant/go-feature-flag](https://github.com/thomaspoignant/go-feature-flag) | 2.145 | ✅ **MIT** (1.072 B) | 2026-11-22 | Flags sin base de datos: el estado es un archivo (S3, Git, HTTP). Encaja con un build de juego. |
| [open-feature/spec](https://github.com/open-feature/spec) | 1.299 | ✅ **Apache-2.0** (11.352 B) | 2026-11-21 | **El estándar CNCF de la capa.** Programás contra la API, no contra el vendor. |
| [open-feature/flagd](https://github.com/open-feature/flagd) | 1.029 | ✅ **Apache-2.0** | 2026-11-19 | El evaluador de referencia de OpenFeature: un demonio, un archivo JSON, cero SaaS. |
| [open-feature/go-sdk](https://github.com/open-feature/go-sdk) | 257 | ✅ **Apache-2.0** | 2026-11-22 | SDK Go de OpenFeature (el lenguaje de Nakama y de la mayoría de los backends de juego). |
| [open-feature/dotnet-sdk](https://github.com/open-feature/dotnet-sdk) | 143 | ✅ **Apache-2.0** | 2026-11-22 | SDK .NET: **la vía más corta hacia Unity**, con las salvedades del gap declarado abajo. |
| [open-feature/java-sdk](https://github.com/open-feature/java-sdk) | 131 | ✅ **Apache-2.0** | 2026-11-16 | SDK Java/Kotlin: servidores de juego JVM y Android. |
| [featurevisor/featurevisor](https://github.com/featurevisor/featurevisor) | 816 | ✅ **MIT** (1.102 B) | 2026-11-18 | Flags, experimentos y *remote config* **como artefactos de build**: sin backend que operar. |
| [statsig-io/statsig-server-core](https://github.com/statsig-io/statsig-server-core) | 23 | ✅ **ISC** | 2026-11-10 | Confirma el patrón: **el SDK es abierto, la plataforma no**. Útil como referencia, no como base. |
| [tryabby/abby](https://github.com/tryabby/abby) | 167 | ⛔ **AGPL-3.0** | 2025-07-24 | ⚠️ **16 meses sin push.** Se lista para que no se vuelva a evaluar: copyleft **y** dormido. |

### 🆕 Tabla nueva · **Personalización, uplift y valor del jugador**

| Agente / herramienta | ★ | Licencia (verificada) | Último push | Para qué sirve en un juego |
|---|---|---|---|---|
| [recommenders-team/recommenders](https://github.com/recommenders-team/recommenders) | 22.009 | ✅ **MIT** (1.212 B) | 2026-11-05 | Proyecto de la Linux Foundation: recetas de recomendadores evaluadas de punta a punta. |
| [gorse-io/gorse](https://github.com/gorse-io/gorse) | 9.927 | ✅ **Apache-2.0** (11.356 B) | 2026-11-20 | Motor de recomendación **desplegable**, no una librería: entra como servicio al lado de Nakama. |
| [VowpalWabbit/vowpal_wabbit](https://github.com/VowpalWabbit/vowpal_wabbit) | 8.742 | ✅ **BSD-3-Clause** (1.584 B; badge `NOASSERTION`) | 2026-10-15 | *Contextual bandits* en producción: elegir oferta/dificultad **aprendiendo online**, sin esperar un A/B. |
| [py-why/dowhy](https://github.com/py-why/dowhy) | 8.410 | ✅ **MIT** | 2026-11-17 | Inferencia causal con el supuesto **escrito**: lo que hay que mostrar cuando el cliente pregunta *"¿el evento causó el gasto?"*. |
| [uber/causalml](https://github.com/uber/causalml) | 6.047 | ✅ **Apache-2.0** (aviso corto, 561 B; badge `NOASSERTION`) | 2026-10-12 | *Uplift modeling*: a **qué** jugador le cambia la conducta una oferta, no a cuál le gusta. |
| [py-why/EconML](https://github.com/py-why/EconML) | 4.861 | ✅ **MIT** + apéndice BSD-3 scikit-learn (2.978 B) | 2026-11-20 | Efectos heterogéneos de tratamiento: la base estadística del *uplift* de arriba. |
| [RUCAIBox/RecBole](https://github.com/RUCAIBox/RecBole) | 4.620 | ✅ **MIT** | 2025-02-24 | ⚠️ **21 meses sin push.** Sirve como banco de comparación de algoritmos, no como dependencia viva. |
| [meta-pytorch/torchrec](https://github.com/meta-pytorch/torchrec) | 2.629 | ✅ **BSD-3-Clause** (1.530 B) | 2026-11-22 | Recomendadores a escala de catálogo grande. ⚠️ **Renombrado**: `pytorch/torchrec` → 422. |
| [facebookexperimental/Robyn](https://github.com/facebookexperimental/Robyn) | 1.539 | ✅ **MIT** | 2026-11-10 | *Marketing mix modeling*: repartir presupuesto de UA **sin** identificadores por usuario (post-ATT). |
| [pymc-labs/pymc-marketing](https://github.com/pymc-labs/pymc-marketing) | 1.314 | ✅ **Apache-2.0** (11.357 B) | 2026-11-21 | **CLV/LTV bayesiano** (BG/NBD, Gamma-Gamma) + MMM. La pieza que faltaba para el *"churn/LTV = cero"* del cycle 16. |
| [st-tech/zr-obp](https://github.com/st-tech/zr-obp) | 720 | ✅ **Apache-2.0** | 2024-06-03 | ⚠️ **29 meses sin push.** *Off-policy evaluation*: evaluar una política de oferta **con el log**, sin exponer jugadores. Referencia académica, no dependencia. |

### 🆕 Tabla nueva · **Monetización: compra, atribución y publicidad**

| Agente / herramienta | ★ | Licencia (verificada) | Último push | Veredicto |
|---|---|---|---|---|
| [prebid/prebid-server](https://github.com/prebid/prebid-server) | 594 | ✅ **Apache-2.0** (10.760 B) | 2026-11-21 | Subasta de publicidad server-side, **auto-hospedable**: la única pieza de ad-tech de esta KB que un estudio puede operar. |
| [prebid/prebid-mobile-android](https://github.com/prebid/prebid-mobile-android) | 86 | ✅ **Apache-2.0** | 2026-11-20 | El lado cliente del anterior, para juegos Android con publicidad. |
| [hyodotdev/openiap](https://github.com/hyodotdev/openiap) | 171 | ✅ **MIT** (1.066 B) | 2026-11-21 | Protocolo unificado de compras in-app. **Sucesor declarado** de `react-native-iap`. |
| [hyochan/react-native-iap](https://github.com/hyochan/react-native-iap) | 3.219 | **MIT**, `archived=true` | — | ⛔ **Archivado.** Se lista sólo para romper la trampa de la T74: tiene **19×** las ★ de su sucesor vivo. |
| [OpenAttribution/open-attribution](https://github.com/OpenAttribution/open-attribution) | 86 | ✅ **Apache-2.0** (11.344 B) | 2025-09-28 | ⚠️ **~14 meses sin push.** El único MMP open source que devolvió la búsqueda. Existe, está permisivo, y **no está vivo**. |

### 🟢 Control negativo y gaps medidos (la mitad del dato)

- **SDK de engine para esta capa: CERO.** Ninguna de las ocho plataformas
  permisivas publica paquete **Unity** ni **Unreal**. Lo que el mercado ofrece con
  SDK de engine es propietario: **Unity Remote Config / Game Overrides**,
  **GameAnalytics** (SDK C++ abierto, plataforma cerrada) y **Joystick**. El camino
  permisivo existe pero hay que escribirlo: OpenFeature define el protocolo y el
  `dotnet-sdk` es el punto de partida razonable para Unity.
- **MMP / atribución móvil open source: UNO, dormido** (OpenAttribution, arriba).
  No es un cero: es un *"existe y está parado"*, que es un dato distinto y peor de
  ignorar.
- **Mediación de publicidad open source: CERO** en esta búsqueda. Prebid cubre la
  subasta, no la mediación entre redes.
- **Modelo de churn de juego open source: sigue en CERO** (reconfirmado el gap del
  cycle 16). Lo que cambia es que **la herramienta para construirlo** ya está
  identificada y es Apache-2.0 (`pymc-marketing`), así que el gap pasa de *"no hay
  nada"* a *"hay que entrenarlo, con esta base"*.
- **Re-verificación de una fila vieja:** `Countly/countly-server` se releyó hoy y
  sigue exactamente como lo publicó el cycle 16 — AGPL-3.0 con **§7 modificada**
  (anti-*debranding*), `LICENSE` de **1.025 B**. Primera fila de esta KB verificada
  dos veces por dos canales distintos **sin derivar**.
- **Hosts que no resuelven desde esta sesión** (ENOTFOUND; se suman a la lista
  muerta por política): `getunleash.io`, `docs.getunleash.io`, `bain.com`. Para
  Unleash la sustitución fue **completa** (el repo es la fuente de verdad: historia,
  `package.json` y tags); para Bain **no hay sustituto** y las cifras se publican
  marcadas como fuente secundaria en `intel/market.md`.


## 🔴 Lo más importante del pass 2026-11-22 (cycle 18): **el canal 6 — el árbol completo**, y el grant no comercial que ningún canal anterior podía ver

Los cycles 1-17 construyeron cinco canales de licencia. Los cinco comparten un
supuesto que nunca se escribió: **que la ruta del archivo de licencia se conoce**.
El badge devuelve un veredicto por repo; el byte-read necesita una ruta; la
enumeración por sonda prueba las rutas que **se le ocurren** al que la escribe; el
README nombra lo que el proyecto quiso nombrar; `git log --follow` sigue un archivo
**que ya tenés**. Ninguno contesta *¿qué archivos de licencia existen?*

### El canal 6

```bash
git clone --filter=blob:none --no-checkout --depth 1 https://github.com/<slug> <dir>
git -C <dir> ls-tree -r --name-only HEAD
git -C <dir> cat-file -p HEAD:<ruta>
```

El clon *blobless* trae el árbol y **cero blobs**; los textos se piden de a uno. En
este pass: **41 repos, 0 fallos**, **329+** archivos de licencia enumerados.

🟢 **Cierra un gap que `intel/trends.md` declaró durante 5 passes** (*"listado de
directorios de un repo: sigue sin canal"*). No era cierto que no hubiera canal: la
API de GitHub está cerrada (`gh api repos/<slug>/contents` → **403** fuera de los
repos de la sesión), pero **git no**.

### 🔴 Hallazgo nº1: `SkyworkAI/Matrix-Game` publica **28** archivos de licencia, esta KB tenía **7**, y el nº 28 es **CC BY-NC 4.0**

| | Archivos de licencia | Cómo se encontraron |
|---|---|---|
| cycle 12 | 2 | ruta adivinada |
| cycle 13 | 7 | sonda sistemática — y se declaró *"no puedo saber si hay más"* |
| 🆕 **cycle 18** | **28** | **enumeración del árbol** |

Los 7 publicados son **correctos**. Los 21 restantes viven bajo `third_party/`, a
hasta **seis niveles** de profundidad, y **ninguna sonda los podía alcanzar**.

**El que decide una propuesta:**

> `Matrix-Game-1/GameWorldScore/GameWorld/third_party/amt/LICENSE` — **18.208 B** —
> **Creative Commons Attribution-NonCommercial 4.0 International**
> (8 ocurrencias del literal `NonCommercial`).

`amt` es *All-pairs Multi-field Transforms*, interpolación de frames. **No está en
el modelo.** Está en **`GameWorldScore`**, el banco de pruebas con el que
Matrix-Game demuestra que el modelo funciona.

⇒ **Consecuencia comercial, que es nueva y es precisa:** el **modelo** de
Matrix-Game sigue siendo embarcarle (raíz MIT 1.118 B; `Matrix-Game-1/3/4`
Apache-2.0). Lo que **no** se puede hacer dentro de un entregable pago es **correr
su propio benchmark**. Un PoC que reporte GameWorldScore al cliente está usando un
componente `NonCommercial`. Ver **Receta 47**.

**Y lo que sólo el árbol permite afirmar:** `Matrix-Game-2/` tiene **88 archivos y
ninguna licencia propia** ⇒ cae bajo el MIT de la raíz. Antes era indecidible.

#### El resto del árbol de Matrix-Game, por capa

| Capa | Rutas | Grants |
|---|---|---|
| Producto | `LICENSE` (1.118 MIT) · `Matrix-Game-1/GameWorldScore/LICENSE` (11.356) · `Matrix-Game-3/LICENSE.txt` (11.358) · `Matrix-Game-4/LICENSE` (11.357) + `NOTICE` | MIT + Apache-2.0 ×3 |
| Componentes propios | `Matrix-Game-4/licenses/MG4CF-LICENSE` (11.357) · `TAEHV-LICENSE` (1.073) · `matrix_game/web/mediamtx-reader.LICENSE` (1.062) | Apache-2.0 + MIT ×2 |
| ⛔ **Benchmark** | `.../third_party/amt/LICENSE` (**18.208**) | ⛔ **CC BY-NC 4.0** |
| Benchmark (resto) | `DROID-SLAM` (1.539) · `RAFT` (1.512) · `lietorch` (1.512) · `tartanair_tools` (1.496, CMU) | BSD-3-Clause ×3 + CMU |
| Benchmark → Eigen | `COPYING.MPL2` (16.726, **el grant real**) · `COPYING.GPL` (35.147, **GPL-3.0**) · `COPYING.LGPL` (26.530, **LGPL-2.1**) · `bench/btl/COPYING` (18.109, **GPL-2.0**) · `COPYING.BSD` · `COPYING.APACHE` · `COPYING.MINPACK` | MPL-2.0 + textos legacy |
| Runtime del juego | `DirectXMath/LICENSE` (1.141) + `SAL-LICENSE.txt` (1.116) · `JoltPhysics` (1.059) · `recastnavigation/License.txt` (877) · `stb` (2.510) | MIT ×3 + **zlib** + dual PD/MIT |

⚠️ **`recastnavigation` aparece dos veces en esta KB**: como fila propia (8.205 ★,
zlib) y **vendorizado dentro de Matrix-Game-4**. Es el mismo código con el mismo
grant, pero el aviso de atribución hay que ponerlo **una vez por copia embarcada**.

### 🆕 T68 · Un filtro anclado al **principio** del nombre no ve los grants con sufijo

Mismo árbol, mismo pass, dos filtros de `git ls-tree`:

| Filtro sobre el basename | Encontrados |
|---|---|
| **empieza con** `licen[cs]e\|copying\|notice` | 23 |
| **contiene** `licen[cs]e\|copying\|notice` | **28** |

Los 5 que se pierden son `MG4CF-LICENSE`, `TAEHV-LICENSE`, `SAL-LICENSE.txt`,
`mediamtx-reader.LICENSE` y `THIRD_PARTY_NOTICES.md` — es decir, **exactamente la
forma que usa un proveedor para licenciar un componente suelto**, y tres de ellos
son los que esta KB ya había encontrado a mano. **Un canal nuevo con un filtro malo
pierde lo que el canal viejo ya tenía.** Se escribe con `contains`, siempre.

### 🆕 T69 · Lo no comercial no entra por el modelo: entra por el **banco de pruebas**

41 repos, 329+ grants, **un solo** `NonCommercial` — y está seis niveles adentro de
un benchmark. Generaliza un patrón que esta KB ya tenía en dos formas (pesos
`CC BY-NC` en `audiocraft` y `YuE`; `minerl` entero `CC-BY-NC-SA`): **el componente
restringido nunca es el titular**, porque el titular es el que mira el equipo legal.
El orden de auditoría correcto es **al revés**: primero `third_party/`, `vendor/`,
`external/`, `benchmark/`, `eval/`; el grant de la raíz al final.

### 🆕 T70 · Apache-2.0 es íntegro en **cuatro** byte-counts; el discriminador es la línea, no el byte

| Repo | Archivo | Bytes | Líneas | `APPENDIX` | Veredicto |
|---|---|---|---|---|---|
| [AccessKit/accesskit](https://github.com/AccessKit/accesskit) | `LICENSE-APACHE` | 9.723 | — | ❌ | íntegro (cycle 16) |
| 🆕 [IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP) | `LICENSE` | **10.197** | **176** | ❌ | **íntegro** |
| 🆕 [contentauth/c2pa-rs](https://github.com/contentauth/c2pa-rs) | `LICENSE-APACHE` | **11.335** | **201** | ✅ | **íntegro** |
| 🆕 [o3de/o3de](https://github.com/o3de/o3de) | `LICENSE_APACHE2.TXT` | **11.557** | **201** | ✅ | **íntegro** |
| (testigo histórico de esta KB) | — | 11.357-11.362 | 201 | ✅ | íntegro |

Los cuatro se leyeron buscando cláusulas añadidas (`additional`, `notwithstanding`,
`shall not be used`, `trademark of`, `amend`): **ninguno adulterado**. 11.335 vs
11.557 con **las mismas 201 líneas** es relleno del placeholder
`[name of copyright owner]` y espacios finales.

⇒ **Reemplazo operativo del checksum para Apache-2.0:** `wc -l` (**201** con
apéndice, **~176** sin) **+** presencia del literal `END OF TERMS AND CONDITIONS`.
El byte-count queda como **triage**, nunca como veredicto.

### 🟢 Control negativo medido (la mitad del dato)

| Medición | Resultado |
|---|---|
| Repos enumerados por canal 6 | **41** · 0 fallos de clonado |
| Archivos de licencia leídos en bytes | **329+** |
| Con grant **no comercial** en cualquier punto del árbol | **1** · Matrix-Game |
| Con copyleft **fuerte en la raíz** | **1** · ALE (GPL-2.0, ya publicado) |
| Con copyleft **débil vendorizado** | **1** · agones (`vendor/hashicorp/*`, MPL-2.0 por archivo ⇒ benigno) |
| Grants vendorizados, top | agones **149** · nakama **81** · Matrix-Game **28** · pinus **14** |

⇒ **El stack permisivo de las recetas de esta KB está limpio a nivel de árbol**, y
eso pasó de supuesto a **medido**.

---

## 🔴 Lo más importante del pass 2026-11-22 (cycle 17): **la FÁBRICA — el carril por donde viaja el trabajo entre que el asset existe y el build sale — y el canal 5: la HISTORIA del archivo de licencia**

El cycle 14 midió **qué se genera**, el 13 **quién habla**, el 11 y el 12 **qué
corre en el servidor**, el 15 **qué toca el jugador**, el 16 **qué se mide**, y la
Receta 19 ya tiene **quién lo juega** (`ramenvr/autopilot`, Apache-2.0). Falta el
carril: **versionado de binarios, CI de build, caché de compilación y seguimiento
de producción.** Inventario plano (regla T64) corrido sobre los 9 archivos de esta
KB antes de buscar nada:

| Término | Ocurrencias en la KB antes de este pass |
|---|---|
| `AYON` / `OpenPype` / `Kitsu` / `CGWire` | 0 |
| `game-ci` / `GameCI` | 0 |
| `AltTester` / `Airtest` | 0 |
| `git-lfs` / `Perforce` / `version control` | 0 |
| `sccache` / caché de compilación | 0 |
| `treeverse` / `DVC` | 0 |

**Seis categorías, cero filas en 16 passes.** No es una capa floja: es la capa que
el cliente ya tiene montada y que decide si lo que esta KB propone se puede
integrar o hay que reescribirlo.

Y el resultado no es un catálogo: es que **en esta capa la licencia no es un
atributo del repo, es una función del TIEMPO y del DUEÑO**, y los cuatro canales
de licencia de esta KB —badge, archivo, enumeración, prosa del README— **sólo ven
el presente**.

---

### 🔴 Hallazgo nº1: AYON es **Apache-2.0 en el cliente y FSL-1.1 en el servidor** — y el relicenciamiento tiene fecha, commit y una última versión permisiva recuperable

AYON (Ynput s.r.o., Chequia) es el sucesor de OpenPype y la plataforma de pipeline
de producción más usada fuera de ShotGrid/ftrack. Toda la prensa dice *"open
source, Apache 2.0"*. La medición dice otra cosa:

| Componente | ★ | SPDX del API | Archivo leído | Qué es |
|---|---|---|---|---|
| [`ynput/ayon-backend`](https://github.com/ynput/ayon-backend) | 40 | `NOASSERTION` | **FSL-1.1-ALv2, 3.744 B** | el servidor que se despliega |
| [`ynput/ayon-frontend`](https://github.com/ynput/ayon-frontend) | 31 | `NOASSERTION` | **FSL-1.1-ALv2, 3.744 B** | la web del servidor |
| [`ynput/ayon-core`](https://github.com/ynput/ayon-core) | 79 | `Apache-2.0` | 11.357 B ✅ | integración con DCCs |
| [`ynput/ayon-launcher`](https://github.com/ynput/ayon-launcher) | 55 | `Apache-2.0` | 11.357 B ✅ | app de escritorio |
| [`ynput/ayon-docker`](https://github.com/ynput/ayon-docker) | 60 | `Apache-2.0` | 11.357 B ✅ | el compose que **despliega la imagen FSL** |
| [`ynput/ayon-python-api`](https://github.com/ynput/ayon-python-api) | 17 | `Apache-2.0` | 11.357 B ✅ | cliente REST |

**El corte es exacto: el servidor es FSL, el cliente es Apache.** Y el `NOASSERTION`
del API no es ruido — es el clasificador diciendo *"esto no es una licencia SPDX
conocida"*, que es la señal T60 leída en la dirección restrictiva.

**Qué prohíbe la FSL-1.1-ALv2, textual:** se puede usar, copiar, modificar y
redistribuir para cualquier *Permitted Purpose*, que es cualquiera salvo un
*Competing Use* — *"making the Software available to others in a commercial
product or service that: 1. substitutes for the Software; 2. substitutes for any
other product or service we offer using the Software…; or 3. offers the same or
substantially similar functionality"*.

🟢 **Y la cláusula que decide si Globant puede tocarlo** — está escrita y es
favorable:

> *"Permitted Purposes specifically include using the Software: 1. for your
> internal use and access; … 4. **in connection with professional services that
> you provide to a licensee using the Software in accordance with these Terms and
> Conditions.**"*

⇒ **Operar, configurar, extender y automatizar el AYON del cliente es un Permitted
Purpose explícito.** Lo prohibido es lo que Globant no iba a hacer: ofrecer *"AYON
gestionado por nosotros"* como producto. **La FSL no bloquea servicios; bloquea
competencia de producto.** Esto hay que decirlo en la propuesta, porque el área
legal del cliente va a ver "no es open source" y frenar.

🔵 **El Change Date, medido** (la Receta 23 de esta KB ya tiene el gate de BSL;
la FSL es su hermana y entra por el mismo gate):

> *"We hereby irrevocably grant you an additional license to use the Software under
> the Apache License, Version 2.0 that is effective on the **second anniversary of
> the date we make the Software available**."*

| Fecha | Evento | Consecuencia |
|---|---|---|
| 2022-02-28 | `chore: Added license file` (Apache-2.0) | el servidor nació permisivo |
| **2025-01-09** | release **1.6.1** | **última versión Apache-2.0 para siempre** |
| **2025-01-27** | commit **`97627e27` — *"License change from Apache 2.0 to FSL-1.1-ALv2"*** | el relicenciamiento, con fecha |
| 2025-01-30 | release **1.7.0** | primera versión FSL · **se vuelve Apache-2.0 el 2027-01-30** |
| 2026-11-19 | release 1.17.7 | FSL hasta 2028-11-19 |

⇒ Un cliente que necesite grant permisivo **hoy** sobre el servidor tiene exactamente
dos salidas medidas: **congelar en 1.6.1** (y perder 11 minors) o **esperar al
2027-01-30** para que 1.7.0 convierta. No hay una tercera.

⚠️ **Por qué ninguna búsqueda lo encuentra:** la prensa que define el consenso
(cgchannel 2023 y 2024, animationmagazine 2023, 80.lv 2023) es **anterior al
2025-01-27 y era correcta cuando se escribió**. Esta KB tenía la regla *"la
documentación que viaja con el proyecto se escribe una vez y no se revisa"*; este
caso es más incómodo: **la fuente no se equivocó, el objeto cambió debajo.**

---

### 🔵 Hallazgo nº2 (método, y el que hay que copiar a toda la familia de KBs): **canal 5 — `git log --follow -- LICENSE`**

Los cuatro canales de esta KB leen el presente: el badge de hoy, el archivo de hoy,
el árbol de hoy, el README de hoy. **Ninguno puede ver un relicenciamiento, y un
relicenciamiento no deja rastro en el archivo resultante** — el `LICENSE` de
`ayon-backend` leído hoy es un FSL impecable, sin una línea que diga que ayer fue
Apache.

El canal 5 cuesta un clone sin blobs y responde tres preguntas que ningún otro canal
responde: **¿cambió?**, **¿cuándo?**, **¿qué versión publicada sigue siendo
permisiva?**

```bash
git clone -q --filter=blob:none --single-branch https://github.com/<slug> /tmp/x
cd /tmp/x && git log --follow --date=short --pretty='%h %ad %s' -- LICENSE
# y si hay un cambio, la versión anterior se recupera por fecha de tag:
for t in $(git tag --sort=creatordate); do echo "$(git log -1 --date=short --pretty=%ad $t) $t"; done
```

**Corrido sobre 4 repos de esta capa, resultado medido:**

| Repo | Historia del `LICENSE` | Lectura |
|---|---|---|
| `ynput/ayon-backend` | Apache 2022-02-28 → **FSL 2025-01-27** | 🔴 relicenciamiento |
| `treeverse/dvc` | `2025-12-01 update copyright`: *"Copyright 2017-2021 **Iterative, Inc.**"* → *"Copyright 2025 **Treeverse.**"* | 🟡 **cambio de dueño sin cambio de licencia** |
| `cgwire/zou` | un solo commit, `2017-03-26 Initial commit` | 🟢 AGPL-3.0 estable 9 años |
| `AirtestProject/Airtest` | un solo commit, `2018-01-24 opensource init version` | 🟢 Apache-2.0 estable 8 años |

**1 relicenciamiento + 1 cambio de dueño + 2 confirmaciones de estabilidad.** Las
dos confirmaciones valen tanto como los hallazgos: *"Apache desde el commit
inicial, nunca tocado"* es una afirmación que esta KB **no podía hacer** hasta hoy.

---

### 🟡 Hallazgo nº3: DVC cambió de dueño, el buscador devuelve el mismo 422 que para un repo inexistente, y el dueño nuevo publica su propio producto bajo BSL

`repo:iterative/dvc` → **HTTP 422** *"the listed users and repositories cannot be
searched either because the resources do not exist or you do not have permission"*
— **idéntico al control negativo** que esta página documenta para
`repo:gmilano/control-does-not-exist-xyz-999`. Pero
`raw.githubusercontent.com/iterative/dvc/HEAD/LICENSE` devuelve **200 y 11.340 B de
Apache-2.0**, y el README resuelve.

Resolución por búsqueda de nombre (no por sonda de slug):

```bash
gh api -X GET search/repositories -f q='dvc in:name stars:>3000'
# → treeverse/dvc | ★15.993 | Apache-2.0 | pushed=2026-11-20
```

> **Regla T66 — el 422 del buscador no distingue "no existe" de "se mudó".** El
> control negativo del cycle 16 sigue siendo válido, pero **no es suficiente**: ante
> un 422 hay que correr una búsqueda **por nombre** antes de escribir "ausente". Un
> repo de **15.993 ★** se estaba leyendo como inexistente.

🟡 **Y el dato que sí importa para una propuesta:** el dueño nuevo, Treeverse,
publica [`treeverse/lakeFS`](https://github.com/treeverse/lakeFS) (5.590★) bajo
**Business Source License 1.1 — 7.475 B medidos**, valor nuevo para la tabla de
checksum de esta KB. El grant Apache-2.0 de las versiones ya publicadas de DVC es
irrevocable y no se toca; **lo que cambia es quién decide la licencia de la próxima
versión, y ese dueño ya eligió BSL para su producto insignia.** Es exactamente el
antecedente que la Receta 23 pide vigilar, y la misma forma del caso AYON: *una
compañía con modelo source-available sosteniendo un repo permisivo.*

---

### 🔴 Hallazgo nº4: en `game-ci`, el repo que recomienda toda guía lleva **44 meses sin un commit**, y los vivos tienen menos estrellas

| Repo | ★ | Licencia (bytes) | Último push | Estado |
|---|---|---|---|---|
| [`game-ci/unity-actions`](https://github.com/game-ci/unity-actions) | **1.097** | MIT, 1.097 B | **2023-03-04** | 🔴 **44 meses muerto** — el que nombran las guías |
| [`game-ci/unity-builder`](https://github.com/game-ci/unity-builder) | 1.113 | MIT, 1.097 B | 2026-11-15 | 🟢 vivo, **el que hay que usar** |
| [`game-ci/docker`](https://github.com/game-ci/docker) | 472 | MIT, 1.071 B | **2026-11-22 (hoy)** | 🟢 vivo |
| [`game-ci/unity-test-runner`](https://github.com/game-ci/unity-test-runner) | 271 | MIT, 1.097 B | 2026-11-15 | 🟢 vivo |

La diferencia de estrellas entre el muerto y el vivo es de **16 ★ sobre 1.100**:
un ranking por estrellas los pone empatados y el nombre genérico (`unity-actions`)
gana la intuición. **Es la "promoción del subcampeón" del cycle 4 en su forma más
barata: no hace falta un rename para que la KB corone un cadáver, basta un nombre
más obvio.**

⚠️ **Y el límite de lo que `game-ci` resuelve:** el código es MIT, pero **Unity
exige una licencia propia en el runner** (se inyecta como secreto). *El grant
permisivo cubre el orquestador, no el motor que orquesta* — misma forma que el
hallazgo de pesos del cycle 14, trasladada a CI.

---

### 🔴 Hallazgo nº5: **el carril de Unreal no tiene CI permisiva, y la de Epic exige Perforce**

- **Horde** es el sistema de build de Epic (el que corre Fortnite). Epic publica la
  fuente de cliente y servidor, pero **dentro del árbol de Unreal Engine y bajo la
  EULA de UE, no bajo una licencia OSS**. Medición coherente: `repo:EpicGames/UnrealEngine`
  → **422**, igual que cualquier repo al que esta sesión no tiene acceso.
- Según la FAQ de Epic, **el CI de Horde no soporta Git ni Subversion: requiere un
  stream de Perforce y UE 5.4+**. ⚠️ *Fuente secundaria: `dev.epicgames.com` no
  resuelve desde esta sesión (ENOTFOUND) — ver la nota de instrumentos.*
- Única integración **permisiva** medida para Unreal:
  [`JetBrains/teamcity-unreal-engine-plugin`](https://github.com/JetBrains/teamcity-unreal-engine-plugin)
  — **Apache-2.0 (11.357 B), 21 ★, push 2026-08-19 (3 meses)** — y requiere
  **TeamCity, que es propietario**.

⇒ **Asimetría con consecuencia económica directa:** un cliente Unity puede montar
la fábrica entera con MIT (`game-ci`) sobre GitHub Actions; un cliente Unreal
necesita **Perforce + Horde bajo EULA** o un CI genérico escrito a mano. *Es la
primera vez que esta KB puede poner costo de pipeline en la conversación de
elección de engine.*

---

### 🔴 Hallazgo nº6: la arquitectura de referencia de **AWS** para QA con agentes embebe un SDK **GPL-3.0** en el build del cliente — y encima pide suscripción de pago

El blog de AWS for Games (*Building an AI game testing agent with Amazon Bedrock*)
propone: AltTester Server + agente QA en contenedores sobre ECS/Fargate, estado de
la escena de Unity expuesto por **AltTester SDK**, filtro que baja 1.400 objetos a
~42, Nova resumiendo y Claude eligiendo la acción.

| Pieza | Licencia medida | Consecuencia |
|---|---|---|
| [`alttester/AltTester-Unity-SDK`](https://github.com/alttester/AltTester-Unity-SDK) | **GPL-3.0 — 35.043 B**, `Copyright (C) 2025 Altom Consulting` | ⛔ **se instrumenta dentro del build**: si ese build se distribuye, el copyleft alcanza al juego |
| AltTester Server en *batchmode* | **requiere suscripción Pro** (lo dice el propio AWS) | ⛔ no es gratis ni open |
| El repo de referencia de AWS | **no publicado** (anunciado como "planean publicarlo") | ⛔ no hay artefacto que auditar |

⇒ **La arquitectura del hiperescalador es, en las tres piezas que la definen, menos
usable que la alternativa comunitaria que esta KB ya tiene**: la Receta 19 corre
sobre `ramenvr/autopilot` (**Apache-2.0**, 881★, UE 5.4-5.8 / Unity 6 / Godot 4 /
Roblox Studio). Dicho sin adorno: **un blog de AWS no es una auditoría de licencias,
y acá la diferencia es GPL-3.0 dentro del build del cliente.**

---

### 🆕 Filas nuevas de este pass (cycle 17) — automatización de QA y fábrica, todas con licencia leída byte a byte

| Agente / Herramienta | Repo | ★ | Licencia (verificada) | Último push | Para qué sirve acá |
|---|---|---|---|---|---|
| **Airtest** | [AirtestProject/Airtest](https://github.com/AirtestProject/Airtest) | 9.657 | **Apache-2.0** (11.356 B) | 2026-03-23 (8 meses) | automatización de UI de juegos por **reconocimiento de imagen**, Android/iOS/Windows; la única base permisiva a escala para QA de juego móvil. Origen **APAC (NetEase)** |
| **Poco** | [AirtestProject/Poco](https://github.com/AirtestProject/Poco) | 1.964 | **Apache-2.0** (11.356 B) | 2024-01-08 (**34 meses**) | inspección de jerarquía de UI cross-engine (Unity/Cocos/UE). ⚠️ **dormido**: usable como librería, no como dependencia viva |
| **sccache** | [mozilla/sccache](https://github.com/mozilla/sccache) | 7.968 | **Apache-2.0** (11.358 B) | **2026-11-22 (hoy)** | caché de compilación distribuida (C/C++/Rust) — la pieza que baja el costo del build de Unreal sin tocar la EULA |
| **Git LFS** | [git-lfs/git-lfs](https://github.com/git-lfs/git-lfs) | 14.610 | **MIT + BSD-3 embebida** (2.818 B) — API dice `NOASSERTION` | 2026-11-06 | versionado de binarios sobre Git; explica el `NOASSERTION` por **composición**, no por ausencia |
| **DVC** | [treeverse/dvc](https://github.com/treeverse/dvc) | 15.993 | **Apache-2.0** (11.340 B) | 2026-11-20 | versionado de assets grandes con remotos S3/GCS; **ojo al dueño nuevo** (hallazgo nº3) |
| **GameCI (unity-builder)** | [game-ci/unity-builder](https://github.com/game-ci/unity-builder) | 1.113 | **MIT** (1.097 B) | 2026-11-15 | build de Unity en CI, multiplataforma |
| **TeamCity UE plugin** | [JetBrains/teamcity-unreal-engine-plugin](https://github.com/JetBrains/teamcity-unreal-engine-plugin) | 21 | **Apache-2.0** (11.357 B) | 2026-08-19 | la única integración permisiva medida para CI de Unreal |

### ⛔ Rechazos **medidos** de este pass — se publican porque el silencio se lee como cobertura

| Candidato | ★ | Qué se midió | Por qué queda afuera |
|---|---|---|---|
| [`alttester/AltTester-Unity-SDK`](https://github.com/alttester/AltTester-Unity-SDK) | 128 | **GPL-3.0, 35.043 B** | copyleft **dentro del build** distribuible; el server en batch pide Pro |
| [`cgwire/kitsu`](https://github.com/cgwire/kitsu) + [`cgwire/zou`](https://github.com/cgwire/zou) | 774 + 202 | **AGPL-3.0, 34.520 B** (ambos), vivos (push 2026-11-20) | AGPL sobre un servicio accesible por red: fuera de la ruta permisiva. **Adoptable tal cual, no derivable** |
| [`treeverse/lakeFS`](https://github.com/treeverse/lakeFS) | 5.590 | **BSL-1.1, 7.475 B** | Change Date; entra por el gate de la Receta 23 |
| [`ynput/OpenPype`](https://github.com/ynput/OpenPype) | 301 | MIT pero **`archived=true`, último push 2024-09-18 (26 meses)** | **archivado y aun así recomendado** por páginas de terceros como si fuera el camino vigente; el sucesor es AYON (hallazgo nº1) |
| [`team-zomsa/aplib.net`](https://github.com/team-zomsa/aplib.net) | 9 | API: `NOASSERTION` · archivo: **AGPL-3.0 por referencia short-form, 709 B** (Utrecht University), push 2024-06-26 (**29 meses**) | AGPL + dormido. 🔎 **5ª instancia medida del falso negativo del short-form**: el `NOASSERTION` no era ausencia de licencia, era una licencia de 709 B que el clasificador no lee |
| Repo de referencia de QA de AWS | — | no existe públicamente | anunciado, no publicado: **no hay artefacto que auditar** |

### 📏 Lo que este pass NO puede medir, dicho explícitamente

1. **Tres hosts primarios no resuelven desde esta sesión** (`ENOTFOUND`):
   `dev.epicgames.com`, `aws.amazon.com`, `www.trade.gov` y `www.planalto.gov.br`.
   Los hallazgos nº5 y nº6 y el dato de Brasil de `intel/market.md` quedan con
   **fuente secundaria declarada**, no con lectura primaria. Agregar a la lista de
   hosts muertos por política de la familia de KBs.
2. **`README.md` no sirve como sonda de existencia:** de 23 slugs verificados,
   **3 devolvieron 404** (`cgwire/zou`, `treeverse/dvc`, `AirtestProject/Poco`)
   **y los tres existen** — tienen `README.rst` u otro nombre. La verificación de
   URL de esta KB se hace contra `HEAD/LICENSE` + fila del buscador, **nunca contra
   `README.md` y nunca contra `github.com` (403 del proxy para todo)**.
3. **No se midió la cuota de mercado** de Perforce vs Git en estudios ni el costo
   real de un runner de Unity con licencia: no hay fuente abierta verificable.
4. **AYON y Kitsu no se probaron sobre un proyecto de juego real** — la afirmación
   de que Kitsu sirve a estudios de videojuego es **del fabricante**, no medida.

---

## 🔴 Lo más importante del pass 2026-11-22 (cycle 16): **el plano de datos del jugador — la primera capa de esta KB donde el proveedor líder prohíbe la producción con todas las letras, y la continuación permisiva es un fork, no un competidor**

Esta KB llevaba 15 passes y 285 slugs sin una sola fila de **telemetría, analítica
de jugador, crash reporting o live-ops**. No era una capa floja: era una capa
ausente. El inventario plano (regla T64) lo midió antes de buscar nada: de 20
incumbentes del plano de datos contrastados contra los 285 slugs de la KB,
**19 estaban ausentes y sólo `grafana/grafana` aparecía**, y aparecía sin fila
propia.

Lo que la capa devolvió no es un catálogo de repos: es **una categoría entera que
se fue del open source entre 2024 y 2026**, y una lección de método que casi
produce un cero falso en esta misma página.

---

### 🔴 Hallazgo nº1: `snowplow/snowplow` tiene 7.051 ★ y badge **Apache-2.0** — y **no es el pipeline**. Los dos componentes que se despliegan prohíben la producción

El gradiente de estrellas es la trampa completa:

| Repo | ★ | Licencia **leída byte a byte** | ¿Se despliega? |
|---|---|---|---|
| [snowplow/snowplow](https://github.com/snowplow/snowplow) | 7.051 | **Apache-2.0** (`LICENSE`, 11.359 B) | ❌ es el repo paraguas / documentación |
| [snowplow/stream-collector](https://github.com/snowplow/stream-collector) | 40 | ⛔ **SLULA 1.1** (`LICENSE.md`) | ✅ sí — es la ingesta |
| [snowplow/enrich](https://github.com/snowplow/enrich) | 31 | ⛔ **SLULA 1.1** (`LICENSE.md`) | ✅ sí — es el enriquecido |
| [snowplow/snowplow-javascript-tracker](https://github.com/snowplow/snowplow-javascript-tracker) | 593 | **BSD-3-Clause** (`LICENSE`, 1.546 B) | ✅ sí — es el SDK de cliente |

**Quien mide "Snowplow" por estrellas y badge concluye Apache-2.0 y se equivoca
exactamente en los dos componentes que iba a instalar.** El badge del repo de
7.051 ★ es correcto y es irrelevante.

Y la restricción es mucho más dura de lo que dice el resumen habitual
("no podés montar un SaaS competidor"). Texto **leído del archivo**, §1.1 de la
Snowplow Limited Use License Agreement, versión 1.1:

> *"No right to distribute or make available the Software is granted under this
> License. Licensee is not granted the right to, and Licensee shall not, exercise
> the License for any Competing Use, and Licensee **may exercise the License only
> for Non-Production Use or Non-Commercial Use**."*

Y §1.2.2 define el término:

> *"**'Non-Production Use'** means any use of the Software to process **test or
> synthetic data** to evaluate the sufficiency of the Software for use by
> Licensee."*

Es decir: para un estudio comercial, **`stream-collector` y `enrich` no se pueden
correr en producción en absoluto**, ni siquiera para uso interno que no compita
con Snowplow. La licencia permite evaluarlos con datos sintéticos. Nada más.

**Fecha de la relicencia:** el core pasó a SLULA el **8 de enero de 2024** (v1.0).
El archivo del repo dice literalmente *"Version 1.1, November, 2024"*; el blog del
vendor fecha la 1.1 en **diciembre de 2024**. ⚠️ **Se cita el archivo, no el blog**
— es la regla de manifiesto-antes-que-anuncio de esta KB aplicada a una fecha.

**Un eje de partición del grant que esta KB no tenía: cliente contra servidor.**
El tracker de JavaScript sigue siendo BSD-3-Clause mientras el pipeline es SLULA.
La licencia no parte el producto por edición ni por directorio: lo parte **por el
lado de la red en que corre el binario**. Si tu arquitectura cruza esa línea, tu
licencia también.

---

### 🔵 Hallazgo nº2 (método, y el más importante del pass): **un slug adivinado que falla NO mide ausencia** — estuve a punto de publicar un cero falso

La continuación permisiva de Snowplow existe y está viva. El camino para
encontrarla casi la pierde:

- `gh api search/repositories -f q='repo:opensnowcat/enrich'` → **422 Validation Failed**
- `git ls-remote https://github.com/opensnowcat/enrich` → **NOT_FOUND**
- `git ls-remote https://github.com/opensnowcat/stream-collector` → **NOT_FOUND**

Tres sondas, tres negativos, y **el proyecto estaba ahí todo el tiempo**: la
convención de nombres no es `opensnowcat/<componente>` sino
**`opensnowcat/opensnowcat-<componente>`**. Una búsqueda por palabra clave de la
organización lo devolvió entero en una sola llamada.

> **Regla nueva (T65): antes de declarar un cero, buscá la ORGANIZACIÓN por
> palabra clave, no el repo por slug adivinado.** Un 422 o un `NOT_FOUND` sobre un
> slug que vos inventaste mide tu suposición sobre el nombre, no la existencia del
> proyecto. Es la misma forma que el descubrimiento de `complyctl-provider-*` por
> convención de nombres en la KB enterprise, y el complemento del hallazgo del
> cycle 10 (`422` no distingue borrado de renombrado): aquí no hubo borrado ni
> renombrado — **el slug nunca fue correcto**.

**OpenSnowcat, medido:**

| Repo | ★ | Licencia **byte a byte** | Último push |
|---|---|---|---|
| [opensnowcat/opensnowcat-collector](https://github.com/opensnowcat/opensnowcat-collector) | 81 | **Apache-2.0** (`LICENSE.txt`, 11.358 B) | 2026-11-13 |
| [opensnowcat/opensnowcat-enrich](https://github.com/opensnowcat/opensnowcat-enrich) | 40 | **Apache-2.0** (`LICENSE.txt`, 11.359 B) | 2026-11-13 |
| [opensnowcat/opensnowcat-devkit](https://github.com/opensnowcat/opensnowcat-devkit) | 14 | Apache-2.0 (badge) | 2026-10-01 |
| [opensnowcat/opensnowcat-rdb-loader](https://github.com/opensnowcat/opensnowcat-rdb-loader) | 7 | Apache-2.0 (badge) | 2025-08-08 |
| [opensnowcat/opensnowcat-iglu-server](https://github.com/opensnowcat/opensnowcat-iglu-server) | 9 | Apache-2.0 (badge) | ⚠️ 2024-08-11 |

Es un fork declarado del Snowplow anterior a la relicencia, y los dos componentes
que importan recibieron commits **hace nueve días**. El registro de esquemas
(`iglu-server`) lleva **más de dos años dormido**: se usa, se fija la versión y se
asume el mantenimiento — está dicho en la Receta 41, no escondido.

---

### 🔴 Hallazgo nº3: la **única** plataforma de analítica hecha a propósito para juegos es Apache-2.0, está completa… y lleva dos años y medio sin un commit

`GameTuner` son **diez repos**, todos con badge Apache-2.0, y cubren el vertical
entero: tracker de Unity, collector, enricher, loader a BigQuery, ETL, motor de
consultas, servicio de metadatos, Terraform para GCP y un sink de eventos malos.
Su propio README se describe como *"a data platform, specialized in mobile gaming,
built on top of Snowplow Open Source data pipeline"*.

**Y las diez tienen 0 ★ y su último push es de febrero–abril de 2024.**

Licencias leídas byte a byte: `GameTuner/collector` 11.358 B, `GameTuner/enricher`
11.359 B, `GameTuner/unity-tracker` 11.325 B — las tres, texto completo de
Apache-2.0.

Procedencia **medida, no supuesta**: el README del collector dice *"is build upon
Snowplow Scala Stream Collector"*; el unity-tracker es un fork del tracker de Unity
de Snowplow v0.5.0 y su línea de copyright es **AlgebraAI, 2022–2024** — **no es de
Nordeus**, que es la atribución que circula. Sólo soporta GCP/pubsub. Sus propios
autores escribieron en 2024 que el proyecto era *"muy joven, usado en un estudio"*.

> ⚠️ **Límite declarado, y hay que leerlo antes de forkear:** **no medí si el
> snapshot de Snowplow vendorizado dentro de GameTuner es anterior al 8-ene-2024**,
> que es la fecha de la relicencia. Si fuera posterior, el `LICENSE` Apache-2.0 del
> repo es **la afirmación de GameTuner**, no necesariamente una sublicencia válida
> del código de Snowplow que lleva dentro. Es verificable commit a commit y **no lo
> hice en este pass**. Usarlo como referencia de esquema es seguro; usarlo como base
> de fork exige esa verificación primero.

---

### 🔴 Hallazgo nº4: la categoría se fue del open source **en bloque**, y el carve-out por directorio ya es la plantilla de la casa

Ocho incumbentes del plano de datos, licencia leída byte a byte. **Seis no son
permisivos:**

| Repo | ★ | Licencia **byte a byte** | Veredicto |
|---|---|---|---|
| [getsentry/sentry](https://github.com/getsentry/sentry) | 45.850 | ⛔ **FSL-1.1-Apache-2.0** (`LICENSE.md`, 3.785 B) | Apache-2.0 **sólo al segundo aniversario de cada versión** |
| [Countly/countly-server](https://github.com/Countly/countly-server) | 5.932 | ⛔ **AGPL-3.0 con §7 modificada** (`LICENSE`, 1.025 B) | *"Countly Lite License"*: AGPL **más** prohibición de quitar el logo |
| [metabase/metabase](https://github.com/metabase/metabase) | 50.470 | ⛔ **AGPL-3.0 + Metabase Commercial** (`LICENSE.txt`, 855 B) | carve-out en `enterprise/` |
| [openreplay/openreplay](https://github.com/openreplay/openreplay) | 13.170 | ⛔ **multi-licencia + propietaria** (`LICENSE`, 36.511 B) | carve-out en `ee/` |
| [grafana/grafana](https://github.com/grafana/grafana) | 78.033 | ⛔ **AGPL-3.0** (`LICENSE`) | copyleft de red |
| [rybbit-io/rybbit](https://github.com/rybbit-io/rybbit) | 13.349 | ⛔ **AGPL-3.0** (badge) | copyleft de red |
| [ClickHouse/ClickHouse](https://github.com/ClickHouse/ClickHouse) | 51.060 | ✅ **Apache-2.0** (© 2016-2026 ClickHouse, Inc.) | permisiva |
| [apache/superset](https://github.com/apache/superset) | 75.615 | ✅ **Apache-2.0** | permisiva |

Las cuatro observaciones que salen de esa tabla:

1. **Sentry es el caso más caro de equivocarse** porque todo el mundo lo llama
   open source y el propio archivo explica por qué no lo es hoy: el grant Apache-2.0
   *"is effective on the second anniversary"* de cada versión. La versión que querés
   desplegar es, por construcción, la que todavía no convirtió.
2. **Countly invierte el sentido del `NOASSERTION`** en la dirección mala: el badge
   no dice nada y la lectura byte a byte devuelve AGPL **endurecida**, no ablandada.
   La regla T60 funciona en los dos sentidos y este pass la ejerció en el restrictivo.
3. **Tercer caso del carve-out por directorio**: PostHog `ee/`, Metabase
   `enterprise/`, OpenReplay `ee/`. Con tres instancias de distintos vendors
   **ya no es una peculiaridad: es la plantilla de la categoría.** El reflejo correcto
   al ver cualquier analítica "open source" es `ls` del árbol buscando `ee/` o
   `enterprise/` **antes** de leer el `LICENSE` raíz.
4. **Un puntero de licencia roto, 2º caso en esta KB** (el 1º fue el `LICENSE` que
   era un fragmento de diff, cycle 8): el `LICENSE` raíz de OpenReplay manda a
   `ee/LICENSE`, que devuelve **404 en `main` y en `dev`**. El archivo real es
   `ee/LICENSE.md`. No cambia el veredicto, pero sí mide que **el puntero y el
   destino se verifican por separado**.

---

### 🟢 Hallazgo nº5: **GlitchTip es MIT — y esta KB no podía verlo porque mide con instrumentos de GitHub**

El reemplazo permisivo de Sentry existe, es compatible con los **SDK de Sentry**
(mismo protocolo de ingesta) y su licencia es **MIT, 1.067 B, leída byte a byte**.

Vive en **GitLab**. En GitHub, `GlitchTip/GlitchTip` son **3 ★ con último push el
2022-08-21** — un stub abandonado cuya descripción es literalmente *"Readme config"*.

> **Todas las sondas de esta KB (`gh api search/repositories`, `git ls-remote` sobre
> github.com, `raw.githubusercontent.com`) tienen forma de GitHub.** Un proyecto
> alojado en GitLab no aparece como ausente: aparece como **muerto**, que es peor,
> porque un cero invita a buscar más y un cadáver cierra la pregunta. Primer caso
> registrado en esta KB. La sonda que funcionó fue
> `curl https://gitlab.com/<org>/<repo>/-/raw/<branch>/LICENSE`, y funciona en esta
> sesión.

---

### 🆕 Filas nuevas de este pass (cycle 16) — las permisivas, todas con licencia verificada

| Agente / repo | Repo | Licencia (bytes) | ★ | Último push | Qué aporta |
|---|---|---|---|---|---|
| **OpenSnowcat Collector** | [opensnowcat/opensnowcat-collector](https://github.com/opensnowcat/opensnowcat-collector) | **Apache-2.0** (11.358) | 81 | 2026-11-13 | Ingesta de eventos del jugador. **El reemplazo desplegable de `snowplow/stream-collector`**, que la SLULA prohíbe en producción. |
| **OpenSnowcat Enrich** | [opensnowcat/opensnowcat-enrich](https://github.com/opensnowcat/opensnowcat-enrich) | **Apache-2.0** (11.359) | 40 | 2026-11-13 | Enriquecido y validación contra esquema. Mismo motivo. |
| **ClickHouse** | [ClickHouse/ClickHouse](https://github.com/ClickHouse/ClickHouse) | **Apache-2.0** | 51.060 | 2026-11-22 | Almacén de eventos de alta cardinalidad. Es donde se implementa el TTL por jurisdicción de la Receta 42. |
| **ClickStack** | [ClickHouse/ClickStack](https://github.com/ClickHouse/ClickStack) | **MIT** (1.077) | 10.061 | 2026-11-21 | Observabilidad unificada (logs, métricas, trazas, errores) sobre OTel + ClickHouse. ⚠️ El copyright dice **© DeploySentinel, Inc.**: la licencia delata la adquisición, igual que en los derivados que esta KB ya listaba. |
| **OpenTelemetry Collector** | [open-telemetry/opentelemetry-collector](https://github.com/open-telemetry/opentelemetry-collector) | **Apache-2.0** | 7.793 | 2026-11-20 | Trazas y métricas del servidor de juego. Capa neutral: no te ata a ningún vendor de observabilidad. |
| **Apache Superset** | [apache/superset](https://github.com/apache/superset) | **Apache-2.0** | 75.615 | 2026-11-22 | BI sobre ClickHouse. **El BI permisivo**: Metabase y Grafana no lo son. |
| **go-wind-uba** | [tx7do/go-wind-uba](https://github.com/tx7do/go-wind-uba) | **MIT** (1.061) | 51 | 2026-11-20 | Analítica de comportamiento con **25 modelos** (eventos, embudos, retención, atribución, rutas, cohortes y **telemetría de juego** explícitamente), Kafka → ClickHouse/Doris, multi-tenant, reporting en Superset. La ruta ligera cuando el pipeline completo no se justifica. |
| **Rawbbit** | [mirlan-irokez/rawbbit](https://github.com/mirlan-irokez/rawbbit) | **Apache-2.0** (11.349) | 11 | 2026-11-09 | Analítica de juego autohospedada: eventos por HTTP → NATS JetStream → Parquet particionado en tu propio object storage, **con un MCP de sólo lectura**. ⚠️ 11 ★ y 3 forks: es temprano. El código es Apache-2.0; el producto de pago del mismo autor es otra cosa. |
| **GlitchTip** | GitLab: `glitchtip/glitchtip-backend` | **MIT** (1.067) | — | — | Crash/error reporting compatible con los SDK de Sentry. **No está en GitHub** (ver hallazgo nº5). |
| **GameTuner** (10 repos) | [GameTuner/collector](https://github.com/GameTuner/collector) · [enricher](https://github.com/GameTuner/enricher) · [unity-tracker](https://github.com/GameTuner/unity-tracker) | **Apache-2.0** (11.358 / 11.359 / 11.325) | 0 | ⚠️ 2024-02-06 | **Referencia de esquema de eventos de juego móvil**, no dependencia viva. Ver el límite declarado del hallazgo nº3 antes de forkear. |

### 🆕 Fuera de la capa — tres incorporaciones verificadas este pass

| Agente / repo | Repo | Licencia (bytes) | ★ | Último push | Qué aporta |
|---|---|---|---|---|---|
| **OpenGameAgent** | [EricSun0218/OpenGameAgent](https://github.com/EricSun0218/OpenGameAgent) | **MIT** (1.096) | 61 | 2026-11-17 | Runtime TypeScript para personajes y mundos AI-native: **estado durable, acciones confiables, memoria y herramientas**, con integraciones Unity, Godot y Unreal. Es la forma de agente que esta KB venía describiendo en las recetas de NPC, empaquetada. Temprano (61 ★) pero MIT limpio y copyright 2026. |
| **awesome-ai-for-games** | [Eurekaleo/awesome-ai-for-games](https://github.com/Eurekaleo/awesome-ai-for-games) | **MIT** | 391 | 2026-11-19 | Índice de investigación sobre IA a lo largo del ciclo de vida del juego. Fuente de candidatos, **no** de veredictos de licencia. |
| **awesome-ai-game-dev-tools** | [SubmitGame/awesome-ai-game-dev-tools](https://github.com/SubmitGame/awesome-ai-game-dev-tools) | **CC0-1.0** | 93 | 2026-11-21 | Índice de herramientas, refrescado a diario, con filtro de actividad en el último año. Mismo uso y misma advertencia. |

### ⛔ Rechazos **medidos** de este pass — se publican porque el silencio se lee como cobertura

| Candidato | Medición | Motivo del rechazo |
|---|---|---|
| [snowplow/stream-collector](https://github.com/snowplow/stream-collector) · [snowplow/enrich](https://github.com/snowplow/enrich) | SLULA 1.1 §1.1 leída | **Producción prohibida.** Sólo evaluación con datos de prueba o sintéticos. |
| [getsentry/sentry](https://github.com/getsentry/sentry) | FSL-1.1-Apache-2.0, 3.785 B | Apache-2.0 sólo a los 2 años de cada versión. |
| [Countly/countly-server](https://github.com/Countly/countly-server) | AGPL-3.0 §7 modificada, 1.025 B | Copyleft de red **más** restricción de marca. |
| [metabase/metabase](https://github.com/metabase/metabase) | AGPL + Commercial, 855 B | Carve-out `enterprise/`. |
| [openreplay/openreplay](https://github.com/openreplay/openreplay) | multi-licencia, 36.511 B | Carve-out `ee/` propietario. |
| [grafana/grafana](https://github.com/grafana/grafana) · [rybbit-io/rybbit](https://github.com/rybbit-io/rybbit) | AGPL-3.0 | Copyleft de red. |
| [CrateBytes/CrateBytes](https://github.com/CrateBytes/CrateBytes) | **`archived=true`**, último push **2025-01-22**, AGPL-3.0 | **Archivado Y copyleft.** Ver la corrección de regla abajo. |
| [AIGeeksGroup/Code2Games](https://github.com/AIGeeksGroup/Code2Games) | 89 ★, **sin archivo de licencia** en ninguna rama/ruta probada | Sin grant no hay permiso. |
| [ADM1SH/churn-prediction-scaling](https://github.com/ADM1SH/churn-prediction-scaling) | 0 ★, licencia `null`, telemetría sintética y webhook mock según su propio README | Prototipo sin grant. |
| [deq-labs/analytics-platform](https://github.com/deq-labs/analytics-platform) | 0 ★, AGPL-3.0 | Copyleft de red, sin tracción. |
| [GlitchTip/GlitchTip](https://github.com/GlitchTip/GlitchTip) *(el de GitHub)* | 3 ★, push 2022-08-21, descripción *"Readme config"* | **No es el proyecto.** El proyecto está en GitLab y es MIT. |

### 🔧 Corrección a una regla de esta KB: el *tell* del "archivado pero recomendado" es **más débil** de lo que el cycle anterior afirmó

La regla heredada decía que, en los siete casos registrados, el proyecto muerto
siempre venía recomendado por **documentación del propio proyecto**, y que una
fuente independiente nunca cometía ese error.

`CrateBytes` es el **8º caso y rompe esa parte de la regla**: está archivado desde
el 2025-01-22 y quien lo recomienda es una **awesome-list de terceros**
([ishandutta2007/Awesome-Game-Live-Operations](https://github.com/ishandutta2007/Awesome-Game-Live-Operations),
MIT, 3 ★, actualizada el 2026-09-21), no su propia documentación.

> **La parte que sobrevive de la regla:** una recomendación **nunca** es evidencia
> de vida, venga de donde venga. **La parte que se cae:** que el origen
> project-adjacent fuera el discriminante. Las listas de terceros se escriben una
> vez y tampoco se revisan. **El único instrumento de liveness sigue siendo el par
> `archived` + `pushed_at`**, y hay que correrlo sobre cada fila, incluidas las que
> llegan por una fuente que parece independiente.

### 📏 Lo que este pass NO puede medir, dicho explícitamente

- **No medí la fecha del snapshot de Snowplow vendorizado dentro de GameTuner.**
  Es la verificación que decide si su Apache-2.0 es oponible. Está dicho arriba.
- **`WebFetch` no resuelve ningún host general en esta sesión** (`law.stanford.edu`
  → `ENOTFOUND`). **2ª medición** del gap declarado en el cycle 9: no es
  intermitente. Todo lo regulatorio de este pass viene de *snippets* de búsqueda y
  está etiquetado como secundario donde corresponde.
- **`gh api users/<org>/repos` está denegado** en esta sesión (403: *"sessions are
  bound to their configured repositories"*). La vuelta que sí funciona es
  `gh api search/repositories -f q='user:<org>'`, y es la que destapó GameTuner
  entero y OpenSnowcat.
- **Las estrellas de `opensnowcat-devkit`, `-rdb-loader` e `-iglu-server`** se leen
  del badge de búsqueda; **sólo `-collector` y `-enrich` tienen la licencia leída
  byte a byte**. Está marcado en la tabla.

---

## 🔴 Lo más importante del pass 2026-11-22 (cycle 15): **la capa OPERATIVA de cara al jugador — accesibilidad, anti-cheat, soporte y moderación de voz — es la primera capa de esta KB donde la OBLIGACIÓN ya existe y la OFERTA open source NO**

Catorce passes auditaron lo que el estudio **construye**: NPCs, assets, diálogo,
voz, world models, backends, pipelines. Este pass mide la capa que el estudio
**opera después del lanzamiento** y que el jugador vive como producto: que el
juego se pueda **usar** (accesibilidad), que se pueda **jugar limpio**
(anti-cheat), que el jugador pueda **pedir ayuda** (soporte) y que el canal de
voz no sea tóxico (moderación de audio).

El resultado se invierte respecto de las catorce capas anteriores. En todas
ellas la pregunta era *"¿cuál de estos repos permisivos elijo?"*. Acá la
enumeración devuelve **casi nada vivo**, mientras la obligación legal **ya está
en vigor en dos regiones y ya se litigó en una** (Francia, nov-2025).

| Sub-capa | Candidatos enumerados | Vivos **y** permisivos | Veredicto medido |
|---|---|---|---|
| Accesibilidad (screen reader / captions) | 9 | **2** | La función **se mudó al motor** |
| Anti-cheat con ML | 8 | **0** | **Desierto medido** |
| Soporte al jugador con LLM | 5 | **0** | Sólo proyectos de 0–1 ★ |
| Moderación de **voz** en juego | 1 | **0** | Sólo propietario |

> **La frase que hay que decir en la propuesta:** en esta capa Globant **no
> compone, construye** — y el motivo no es que falte madurez, es que la oferta
> abierta no existe. Decirlo primero es la diferencia entre un alcance honesto y
> una integración que se descubre imposible en el mes tres.

### 🔴 Hallazgo nº1: `AccessKit/accesskit` — **tres errores anidados en el repo que sostiene la accesibilidad de Godot 4.5**

`AccessKit` es la infraestructura de accesibilidad que Godot 4.5 adoptó para dar
soporte de lector de pantalla a los nodos `Control`. Es decir: **todo juego
publicado con Godot 4.5+ que exponga UI accesible está embarcando este repo**.
Esta KB nunca lo había mirado. Al mirarlo, los tres canales de verificación de
esta KB se contradicen entre sí:

| Canal | Qué dice | ¿Correcto? |
|---|---|---|
| Badge de GitHub (`search/repositories`) | `BSD-3-Clause` | ❌ **No es el grant del proyecto** |
| Archivos de licencia en la raíz | `LICENSE-APACHE` (9.723 B) + `LICENSE-MIT` (1.023 B) | ✅ |
| `README.md`, sección *License* | *"licensed under the Apache License, Version 2.0 **or** the MIT license, **at your option**"* | ✅ **Éste es el grant** |

**De dónde sale el `BSD-3-Clause`:** el repo tiene un tercer archivo,
`LICENSE.chromium` (1.559 B), que empieza
`// Copyright 2015 The Chromium Authors. All rights reserved.`. El clasificador
de GitHub eligió **la licencia del código de terceros vendorizado** por encima
del grant propio del proyecto. El README lo dice explícitamente:

> *"Significant portions of AccessKit are derived from Chromium and are covered
> by its BSD-style license."*

⇒ **El badge no es una licencia inventada: es una licencia real del repo
equivocado.** Es el reverso exacto de la regla T60 de la flota (*NOASSERTION no
es un veredicto*): acá el badge **sí** afirma algo, y lo que afirma es un
componente, no el producto.

**La consecuencia comercial, que no es teórica:** el grant que Globant usa es
Apache-2.0 **o** MIT (elegir Apache-2.0 por la concesión expresa de patentes).
Pero las porciones derivadas de Chromium conservan **BSD-3-Clause**, cuya
cláusula 2 obliga a reproducir el aviso de copyright y el disclaimer **en la
distribución binaria**. Un juego hecho con Godot 4.5 que no incluya el aviso de
Chromium en su pantalla de créditos/licencias **está incumpliendo una obligación
de atribución**, no de copyleft. Es barato de cumplir y caro de descubrir tarde.

#### 📏 5º defecto medido del *checksum de bytes* de esta KB: **el Apache-2.0 sin apéndice**

`LICENSE-APACHE` de AccessKit mide **9.723 B**, no los **11.357 B** canónicos
que esta KB usa como testigo de Apache-2.0. No está adulterado: el archivo
**termina en `END OF TERMS AND CONDITIONS`** y omite el bloque
*"APPENDIX: How to apply the Apache License to your work"*, que es boilerplate
instructivo y **no** parte de los términos.

⇒ **Un Apache-2.0 de ~9.7 KB es Apache-2.0 íntegro.** El checksum da aquí un
**falso negativo**, y la causa es nueva: no es un `LICENSE` roto (cycle 8), ni un
SPDX short-form (cycle 13), ni mayúsculas (cycle 5) — es **la poda del apéndice**.

#### 📏 6º defecto, encontrado en el mismo pass: **la variante Markdown mueve el byte-count de CUALQUIER licencia**

Dos repos de este pass llevan **la misma** MPL-2.0 y miden distinto, y un tercero
rompe el testigo de GPL:

| Repo | Licencia | Archivo | Bytes | Causa de la diferencia |
|---|---|---|---|---|
| [waldo-vision/optical.flow.demo](https://github.com/waldo-vision/optical.flow.demo) | MPL-2.0 | `LICENSE` | **16.725** | Texto plano (≈ testigo ~16.700 de esta KB ✅) |
| [ethindp/prism](https://github.com/ethindp/prism) | MPL-2.0 | `master/LICENSE` | **15.504** | **Markdown** (`### 1. Definitions`, `**1.1.**`) |
| [Farama-Foundation/Arcade-Learning-Environment](https://github.com/Farama-Foundation/Arcade-Learning-Environment) | GPL-2.0 | `LICENSE.md` | **17.879** | **Markdown**; además esta KB no tenía testigo de GPL-2.0 |

⇒ **Regla operativa:** el checksum de bytes sólo es válido contra un archivo
**en texto plano**. Si el nombre del archivo termina en `.md`, o la primera línea
empieza con `#`, el testigo no aplica y hay que leer el encabezado. Añadir a la
tabla de testigos: **GPL-2.0 Markdown ≈ 17.880 B**, **MPL-2.0 Markdown ≈ 15.500 B**.

### 🔴 Hallazgo nº2: **la accesibilidad se mudó al motor — y los plugins que todo buscador recomienda llevan entre 3 y 5 años muertos**

Esta es la razón estructural por la que la enumeración devuelve tan poco: la
función **dejó de ser un plugin** y pasó a ser una primitiva del engine.

| Repo | ★ | Licencia (byte-read) | Último push | Estado medido |
|---|---|---|---|---|
| [AccessKit/accesskit](https://github.com/AccessKit/accesskit) | 1.567 | **Apache-2.0 OR MIT** (dual, 9.723 B + 1.023 B) | **2026-11-21** | 🟢 Vivo. **Es la base del screen reader de Godot 4.5** |
| [ethindp/prism](https://github.com/ethindp/prism) | 67 | **MPL-2.0** (15.504 B, Markdown) | **2026-11-21** | 🟢 Vivo. Sucesor declarado de SRAL |
| [mikrima/UnityAccessibilityPlugin](https://github.com/mikrima/UnityAccessibilityPlugin) | 85 | **Apache-2.0** (`LICENSE`, 11.357 B ✅ checksum exacto) | ⚠️ 2023-05-13 | 🟡 **3,5 años sin push**. El único plugin de Unity permisivo que existe |
| [lightsoutgames/godot-accessibility](https://github.com/lightsoutgames/godot-accessibility) | 87 | **MIT** (`master/LICENSE`, 1.070 B) | ⛔ 2021-05-19 | 🔴 **5,5 años sin push** — y es el primer resultado de todo buscador |
| [m1maker/SRAL](https://github.com/m1maker/SRAL) | 21 | **MIT** (`LICENSE`, 1.064 B) | 2026-08-05 | ⛔ **ARCHIVADO** por su autor |
| [jumpingmushroom/Earshot](https://github.com/jumpingmushroom/Earshot) | 1 | **MIT** (`master/LICENSE`, 1.069 B) | 2026-10-18 | 🟢 Vivo. Mod de Valheim: *closed captions* de sonidos con **flecha de dirección** |
| [dominicbytes/game-accessibility-skill](https://github.com/dominicbytes/game-accessibility-skill) | 1 | **MIT** | 2026-10-18 | 🟢 Vivo. *Agent Skill* que hace que un agente de código implemente opciones accesibles |
| [Unity-Technologies/a11y-public-sample](https://github.com/Unity-Technologies/a11y-public-sample) | 26 | ⛔ **Unity Companion License** (`LICENSE.md`, **453 B**) | 2026-06-12 | 🔴 Badge `NOASSERTION`. **No es open source** |

**La lectura de Godot 4.5** (notas de release oficiales): *"Thanks to AccessKit,
we added screen reader support to Control nodes"*, con soporte del **editor**
todavía parcial (sólo Project Manager, nodos UI estándar e inspector). Hay
reportes de usuarios de que funciona con **Orca** en Linux y con el **Narrador de
Windows 11** — incluyendo el caso de un `TextureRect` usado como cursor que el
narrador leía en voz alta, es decir: **la primitiva del engine lee de más si
nadie la configura**. La accesibilidad del engine no es un interruptor.

#### 🔴 La regla nueva: **el sucesor puede ser MENOS libre que el archivado**

`m1maker/SRAL` (**MIT**, archivado 2026-08-05) remite en su propio README a
`ethindp/prism` (**MPL-2.0**, vivo). El autor explica el motivo —problemas de
arquitectura— y no menciona la licencia.

⇒ **Migrar fuera de una dependencia archivada NO es neutral en licencia.** Acá el
camino de salida cuesta pasar de permisiva pura a **copyleft por archivo**:
MPL-2.0 es perfectamente usable en un juego propietario (no contamina el binario
enlazado), pero obliga a **publicar las modificaciones de los archivos MPL** y a
declararlo. Es manejable; no declararlo no lo es.

**Segunda instancia medida del mismo patrón en esta KB.** El cycle 13 registró
`Piper`: relicenciado de **MIT a GPL-3.0** al cambiar de repo, con el repo MIT
archivado. Dos capas distintas, mismo movimiento ⇒ deja de ser una anécdota y
pasa a ser **un chequeo obligatorio**: al reemplazar un repo archivado, la
licencia del sucesor se verifica **antes** de aceptar la migración.

### 🔴 Hallazgo nº3: **no existe anti-cheat con ML open source viable — y esto es un resultado, no una búsqueda fallida**

Se enumeró la categoría completa (`anticheat machine learning`, `game-security`,
`anticheat-plugin`, más la búsqueda general de 2026). **Ocho candidatos. Cero
utilizables.** Se publica la tabla de rechazos porque, como dice esta KB, *el
silencio se lee como cobertura*:

| Repo | ★ | Licencia | Estado medido | Motivo del rechazo |
|---|---|---|---|---|
| [waldo-vision/optical.flow.demo](https://github.com/waldo-vision/optical.flow.demo) | 526 | **MPL-2.0** (16.725 B) | ⛔ **ARCHIVADO** 2021-11-20 | El proyecto más grande de la categoría lleva **5 años muerto** |
| [lrq3000/oacs](https://github.com/lrq3000/oacs) | 35 | ⛔ **sin ningún archivo de licencia** | ⛔ push 2017-03-10 | **9,7 años muerto** + sin grant. Sólo tenía puente a OpenArena |
| [korpys667/MKAC](https://github.com/korpys667/MKAC) | — | **GPL-3.0** | vivo | Copyleft fuerte: inviable en servidor distribuido de cliente |
| [gtxcinerbaba-hash/ZeyronAC](https://github.com/gtxcinerbaba-hash/ZeyronAC) | 4 | **GPL-3.0** | 2026-08-22 | Copyleft + sólo Minecraft |
| [KaelusAI/Shard](https://github.com/KaelusAI/Shard) | — | — | vivo | La detección ML **corre en la nube del proveedor**: no es autohospedable |
| [SoMax1soft/mls-network-plugin](https://github.com/SoMax1soft/mls-network-plugin) | — | no estándar | vivo | El transporte del modelo real **no está en el código público** |
| [islamyasin07/Ares-AntiCheat-System](https://github.com/islamyasin07/Ares-AntiCheat-System) | 2 | ⛔ `null` | 2025-12-23 | Sin grant |
| [DevInsilicon/MLAAC](https://github.com/DevInsilicon/MLAAC) | 0 | ⛔ `null` | 2026-02-17 | Sin grant, sin tracción |

**Patrón:** siete de los ocho son **específicos de Minecraft**. La categoría
existe como ecosistema de plugins de servidores de Minecraft, **no** como
tecnología reutilizable. El mercado real (Riot Vanguard, BattlEye, Easy
Anti-Cheat) es **íntegramente propietario y de kernel**.

⇒ **Para un cliente, anti-cheat es "construir o comprar propietario".** No hay
punto de partida abierto. La única pieza honesta que esta KB puede ofrecer es
**detección conductual de anomalías sobre la telemetría del backend** —que es lo
que ya describe la Receta 3 con Nakama— y hay que presentarla como lo que es: un
detector estadístico, no un anti-cheat.

### 🔴 Hallazgo nº4: **soporte al jugador y moderación de VOZ: dos sub-capas más con oferta cero**

- **Soporte al jugador con LLM:** la enumeración devuelve 5 repos, **todos de
  0–1 ★**, y ninguno es un agente de soporte: son experimentos de *roleplay*,
  benchmarks de juegos sociales y perfiles personales. La Receta 5 de esta KB
  (*Game Support Agent*) sigue siendo **una construcción desde frameworks
  genéricos**, y así hay que venderla.
- **Moderación de voz en juego:** 1 solo resultado
  ([vinifborgess/azure-guardian](https://github.com/vinifborgess/azure-guardian),
  MIT, **0 ★**), y es un envoltorio sobre un servicio de Azure. La moderación de
  **texto** sí tiene oferta real —`unitaryai/detoxify` y `guardrails-ai`, ya en
  esta KB desde el cycle 13— pero **el audio no**. La brecha importa porque el
  CVAA obliga a que el chat de voz tenga equivalente en texto (ver `intel/market.md`):
  **el camino regulatorio y el camino técnico coinciden** — transcribir la voz
  habilita a la vez la accesibilidad y la moderación. Un solo componente, dos
  obligaciones.

### 🔵 Hallazgo nº5: inventario plano (regla T64) — **5 incumbentes ausentes en 14 passes**

La regla de la flota dice que un vocabulario de consultas con forma de
*problema* tiene un punto ciego con forma de *producto*. Se gastó una consulta en
un inventario plano contra los incumbentes obvios de categorías que esta KB
**ya tiene abiertas**. Cinco faltaban:

| Repo | ★ | Licencia (byte-read) | Push | Por qué importa / categoría que ya existía aquí |
|---|---|---|---|---|
| [SanderMertens/flecs](https://github.com/SanderMertens/flecs) | 8.854 | **MIT** (`master/LICENSE`, 1.131 B) — badge `NOASSERTION` | 2026-11-20 | ECS rápido para C/C++. El repo más grande ausente de esta KB |
| [danijar/dreamerv3](https://github.com/danijar/dreamerv3) | 4.004 | **MIT** (1.058 B) | 2026-10-04 | **El world model canónico.** La KB tiene sección de world models desde el cycle 1 |
| [Farama-Foundation/Arcade-Learning-Environment](https://github.com/Farama-Foundation/Arcade-Learning-Environment) | 2.463 | ⚠️ **GPL-2.0** (`LICENSE.md`, 17.879 B) | 2026-11-15 | ALE, el benchmark más viejo de *agentes que juegan*. **Copyleft: sólo harness interno de evaluación, nunca embarcado** |
| [MichaelTMatthews/Craftax](https://github.com/MichaelTMatthews/Craftax) | 476 | **MIT** (1.059 B) | 2026-10-27 | Crafter + NetHack en JAX. ICML 2024 *spotlight*. Benchmark de exploración |
| [vivekjoshy/openskill.py](https://github.com/vivekjoshy/openskill.py) | 377 | **MIT** (1.068 B) | 2026-05-12 | Rating multijugador (Weng-Lin). **La KB tenía `open-match` como matchmaking y ningún sistema de rating.** Puertos: [intinig/go-openskill](https://github.com/intinig/go-openskill) (MIT, 16 ★), [pocketcombats/openskill-java](https://github.com/pocketcombats/openskill-java) (Apache-2.0, 13 ★) |

**`flecs` confirma el mecanismo T60 entre KBs.** Badge `NOASSERTION`, archivo
`LICENSE` = MIT íntegro de 1.131 B. ¿Por qué falla el clasificador? Porque bajo
el título lleva **una segunda línea de copyright**:
`Portions Copyright (c) Meta Platforms, Inc. and affiliates`. Es **exactamente**
el mecanismo que la KB de *enterprise* midió en `Tencent/LoopForge` (preámbulo
corporativo que derrota la detección de MIT). Dos industrias, dos repos sin
relación, mismo fallo del clasificador ⇒ **un `NOASSERTION` con una línea de
copyright corporativo extra es, hasta prueba en contrario, una permisiva intacta.**

**`ALE` confirma el hallazgo del cycle 11.** Aquel pass midió que la Farama
Foundation publica MIT, GPL-2.0 y *nada* a la vez, pero **no nombró cuál era el
GPL-2.0**. Es éste. Queda cerrado el pendiente.

### 🆕 Repos nuevos verificados este pass (cycle 15) — fuera de la capa operativa

| Repo | ★ | Licencia (byte-read) | Push | Descripción |
|---|---|---|---|---|
| [ForgeAX/forgeax-engine](https://github.com/ForgeAX/forgeax-engine) | 116 | **Apache-2.0** (`LICENSE`, 11.358 B) | 2026-11-20 | Engine TypeScript *AI-native* pensado para que lo maneje un agente, multiplataforma |
| [TohmaN233/YN-translation-workshop](https://github.com/TohmaN233/YN-translation-workshop) | 235 | **MIT** | 2026-11-11 | Banco de trabajo de traducción y revisión con IA para ficción larga **y localización de juegos** |

> Nota sobre `forgeax-engine`: **11.358 B**, un byte sobre el testigo canónico de
> Apache-2.0 (11.357 B). Es un salto de línea final. **Tolerancia ±2 B sobre el
> testigo = mismo texto**; fuera de eso, leer.

### 📏 Lo que este pass NO puede medir, dicho explícitamente

- **No se pudo confirmar si EN 301 549 v4.1.1 fue citada en el DOUE.** Un
  proveedor la pronostica para **nov-2026** (este mes). Sin la cita, la versión
  con efecto legal sigue siendo **v3.2.1**. No usar v4.1.1 como requisito
  contractual hasta verificar el Diario Oficial.
- **No se midió cobertura de accesibilidad en exportación web de Godot.** Hay una
  pregunta abierta en el foro oficial y ninguna respuesta. Si el entregable es
  WebGL, **esto es un riesgo abierto, no una función asumida**.
- **No hay dato de adopción de accesibilidad en APAC ni LATAM** (ver
  `intel/market.md`): se buscó y no hay. Es un cero medido, no un cero asumido.
- **No se verificó el alcance exacto del CVAA** contra el texto de la FCC: la
  fecha de 1-ene-2019 y la regla de *substantial upgrades* vienen de guías
  secundarias de accesibilidad, no del `47 CFR`. **Verificar con el abogado del
  cliente antes de ponerlo en un contrato.**


## 🔴 Lo más importante del pass 2026-11-22 (cycle 14): **la capa de PRODUCCIÓN DE ASSETS — y el primer eje donde la licencia del código y la licencia de lo que el código PRODUCE son dos objetos distintos**

> **Eje elegido aplicando la regla de método nº14 del cycle 13** (*la saturación
> se mide, no se intuye*). El cycle 13 abrió la capa que el jugador **lee y
> escucha** (diálogo, moderación, voz, localización). Queda abierta la capa que
> el jugador **mira**, y que es la línea de coste más grande de un juego:
> **malla → textura → rig → animación → música y SFX**. Medición previa a
> escribir nada:
>
> | Eje | Candidatos devueltos por las búsquedas | Ya estaban en esta KB | Nuevos |
> |-----|-----------------------------------------|------------------------|--------|
> | Engine / MCP / NPC / backend (el eje histórico) | 8 re-verificados | **8** | 0 |
> | Malla / textura / rig / motion / música-SFX | 24 | **0** | **24** |
>
> ⇒ Segundo eje consecutivo con tasa de novedad del **100%** y **cero** filas
> previas después de trece passes. La KB tenía 233 slugs y ni uno solo de
> generación de assets.
>
> **Canales usados: los tres de la casa, más uno nuevo que este pass se ve
> obligado a inventar.** Ver el hallazgo nº2: hay una clase de restricción que
> **ninguno de los tres canales puede ver**, porque no vive dentro del repo.

### 🔴 Hallazgo nº1: **el slug que devuelve el buscador no es un repo** — 4 de 5 resultados apuntaban a un dueño que no existe

La consulta `open source 3D asset generation game development 2026 mesh texture
MIT Apache` devolvió **cinco** resultados para el mismo proyecto, *AssetFurnace*,
bajo **cinco dueños distintos**. Verificados uno por uno:

| Slug tal como lo devolvió el buscador | `search/repositories` | `raw.../HEAD/README.md` | Veredicto |
|---|---|---|---|
| `matteodante/AssetFurnace` (**el resultado nº1**) | HTTP **422** *"resources do not exist"* | HTTP **404**, 14 B | 🔴 **no existe** |
| `johndeeC/image-to-3dlab` | 0 items | — | fork (search excluye forks) |
| `vitalis/image-to-3dlab` | 0 items | — | fork |
| `zorrobyte/image-to-3dlab` | 0 items | — | fork |
| ✅ **`Bingeljell/image-to-3dlab`** | **1.244 ★**, `Apache-2.0`, push **2026-11-20**, `main` | HTTP **200**, 31.857 B | 🟢 **el repo real** |

El `README.md` del repo real abre con `<h1 align="center">AssetFurnace</h1>` y
`<sub>by Bingeljell</sub>`: **el proyecto se llama AssetFurnace y el repo se
llama `image-to-3dlab`.** El buscador sintetizó el slug juntando el nombre del
proyecto con el dueño de un fork, y lo hizo mal cuatro veces de cinco.

> **Regla de método nº15 (para copiar a toda la familia de KBs): el título de un
> resultado de búsqueda no es un identificador.** El único identificador es el
> par `(owner, repo)` que sobrevive a `search/repositories` **y** a un `raw`
> 200. Y los dos códigos de error significan cosas distintas y hay que
> distinguirlos: **422** = el owner/repo no existe o no es visible; **0 items
> con HTTP 200** = existe pero es un **fork** (la búsqueda de GitHub los excluye
> por defecto). Tratar los dos como "no encontrado" borra la información que
> dice *dónde está el upstream*.

Licencia del repo real, leída en bytes: `LICENSE` **11.358 B = Apache-2.0**
estándar, más un `NOTICE` de 1.162 B. ✅ Usable.

### 🔴 Hallazgo nº2: **los dos generadores 3D MIT más grandes del mundo no son usables comercialmente tal como se envían** — y la restricción no está en ningún archivo del repo

Éste es el hallazgo caro del pass. `microsoft/TRELLIS` (**14.370 ★**) y
`microsoft/TRELLIS.2` (**13.144 ★**) son, por estrellas, los líderes abiertos de
imagen-a-3D. Los **tres** canales de la casa dicen que son limpios:

| Canal | TRELLIS | TRELLIS.2 |
|---|---|---|
| 1. `search/repositories` → SPDX | `MIT` | `MIT` |
| 2. `raw.../HEAD/LICENSE` byte-read | MIT estándar | MIT estándar |
| 3. `search/code filename:LICENSE` (enumeración) | `LICENSE` (1 archivo) | `LICENSE` + `assets/hdri/license.txt` (**CC0**, Poly Haven) |

Y sin embargo el propio `README.md` lo dice, en prosa, en su sección `## ⚖️ License`:

> **TRELLIS** — *"TRELLIS models and the majority of the code are licensed under
> the MIT License. **The following submodules may have different licenses**"* →
> `diffoctreerast`, derivado de `diff-gaussian-rasterization`.
>
> **TRELLIS.2** — *"Please note that **certain dependencies operate under
> separate license terms**"* → `nvdiffrast`, `nvdiffrec`.

Esas dependencias, leídas byte a byte en su propio repo:

| Dependencia | Repo | Bytes | Licencia | Cláusula operativa (literal) |
|---|---|---|---|---|
| `diff-gaussian-rasterization` (1.512 ★) | [graphdeco-inria/diff-gaussian-rasterization](https://github.com/graphdeco-inria/diff-gaussian-rasterization) | 4.275 | **Gaussian-Splatting License** (Inria + MPII) | *"THE USER CANNOT USE, EXPLOIT OR DISTRIBUTE THE SOFTWARE FOR COMMERCIAL PURPOSES"* |
| `diffoctreerast` | [JeffreyXiang/diffoctreerast](https://github.com/JeffreyXiang/diffoctreerast) (66 ★, `NOASSERTION`) | 3.645 | derivado de la anterior — *"This Software is a derivative work of the Original Software"* | idem, misma frase literal |
| `nvdiffrast` (1.942 ★) | [NVlabs/nvdiffrast](https://github.com/NVlabs/nvdiffrast) | 4.445 | **Nvidia Source Code License (1-Way Commercial)** | *"only may be used or intended for use **non-commercially** … means for research or evaluation purposes only"* |
| `nvdiffrec` (2.301 ★) | [NVlabs/nvdiffrec](https://github.com/NVlabs/nvdiffrec) | 4.312 | **NVIDIA Source Code License** | *"3.3 Use Limitation … only may be used or intended for use non-commercially"* |

**Dónde entra cada una, medido en el código (no inferido):**

| Archivo de TRELLIS | Import | Momento | Licencia efectiva de ese camino |
|---|---|---|---|
| `trellis/renderers/mesh_renderer.py` | `import nvdiffrast.torch as dr` (línea 2) | **top-level, incondicional** | ⛔ no comercial |
| `trellis/renderers/gaussian_render.py` | `from diff_gaussian_rasterization import …` (línea 58) | dentro de función (lazy) | ⛔ no comercial |
| `trellis/renderers/octree_renderer.py` | `diffoctreerast` | módulo | ⛔ no comercial |
| `trellis/renderers/__init__.py` | `importlib` en `__getattr__` | **lazy**: nada se importa hasta que se nombra el renderer | — |

⇒ **Los tres renderers de TRELLIS importan un paquete no comercial. No hay un
cuarto.** La parte buena, y es la que hace accionable el hallazgo: `setup.sh`
expone `--nvdiffrast`, `--diffoctreerast`, `--kaolin` como **flags opcionales**
—no se instalan por defecto— y el `__init__.py` es **perezoso**. Es decir: el
camino de *generación* (MIT, pesos incluidos: el README dice *"TRELLIS **models**
and the majority of the code"*) **se puede ejecutar sin tocar ningún renderer**.
Lo que no se puede es **enviar el código de visualización del repo**.

> **Regla de método nº16, la central de este pass: la enumeración cubre el repo;
> no cubre lo que el repo te manda a instalar.** La licencia de un proyecto es
> la licencia de su **clausura de dependencias**, y el único lugar donde esa
> clausura está declarada es **prosa en el README** — que ningún canal
> automático lee. El cycle 13 cerró la brecha entre *sonda* y *enumeración*.
> Ésta es la brecha siguiente: entre *enumeración* y **clausura**.
>
> **Canal 4 (nuevo, obligatorio para esta página desde hoy):** leer la sección
> `## License` del `README.md` y **resolver cada dependencia nombrada en ella
> como si fuera un repo de esta KB**. Coste: 1 `curl` + 1 `grep -i licen` por
> repo. En este pass cambió el veredicto de **27.514 ★** en dos filas.

**Sustituto medido, no especulado.** `nv-tlabs/FlexiCubes` —la extracción de
malla que TRELLIS usa en versión modificada— es **Apache-2.0** (`LICENSE.txt`,
11.357 B), sin restricción. La ruta malla-sin-render es la que un juego
necesita; la de *preview* es la que hay que reescribir. Ver la **Receta 36** en
`compose/patterns.md`.

### 🔴 Hallazgo nº3: **en esta capa los PESOS son un objeto de licencia aparte, y están medidos como NO comerciales en 3 de 3 casos**

La KB ya tenía el eje *pesos-vs-código* anotado desde el cycle 6, pero con un
solo ejemplo parcial. En la capa de audio el eje no es una excepción: **es la
norma**, y el canal que lo detecta es la enumeración (canal 3), porque el
segundo archivo tiene un **nombre distinto de `LICENSE`**.

| Proyecto | ★ | Código (byte-read) | **Pesos** (archivo separado, byte-read) | ¿Se puede enviar el audio generado? |
|---|---|---|---|---|
| [facebookresearch/audiocraft](https://github.com/facebookresearch/audiocraft) | 23.943 | `LICENSE` → **MIT** | `LICENSE_weights` → **CC BY-NC 4.0** (19.333 B) | ⛔ **no** — MusicGen/AudioGen son NC |
| [multimodal-art-projection/YuE](https://github.com/multimodal-art-projection/YuE) | 13.656 | `LICENSE` → **Apache-2.0** (11.358 B) | `MODEL_LICENSE` → **CC BY-NC 4.0** (23.876 B), literal: *"The YuE2-3B, YuE2-Vae and YuE2-Vae-legacy checkpoint weights are licensed under CC BY-NC 4.0"* | ⛔ **no** |
| [HamSanders/soundbox](https://github.com/HamSanders/soundbox) | 239 | `LICENSE` → **MIT** (1.713 B) | el propio MIT trae un addendum: *"This MIT licence covers the … application code only. It does not cover the machine-learning model weights … several of which prohibit commercial use"* | ⚠️ **depende del modelo que cargues** |

**El caso `soundbox` es el más instructivo y por eso entra pese a sus 239 ★.**
Su MIT pesa **1.713 B** contra los ~1.070 B del MIT estándar: **+643 B de
addendum**. Ése es exactamente el *checksum de bytes* de la casa funcionando
como estaba diseñado — un MIT que no mide ~1.070 B **no es un MIT limpio**. Y el
addendum resuelve el problema de la forma correcta: *"does not redistribute any
model weights … downloads them from Hugging Face at the user's direction, using
the user's own account"*. ⇒ la app es MIT de verdad; **la obligación de licencia
se transfiere al operador**, por diseño y por escrito.

⚠️ **Nota de inventario:** el texto de licencia de `soundbox` nombra al producto
**"Mirli"**, no "soundbox". Repo y producto no coinciden — el mismo patrón del
hallazgo nº1.

### 🟡 Hallazgo nº4: la plantilla territorial de Tencent **no es una peculiaridad de los world models: es la plantilla de la casa** — 4º repo, línea de producto distinta

El cycle 10 encontró la exclusión EU / UK / Corea del Sur en los **tres** repos
de world models de Tencent y la llamó *"la plantilla de la casa"*. Este pass la
confirma fuera de esa línea de producto, en generación de malla:

| Repo | ★ | SPDX | Licencia leída en bytes | Territorio |
|---|---|---|---|---|
| 🆕 [Tencent-Hunyuan/Hunyuan3D-2.1](https://github.com/Tencent-Hunyuan/Hunyuan3D-2.1) | 4.519 | `NOASSERTION` | **Tencent Hunyuan 3D 2.1 Community License** (`LICENSE`, 17.914 B, release 13-jun-2025) | ⛔ **no aplica en EU / UK / Corea del Sur** |

Primera línea del archivo, **idéntica carácter por carácter** a la de los tres
repos del cycle 10:

```
THIS LICENSE AGREEMENT DOES NOT APPLY IN THE EUROPEAN UNION, UNITED KINGDOM
AND SOUTH KOREA AND IS EXPRESSLY LIMITED TO THE TERRITORY, AS DEFINED BELOW.
```

⇒ La exclusión territorial pasa de **3 repos / 1 línea de producto** a **4 repos
/ 2 líneas**. Para un engagement en EMEA o en Corea esto ya no es un dato sobre
*world models*: es un dato sobre **el proveedor**. La regla práctica que esta KB
puede publicar hoy con 4 observaciones y 0 contraejemplos: **asumir exclusión
territorial en cualquier release abierto de Tencent-Hunyuan y verificar la
primera línea del archivo de licencia antes de evaluar nada.**

### 🆕 Agentes y repos nuevos verificados este pass (cycle 14)

Los 3 canales de siempre + el canal 4 (clausura declarada en README) sobre cada
fila. ★ y `pushed_at` leídos hoy, 2026-11-22.

#### Generación de malla y textura

| Agente / repo | Repo | Licencia (byte-read) | ★ | Último push | Qué hace |
|---|---|---|---|---|---|
| **TRELLIS** | [microsoft/TRELLIS](https://github.com/microsoft/TRELLIS) | **MIT** (código **y pesos**) ⚠️ renderers con dependencias NC — ver hallazgo nº2 | 14.370 | 2026-10-27 | imagen/texto → 3D con representación latente estructurada; salida 3DGS, radiance field y malla |
| **TRELLIS.2** | [microsoft/TRELLIS.2](https://github.com/microsoft/TRELLIS.2) | **MIT** + `assets/hdri/license.txt` **CC0** (Poly Haven) ⚠️ idem | 13.144 | 2026-07-10 | sucesor; PBR nativo (base color / roughness / metallic) |
| **TripoSG** | [VAST-AI-Research/TripoSG](https://github.com/VAST-AI-Research/TripoSG) | **MIT** | 1.935 | 2025-04-18 ⚠️ **19 meses sin push** | imagen → malla con flow matching rectificado |
| **AssetFurnace** | [Bingeljell/image-to-3dlab](https://github.com/Bingeljell/image-to-3dlab) | **Apache-2.0** (11.358 B) + `NOTICE` | 1.244 | 2026-11-20 | pipeline local completo prompt/imagen → personaje texturizado, riggeado y animado, en Apple Silicon o NVIDIA; **anota la licencia de cada asset que produce** |
| **3AGameFactory** | [OpenDCAI/GameFactory-3A](https://github.com/OpenDCAI/GameFactory-3A) | **Apache-2.0** (11.338 B) | 1.344 | 2026-11-15 | framework de generación imagen + 3D + motion + audio + CG-video con salida a UE5, Blender, Unity, Godot 4 y three.js |

#### Rigging y animación

| Agente / repo | Repo | Licencia (byte-read) | ★ | Último push | Qué hace |
|---|---|---|---|---|---|
| ⚠️ **UniRig** | [VAST-AI-Research/UniRig](https://github.com/VAST-AI-Research/UniRig) | badge **MIT**, raíz MIT (1.124 B) — **pero `src/model/michelangelo/` es GPL-3.0 (35.148 B) y está en el camino por defecto**. Ver hallazgo nº5 | 2.189 | 2026-06-04 | auto-rigging: predice esqueleto y skinning weights con un transformer autorregresivo (SIGGRAPH'25, Tsinghua + Tripo) |
| ⚠️ **MDM** | [GuyTevet/motion-diffusion-model](https://github.com/GuyTevet/motion-diffusion-model) | **MIT** en el código; `utils/PYTORCH3D_LICENSE` aparte. ⚠️ **requiere los body models SMPL**, que `prepare/download_smpl_files.sh` baja de un **Google Drive sin autenticación** en vez del portal con click-through de MPI | 4.154 | 2025-10-01 | texto → movimiento humano (diffusion); salida a `.obj` por frame, integrable en Blender/Maya |
| **MotionGPT** | [OpenMotionLab/MotionGPT](https://github.com/OpenMotionLab/MotionGPT) | **MIT** | 2.006 | 2025-07-01 ⚠️ 17 meses | movimiento humano tratado como lenguaje; texto↔motion en un solo modelo |

#### Música, SFX y foley

| Agente / repo | Repo | Licencia (byte-read) | ★ | Último push | Qué hace |
|---|---|---|---|---|---|
| ✅ **ACE-Step** | [ace-step/ACE-Step](https://github.com/ace-step/ACE-Step) | **Apache-2.0**, confirmado en README (`## 📜 License & Disclaimer`) | 5.095 | 2026-02-15 | generación musical con letra; extender, versionar y *repaint* de una pista |
| ✅ **MOSS-TTS** (familia, incluye **MOSS-SoundEffect**) | [OpenMOSS/MOSS-TTS](https://github.com/OpenMOSS/MOSS-TTS) | **Apache-2.0** (11.375 B); README literal: *"Models in MOSS-TTS Family are licensed under the Apache License 2.0"* ⇒ **cubre los pesos** | 4.358 | 2026-09-06 | SFX de hasta 30 s a 48 kHz desde prompt de texto, además de TTS |
| ✅ **stable-audio-tools** | [Stability-AI/stable-audio-tools](https://github.com/Stability-AI/stable-audio-tools) | **MIT** en el código ⚠️ los checkpoints de Stability van por *Stability AI Community License* aparte | 3.920 | 2026-11-01 | entrenamiento e inferencia de modelos generativos de audio |
| ⚠️ **audiocraft** | [facebookresearch/audiocraft](https://github.com/facebookresearch/audiocraft) | **MIT** código / **CC BY-NC 4.0** pesos (`LICENSE_weights`) | 23.943 | 2026-03-03 | MusicGen + AudioGen + EnCodec |
| ⚠️ **YuE** | [multimodal-art-projection/YuE](https://github.com/multimodal-art-projection/YuE) | **Apache-2.0** código / **CC BY-NC 4.0** pesos (`MODEL_LICENSE`) | 13.656 | 2026-11-01 | canción completa de larga duración con voz y letra |
| ⚠️ **soundbox** | [HamSanders/soundbox](https://github.com/HamSanders/soundbox) | **MIT + addendum de pesos** (1.713 B) | 239 | 2026-11-22 | app local tipo ElevenLabs/Suno con **servidor MCP de 21 herramientas** para Claude Code y Cursor; no redistribuye pesos |

### ⚫ Candidatos nuevos evaluados y **rechazados** este pass (cycle 14)

Se listan con la causa medida, porque un rechazo sin causa se vuelve a evaluar
el pass siguiente (regla heredada del cycle 4: *los rechazos caducan como las
recomendaciones*).

| Repo | ★ | Causa medida (byte-read) | Bytes |
|---|---|---|---|
| [Stability-AI/stable-fast-3d](https://github.com/Stability-AI/stable-fast-3d) | 1.947 | **Stability AI Community License** — no es SPDX permisiva; umbral de facturación. Además **sin push desde 2025-01-22** (22 meses) | 11.727 |
| [ZeyueT/AudioX](https://github.com/ZeyueT/AudioX) | 1.559 | **CC BY-NC 4.0** — prohibición comercial directa | 19.355 |
| [declare-lab/TangoFlux](https://github.com/declare-lab/TangoFlux) | 887 | literal: *"created for **non-commercial, research-only** purposes"* | 2.528 |
| [Tencent-Hunyuan/Hunyuan3D-2.1](https://github.com/Tencent-Hunyuan/Hunyuan3D-2.1) | 4.519 | sin grant en **EU / UK / Corea del Sur** (hallazgo nº4). Utilizable sólo fuera de ese territorio | 17.914 |
| [openai/jukebox](https://github.com/openai/jukebox) | 8.027 | **`archived: true`**, último push 2024-06-19 | — |
| [backblaze-labs/awesome-audio-generation](https://github.com/backblaze-labs/awesome-audio-generation) | **12** | CC0-1.0 y vivo, pero **12 ★**: índice útil para investigar, no una base sobre la que construir | — |
| `matteodante/AssetFurnace` | — | **no existe** (hallazgo nº1) | — |

### 🔴 Hallazgo nº5: `VAST-AI-Research/UniRig` — badge **MIT**, y **GPL-3.0 en el camino de importación por defecto**

El único auto-rigger abierto serio de la lista. Los canales 1 y 2 lo dan limpio:
SPDX `MIT`, raíz `LICENSE` = MIT (1.124 B). **El canal 3 lo rompe.**
`search/code -f q='repo:VAST-AI-Research/UniRig filename:LICENSE'` devuelve
**tres** archivos:

| Ruta | Bytes | Licencia |
|---|---|---|
| `LICENSE` (raíz) | 1.124 | **MIT** |
| `src/model/pointcept/LICENSE` | 1.065 | MIT |
| 🔴 **`src/model/michelangelo/LICENSE`** | **35.148** | **GNU GPL-3.0** |

Y **no es código muerto vendorizado**: la cabecera del propio archivo lo declara,
y la cadena de imports es incondicional.

```
# This file is part of UniRig.
# This file is derived from https://github.com/NeuralCarver/Michelangelo
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License ...
```

```
src/model/parse_encoder.py:3:  from .michelangelo.get_model import get_encoder as get_encoder_michelangelo
src/model/parse_encoder.py:4:  from .michelangelo.get_model import AlignedShapeLatentPerceiver
src/model/unirig_ar.py:63:    isinstance(self.mesh_encoder, MAP_MESH_ENCODER.michelangelo) or ...
src/model/unirig_skin.py:258: isinstance(self.global_encoder, MAP_MESH_ENCODER.michelangelo_encoder)
```

**Confirmación aguas arriba:** el upstream nombrado en la cabecera,
[NeuralCarver/Michelangelo](https://github.com/NeuralCarver/Michelangelo)
(**486 ★**), es **GPL-3.0 también en su badge SPDX** — o sea que UniRig no
malinterpretó nada: vendorizó correctamente un proyecto GPL y **lo declaró en el
archivo**, pero el badge del repo contenedor sigue diciendo MIT.

`parse_encoder.py` importa el subárbol GPL **en el import de módulo, sin
condición**, y es quien construye los encoders que usan tanto el modelo
autorregresivo (`unirig_ar`) como el de skinning (`unirig_skin`) — los dos
modelos que son UniRig. ⇒ **El badge MIT es correcto para el archivo de la raíz
y engañoso para el artefacto.** Un estudio que enlace UniRig dentro de una
herramienta propietaria está distribuyendo GPL-3.0.

**El 35.148 B es el checksum.** GPL-3.0 ≈ 35.200 B es una de las ocho entradas
de la tabla de checksum derivada en el cycle 4. Funcionó exactamente para lo que
fue construida: ningún humano leyó 35 KB, el tamaño lo delató.

### 📏 Lo que este pass NO puede medir, dicho explícitamente

- **La licencia de SMPL.** `GuyTevet/motion-diffusion-model` baja los body
  models desde un enlace de Google Drive sin autenticación
  (`prepare/download_smpl_files.sh`, verificado byte a byte) en lugar del portal
  de MPI, que tiene aceptación de licencia. **El host `smpl.is.tue.mpg.de` no
  resuelve desde esta sesión** (fuera de los dominios permitidos), así que
  **esta KB no afirma cuáles son los términos de SMPL**: afirma sólo lo que
  verificó — que el script **evita el portal donde se aceptan**. Gap declarado,
  pendiente para el pass siguiente con el dominio habilitado.
- **Los pesos publicados en Hugging Face.** Esta sesión sólo tiene canal a
  GitHub (`search/*` + `raw`). Donde el repo no trae un archivo de licencia de
  pesos, esta KB **no infiere**: lo deja en blanco. Es el caso de
  `Stability-AI/stable-audio-tools` (el código es MIT; los checkpoints se
  distribuyen aparte y **no se leyeron**).
- **Deltas de ★.** El pass anterior corrió hace ~1 hora. No se reportan
  variaciones de estrellas, por la misma razón que en los cycles 10-13.


## 🔴 Lo más importante del pass 2026-11-22 (cycle 13): **la capa de texto y voz que mira el jugador nunca se auditó en 12 passes — y vive bajo tres familias de licencia que esta KB no tenía**

Los doce passes anteriores trabajaron sobre el mismo eje: **engines, MCP, NPCs,
world models, backends**. Este pass lo mide antes de seguir, y el número es la
razón para cambiar de eje:

| Eje | Candidatos encontrados por las búsquedas del runbook | Ya estaban en esta KB | Nuevos |
|-----|------------------------------------------------------|------------------------|--------|
| Engine / MCP / NPC / nav (el eje de siempre) | 15 | **13** | 2 |
| Diálogo autorado, moderación, voz, localización | 24 | **0** | **24** |

⇒ **El eje de siempre está saturado: 13 de 15 ya estaban escritos.** La capa que
el jugador efectivamente *lee y escucha* —el texto del diálogo, el chat que hay
que moderar, la voz del NPC, el idioma en que se entrega— tenía **cero filas**
después de doce passes. Y no está vacía por falta de proyectos: está llena de
proyectos **grandes, vivos, y con licencias que esta KB no había visto nunca**.

> **Regla de método nº14 (para copiar a toda la familia de KBs): la saturación de
> un eje se mide, no se intuye.** Si la tasa de "ya estaba" de una búsqueda supera
> ~80%, el pass siguiente no debe refinar esa búsqueda: debe cambiar de eje. Un KB
> que repite su eje productivo converge a reescribir sus propias filas con
> ★ actualizadas, y eso se parece mucho a trabajar.

### 🆕 Hallazgo nº1: **la Yarn Spinner Public Licence (YSPL) 1.0** — el motor que elegís decide tu licencia, y hay una cláusula que regula **tus agentes de código**, no tu producto

`YarnSpinnerTool` es *un solo proyecto* con seis repos oficiales. Leídos byte a
byte, tienen **cuatro estados de licencia distintos**:

| Repo | ★ | Badge SPDX | **Licencia leída en bytes** | Bytes | Archivo / rama |
|------|---|-----------|------------------------------|-------|----------------|
| [YarnSpinnerTool/YarnSpinner](https://github.com/YarnSpinnerTool/YarnSpinner) | 2.876 | MIT | **MIT** ✅ | 1.138 | `LICENSE.md` @ `main` |
| [YarnSpinnerTool/YarnSpinner-Unity](https://github.com/YarnSpinnerTool/YarnSpinner-Unity) | 854 | MIT | **MIT** ✅ | 1.079 | `LICENSE.md` @ `main` |
| [YarnSpinnerTool/YarnSpinner-Godot](https://github.com/YarnSpinnerTool/YarnSpinner-Godot) | 218 | MIT | **MIT** ✅ | 1.104 | `LICENSE` @ **`master`** (no existe `main`) |
| [YarnSpinnerTool/YarnSpinner-Rust](https://github.com/YarnSpinnerTool/YarnSpinner-Rust) | 299 | Apache-2.0 | **MIT** (hay `LICENSE-MIT`; el badge eligió una de las dos ⇒ **dual**) | 1.091 | `LICENSE-MIT` @ `main` |
| [YarnSpinnerTool/YarnSpinner-Godot-GDScript](https://github.com/YarnSpinnerTool/YarnSpinner-Godot-GDScript) | 99 | `NOASSERTION` | 🔴 **YSPL 1.0** | 17.709 | `LICENSE.md` @ `main` |
| [YarnSpinnerTool/YarnSpinner-UnrealEngine](https://github.com/YarnSpinnerTool/YarnSpinner-UnrealEngine) | 52 | `NOASSERTION` | 🔴 **YSPL 1.0** | 17.709 | `LICENSE.md` @ `main` |

**La consecuencia operativa es directa sobre el stack recomendado de esta KB
(Godot + Nakama + MCP):** hay **dos** ports de Godot y **no comparten licencia**.
`YarnSpinner-Godot` (C#, 218 ★) es MIT; `YarnSpinner-Godot-GDScript` (99 ★) es
YSPL. Un equipo que trabaja en GDScript y elige "el port de Godot" por nombre
se lleva la licencia restrictiva sin enterarse.

**Qué dice la YSPL 1.0, leída en el operativo (no en el *Quick Summary*, que el
propio texto declara sin efecto legal):**

- ✅ **Hacer y vender un juego que la embebe: permitido.** Define `"Licensed
  Product"` como el producto final con contenido original propio.
- ✅ **Contratistas: permitidos y nombrados.** Define `"Contractor"` = *"any third
  party, including any contractor, freelancer, or porting service provider,
  engaged by You to perform work on Your Licensed Product on Your behalf"*.
  ⇒ **Globant como integrador de un cliente está explícitamente contemplado.**
- ⛔ **Construir una herramienta de diálogo con ese código: prohibido.** Define
  `"Competing Product"` como cualquier tool/plugin/librería/servicio cuyo
  propósito primario sea habilitar a terceros a crear sistemas de diálogo
  ramificado, narrativa no lineal, editores de árboles de conversación, o *"a
  compiler, runtime, or interpreter for the Yarn language"*. ⇒ **un acelerador de
  diálogo reutilizable entre cuentas es exactamente lo prohibido.** La excepción
  escrita: una herramienta **interna de una sola Legal Entity, no distribuida**,
  no es Competing Product.
- ⛔ **Redistribuir el fuente sin modificar: prohibido.** Fork público con
  modificaciones genuinas: permitido, bajo la misma licencia.
- 🔴 **Y la cláusula nueva para esta KB:** *"Want to feed our code to an AI
  training system? Also no. You can use AI tools to help you build your game, but
  you must do your best to make sure those tools do not send our source code back
  for training purposes."*

> 🔴 **Es la primera licencia de esta KB que regula el TOOLING de Globant y no el
> producto de Globant.** Todas las restricciones catalogadas hasta acá acotaban
> *qué se puede enviar al cliente*. Ésta acota *con qué herramientas se puede
> trabajar el repo*: obliga a que los agentes de código que se pasen sobre ese
> árbol estén configurados sin retención/entrenamiento. Para un AI Studio que
> vende pipelines asistidos por agentes, es una condición de arquitectura, no una
> nota al pie — y es verificable antes de empezar (settings de retención del
> proveedor), no después.

### 🆕 Hallazgo nº2: **el grant puede vivir en el README** — `inkle/inky`, 2.758 ★, badge `null`, y sin un solo archivo de licencia

[`inkle/ink`](https://github.com/inkle/ink) (5.029 ★) es el runtime del lenguaje
**ink** — el motor narrativo detrás de *80 Days*, *Heaven's Vault* y una parte
grande de la narrativa indie. Su licencia es **MIT verificada en bytes**
(`LICENSE.txt` @ `master`, 1.067 B, *"Copyright (c) 2025 inkle Ltd."*).

Su editor oficial, [`inkle/inky`](https://github.com/inkle/inky) (2.758 ★), es
otra historia. Badge SPDX: **`null`**. Sondas sobre `master` y `main` para
`LICENSE`, `LICENSE.md`, `LICENSE.txt`, `License.txt`, `license.txt`, `LICENCE`,
`COPYING` y `NOTICE`: **404 en las ocho**. Por la regla vigente de esta KB eso se
publicaba como *gap declarado*.

**No es un gap. El grant está en el `README.md`, con el cuerpo completo:**

```
## License
**Inky** and **ink** are released under the MIT license. [...]
### The MIT License (MIT)
Copyright (c) 2016 inkle Ltd.
Permission is hereby granted, free of charge, to any person obtaining a copy...
```

> **📐 Regla de método nº15 — 8º testigo de la lista de sondas: `README.md`,
> sección *License*. Y la distinción que la hace utilizable:**
> - un README que **nombra** una licencia (*"MIT licensed"*) es una **afirmación**:
>   no otorga nada, y se registra como gap igual que antes;
> - un README que **contiene el cuerpo verbatim + el titular del copyright + el
>   año** es un **grant**: es el mismo texto que estaría en el archivo, en otro
>   archivo.
>
> `inky` es el segundo caso: cuerpo completo, titular (*inkle Ltd.*) y año (2016).
> ⇒ **se resuelve como MIT, no como gap.** El badge `null` de GitHub sólo dice que
> el detector busca archivos, no prosa.

⚠️ Nota de estado, que es una cosa distinta de la licencia: `inky` lleva **sin
commits desde 2026-05-05** (~6,5 meses). El runtime `ink` sí está vivo
(2026-11-16). Se recomienda `ink` como runtime; el editor, con la advertencia.

### 🆕 Hallazgo nº3: `meta-llama/PurpleLlama` — **el padre es MENOS libre que sus hijos**, y hay un 9º eje de partición del grant

El pass cycle 12 estableció que *"el derivado puede ser MENOS libre que su
padre"*. Este pass mide el caso **inverso**, y es el que cuesta plata.

`meta-llama/PurpleLlama` (4.488 ★, badge `NOASSERTION`) tiene en la raíz un
`LICENSE` de **7.669 B** que es la **LLAMA 3.2 COMMUNITY LICENSE AGREEMENT** — no
es open source. Leer sólo la raíz ⇒ **rechazar el repo entero**. Eso es lo que
habría hecho esta KB.

Con el canal de **enumeración** (`gh api search/code -f q='repo:<slug>
filename:LICENSE'`) aparecen **14 archivos de licencia** y **cinco grants
distintos**:

| Ruta | Bytes | Grant leído | ¿Usable por Globant? |
|------|-------|-------------|----------------------|
| `LICENSE` (raíz) | 7.669 | Llama 3.2 Community | ⛔ no |
| `CodeShield/LICENSE` | 1.088 | **MIT** | ✅ **sí** |
| `LlamaFirewall/LICENSE` | 1.088 | **MIT** | ✅ **sí** |
| `CybersecurityBenchmarks/LICENSE` | 1.088 | **MIT** | ✅ **sí** |
| `SensitiveDocClassification/LICENSE` | 1.060 | **MIT** (*"Copyright (c) 2025 Meta"*) | ✅ **sí** |
| `Llama-Guard/LICENSE`, `Llama-Guard2/LICENSE` | 6.753 | Llama **2** Community | ⛔ no |
| `Llama-Guard3/1B/`, `/8B/`, `/11B-vision/LICENSE` | 7.668 | Llama **3.2** Community | ⛔ no |
| `Llama-Guard4/12B/LICENSE` | 7.553 | Llama **4** Community | ⛔ no |
| `Llama-Prompt-Guard-2/86M/`, `/22M/LICENSE` | 7.553 | Llama **4** Community | ⛔ no |
| `CybersecurityBenchmarks/website/docs/LICENSE.md` | 12.457 | Meta Llama **3** Community | ⛔ no (es doc del sitio) |

Dos conclusiones, las dos nuevas:

1. 🟢 **Se rescatan cuatro herramientas MIT que el rechazo por raíz habría
   tirado** — entre ellas **LlamaFirewall**, que es exactamente una capa de
   guardrail para agentes (prompt injection, salidas inseguras) y entra en el
   stack de moderación de abajo. **El código es MIT; los pesos no lo son.** La
   línea no corre entre repos: corre **dentro** del repo, entre `/` (modelo) y
   `/<tool>/` (código).
2. 🔴 **9º eje de partición de un grant: POR TAMAÑO DE MODELO.** `Llama-Guard3/1B`
   y `Llama-Guard3/8B` comparten grant (Llama 3.2), pero `Llama-Guard4/12B` y
   `Llama-Prompt-Guard-2/22M` están bajo **Llama 4 Community**, que es otro
   documento. ⇒ **elegir el checkpoint chico para moderación on-device y el
   grande para servidor es elegir dos generaciones de licencia distintas en el
   mismo sprint.** Ningún eje anterior de esta KB (por repo, por organización,
   por subdirectorio, por dependencia vendorizada, por rama, por producto, por
   región, por audiencia) cubría esto.

### 🔴 Hallazgo nº4: **4º defecto medido del checksum de bytes** — el SPDX short-form da falso negativo

El checksum de bytes de esta KB (MIT≈1.100 / Ms-PL≈3.900 / Apache≈11.350 /
MPL≈16.700 / CC-BY≈18.650 / LGPL2.1≈26.400 / AGPL≈34.500 / GPL3≈35.200) tenía
tres defectos documentados. Éste es el cuarto, y lo cazó un repo grande:

[`NVIDIA-NeMo/Guardrails`](https://github.com/NVIDIA-NeMo/Guardrails) — 7.544 ★,
badge **`NOASSERTION`**, `LICENSE.md` de **654 bytes**. 654 no se parece a ningún
valor de la tabla ⇒ el checksum dice *"licencia rara, probablemente bespoke"*.
**Es Apache-2.0.** El archivo completo es un *short-form*:

```
SPDX-FileCopyrightText: Copyright (c) 2023 NVIDIA CORPORATION & AFFILIATES.
SPDX-License-Identifier: Apache-2.0

Licensed under the Apache License, Version 2.0 (the "License"); ...
```

> **📐 Regla de método nº16 — corrección al checksum:** si el archivo pesa
> **< 1.000 B** y contiene la cadena `SPDX-License-Identifier:`, **el
> identificador manda y el conteo de bytes no aplica**. El short-form SPDX es un
> grant por referencia, no un grant por transcripción; medir su longitud es medir
> la referencia, no la licencia. Verificado en las dos ramas (`main` y `develop`:
> 654 B idénticos).
>
> ⚠️ Y el rename que lo acompaña: **`NVIDIA/NeMo-Guardrails` → `NVIDIA-NeMo/Guardrails`**.
> El slug viejo da **HTTP 422** en `search/repositories`. Cualquier documento que
> cite el slug viejo cita un repo que ya no responde a la API.

### 🔴 Hallazgo nº5: **`fishaudio/fish-speech` — una licencia nueva (13-oct-2026) cuyo umbral se evalúa sobre el CLIENTE, no sobre Globant**

[`fishaudio/fish-speech`](https://github.com/fishaudio/fish-speech) — 33.825 ★,
badge `NOASSERTION`, vivo (2026-11-14). `LICENSE` = **19.685 B**: *"FISH AUDIO
COMMUNITY LICENSE AGREEMENT, Version 1.0 — Last Updated: October 13, 2026"*.
Familia de licencia **nueva en esta KB**: *umbral por facturación*.

Lo que dice, en el orden en que le pega a un AI Studio:

- Investigación y uso no comercial: **gratis**.
- Uso comercial: **permitido sólo mientras tu `Annual Revenue` sea < US$ 500.000**
  (12 meses móviles, **agregando afiliadas**, todas las fuentes de ingreso, no
  sólo las derivadas del software).
- 🔴 **La cláusula que apunta al integrador:** *"If You deploy, install, host, or
  operate the Fish Audio Materials [...] for a particular third party [...] **(for
  example, as a contractor, integrator, consultant, or on-premises vendor)**, the
  license [...] extends to that work only if **that third party's Annual Revenue**
  [...] is also less than the Revenue Threshold."*
  ⇒ **El umbral se evalúa sobre el cliente.** Un estudio con más de US$ 500k de
  facturación —es decir, cualquier cuenta objetivo de esta KB— **no tiene licencia**,
  y el integrador tampoco la tiene *por* él.
- ✅ **Dos salidas escritas, las dos utilizables:**
  1. **Entregar sólo el output.** *"delivering to a third party only output (such
     as audio files) that You generate Yourself, for example as an agency, studio,
     or freelancer producing voiceovers, dubbing, or audiobooks for a client"* —
     **no** está sujeto al umbral del cliente. Modelo *servicio de doblaje*, no
     *entrega de software*.
  2. **Producto propio generalmente disponible** cuyos usuarios no reciben copia
     de los materiales.
- ⛔ **`Hosted Service` nunca está licenciado**, sin importar facturación: ninguna
  API/SDK/web cuyo propósito primario sea TTS o clonado de voz.
- ✅ **Pero los NPCs están carve-out EXPLÍCITO del `Hosted Service`:** *"generating
  speech for content that You create, select, or control, or that Your own
  product generates (for example, narration, dubbing, **game or app characters**,
  or an assistant's spoken replies)"* **no es** Hosted Service.
  ⇒ **Para un juego, el bloqueo no es la cláusula de hosting: es el umbral de
  facturación del cliente.** La licencia nombra "game characters" y los deja pasar.
- Atribución obligatoria: archivo `Notice` con texto verbatim + *"Built with Fish
  Audio"* visible en sitio/UI/docs.
- Cláusula de `Feedback`: licencia perpetua, irrevocable y sublicenciable sobre
  cualquier comentario que mandes.

> 🔴 **Segunda licencia de esta KB que restringe a Globant y no al cliente** — la
> primera fue `defold/defold` (cycle 7). Pero ésta invierte el mecanismo: defold
> restringe *lo que hace el integrador*; fish-speech **mide la facturación del
> cliente y se la aplica al integrador**. La due diligence deja de ser "¿qué
> licencia tiene el repo?" y pasa a ser "**¿cuánto factura la cuenta?**" — un dato
> que el área comercial ya tiene y que legal nunca le pidió.

### 🔴 Hallazgo nº6: **Piper se relicenció de MIT a GPL-3.0 al cambiar de repo** — y el repo MIT está archivado

El TTS open source más citado para voz de NPC en CPU:

| Repo | ★ | Licencia (byte-read) | Bytes | Estado |
|------|---|----------------------|-------|--------|
| [`rhasspy/piper`](https://github.com/rhasspy/piper) | 11.336 | MIT (`LICENSE.md`) | 1.071 | 🔴 **ARCHIVADO** — último push 2025-08-26 |
| [`OHF-Voice/piper1-gpl`](https://github.com/OHF-Voice/piper1-gpl) | 6.347 | 🔴 **GPL-3.0** (`COPYING`) | 35.148 | ✅ vivo — push 2026-11-21 |

El repo sucesor **lo dice en el nombre**. ⇒ cualquier documento de arquitectura
que diga *"Piper, MIT"* está citando **un repo archivado hace 15 meses**; el que
se instala hoy es **copyleft fuerte**. Para TTS embebido en un cliente de juego
distribuido, GPL-3.0 es un bloqueo, no una molestia.

⚠️ Y el vecino: [`coqui-ai/TTS`](https://github.com/coqui-ai/TTS) — 46.266 ★,
**MPL-2.0** (permisiva por archivo), **no archivado**… y **sin un commit desde
2024-08-16** (27 meses). Es el patrón `0ad` otra vez: **`archived=false` no
significa vivo.** Se mide con `pushed_at`, no con el flag.

### 🆕 Filas nuevas de este pass — 16 repos, los 16 con licencia leída byte a byte

#### Diálogo autorado y narrativa

| Agente / repo | ★ | Licencia (byte-read) | Qué hace |
|---------------|---|----------------------|----------|
| [inkle/ink](https://github.com/inkle/ink) | 5.029 | **MIT** (1.067 B) | Lenguaje + runtime de narrativa ramificada. El estándar de facto indie. Integraciones Unity/Godot/web. |
| [YarnSpinnerTool/YarnSpinner](https://github.com/YarnSpinnerTool/YarnSpinner) | 2.876 | **MIT** (1.138 B) | Compilador y runtime del lenguaje Yarn: diálogo como guion, con variables y comandos. |
| [inkle/inky](https://github.com/inkle/inky) | 2.758 | **MIT en el README** (sin archivo) | Editor de escritorio para *ink*. ⚠️ sin commits desde 2026-05-05. |
| [YarnSpinnerTool/YarnSpinner-Unity](https://github.com/YarnSpinnerTool/YarnSpinner-Unity) | 854 | **MIT** (1.079 B) | Integración oficial Unity. |
| [YarnSpinnerTool/YarnSpinner-Rust](https://github.com/YarnSpinnerTool/YarnSpinner-Rust) | 299 | **MIT** (dual con Apache; 1.091 B) | Runtime Rust — la vía para Bevy. |
| [YarnSpinnerTool/YarnSpinner-Godot](https://github.com/YarnSpinnerTool/YarnSpinner-Godot) | 218 | **MIT** (1.104 B, rama `master`) | Port Godot **C#**. ✅ el que hay que usar. |
| [YarnSpinnerTool/YarnSpinner-Godot-GDScript](https://github.com/YarnSpinnerTool/YarnSpinner-Godot-GDScript) | 99 | 🔴 **YSPL 1.0** (17.709 B) | Port Godot **GDScript**. ⚠️ licencia restrictiva: ver hallazgo nº1. |
| [YarnSpinnerTool/YarnSpinner-UnrealEngine](https://github.com/YarnSpinnerTool/YarnSpinner-UnrealEngine) | 52 | 🔴 **YSPL 1.0** (17.709 B) | Port Unreal. ⚠️ misma licencia. |

#### Moderación, guardrails y seguridad de contenido — la capa que faltaba entera

| Agente / repo | ★ | Licencia (byte-read) | Qué hace |
|---------------|---|----------------------|----------|
| [guardrails-ai/guardrails](https://github.com/guardrails-ai/guardrails) | 7.635 | **Apache-2.0** (11.357 B ✅ checksum) | Framework de validadores sobre salidas de LLM: estructura, PII, toxicidad, política propia. |
| [NVIDIA-NeMo/Guardrails](https://github.com/NVIDIA-NeMo/Guardrails) | 7.544 | **Apache-2.0** (SPDX short-form, 654 B) | Rieles conversacionales programables (Colang): acota de qué habla un NPC y qué no. |
| [meta-llama/PurpleLlama → `LlamaFirewall/`](https://github.com/meta-llama/PurpleLlama) | 4.488 (repo) | **MIT** (1.088 B) | Guardrail para agentes: prompt injection, acciones inseguras. **El código es MIT; los pesos NO.** |
| [meta-llama/PurpleLlama → `CodeShield/`](https://github.com/meta-llama/PurpleLlama) | 4.488 (repo) | **MIT** (1.088 B) | Filtro de código inseguro generado por LLM — aplica al pipeline de dev, no al juego. |
| [unitaryai/detoxify](https://github.com/unitaryai/detoxify) | 1.320 | **Apache-2.0** (11.357 B ✅ checksum) | Clasificador de toxicidad entrenado (multilingüe). Corre local, sin API. Vivo 2026-11-11. |

#### Voz de NPC — STT y TTS

| Agente / repo | ★ | Licencia (byte-read) | Qué hace |
|---------------|---|----------------------|----------|
| [openai/whisper](https://github.com/openai/whisper) | 113.247 | **MIT** (1.063 B) | STT de referencia. Entrada de voz del jugador. |
| [ggml-org/whisper.cpp](https://github.com/ggml-org/whisper.cpp) | 55.409 | **MIT** (1.078 B) | Whisper en C/C++: corre en el cliente, sin servidor. Vivo 2026-11-20. |
| [SYSTRAN/faster-whisper](https://github.com/SYSTRAN/faster-whisper) | 26.929 | **MIT** (1.064 B) | Whisper con CTranslate2: ~4× más rápido, mismo modelo. Para el lado servidor. |
| [SWivid/F5-TTS](https://github.com/SWivid/F5-TTS) | 15.633 | **MIT** (1.068 B) | TTS con clonado por *flow matching*. 🟢 **La alternativa permisiva a fish-speech.** |

#### Localización

| Repo | ★ | Licencia (byte-read) | Nota |
|------|---|----------------------|------|
| [i18next/i18next](https://github.com/i18next/i18next) | 8.666 | **MIT** (1.082 B) | Framework de i18n. La capa de *runtime* del idioma. |
| [WeblateOrg/weblate](https://github.com/WeblateOrg/weblate) | 6.178 | ⚠️ **GPL-3.0** | Plataforma de traducción continua. **Se usa como servicio, no se embebe** ⇒ utilizable. |

### ⛔ Rechazos **medidos** de este pass — se publican porque el silencio se lee como cobertura

| Candidato | ★ | Motivo medido |
|-----------|---|---------------|
| [fishaudio/fish-speech](https://github.com/fishaudio/fish-speech) | 33.825 | Fish Audio Community License 1.0 (19.685 B): umbral de **US$ 500k evaluado sobre el cliente**. ⛔ para entrega de software; ✅ sólo para entrega de output. |
| [OHF-Voice/piper1-gpl](https://github.com/OHF-Voice/piper1-gpl) | 6.347 | **GPL-3.0** (35.148 B) — copyleft fuerte sobre un binario que se distribuye al jugador. |
| [coqui-ai/TTS](https://github.com/coqui-ai/TTS) | 46.266 | MPL-2.0 ✅, pero **27 meses sin commits**. Licencia buena, proyecto muerto. |
| [rhasspy/piper](https://github.com/rhasspy/piper) | 11.336 | MIT ✅ pero **`archived=true`** desde 2025-08-26. |
| `meta-llama/PurpleLlama` → **todos los pesos** `Llama-Guard*`, `Llama-Prompt-Guard-2*` | — | Llama 2 / 3.2 / 4 Community (6.753 / 7.668 / 7.553 B): no open source. **Sólo el código MIT se admite.** |
| [HIllya51/LunaTranslator](https://github.com/HIllya51/LunaTranslator) | 14.258 | **GPL-3.0**. Herramienta de traducción de juegos enorme y viva (2026-11-21), pero no embebible. ✅ como tool de escritorio del equipo de loc. |

### 🔧 Mapa de instrumentos de ESTA sesión — y un defecto en la sonda que el propio runbook prescribe

El runbook de ingesta dice *"verificá cada URL con `curl -sI`"*. **En esta sesión
esa sonda devuelve un falso positivo para todo**, y se demuestra con un slug
inventado:

```
$ curl -sI https://github.com/definitely-not-a-real-owner-zz9/definitely-not-a-real-repo-zz9
HTTP/1.1 200 Connection Established     <-- ❌ ESTO ES EL PROXY, NO GITHUB

HTTP/1.1 403 Forbidden                  <-- esto es el origen
```

**Hay DOS líneas de estado.** La primera es la respuesta `CONNECT` del proxy de
la sesión y vale 200 para cualquier host alcanzable, exista el repo o no.
Cualquier verificación que lea `head -1` —y es lo natural— **promueve 404s a
"verificado"**. El origen, además, responde **403 tanto a lo real como a lo
falso**, así que `curl` no distingue ni leyendo la segunda línea.

| Canal | Estado en esta sesión | Qué entrega |
|-------|------------------------|-------------|
| `curl -sI https://github.com/<slug>` | ❌ **inservible** | 200 del proxy + 403 del origen, para repos reales e inventados |
| `gh api repos/<owner>/<repo>` | ❌ **403** | *"GitHub access to this repository is not enabled for this session"* |
| `gh api repos/<o>/<r>/contents/<path>` | ❌ **403** | ídem |
| ✅ `gh api -X GET search/repositories -f q='repo:<slug>'` | ✅ **funciona** | `total_count` + ★ + SPDX + `pushed_at` + `archived`. **Sustituye al endpoint bloqueado.** |
| ✅ `gh api -X GET search/code -f q='repo:<slug> filename:LICENSE'` | ✅ **funciona** | enumeración completa (14 rutas en PurpleLlama) |
| ✅ `raw.githubusercontent.com/<slug>/<rama>/<archivo>` | ✅ **funciona** | el grant en bytes; 404 limpio cuando no está |
| `WebFetch` sobre hosts generales | ❌ **ENOTFOUND** | `github.blog` no resuelve: fuera de los dominios permitidos |

> **📐 Regla de método nº17: la prueba de existencia de un repo es
> `search/repositories -f q='repo:<slug>'` → `total_count == 1`.** Control
> negativo ejecutado este pass: `repo:NVIDIA/NeMo-Guardrails` (slug viejo, real
> hasta el rename) → **HTTP 422**; `repo:Textractor/Textractor` (slug mal
> escrito) → **HTTP 422**. La sonda distingue; `curl -sI` no.

> **📐 Regla de método nº18: la rama por defecto no siempre es `main`, y `raw` no
> siempre aliasea.** El cycle 7 midió que `raw` resuelve `master` → `main` en
> `Unity-Technologies/ml-agents`. **`YarnSpinnerTool/YarnSpinner-Godot` es el
> contraejemplo**: no tiene `main`, y sondear sólo `main` devolvió *"sin archivo
> de licencia"* sobre un repo **que es MIT en `master`**. ⇒ el aliasing es por
> repo, no del canal. **Sondear siempre `main` y `master` antes de declarar un gap.**

---

## 🔴 Lo más importante del pass 2026-11-22 (cycle 12): **la organización no es el repo, el repo no es el producto, y el derivado puede ser MENOS libre que su padre**

Los once passes anteriores aprendieron a leer *una licencia*. Este pass mide tres
casos donde la licencia estaba bien leída y **la unidad de análisis estaba mal
elegida**. En los tres, la enumeración (canal 3) encuentra lo que la sonda no
podía ni preguntar.

> **Canales de esta sesión — medidos, no supuestos:**
> 1. `gh api search/repositories -f q='repo:<slug>'` → ✅ **funciona**. Existencia,
>    SPDX, ★, `archived`, `pushed_at`, `forks`, `default_branch`.
> 2. `raw.githubusercontent.com/<slug>/<rama>/<archivo>` → ✅ **funciona**. El
>    grant en bytes.
> 3. `gh api search/code -f q='repo:<slug> filename:LICENSE'` → ✅ **funciona**.
>    Controles positivos: `TaloDev/backend`→1, `godotengine/godot`→3,
>    `filename:LICENSE godot`→5.632. Control negativo de (1):
>    `repo:gmilano/control-does-not-exist-xyz-999` → HTTP 422. ✅
>
> ⛔ **Caídos en esta sesión** (gap declarado, no silenciado):
> `gh api repos/<owner>/<repo>` → **HTTP 403** (el scope de GitHub de esta sesión
> cubre `search/*` y `raw`, no `repos/*`). `curl -sI https://github.com/<slug>`
> → devuelve `HTTP/1.1 200 Connection Established` **del proxy**, no del repo:
> sigue sin servir como prueba. `huggingface.co` → **no resuelve**
> (`curl` = 000, WebFetch = `ENOTFOUND`, dos sondas independientes).

---

### 🔴 Hallazgo nº1: `QwenLM/Qwen3` — 27.758 ★, 2.113 forks, y **cero archivos de licencia en el repo**

NVIDIA publicó que **ACE ya soporta Qwen3-8B como SLM on-device** para NPCs en
juegos de PC. Es decir: el modelo abierto que el stack de NPC más citado de la
industria te invita a correr **dentro del juego, en la máquina del jugador**.
Esta KB nunca lo había listado como fundacional.

Medición del grant, con los tres canales:

| Canal | Resultado |
|---|---|
| Badge SPDX (`search/repositories`) | **`NONE`** |
| `raw` × 3 ramas (`main`/`master`/`dev`) × 3 nombres (`LICENSE`, `LICENSE.txt`, `Qwen3_LICENSE`) | **9 × HTTP 404** |
| 🆕 Enumeración (`search/code … filename:LICENSE`) | **`total_count` = 0** |

No es que la sonda tuviera mal el nombre: **no hay ningún archivo de licencia en
el repositorio.** Y sin embargo el grant existe. Está en el `README.md`,
líneas 395-398, verbatim:

```
## License Agreement

All our open-weight models are licensed under Apache 2.0.
You can find the license files in the respective Hugging Face repositories.
```

> **📐 Regla de método nº14 (nueva, para copiar a toda la familia de KBs):
> cuando la enumeración devuelve CERO archivos de licencia, el repo no está
> necesariamente sin grant — hay que leer el `README`.** El grant puede estar
> *declarado* en GitHub y *alojado* en otro lado. Escribir "sin licencia" ahí es
> un falso negativo, y es el error más caro que puede cometer esta KB: descarta
> un fundacional de 27.758 ★ que sí se puede usar.

⚠️ **Lo que este pass NO puede afirmar:** que los archivos de Hugging Face digan
Apache-2.0. `huggingface.co` no resuelve en esta sesión (dos sondas). Lo
verificable hoy es que **el propietario declara Apache-2.0 para los pesos
abiertos, en su propio repo**. Para producción: leer la *model card* del tamaño
exacto que se embarque (8B ≠ 235B ≠ los derivados `-Instruct`), porque esta KB ya
documentó que **el grant se parte por variante de modelo**.

---

### 🔴 Hallazgo nº2: `LumioGames/LGUI` — **PolyForm Shield**, familia de licencia nueva en esta KB, y el padre MIT escondido adentro

`LumioGames/LGUI` (148 ★, 21 forks, push 2026-11-19) se presenta como *"Game UI
framework for AI agents"*. Badge: `NOASSERTION`. Enumeración: **8 archivos de
licencia**. Leídos todos, byte a byte:

| Ruta | Bytes | Grant |
|---|---|---|
| `LICENSE` (raíz) | 8.363 | 🟡 **PolyForm Shield 1.0.0 + Additional Permission** |
| `plugin/LICENSE` | 8.363 | 🟡 el mismo |
| `plugin/editor/LICENSE` | 1.092 | 🟢 **MIT** — *FairyGUI Editor Online* |
| `scripts/fairygui-dom/upstream/LICENSE.txt` | 1.086 | 🟢 **MIT** — *FairyGUI, 2021* |
| `plugin/framework/backends/unity/package/FairyGUI/LICENSE` | 1.099 | 🟢 **MIT (MIT)** — *fairygui.com, 2015* |
| `plugin/framework/backends/h5/vendor/FairyGUI-LICENSE.txt` | — | 🟢 MIT (familia FairyGUI) |
| `plugin/framework/backends/unity/package/LICENSE` | — | 🟢 MIT (familia FairyGUI) |
| `tests/fixtures/unity/puerts-3.0.3/LICENSE.txt` | 3.473 | 🟢 **BSD 3-Clause** detrás de preámbulo corporativo de Tencent |

**La cláusula que decide, citada:**

> *"**Your own products.** You may use, change and distribute the software as
> part of your own games, applications, websites and other products, including
> commercial ones … **as long as the product itself is not a UI framework, UI
> toolkit, UI editor or game development tool.**"*
>
> *"**What competes.** … a product competes with the software if it is a UI
> framework, UI toolkit, UI editor, UI asset pipeline **or game development
> tool** …"*

> 🔴 **Traducción al negocio de Globant, y es una línea que esta KB no tenía:**
> el **mismo repo** es 🟢 **verde para un juego de cliente** y 🔴 **rojo para un
> acelerador reutilizable de Globant**. Si LGUI entra en un entregable de
> engagement, está permitido y es comercial. Si entra en el *toolkit interno que
> se revende de cuenta en cuenta*, es exactamente lo que la licencia llama
> *"game development tool"* y hay que pedir licencia aparte. **El eje ya no es
> la licencia ni la región: es el TIPO DE ENTREGABLE.**

**Y la salida, que sólo aparece por enumeración:** LGUI es un envoltorio
PolyForm sobre **FairyGUI, que es MIT**. El padre está vivo y listado:

| Repo padre | ★ | Licencia | Push |
|---|---|---|---|
| [fairygui/FairyGUI-unity](https://github.com/fairygui/FairyGUI-unity) | **2.978** | 🟢 **MIT** | 2026-02-03 ⚠️ ~9 meses |
| [fairygui/FairyGUI-cocoscreator](https://github.com/fairygui/FairyGUI-cocoscreator) | 627 | 🟢 **MIT** | 2024-08-05 ⚠️ dormido |
| [fairygui/FairyGUI-Editor](https://github.com/fairygui/FairyGUI-Editor) | 802 | 🔴 **sin grant** (`NONE`) | 2024-03-11 ⚠️ |

> 📐 **Regla de método nº15: la regla "el LICENSE de un derivado delata a sus
> padres" también corre al revés.** Esta KB la usaba para *sospechar* de un
> derivado permisivo. Aquí sirve para lo contrario: **el derivado es MÁS
> restrictivo que el padre, y la enumeración encuentra el padre permisivo
> adentro.** Antes de aceptar un grant condicional, enumerar: el componente que
> de verdad se necesita puede estar vendorizado con licencia mejor.
>
> ⚠️ Matiz obligatorio: FairyGUI-unity lleva ~9 meses sin push y el **editor no
> tiene grant**. Tomar el padre es más libre pero **menos mantenido**, y el
> editor — la pieza que hace usable al framework — **no está licenciada**.

---

### 🔴 Hallazgo nº3: corrección a esta KB — **"Talo: MIT completo, sin tier cerrado" necesita un alcance**

`repos/foundations.md` y `verticals/solutions.md` recomiendan Talo desde hace
varios passes y afirman *"MIT completo, sin tier cerrado"*. Esta KB listaba
**1 de los 5 repos** de la organización. Enumerados los cinco:

| Repo | ★ | Licencia (bytes) | Archivos de licencia (enumerados) | Push |
|---|---|---|---|---|
| [TaloDev/godot](https://github.com/TaloDev/godot) | **251** | 🟢 **MIT** (1.074) | 4 — todos MIT | 2026-11-16 |
| [TaloDev/backend](https://github.com/TaloDev/backend) | 105 | 🟢 **MIT** (1.074) | 1 | 2026-11-21 |
| [TaloDev/unity](https://github.com/TaloDev/unity) | 43 | 🟢 **MIT** (1.074) | 2 | 2026-11-16 |
| [TaloDev/hosting](https://github.com/TaloDev/hosting) | 36 | 🔴 **SIN GRANT** | **0** | 2026-10-14 |
| [TaloDev/frontend](https://github.com/TaloDev/frontend) | 21 | 🟢 **MIT** (1.074) | 1 | 2026-11-17 |

Dos correcciones concretas:

1. **El repo más estrellado de Talo no es el backend: es el plugin de Godot**
   (251 ★ vs 105 ★, 2,4×). La puerta de entrada real del producto es el cliente,
   no el servidor — y era el que faltaba en la tabla.
2. 🔴 **`TaloDev/hosting` — el "self-hosting starter pack" — no tiene ningún
   archivo de licencia.** Cero por enumeración, no por sonda fallida. Es decir:
   el producto es MIT, pero **el camino de despliegue que el propio proyecto
   documenta no está otorgado.** Para un cliente que elige Talo *precisamente
   por self-hosting*, el gate no está en el producto: está en el runbook.
   **Mitigación:** usar `TaloDev/backend` (MIT, que sí cubre el servidor) con IaC
   propia, y no copiar `hosting/`.

`TaloDev/godot` vendoriza, todos MIT y todos leídos: `addons/talo/LICENSE`
(1.074), `addons/gdUnit4/LICENSE` (1.069, Mike Schulze 2023),
`addons/GDQuest_GDScript_formatter/LICENSE` (1.072, GDQuest). **Cadena limpia.**

---

### 📏 Ampliación del checksum de bytes de esta KB — tres valores medidos este pass

| Licencia | Bytes medidos | Nota |
|---|---|---|
| **BSD 3-Clause** (detrás de preámbulo Tencent) | **3.473** | 🆕 primera medición en esta KB. El preámbulo corporativo **no cambia** que el grant es permisivo — mismo patrón que la KB de enterprise documentó en Tencent/LoopForge. |
| **PolyForm Shield 1.0.0 + Additional Permission** | **8.363** | 🆕 familia nueva. Cae **entre** Apache-2.0 (~11.350) y MIT (~1.100): un lector que sólo mire el tamaño no la distingue de nada conocido. |
| **LGPL-3.0 "suplemento"** | **7.652** | Revalidado por enumeración: `search/code` devuelve **1** archivo en `thedivergentai/Game-Dev-Agentic-Skills` — el suplemento, sin la GPL-3.0 base (~35.200) que incorpora. El cycle 11 ya lo había visto por sonda; **ahora está probado por enumeración.** |

---

### 🆕 Agentes nuevos incorporados este pass — 4 filas, las 4 con licencia leída byte a byte y enumerada

| Agente / pack | Repo | Licencia (3 canales) | Qué es | ★ |
|---|---|---|---|---|
| **Talo — plugin Godot** | [TaloDev/godot](https://github.com/TaloDev/godot) | 🟢 **MIT** (1.074; 4 archivos, 4 MIT) | Cliente Godot de Talo: leaderboards, stats, auth, game saves, analytics. **El repo más estrellado de Talo y la puerta de entrada real.** Vendoriza gdUnit4 y el formatter de GDQuest, ambos MIT | **251** |
| **GodotPrompter** | [jame581/GodotPrompter](https://github.com/jame581/GodotPrompter) | 🟢 **MIT** (1.083; 1 archivo) | Gestión y ejecución de prompts **dentro del editor de Godot**. 63 forks. Cubre el hueco entre "MCP para el engine" y "pack de skills del agente": el prompt vive en el proyecto, versionado | **983** |
| **blender-game-skills** | [majidmanzarpour/blender-game-skills](https://github.com/majidmanzarpour/blender-game-skills) | 🟢 **MIT** (1.073; 1 archivo) | Skills de agente para el **pipeline de assets en Blender** orientado a juego. Par del `threejs-game-skills` del mismo autor que esta KB ya lista. ⚠️ push 2026-09-24 (~2 meses) | 195 |
| **LGUI** | [LumioGames/LGUI](https://github.com/LumioGames/LGUI) | 🟡 **PolyForm Shield 1.0.0 + Additional Permission** (8.363; **8 archivos**, 6 de ellos MIT/BSD vendorizados) | Framework de **UI de juego manejable por agentes**: pantallas y layouts generados. 🟡 **Condicional: verde en un juego de cliente, ROJO dentro de un acelerador reutilizable de Globant.** El padre MIT (`fairygui/FairyGUI-unity`, 2.978 ★) está vendorizado adentro | 148 |

---

### ⛔ Rechazos **medidos** de este pass — se publican porque el silencio se lee como cobertura

| Candidato | ★ | Medición | Motivo del rechazo |
|---|---|---|---|
| [Yuki001/game-dev-skills](https://github.com/Yuki001/game-dev-skills) | 123 | Badge `NONE`; `raw` 404 en `LICENSE` y `LICENSE.md`; **enumeración = 0**; `README.md` = 200 (5.774 B) **y no declara licencia** | 🔴 **Sin grant en ninguno de los tres canales.** A diferencia de Qwen3, acá la regla nº14 se aplicó y **no salvó al repo**: el README existe y no dice nada. Sin otorgamiento = todos los derechos reservados. **No usar.** |
| [anthill-platform/anthill](https://github.com/anthill-platform/anthill) | **4** | MIT real, pero `pushed_at` = **2018-10-05**, `created_at` = 2016, 2 forks, 0 issues | 🔴 Una guía de backends de 2026 lo describe como *"actively maintained"*. **El API dice 8 años sin un commit.** Caso 8º de *recomendación-por-blog vs medición*: la licencia estaba bien, la vitalidad era falsa. **No es alternativa a Nakama ni a Talo.** |
| [Cynthia-wlan9509/offline-game-revival](https://github.com/Cynthia-wlan9509/offline-game-revival) | 112 | 🟢 MIT (1.073), enumeración = 1, push 2026-11-16 | 🟡 **Licencia limpia, caso de uso no recomendable para engagement.** "Revive juegos móviles dados de baja como offline" toca servidores y binarios de terceros: el riesgo no es de licencia del repo, es de **derechos sobre el juego revivido**. Se mide y se lista para que no se vuelva a evaluar; no se recomienda. |

---

## 🔴 Lo más importante del pass 2026-11-22 (cycle 11): **la licencia NO se hereda de la organización** — la Farama Foundation publica MIT, GPL-2.0 y *nada* a la vez

Esta KB lista **cuatro** repos de la Farama Foundation
([Gymnasium](https://github.com/Farama-Foundation/Gymnasium) MIT,
[PettingZoo](https://github.com/Farama-Foundation/PettingZoo) MIT,
[MicroRTS-Py](https://github.com/Farama-Foundation/MicroRTS-Py) MIT y
[Minigrid](https://github.com/Farama-Foundation/Minigrid), cuyo badge es
`NOASSERTION` y **queda sin resolver en este pass**) y de ahí se
desprende, sin que nadie lo haya escrito nunca, una inferencia cómoda:
*"Farama = permisivo"*. Este pass midió los dos repos de esa fundación que la KB
**no** tenía, y la inferencia se cae:

| Repo de Farama | Canal API (SPDX) | Canal raw (lectura byte a byte) | Veredicto |
|---|---|---|---|
| [Gymnasium](https://github.com/Farama-Foundation/Gymnasium) | `MIT` | `LICENSE` 1.104 B | 🟢 **MIT** — usable |
| [PettingZoo](https://github.com/Farama-Foundation/PettingZoo) | `MIT` | `LICENSE` 1.073 B | 🟢 **MIT** — usable |
| 🆕 [Arcade-Learning-Environment](https://github.com/Farama-Foundation/Arcade-Learning-Environment) | **`GPL-2.0`** | — | 🔴 **COPYLEFT FUERTE.** 2.463 ★, vivo (push 2026-11-15) |
| 🆕 [ViZDoom](https://github.com/Farama-Foundation/ViZDoom) | **`null`** | **9 nombres × 3 refs (`HEAD`, `master`, `main`) = 27 sondas, las 27 en 404** | 🔴 **SIN GRANT RESOLUBLE.** 2.110 ★, push **hoy** |

🔴 **Por qué esto importa más que dos filas nuevas.** ALE es *el* banco de pruebas
canónico de RL sobre juegos — el de los papers de Atari — y ViZDoom es el de
shooters en primera persona. Un equipo que arma un pipeline de QA con agentes
llega a los dos por el camino natural (vienen recomendados en la documentación de
Gymnasium, que es MIT), y **los dos fallan la barra permisiva de Globant por
motivos distintos**: uno por GPL-2.0 declarado, el otro porque *no hay nada que
leer*. Ninguno de los dos se detecta mirando la organización.

> 🆕 **Regla de método nº18 — la licencia es una propiedad del repositorio, nunca
> de la cuenta que lo aloja.** Vale para fundaciones (Farama), para empresas
> (Tencent-Hunyuan, medido en el cycle 10) y para los monorepos partidos por
> carpeta (GDevelop, medido abajo). **Una fila heredada de una organización es una
> fila sin medir.**

### 🔴 El caso ViZDoom: **vivo y sin licencia no es lo mismo que muerto**

ViZDoom tiene commit **del día de hoy** y 2.110 ★. No es un proyecto abandonado:
es un proyecto activo cuyo grant **no se puede resolver por ninguno de los dos
canales que esta KB usa**. El badge del API dice `null` y el archivo no existe
bajo ningún nombre habitual, en ninguna de las tres referencias probadas.

| Sonda | Resultado |
|---|---|
| `gh api search/repositories -f q='repo:Farama-Foundation/ViZDoom'` | `license: null` |
| `raw.../HEAD/{LICENSE, LICENSE.md, license.md, LICENSE.txt, license.txt, COPYING, COPYING.txt, LICENCE, LICENSE-APACHE}` | 404 × 9 |
| idem sobre `master` y sobre `main` | 404 × 18 |

⚠️ **Lo que NO se puede concluir:** que ViZDoom sea propietario. El proyecto
envuelve el motor de *Doom* (1993), cuya cadena de licencias es conocida por ser
compleja. **Lo que sí se concluye, y es lo que va al entregable:** *no existe un
grant verificable mecánicamente*, así que **no se compone** hasta que alguien lea
la cadena completa a mano. Es el mismo veredicto operativo que un rechazo, con
una causa distinta — y la causa importa, porque es reversible.

### 🆕 Agentes nuevos incorporados este pass — 6 filas, las 6 con licencia leída byte a byte

| Agente / pack | Repo | Licencia (verificada) | Qué hace | ★ | Último push |
|---|---|---|---|---|---|
| 🆕 **Microverse** | [KsanaDock/Microverse](https://github.com/KsanaDock/Microverse) | **MIT** (`LICENSE`, 1.065 B) | Sandbox de simulación divina sobre **Godot 4** como **sistema de simulación social multi-agente**. Es el eslabón que faltaba entre `concordia` (poblaciones de NPC, pero sin engine) y los plugins de Godot (engine, pero un NPC por vez): acá la población **corre dentro del engine**. ⚠️ 7 meses sin push | **2.515** | 2026-04-10 |
| 🆕 **threejs-game-skills** | [majidmanzarpour/threejs-game-skills](https://github.com/majidmanzarpour/threejs-game-skills) | **MIT** (`LICENSE`, 1.073 B, © 2026) | Pack de **agent skills** para construir juegos de navegador en **three.js** jugables y terminados (game feel incluido). Es el par web de `godogen`: la capa de skills de esta KB estaba cubierta para Godot/Bevy/Unreal y **vacía para el navegador puro** | **2.873** | 2026-09-28 |
| 🆕 **Agentshire** | [Agentshire/Agentshire](https://github.com/Agentshire/Agentshire) | **MIT** (`LICENSE`, 1.085 B, © 2025-2026) | Plugin que **visualiza agentes AI como NPCs 3D en un pueblo**. Invierte la dirección habitual de esta KB: no es AI *dentro* de un juego, es un juego usado como **interfaz de observabilidad de una flota de agentes**. Para una demo de cliente, es la pieza que hace visible un sistema multi-agente | 1.317 | 2026-04-15 |
| 🆕 **GameDesignOS** | [DY-2026/GameDesignOS](https://github.com/DY-2026/GameDesignOS) | **MIT** (`LICENSE`, 1.065 B, © 2026) | *Local-first game design OS* para agentes: convierte sesiones de diseño en **evidencia y experimentos**. Cubre la etapa **preproducción**, que en esta KB estaba representada sólo por generadores de assets | 459 | 2026-08-17 |
| 🆕 **GameBlocks** | [xt4d/GameBlocks](https://github.com/xt4d/GameBlocks) | **MIT** (`LICENSE`, 1.069 B, © 2026) | Bloques de construcción concisos y autoexplicativos para que **un agente de código** prototipe juegos. El contrapunto minimalista de los packs de 49 agentes: cabe en una ventana de contexto | 434 | 2026-07-10 |
| 🆕 **Legends-Of-Heroes** | [FlameskyDexive/Legends-Of-Heroes](https://github.com/FlameskyDexive/Legends-Of-Heroes) | **MIT** (`LICENSE`, 1.076 B) | Juego de bolas estilo LoL **construido sobre agentes AI**, con soporte de agent skills. Valor como **referencia end-to-end**: un título jugable y completo cuya arquitectura es agéntica, no un demo de una sola mecánica | 939 | 2026-09-22 |

### ⛔ Rechazos **medidos** de este pass — se publican porque el silencio se lee como cobertura

| Repo | ★ | Causa medida | Por qué estaba en el camino |
|---|---|---|---|
| [art-from-the-machine/Mantella](https://github.com/art-from-the-machine/Mantella) | 427 | **AGPL-3.0** (`LICENSE`, **34.523 B** — el checksum AGPL~34.500 del cycle 4 vuelve a acertar) | Mod de **Skyrim y Fallout 4** con NPC conversacionales por voz. Esta KB ya lista `SkyrimNet-GamePlugin`; Mantella es el otro grande del nicho y **es el que no se puede tocar** |
| [GigaxGames/gigax](https://github.com/GigaxGames/gigax) | 346 | **Sin licencia de ningún tipo** (API `null` + sin archivo) **y 28 meses sin push** (2024-07-26) | *"LLM-powered NPCs running on your hardware"* — el título describe exactamente lo que un cliente pide. Doble descarte: sin grant y muerto |
| [SerpentAI/SerpentAI](https://github.com/SerpentAI/SerpentAI) | **6.987** | **MIT, pero ARCHIVADO** desde 2022-11-07 | *Game Agent Framework*. 🔴 **La trampa de estrellas altas del pass:** casi 7.000 ★ y licencia impecable sobre un repo que GitHub marca archivado hace 4 años. **El badge de licencia no dice nada sobre si el proyecto existe** |

---

## 🔴 Lo más importante del pass 2026-11-22 (cycle 10): **la línea de world models de Tencent no tiene licencia en EMEA — las tres, no una — y dentro de una de ellas hay una isla Apache-2.0**

> **Primer pass de un día nuevo** (el anterior fue el cycle 9, 2026-11-21). Como
> en los passes del 21, **no se reportan deltas de ★**: 24 h no mueven una
> estrella de forma interpretable. Lo que sí cambió en el intervalo son **hechos
> con fecha** (la Comisión Europea publicó un texto el 11-nov; el canal de
> licencias destapó una partición territorial sistemática) y eso es lo que trae
> este pass.
>
> **Auditoría mecánica de este pass: 159/159 slugs del inventario re-leídos** con
> `gh api search/repositories -f q='repo:<slug>'` (★ + SPDX + `archived` +
> `pushed_at` + `fork`). Resultado: **0 forks disfrazados de upstream**, 2
> archivados (ambos ya anotados: `0ad/0ad`, `openai/gym`), **4 slugs con 422** y
> **1 rename nuevo**. Ver el hallazgo nº3.

### 🔴 Hallazgo nº1: las **tres** releases abiertas de world models de Tencent excluyen EU, UK y Corea del Sur — y `HY-WorldPlay` enseña cómo salir

Esta KB ya tenía anotada la exclusión territorial de `HY-World-2.0` y de
`Hunyuan-GameCraft-1.0`. Este pass agrega el tercer repo de la línea y, al
enumerar sus archivos de licencia, encuentra que **la exclusión no es una
peculiaridad de un release: es la plantilla de la casa** — y que dentro del
mismo repo convive un subdirectorio genuinamente permisivo.

| Repo | ★ | SPDX de GitHub | Licencia **leída byte a byte** | Territorio |
|---|---|---|---|---|
| [Tencent-Hunyuan/HY-World-2.0](https://github.com/Tencent-Hunyuan/HY-World-2.0) | 2.901 | `NOASSERTION` | **Tencent HY-World 2.0 Community License** (`License.txt`, 16.858 B, release **15-abr-2026**) | ⛔ **no aplica en EU / UK / Corea del Sur** |
| [Tencent-Hunyuan/Hunyuan-GameCraft-1.0](https://github.com/Tencent-Hunyuan/Hunyuan-GameCraft-1.0) | 765 | `NOASSERTION` | **Tencent Hunyuan Community License** (`LICENSE`, 16.386 B, release **14-ago-2025**) | ⛔ idem |
| 🆕 [Tencent-Hunyuan/HY-WorldPlay](https://github.com/Tencent-Hunyuan/HY-WorldPlay) | 1.630 | `NOASSERTION` | **Tencent HY-WorldPlay Community License** (`License.txt`, 16.045 B, release **17-dic-2025**) | ⛔ idem |

La cláusula es literal y es la **primera línea** de los tres archivos:

```
THIS LICENSE AGREEMENT DOES NOT APPLY IN THE EUROPEAN UNION, UNITED KINGDOM
AND SOUTH KOREA AND IS EXPRESSLY LIMITED TO THE TERRITORY, AS DEFINED BELOW.
```

Y la definición que la hace operativa:

> *"**Territory** shall mean the worldwide territory, **excluding** the territory
> of the European Union, United Kingdom and South Korea."*

**Qué significa, sin eufemismos.** No es una licencia restrictiva en EMEA: es
**la ausencia de licencia** en EMEA. Fuera del Territory no hay grant — el
default legal es *todos los derechos reservados*. Un engagement europeo o
coreano que integre cualquiera de los tres **no tiene permiso de uso**, ni
siquiera para evaluar, y el badge `NOASSERTION` de GitHub no lo dice.

#### 🟢 La parte nueva y accionable: `HY-WorldPlay` tiene **tres** archivos de licencia, y uno es Apache-2.0

El canal de enumeración (`gh api search/code -f q='repo:<slug> filename:LICENSE'`)
devuelve `total_count: 3` para `HY-WorldPlay` — contra `1` para `HY-World-2.0`:

| Ruta dentro del repo | Bytes | Licencia | ¿Usable en EMEA? |
|---|---|---|---|
| `License.txt` (raíz) | 16.045 | Tencent HY-WorldPlay Community License | ⛔ **no** |
| 🟢 **`worldcompass/LICENSE`** | 10.757 | **Apache-2.0** (texto estándar) | ✅ **sí, sin restricción territorial** |
| `worldcompass/reward_function/HunyuanWorldMirror/License.txt` | 17.267 | Tencent HunyuanWorld-Mirror Community License (release 22-oct-2025) | ⛔ **no** |

Es decir: **el subárbol `worldcompass/` es Apache-2.0 de verdad, pero la función
de reward que vive dentro de él vuelve a caer bajo licencia Tencent con
exclusión territorial.** Tres niveles, tres grants, y el anidamiento va
permisivo → restrictivo → permisivo → restrictivo según la profundidad. Un
`grep` de "Apache" en la raíz del checkout no lo distingue.

> **Regla de método nº14 (nueva, para copiar a toda la familia de KBs): el
> territorio es un eje de partición del grant tan real como el directorio, y es
> el único que convierte una decisión técnica en una decisión de *dónde se
> vende*.** Esta KB ya tenía 7 ejes de partición (por directorio, por archivo,
> por rama, por componente, por dependencia vendorizada, por pesos-vs-código,
> por carve-out `ee/`). El territorio es el octavo, y es el primero que un
> arquitecto **no puede resolver leyendo el repo**: hay que saber en qué
> jurisdicción publica el cliente.

**El sustituto medido para EMEA.** `SkyworkAI/Matrix-Game` (2.951 ★) cubre el
mismo caso de uso — mundo interactivo jugable generado — y su grant **no tiene
cláusula territorial**: raíz `LICENSE` = **MIT** (1.118 B), `Matrix-Game-4/LICENSE`
= **Apache-2.0** (11.357 B). Ver la Receta 25 en `compose/patterns.md`.

### 🔴 Hallazgo nº2: `youichi-uda/godot-mcp-pro` — 649 ★, cabecera **MIT**, y el producto real es **de pago y propietario**

`gh api search/repositories` lo devuelve como `NOASSERTION`. El byte-read del
`LICENSE` (1.282 B) abre con `MIT License` — exactamente el patrón que el cycle 4
de esta KB bautizó *"licence rule inverted"*: un `NOASSERTION` que esconde un
grant permisivo. **Acá la inversión no se sostiene, y la prueba está en los
últimos 200 bytes del mismo archivo:**

```
NOTE: This license applies to the Godot editor plugin (addons/godot_mcp/).
The MCP server (TypeScript) is distributed separately under a proprietary
license as part of the paid Godot MCP Pro package.
```

El repo se anuncia como *"162 MCP tools for AI-powered Godot 4 development"*.
**Las 162 herramientas viven en el servidor MCP, y el servidor MCP no está
cubierto.** Lo que es MIT es el plugin del editor: el extremo tonto del cable.

> **Corrección a la regla del cycle 4.** Aquella decía: *"NOASSERTION es la
> ausencia de un veredicto, resolver por byte-read en ambos sentidos"*. Sigue
> siendo cierta, pero estaba incompleta: **el byte-read hay que hacerlo del
> archivo ENTERO, no de la cabecera.** Los 1.070 B del MIT canónico son un
> checksum útil; **1.282 B no es "MIT con un copyright largo", es MIT más algo**,
> y ese algo fue, en este caso, el modelo de negocio. Regla nº15: **si el conteo
> de bytes supera al canónico, leer la cola antes que la cabeza.**

Es el 2.º caso de *open-core en la capa de agentes* que encuentra esta KB (el
1.º fue el carve-out `ee/` de PostHog) y el 1.º donde **el núcleo cerrado es
justamente la funcionalidad que da fama al repo**.

### 🔴 Hallazgo nº3: `422` no distingue *borrado* de *renombrado* — pero combinado con `raw`, sí

La auditoría de los 159 slugs devolvió **4 con HTTP 422** (*"resources do not
exist or you do not have permission"*). Esta KB venía usando ese 422 como
control negativo de inexistencia. **Es insuficiente:** un repo renombrado
devuelve exactamente el mismo 422 que uno que nunca existió.

El discriminador es cruzar con el canal `raw`, que **sí sigue el redirect del
rename**:

| Slug del inventario | `search/repositories` | `raw.../HEAD/README.md` | Veredicto |
|---|---|---|---|
| 🆕 `kolbytn/mindcraft` | **422** | **200** | 🔄 **RENOMBRADO** (nuevo este pass) |
| `minetest/minetest` | **422** | **200** | 🔄 renombrado → `luanti-org/luanti` (ya anotado) |
| `rivet-gg/toolchain` | **422** | **200** | 🔄 renombrado (cadena de 3 saltos, ya anotada) |
| `pelican-dev/panel` | **422** | **404** (cuerpo de 14 B) | ⛔ **BORRADO** (ya anotado) |

> **Regla de método nº16: `422` + `raw 200` = se mudó, tu URL redirige y va a
> seguir redirigiendo hasta que alguien registre el slug viejo. `422` + `raw 404`
> = no está. Un 422 solo no autoriza a escribir "no existe".**

**El rename nuevo, resuelto:**

| Slug viejo (el que tenía esta KB) | Slug real | ★ | Licencia | Último push |
|---|---|---|---|---|
| `kolbytn/mindcraft` | 🆕 **[mindcraft-bots/mindcraft](https://github.com/mindcraft-bots/mindcraft)** | **6.039** | **MIT** | 2026-10-27 |

Mindcraft pasó de cuenta personal a **organización** — la misma transición que
`justinpbarnett → CoplayDev`, `googleforgames → agones-dev` y
`minetest → luanti-org`. **Cuarto caso del mismo patrón en esta KB: el proyecto
que funciona se institucionaliza, y el slug personal queda como redirect.** Es
un indicador de madurez, no un problema — pero una tabla que cita el slug viejo
envejece sin avisar, porque el redirect nunca falla.

### 🆕 Filas nuevas de este pass — 11 repos, **las 11 con licencia leída byte a byte**

| Agente / herramienta | Repo | Licencia (byte-read) | ★ | Último push | Para qué sirve |
|---|---|---|---|---|---|
| **utopia** | [deeplethe/utopia](https://github.com/deeplethe/utopia) | **Apache-2.0** (`LICENSE`, 11.353 B) | **8.745** | 2026-11-22 | World model abierto de escala "enterprise". **El repo de world models más grande que entra a esta KB** y, a diferencia de toda la línea Tencent, **sin cláusula territorial**: utilizable en EMEA. Creado 2026-08-07. |
| **Project Lyra** | [nv-tlabs/lyra](https://github.com/nv-tlabs/lyra) | **Apache-2.0** (`LICENSE`, 11.356 B) | **2.888** | 2026-07-20 | World models 3D generativos del **NVIDIA Toronto AI Lab**. Genera escena 3D, no video: es el lado "asset utilizable" del problema, igual que HY-World 2.0 pero **con Apache-2.0 limpia y sin restricción de territorio**. ⚠️ 4 meses sin push. |
| **stable-worldmodel** | [galilai-group/stable-worldmodel](https://github.com/galilai-group/stable-worldmodel) | **MIT** (`LICENSE`, 1.070 B) | **2.315** | 2026-10-02 | Plataforma de **evaluación reproducible** de world models. La pieza que falta para decidir entre Matrix-Game, Lyra y utopia con una medición en vez de un README. |
| **DotRecast** | [ikpil/DotRecast](https://github.com/ikpil/DotRecast) | **Zlib** (`LICENSE.txt`, 904 B) | **968** | 2026-10-03 | Port a **C#** de Recast & Detour (navmesh estándar de la industria). Esta KB ya listaba el original en C++; **DotRecast es la vía de entrada a Unity/stacks .NET**, y Zlib es la licencia más permisiva del inventario. |
| **VibeUE** | [kevinpbuckley/VibeUE](https://github.com/kevinpbuckley/VibeUE) | **MIT** (`LICENSE`, 1.091 B) | **845** | 2026-11-20 | *Vibe coding* para **Unreal Engine**. Tapa el hueco más viejo de esta tabla: el lado Unreal estaba cubierto sólo por `chongdashu/unreal-mcp` (**2.119 ★ pero `null` de licencia y 19 meses dormido**). |
| **gym-super-mario-bros** | [Kautenja/gym-super-mario-bros](https://github.com/Kautenja/gym-super-mario-bros) | **MIT** (`LICENSE`, 1.073 B) | **910** | 2026-09-30 | Entornos Gymnasium para NES/Mario con metadata de tarea y recompensa instrumentada. Benchmark clásico, **vivo** (push hace 2 meses) y compatible con `Farama-Foundation/Gymnasium` que esta KB ya lista. |
| **unity-cli-loop** | [hatayama/unity-cli-loop](https://github.com/hatayama/unity-cli-loop) | **MIT** (`LICENSE.md`, 1.075 B) | **602** | 2026-11-21 | Maneja Unity desde el Editor **hasta Play Mode** por CLI. Es la mitad que le falta a los MCP de Unity: casi todos editan la escena pero no saben *correr* el juego. ⚠️ Archivo `LICENSE.md`, no `LICENSE` — la sonda estándar le erraba. |
| **godot-mcp (tugcantopaloglu)** | [tugcantopaloglu/godot-mcp](https://github.com/tugcantopaloglu/godot-mcp) | **MIT** (`LICENSE`, 1.169 B) | **521** | 2026-10-09 | 157 herramientas MCP para control completo de Godot 4.x. **MIT íntegro — contrastar con `godot-mcp-pro`, mismo pitch y núcleo cerrado** (hallazgo nº2). |
| **Godot-MCP (IvanMurzak)** | [IvanMurzak/Godot-MCP](https://github.com/IvanMurzak/Godot-MCP) | **Apache-2.0** (`LICENSE`, 11.357 B) | **376** | **2026-11-21** | Del autor de `IvanMurzak/Unity-MCP`, que esta KB ya lista. **El Godot-MCP con el push más reciente de todo el clúster** (ver abajo). Apache-2.0 ⇒ trae grant de patentes, que MIT no da. |
| **funplay-unity-mcp** | [FunplayAI/funplay-unity-mcp](https://github.com/FunplayAI/funplay-unity-mcp) | **MIT** (`LICENSE`, 1.064 B) | **330** | 2026-11-19 | MCP de Unity con `execute_code` + *prompts/resources*. El `execute_code` es lo que lo separa: permite al agente correr C# arbitrario en el Editor en vez de pedir una herramienta por operación. |
| **Doyunha-Gopeak** | [HaD0Yun/Doyunha-Gopeak](https://github.com/HaD0Yun/Doyunha-Gopeak) | **MIT** (`LICENSE`, 1.070 B) | **274** | 2026-11-13 | 95+ herramientas MCP para Godot: escena, animación, física. Tercera opción MIT del clúster Godot. |

### 🟡 El clúster `godot-mcp`: **el líder por estrellas lleva 7 meses sin un commit**, y hay 6 alternativas vivas

Esta KB ya había detectado en el cycle 4 que `Coding-Solo/godot-mcp` estaba
muerto. Este pass **mide el clúster entero** y el resultado cambia la
recomendación:

| Repo | ★ | Licencia (byte-read) | Último push | Edad del push |
|---|---|---|---|---|
| [Coding-Solo/godot-mcp](https://github.com/Coding-Solo/godot-mcp) | **6.732** | MIT | 2026-04-16 | ⛔ **7 meses** |
| [youichi-uda/godot-mcp-pro](https://github.com/youichi-uda/godot-mcp-pro) | 649 | ⛔ MIT **sólo del plugin** (núcleo de pago) | 2026-09-24 | 2 meses |
| [tugcantopaloglu/godot-mcp](https://github.com/tugcantopaloglu/godot-mcp) | 521 | ✅ MIT | 2026-10-09 | 1,5 meses |
| [tomyud1/godot-mcp](https://github.com/tomyud1/godot-mcp) | 454 | ✅ MIT | 2026-10-17 | 1 mes |
| [IvanMurzak/Godot-MCP](https://github.com/IvanMurzak/Godot-MCP) | 376 | ✅ Apache-2.0 | **2026-11-21** | 🟢 **1 día** |
| [HaD0Yun/Doyunha-Gopeak](https://github.com/HaD0Yun/Doyunha-Gopeak) | 274 | ✅ MIT | 2026-11-13 | 9 días |
| [satelliteoflove/godot-mcp](https://github.com/satelliteoflove/godot-mcp) | 188 | ✅ MIT | 2026-11-16 | 6 días |

**El líder tiene 10× las estrellas del repo más activo.** Es la *runner-up
promotion* que el cycle 4 documentó en Unity, repetida en Godot con una
diferencia: acá **no hubo rename**. Nadie escondió al líder; simplemente las
estrellas no caducan y los commits sí. Una búsqueda ordenada por ★ —la que hace
cualquiera, incluida esta KB en sus primeros passes— devuelve siempre primero el
repo muerto.

> **Regla nº17: ordenar por ★ responde "qué fue importante"; ordenar por
> `pushed_at` responde "qué está vivo". Para elegir una base sobre la que
> construir, la segunda pregunta es la única que importa.**

### ⛔ Candidatos nuevos **rechazados** este pass, con motivo medido

| Candidato | Repo | Qué es | Motivo del rechazo |
|---|---|---|---|
| **godot-mcp-pro** | [youichi-uda/godot-mcp-pro](https://github.com/youichi-uda/godot-mcp-pro) | 649 ★, 162 herramientas MCP para Godot 4 | 🔴 **Open-core.** El MIT (1.282 B) cubre **sólo** `addons/godot_mcp/`; el servidor MCP —donde viven las 162 herramientas— es **propietario y de pago**. Ver hallazgo nº2. |
| **STS2-Agent** | [CharTyr/STS2-Agent](https://github.com/CharTyr/STS2-Agent) | 366 ★, expone *Slay the Spire 2* como MCP server para que un agente lo juegue | 🔴 **AGPL-3.0** (`LICENSE`, **33.985 B** — el checksum de AGPL de esta KB es ~34.500 B ✅). GitHub decía `NOASSERTION`. Copyleft de red: inaceptable para un entregable de cliente. Técnicamente es el mejor ejemplo nuevo de *game-as-MCP-server*, y por eso se anota en vez de omitirse. |
| **Locus** | [r1n7aro/Locus](https://github.com/r1n7aro/Locus) | 762 ★, *"the open source Unity Dev Agent"* | 🔴 **GPL-3.0** (`LICENSE`, 35.149 B; checksum GPL-3 ≈ 35.200 B ✅). Hay 6 alternativas MIT/Apache en la misma categoría. |
| **corosolto/client** | [corosolto/client](https://github.com/corosolto/client) | 263 ★, FPS de navegador con 5 facciones brasileñas, 44 personajes | 🔴 **Doble rechazo:** AGPL-3.0 (`LICENSE`, 34.523 B) **y no es un agente ni una herramienta** — es un juego. Se anota porque es **el repo de gaming de origen LATAM con más estrellas que encontró el canal de enumeración en 10 passes**, y ese dato vale para `intel/market.md` aunque la fila no entre acá. |
| **HY-WorldPlay** (raíz) | [Tencent-Hunyuan/HY-WorldPlay](https://github.com/Tencent-Hunyuan/HY-WorldPlay) | 1.630 ★, world model interactivo en tiempo real | 🟡 **Rechazo parcial, el más útil del pass.** La raíz está bajo licencia Tencent con exclusión EU/UK/Corea ⇒ **inusable en EMEA**. Pero `worldcompass/` es **Apache-2.0** y sí se puede usar. Entra a la KB como *entrada condicionada por región*, no como fila de tabla. Ver hallazgo nº1. |

---


## 🔴 Lo más importante del pass 2026-11-21 (cycle 9): **el backend de 25.570 ★ que esta KB nunca miró, y las dos sondas que esta sesión NO tiene**

### 🔴 Hallazgo nº1: `clockworklabs/SpacetimeDB` — 25.570 ★, ausente de esta KB en 8 passes, y **no es open source**

Medido hoy, no citado:

| Campo | Valor medido | Canal |
|---|---|---|
| Slug | [clockworklabs/SpacetimeDB](https://github.com/clockworklabs/SpacetimeDB) | `gh api search/repositories` |
| Estrellas / forks | **25.570 ★** / 1.121 forks | idem |
| Creado / último push | 2023-06-17 / **2026-11-21 (hoy)** | idem |
| `archived` | `false` — vivo | idem |
| Licencia según GitHub | **`NOASSERTION`** | idem |
| Licencia **leída byte a byte** | **BSL 1.1** (`LICENSE.txt`, **42.562 B**) | `raw.githubusercontent.com` |

El íncipit del archivo: `SPACETIMEDB BUSINESS SOURCE LICENSE AGREEMENT / Business Source License 1.1`.
Licensor: Clockwork Laboratories, Inc. Licensed Work: `SpacetimeDB 2.16.2`.

**Los dos parámetros que deciden:**

- **Additional Use Grant:** se permite usar el trabajo *"provided your application or
  service uses the Licensed Work with **no more than one SpacetimeDB instance in
  production**"*, y prohíbe ofrecer un *"Database Service"* a terceros.
- **Change Date: `2031-11-20`. Change License: `AGPL-3.0 with a linking exception`.**

**Veredicto: ⛔ fuera de la ruta permisiva de Globant.** No porque sea copyleft —
no lo es todavía— sino porque el grant de uso **limita producción a UNA instancia**.
Un juego multijugador comercial que escale horizontalmente está fuera del grant el
día que levanta la segunda instancia, y la conversión de 2031 es a AGPL (con
excepción de linking), no a una licencia permisiva.

**Por qué esto importa más que una fila rechazada:** los comparativos de terceros que
circulan en 2026 (*"SpacetimeDB vs PlayFab vs Nakama"*) lo listan **al lado de Nakama
(Apache-2.0) y de Talo (MIT) sin marcar que es BSL**. Un equipo que elija por tabla
comparativa de blog elige una licencia no-open-source creyendo que compara iguales.
Nakama y Talo siguen siendo las dos opciones recomendadas de esta KB; SpacetimeDB
queda registrado como **rechazo medido**, no como silencio.

#### 🆕 9º eje de partición de un grant: **por subdirectorio, declarado en la licencia base — y con la cláusula de conflicto contradiciéndose**

`LICENSE.txt` trae un preámbulo de 28 líneas que los 8 passes anteriores no podían
ver porque nadie había abierto el repo. Dice, textual:

- §2 *"Subdirectory-Specific Licenses ... If a subdirectory contains its own LICENSE
  or LICENSE.txt file, the terms in that file **apply exclusively** to all files and
  subfolders within that subdirectory."*
- y acto seguido: *"In the event of any conflict between this base license and a
  subdirectory's license, **the base license will govern** for that subdirectory's
  contents."*

**Las dos frases no pueden ser ambas verdaderas.** Si la licencia del subdirectorio
aplica *exclusively*, no puede a la vez ceder ante la base en caso de conflicto.
Para una auditoría esto no es trivia: significa que **no se puede resolver el grant
de un subdirectorio de este repo leyendo sólo ese subdirectorio**, y que la lectura
más conservadora (BSL gobierna todo) es la única defendible ante un cliente.

**Y el mismo texto aparece dos veces con distinto tamaño:** `licenses/BSL.txt` tiene
**40.980 B** — exactamente el mismo cuerpo menos el preámbulo de 28 líneas (29 líneas
de `diff`). Dos archivos de licencia, dos byte-counts, un solo grant: el checksum de
bytes **tríaga, no dicta** (regla heredada del cycle 7, revalidada aquí).

### 🔴 Hallazgo nº2: dos de las tres sondas de verificación de esta KB **no funcionan en esta sesión**, y una de ellas miente en silencio

Medición directa, repo real contra slug inventado:

| Sonda | Repo real (`godotengine/godot`) | Slug inventado (`zzz-definitely-not-a-real-repo-9988/nope`) | ¿Discrimina? |
|---|---|---|---|
| `curl -sI https://github.com/<slug>` | **403** | **403** | ❌ **NO. Da el mismo código a lo real y a lo inventado.** |
| `gh api repos/<slug>` | 403 `access ... not enabled for this session` | 403 idem | ❌ no habilitada |
| `gh api search/repositories` | devuelve el repo con ★/licencia/`archived`/`pushed_at` | no devuelve nada | ✅ **sí** |
| `raw.githubusercontent.com/.../LICENSE` | **200**, 1.149 B de texto real | **404**, cuerpo de **14 B** | ✅ **sí** |

**Consecuencia operativa:** la instrucción *"verificá cada URL con `curl -sI`"* es
**inejecutable en este entorno** y, peor, es *silenciosamente* inejecutable: devuelve
403 para todo, y un 403 se puede confundir con "existe pero es privado". Los dos
canales que sí discriminan son `gh api search/repositories` y el byte-read por `raw`.
**Toda fila nueva de este pass se verificó con esos dos, nunca con `curl -sI`.**

⚠️ El cuerpo de 404 de `raw` pesa **14 B** — es el *trap de los 14 bytes* que esta KB
documentó en el cycle 6. Sigue vivo y sigue siendo la forma más fácil de registrar
un archivo de licencia inexistente como si fuera uno diminuto.

### ⛔ Gap de canal declarado (no silenciado): **WebFetch no resuelve ningún host general en esta sesión**

Tres intentos, tres `ENOTFOUND`: `github.blog`, `es.kotaku.com`, `www.pocketgamer.biz`.
No es que las páginas no existan: **el entorno no resuelve hosts fuera de la política
de red de la sesión**. Por lo tanto, en este pass:

- Todo dato de **repositorio** es de primera mano (`search/repositories` + byte-read).
- Todo dato de **prensa / informes de mercado** viene de *snippets* de búsqueda, **no
  de la fuente primaria**. Donde dos fuentes discrepan, abajo se registran **las dos**
  y se dice que no se pudo dirimir, en vez de elegir la que suena mejor.

Para cerrar este gap hay que habilitar `github.blog`, los sitios de prensa de la
industria y los dominios de los informes en la configuración de red de la sesión.

### 🆕 Filas nuevas de este pass — 11 repos, **las 11 con licencia leída byte a byte**

Ninguno de estos 11 estaba en la KB antes de hoy (verificado contra el índice de 145
slugs ya presentes). Estrellas, `archived` y `pushed_at` medidos hoy 2026-11-21.

| Agente / herramienta | Repo | Licencia (byte-read) | ★ | Último push | Para qué sirve |
|---|---|---|---|---|---|
| **Scenario Skills** | [scenario-labs/skills](https://github.com/scenario-labs/skills) | **MIT** (`LICENSE`, 1.070 B) | **3.961** | 2026-11-21 | Skills para que un agente genere imagen, video, audio y **3D** eligiendo modelo y **cotizando el costo antes de gastar**, con consistencia de personaje. El repo más grande de los nuevos y el único que ataca el control de gasto del pipeline de assets. 350 forks. |
| **MineStudio** | [CraftJarvis/MineStudio](https://github.com/CraftJarvis/MineStudio) | **MIT** (`LICENSE`, 1.068 B, rama `master`) | 423 | 2026-09-28 | Paquete para desarrollar agentes de Minecraft: datos, modelos, entrenamiento e inferencia en una sola API. Complementa a Mindcraft (que esta KB ya lista) por el lado del *training*, no del *prompting*. |
| **VibeGame** | [tettethu/VibeGame](https://github.com/tettethu/VibeGame) | **Apache-2.0 + avisos de terceros embebidos** (`LICENSE`, 14.096 B — ver nota) | 389 | 2026-11-21 | Framework multi-agente auto-evolutivo con engine AI-native: convierte lenguaje natural en un juego jugable. Creado 2026-08-12 y ya en 389 ★. |
| **UnityMCP (isuzu)** | [isuzu-shiranui/UnityMCP](https://github.com/isuzu-shiranui/UnityMCP) | **MIT** (`LICENSE`, 1.066 B) | 344 | 2026-11-20 | El **Editor de Unity sirve MCP él mismo por HTTP**: no hay un segundo proceso puente que levantar. Arquitectura distinta a la de `CoplayDev/unity-mcp`, y la razón por la que vale listarlo aparte. |
| **UltraGameStudio** | [wellingfeng/UltraGameStudio](https://github.com/wellingfeng/UltraGameStudio) | **MIT** (`LICENSE`, 1.068 B) | 344 | 2026-11-21 | Agente de codificación para game dev: flujos de engine, código de gameplay y generación de assets en un solo agente. |
| **roblox-brain** | [TabooHarmony/roblox-brain](https://github.com/TabooHarmony/roblox-brain) | **MIT** (`LICENSE`, 1.069 B) | 199 | 2026-10-15 | Librería de skills con el conocimiento de **Roblox Studio** que a un agente de código le falta. **Primera entrada de Roblox como plataforma de desarrollo en esta KB** (ver `verticals/solutions.md`). |
| **godot-mcp (satelliteoflove)** | [satelliteoflove/godot-mcp](https://github.com/satelliteoflove/godot-mcp) | **MIT** (`LICENSE`, 1.056 B) | 188 | 2026-11-16 | Edición de escena, **inyección de input**, **playtesting determinista** y estado de juego en vivo para el agente. El *playtesting determinista* es lo que lo separa del `godot-mcp` de Coding-Solo que esta KB ya lista: es la pieza que falta para QA reproducible (Receta 9). |
| **RSIGame** | [WenyiWU0111/RSIGame](https://github.com/WenyiWU0111/RSIGame) | **Apache-2.0** (`LICENSE`, 11.400 B — ver nota de procedencia) | 178 | 2026-10-30 | Desarrollo de juegos agéntico y autónomo con **auto-mejora recursiva**. Creado 2026-09-28: el más joven de la tabla. |
| **ritmo** | [typeonce-dev/ritmo](https://github.com/typeonce-dev/ritmo) | **MIT** (`LICENSE`, 1.072 B) | 170 | 2026-11-21 | Toolkit *agent-first* de game dev: ECS, assets, efectos, sonido y música. Pensado desde el día uno para que lo maneje un agente, no una persona. |
| **overtime** | [ukanwat/overtime](https://github.com/ukanwat/overtime) | **MIT** (`LICENSE`, 1.078 B) | 403 | 2026-10-18 | Le das un *brief* a un agente de código, no un chat, y trabaja solo a través de sesiones. Incluye como ejemplo una ciudad open-world construida en un engine real. **Herramienta genérica**: se lista por el patrón de brief-en-vez-de-chat, no como herramienta de gaming. |
| **UnityMCPIntegration (quaza)** | [quazaai/UnityMCPIntegration](https://github.com/quazaai/UnityMCPIntegration) | **MIT** (`LICENSE`, 1.068 B) | 155 | **2025-04-15 ⚠️** | Control de Unity por agentes. **⚠️ Último push hace 19 meses.** Se lista para que quede registrado que está dormido: es el tipo de repo que una búsqueda por estrellas sigue devolviendo arriba y que no hay que tomar como base viva. |

**Nota sobre `VibeGame` (los 2.696 B de más):** el Apache-2.0 estándar pesa ~11.350 B;
este archivo pesa 14.096 B. Byte-read del excedente: **no son términos adicionales**,
es una sección `Third-Party Notices` anexada **dentro del propio `LICENSE`**, después
del `APPENDIX` de Apache, con texto MIT embebido para las dependencias vendorizadas.
Es decir: el inventario de terceros que el cycle 6 encontró en `THIRD_PARTY_NOTICES.md`
acá vive **adentro del archivo de licencia**. El grant base sigue siendo Apache-2.0 y
el repo es permisivo. **Regla: una desviación de byte-count obliga a leer, no a
rechazar** — acá la desviación era benigna y el rechazo automático habría sido un
falso positivo.

**Nota de procedencia sobre `RSIGame`:** su `LICENSE` Apache **no es la plantilla en
blanco**. Donde el template dice `Copyright [yyyy] [name of copyright owner]`, este
archivo dice `Copyright 2025 Google LLC` / `Copyright 2025 Qwen` /
`Copyright 2026 OpenGame contributors`. No es un defecto: Apache-2.0 **obliga** a
preservar los avisos de las obras de las que se deriva. Lo que dice es que RSIGame es
**trabajo derivado** de código de Google y de Qwen, y que quien lo forkee hereda ese
deber de atribución. **Señal de procedencia utilizable: el `LICENSE` de un derivado
permisivo delata sus padres.**

### ⛔ Candidato nuevo **rechazado** este pass, con motivo medido

| Candidato | Repo | Qué es | Motivo del rechazo |
|---|---|---|---|
| **ontobricks** | [databrickslabs/ontobricks](https://github.com/databrickslabs/ontobricks) | 485 ★, `NOASSERTION`, push 2026-11-21 | **Homónimo de canal.** Apareció en la búsqueda `unity ai agent mcp` porque convierte tablas de **Unity Catalog** —el metastore de Databricks— en un grafo de conocimiento expuesto por MCP. **No tiene ninguna relación con el engine Unity.** Es el equivalente, dentro de GitHub, del homónimo *gaming = casino* que el cycle 8 encontró en la prensa: **"Unity" también es ambiguo, y la ambigüedad entra por el buscador de repos.** |

> **Regla nueva (nº14):** antes de aceptar una fila que llegó por búsqueda de texto,
> comprobar que el término que la trajo significa en ese repo lo que significa acá.
> Dos homónimos ya medidos en esta KB: **`gaming`** (videojuego vs casino, en prensa) y
> **`Unity`** (engine vs Unity Catalog de Databricks, en GitHub).

## 🔴 Lo más importante del pass 2026-11-21 (cycle 8): **un archivo de licencia puede estar ROTO**, y los dos canales en los que esta KB más confía **se contradicen entre sí**

Siete passes construyeron un método para **leer** un grant: checksum de bytes
(R10/R20), ejes de alcance (R14), enumeración en vez de sonda (R17), el íncipit
como dictamen. Todos esos canales asumen lo mismo sin decirlo nunca:
**que el archivo que vas a leer es una instancia bien formada de la licencia que
declara.** Este pass encontró el caso donde no lo es — y, en el mismo repo, el
caso donde **el manifiesto y el `LICENSE` dicen dos familias de licencia
distintas**, sin que ninguno de los dos sea un anuncio de marketing.

### 🔴 Hallazgo nº1: `Farama-Foundation/Minigrid` — el `LICENSE` es **un fragmento de diff**, y hay una contradicción a tres bandas

`Minigrid` (2.543 ★, benchmark RL estándar, push 2026-11-14) devuelve
`NOASSERTION` en el badge. Resuelto por byte-read en las dos direcciones, como
manda T60, aparece esto:

| Canal | Qué dice | Evidencia leída 2026-11-21 |
|-------|----------|----------------------------|
| 1. Badge SPDX (`search/repositories`) | **`NOASSERTION`** | sin veredicto |
| 2. `LICENSE` (raíz) | **"Apache License, Version 2.0"** | **2.507 B / 40 líneas.** Contiene **sólo** la § 1 (Definitions, parcial) y la § 9. **Faltan las §§ 2 a 8** — es decir, falta el **grant mismo** (§ 2 Grant of Copyright License, § 3 Grant of Patent License, § 4 Redistribution). En la línea 18 hay un **encabezado de hunk de diff literal: `@@ -154,49 +194,23 @@`** |
| 3. `pyproject.toml` (manifiesto) | **MIT** | `license = { text = "MIT License" }` + classifier `License :: OSI Approved :: MIT License` |
| 4. `docs/LICENSE` ← **sólo visible por enumeración (R17)** | **Composición MIT + Apache-2.0, completa** | **12.201 B.** *"This repository is licensed as follows: All assets in this repository are the copyright of the Farama Foundation… Some code in this repository has been taken from other open source projects and was originally released under the MIT or Apache 2.0 licenses… The MIT and Apache 2.0 licenses are included below."* |

**Veredicto: `Minigrid` ES PERMISIVO** (composición MIT + Apache-2.0) y se puede
usar. **Pero el archivo que hay que citar es `docs/LICENSE`, no `LICENSE`.**

**Por qué este caso importa más que la fila que corrige — rompe tres reglas de la casa:**

1. **El checksum de bytes lo habría clasificado mal, no lo habría cazado.** 2.507 B
   con íncipit `Apache License` encaja de taquito en la categoría que esta KB ya
   tiene inventada y bendecida: *"Apache sin apéndice"* (`libgdx`, 10.174 B;
   `Calix-L/DanKS`, 11.348 B). El checksum sabe decir *"no es el canónico"*;
   **no sabe decir "no es un documento"**. Un archivo incompleto y un archivo
   recortado a propósito son indistinguibles por tamaño.
2. **La regla "leé el manifiesto, no el anuncio" (T63) aquí da MIT; el byte-read
   del archivo da Apache-2.0.** Ninguno de los dos es prosa de blog: los dos son
   artefactos versionados en el árbol. **Los dos canales primarios de la casa se
   contradicen sobre la misma línea de la misma tabla.** La salida no es elegir
   el que más gusta: es que **manifiesto ≠ `LICENSE` en la *familia* de licencia
   no es un veredicto, es una ambigüedad abierta** — y se resuelve buscando el
   tercer documento, no votando entre los dos primeros.
3. **La enumeración (R17) no fue "un canal más": fue el único que encontró el
   grant real.** Una sonda por `LICENSE` devuelve **el archivo equivocado** y lo
   devuelve con `HTTP 200` y 2.507 B — ni 404 ni byte-count sospechoso de
   no-hallazgo. El cycle 6 escribió *"una sonda confirma lo que ya sospechás;
   sólo la enumeración te dice lo que te faltaba"*. **Caso nuevo y peor: la
   sonda no omite el archivo, lo sustituye.**

**🟢 `NOASSERTION` acertó, y por dos motivos independientes a la vez** (archivo
dañado **y** repo genuinamente compuesto). Es el primer caso registrado en esta
KB+energy+enterprise donde el badge es correcto **y** su causa es doble.

**Los hermanos están sanos** ⇒ el daño es de `Minigrid`, no una práctica de la
Farama Foundation: `Gymnasium` `LICENSE` 1.104 B MIT, 0 artefactos de diff;
`PettingZoo` 1.073 B MIT, 0 artefactos. **No propagar la sospecha al org.**

**Chequeo mecánico nuevo, de 2 líneas, que esto deja como regla (ver Receta 21):**

```bash
grep -cE '^\s*(@@|<<<<<<<|=======|>>>>>>>|\+\+\+|---)' LICENSE   # debe dar 0
grep -cE '^ *[0-9]+\. ' LICENSE                                  # Apache-2.0 → 9
```

### 🔴 Hallazgo nº2: `MineDojo/NitroGen` — **"open foundation model" en el abstract, uso NO COMERCIAL VIRAL en el archivo**

El modelo de agente de juego más visible del año **no estaba en esta KB** y, al
encontrarlo, **hay que rechazarlo**. `MineDojo/NitroGen` (**2.251 ★**, push
2026-01-25, **oral en CVPR 2026**, arXiv `2601.02427`): behaviour cloning sobre
**40.000 horas de gameplay en más de 1.000 juegos**, wrapper compatible con
Gymnasium, checkpoint en Hugging Face (`nvidia/NitroGen`).

- Badge: `NOASSERTION`. **Byte-read: `LICENSE` = 4.729 B, `NVIDIA License`.**
- **§ 3.3 Use Limitation:** *"The Work and any derivative works thereof only may
  be used or intended for use **non-commercially**… **non-commercial research
  purposes only**, and excludes any military, surveillance, service of nuclear
  technology or biometric processing purposes."*
- **§ 3.2 lo hace VIRAL:** podés poner tus propios términos a un derivado **sólo
  si** tus términos **arrastran la limitación de uso de la § 3.3**.
- **§ 3.7:** arrastra además componentes bajo *Other Licenses*, nombrando el
  **Meta OPT-IML 175B License Agreement**.
- **§ 3.4:** cláusula de **terminación por reclamo de patente**.

⛔ **Exclusión dura para cualquier entregable de Globant**, incluido un PoC
facturado: el límite es *"used **or intended for use** non-commercially"* — un
piloto pago ya está fuera, y el derivado no se puede relicenciar.

**La regla que deja, y que la flota venía necesitando:** el paper dice
*"we open-source the NitroGen dataset, simulator, and pre-trained weights"* y la
model card de Hugging Face dice *"Governing Terms: NVIDIA License"*.
**La palabra "open-source" en un abstract revisado por pares no es un grant.**
Ésta es la T63 (*"leé el manifiesto, no el anuncio"*) aplicada a un paper de
CVPR: **un venue de primer nivel no audita licencias**, y el abstract es el
anuncio. Es también el contrapunto exacto al hallazgo nº1 del cycle 7 sobre los
pesos de `GameScaling`: ahí el grant de pesos resultó **mejor** de lo que el
badge decía; acá, **mucho peor** de lo que dice el paper.

🔵 **Nota de inventario:** esta KB tenía `MineDojo/Voyager` (7.339 ★, congelado
desde 2024-04-03) y **no tenía ni `MineDojo/MineDojo` ni `MineDojo/NitroGen`** —
el laboratorio estaba en la KB **por su repo muerto y no por su repo vivo**.
Encontrado por el censo plano de incumbentes (regla T64), no por una consulta
con forma de problema.

### 🟡 Hallazgo nº3: **"gaming" es un homónimo, y el informe-faro de 2026 es del otro gaming**

Las búsquedas de este pass sobre adopción y regulación devolvieron, en primer
lugar y en las cuatro regiones, **"The State of AI in Gaming 2026"** (UNLV
International Gaming Institute + KPMG, primer informe de referencia del rubro):
*AI Maturity Index* que **no pasa de 45 sobre 100**, adopción amplia con
gobernanza rezagada, y una **divergencia medida entre reguladores e industria**
(los reguladores ponen el riesgo de juego problemático y de resultados injustos
arriba; la industria, último).

🔴 **Ese informe es de CASINO / APUESTAS, no de videojuegos.** En inglés
*"gaming"* es, en contexto regulatorio y de mercado, **el rubro del juego de
azar** — y por eso domina los resultados de cualquier búsqueda de
*"AI gaming regulation 2026"*. **Una KB de videojuegos que ingiera esas cifras
como propias se corrompe sola**, y el riesgo es alto justamente porque el
informe es bueno, cuantitativo y reciente.

**Regla de ingesta para toda la flota:** en gaming, **antes de citar una cifra
hay que clasificar la fuente en `videojuegos` o `azar/iGaming`**, y decirlo en
la misma línea. El mismo criterio que la R14 aplica al alcance de un grant,
aplicado al alcance de un número. Las cifras de este pass quedan etiquetadas en
`intel/market.md`. **Ninguna cifra de UNLV/KPMG se incorpora al tamaño de
mercado ni a la adopción de esta KB.**

### 🔧 Hallazgo nº4: contradicción **interna** corregida — C2PA en la Receta 7

`agents/top.md` publica desde el cycle 7 que `contentauth/c2pa-rs` y
`c2pa-python` son **MIT *o* Apache-2.0** a elección (`LICENSE-MIT` 1.083 B +
`LICENSE-APACHE` 11.335 B; **re-verificado en bytes en este pass, idéntico**).
`compose/patterns.md` seguía diciendo **"Apache-2.0"** a secas en el stack de la
Receta 7 y en su lista de repos. **Corregido.**

No es cosmético: **en un grant dual, el integrador elige**, y para un entregable
de cliente **conviene elegir Apache-2.0 por el grant expreso de patente** (MIT
no lo tiene) — justo en el componente que firma el marcado del Art. 50(2). Una
fila que dice "Apache-2.0" pierde la elección; una que dice "MIT" pierde la
patente. **Un hallazgo de licencia que no se propaga a la receta que lo usa es
medio hallazgo**, y es el primer caso medido de deriva interna entre páginas de
esta KB.

### ⛔ Gap de canal declarado (no silenciado)

Cuatro hosts **no resuelven** por el proxy de esta sesión (`ENOTFOUND`):
**`www.dlapiper.com`, `chambers.com`, `www.unlv.edu`, `github.blog`.** En
consecuencia, y a diferencia del cycle 7:

- Las fechas del Art. 50 / Digital Omnibus de este pass son **secundarias**
  (snippets de búsqueda), **no verificadas en la fuente**. Coinciden con lo que
  los cycles 5-7 ya publicaron ⇒ cuentan como **4ª confirmación independiente
  del 2-dic-2026**, **no** como verificación primaria. Nadie debe citar este
  pass como lectura de fuente.
- El informe UNLV/KPMG se cita **sólo** por snippet, y por eso se usa para
  **delimitar el homónimo**, nunca como dato de mercado.

Se suma a la lista de hosts muertos por política de la flota (`modelcontextprotocol.io`
en enterprise, `arxiv` en energy). **Un gap escrito es información; el silencio
se lee igual que la cobertura.**

### 🔴 Defecto de instrumento: **`curl -sI https://github.com/<slug>` no verifica nada en esta sesión**

El runbook de esta rotación prescribe *"verificá toda URL antes de escribirla
(`curl -sI`)"*. **Medido en este pass, ese instrumento tiene poder
discriminante CERO:**

| Sonda | Repo real (`openai/gym`) | Repo inexistente (`gmilano/this-repo-does-not-exist-xyz123`) | ¿Discrimina? |
|-------|--------------------------|--------------------------------------------------------------|--------------|
| `curl -sI github.com/<slug>` | **403** | **403** | ⛔ **NO** |
| `curl -sI raw.../<slug>/HEAD/LICENSE` | 404 | 404 | ⛔ **NO** (prueba el *archivo*, no el repo) |
| `gh api search/repositories -f q='repo:<slug>'` | **`total_count: 1`** | **HTTP 422 Validation Failed** | 🟢 **SÍ** |

El proxy devuelve **403 a todo HTML de `github.com`**. ⇒ Un pass que siga el
runbook al pie de la letra **rechazaría el 100% de las filas verdaderas** y
escribiría *"404, no es un hallazgo"* sobre repos vivos. Y al revés: un 403
**nunca** es evidencia de que un repo no exista.

🟢 **El único canal de existencia válido acá es `gh api search/repositories`**, y
es el que se usó para **los 139 slugs del inventario y las 7 filas nuevas** de
este pass. `raw` sirve para **leer el grant en bytes**, no para probar existencia.

**La regla general, que ya tiene precedente en esta casa:** una sonda que
devuelve **el mismo código para todo** está midiendo el proxy, no el objeto —
igual que el trap de los 14 bytes del cycle 6 medía el cuerpo de un 404 y no una
licencia. **Antes de confiar en un instrumento nuevo hay que correrlo contra un
control negativo conocido.** Ningún pass anterior lo había hecho con esta sonda.

### 🆕 Filas nuevas de este pass (todas byte-read, todas permisivas salvo la rechazada)

| Nombre | Repo | Licencia (archivo, bytes) | Descripción | ★ |
|--------|------|---------------------------|-------------|---|
| 🆕 🟢 **LightZero** | [opendilab/LightZero](https://github.com/opendilab/LightZero) | **Apache-2.0** (`LICENSE`, 11357 — canónico) | **La pieza que el stack de decisión de esta KB no tenía: MCTS + la familia AlphaZero/MuZero** (incl. variantes sampled y stochastic) en un toolkit unificado. PPO sobre Gymnasium aprende **políticas reactivas**; esto hace **búsqueda con modelo aprendido** ⇒ es la base correcta para **IA de juego de tablero/carta, dificultad calibrada y oponentes que planifican**, no para locomoción. Push 2026-11-19. | 1.663 |
| 🆕 🟢 **DI-engine** | [opendilab/DI-engine](https://github.com/opendilab/DI-engine) | **Apache-2.0** (`LICENSE`, 11342) | Plataforma RL general del mismo laboratorio (OpenDILab), con el catálogo de algoritmos más amplio del inventario y entornos de juego ya integrados. **Es el marco del que LightZero es el módulo de búsqueda** ⇒ se adoptan juntos o se adopta sólo LightZero. ⚠️ **push 2025-12-07: ~11,5 meses sin commit** — vivo pero lento; verificar antes de comprometerlo en un roadmap. | 3.645 |
| 🆕 🟢 **Minigrid** | [Farama-Foundation/Minigrid](https://github.com/Farama-Foundation/Minigrid) | 🟡 **MIT + Apache-2.0 (composición)** — leer **`docs/LICENSE`** (12.201 B), **NO** `LICENSE` (2.507 B, **dañado**; ver hallazgo nº1) | Entornos de grilla minimalistas para RL con objetivos, llaves, puertas y **misiones en lenguaje natural**. En gaming sirve para lo más barato de todo: **prototipar y testear la lógica de un agente sin pagar el costo de un engine 3D**. Mismo API Gymnasium ⇒ el agente migra después. Push 2026-11-14. | 2.543 |
| 🆕 **MicroRTS-Py** | [Farama-Foundation/MicroRTS-Py](https://github.com/Farama-Foundation/MicroRTS-Py) | **MIT** (`LICENSE`, 1117) | Entorno RTS en miniatura para RL: **el caso de estrategia en tiempo real** (economía, build order, micro) a escala de laboratorio. ⚠️ push 2025-08-11, 295 ★ ⇒ **señal, no base de producción.** | 295 |
| 🆕 ⚠️ **MineDojo** | [MineDojo/MineDojo](https://github.com/MineDojo/MineDojo) | **MIT** (`LICENSE`, 1070) | La suite de entornos y benchmark de Minecraft sobre la que se construyó `Voyager`. Hueco histórico: la KB listaba el agente y no el entorno. 🔴 **push 2024-03-18 (2 años y 8 meses)** ⇒ 10º caso de *"congelado pero citado"*. **Leerlo como benchmark, no forkearlo como base viva.** | 2.285 |
| 🆕 ⛔ **NitroGen** | [MineDojo/NitroGen](https://github.com/MineDojo/NitroGen) | ⛔ **`NVIDIA License`** (`LICENSE`, 4729) — **NO COMERCIAL y VIRAL** (§ 3.3 + § 3.2) | Foundation model de agente generalista de NVIDIA: 40.000 h de gameplay, +1.000 juegos, oral en CVPR 2026. **Técnicamente lo más relevante del nicho; legalmente inusable.** Ver hallazgo nº2. **Citar en una propuesta como estado del arte, nunca como componente.** | 2.251 |
| 🆕 ⛔ **gym** (histórico) | [openai/gym](https://github.com/openai/gym) | — | 🔴 **ARCHIVADO** (`archived: true`, 37.237 ★). Sucesor mantenido: **`Farama-Foundation/Gymnasium`**, ya en esta KB. Se anota **para que nadie lo proponga**: 37 k ★ lo mantienen primero en cualquier búsqueda de "RL environments". 11º caso de *"muerto pero recomendado"*. | 37.237 |

## Lo más importante del pass 2026-11-21 (cycle 7): **la regla del pass anterior, aplicada un nivel más arriba**

El pass cycle 6 cerró con la regla central de esta familia de KBs:

> *"Una sonda sólo puede confirmar lo que ya sospechabas. Sólo la enumeración te
> dice lo que te faltaba."*

La aplicó a **los archivos de licencia dentro de un repo**. Este pass la aplica a
**la lista de repos misma** — y ahí la KB estaba tan sesgada como el inventario
de sondas que la regla describe.

🔴 **El hallazgo:** esta KB tenía **117 slugs** y **seis passes**, y no tenía
`bevyengine/bevy` — **49.914 ★, commit de anteayer, el segundo engine open source
más estrellado del mundo después de Godot.** Tampoco `phaserjs/phaser` (40.756 ★),
ni `raysan5/raylib` (35.758 ★), ni `libgdx/libgdx` (25.553 ★), ni
`google-deepmind/open_spiel` (5.603 ★, el framework de referencia de RL en juegos).

**Por qué se escaparon, y es un defecto de método, no de suerte:** todas las
consultas de esta KB tienen forma de **problema** ("NPC con LLM", "MCP para
engines", "agentes que juegan", "licencias que bloquean"). Un **engine de
propósito general no responde a ningún problema en particular**, así que no
aparece en ninguna búsqueda — por más veces que se busque. `bevy` sólo figuraba
en esta KB como la palabra *"paridad Bevy"* **dentro de la descripción de un pack
de skills ajeno**.

> 📐 **Regla de método nº14 — el chequeo de inventario plano.** Toda KB a partir
> de su tercer pass debe gastar **una consulta por pass** en un **censo contra
> los incumbentes obvios** de la industria, sin forma de problema: "los 10
> engines open source más usados", "las 10 plataformas de X". Un vocabulario de
> problemas tiene un punto ciego **con la forma exacta de un producto**.
> *(La KB de enterprise llegó a la misma conclusión de forma independiente al
> descubrir que le faltaba `google/adk-python` con 22.295 ★. Dos industrias, el
> mismo agujero: no es casualidad, es el método.)*

---

## 🔴 Hallazgo nº2: el **checksum de bytes** —la técnica estrella de esta KB— tiene tres defectos medidos, y dos producen falsos negativos

El checksum heredado dice `Apache~11350`. Este pass midió **tres valores legítimos
distintos de Apache-2.0** y uno que **no es Apache y cae a 196 bytes de él**:

| Bytes | Qué es | Medido en | Consecuencia |
|-------|--------|-----------|--------------|
| **11358** | Apache-2.0 **canónico**, con `APPENDIX: How to apply` + boilerplate | `open_spiel`, `mujoco`, `ramenvr/autopilot` | Es el valor que el checksum conocía |
| **10173 / 10174** | Apache-2.0 **sin el apéndice** — el texto termina en `END OF TERMS AND CONDITIONS` | `bevyengine/bevy`, `libgdx/libgdx` | 🔴 **FALSO NEGATIVO**: el checksum heredado los habría marcado como "no Apache". Δ = **1.185 B = exactamente el bloque del apéndice** |
| **15057** | Apache-2.0 **+ texto añadido dentro del mismo archivo** | `NVIDIA/Audio2Face-3D-training-framework` | 🔴 Un conteo **por encima** del canónico significa **cláusulas añadidas** y obliga a leer. Acá son 3 dependencias vendorizadas MIT (Charsiu, MDM, OpenAI Guided Diffusion) declaradas bajo `PORTIONS LICENSED AS FOLLOWS`, **con su ruta** (`audio2face/deps/...`) |
| **9977** | 🔴 **NO es Apache**: `Defold License Version 1.0, May 2020` | `defold/defold` | Está a **196 B** del Apache sin apéndice. **El checksum no puede distinguirlos.** |

> 📐 **Regla de método nº15 — el checksum tríaga, el íncipit dicta.** Un conteo de
> bytes **acota el candidato**; nunca lo resuelve. Tres correcciones concretas:
> 1. **Apache-2.0 tiene dos valores válidos**, con y sin apéndice (~11358 / ~10173).
>    Usar sólo uno descarta mal.
> 2. Un conteo **por encima** del canónico es **tan informativo como uno por
>    debajo**: significa texto añadido. Leerlo siempre.
> 3. La **primera línea** es el único dato que separa un grant estándar de uno
>    hecho a medida. `Defold` copia Apache palabra por palabra y le cambia el
>    nombre y una cláusula.

### 🔴 `defold/defold` (6.476 ★): la restricción que apunta a Globant, no al cliente

El badge dice `NOASSERTION`. El byte-read dice `Defold License 1.0` (9977 B), que
es **Apache-2.0 verbatim** (diff a nivel palabra: 127 palabras de diferencia sobre
1.413) más **una definición y una cláusula**:

```
"Game Engine Product" shall mean software used for video game development. This
includes both the content authoring software and the software used to show the
created content.
...
4. a) You do not sell or otherwise commercialise the Work or Derivative Works as
a Game Engine Product; and
```

🔴 **Es una restricción de campo de uso** ⇒ **falla el punto 6 de la Open Source
Definition** ⇒ **source-available, no open source**, por mucho que el cuerpo sea
Apache.

🟢 **Y el matiz comercial es el inverso del habitual en esta KB:** un estudio que
**hace un juego** con Defold está perfectamente dentro del grant. Quien choca con
la cláusula es **quien vende herramientas de autoría** — es decir, **exactamente
el entregable "pipeline de desarrollo asistido por AI" que esta KB propone en las
Recetas 6 y 11.** Es la única licencia del inventario cuya restricción apunta al
**modelo de negocio de la consultora** y no al del cliente. Si el entregable es un
autoring tool, Defold está fuera; si es un juego, está dentro.

---

## 🔴 Hallazgo nº3: `Unity-Technologies/ml-agents` — **un 8º eje de partición del grant: por RAMA**, y una corrección a lo que esta KB publicaba

Esta KB publica, en `repos/foundations.md` y en `repos/trending.md`:

> *`Apache-2.0` (`LICENSE.md`, 572 — **cabecera-puntero, no el texto completo**)*

Medido hoy, el **mismo nombre de archivo** en el **mismo repo**:

| Rama | `LICENSE.md` | Qué contiene |
|------|-------------|--------------|
| **`develop`** ← rama **default** | **572 B** | El **aviso corto** de Apache ("Licensed under the Apache License, Version 2.0... You may obtain a copy at...") |
| `main` | **11.324 B** | El **texto completo** de Apache-2.0 |

🔴 **Dos correcciones, no una:**
1. **El texto completo existe** — está en `main`. La KB leyó sólo la rama default
   (`HEAD` la resuelve sola) y concluyó "no está el texto completo", que es cierto
   *de esa rama* y falso *del repo*.
2. **"Cabecera-puntero" sugiere un grant deficiente, y no lo es.** Esos 572 B son
   el **boilerplate que Apache-2.0 recomienda** en su propio apéndice — el mismo
   bloque que `bevy` y `libgdx` omiten de su `LICENSE`. Es un grant válido y
   completo por referencia. **No hay nada que arreglar en ml-agents; había algo
   que arreglar en nuestra lectura.**

> 📐 **Regla de método nº16 — el grant puede partirse por rama.** Un archivo de
> licencia **corto no es un grant incompleto**: puede ser un artefacto de rama.
> Sondear `HEAD` **y** `main` **y** `master`, comparar bytes, y tomar **el texto
> más largo como el operativo**. 20× de diferencia en el mismo nombre de archivo
> del mismo repo, medido.

### 🧪 Defecto de instrumento encontrado dentro de este pass: `raw` **aliasea `master` → `main`**

Al verificar lo anterior: `ml-agents` **no tiene** `refs/heads/master`
(`git ls-remote` lista `develop`, `main`, `hotfix-bump-version-master`,
`master-limitation-docs`, `master-project-upgrade`, `master-reward-providers`,
`master-tracking` — ninguna es `master`). Y sin embargo:

```
raw.githubusercontent.com/Unity-Technologies/ml-agents/master/LICENSE.md
  → HTTP 200, 11.324 B, byte-idéntico a main
raw.githubusercontent.com/Unity-Technologies/ml-agents/zzz-no-such-branch/LICENSE.md
  → HTTP 404   ← control negativo
```

⇒ **`raw` resuelve `master` como alias de la rama default renombrada.** Un bucle
de sondas sobre `main` y `master` **cuenta el mismo archivo dos veces** y hace
creer que existe una rama que no existe. 🔴 **"Lo encontré en `master`" no es
prueba de que `master` exista.** Deduplicar por **hash de contenido**, no por
nombre de rama.

---

## 🆕 Agentes nuevos verificados este pass (cycle 7)

★, `archived` y `pushed_at` leídos con canal 1 el **2026-11-21**; grant leído en
bytes con status HTTP; multiplicidad de archivos de licencia enumerada con canal 3.

| Nombre | Repo | Licencia (archivo, bytes) | Descripción | ★ |
|--------|------|---------------------------|-------------|---|
| 🆕 🟢 **Aura Autopilot** | [ramenvr/autopilot](https://github.com/ramenvr/autopilot) | **Apache-2.0** (`LICENSE`, 11358) | **El agente de playtesting open source que esta KB venía pidiendo.** Sistema de playtesting general que aprende el juego y mejora corrida a corrida. Soporta **UE 5.4–5.8, Unity 6, Godot 4 y Roblox Studio**, Windows y macOS, y expone un **MCP** usable desde Claude Code o Codex. Commit de hoy. 🟢 **Verificado que el grant cubre fuente real, no un envoltorio**: 651 archivos, `plugin/` de 10 MB, 50 `.cpp` / 24 `.h` / 81 `.cs` / 47 `.gd` / 156 `.py` y un `.uplugin`. ⚠️ **El camino de instalación por defecto descarga binarios precompilados de `tryaura.dev`**, no del repo: para un cliente que exige auditabilidad hay que **compilar de fuente** (el README lo soporta explícitamente para Unreal). Las cifras de rendimiento (43% más preciso, 2×, 7/7 vs 5/7) **son del vendor y no están verificadas independientemente**. | 881 |
| 🆕 🟢 **Godogen** | [htdt/godogen](https://github.com/htdt/godogen) | **MIT** (`LICENSE.md`, 1077) | Dos skills para Claude Code / Codex / Antigravity: **`engine-guide`** (Godot C#/GDScript, **Bevy**, Babylon.js, three.js, HTML — cómo elegir, captura headless a screenshot y vídeo, y las trampas que fallan sin error) y **`asset-gen`** (sprites, 3D texturizado, humanoides riggeados, SFX y voz). Commit de hoy. **El pack de skills de gaming más estrellado que existe, y es MIT** ⇒ **contraejemplo medido a la tendencia 16** (el copyleft subiendo a la capa de skills): la cima de esa capa es permisiva. 🔴 **Pero ver el hallazgo nº4: te instala una dependencia sin licencia.** | **7.378** |
| 🆕 ⚫ **godogen_assets** | [htdt/godogen_assets](https://github.com/htdt/godogen_assets) | 🔴 **SIN LICENCIA — ningún grant** (69 archivos versionados, **0** archivos de licencia) | El motor de generación local de assets de Godogen (Blender, imagen, mesh, rig, motion, lipsync, sfx, voz vía `bin/*`). **Dos canales independientes concuerdan**: `find` sobre el clon → 0 resultados; `search/code repo:htdt/godogen_assets filename:LICENSE` → `total_count: 0`. ⛔ **Sin licencia no hay permiso**: no se puede copiar, modificar ni redistribuir en un entregable de cliente. Además exige un token de Hugging Face cuya cuenta haya **aceptado la licencia de `stabilityai/stable-audio-3-small-sfx`** (único modelo *gated*). | 2 |
| 🆕 **OpenGameAgent** | [EricSun0218/OpenGameAgent](https://github.com/EricSun0218/OpenGameAgent) | **MIT** (`LICENSE`, 1096) | Runtime TypeScript para **personajes y mundos AI-native**. Muy nuevo (push 2026-11-17) y muy chico: **se anota como señal temprana, no como base recomendada**. | 61 |
| 🆕 ⚠️ **Voyager** | [MineDojo/Voyager](https://github.com/MineDojo/Voyager) | **MIT** (`LICENSE`, 1070) | El agente LLM *open-ended* de Minecraft (currículo automático + skill library + prompting iterativo): **la implementación de referencia más citada de "LLM que juega"**, y un hueco histórico de esta KB. 🔴 **`pushed_at` = 2024-04-03: 2 años y 8 meses sin un commit.** Todo roundup de 2026 lo sigue recomendando. ⇒ **9º caso en esta KB+energy+enterprise de "archivado/congelado pero recomendado"**, y otra vez el patrón: lo recomienda la documentación adyacente al proyecto, nunca una fuente independiente. **Leer el paper y la skill library; no forkear como base viva.** | 7.339 |
| 🆕 **Mindcraft** | [mindcraft-bots/mindcraft](https://github.com/mindcraft-bots/mindcraft) | **MIT** (`LICENSE`, 1073) | Framework de agentes LLM en Minecraft (multi-agente, colaboración, construcción). 🟢 **RESUELTO el 2026-11-22: no estaba fuera del índice — estaba RENOMBRADO.** `kolbytn/mindcraft` → **`mindcraft-bots/mindcraft`**, **6.039 ★**, MIT, push 2026-10-27. El 422 de `search/repositories` era el rename, no una ausencia del índice. **Un repo ausente del índice no es un repo inexistente — pero inventarle una cifra sería peor que no darla.** 🔴 Figuraba en el cycle 3 (ago-2026) y **se perdió**: ese pass quedó en commit local sin push. | n/d |

### 🟢 NVIDIA ACE / Audio2Face-3D: la KB lo citaba en 8 archivos sin un solo grant leído

`NVIDIA/Audio2Face-3D` (574 ★) **no tiene licencia porque no tiene código**: 3
entradas versionadas (`README.md`, `RELEASE.md`, `media/`) y **cero** archivos de
licencia. Es un **repo-señalizador** que declara las licencias de sus componentes
**en prosa, dentro de una tabla HTML del README**. Leídas en bytes, una por una:

| Componente | Repo | Licencia (archivo, bytes) | ★ | `pushed_at` |
|------------|------|---------------------------|---|-------------|
| **A2F-3D SDK** | [NVIDIA/Audio2Face-3D-SDK](https://github.com/NVIDIA/Audio2Face-3D-SDK) | **MIT** (`LICENSE.txt`, 1097) | 238 | 2025-08-28 |
| **A2F-3D Training Framework** | [NVIDIA/Audio2Face-3D-training-framework](https://github.com/NVIDIA/Audio2Face-3D-training-framework) | **Apache-2.0 + 3 porciones MIT vendorizadas** (`LICENSE`, **15057**) | 125 | 2025-10-15 |
| **Maya ACE** | [NVIDIA/Maya-ACE](https://github.com/NVIDIA/Maya-ACE) | **MIT** (`LICENSE.txt`, 1098) | 103 | 2026-02-05 |

🟢 **Veredicto: los tres son usables comercialmente** y las 3 porciones
vendorizadas del training framework (Charsiu, MDM, OpenAI Guided Diffusion) son
**todas MIT**, así que no hay contaminación. ⚠️ **El plugin de UE5 que el README
lista como MIT no tiene repo de GitHub enlazado** — se distribuye por otra vía y
**su grant no se pudo leer**: queda como gap declarado, no como fila MIT.
⚠️ **Y la prosa del README no es el grant**: acertó en los tres casos
verificables, pero se verificó, no se adoptó.

### ⚫ Candidatos nuevos **rechazados** este pass, con motivo medido

| Candidato | Repo | Motivo | ★ |
|-----------|------|--------|---|
| **minerl** | [minerllabs/minerl](https://github.com/minerllabs/minerl) | Badge `NOASSERTION` → byte-read: **CC-BY-NC-SA 4.0** (`LICENSE`, 20850). 🔴 **`NonCommercial`: exclusión dura** para cualquier entregable de cliente. Además congelado (push 2025-01-22). Es el entorno RL de Minecraft que la literatura académica recomienda por defecto. | 981 |
| **SillyTavern** | [SillyTavern/SillyTavern](https://github.com/SillyTavern/SillyTavern) | **AGPL-3.0** (`LICENSE`, 34523 — **el checksum heredado `AGPL~34500` acertó**). Es el front-end dominante de roleplay con personajes y **un cliente va a preguntar por él**: se anota para poder responder, fuera de la ruta permisiva. | 35.925 |
| **GameReverie** | — | 🔵 **GAP, no rechazo.** Varias fichas de lanzamiento (Product Hunt, prensa de nicho) lo describen como *Codex Skill* **MIT** para desarrollo de juegos multi-agente, sep-2026. **No tiene slug resoluble**: `search/repositories q=gamereverie` → `total_count: 0`, y ninguna fuente publica la URL del repo. **No se escribe fila sin repo verificable.** | n/d |


---

## 📜 Passes anteriores

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
