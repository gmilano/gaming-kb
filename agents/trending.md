---
industry: gaming
region: Global
updated: 2026-11-22
---

# Agentes trending — Gaming AI

> **Archivo APPEND-ONLY.** Cada pass agrega una sección fechada arriba y conserva
> la historia completa debajo. La historia es el dato: sin ella no hay tendencia,
> sólo una foto.

---

## 🆕 2026-11-22 — pass cycle 19

**Tema del pass:** el **plano de decisión** — la capa que decide qué ve y qué paga
cada jugador (*remote config*, *feature flags*, asignación de experimentos,
recomendación, LTV). En las 8 páginas de esta KB había **cero** ocurrencias de
`openfeature`, `growthbook`, `flagsmith`, `flipt`, `unleash`, `gorse`, `bandit` y
`remote config` antes de hoy. **Método nuevo: canal 7 — el censo del carve-out y su
deriva.**

### 🔴 El movimiento de licencia de la semana: Unleash dejó de ser Apache-2.0 (y es de mayo, no de hoy)

| Medición | Valor |
|---|---|
| Commit | `34b5661eb` · **2026-05-26** · *"task: Move to AGPL-v3 (#12086)"* |
| `LICENSE` antes → después | **11.316 B** (Apache-2.0) → **34.523 B** (AGPL-3.0) |
| Paquete npm hoy | `unleash-server` **8.3.1**, `"license": "AGPL-3.0-or-later"` |
| Último release permisivo | **v7.6.4**, 2026-05-14 |
| SDKs | **siguen Apache-2.0 / MIT**, siete repos leídos uno por uno |
| ⚠️ Renames | `unleash-client-go` y `unleash-proxy-client-js` → **422**; el patrón nuevo es `unleash-<lenguaje>-sdk` |

**Por qué entra en un archivo de tendencia:** el relicenciamiento tiene **seis meses**
y casi todas las comparativas y roundups lo siguen publicando como Apache-2.0. La
tendencia no es el cambio: es **la latencia entre el cambio y lo que la web dice**.

### 🔴 Tres licencias en un repo: `flipt-io/flipt`

Rama por defecto `v2` = **FCL-1.0-MIT**; rama `main` (v1, viva, último commit
2026-11-17) = **GPL-3.0** (35.149 B); `sdk/go/`, `errors/`, `rpc/flipt/` = **MIT**
(1.076 B). Lo que decide cuál te toca **no es el repo, es el comando**. 🟢 La FCL
permite expresamente el uso *"in connection with professional services that you
provide to a licensee"* — primera licencia no-OSI de esta KB que contempla por
escrito el modelo de una consultora.

### 🆕 Agentes y herramientas nuevos verificados este pass (cycle 19)

Existencia y ★ por `gh api search/repositories`; licencia por byte-read de
`raw.githubusercontent.com`. Todo el 2026-11-22.

