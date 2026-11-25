---
industry: gaming
region: Global
updated: 2026-11-25
---

# 🎮 Gaming KB

> Knowledge base de la industria **Gaming** para Globant AI Studios.
> NPC agents, procedural content, soporte in-game, anti-cheat, game analytics,
> desde el cycle 13 la capa de **diálogo autorado, voz, moderación de chat y
> localización**, desde el cycle 14 la de **producción de assets** (malla, textura,
> rig, movimiento, música y SFX), desde el cycle 15 la **operativa de cara al
> jugador**, desde el cycle 16 el **plano de datos del jugador** y —desde el cycle
> 17— la **fábrica**: versionado de binarios, CI de build, caché de compilación,
> seguimiento de producción y automatización de QA. **Desde el cycle 19**, el
> **plano de decisión**: *remote config*, *feature flags*, asignación de
> experimentos, recomendación y valor del jugador — la capa que decide qué ve y
> qué paga cada jugador, y la que la regulación de 2026 persigue por su nombre.
>
>
> **Desde el cycle 20**, el **camino del dinero**: tienda y webshop D2C,
> validación de recibo, otorgamiento del derecho (*entitlement*), libro mayor de
> moneda virtual, y la entrega de lo comprado (parcheo diferencial, actualizador,
> tienda propia). Es la capa que **2026 abrió por ley en las cuatro regiones** —
> Japón MSCA, DMA, acuerdo CADE de Brasil, *Epic v. Apple*— y la primera capa de
> esta KB donde el entregable es **transaccional**: si falla, no se ve mal, se
> pierde dinero.
>
>
> **Desde el cycle 21**, el **plano del espectador**: cómo el juego llega a quien
> **no** lo está jugando — captura, encode, transporte, servidor de medios, cloud
> gaming y pixel streaming, espectador, clip automático, relato (*caster*) y
> archivo. Es la **primera capa de esta KB donde el copyleft es el default y no la
> excepción** (12 repos del lado píxel medidos, **cero permisivos**), y la primera
> donde el **gate de licencia y el gate regulatorio son la misma línea de
> pipeline**: la etiqueta de contenido sintético que exige China va embebida en el
> archivo, y el metadato se escribe **al encodear**.
>
> **Desde el cycle 22**, el **plano de la puerta**: quién puede jugar, con qué
> identidad, a qué **edad verificada** y con qué consentimiento. Es la capa que
> **las cuatro regiones legislaron en cuatro puntos distintos del stack** —NA en
> el sistema operativo, EMEA en el wallet del jugador, APAC en un registro
> estatal, LATAM en la plataforma con cuenta parental—, de modo que el *age gate*
> **no es un componente sino cuatro adapters detrás de una interfaz** (Receta
> 61). Y es la primera capa de esta KB donde **una norma nombra un repositorio**:
> el Annex A §A.8 del perfil europeo manda `longfellow-libzk-v1` y declara que
> cualquier otro sistema de prueba de conocimiento cero **no es conformidad**.
> Trae tres regímenes legales en un mismo stack obligatorio —spec **CC-BY-4.0**,
> bibliotecas **Apache-2.0/MPL-2.0**, apps **EUPL-1.2**— y el modo de falla
> cambia: si esta capa falla, no se ve mal, **nadie entra**.

>
> **Desde el cycle 23**, el **plano del dispositivo**: cómo el jugador **toca** el
> juego — entrada, mando, háptica, runtime XR, manos y ojos. Es la **primera capa
> de esta KB donde el estándar convergió SOLO**: OpenXR es Apache-2.0 y Khronos
> publica hasta la **suite de conformance** (`OpenXR-CTS`, 11.358 B), sin que
> ninguna norma lo obligue — al revés del cycle 22, donde la UE *nombraba* un
> repositorio. La fricción se movió al **objeto físico**: un mando inalámbrico
> necesita **cuatro homologaciones** (FCC · RED+EN 18031 · Giteki+PSE · ANATEL) y
> **ninguna región acepta el certificado de otra**, así que —a diferencia del age
> gate— **esto no se puede esconder detrás de una interfaz**: son cuatro
> expedientes. Y trae el primer reloj **ya vencido** de esta KB: el estudio que
> empaqueta Monado + SDL es **fabricante** bajo el CRA y reporta desde el
> **11-sep-2026**, mientras el upstream es *steward* y no entra hasta el
> **11-dic-2027, sin sanciones**.

