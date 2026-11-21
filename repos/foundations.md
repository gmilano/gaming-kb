---
industry: gaming
region: Global
updated: 2026-11-21
---

# 🏗️ Repos fundacionales — Gaming

> Bases sobre las cuales construir. Licencia verificada primera mano.
> Última actualización: **2026-11-20** (passes previos: 2026-10-26, 2026-07-02).
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

## RL y entornos de entrenamiento

| Repo | Licencia (archivo, bytes) | Descripción | ★ |
|------|--------------------------|-------------|---|
| [Farama-Foundation/Gymnasium](https://github.com/Farama-Foundation/Gymnasium) | **MIT** (`LICENSE`, 1104) | API estándar de entornos RL. Sucesor mantenido de `openai/gym`. | 12.843 |
| [Farama-Foundation/PettingZoo](https://github.com/Farama-Foundation/PettingZoo) | **MIT** (`LICENSE`, 1073) | Equivalente multi-agente de Gymnasium. Para NPCs que compiten o cooperan. | 3.556 |
| [DLR-RM/stable-baselines3](https://github.com/DLR-RM/stable-baselines3) | **MIT** (`LICENSE`, 1075) | PPO, SAC, A2C, DQN en PyTorch. El algoritmo por defecto para QA bots. ⚠️ default `master`. | 14.038 |
| [Unity-Technologies/ml-agents](https://github.com/Unity-Technologies/ml-agents) | **Apache-2.0** (`LICENSE.md`, 572 — cabecera-puntero, no el texto completo) | Toolkit RL oficial de Unity. ⚠️ default **`develop`**. Commit de hoy. | 19.887 |
| [edbeeching/godot_rl_agents](https://github.com/edbeeching/godot_rl_agents) | **MIT** (`LICENSE`, 1071) | Puente Godot ↔ Python para 4 frameworks RL. **900+ → 1.639 (+82%)**: mayor salto relativo de la KB. | 1.639 |

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

*Ver también: `verticals/solutions.md` para plataformas verticales completas y
`agents/top.md` para el método de verificación, el checksum de bytes y los grants
bloqueantes.*