| Repo | ★ | Licencia | Capa | Último push |
|---|---|---|---|---|
| [recommenders-team/recommenders](https://github.com/recommenders-team/recommenders) | 22.009 | **MIT** | personalización | 2026-11-05 |
| [Unleash/unleash](https://github.com/Unleash/unleash) | 14.337 | ⛔ **AGPL-3.0** (v8) | flags | 2026-11-20 |
| [gorse-io/gorse](https://github.com/gorse-io/gorse) | 9.927 | **Apache-2.0** | recomendación | 2026-11-20 |
| [VowpalWabbit/vowpal_wabbit](https://github.com/VowpalWabbit/vowpal_wabbit) | 8.742 | **BSD-3-Clause** | bandits | 2026-10-15 |
| [growthbook/growthbook](https://github.com/growthbook/growthbook) | 8.645 | 🟡 **MIT Expat** + carve-out 4,55 % | flags + estadística | 2026-11-21 |
| [Flagsmith/flagsmith](https://github.com/Flagsmith/flagsmith) | 8.533 | ✅ **BSD-3-Clause** | flags + remote config | 2026-11-21 |
| [py-why/dowhy](https://github.com/py-why/dowhy) | 8.410 | **MIT** | causalidad | 2026-11-17 |
| [uber/causalml](https://github.com/uber/causalml) | 6.047 | **Apache-2.0** | uplift | 2026-10-12 |
| [flipt-io/flipt](https://github.com/flipt-io/flipt) | 4.953 | 🟡 **FCL/GPL/MIT** (3 grants) | flags GitOps | 2026-11-21 |
| [py-why/EconML](https://github.com/py-why/EconML) | 4.861 | **MIT** + apéndice BSD-3 | efectos heterogéneos | 2026-11-20 |
| [RUCAIBox/RecBole](https://github.com/RUCAIBox/RecBole) | 4.620 | **MIT** | recomendación | ⚠️ 2025-02-24 |
| [meta-pytorch/torchrec](https://github.com/meta-pytorch/torchrec) | 2.629 | **BSD-3-Clause** | recomendación a escala | 2026-11-22 |
| [thomaspoignant/go-feature-flag](https://github.com/thomaspoignant/go-feature-flag) | 2.145 | ✅ **MIT** | flags sin DB | 2026-11-22 |
| [facebookexperimental/Robyn](https://github.com/facebookexperimental/Robyn) | 1.539 | **MIT** | MMM / UA | 2026-11-10 |
| [pymc-labs/pymc-marketing](https://github.com/pymc-labs/pymc-marketing) | 1.314 | **Apache-2.0** | CLV/LTV + MMM | 2026-11-21 |
| [open-feature/spec](https://github.com/open-feature/spec) | 1.299 | ✅ **Apache-2.0** | estándar CNCF | 2026-11-21 |
| [open-feature/flagd](https://github.com/open-feature/flagd) | 1.029 | ✅ **Apache-2.0** | evaluador | 2026-11-19 |
| [featurevisor/featurevisor](https://github.com/featurevisor/featurevisor) | 816 | ✅ **MIT** | flags como build | 2026-11-18 |
| [st-tech/zr-obp](https://github.com/st-tech/zr-obp) | 720 | **Apache-2.0** | off-policy eval | ⚠️ 2024-06-03 |
| [prebid/prebid-server](https://github.com/prebid/prebid-server) | 594 | **Apache-2.0** | subasta de ads | 2026-11-21 |
| [open-feature/go-sdk](https://github.com/open-feature/go-sdk) | 257 | ✅ **Apache-2.0** | SDK | 2026-11-22 |
| [hyodotdev/openiap](https://github.com/hyodotdev/openiap) | 171 | ✅ **MIT** | compras in-app | 2026-11-21 |
| [tryabby/abby](https://github.com/tryabby/abby) | 167 | ⛔ **AGPL-3.0** | flags | ⚠️ 2025-07-24 |
| [open-feature/dotnet-sdk](https://github.com/open-feature/dotnet-sdk) | 143 | ✅ **Apache-2.0** | SDK (vía Unity) | 2026-11-22 |
| [open-feature/java-sdk](https://github.com/open-feature/java-sdk) | 131 | ✅ **Apache-2.0** | SDK | 2026-11-16 |
| [OpenAttribution/open-attribution](https://github.com/OpenAttribution/open-attribution) | 86 | **Apache-2.0** | MMP / atribución | ⚠️ 2025-09-28 |
| [prebid/prebid-mobile-android](https://github.com/prebid/prebid-mobile-android) | 86 | **Apache-2.0** | ads cliente | 2026-11-20 |
| [Unleash/unleash-edge](https://github.com/Unleash/unleash-edge) | 67 | **MIT** salvo `crates/enterprise/` | edge de flags | 2026-11-20 |
| [statsig-io/statsig-server-core](https://github.com/statsig-io/statsig-server-core) | 23 | **ISC** | SDK (plataforma cerrada) | 2026-11-10 |

**28 repos nuevos, 0 heredados.** Cuatro con ⚠️ por inactividad (RecBole 21 meses,
zr-obp 29, abby 16, open-attribution 14) y se listan **con la marca puesta**, no
omitidos: saber que algo existe y está parado es un dato distinto de no saber que
existe.

### 🔴 Tendencia de archivo: el muerto se queda con las estrellas

`hyochan/react-native-iap` = **3.219 ★** y `archived=true`, con el sucesor declarado
en su propia descripción. `hyodotdev/openiap` = **171 ★**, MIT, vivo. **19× a favor
del archivado.** Cualquier ranking por tracción de esta capa corona el cadáver ⇒
`archived` pasa de campo informativo a **filtro obligatorio**.

### 📉 Lo que se buscó y no está (medido, no inferido)

**Cero** SDKs Unity y **cero** Unreal entre las ocho plataformas permisivas de esta
capa. **Cero** mediación de publicidad open source. **Un** MMP open source y
dormido. El gap no es la plataforma: es **el último metro hasta el cliente de
juego**.

---

## 🆕 2026-11-22 — pass cycle 18

**Tema del pass:** el **canal 6 — el árbol completo**. Los cycles 1-17 leyeron
licencias cuya **ruta se podía adivinar**: la raíz, `LICENSE.md`, `licenses/`, lo
que el README nombraba. Este pass agrega el canal que lee las que **no se pueden
adivinar**, y el primer repo al que se le aplica resulta tener **28 archivos de
licencia donde esta KB publicaba 7** — uno de ellos **no comercial**.

### 🔧 El canal 6, y por qué cierra un gap declarado durante 5 passes

`intel/trends.md` viene publicando, sin cambios, esto:

> *"**Listado de directorios de un repo**: sigue sin canal. Por eso el grant
> partido de Matrix-Game se verifica por sonda de rutas conocidas y no puede
> afirmarse que `Matrix-Game-4/` sea el único subdirectorio con licencia propia."*

**Sí hay canal.** No es la API de GitHub (`gh api repos/<slug>/contents` devuelve
**403** fuera de los repos de la sesión); es **git**:

```bash
git clone --filter=blob:none --no-checkout --depth 1 https://github.com/<slug> <dir>
cd <dir>
git ls-tree -r --name-only HEAD                 # el árbol COMPLETO, sin bajar un byte de contenido
git cat-file -p HEAD:<ruta>                     # el texto de CUALQUIER archivo, a demanda
```

El clon *blobless* baja el árbol y **ningún blob**: `SkyworkAI/Matrix-Game` son
**2.765 archivos** listados sin descargar el repo. Medido este pass en **41 repos,
0 fallos de clonado**.

| Canal | Qué ve | Limitación que el canal 6 levanta |
|---|---|---|
| 1 · Badge (`search/repositories`) | 1 veredicto por repo | Elige **un** archivo, y puede elegir uno vendorizado (AccessKit, cycle 16) |
| 2 · Archivo en una ruta conocida | el texto, en bytes | **Hay que saber la ruta** |
| 3 · Enumeración por sonda | las rutas que se adivinan | No distingue *"no existe"* de *"no se me ocurrió"* |
| 4 · Prosa del README | lo que el proyecto declara | Puede estar desactualizada |
| 5 · `git log --follow -- LICENSE` | la **historia** del archivo | Sólo de los archivos que ya conocés |
| 🆕 **6 · `git ls-tree -r HEAD`** | **todos los archivos que existen** | — |

### 🔴 Hallazgo nº1: `SkyworkAI/Matrix-Game` tiene **28** archivos de licencia, no 7 — y uno es **CC BY-NC 4.0**

`repos/foundations.md` publica desde el cycle 13: *"**7 archivos** (raíz MIT;
`Matrix-Game-1/3/4` Apache-2.0; `licenses/MG4CF-LICENSE` Apache-2.0;
`licenses/TAEHV-LICENSE` MIT; `web/mediamtx-reader.LICENSE` MIT). El pass anterior
publicó 2 y declaró que no podía saber si había más. Hay 7."*

**Hay 28.** Los 7 son correctos; los otros 21 viven bajo `third_party/` y **ninguna
sonda los podía alcanzar**. El que decide una propuesta:

| Ruta | Bytes | Grant | Consecuencia |
|---|---|---|---|
| 🔴 `Matrix-Game-1/GameWorldScore/GameWorld/third_party/amt/LICENSE` | **18.208** | ⛔ **CC BY-NC 4.0** (*Attribution-NonCommercial*) | **Exclusión dura.** `NonCommercial` dentro de un repo cuya raíz es MIT |
| `.../third_party/DROID-SLAM/thirdparty/eigen/COPYING.GPL` | 35.147 | GPL-3.0 (texto de Eigen) | Eigen es MPL-2.0; arrastra los textos GPL/LGPL de sus módulos legacy |
| `.../eigen/COPYING.LGPL` | 26.530 | LGPL-2.1 | idem |
| `.../eigen/COPYING.MPL2` | 16.726 | MPL-2.0 (el grant real de Eigen) | copyleft **por archivo**, manejable |
| `.../eigen/bench/btl/COPYING` | 18.109 | **GPL-2.0** | sólo el benchmark de Eigen |
| `.../third_party/DROID-SLAM/LICENSE` · `RAFT/LICENSE` · `lietorch/LICENSE` | 1.539 / 1.512 / 1.512 | BSD-3-Clause | permisivos, atribución obligatoria |
| `.../tartanair_tools/LICENSE` | 1.496 | Carnegie Mellon University | permisivo |
| `Matrix-Game-4/game/third_party/{DirectXMath,JoltPhysics,recastnavigation,stb}` | 1.141 / 1.059 / 877 / 2.510 | MIT · MIT · **zlib** · dual PD-o-MIT | ⚠️ `recastnavigation` es **el mismo repo** que esta KB ya recomendaba suelto |

**Dónde está exactamente el problema:** `amt` (*All-pairs Multi-field Transforms*,
interpolación de frames) **no está en el modelo**. Está en **`GameWorldScore`**, que
es el *benchmark* con el que Matrix-Game demuestra que el modelo funciona.

⇒ **Se puede embarcar el modelo. No se puede correr su propio benchmark dentro de
un entregable pago** sin extirpar `amt` o reimplementar la métrica. Ver Receta 47.

**Y un dato que sólo el árbol da:** `Matrix-Game-2/` tiene **88 archivos y ninguna
licencia propia** ⇒ cae bajo el MIT de la raíz. Eso no era afirmable antes.

### 🔴 Hallazgo nº2: `4ian/GDevelop` (29.166 ★) **no es "MIT en todo el código"** — embarca un runtime con licencia **paga por usuario**

El cycle 11 resolvió el badge `NOASSERTION` de GDevelop probando **4 carpetas** y
publicó *"MIT en todo el código"*. El árbol tiene **11 archivos de licencia**. Las
4 probadas son MIT y están bien; entre las 7 restantes hay una que no es open
source:

> `Extensions/Spine/spine-pixi-v7/Spine-Runtimes-License-Agreement.txt` (**1.472 B**,
> Esoteric Software LLC, 2024-02-20): *"provided that **each user of the Products
> must obtain their own Spine Editor license**"*.

⇒ **No es copyleft ni es no comercial: traslada un costo por-usuario a un tercero.**
Un juego vendido a jugadores finales que use la extensión Spine obligaría a **cada
jugador** a comprar Spine Editor. En la práctica, inviable.

Y el archivo de la raíz (`LICENSE.md`, **305 B**) **no es un grant**: es un puntero
(*"See the license.md in each folders"*). Es el segundo archivo de este tipo medido
este pass, junto al `LICENSE.txt` de o3de ⇒ **categoría nueva: el archivo de
licencia que no licencia.**

**GDevelop se mantiene como vertical recomendada**, con la condición explícita de
excluir `Extensions/Spine/` del build. Detalle en `verticals/solutions.md`.

### 🆕 El eje de licencia nº 9: **por *feature* activada**

Las Recetas 14 y 20 enumeran 8 ejes de partición de un grant. GDevelop agrega el
noveno y el peor de auditar: **la licencia efectiva de un engine *no-code* depende
de qué extensiones activó el diseñador**, es decir, de un hecho que ocurre **en el
proyecto del cliente y después** de que legales revisó el repo. ⇒ La auditoría se
corre sobre el **manifiesto del proyecto**, no sobre el repo del engine.

### 🆕 T68 · Un filtro anclado al principio del nombre **no ve** los grants con sufijo

Sobre **el mismo árbol**, el mismo pass, dos filtros:

| Filtro del nombre de archivo | Archivos encontrados |
|---|---|
| empieza con `licen[cs]e\|copying\|notice` | **23** |
| **contiene** `licen[cs]e\|copying\|notice` | **28** |

Los 5 invisibles son exactamente los que un proveedor usa para licenciar **un
componente**: `MG4CF-LICENSE`, `TAEHV-LICENSE`, `SAL-LICENSE.txt`,
`mediamtx-reader.LICENSE`, `THIRD_PARTY_NOTICES.md` — es decir, **los tres que esta
KB ya había encontrado a mano** más dos nuevos. Un canal nuevo con un filtro malo
habría *perdido* lo que la sonda ya tenía.

⇒ **Regla:** el filtro se escribe sobre el **basename completo**, con `contains`,
nunca con ancla inicial.

### 🆕 T69 · Lo no-comercial no entra por el modelo, entra por el **banco de pruebas**

De **41 repos y 329+ archivos de licencia** medidos este pass, hay **un solo grant
no comercial**, y está a **seis niveles de profundidad dentro de un benchmark**. El
componente contaminado nunca es el titular: el titular es el que el equipo legal
mira.

### 🆕 T70 · Apache-2.0 es íntegro en **cuatro** byte-counts distintos; el discriminador es la **línea**, no el byte

El cycle 16 midió el Apache-2.0 sin apéndice (AccessKit, 9.723 B). Este pass agrega
un segundo testigo y dos variantes más:

| Repo | Archivo | Bytes | Líneas | `APPENDIX` | Veredicto |
|---|---|---|---|---|---|
| [AccessKit/accesskit](https://github.com/AccessKit/accesskit) | `LICENSE-APACHE` | 9.723 | — | ❌ | Apache-2.0 íntegro |
| 🆕 [IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP) | `LICENSE` | **10.197** | **176** | ❌ | Apache-2.0 íntegro |
| 🆕 [contentauth/c2pa-rs](https://github.com/contentauth/c2pa-rs) | `LICENSE-APACHE` | **11.335** | **201** | ✅ | Apache-2.0 íntegro |
| 🆕 [o3de/o3de](https://github.com/o3de/o3de) | `LICENSE_APACHE2.TXT` | **11.557** | **201** | ✅ | Apache-2.0 íntegro |

Los cuatro se leyeron íntegros buscando cláusulas añadidas (`additional`,
`notwithstanding`, `shall not be used`, `trademark of`): **ninguno está adulterado**.
La diferencia entre 11.335 y 11.557 con **las mismas 201 líneas** es relleno del
placeholder `[name of copyright owner]` y espacios finales.

⇒ **Discriminador correcto:** `wc -l` (**201** con apéndice, **~176** sin) + la
presencia del literal `END OF TERMS AND CONDITIONS`. El byte-count queda como
**triage**, nunca como veredicto — que es lo que esta KB viene diciendo desde el
cycle 13, ahora con el reemplazo concreto.

### 🟢 Control negativo (lo que **no** apareció, que es la mitad del dato)

| Medición | Resultado |
|---|---|
| Repos clonados por canal 6 | **41**, 0 fallos |
| Archivos de licencia enumerados | **329+** |
| Repos con grant **no comercial** en cualquier parte del árbol | **1** (Matrix-Game) |
| Repos con copyleft fuerte **en la raíz** | **1** ([ALE](https://github.com/Farama-Foundation/Arcade-Learning-Environment), GPL-2.0 — ya publicado) |
| Repos con copyleft **débil** vendorizado | **1** ([agones](https://github.com/agones-dev/agones): `vendor/hashicorp/golang-lru` 15.921 B + `hcl` 15.977 B, MPL-2.0 por archivo, dentro de `vendor/` ⇒ benigno) |
| Repos con más grants vendorizados | agones **149**, [nakama](https://github.com/heroiclabs/nakama) **81**, Matrix-Game **28**, [pinus](https://github.com/node-pinus/pinus) **14** |

⇒ **El stack permisivo que esta KB recomienda está limpio a nivel de árbol.** Eso
ahora está **medido**, no supuesto.

### 🔁 Reconfirmaciones por un canal independiente

El canal 6 reprodujo, sin conocerlas de antemano, cinco conclusiones que esta KB
había sacado por sonda o por badge:

| Repo | Lo que la KB publicaba | Canal 6 |
|---|---|---|
| [AccessKit/accesskit](https://github.com/AccessKit/accesskit) | badge `BSD-3-Clause` = grant **vendorizado**, real es MIT **o** Apache | ✅ `LICENSE-MIT` 1.023 + `LICENSE-APACHE` 9.723 + `LICENSE.chromium` 1.559 |
| [o3de/o3de](https://github.com/o3de/o3de) | Apache-2.0 **o** MIT | ✅ + `LICENSE.txt` (1.475) es una **política**, no un grant: *"default license is Apache-2.0… you may elect at your option… MIT"* |
| [luanti-org/luanti](https://github.com/luanti-org/luanti) | código LGPL-2.1+, medios aparte | ✅ `COPYING.LESSER` 26.415 + `LICENSE.txt` 8.793 = **sólo texturas y sonidos**. No hay otro grant en la raíz |
| [contentauth/c2pa-rs](https://github.com/contentauth/c2pa-rs) | MIT **o** Apache (badge `NOASSERTION`) | ✅ dual confirmado |
| [MineDojo/NitroGen](https://github.com/MineDojo/NitroGen) | `NVIDIA License`, **4.729 B**, no comercial | ✅ **4.729 B exactos**, y el árbol tiene **un solo** archivo de licencia |

### ⛔ Canales re-testeados este pass (la regla de la rotación: un canal bloqueado se reintenta, no se hereda)

| Canal | Resultado 2026-11-22 | Estado |
|---|---|---|
| `huggingface.co/api/*` | código **000** (sin conexión) | 🔴 sigue muerto ⇒ **licencia de los pesos sigue sin verificar** |
| `eur-lex.europa.eu` | código **000** | 🔴 sigue muerto ⇒ fuente primaria del EU AI Act sin leer, **6º pass** |
| `gh api repos/<slug>/contents` | **403** *"GitHub access to this repository is not enabled for this session"* | 🔴 bloqueado fuera de los repos de la sesión |
| `gh api search/repositories` | 200, SPDX + ★ + `pushed_at` + `archived` | 🟢 vivo |
| 🆕 `git clone --filter=blob:none --no-checkout` | 41/41 | 🟢 **canal 6, nuevo** |

### 📊 Metadatos releídos este pass (canal 1)

| Repo | ★ | Badge SPDX | Último push | Nota |
|---|---|---|---|---|
| [CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp) | **16.112** | MIT | 2026-11-17 | +25 desde el cycle 4 |
| [luanti-org/luanti](https://github.com/luanti-org/luanti) | **13.886** | `NOASSERTION` | 2026-11-20 | LGPL-2.1+ por árbol |
| [heroiclabs/nakama](https://github.com/heroiclabs/nakama) | **13.763** | Apache-2.0 | 2026-11-18 | 81 grants vendorizados, todos permisivos |
| [o3de/o3de](https://github.com/o3de/o3de) | **9.897** | `NOASSERTION` | **2026-11-22 (hoy)** | dual Apache/MIT |
| [recastnavigation/recastnavigation](https://github.com/recastnavigation/recastnavigation) | **8.205** | Zlib | ⚠️ 2026-02-27 (9 meses) | |
| [agones-dev/agones](https://github.com/agones-dev/agones) | **7.107** | Apache-2.0 | 2026-11-20 | |
| [IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP) | **4.681** | Apache-2.0 | **2026-11-22 (hoy)** | |
| [SkyworkAI/Matrix-Game](https://github.com/SkyworkAI/Matrix-Game) | **2.956** | MIT | ⚠️ 2026-10-29 | 🔴 **el badge MIT no ve el CC BY-NC del árbol** |
| [Farama-Foundation/Arcade-Learning-Environment](https://github.com/Farama-Foundation/Arcade-Learning-Environment) | 2.463 | GPL-2.0 | 2026-11-15 | |
| [MineDojo/NitroGen](https://github.com/MineDojo/NitroGen) | **2.252** | `NOASSERTION` | ⚠️ 2026-01-25 | ⛔ no comercial |
| [AccessKit/accesskit](https://github.com/AccessKit/accesskit) | 1.567 | `BSD-3-Clause` ❌ | 2026-11-21 | badge = grant vendorizado |
| [contentauth/c2pa-rs](https://github.com/contentauth/c2pa-rs) | 453 | `NOASSERTION` | 2026-11-21 | MIT **o** Apache |

---

## 🆕 2026-11-22 — pass cycle 17

**Tema del pass:** la **fábrica** — versionado de binarios, CI de build, caché de
compilación, seguimiento de producción y automatización de QA. Seis categorías que
tenían **cero filas en 16 passes**. El hallazgo del pass no es un repo: es que en
esta capa **la licencia cambia con el tiempo y con el dueño**, y los cuatro canales
de licencia de esta KB sólo ven el presente.

### Lo que entró (verificado, licencia leída byte a byte)

| Repo | ★ | Licencia (bytes) | Capa | Último push |
|---|---|---|---|---|
| [AirtestProject/Airtest](https://github.com/AirtestProject/Airtest) | 9.657 | **Apache-2.0** (11.356) | QA de UI por reconocimiento de imagen (NetEase) | 2026-03-23 |
| [AirtestProject/Poco](https://github.com/AirtestProject/Poco) | 1.964 | **Apache-2.0** (11.356) | inspección de jerarquía cross-engine | ⚠️ 2024-01-08 (34 meses) |
| [mozilla/sccache](https://github.com/mozilla/sccache) | 7.968 | **Apache-2.0** (11.358) | caché de compilación | **2026-11-22 (hoy)** |
| [git-lfs/git-lfs](https://github.com/git-lfs/git-lfs) | 14.610 | **MIT + BSD-3** (2.818) | versionado de binarios | 2026-11-06 |
| [treeverse/dvc](https://github.com/treeverse/dvc) | 15.993 | **Apache-2.0** (11.340) | versionado de assets grandes | 2026-11-20 |
| [game-ci/unity-builder](https://github.com/game-ci/unity-builder) | 1.113 | **MIT** (1.097) | CI de Unity | 2026-11-15 |
| [game-ci/docker](https://github.com/game-ci/docker) | 472 | **MIT** (1.071) | imágenes de runner | **2026-11-22 (hoy)** |
| [game-ci/unity-test-runner](https://github.com/game-ci/unity-test-runner) | 271 | **MIT** (1.097) | tests de Unity en CI | 2026-11-15 |
| [JetBrains/teamcity-unreal-engine-plugin](https://github.com/JetBrains/teamcity-unreal-engine-plugin) | 21 | **Apache-2.0** (11.357) | CI de Unreal (única permisiva medida) | 2026-08-19 |
| [ynput/ayon-core](https://github.com/ynput/ayon-core) | 79 | **Apache-2.0** (11.357) | pipeline de producción (cliente) | 2026-11-21 |

### Movimientos de licencia detectados por el **canal 5** (`git log --follow -- LICENSE`)

| Repo | Movimiento | Fecha | Consecuencia |
|---|---|---|---|
| [ynput/ayon-backend](https://github.com/ynput/ayon-backend) | 🔴 Apache-2.0 → **FSL-1.1-ALv2** (commit `97627e27`) | **2025-01-27** | última release Apache: **1.6.1**; la 1.7.0 convierte el **2027-01-30** |
| [ynput/ayon-frontend](https://github.com/ynput/ayon-frontend) | 🔴 **FSL-1.1-ALv2** (3.744 B) | — | mismo corte que el backend |
| [treeverse/dvc](https://github.com/treeverse/dvc) | 🟡 copyright `Iterative, Inc.` → **`Treeverse.`** (commit `046d2fd7`), grant **sin cambios** | **2025-12-01** | dueño nuevo cuyo producto insignia es **BSL-1.1** |
| [cgwire/zou](https://github.com/cgwire/zou) | 🟢 AGPL-3.0 **desde el commit inicial** | 2017-03-26 | estabilidad confirmada, 9 años |
| [AirtestProject/Airtest](https://github.com/AirtestProject/Airtest) | 🟢 Apache-2.0 **desde el commit inicial** | 2018-01-24 | estabilidad confirmada, 8 años |

### Lo que NO entró, y por qué (rechazos medidos)

| Repo | ★ | Motivo |
|---|---|---|
| [alttester/AltTester-Unity-SDK](https://github.com/alttester/AltTester-Unity-SDK) | 128 | **GPL-3.0** (35.043 B) **dentro del build** + server batch con suscripción Pro |
| [cgwire/kitsu](https://github.com/cgwire/kitsu) + [cgwire/zou](https://github.com/cgwire/zou) | 774 + 202 | **AGPL-3.0** (34.520 B): adoptable, no derivable |
| [treeverse/lakeFS](https://github.com/treeverse/lakeFS) | 5.590 | **BSL-1.1** (7.475 B) — valor nuevo del checksum |
| [ynput/OpenPype](https://github.com/ynput/OpenPype) | 301 | MIT pero **archivado** (2024-09-18, 26 meses) y todavía recomendado por terceros |
| [game-ci/unity-actions](https://github.com/game-ci/unity-actions) | 1.097 | MIT pero **44 meses sin commit**; es el que devuelven las guías |
| [team-zomsa/aplib.net](https://github.com/team-zomsa/aplib.net) | 9 | **AGPL-3.0** por short-form (709 B, Utrecht University; el API dice `NOASSERTION`) + 29 meses dormido |

### Lo que se buscó y **no existe** (se publica para que el silencio no se lea como cobertura)

- CI de Unreal open source y permisiva equivalente a `game-ci`: **no existe**. Horde
  (Epic) se publica bajo EULA de UE y su CI **no soporta Git ni SVN, sólo Perforce**.
- Repo de la arquitectura de referencia de QA con Bedrock de AWS: **anunciado, no
  publicado**.
- Reemplazo vivo de Poco para inspección de jerarquía cross-engine permisiva: **no hay**.

---

## 🆕 2026-11-22 — pass cycle 16

**Tema del pass:** el **plano de datos del jugador** (telemetría, analítica,
crash reporting, live-ops). Primera capa de esta KB donde **el líder de la
categoría prohíbe explícitamente la producción**, y donde la respuesta de la
comunidad no fue un competidor nuevo sino **un fork del último commit permisivo**.

### Lo que entró (verificado, licencia leída byte a byte)

| Repo | ★ | Licencia (bytes) | Capa | Último push |
|---|---|---|---|---|
| [opensnowcat/opensnowcat-collector](https://github.com/opensnowcat/opensnowcat-collector) | 81 | **Apache-2.0** (11.358) | ingesta de eventos | 2026-11-13 |
| [opensnowcat/opensnowcat-enrich](https://github.com/opensnowcat/opensnowcat-enrich) | 40 | **Apache-2.0** (11.359) | enriquecido | 2026-11-13 |
| [ClickHouse/ClickHouse](https://github.com/ClickHouse/ClickHouse) | 51.060 | **Apache-2.0** | almacén de eventos | 2026-11-22 |
| [ClickHouse/ClickStack](https://github.com/ClickHouse/ClickStack) | 10.061 | **MIT** (1.077) | observabilidad | 2026-11-21 |
| [apache/superset](https://github.com/apache/superset) | 75.615 | **Apache-2.0** | BI | 2026-11-22 |
| [open-telemetry/opentelemetry-collector](https://github.com/open-telemetry/opentelemetry-collector) | 7.793 | **Apache-2.0** | trazas | 2026-11-20 |
| [tx7do/go-wind-uba](https://github.com/tx7do/go-wind-uba) | 51 | **MIT** (1.061) | analítica de comportamiento | 2026-11-20 |
| [mirlan-irokez/rawbbit](https://github.com/mirlan-irokez/rawbbit) | 11 | **Apache-2.0** (11.349) | analítica de juego | 2026-11-09 |
| [EricSun0218/OpenGameAgent](https://github.com/EricSun0218/OpenGameAgent) | 61 | **MIT** (1.096) | runtime de NPC | 2026-11-17 |
| [Eurekaleo/awesome-ai-for-games](https://github.com/Eurekaleo/awesome-ai-for-games) | 391 | **MIT** | índice | 2026-11-19 |
| [SubmitGame/awesome-ai-game-dev-tools](https://github.com/SubmitGame/awesome-ai-game-dev-tools) | 93 | **CC0-1.0** | índice | 2026-11-21 |
| GitLab: `glitchtip/glitchtip-backend` | — | **MIT** (1.067) | crash reporting | — |

### La señal de tendencia de este pass

- **El gradiente de estrellas invertido, 2ª reproducción en esta KB.** 7.051 ★ en
  `snowplow/snowplow`, que es documentación y es Apache-2.0; **40 ★ y 31 ★** en
  `stream-collector` y `enrich`, que son el producto y están bajo SLULA. La primera
  reproducción fue el clúster `godot-mcp` (cycle 10). **Las estrellas miden
  atención, no el objeto que vas a instalar.**
- **La continuación permisiva de una capa madura es un FORK, no un competidor.**
  OpenSnowcat recibió commits hace nueve días con 81 ★. Nadie escribió un pipeline
  de eventos nuevo: forkearon el último commit Apache-2.0 y lo sostienen.
- **El carve-out por directorio llegó a tres vendors distintos** (PostHog `ee/`,
  Metabase `enterprise/`, OpenReplay `ee/`). Dejó de ser anécdota.
- **Un repo de 0 ★ puede ser el único de su categoría.** `GameTuner` es la única
  plataforma de analítica pensada para juegos, es Apache-2.0 y tiene **cero**
  estrellas en sus diez repos. El filtro por estrellas de un pass anterior la habría
  descartado sin verla.

### Lo que NO entró, medido

`snowplow/stream-collector` y `snowplow/enrich` (SLULA: producción prohibida) ·
`getsentry/sentry` (FSL-1.1) · `Countly/countly-server` (AGPL §7 modificada) ·
`metabase/metabase` (AGPL + comercial) · `openreplay/openreplay` (carve-out `ee/`) ·
`grafana/grafana` y `rybbit-io/rybbit` (AGPL-3.0) · `CrateBytes/CrateBytes`
(**archivado** 2025-01-22) · `AIGeeksGroup/Code2Games` (89 ★, **sin licencia**) ·
`ADM1SH/churn-prediction-scaling` (sin licencia) · `deq-labs/analytics-platform`
(AGPL-3.0, 0 ★).

---

## 🆕 2026-11-22 — pass cycle 15

**Tema del pass:** la capa operativa de cara al jugador —accesibilidad,
anti-cheat, soporte, moderación de voz—. **Primera capa de esta KB donde la
enumeración devuelve más obligación que oferta.**

### Lo que entró (verificado, licencia leída byte a byte)

| Repo | ★ | Licencia (bytes) | Capa | Último push |
|---|---|---|---|---|
| [AccessKit/accesskit](https://github.com/AccessKit/accesskit) | 1.567 | **Apache-2.0 OR MIT** (9.723 + 1.023) | accesibilidad | 2026-11-21 |
| [ethindp/prism](https://github.com/ethindp/prism) | 67 | **MPL-2.0** (15.504, Markdown) | accesibilidad | 2026-11-21 |
| [mikrima/UnityAccessibilityPlugin](https://github.com/mikrima/UnityAccessibilityPlugin) | 85 | **Apache-2.0** (11.357 ✅) | accesibilidad | ⚠️ 2023-05-13 |
| [jumpingmushroom/Earshot](https://github.com/jumpingmushroom/Earshot) | 1 | **MIT** (1.069) | captions | 2026-10-18 |
| [dominicbytes/game-accessibility-skill](https://github.com/dominicbytes/game-accessibility-skill) | 1 | **MIT** | agent skill | 2026-10-18 |
| [SanderMertens/flecs](https://github.com/SanderMertens/flecs) | 8.854 | **MIT** (1.131) — badge `NOASSERTION` | ECS | 2026-11-20 |
| [danijar/dreamerv3](https://github.com/danijar/dreamerv3) | 4.004 | **MIT** (1.058) | world models | 2026-10-04 |
| [Farama-Foundation/Arcade-Learning-Environment](https://github.com/Farama-Foundation/Arcade-Learning-Environment) | 2.463 | ⚠️ **GPL-2.0** (17.879) | eval / QA | 2026-11-15 |
| [MichaelTMatthews/Craftax](https://github.com/MichaelTMatthews/Craftax) | 476 | **MIT** (1.059) | benchmark | 2026-10-27 |
| [vivekjoshy/openskill.py](https://github.com/vivekjoshy/openskill.py) | 377 | **MIT** (1.068) | rating / matchmaking | 2026-05-12 |
| [ForgeAX/forgeax-engine](https://github.com/ForgeAX/forgeax-engine) | 116 | **Apache-2.0** (11.358) | engine AI-native | 2026-11-20 |
| [TohmaN233/YN-translation-workshop](https://github.com/TohmaN233/YN-translation-workshop) | 235 | **MIT** | localización | 2026-11-11 |

### ⛔ Rechazos **medidos** de este pass — se publican porque el silencio se lee como cobertura

| Repo | ★ | Motivo |
|---|---|---|
| [waldo-vision/optical.flow.demo](https://github.com/waldo-vision/optical.flow.demo) | 526 | **ARCHIVADO** 2021-11-20. El mayor proyecto de anti-cheat ML |
| [lrq3000/oacs](https://github.com/lrq3000/oacs) | 35 | **Sin archivo de licencia** + push 2017-03-10 |
| [lightsoutgames/godot-accessibility](https://github.com/lightsoutgames/godot-accessibility) | 87 | MIT, pero **sin push desde 2021-05-19**; absorbido por el engine en Godot 4.5 |
| [m1maker/SRAL](https://github.com/m1maker/SRAL) | 21 | MIT, **ARCHIVADO**; su README remite a `prism` |
| [Unity-Technologies/a11y-public-sample](https://github.com/Unity-Technologies/a11y-public-sample) | 26 | **Unity Companion License**: no es open source |
| [korpys667/MKAC](https://github.com/korpys667/MKAC) · [gtxcinerbaba-hash/ZeyronAC](https://github.com/gtxcinerbaba-hash/ZeyronAC) | — · 4 | **GPL-3.0**, sólo Minecraft |
| [KaelusAI/Shard](https://github.com/KaelusAI/Shard) · [SoMax1soft/mls-network-plugin](https://github.com/SoMax1soft/mls-network-plugin) | — | El modelo real **no está en el código público** / corre en nube del proveedor |
| [islamyasin07/Ares-AntiCheat-System](https://github.com/islamyasin07/Ares-AntiCheat-System) · [DevInsilicon/MLAAC](https://github.com/DevInsilicon/MLAAC) | 2 · 0 | Licencia `null`, sin tracción |
| [vinifborgess/azure-guardian](https://github.com/vinifborgess/azure-guardian) | 0 | Único candidato de moderación de voz: envoltorio de un servicio Azure |

### 📏 Recuento del pass

| Métrica | Valor |
|---|---|
| Candidatos enumerados en la capa operativa | **23** |
| Vivos **y** permisivos | **2** |
| Incumbentes ausentes recuperados (regla T64) | **5** |
| Licencias leídas byte a byte | **17** |
| Defectos nuevos del checksum de bytes | **2** (5º y 6º) |

---

## 🆕 2026-11-22 — pass cycle 14

**La tendencia de este pass no es qué apareció: es DÓNDE se mudó la
restricción.** Segundo eje consecutivo con tasa de novedad del 100% (24 de 24
candidatos nuevos, 0 filas previas en 13 passes), ahora sobre **producción de
assets**: malla, textura, rig, movimiento, música y SFX.

### 🔴 El movimiento que esta página puede registrar hoy: la restricción salió del archivo de licencia y se mudó a **la lista de dependencias**

Historia registrada en este mismo archivo, pass por pass — y la dirección es
consistente:

| Pass | Dónde vivía la restricción | Ejemplo medido |
|---|---|---|
| cycles 4-5 | **el archivo de licencia de la raíz** | MonoGame Ms-PL detrás de un badge MIT |
| cycle 6-7 | **subdirectorios del mismo repo** | `Matrix-Game-4/` con grant propio |
| cycle 10 | **el territorio** | Tencent: sin grant en EU / UK / Corea |
| cycle 12 | **el carve-out por tipo de entregable** | `ee/`, PolyForm Shield |
| cycle 13 | **el modelo de negocio del licenciatario** | YSPL 1.0, Fish Audio 1.0 (umbral US$ 500k) |
| 🆕 **cycle 14** | **la clausura de dependencias, declarada sólo en prosa del README** | TRELLIS / TRELLIS.2 |

Cada escalón es **menos visible para una auditoría automática** que el anterior.
El de este pass es el primero que **ningún canal que opere sobre el repo puede
ver**, porque el dato no está en el repo: está en lo que el repo te manda a
instalar.

### 🔴 El caso, con las dos cifras que lo hacen tendencia y no anécdota

- [microsoft/TRELLIS](https://github.com/microsoft/TRELLIS) — **14.370 ★**,
  SPDX `MIT`, `LICENSE` MIT estándar, un solo archivo de licencia en todo el
  repo. Su `README.md` dice que `diffoctreerast` *"may have different
  licenses"*. Ese paquete es un derivado declarado de la **Gaussian-Splatting
  License** de Inria + MPII: *"THE USER CANNOT USE, EXPLOIT OR DISTRIBUTE THE
  SOFTWARE FOR COMMERCIAL PURPOSES"*.
- [microsoft/TRELLIS.2](https://github.com/microsoft/TRELLIS.2) — **13.144 ★**,
  mismo perfil. Sus dependencias de render son `nvdiffrast` y `nvdiffrec`,
  ambas **NVIDIA Source Code License**: *"only may be used or intended for use
  non-commercially … research or evaluation purposes only"*.

**27.514 ★ sumadas.** Los dos proyectos más votados de la capa que todo el
mercado está a punto de adoptar, y los dos tienen el mismo problema en el mismo
lugar.

### 🟡 Lo que NO se movió, y conviene decirlo

La adopción. Las encuestas de 2026 que este pass encontró colocan la generación
de assets en **~19% del uso reportado de AI**, contra **~81% en investigación y
brainstorming** (GDC 2026, 2.300+ respondentes, vía resúmenes secundarios — el
informe primario no se recuperó). ⇒ **Esta capa no es donde está la adopción
hoy; es hacia donde se está moviendo.** Que las licencias se estén cerrando
*antes* de que llegue el volumen es precisamente lo que la hace registrable
ahora.

### 🆕 Repos que aparecen por primera vez en esta KB (14 filas, todas verificadas hoy)

| Repo | ★ | Licencia (byte-read) | Veredicto |
|---|---|---|---|
| [microsoft/TRELLIS](https://github.com/microsoft/TRELLIS) | 14.370 | MIT (código y pesos) | ⚠️ renderers NC |
| [microsoft/TRELLIS.2](https://github.com/microsoft/TRELLIS.2) | 13.144 | MIT + HDRIs CC0 | ⚠️ renderers NC |
| [facebookresearch/audiocraft](https://github.com/facebookresearch/audiocraft) | 23.943 | MIT / pesos CC BY-NC 4.0 | ⛔ pesos |
| [multimodal-art-projection/YuE](https://github.com/multimodal-art-projection/YuE) | 13.656 | Apache-2.0 / pesos CC BY-NC 4.0 | ⛔ pesos |
| [ace-step/ACE-Step](https://github.com/ace-step/ACE-Step) | 5.095 | Apache-2.0 | ✅ |
| [Tencent-Hunyuan/Hunyuan3D-2.1](https://github.com/Tencent-Hunyuan/Hunyuan3D-2.1) | 4.519 | Tencent Community | ⛔ sin grant en EU/UK/KR |
| [OpenMOSS/MOSS-TTS](https://github.com/OpenMOSS/MOSS-TTS) | 4.358 | Apache-2.0 **incl. pesos** | ✅ |
| [GuyTevet/motion-diffusion-model](https://github.com/GuyTevet/motion-diffusion-model) | 4.154 | MIT | ⚠️ SMPL |
| [Stability-AI/stable-audio-tools](https://github.com/Stability-AI/stable-audio-tools) | 3.920 | MIT (código) | ⚠️ checkpoints aparte |
| [VAST-AI-Research/UniRig](https://github.com/VAST-AI-Research/UniRig) | 2.189 | MIT raíz / **GPL-3.0 en el path** | 🔴 |
| [OpenMotionLab/MotionGPT](https://github.com/OpenMotionLab/MotionGPT) | 2.006 | MIT | ⚠️ 17 meses dormido |
| [VAST-AI-Research/TripoSG](https://github.com/VAST-AI-Research/TripoSG) | 1.935 | MIT | ⚠️ 19 meses dormido |
| [OpenDCAI/GameFactory-3A](https://github.com/OpenDCAI/GameFactory-3A) | 1.344 | Apache-2.0 | ✅ |
| [Bingeljell/image-to-3dlab](https://github.com/Bingeljell/image-to-3dlab) | 1.244 | Apache-2.0 + NOTICE | ✅ |

**Tasa limpia: 4 de 14.** Es el peor ratio que esta KB ha medido en una capa
nueva — el cycle 13 (texto y voz) dio 9 de 24.

### ⏱️ Sobre los deltas

Este pass corre ~1 hora después del cycle 13. **No se reportan variaciones de
★**: en 60 minutos ninguna se mueve de forma interpretable, y reescribir cifras
heredadas sin cambio de significado destruye la serie que este archivo existe
para conservar. Lo que sí es nuevo son **repos que nunca estuvieron aquí** y
**un eje de restricción que nunca se había registrado**.

---

## 🆕 2026-11-22 — pass cycle 13

**El evento del pass no es un repo: es un eje.** Las búsquedas del runbook sobre
engines / MCP / NPC devolvieron **15 candidatos, de los cuales 13 ya estaban en
esta KB** (tasa de novedad: 13%). Las mismas búsquedas corridas sobre **diálogo
autorado, moderación de chat, voz y localización** devolvieron **24 candidatos, de
los cuales 24 eran nuevos** (tasa de novedad: 100%). Doce passes de profundidad
sobre un eje y **cero** sobre el otro.

⇒ La tendencia que esta página puede registrar hoy y no podía ayer: **la capa de
texto y voz que mira el jugador se está consolidando bajo licencias que no son
MIT ni Apache**, y eso pasó en los últimos ~6 meses.

### 🔴 Movimiento de licencia nº1: nace la **Yarn Spinner Public Licence (YSPL) 1.0**

Yarn Spinner —el estándar de diálogo junto con *ink*— mantiene **seis repos
oficiales** y ya **no** los publica todos igual:

- `YarnSpinner` (2.876 ★), `YarnSpinner-Unity` (854 ★), `YarnSpinner-Godot`
  (218 ★, C#): **MIT**, leído en bytes.
- `YarnSpinner-Rust` (299 ★): **MIT + Apache** (dual).
- `YarnSpinner-Godot-GDScript` (99 ★) y `YarnSpinner-UnrealEngine` (52 ★):
  **YSPL 1.0** — 17.709 B, badge `NOASSERTION`.

**El motor que elige el equipo decide la licencia.** Y la YSPL trae una cláusula
que ninguna otra licencia de esta KB tenía: **prohíbe alimentar el código a
sistemas de entrenamiento de AI**, exigiendo que las herramientas asistidas que
se usen sobre el árbol no retengan el fuente. Es la primera restricción de esta
KB sobre el **tooling** del integrador y no sobre su entregable.

### 🔴 Movimiento de licencia nº2: **Fish Audio Community License 1.0 — fechada 13-oct-2026**

`fishaudio/fish-speech` (33.825 ★, vivo) estrena una licencia **de hace seis
semanas** con un mecanismo nuevo para esta KB: **umbral de facturación**
(US$ 500k/año) — **y el umbral se mide sobre el cliente del integrador**, con
*"contractor, integrator, consultant"* nombrados en el texto. El TTS abierto más
grande del mercado queda fuera de alcance para entrega de software a cualquier
estudio que facture, y dentro de alcance para entrega de **output** (doblaje,
voiceover). Carve-out explícito: *"game or app characters"* no es `Hosted Service`.

### 🔴 Movimiento de licencia nº3: **Piper pasó de MIT a GPL-3.0 al mudarse de repo**

`rhasspy/piper` (11.336 ★, **MIT**) quedó **archivado** el 2025-08-26. El sucesor
vivo es `OHF-Voice/piper1-gpl` (6.347 ★, **GPL-3.0**, `COPYING` de 35.148 B,
commit de ayer). **El cambio de licencia está anunciado en el propio nombre del
repo** y aun así no aparece en ninguna guía de stack: quien escribió *"Piper,
MIT"* en un documento de arquitectura está citando un repo muerto.

### 🆕 Repos nuevos de este pass — 16 filas, las 16 con licencia leída byte a byte

| Repo | ★ | Licencia (byte-read) | Capa | Push |
|------|---|----------------------|------|------|
| [openai/whisper](https://github.com/openai/whisper) | 113.247 | MIT (1.063 B) | voz / STT | 2026-08-31 |
| [ggml-org/whisper.cpp](https://github.com/ggml-org/whisper.cpp) | 55.409 | MIT (1.078 B) | voz / STT on-device | 2026-11-20 |
| [SYSTRAN/faster-whisper](https://github.com/SYSTRAN/faster-whisper) | 26.929 | MIT (1.064 B) | voz / STT servidor | 2026-11-12 |
| [SWivid/F5-TTS](https://github.com/SWivid/F5-TTS) | 15.633 | MIT (1.068 B) | voz / TTS | 2026-11-03 |
| [i18next/i18next](https://github.com/i18next/i18next) | 8.666 | MIT (1.082 B) | localización | 2026-11-21 |
| [guardrails-ai/guardrails](https://github.com/guardrails-ai/guardrails) | 7.635 | Apache-2.0 (11.357 B) | moderación | 2026-11-18 |
| [NVIDIA-NeMo/Guardrails](https://github.com/NVIDIA-NeMo/Guardrails) | 7.544 | Apache-2.0 (SPDX, 654 B) | moderación | 2026-11-19 |
| [WeblateOrg/weblate](https://github.com/WeblateOrg/weblate) | 6.178 | GPL-3.0 (⚠️ servicio, no embeber) | localización | 2026-11-22 |
| [inkle/ink](https://github.com/inkle/ink) | 5.029 | MIT (1.067 B) | diálogo | 2026-11-16 |
| [meta-llama/PurpleLlama](https://github.com/meta-llama/PurpleLlama) → `LlamaFirewall/`, `CodeShield/`, `SensitiveDocClassification/` | 4.488 | **MIT** (1.088 / 1.060 B) ⚠️ la raíz NO | moderación | 2026-11-20 |
| [YarnSpinnerTool/YarnSpinner](https://github.com/YarnSpinnerTool/YarnSpinner) | 2.876 | MIT (1.138 B) | diálogo | 2026-11-20 |
| [inkle/inky](https://github.com/inkle/inky) | 2.758 | MIT **en el README** | diálogo / editor | 2026-05-05 |
| [GDQuest/godot-steering-ai-framework](https://github.com/GDQuest/godot-steering-ai-framework) | 1.598 | MIT | movimiento / steering | 2024-09-13 ⚠️ |
| [unitaryai/detoxify](https://github.com/unitaryai/detoxify) | 1.320 | Apache-2.0 (11.357 B) | moderación | 2026-11-11 |
| [YarnSpinnerTool/YarnSpinner-Unity](https://github.com/YarnSpinnerTool/YarnSpinner-Unity) | 854 | MIT (1.079 B) | diálogo | 2026-11-20 |
| [YarnSpinnerTool/YarnSpinner-Godot](https://github.com/YarnSpinnerTool/YarnSpinner-Godot) | 218 | MIT (1.104 B, `master`) | diálogo | 2026-10-22 |

### ⛔ Rechazos medidos de este pass — se publican: el silencio se lee como cobertura

| Candidato | ★ | Motivo medido |
|-----------|---|---------------|
| `fishaudio/fish-speech` | 33.825 | Fish Audio Community 1.0: umbral US$ 500k **del cliente** |
| `coqui-ai/TTS` | 46.266 | MPL-2.0 ✅ pero **27 meses sin commit** (`archived=false` miente) |
| `HIllya51/LunaTranslator` | 14.258 | GPL-3.0 — ✅ como tool del equipo de loc, ⛔ embebido |
| `rhasspy/piper` | 11.336 | MIT ✅ pero `archived=true` |
| `OHF-Voice/piper1-gpl` | 6.347 | GPL-3.0 sobre binario distribuido al jugador |
| `YarnSpinner-Godot-GDScript`, `YarnSpinner-UnrealEngine` | 99 / 52 | **YSPL 1.0**: prohíbe construir herramientas de diálogo reutilizables |
| pesos `Llama-Guard*` / `Llama-Prompt-Guard-2*` | — | Llama 2 / 3.2 / 4 Community — no open source |

### 🟡 La búsqueda que devolvió casi nada, y vale escribirlo

`toxicity detection chat moderation` y `content moderation LLM guardrails`
ordenadas por ★ devuelven, en su totalidad, repos de **0 a 8 ★** (ejercicios de
curso, TPs). **No existe un proyecto open source líder de moderación de chat de
juego.** Los candidatos reales no aparecen por esa consulta: hay que ir a buscarlos
por nombre (`detoxify`, `guardrails`, `NeMo Guardrails`, `LlamaFirewall`), y
ninguno es específico de gaming. ⇒ **es un hueco de mercado medido**, no una
ausencia de búsqueda — y es el hueco donde cae la regulación de 2026 (ver
`intel/market.md`, Texas SB 2420 y Roblox).

---

## 🆕 2026-11-22 — pass cycle 12

**Tema del pass: la licencia dejó de vivir en el repositorio.** Tres repos
devolvieron **`enumeración = 0` archivos de licencia** y los tres veredictos son
distintos: uno tiene el grant **declarado en el README y alojado en Hugging
Face** (`QwenLM/Qwen3`, 27.758 ★), otro **no tiene grant en ningún lado**
(`Yuki001/game-dev-skills`) y el tercero es un **repo auxiliar que nadie
licenció dentro de un producto MIT** (`TaloDev/hosting`). *Cero* es ambiguo,
no negativo.

| Agente / pack | Repo | Licencia (byte a byte + enumeración) | Qué es | ★ |
|---|---|---|---|---|
| **Talo — plugin Godot** | [TaloDev/godot](https://github.com/TaloDev/godot) | 🟢 **MIT** (1.074) — **4 archivos, 4 MIT** | Cliente Godot de Talo: leaderboards, stats, auth, cloud saves. 🔴 **2,4× las ★ del backend que esta KB sí listaba: la puerta de entrada real era el cliente** | **251** |
| **GodotPrompter** | [jame581/GodotPrompter](https://github.com/jame581/GodotPrompter) | 🟢 **MIT** (1.083) — 1 archivo | Gestión y ejecución de prompts **dentro del editor de Godot**: el prompt vive en el proyecto, versionado. Cubre el hueco entre MCP-de-engine y pack-de-skills. 63 forks | **983** |
| **blender-game-skills** | [majidmanzarpour/blender-game-skills](https://github.com/majidmanzarpour/blender-game-skills) | 🟢 **MIT** (1.073) — 1 archivo | Skills de agente para el **pipeline de assets en Blender** orientado a juego. Par del `threejs-game-skills` del mismo autor. ⚠️ push 2026-09-24 | 195 |
| **LGUI** | [LumioGames/LGUI](https://github.com/LumioGames/LGUI) | 🟡 **PolyForm Shield 1.0.0 + Additional Permission** (8.363) — **8 archivos**, 6 vendorizados MIT/BSD | UI de juego manejable por agentes. 🟡 **Verde en un juego de cliente, ROJO en un acelerador reutilizable**: la licencia nombra *"game development tool"* como producto competidor | 148 |
| **Qwen3** *(capa de modelo, nueva en esta KB)* | [QwenLM/Qwen3](https://github.com/QwenLM/Qwen3) | 🟡 badge `NONE` · 9×404 · **enumeración = 0** → 🟢 **Apache-2.0 declarado en `README.md` §*License Agreement*** | El SLM que **NVIDIA ACE soporta on-device** para NPCs de PC. Corre en la GPU del jugador: **ninguna conversación sale del equipo**. 2.113 forks | **27.758** |

### ⛔ Rechazos medidos de este pass

| Candidato | ★ | Medición | Motivo |
|---|---|---|---|
| [Yuki001/game-dev-skills](https://github.com/Yuki001/game-dev-skills) | 123 | badge `NONE`; `raw` 404; **enumeración = 0**; README 200 (5.774 B) **sin mención de licencia** | 🔴 **Sin grant en los tres canales.** Aquí la regla "leer el README" se aplicó y **no salvó al repo** |
| [anthill-platform/anthill](https://github.com/anthill-platform/anthill) | **4** | MIT real, `pushed_at` = **2018-10-05**, 2 forks, 0 issues | 🔴 Un blog de 2026 lo llama *"actively maintained"*. **8 años sin un commit.** Licencia bien, vitalidad falsa |
| [Cynthia-wlan9509/offline-game-revival](https://github.com/Cynthia-wlan9509/offline-game-revival) | 112 | 🟢 MIT (1.073), enumeración = 1, push 2026-11-16 | 🟡 Licencia limpia, **caso de uso no recomendable**: revivir juegos dados de baja toca derechos sobre el juego ajeno, no sobre el repo |

### 🟢 Movimientos de los que esta KB ya listaba

| Repo | ★ cycle 11 | ★ ahora | Nota |
|---|---|---|---|
| [Donchitos/Claude-Code-Game-Studios](https://github.com/Donchitos/Claude-Code-Game-Studios) | 40.377 (2026-11-20) | **40.527** | 5.309 forks. Sigue siendo el líder de la categoría de orquestación |
| [gamedev-skills/awesome-gamedev-agent-skills](https://github.com/gamedev-skills/awesome-gamedev-agent-skills) | — | **10.042** | Apache-2.0, 1.027 forks |
| [fennaraOfficial/fennara-godot-ai](https://github.com/fennaraOfficial/fennara-godot-ai) | — | 320 | MIT, push **2026-11-21** — de los más vivos del clúster Godot |
| [SummerEngine/summer](https://github.com/SummerEngine/summer) | — | 111 | MIT, push 2026-11-20 |

⚠️ **Deltas: sólo el de `Claude-Code-Game-Studios` tiene lectura previa fechada
(2026-11-20).** Los demás se listan como foto de hoy, sin delta, porque esta KB
no tiene su valor anterior medido. **Un número sin su medición previa no es un
crecimiento.**

---

## 🆕 2026-11-22 — pass cycle 11

**Tema del pass: la licencia no se hereda de la organización.** De los repos de
la Farama Foundation que esta KB listaba, los de badge limpio son **MIT**
(Gymnasium, PettingZoo). Los dos que **no** listaba son **GPL-2.0** (ALE) y
**sin grant legible** (ViZDoom). Misma fundación, tres resultados distintos.

| Agente / pack | Repo | Licencia (byte a byte) | Qué es | ★ |
|---|---|---|---|---|
| **Microverse** | [KsanaDock/Microverse](https://github.com/KsanaDock/Microverse) | **MIT** (1.065) | Simulación social **multi-agente dentro de Godot 4**. El eslabón entre `concordia` (población sin engine) y los plugins de Godot (engine, un NPC por vez). ⚠️ 7 meses sin push | **2.515** |
| **threejs-game-skills** | [majidmanzarpour/threejs-game-skills](https://github.com/majidmanzarpour/threejs-game-skills) | **MIT** (1.073) | Skills de agente para juegos **three.js** de navegador. El par web de `godogen`: la capa de skills estaba vacía para navegador puro | **2.873** |
| **Agentshire** | [Agentshire/Agentshire](https://github.com/Agentshire/Agentshire) | **MIT** (1.085) | Visualiza **agentes como NPCs 3D en un pueblo**: el juego como interfaz de observabilidad de una flota de agentes, no AI dentro del juego | 1.317 |
| **Legends-Of-Heroes** | [FlameskyDexive/Legends-Of-Heroes](https://github.com/FlameskyDexive/Legends-Of-Heroes) | **MIT** (1.076) | Título jugable estilo LoL con **arquitectura agéntica end-to-end** + agent skills | 939 |
| **GameDesignOS** | [DY-2026/GameDesignOS](https://github.com/DY-2026/GameDesignOS) | **MIT** (1.065) | *Game design OS* local-first: convierte sesiones de diseño en evidencia. Cubre **preproducción**, etapa que la KB no tenía | 459 |
| **GameBlocks** | [xt4d/GameBlocks](https://github.com/xt4d/GameBlocks) | **MIT** (1.069) | Bloques mínimos para que un agente de código prototipe juegos. Contrapunto de los packs de 49 agentes | 434 |

### ⛔ Rechazos medidos este pass (se publican: el silencio se lee como cobertura)

| Repo | ★ | Causa |
|---|---|---|
| [Farama-Foundation/Arcade-Learning-Environment](https://github.com/Farama-Foundation/Arcade-Learning-Environment) | 2.463 | **GPL-2.0.** El benchmark canónico de Atari, vivo (2026-11-15), copyleft fuerte |
| [Farama-Foundation/ViZDoom](https://github.com/Farama-Foundation/ViZDoom) | 2.110 | **Sin grant resoluble**: API `null` + **27 sondas en 404** (9 nombres × `HEAD`/`master`/`main`). Push **de hoy**: vivo ≠ usable |
| [art-from-the-machine/Mantella](https://github.com/art-from-the-machine/Mantella) | 427 | **AGPL-3.0** (34.523 B — checksum acierta otra vez). NPC por voz en Skyrim/Fallout 4 |
| [SerpentAI/SerpentAI](https://github.com/SerpentAI/SerpentAI) | **6.987** | **MIT pero ARCHIVADO** desde 2022-11-07. La trampa de estrellas altas del pass |
| [GigaxGames/gigax](https://github.com/GigaxGames/gigax) | 346 | **Sin licencia** + 28 meses sin push |

> **Sondas de este pass.** `gh api search/repositories` ✅ · `raw.githubusercontent.com` ✅
> (**distingue mayúsculas** — ver regla nº19) · `gh api repos/<o>/<r>/contents/` ❌
> *"Request path could not be canonicalized"* · `gh api users/<org>` ❌ *"sessions are
> bound to their configured repositories"* · `curl -sI github.com` ❌ 403 sobre repos
> reales · **WebFetch ❌ `ENOTFOUND` en todos los hosts externos** ⇒ todo lo
> regulatorio de este pass es **secundario**, marcado 🔵.

---

## 🆕 2026-11-22 — pass cycle 10

> **Primer pass de un día nuevo** (el último fue el cycle 9, 2026-11-21).
> **Sigue sin reportarse delta de ★**: 24 h no mueven una estrella de forma
> interpretable y ninguna cifra heredada se reescribe. Lo que aporta este pass
> es **una auditoría mecánica completa del inventario (159/159 slugs)** y
> **11 filas nuevas**, todas con licencia leída byte a byte.

### Lo que la auditoría de 159 slugs encontró

| Señal | Resultado |
|---|---|
| Slugs re-leídos con `gh api search/repositories` | **159 / 159** |
| Forks disfrazados de upstream | **0** |
| Archivados | **2** — `0ad/0ad`, `openai/gym` (ambos ya anotados) |
| HTTP 422 | **4** — 3 renames + 1 borrado (ver abajo) |
| **Renames nuevos** | **1** 🆕 |
| Repos dormidos >6 meses | **26** |

### 🔄 Rename nuevo de este pass

| Slug que tenía esta KB | Slug real | ★ | Licencia | Último push |
|---|---|---|---|---|
| `kolbytn/mindcraft` | **[mindcraft-bots/mindcraft](https://github.com/mindcraft-bots/mindcraft)** | **6.039** | **MIT** | 2026-10-27 |

Cuarto caso del mismo patrón en esta KB (**cuenta personal → organización**),
después de `justinpbarnett → CoplayDev`, `googleforgames → agones-dev` y
`minetest → luanti-org`. **El redirect nunca falla, así que una tabla con el
slug viejo envejece en silencio.**

### 🆕 Agentes y herramientas nuevos — 11 filas

| Agente / herramienta | Repo | Licencia (byte-read) | ★ | Último push |
|---|---|---|---|---|
| **utopia** | [deeplethe/utopia](https://github.com/deeplethe/utopia) | **Apache-2.0** (11.353 B) | 8.745 | 2026-11-22 |
| **Project Lyra** | [nv-tlabs/lyra](https://github.com/nv-tlabs/lyra) | **Apache-2.0** (11.356 B) | 2.888 | 2026-07-20 |
| **stable-worldmodel** | [galilai-group/stable-worldmodel](https://github.com/galilai-group/stable-worldmodel) | **MIT** (1.070 B) | 2.315 | 2026-10-02 |
| **DotRecast** | [ikpil/DotRecast](https://github.com/ikpil/DotRecast) | **Zlib** (904 B) | 968 | 2026-10-03 |
| **gym-super-mario-bros** | [Kautenja/gym-super-mario-bros](https://github.com/Kautenja/gym-super-mario-bros) | **MIT** (1.073 B) | 910 | 2026-09-30 |
| **VibeUE** | [kevinpbuckley/VibeUE](https://github.com/kevinpbuckley/VibeUE) | **MIT** (1.091 B) | 845 | 2026-11-20 |
| **unity-cli-loop** | [hatayama/unity-cli-loop](https://github.com/hatayama/unity-cli-loop) | **MIT** (`LICENSE.md`, 1.075 B) | 602 | 2026-11-21 |
| **godot-mcp (tugcantopaloglu)** | [tugcantopaloglu/godot-mcp](https://github.com/tugcantopaloglu/godot-mcp) | **MIT** (1.169 B) | 521 | 2026-10-09 |
| **Godot-MCP (IvanMurzak)** | [IvanMurzak/Godot-MCP](https://github.com/IvanMurzak/Godot-MCP) | **Apache-2.0** (11.357 B) | 376 | 2026-11-21 |
| **funplay-unity-mcp** | [FunplayAI/funplay-unity-mcp](https://github.com/FunplayAI/funplay-unity-mcp) | **MIT** (1.064 B) | 330 | 2026-11-19 |
| **Doyunha-Gopeak** | [HaD0Yun/Doyunha-Gopeak](https://github.com/HaD0Yun/Doyunha-Gopeak) | **MIT** (1.070 B) | 274 | 2026-11-13 |

### ⛔ Rechazados este pass, con motivo medido

| Candidato | Repo | ★ | Motivo |
|---|---|---|---|
| **godot-mcp-pro** | [youichi-uda/godot-mcp-pro](https://github.com/youichi-uda/godot-mcp-pro) | 649 | 🔴 **Open-core**: el MIT cubre sólo `addons/godot_mcp/`; el servidor MCP con las 162 herramientas es **propietario y de pago** |
| **STS2-Agent** | [CharTyr/STS2-Agent](https://github.com/CharTyr/STS2-Agent) | 366 | 🔴 **AGPL-3.0** (33.985 B) tras badge `NOASSERTION` |
| **Locus** | [r1n7aro/Locus](https://github.com/r1n7aro/Locus) | 762 | 🔴 **GPL-3.0** (35.149 B); hay 6 alternativas MIT/Apache iguales |
| **corosolto/client** | [corosolto/client](https://github.com/corosolto/client) | 263 | 🔴 AGPL-3.0 **y no es un agente** (es un juego). Se anota por ser el repo de gaming **de origen LATAM** con más tracción que vio el canal en 10 passes |
| **HY-WorldPlay** (raíz) | [Tencent-Hunyuan/HY-WorldPlay](https://github.com/Tencent-Hunyuan/HY-WorldPlay) | 1.630 | 🟡 **Rechazo parcial**: raíz con exclusión EU/UK/Corea; **`worldcompass/` es Apache-2.0 y sí se usa** |

### 🟡 La tendencia que este pass puede medir y los anteriores no: **el clúster `godot-mcp` se fragmentó en 7, y el líder está muerto**

| Repo | ★ | Licencia | Último push |
|---|---|---|---|
| [Coding-Solo/godot-mcp](https://github.com/Coding-Solo/godot-mcp) | **6.732** | MIT | ⛔ 2026-04-16 (**7 meses**) |
| [youichi-uda/godot-mcp-pro](https://github.com/youichi-uda/godot-mcp-pro) | 649 | ⛔ open-core | 2026-09-24 |
| [tugcantopaloglu/godot-mcp](https://github.com/tugcantopaloglu/godot-mcp) | 521 | ✅ MIT | 2026-10-09 |
| [tomyud1/godot-mcp](https://github.com/tomyud1/godot-mcp) | 454 | ✅ MIT | 2026-10-17 |
| [IvanMurzak/Godot-MCP](https://github.com/IvanMurzak/Godot-MCP) | 376 | ✅ Apache-2.0 | 🟢 **2026-11-21** |
| [HaD0Yun/Doyunha-Gopeak](https://github.com/HaD0Yun/Doyunha-Gopeak) | 274 | ✅ MIT | 2026-11-13 |
| [satelliteoflove/godot-mcp](https://github.com/satelliteoflove/godot-mcp) | 188 | ✅ MIT | 2026-11-16 |

**6 de los 7 nacieron entre dic-2025 y jun-2026.** Es un mercado que se abrió
hace once meses y todavía no consolidó: nadie pasó de 700 ★ salvo el que dejó de
publicar. Si hay que apostar a uno, el criterio no es la estrella — es el push.

---
## 🆕 2026-11-21 — pass cycle 9

> **Quinto pass del día.** Igual que en los cycles 5-8, **no se reportan deltas de
> ★**: una hora no mueve una estrella de forma interpretable, y ninguna cifra
> heredada se reescribe. Lo que aporta este pass es **cobertura nueva, por primera
> vez en tres passes**: 11 repos que esta KB no tenía, más el rechazo medido del
> backend de 25.570 ★ que llevaba 8 passes sin ser mirado.

### 🔴 El evento del pass: `SpacetimeDB` existía, era enorme, y nadie lo había abierto

[clockworklabs/SpacetimeDB](https://github.com/clockworklabs/SpacetimeDB) — **25.570 ★**,
1.121 forks, push de **hoy**, `archived=false`. GitHub lo reporta como `NOASSERTION`.
El byte-read de `LICENSE.txt` (**42.562 B**) resuelve: **Business Source License 1.1**,
Change Date **2031-11-20**, Change License **AGPL-3.0 con excepción de linking**, y un
Additional Use Grant que permite **una sola instancia en producción**.

**⛔ Rechazado para la ruta permisiva.** El detalle que lo mata no es el copyleft
futuro: es el tope de **una instancia**, que un multijugador comercial rompe el día
que escala. Nakama (Apache-2.0) y Talo (MIT) siguen siendo las recomendaciones.

**Lo que esto dice del método:** ocho passes de forensia de licencias cada vez más
fina sobre el catálogo ya listado, y el agujero más caro era de **cobertura**, no de
precisión. Un repo de 25,5k ★ ausente del índice no genera ninguna contradicción que
las Recetas 10/14/17/20/21 puedan cazar, porque esas recetas auditan lo que está
escrito. **Un silencio no se contradice a sí mismo.**

### 🆕 11 repos nuevos, todos con licencia leída byte a byte

Tabla completa, con ★, `pushed_at` y bytes de licencia, en
[`agents/top.md`](../agents/top.md). Resumen de lo que mueve la aguja:

| Repo | Licencia | ★ | Por qué entra |
|---|---|---|---|
| [scenario-labs/skills](https://github.com/scenario-labs/skills) | MIT (1.070 B) | 3.961 | Skills de generación de imagen/video/audio/3D que **cotizan el costo antes de gastar**. Único de los nuevos que ataca el control de gasto del pipeline de assets. |
| [CraftJarvis/MineStudio](https://github.com/CraftJarvis/MineStudio) | MIT (1.068 B) | 423 | Training de agentes de Minecraft. Complementa a Mindcraft por el lado del entrenamiento. |
| [ukanwat/overtime](https://github.com/ukanwat/overtime) | MIT (1.078 B) | 403 | Brief en vez de chat; agente que persiste entre sesiones. Genérico, no gaming-específico. |
| [tettethu/VibeGame](https://github.com/tettethu/VibeGame) | Apache-2.0 (14.096 B) | 389 | Engine AI-native + multi-agente auto-evolutivo. Creado hace 3 meses. |
| [isuzu-shiranui/UnityMCP](https://github.com/isuzu-shiranui/UnityMCP) | MIT (1.066 B) | 344 | El Editor **sirve MCP él mismo por HTTP**: sin proceso puente. |
| [wellingfeng/UltraGameStudio](https://github.com/wellingfeng/UltraGameStudio) | MIT (1.068 B) | 344 | Agente de codificación para game dev end-to-end. |
| [TabooHarmony/roblox-brain](https://github.com/TabooHarmony/roblox-brain) | MIT (1.069 B) | 199 | **Primera entrada de Roblox** en esta KB. |
| [satelliteoflove/godot-mcp](https://github.com/satelliteoflove/godot-mcp) | MIT (1.056 B) | 188 | **Playtesting determinista** + inyección de input: la pieza que le faltaba a la Receta 9. |
| [WenyiWU0111/RSIGame](https://github.com/WenyiWU0111/RSIGame) | Apache-2.0 (11.400 B) | 178 | Auto-mejora recursiva aplicada a game dev. |
| [typeonce-dev/ritmo](https://github.com/typeonce-dev/ritmo) | MIT (1.072 B) | 170 | ECS *agent-first*. |
| [quazaai/UnityMCPIntegration](https://github.com/quazaai/UnityMCPIntegration) | MIT (1.068 B) | 155 | **⚠️ Dormido: push 2025-04-15, 19 meses.** Se lista para marcarlo, no para usarlo. |

### 🔴 Defecto de instrumento medido este pass: **`curl -sI` da 403 a lo real y a lo falso**

`godotengine/godot` → **403**. Un slug inventado → **403**. La sonda que el prompt de
esta KB pide usar para verificar URLs **no discrimina nada en este entorno**, y falla
en silencio. Los dos canales que sí discriminan, y con los que se verificó todo lo de
arriba: `gh api search/repositories` y el byte-read por `raw.githubusercontent.com`
(200 + bytes reales vs 404 + cuerpo de 14 B).

⛔ **Gap de canal declarado:** `WebFetch` no resuelve **ningún** host general en esta
sesión (`github.blog`, `es.kotaku.com`, `www.pocketgamer.biz` → `ENOTFOUND`). Los datos
de repos son de primera mano; los de prensa, de *snippets* de búsqueda.

### ⛔ Rechazo nuevo: el homónimo entró por el buscador de repos

[databrickslabs/ontobricks](https://github.com/databrickslabs/ontobricks) (485 ★) llegó
a la búsqueda `unity ai agent mcp` porque expone **Unity Catalog** (el metastore de
Databricks) por MCP. **Nada que ver con el engine Unity.** Segundo homónimo medido de
esta KB, después de *gaming* = casino: **`Unity` es ambiguo dentro de GitHub**.

---

## 🆕 2026-11-21 — pass cycle 8

> **Cuarto pass del día.** Igual que en los cycles 5, 6 y 7, **no se reportan
> deltas de ★**: una hora no mueve una estrella de forma interpretable, y
> ninguna cifra heredada se reescribe. Lo que aporta este pass no es crecimiento
> ni cobertura de catálogo: es **integridad del dato de licencia** — el caso en
> que el archivo que la casa lee **está roto**, y el caso en que el agente más
> importante del nicho **hay que rechazarlo**.

### 🔴 El evento del pass nº1: el archivo de licencia como artefacto dañado

`Farama-Foundation/Minigrid` (2.543 ★) tiene un `LICENSE` de **2.507 B** que
dice *"Apache License, Version 2.0"*, contiene **sólo las §§ 1 y 9**, le faltan
**las §§ 2-8 — el grant de copyright, el de patente y la redistribución** — y
trae un **hunk de diff literal (`@@ -154,49 +194,23 @@`)** en la línea 18.
El `pyproject.toml` del mismo repo declara **MIT**. El badge dice `NOASSERTION`.
El grant real está en **`docs/LICENSE` (12.201 B): composición MIT + Apache-2.0**,
y **sólo la enumeración lo encuentra**.

🟢 **Minigrid es permisivo y usable** — pero se cita `docs/LICENSE`, no `LICENSE`.

Tres consecuencias de método, detalladas en `agents/top.md` y `repos/foundations.md`:

1. **El checksum de bytes clasifica mal este caso en lugar de cazarlo**: 2.507 B
   con íncipit Apache entra en la categoría ya bendecida *"Apache sin apéndice"*.
   El tamaño distingue *recortado* de *canónico*; **no distingue *recortado* de
   *roto***.
2. **Manifiesto y `LICENSE` se contradicen en la familia de licencia**, y los dos
   son artefactos del árbol, no anuncios ⇒ **regla nueva: esa contradicción es
   una ambigüedad abierta, no un veredicto.**
3. **La sonda no omitió el grant: lo sustituyó**, con `HTTP 200` y 2.507 B (por
   encima del umbral de no-hallazgo de 50 B). Peor que el caso del cycle 6.

Hermanos sanos: `Gymnasium` 1.104 B MIT, `PettingZoo` 1.073 B MIT, **0
artefactos de diff**. El daño es del repo, no del org.

### 🔴 El evento del pass nº2: `NitroGen` — el agente que hay que citar y no se puede usar

`MineDojo/NitroGen` (**2.251 ★**, oral **CVPR 2026**, arXiv 2601.02427): 40.000 h
de gameplay, **+1.000 juegos**, wrapper Gymnasium, pesos en Hugging Face.
**No estaba en esta KB.** Encontrado, hay que **excluirlo**:
`LICENSE` = **4.729 B, `NVIDIA License`**, § 3.3 **uso no comercial**
(*"or intended for use"*), § 3.2 **viral** sobre derivados, § 3.7 arrastra el
**Meta OPT-IML 175B License Agreement**, § 3.4 terminación por patente.

⛔ **Fuera de todo entregable, PoC pago incluido.** 🔴 Y la regla que deja:
el abstract dice *"we open-source the dataset, simulator, and pre-trained
weights"* — **"open-source" en un paper revisado por pares no es un grant.**
Un venue top **no audita licencias**; el abstract es el anuncio.

🔵 El laboratorio estaba en esta KB **por su repo muerto** (`Voyager`, congelado
2024-04-03) **y no por su repo vivo**. Lo encontró el censo plano de incumbentes,
no una consulta con forma de problema.

### 🟡 El evento del pass nº3: *"gaming"* es un homónimo y el informe-faro es del otro rubro

Las búsquedas de adopción y regulación devolvieron, primero y en **las cuatro
regiones**, *"The State of AI in Gaming 2026"* (UNLV International Gaming
Institute + KPMG): *AI Maturity Index* **≤ 45/100**, gobernanza rezagada,
divergencia medida regulador-industria. 🔴 **Es de casino/apuestas, no de
videojuegos.** Ninguna de sus cifras entra al mercado ni a la adopción de esta
KB. **Regla de ingesta: clasificar la fuente en `videojuegos` o `azar/iGaming`
antes de citar el número, y decirlo en la misma línea.**

### ⛔ Gap de canal declarado

`www.dlapiper.com`, `chambers.com`, `www.unlv.edu` y `github.blog` **no
resuelven** por el proxy (`ENOTFOUND`). Las fechas del Art. 50 de este pass son
**secundarias**; coinciden con los cycles 5-7 ⇒ **4ª confirmación independiente
del 2-dic-2026**, **no** verificación primaria. El informe UNLV se usa sólo para
delimitar el homónimo. **Un gap escrito es información.**

### Agentes y entornos nuevos (byte-read, 2026-11-21)

| Nombre | Repo | Licencia (bytes) | Nota | ★ |
|--------|------|------------------|------|---|
| **LightZero** | [opendilab/LightZero](https://github.com/opendilab/LightZero) | **Apache-2.0** (11357) | MCTS + AlphaZero/MuZero: agentes que **planifican**, no reactivos. Push 2026-11-19 | 1.663 |
| **DI-engine** | [opendilab/DI-engine](https://github.com/opendilab/DI-engine) | **Apache-2.0** (11342) | Plataforma RL del mismo lab. ⚠️ push 2025-12-07 | 3.645 |
| **Minigrid** | [Farama-Foundation/Minigrid](https://github.com/Farama-Foundation/Minigrid) | 🟡 **MIT + Apache-2.0** — `docs/LICENSE` (12201) | Prototipado de lógica de agente sin engine 3D. `LICENSE` raíz **dañado** | 2.543 |
| **MicroRTS-Py** | [Farama-Foundation/MicroRTS-Py](https://github.com/Farama-Foundation/MicroRTS-Py) | **MIT** (1117) | RTS en miniatura. ⚠️ push 2025-08-11 | 295 |
| **MineDojo** | [MineDojo/MineDojo](https://github.com/MineDojo/MineDojo) | **MIT** (1070) | El entorno detrás de Voyager. 🔴 push 2024-03-18 | 2.285 |
| ⛔ **NitroGen** | [MineDojo/NitroGen](https://github.com/MineDojo/NitroGen) | ⛔ **NVIDIA License** (4729) | **No comercial y viral.** Citar, no usar | 2.251 |
| ⛔ **gym** | [openai/gym](https://github.com/openai/gym) | — | 🔴 **ARCHIVADO**; usar `Gymnasium` | 37.237 |

### Verificación de este pass

- **139 slugs de la KB re-verificados** por `search/repositories`: **134 vivos**,
  **5 sin resolver** — y los 5 (`JonTelep/awesome-ai-game-generation`,
  `kolbytn/mindcraft`, `minetest/minetest`, `pelican-dev/panel`,
  `rivet-gg/toolchain`) **ya estaban documentados** como rename, fuera de índice
  o muerto. 🟢 **0 filas vivas falsas** — la higiene de los cycles 4-7 aguanta.
- **1 archivado** en todo el inventario (`0ad/0ad`, ya anotado) + `openai/gym`
  (nuevo, se agrega ya marcado).
- Licencias re-leídas en bytes y **confirmadas**: `ml-agents` Apache-2.0,
  `c2pa-rs` MIT **o** Apache-2.0 (1083 + 11335, idénticos al cycle 7),
  `cocos` MIT 1164, `panda3d` BSD 1569. **0 correcciones necesarias** a filas
  heredadas: lo que el cycle 7 escribió se sostiene.
- **1 contradicción interna corregida**: `compose/patterns.md` decía C2PA
  "Apache-2.0" a secas contra el "MIT *o* Apache-2.0" de `agents/top.md`.

---

## 🆕 2026-11-21 — pass cycle 7

> Tercer pass del día. **Igual que en los cycles 5 y 6, no se reportan deltas de
> ★**: una hora no mueve una estrella de forma interpretable. Lo que este pass
> aporta no es crecimiento, es **cobertura** — y el hallazgo es que la cobertura
> estaba peor de lo que seis passes de verificación de licencias podían detectar.

### 🔴 El evento del pass: la KB tenía un agujero de 158.000 ★ y ninguna sonda podía verlo

El cycle 6 formuló la regla *"una sonda sólo confirma lo que ya sospechabas; sólo
la enumeración te dice lo que te faltaba"* y la aplicó a los archivos de licencia
**dentro** de cada repo. Aplicada a **la lista de repos**, aparecieron de golpe
8 engines y 2 frameworks de RL que esta KB **nunca había listado en 6 passes**,
encabezados por **`bevyengine/bevy` (49.914 ★, release 0.20 el 8-oct-2026)**.

**La causa es el vocabulario, no el esfuerzo:** todas las consultas de esta KB
tienen forma de problema ("NPC con LLM", "MCP para engines", "agentes que
juegan"). Un engine de propósito general **no responde a ningún problema
concreto** y por eso no aparece nunca. Ver `repos/foundations.md` para el censo
completo y `agents/top.md` para las reglas de método nº14–16.

### 🟢 Agentes nuevos con licencia leída en bytes (6)

| Agente | Repo | Licencia (bytes) | Señal | ★ |
|--------|------|------------------|-------|---|
| **Aura Autopilot** | [ramenvr/autopilot](https://github.com/ramenvr/autopilot) | **Apache-2.0** (11358) | 🟢 **La novedad más relevante del pass para vender.** Agente de playtesting OSS para **UE 5.4–5.8 / Unity 6 / Godot 4 / Roblox Studio**, con MCP para Claude Code y Codex. Anunciado ~30-oct-2026; **commit de hoy**. Fuente real verificada (651 archivos, 50 cpp / 81 cs / 47 gd / 156 py) | 881 |
| **Godogen** | [htdt/godogen](https://github.com/htdt/godogen) | **MIT** (1077) | **El pack de skills de gaming más estrellado que existe — y es MIT.** Contraejemplo a la tendencia 16. Cubre Godot, **Bevy**, Babylon.js, three.js. Commit de hoy | **7.378** |
| **godogen_assets** | [htdt/godogen_assets](https://github.com/htdt/godogen_assets) | 🔴 **ninguna** (0 archivos de licencia / 69 versionados) | ⛔ La dependencia **sin grant** que Godogen instala. Ver abajo | 2 |
| **Voyager** | [MineDojo/Voyager](https://github.com/MineDojo/Voyager) | **MIT** (1070) | ⚠️ Hueco histórico cerrado **como referencia, no como base**: `pushed_at` 2024-04-03, **2 años 8 meses congelado** | 7.339 |
| **Mindcraft** | [kolbytn/mindcraft](https://github.com/kolbytn/mindcraft) | **MIT** (1073) | Recuperado del cycle 3, que **se perdió por quedar en commit local sin push**. 🔵 Sin ★: ausente del índice de `search/repositories` | n/d |
| **OpenGameAgent** | [EricSun0218/OpenGameAgent](https://github.com/EricSun0218/OpenGameAgent) | **MIT** (1096) | Runtime TS para personajes AI-native. push 2026-11-17. Señal temprana, 61 ★ | 61 |

### 🔴 La ruta de contaminación nueva, y es peor que el copyleft: **la ausencia de grant**

La tendencia 16 de esta KB dice que *el copyleft se mudó hacia arriba del stack,
a los packs de skills*. Este pass encuentra la variante **más peligrosa y menos
visible**:

```
htdt/godogen  (MIT, 7.378 ★)   ← el pack que el cliente instala
      │  su README: "el agente te instala godogen_assets en tu GPU"
      ▼
htdt/godogen_assets  (2 ★)     ← 69 archivos versionados, CERO archivos de licencia
      │  su README: requiere token de Hugging Face cuya cuenta haya
      ▼                           aceptado la licencia de stable-audio-3-small-sfx
modelos gated de terceros
```

🔴 **Sin licencia no hay permiso.** Un repo sin grant no es "permisivo por
defecto": por derecho de autor **no se puede copiar, modificar ni redistribuir**.
Y el vehículo de entrega es un pack **MIT con 7.378 ★**, es decir el que *pasa*
cualquier gate de licencia que mire sólo el repo que el cliente nombró.

**Confirmado por dos canales independientes** (no por un 404 mal leído):
`find` sobre el clon → 0 resultados; `search/code repo:htdt/godogen_assets
filename:LICENSE` → `total_count: 0`.

> 📐 **Consecuencia para la Receta 16** (gate de licencia para packs de skills):
> el gate no puede parar en el repo del pack. Tiene que **seguir las
> instrucciones de instalación del pack** y auditar lo que el agente va a
> clonar. Un pack es un **instalador**, y se audita como un instalador.

### 🟢 El dato que corrige a esta página sobre NVIDIA ACE

Esta KB cita **ACE en 8 archivos** desde el cycle 3 y **nunca había leído un
grant**. `NVIDIA/Audio2Face-3D` (574 ★) resulta ser un **repo-señalizador sin
código**: 3 entradas versionadas, 0 licencias, y las licencias de los componentes
**declaradas en prosa en una tabla HTML del README**. Leídos los componentes:
SDK **MIT** (1097), Training Framework **Apache-2.0 + 3 porciones MIT
vendorizadas** (15057), Maya ACE **MIT** (1098). 🟢 Los tres usables. ⚠️ El
plugin de UE5 que el README declara MIT **no tiene repo enlazado**: gap, no fila.

### ⛔ Candidatos nuevos rechazados (para no re-gastar la búsqueda)

| Candidato | Motivo medido |
|-----------|---------------|
| [minerllabs/minerl](https://github.com/minerllabs/minerl) | Badge `NOASSERTION` → **CC-BY-NC-SA 4.0** (20850 B). **NonCommercial = exclusión dura.** Congelado (2025-01-22). Es el entorno RL de Minecraft que la literatura recomienda por defecto ⇒ **un cliente lo va a proponer** |
| [SillyTavern/SillyTavern](https://github.com/SillyTavern/SillyTavern) | **AGPL-3.0** (34523 B). 35.925 ★ y front-end dominante de roleplay con personajes: se anota para poder contestar, no para construir |
| GameReverie | 🔵 **Gap declarado, no rechazo**: descrito como Codex Skill MIT (sep-2026) en fichas de lanzamiento, **sin slug resoluble** (`search/repositories` → 0). Sin repo verificable no se escribe fila |

### Método y control negativo de este pass

- **Canal 1** `search/repositories` (★/`archived`/`pushed_at`/badge SPDX): OK.
  Control negativo heredado sigue válido (422 para slug inexistente).
- **Canal 2** byte-read en `raw`: OK. 🧪 **Defecto nuevo encontrado:** `raw`
  **aliasea `master` → la rama default renombrada**. `ml-agents` no tiene
  `refs/heads/master` y sin embargo `raw/.../master/LICENSE.md` devuelve **200 y
  bytes idénticos a `main`**; una rama inventada devuelve 404. ⇒ un bucle de
  sondas sobre `main` y `master` **cuenta el mismo archivo dos veces**.
  Deduplicar por contenido.
- **Canal 3** `search/code` (enumeración): OK, y es el que confirmó el cero de
  `godogen_assets` y los **164** archivos de licencia de `bevy`.
- 🔴 **WebFetch está muerto para hosts arbitrarios en esta sesión** (ENOTFOUND):
  `dlapiper.com`, `github.blog`, `digital-strategy.ec.europa.eu`, `orrick.com`.
  La API de `huggingface.co` responde **vacío**. ⇒ la licencia de **pesos** de
  Audio2Face **sigue sin poder leerse**, y el Art. 50 del AI Act **no pudo
  verificarse en fuente primaria** (ver `intel/market.md`: se resolvió con 4+
  secundarias concordantes que citan el **Art. 111(4)**).

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
