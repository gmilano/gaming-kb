---
industry: gaming
region: Global
updated: 2026-11-21
---

# 🏗️ Repos fundacionales — Gaming

> Bases sobre las cuales construir. Licencia verificada primera mano.
> Última actualización: **2026-11-21 (cycle 7)** (passes previos: 2026-11-21 cycle 6, 2026-11-21 cycle 5, 2026-11-20, 2026-10-26, 2026-07-02).
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