> **Desde el cycle 24**, el **plano del cable**: cómo dos jugadores comparten un
> mismo estado — transporte, replicación, rollback, relay y sesión. Es la capa que
> **todo juego multijugador atraviesa sin excepción** y la que esta KB tenía más
> vacía: `netcode`, `rollback`, `replication` y `dedicated server` medían **cero
> ocurrencias en 23 passes**. Es la **primera capa donde la gran pregunta
> regulatoria de 2026 se respondió NO**: la ICE *Stop Destroying Videogames* llegó
> con **1.294.188 firmas verificadas** y la Comisión contestó el **16-jun-2026**
> que **no propondrá requisitos obligatorios**, sólo un **código de conducta
> voluntario** —no publicado a 2026-11-15—, así que el *apagado digno* del
> servidor es **diferenciador de producto, no requisito legal**. Y es la primera
> capa donde el riesgo medido no es conformidad ni homologación sino
> **supervivencia del proveedor**: **Hathora cerró el 5-may-2026** y **Rivet
> repivotó a infraestructura de agentes**, mientras las siete plataformas open
> source de la capa siguen en pie.

> **Último pass: 2026-11-25 (cycle 24).**

## Estructura

```
gaming-kb/
├── agents/        # Agentes AI open source de la industria
├── repos/         # Repos MIT/Apache como punto de partida
├── verticals/     # Plataformas verticales customizables con AI
├── intel/         # Mercado, players, tendencias
├── compose/       # Recetas: cómo componer soluciones
└── ingest/        # Scripts de actualización automática
```

## Uso

1. **Nuevo engagement**: leer `intel/market.md` + `repos/foundations.md`
2. **Proponer solución AI**: `agents/top.md` + `compose/patterns.md`
3. **Mantenerse al día**: correr `ingest/update.sh` semanalmente

## Cómo leer esta KB

- **`agents/trending.md` y `repos/trending.md` son APPEND-ONLY.** Cada pass
  agrega una sección fechada arriba y conserva la historia debajo: la historia
  es el dato.
