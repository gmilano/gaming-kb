---
industry: gaming
region: Global
updated: 2026-11-22
---

# 🎮 Agentes AI — Gaming

> Agentes y herramientas AI open source. Foco: MIT / Apache 2.0 / BSD / Zlib.
> Última actualización: **2026-11-22 (pass cycle 11)**. Passes previos: 2026-11-22
> (cycle 10), 2026-11-21 (cycles 9, 8, 7, 6, 5), 2026-11-20, 2026-10-26, 2026-07-02.
>
> 🔴 **Lo que este pass cambia de raíz:** los seis passes anteriores afinaron
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