- **Toda licencia de esta KB está leída en bytes del archivo del repo**, no
  tomada del badge de la API. Las reglas de método viven en `agents/top.md` y el
  procedimiento ejecutable en `compose/patterns.md` → **Receta 20**.
  🔴 **Cinco cosas que esta KB aprendió a no confiar: el badge no es el grant,
  el conteo de bytes no es el grant, la rama default no es el repo, *el repo no
  es la organización* (cycle 12), y —desde el cycle 13— **la raíz no es el
  repo**: `meta-llama/PurpleLlama` tiene **14 archivos de licencia y 5 grants**,
  con la raíz **menos** libre que cuatro de sus subcarpetas, que son MIT.
  Y el grant puede estar **en el `README.md`** (`inkle/inky`: 2.758 ★, badge
  `null`, MIT verbatim en la prosa).** Un producto MIT puede tener al
  lado un repo auxiliar sin ningún grant (`TaloDev/hosting`), y un repo con
  **cero** archivos de licencia puede tener el grant **declarado en el README y
  alojado afuera** (`QwenLM/Qwen3`). **`enumeración = 0` es ambiguo, no
  negativo:** el procedimiento para resolverlo es la **Receta 30**.
  🔴 **Y la sexta, del cycle 14: el repo no es su clausura.** La restricción
  puede vivir **fuera del repo**, en una dependencia que sólo se nombra en la
  prosa del `README.md` — `microsoft/TRELLIS` y `TRELLIS.2` (**27.514 ★
  sumadas**) pasan los tres canales de esta casa como MIT limpias y sus tres
  renderers importan paquetes **no comerciales**. El procedimiento es el
  **canal 4** y la **Receta 36**.
  🔴 **Y la séptima, del cycle 22: el archivo de licencia no es la licencia.**
  Puede ser un **PUNTERO**: `paemuri/brdoc` (**206 B**) dice "ver
  `LICENSES/MPL-2.0.txt`" y además que **un aviso por archivo sobrescribe la
  raíz**; la spec de verificación de edad de la UE (**227 B**) sólo apunta a una
  URL de Creative Commons. Un lector que cuente bytes concluye "licencia corta
  tipo MIT" y se equivoca las dos veces. El procedimiento es el **canal 9** y la
  **Receta 63**. Corolario medido: **la confiabilidad del byte-count es por
  FAMILIA** — Apache-2.0 midió **8 pesos distintos en un solo pass** (9.452 a
  11.606), mientras **GPL-3.0 midió 35.149 B en tres repos independientes**.
  🔴 **Y la octava, del cycle 23: el archivo más CORTO puede ser el más
  RESTRICTIVO.** `Unity-Technologies/InputSystem` (**1.527 ★**, vivo) tiene el
  `LICENSE.md` **más corto de todo el catálogo —491 B—** y **no es open source**:
  es un **puntero a un destino propietario** (Unity Companion License) cuyo grant
  está condicionado a *"Unity-dependent projects"*. Orden medido en un solo pass:
  **491 B (propietario) < 884 B (Zlib, permisiva) < 1.064-1.131 B (MIT) <
  1.280/1.338 B (BSL-1.0) < 1.488/1.518 B (BSD-3)**. Y el otro extremo existe
  también: `hifihedgehog/PadForge` tiene **251.044 B** de `LICENSE` que son
  CC-BY-NC-SA-4.0 + un epígrafe + **4.700 líneas de manifiesto de atribución de
  terceros** — *NonCommercial*, es decir inusable, con push de ayer.
  🔵 **Y el corolario del método: BSL-1.0 midió 1.280 B y 1.338 B para un grant
  cuyo texto operativo es palabra por palabra idéntico** — los 58 B de diferencia
  son la línea de título y **el ancho de plegado de línea**. La varianza del
  byte-count puede ser **tipográfica**, no jurídica. El procedimiento del upstream
  inalcanzable es el **canal 10** y la **Receta 64**; el de no confundir un canal
  muerto con un recurso muerto, la **Receta 65**.
  🔴 **Y la novena, del cycle 24: el proveedor no es la licencia, y la licencia
  puede ser función de la VERSIÓN.** `Unity-Technologies/com.unity.netcode.gameobjects`
  (**2.3k ★**) es **MIT** (1.075 B) en `develop` y `release/1.0.0`, y **Unity
  Companion License** (501 B, propietaria, condicionada a *"Unity-dependent
  projects"*) en `release/2.0.0` y en **`develop-3.x.x`, que es la rama default**.
  La frontera está en la **v2.0.0** y **las dos licencias coexisten hoy** en el
  mismo repo. Las 23 particiones de grant catalogadas antes eran **espaciales**
  (repo, organización, subdirectorio, raíz, rama, archivo); ésta es **temporal**.
  Corolario obligatorio: **toda afirmación de licencia lleva la rama o la versión
  donde se midió** — *"NGO es MIT"* no es falso, es **incompleto**, y lo que falta
  invierte el veredicto. El procedimiento es el **canal 11** y la **Receta 68**.
  Y el mismo proveedor publica **MIT en el cable (v1)**, **UCL en el cable (v2/v3)**
  y **UCL en la entrada** (`InputSystem`, 491 B, cycle 23): no se puede decir
  "Unity es X".
  🔴 **Y la décima: un grant puede prohibir la competencia por escrito.**
  `FirstGearGames/FishNet` (**2.1k ★**, vivo) tiene una licencia propia de
  **3.640 B**, sin SPDX, cuyo §2.b dice que *"other products of like Software
  (explicitly **networking solutions**) may not use, reverse engineer, or
  implement Software in part or full"*. Un juego hecho con FishNet está permitido
  (§2.d); **un entregable de red para un cliente no**. Es el extremo opuesto de
  **FCL-1.0-MIT** (cycle 19), que *permite por escrito* los *"professional
  services provided to a licensee"*. **Las licencias de 2026 codifican el modelo
  de negocio del licenciante, a favor o en contra del integrador — y ninguna de
  las dos se detecta por peso de archivo.**
  🔵 **Y la corrección de instrumento del cycle 24: el conteo de bytes se mide
  sobre ARCHIVO, no sobre variable de shell.** `$(curl …)` **elimina los `\n`
  finales**, así que todo archivo terminado en salto de línea mide **un byte
  menos**: el primer barrido de este pass dio `open-match` = 11.344 B contra los
  **11.345 B** correctos del registro. **Un byte de diferencia no es deriva de
  licencia hasta haber descartado el idioma de la herramienta.** Y en el mismo
  pass, `SpacetimeDB` movió su **Change Date de 2031-11-20 a 2031-11-22** (v2.16.2
  → v2.16.3) **con el archivo en los mismos 42.562 B**: la licencia cambió de
  contenido **sin cambiar de tamaño**, de modo que un detector por `size` no ve
  nada. El reloj BUSL **es de la versión, no del proyecto**.
  🔵 **Y las tres formas de leer mal un grant, las tres cometidas y corregidas en
  el cycle 24 (Receta 69):** (1) **falso ENUM=0** — `lucasteles/Backdash` es MIT
  en un archivo llamado **`LICENCE.md`**, ortografía *británica*, invisible a una
  sonda que enumere sólo `LICENSE*`; (2) **falso UNKNOWN** — **el grant MIT puede
  no llevar título** y arrancar en `Copyright (c)` (`enet` 1.060 B,
  `godot-rollback-netcode` 1.062 B, `Facepunch.Steamworks` 1.065 B), así que hay
  que buscar la **cláusula operativa** y nunca el título; (3) **falso positivo** —
  el `LICENSE` de **216 B** de `gschup/ggrs` es un **puntero** que nombra *"Apache
  License"* y en realidad ofrece **dual MIT/Apache a elección del usuario**.
- **`curl` y `WebFetch` no son el mismo canal contra el mismo host.** Medido en el
  cycle 24: `github.com` responde **403 a `curl`** y **200 a `WebFetch`** (★,
  `archived`, rama default), mientras `raw.githubusercontent.com` responde **200 a
  ambos** y `api.github.com/repos/...` y `codeload.github.com` dan **403**. Antes
  de declarar un host política-muerto, **probar los dos canales**.
- **En la capa de modelos generativos, código y pesos son dos licencias
  distintas, en archivos con nombres distintos** (`LICENSE_weights`,
  `MODEL_LICENSE`, o un addendum dentro del propio `LICENSE`). Medición del
  cycle 14: **3 de 3** proyectos de audio con pesos licenciados aparte los
  tienen en **modo no comercial**.
- **Las oportunidades de `intel/market.md` están organizadas bajo un único
  encabezado `## Opportunities by region`** con una subsección por región
  (North America / EMEA / APAC / LATAM). **Los gaps se escriben como gaps**: un
  cero medido es información, un silencio parece cobertura.
- ⚠️ **"Gaming" es ambiguo en fuentes regulatorias**: las consultas con esa
  palabra devuelven la industria del **juego de azar**. El material de *iGaming*
  de esta KB está etiquetado como carril separado y **no debe mezclarse con
  videojuegos**. Para investigar la industria, el instrumento correcto dice
  **"video game"**.

---
*Red de KBs Globant AI Studios → [globant-kb](../globant-kb/)*
