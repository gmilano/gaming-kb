---
industry: gaming
region: Global
updated: 2026-11-25
---

## 🆕 Tendencias — pass cycle 25 (2026-11-25)

### 1. 🔴 La tendencia de fondo: **la capa de simulación se consolidó en fundaciones, no en estudios**

Secuencia medible en los repos, no en la prensa:

| Pieza | Quién la gobierna hoy | Licencia **medida** |
|---|---|---|
| **Newton** (5.987 ★, 1.0 GA en GTC 2026) | **Linux Foundation**; contribuida por Disney Research + Google DeepMind + NVIDIA | Apache-2.0 (10.173 B) |
| **MuJoCo** (18.474 ★) | Google DeepMind | Apache-2.0 (11.358 B) |
| **Gymnasium / PettingZoo / ViZDoom** | **Farama Foundation** | MIT (1.104 / 1.073) · **ninguna** |
| **Genesis** (30.310 ★) | Consorcio académico (renombrado a `genesis-world`) | Apache-2.0 (11.356 B) |

🔵 **Lo que significa para Globant:** la capa que simula el mundo **ya no es un
activo competitivo de un vendor**: es infraestructura compartida con grant
permisivo. Es el patrón contrario al del cycle 19 (el líder de flags **se fue**
de Apache) y al del cycle 16 (la categoría se fue del open source en bloque).
**Cuando una capa se mueve hacia una fundación, el riesgo de relicenciamiento
baja y el de bus factor se reparte.**

### 2. 🔴 La tendencia que esta KB puede afirmar por medición propia: **el determinismo es el cuello de botella de 2026, no el ancho de banda**

El cycle 24 midió el cable y encontró transporte resuelto (kcp 17k, GNS 10,1k) y
rollback diminuto (máximo vivo 707 ★). Este pass explica **por qué**: el rollback
exige que **la simulación sea una función pura de los inputs**, y eso obliga a
abandonar la coma flotante. Medido hoy, la oferta de punto fijo entera suma
**menos de 1.400 ★** (`libfixmath` 888 + `soft-float` 224 + `FixedMathSharp` 97 +
`Klotho` 88 + `GridForge` 23 + `SwiftCollections` 14 + `Trailblazer` 6 +
`Gravitas` 4 + SG Physics 53 en GitLab) frente a **más de 50.000 ★** sumadas por
los motores de física que **no** lo garantizan.

📌 **Tendencia accionable:** el valor por hora de ingeniería en esta capa está en
**el analizador, no en el solver**. La única pieza preventiva medida en 25 passes
es el `DeterminismAnalyzer` de Klotho (marca `float` y APIs no deterministas en
tiempo de build). Replicar eso para C++/Unreal es un **producto que hoy no
existe**.

### 3. 🟡 La tendencia de licencia: **el carve-out ya no es noticia, el PUNTERO sí**

Esta KB midió carve-out por directorio en identity (22), analítica (16), flags
(19) y ahora en física: **`bulletphysics/bullet3`, 14.894 ★, zlib excepto
`Extras/` y `examples/ThirdPartyLibs`**. Lo nuevo de 2026 es la otra forma: el
archivo de licencia que **no otorga, redirige**.

| Forma | Caso de este pass | Bytes |
|---|---|---|
| Puntero a otro archivo del repo | `isaac-sim/IsaacLab` → *"Please see README.md"* (y el README nombra 4 rutas más) | 1.690 |
| Puntero a un host externo | `Farama-Foundation/ViZDoom` → una frase del README → `zdoom.org/wiki/license` (**host inalcanzable**) | 0 |
| Puntero a un PDF fuera de GitHub | `isaacsim-license.txt` → *"additional components licenced under other terms"* en un PDF de NVIDIA | 10.770 |
| Puntero por referencia legítimo | `Unity-Technologies/ml-agents`: 572 B en `develop`, 11.324 B en `main` | 572 / 11.324 |

> 🔵 **La regla que esta KB puede llevar a todas las industrias:** *el número de
> saltos* entre el badge del repo y el texto que efectivamente otorga derechos es
> una **métrica de riesgo**. Cero saltos = MIT en la raíz. **Tres saltos =
> ViZDoom** (badge → README → wiki externo → esquemas mixtos de terceros), y el
> tercero no se puede abrir.

### 4. 🟢 La tendencia de producto: **el simulador de robótica es el nuevo tooling de gameplay**

Cinco piezas pensadas para robots, medidas hoy con push de las últimas 48 h
(Newton, Warp, mujoco_warp, mjlab, Genesis), **todas Apache-2.0**. Lo que un
estudio saca de ahí no es un robot: es **animación procedural con física
aprendida, ragdolls que se recuperan, y controladores de locomoción entrenados en
vez de animados**. Es la continuación natural de lo que esta KB ya tenía con
`MuJoCo` en `repos/foundations.md`, pero con un carril de entrenamiento completo
y permisivo que **antes de este pass no estaba inventariado**.

⚠️ **El límite, medido:** `mani-skill/ManiSkill` publica **código Apache-2.0 y
assets CC BY-NC 4.0**, y `Nekuzaky/kinema-motion-matching` incluye un personaje
**Mixamo** explícitamente no cubierto por su MIT. **En esta capa el dato de
entrenamiento y la demo son objetos de licencia distintos del código**, igual que
en la capa de generación de assets del cycle 14.

### 5. 🔴 La tendencia de mercado que contradice el optimismo de la prensa

Lo que las fuentes de 2026 dicen de la industria (y esta KB no puede verificar
por repos, así que se cita como lo que es — **encuestas y análisis**, con los
hosts inalcanzables desde esta sesión y por tanto citados en prosa):

- **KPMG + UNLV International Gaming Institute, *The State of AI in Gaming
  2026***: encuesta de **83 compañías**; **más del 80 %** usa IA generativa para
  contenido e insights, pero **sólo 1 de cada 5** declara retorno significativo,
  y el índice de madurez de IA del sector **no pasa de 45 sobre 100**. La
  adopción de **agentes** va muy por detrás de la de generativa.
- **Omdia (*Trends to Watch 2026, Games Tech*)**: cuatro ejes — visibilidad
  creciente de la IA en los juegos, **reacción adversa del público**, cambio de
  dinámica en el mercado de engines y **subida de costes de servidor e
  infraestructura**.
- **GamesBeat / AlixPartners**: ~**90 %** de los desarrolladores usa IA en alguna
  parte del pipeline; KPMG espera que las integraciones profundas queden
  **detrás de la escena**, no en contenido de cara al jugador; AlixPartners
  proyecta múltiplos de valoración **2–3×** para quien integre IA sobre IP
  fuerte.

🔵 **Por qué esto encaja con el plano del cuerpo mejor que con cualquier capa
anterior:** es exactamente **"detrás de la escena"**. Un controlador de
locomoción entrenado, un navmesh determinista o un solver de física no generan
contenido sintético, **no se anuncian al jugador y no disparan la reacción
adversa** que la propia industria mide. Es la capa donde la IA se puede usar sin
el coste reputacional que el cycle 4 midió (≈85 % de rechazo global, 48,8 % de
aceptación en LATAM).

## 🆕 Tendencias — pass cycle 24 (2026-11-25)

### 1. 🔴 La tendencia de fondo: **la regulación llegó a esta capa y decidió NO legislar**

Secuencia que esta KB puede leer en sus propios passes, ahora con un término que
faltaba: el **no**.

| Momento | Dónde estaba la fricción | Cómo se resolvió |
|---|---|---|
| cycles 10-11 | Etiquetar contenido AI (Art. 50) | En el pipeline de contenido |
| cycle 19 | Qué ve y qué paga cada jugador | En el plano de decisión |
| cycle 20 | Cobrar fuera de la tienda del fabricante | **Por ley**, en cuatro regiones |
| cycle 22 | Quién entra y con qué edad | Cuatro arquitecturas de software |
| cycle 23 | Cómo el jugador toca el juego | Software convergió solo; hardware divergió por regulación |
| **cycle 24** | **Si el publisher puede apagar el servidor** | 🔴 **1.294.188 firmas verificadas → la Comisión respondió que NO legislará.** Vía: código de conducta **voluntario**, sin publicar a 2026-11-15 |

**La novedad no es que la capa esté desregulada. Es que se intentó regularla con
el instrumento democrático más fuerte que tiene la UE —una iniciativa ciudadana
con 1,29 M de firmas— y el resultado fue un compromiso voluntario.** Para esta KB
eso es tan informativo como una obligación: define que, en esta capa, el
*fin de vida digno* es **diferenciador de producto**, no requisito de entrada.

### 2. 🔴 El grant como arma competitiva: de "permite consultoría" a "prohíbe competencia"

Dos passes, dos extremos del mismo eje, los dos medidos en texto de licencia:

| Pass | Grant | Qué hace con el tercero que cobra por integrar |
|---|---|---|
| cycle 19 | **FCL-1.0-MIT** (FLIPT) | ✅ Lo **permite por escrito**: *"professional services provided to a licensee"* |
| **cycle 24** | **FishNet propia** (3.640 B) | ⛔ Lo **excluye por escrito** si el entregable es *"networking solutions"* |

**La tendencia que esto marca:** las licencias de 2026 dejaron de ser sólo una
elección entre permisivo y copyleft y empezaron a **codificar el modelo de negocio
del licenciante** — a favor (FCL) o en contra (FishNet) del integrador. Ninguna
de las dos es OSI. Ninguna de las dos se detecta por peso de archivo. **Las dos
deciden si Globant puede o no tomar el trabajo.**

### 3. 🔵 La relicenciación por versión mayor, que es nueva en esta KB

Unity movió `com.unity.netcode.gameobjects` de **MIT (v1)** a **Unity Companion
License (v2, v3)**. Las 23 formas de partición de grant que esta KB había
catalogado eran **espaciales** (por repo, por organización, por subdirectorio,
por raíz, por rama, por archivo). Ésta es **temporal**: la licencia es función de
la **versión**, y las dos versiones **coexisten hoy** en ramas vivas del mismo
repo.

**Consecuencia de método, generalizable fuera de gaming:** una afirmación de
licencia **tiene que llevar la versión o la rama donde se midió**. *"NGO es MIT"*
no es falso: es **incompleto**, y la parte que falta invierte el veredicto.

### 4. 🟢 Donde esta capa está resuelta y donde no: la forma de la curva de estrellas

| Eslabón | Máximo vivo medido | Lectura |
|---|---|---|
| **Transporte** | **17.0k ★** (kcp) · 10.1k (GNS) · 3.3k (enet) | 🟢 **Resuelto.** Varias opciones permisivas, maduras, sustituibles entre sí |
| **Replicación** | **7.6k ★** (Colyseus) · 6.4k (Mirror) | 🟢 **Resuelto en permisivo**, y con el competidor propietario (NGO v3, FishNet) por debajo en ★ |
| **Rollback** | 🔴 **707 ★** (ggrs); el clásico `pond3r/ggpo` **quieto desde 2015** | 🔴 **No resuelto.** Seis sucesores vivos, todos pequeños. Se **construye**, no se elige |
| **Relay / NAT** | 2.8k ★ (libdatachannel, MPL-2.0) | 🟠 Resuelto, con el único copyleft de la capa — y con gemelo permisivo (pion) |

### 5. 🔵 La divergencia regional de adopción de IA alcanzó su máximo medido

No es tendencia de la capa, es del mercado, y este pass la mide en su punto más
extremo hasta ahora:

| Región | Cifra medida 2026 | Fuente |
|---|---|---|
| **APAC** (Japón) | **100%** de las empresas de juego online encuestadas usan IA generativa (Gemini 94%) | JOGA 2026 |
| **APAC** (Japón) | **85,8%** de desarrolladores, desde **51%** un año antes | CESA |
| **APAC** (China) | **>60%** con IA en el pipeline | prensa sectorial |
| **North America** | 🔴 **52%** dice que la IA generativa es **negativa** para la industria (desde 30% y 18%); **36%** la usa | GDC 2026 |
| **LATAM** | Parte grande de los jugadores **dispuesta** a consumir juegos hechos con IA | PGB Latam 2026 (gamescom latam) |

**Misma tecnología, dos mercados en direcciones opuestas.** Japón pasó de 51% a
85,8% en un año mientras NA pasó de 18% a 52% **de rechazo**. 🔵 **Y la capa de
este pass es el refugio de esa tensión:** el cable es infraestructura, no
contenido generado, así que **no carga el backlash** que sí cargan assets, voces
y arte. Un proyecto de IA en gaming con riesgo reputacional bajo vive en el
backend, no en el pixel — que es exactamente lo que KPMG anticipó para 2026
(*"las integraciones más profundas de IA llegarán por el back end"*).

### 6. ⬜ Lo que esta capa NO tiene, medido y escrito como ausencia

- **Agentes de IA para netcode: cero.** Ni tuning de predicción, ni detección de
  desync asistida, ni generación de cargas de prueba. Buscado explícitamente.
- **Mediación de anuncios open source: cero** (sin cambio desde el cycle 19).
- **Norma de arquitectura de red para videojuegos: cero en las cuatro regiones.**
  Lo más cerca es el requisito de **localización de datos de Vietnam** (Ley de
  Ciberseguridad 2025, aplica desde **2026-07-01**, decreto de implementación
  **pendiente**) y, en China, no una regla de servidor sino el **socio de
  publicación licenciado** obligatorio.

---
## 🆕 Tendencias — pass cycle 23 (2026-11-25)

### 1. 🔴 La tendencia de fondo: **el estándar de software ganó; la frontera se movió al objeto físico**

Secuencia que esta KB puede leer en sus propios passes, ahora completa hasta el
sustrato:

| Momento | Dónde estaba la fricción | Cómo se resolvió |
|---|---|---|
| cycles 10-11 | Etiquetar contenido AI (Art. 50) | En el pipeline de contenido |
| cycle 19 | Qué ve y qué paga cada jugador | En el plano de decisión |
| cycle 20 | Cobrar fuera de la tienda del fabricante | Por ley, en cuatro regiones |
| cycle 22 | Quién entra y con qué edad | Cuatro arquitecturas de software |
| **cycle 23** | **Cómo el jugador toca el juego** | 🔵 **El software convergió solo (OpenXR, Apache-2.0). El hardware divergió por regulación: cuatro homologaciones** |

**La novedad no es que haya estándar. Es que esta vez el estándar llegó sin que
una norma lo mandara** —al contrario del cycle 22, donde el Annex A §A.8 europeo
*nombraba un repositorio* y lo volvía requisito de conformidad—. Aquí la industria
convergió por su cuenta y publicó hasta la suite de pruebas en Apache-2.0.

⇒ **Consecuencia práctica:** en esta capa el riesgo legal **no está en elegir mal
la biblioteca.** Está en (a) el objeto físico, si el engagement embarca hardware,
y (b) **el último metro propietario del engine**.

### 2. 🔴 El último metro, otra vez: **la pieza que el estudio toca a diario es la única que no es open source**

`Unity-Technologies/InputSystem`: **1.527 ★**, push **2026-11-24**, `LICENSE.md`
de **491 B** que remite a la **Unity Companion License** *"for Unity-dependent
projects"*.

| Capa | Grant |
|---|---|
| Estándar (OpenXR) | ✅ Apache-2.0 |
| Runtime (Monado) | ✅ BSL-1.0 |
| Entrada de bajo nivel (SDL) | ✅ Zlib |
| **Modelo de acciones/bindings dentro de Unity** | 🔴 **Propietario, y sin sustituto permisivo con paridad** |

Es **exactamente la forma** que el cycle 19 encontró en el plano de decisión:
*ocho plataformas permisivas existen; lo que no existe es el SDK de engine*. Dos
capas distintas, mismo hueco, mismo sitio: **el último metro.**

> 🔵 **T94 — el hueco del open source en gaming no está en las plataformas, está
> en la integración con el engine.** Medido ya dos veces en dos capas
> independientes (decisión, dispositivo). Es una tendencia estructural de la
> industria, no una casualidad de catálogo — y es **donde un estudio de servicios
> tiene ventaja**, porque es trabajo de integración, no de producto.

### 3. 🔵 El hardware abierto genera ecosistema en semanas, cuando el fabricante publica los planos

Caso medido, y es el más limpio que esta KB tiene:

| Hecho | Fecha |
|---|---|
| Valve lanza el **Steam Frame** (SteamOS/Arch, Snapdragon 8 Gen 3, 16 GB) | **sep-2026** (⚠️ fuentes discrepan: 14 vs 18) |
| Valve publica **CAD + documento eléctrico del puerto de expansión frontal** | **12-oct-2026** |
| **Seis repos** de terceros, cinco de ellos permisivos, **todos con push en los últimos 7 días** | **nov-2026** |

**Frametop (420 ★, MIT) tiene más estrellas que `KhronosGroup/OpenXR-CTS`
(81 ★).** La atención de la comunidad va al **dispositivo concreto**, no al
estándar que lo hace portable.

⚠️ **Contrapeso obligatorio:** el cluster es de **seis semanas de edad**; uno de
los seis tiene **0 ★** y se declara no oficial. **Señal de tendencia, no
fundación.**

### 4. 🔴 El vector regulatorio de 2026 en esta capa no es el dispositivo: es **la biometría que produce**

Lo que apareció, medido, es litigio civil en EE.UU. sobre **iris y retina**
(demanda vs Apple, jul-2026, $1.000-$5.000 por infracción bajo BIPA) y la
reactivación por el 7º Circuito de un caso de *virtual try-on* (jul-2026).

**Pero la honestidad del registro importa más que la narrativa:** ⚠️ **no se
encontró ninguna demanda BIPA contra eye tracking dentro de un headset de VR.** El
caso Apple es **una demanda, no una sentencia**, y su teoría —que el infrarrojo de
*attention tracking* puede construir un mapa de iris— **todavía no ha sido
validada por un tribunal.** La tendencia es **la plausibilidad del vector**, no un
precedente.

🔵 Lo que sí es sólido y es académico: existe trabajo revisado que establece
**autenticación por mirada en VR sobre conjuntos de datos muy grandes** ⇒ **el
dato de gaze identifica a la persona.** Eso es lo que hace al vector creíble,
independientemente de cómo acabe el caso Apple.

### 5. 🔵 CRA: la primera tendencia de esta KB con **el reloj ya vencido**

El 11-sep-2026 abrió la *Single Reporting Platform* de ENISA y, con ella, la
obligación de **los fabricantes** de reportar vulnerabilidades activamente
explotadas. **Los *open-source stewards* no entran hasta el 11-dic-2027, y sin
sanciones.**

⇒ Un estudio que embarca Monado + SDL en un producto vendido en la UE **es
fabricante**. La tendencia que importa: **durante 2026-2027 la responsabilidad de
ciberseguridad del stack abierto está, legalmente, en quien lo empaqueta — no en
quien lo escribe.**

⚠️ Conflicto de fuentes **sin resolver** (Red Hat y Greenbone dicen que los
stewards reportan desde sep-2026; OpenSSF y la Comisión dicen que no; la página
primaria no resuelve DNS en este entorno). Detalle en `intel/market.md` → EMEA.

### 6. 🔵 La migración de estándar se puede leer en las fechas de push

Sin abrir una sola nota de release:

| Era | Repos | Último push |
|---|---|---|
| **Pre-OpenXR** | `ValveSoftware/openvr` | 8 meses |
| | `GodotVR/godot_openxr_for_godot_3.x` | **4 años** + "maintenance mode" |
| | `Alia5/GlosSI` | **archivado** |
| **OpenXR nativo** | `libsdl-org/SDL`, `wayvr`, `open-brush` | **hoy** |
| | `ALVR`, `WiVRn`, `Silk.NET`, `lovr` | 1-2 días |

> 🔵 **Método reutilizable: `pushed_at` es un indicador de adopción de estándar.**
> Cuando una capa migra, las piezas del estándar viejo no se archivan de golpe —
> **se enfrían.** Un corte en "sin push desde hace >6 meses" separó aquí las dos
> eras con **cero falsos positivos** sobre 46 repos.

### 7. ⚠️ Tendencia de la industria AI que las consultas devolvieron, y su sesgo

Las consultas globales mandadas por el procedimiento devolvieron material de 2026
que **no es de esta capa** pero sí es contexto de engagement:

- **La integración profunda de AI está en el back-end, no en el contenido
  player-facing** (KPMG, vía GamesBeat). Coincide con lo que esta KB mide pass a
  pass.
- **~90 % de los desarrolladores usa AI en algún punto del pipeline** (Google
  Cloud, vía GamesBeat) — **cifra de encuesta, de segunda mano.**
- **Omdia** señala el *AI backlash* como tendencia de 2026 junto al coste de
  servidores. **AlixPartners** predice múltiplos de valoración 2-3× para quien
  integre AI en IP fuerte — **es un pronóstico, no una medición.**

🔴 **Y el sesgo, cuantificado:** las **4 de 4** consultas regionales mandadas
(`AI gaming {region} 2026 adoption regulation players`) devolvieron **iGaming /
apuestas**, no videojuego, **en las cuatro regiones**. Los informes que
devolvieron (KPMG *State of AI in Gaming 2026*, benchmark de 83 empresas de
*gambling* y 113 reguladores) son de **la industria del juego de azar**. El aviso
del README queda **confirmado con medición, no por recuerdo**: el instrumento
correcto para esta industria dice **"video game"**.

---

## 🆕 Tendencias — pass cycle 22 (2026-11-25)

### 1. 🔴 La tendencia de fondo: **la regulación pasó de pedir una etiqueta a pedir una PUERTA**

Secuencia medida por esta KB a lo largo de sus propios passes, que ahora se lee
como una línea:

| Momento | Qué pedía la norma | Dónde se implementaba |
|---|---|---|
| cycles 10-11 | **Etiquetar** contenido generado por AI (Art. 50) | En el pipeline de contenido |
| cycle 13 | **Clasificar** al usuario por categoría de edad (SB 2420) | En la tienda + el cliente |
| cycle 19 | **No targetear** por vulnerabilidad (Art. 5, precios) | En el plano de decisión |
| cycle 20 | **Abrir** el camino del dinero (DMA, MSCA, CADE) | En la tienda y el webshop |
| **cycle 22** | **Probar la edad y el consentimiento antes de dejar entrar** | 🔴 **En el login** — el punto por el que pasa el 100 % de los jugadores |

> La diferencia operativa: etiquetar es un trabajo de pipeline que se corre una
> vez por asset. La puerta se ejecuta **en cada sesión de cada jugador**, y si
> falla, **no se ve mal: nadie entra.** Es la primera capa de esta KB cuyo modo
> de falla es *denegación de servicio a usuarios legítimos*.

### 2. 🔴 Las cuatro regiones eligieron cuatro arquitecturas incompatibles

No es fragmentación de *requisitos* (las cuatro quieren lo mismo: que el menor no
entre). Es fragmentación del **punto de integración**:

- **NA → la señal nace en el sistema operativo.** AB 1043 (California) obliga al
  OS a recolectar edad en el setup del dispositivo y a entregar una **señal** a
  las apps. El desarrollador **consume**; no verifica.
- **EMEA → la prueba nace en el wallet del jugador**, y la norma **nombra el
  algoritmo** (`longfellow-libzk-v1`).
- **APAC → el veredicto nace en el Estado.** El real-name system del NPPA es
  requisito de aprobación; no hay implementación propia posible.
- **LATAM → el deber nace en la plataforma**, con cuenta parental vinculada, y
  **con una salida por producto**: apagar loot boxes por default.

🔴 **Consecuencia para quien construye:** el "age gate" no es un componente, son
**cuatro adapters detrás de una interfaz** (Receta 61). Un equipo que diseñe el
de su región primero y "lo adapte después" va a descubrir que el de NA **no
verifica nada** y el de APAC **no se puede implementar**.

### 3. 🟢 Tendencia técnica: el ZKP salió del paper y entró en una cláusula de conformidad

Hito concreto y fechado: el **perfil Annex A §A.8** de la UE declara
`longfellow-libzk-v1` **el único** sistema ZKP conforme. Alrededor de esa decisión
se formó un ecosistema real en meses:

| Pieza | Quién | Señal de madurez |
|---|---|---|
| `google/longfellow-zk` | Google | El prober de referencia; Apache-2.0 |
| `abetterinternet/zk-cred-longfellow` | **ISRG / Let's Encrypt** | **Segunda** implementación (Rust) ⇒ el esquema deja de depender de un solo proveedor |
| `privacybydesign/longfellow-go` | Yivi | Bindings Go; **compila desde fuente y pinea commit**, por requisito explícito del issue #724 |
| `pipe23-org/zk-age-verifier` | Independiente | El lado verificador; se declara **experimental e inestable** |
| `av-lib-ios-longfellow-zkp` | **Comisión Europea** | Biblioteca oficial iOS |
| IETF CFRG | — | `libZK`/Longfellow presentado en **IETF 125**; estandarización formal **en curso, no cerrada** |

🟡 **La reserva honesta:** "Anonymous Credentials from ECDSA" está publicado pero
**su estandarización formal sigue abierta**, y el verificador de referencia se
autodeclara experimental. ⇒ Es una base para **pilotos y arquitectura**, no para
prometerle a un cliente un despliegue estable hoy.

### 4. 🔴 Tendencia de mercado: las plataformas **compraron**, no construyeron

Movimientos medidos en 2026, todos en la misma dirección:

- **PlayStation** → Yoti (UK/IE), con tres métodos: número de móvil, escaneo
  facial o documento. Quien no verifica **pierde chat de voz y mensajes**, no el
  juego ⇒ la gradación es por **función social**, no por acceso.
- **Minecraft** → Yoti (ene-2026).
- **Xbox** → Yoti + VerifyMy, y después **relajó**: ya no exige verificación
  completa para funciones sociales si la cuenta tiene **actividad de larga data**.
- **Steam** → en Australia pide **tarjeta de crédito australiana**; eSafety
  observó que como método único **difícilmente cumpla el requisito de "fairness"**.
- **Roblox** → verificación obligatoria para chat, global, desde ene-2026 (ya en
  el cycle 13), y en abr-2026 anunció **cuentas segmentadas por edad**: *Roblox
  Kids* (5-8) y *Roblox Select* (9-15).

> 🔴 **La tendencia de producto que se desprende, y es la más accionable:** nadie
> está poniendo la puerta en la **entrada al juego**. La están poniendo en la
> **función** — chat, voz, mensajes, compra, loot box. ⇒ El diseño correcto no es
> un gate de login, es un **catálogo de capacidades con edad mínima por
> capacidad**. Esto también explica el *bypass* por antigüedad de Xbox: si lo que
> se protege es el chat y la cuenta tiene diez años de historial, el riesgo ya
> está acotado por otra vía.

### 5. 🟡 Tendencia de adopción: AI en gaming llegó a *mainstream* y a *backlash* al mismo tiempo

Material de este pass, con su procedencia marcada:

- **GDC 2026 State of the Industry** (>2.000 desarrolladores): uso extendido de
  AI en ideación, prototipado y apoyo a programación.
- **Omdia** marca el *AI backlash* como tendencia 2026, junto con costos de
  infraestructura en alza.
- **KPMG**: las integraciones más profundas van al **back end**, no al contenido
  de cara al jugador ⇒ **coincide con lo que mide este pass**: la capa que se está
  construyendo en serio es infraestructura (puerta, decisión, dinero), no NPCs.
- **BCG** espera una avalancha de juegos nuevos por GenAI. ⚠️ Es **pronóstico**.
- **AlixPartners**: múltiplos 2-3× para quien integre AI con IP fuerte. ⚠️
  Pronóstico de consultora con interés en el tema.
- **Games Workshop** prohibió a sus empleados usar contenido generado por AI
  (ene-2026) ⇒ el *backlash* ya es **política interna de publisher**, no sólo
  sentimiento de jugadores.
- ⚠️ **Cifra que NO se publica:** "más de 7.300 juegos de Steam declaran GenAI,
  casi el doble que 2024" circula atribuida a BCG **vía un tercero**, sin poder
  confirmarse contra el informe original.

### 6. ⚠️ El homónimo se revalida por sexta vez, y ahora con número

`AI gaming {región} 2026 adoption regulation players` devolvió, en **las cuatro
regiones**, el informe **KPMG/UNLV *State of AI in Gaming 2026*** en los primeros
puestos. Es de **casino**. Sus datos —índice de madurez **45/100**, sólo **1 de
cada 5** empresas con retorno significativo, **>80 %** usando GenAI para contenido
e insights, adopción agéntica frenada por *compliance* y seguridad del jugador—
se registran como **mercado adyacente**, no como videojuegos.

🔴 **Y trae un dato que sí vale como señal cruzada:** el mismo informe mide que
reguladores y operadores **invierten el ranking de riesgos**. Los reguladores
ponen arriba el daño al jugador; la industria lo pone **último**. En la capa de
este pass —donde el regulador escribió la arquitectura— esa brecha explica por
qué la norma llegó tan abajo en el stack: **no confió en que la industria lo
hiciera sola.**

## 🆕 Tendencias — pass cycle 21 (2026-11-25)

### 1. 🔴 La capa que lleva el juego al espectador **se parte en el encoder**, y es la primera capa de esta KB donde el copyleft es el default

33 repos medidos hoy, uno por uno. La frontera cae exactamente donde el software
deja de **tocar píxeles** y empieza a **mover bytes**: 12 repos del lado píxel,
**cero permisivos** (GPL-2.0, GPL-3.0, LGPL-2.0, LGPL-2.1, MPL-2.0); 21 del lado
byte, **permisivos salvo uno** (MIT, Apache-2.0, ISC, BSD-2, BSD-3, Unlicense).

La tendencia no es "hay copyleft en multimedia" —eso es viejo—. Es que **la regla
de esta KB ("sólo MIT/Apache") deja de ser aplicable por primera vez en 21 passes**,
y la respuesta correcta no es relajar la regla ni abandonar la capa: es **cambiar
la forma de consumir** (separación por proceso, Receta 56). Lo vendible es que la
decisión ya está tomada y medida, no que se descubra en medio de un proyecto.

### 2. 🔴 La licencia dejó de estar en el repositorio: ahora está en el **Dockerfile**

FFmpeg es el caso límite y es la pieza más usada de la capa. Su grant es una
**función del comando de compilación**: sin flags **LGPL-2.1+**; con
`--enable-gpl` **GPL-2+**; con `--enable-version3` **v3**; con `--enable-nonfree`
el binario queda **no redistribuible** (texto literal del `LICENSE.md`).

Es el **9º eje de partición de grant** que documenta esta KB (los otros 8: por
organización, repo, directorio, rama, comando, tipo de entregable, región y
versión) y el primero donde **auditar el repositorio da la respuesta equivocada**.
En un cliente que consume FFmpeg desde una imagen de Docker Hub o desde el paquete
de la distro, **la licencia efectiva la eligió el empaquetador, no el cliente** —
y nadie la auditó nunca. Receta 57.

### 3. 🔴 La obligación regulatoria de 2026 aterriza **en el mismo punto** que el problema de licencia

China exige la etiqueta **embebida en el archivo** (metadato), con norma técnica
obligatoria (**GB 45438-2025**) y **enforcement activo** (baja de un mini-programa
en casos publicados el 15-sep-2026; tres productos de ByteDance nombrados el
28-abr-2026). El metadato se escribe **al encodear** ⇒ la pieza que cumple la ley
es FFmpeg/OBS, **la pieza copyleft**.

Esto es nuevo para esta KB: en todas las capas anteriores el gate de licencia y el
gate regulatorio eran controles **separados**, en momentos distintos del pipeline.
Acá son **la misma línea**. Una sola receta cubre los dos (58), y el mismo paso
sirve para el **Art. 50(2) de la UE** (vence el 2-dic-2026, a 7 días) — **se
construye una vez y se cobra en dos regiones**.

### 4. 🔴 La tier de **AI caster** está vacía de oferta: hay que construirla, no adoptarla

Medido, no supuesto: 6 candidatos, **4 con 0 ★**, **3 sin ningún archivo de
licencia** (4 nombres probados cada uno), y el líder por ★
([`showlab/livecc`](https://github.com/showlab/livecc), 485 ★) **sin licencia y 13
meses sin push**. El único con grant usable
([`zicojiao/worldcupvoice`](https://github.com/zicojiao/worldcupvoice), MIT, 111 ★)
está atado al SDK **propietario** de Agora.

La contracara es la buena noticia: **las piezas para construirlo sí existen y son
MIT** (`whisper.cpp` 55.496 ★ para el audio, los parsers de replay para el evento,
un LLM para el texto). Receta 59. Y el mapa propietario de la tier
(Medal.tv, Allstar, Overwolf, Opus Clip) confirma que **hay demanda pagando** por
algo que no tiene sustituto abierto.

### 5. 🟢 Epic reparte MIT en la mitad de Pixel Streaming que se modifica

`EpicGamesExt/PixelStreamingInfrastructure` sirve un `LICENSE.md` de **1.051 B**
con **texto MIT verbatim** firmado por Epic Games. El engine sigue bajo EULA. Es
el patrón de **grant partido por componente** que esta KB documentó muchas veces
en contra y acá aparece **a favor**: la pieza que un integrador realmente forkea
—señalización, matchmaker, player web— es permisiva.

### 6. 🔴 El canal de verificación de esta KB se degradó, y el que queda **miente por omisión**

En esta sesión: `curl -sI github.com` → **403 para todo** (incluidos repos que
existen con certeza), `WebFetch` sobre `github.com` → **503**, `gh api repos/…` →
**403**. Quedan dos canales vivos: `git ls-remote --symref` (existencia **+ rama
default real**) y `raw.githubusercontent.com` (licencia **+ byte-count**).

Y el canal de metadatos (`search/repositories` con `repo:`) tiene un defecto nuevo
y grave para censos: **descarta repos en silencio cuando van en lote** (pedí 10,
volvieron 8, sin aviso) y sólo tira `422` cuando el repo problemático va **solo**.
Cuatro repos **vivos** son invisibles para él (`Sunshine`, `OvenMediaEngine`,
`PixelStreamingInfrastructure`, `selkies-gstreamer`).

🔴 **Corrige la firma de rename del cycle 4**: *`repo:` NOT_FOUND + raw 200* ya no
significa "renombrado"; puede significar **"vivo y el canal es ciego"**. Regla
nueva (**T76**): diffear *pedidos vs devueltos* en cada lote, y que **sólo
`ls-remote` fallando** cuente como prueba de ausencia.

### 7. El byte-count: GPL-3.0 entra al club de los que **flagean y no deciden**

Medidos hoy **tres** valores distintos de GPL-3.0: **35.141**, **35.149** y
**35.695**. La regla del cycle 19 para Apache-2.0 (*el checksum flagea, nunca
decide; el incipit dicta*) **se extiende a GPL-3**. Nuevos valores al catálogo:
**LGPL-2.0 = 23.330** (primero de la KB), **Apache-2.0 = 11.466**, **MPL-2.0 =
16.725/16.726**, **LGPL-2.1 = 26.431/26.517**, **AGPL-3.0 = 34.453**.

Y una trampa nueva de clasificador: **un `LICENSE` envuelto en sintaxis de
comentario** (`coturn` sirve BSD-3 verbatim dentro de `/* … */` de C) reporta
`NOASSERTION`. Además `NOASSERTION` **se invirtió respecto del cycle 19**: allá
corrió 4 de 5 permisivo, acá **3 de 5 copyleft débil**. La heurística depende de
la capa y no se hereda entre capas.

### 8. ⚠️ Gap de vocabulario, confirmado por segundo pass consecutivo

Las búsquedas regionales prescritas (`AI {industria} {región} 2026 adoption
regulation players`) devuelven **apuestas online, no videojuegos**, en **las
cuatro regiones**. Dos passes seguidos midiéndolo lo convierten en dato: ese
vocabulario está **capturado por el sector del gambling**. Lo que funciona es
buscar por **instrumento** (`labeling synthetic content`, `NO FAKES Act`) o por
**pieza técnica**. Y ojo con mezclar perímetros: en gambling se mide *"cero
actividad regulatoria de AI en Asia-Pacífico"*, mientras que en **videojuegos y
medios sintéticos APAC es la región más dura del mundo**. Dos perímetros,
conclusiones opuestas.

### 9. Contexto de industria del pass (AI en juegos, no plano del espectador)

Para no mezclarlo con lo medido arriba: el consenso de prensa sectorial de 2026 es
**adopción casi universal en producción** (se cita ~90 % de desarrolladores usando
AI en algún punto del pipeline, dato de encuesta de proveedor) junto con un
**backlash en curso** y la expectativa de que las integraciones más profundas
queden **detrás de escena** y no en contenido generado de cara al jugador. Esto
**coincide** con lo que esta KB mide del lado del jugador (~85 % de actitud
negativa, Quantic Foundry) y es, precisamente, el argumento para que el plano del
espectador se construya con **procedencia firmada y disclosure explícito** desde
el día uno.

---


## 🆕 Tendencias — pass cycle 20 (2026-11-25)

### 1. 🔴 2026 fue el año en que se abrió el canal de cobro — y la oferta open source para juegos llegó vacía

Cuatro cambios legales en 12 meses, uno por región, todos en la misma dirección
(ver `intel/market.md`): **Japón MSCA en vigor 18-dic-2025**, **Apple UE modelo
único 1-ene-2026** (CTF → CTC 5 %), **Brasil/CADE en vigor con iOS 26.5 jun-2026**,
**EE.UU. comisión aún indeterminada** (certiorari concedido 30-jun-2026).

La tendencia no es "hay más competencia en pagos". Es más específica y más
vendible: **el permiso dejó de ser el cuello de botella y pasó a serlo la
ingeniería**. El indicador que lo prueba: en Japón, con el canal abierto desde
hace casi un año, **más del 90 % de los desarrolladores no lo adoptó**. Cuando un
mercado tiene permiso y no lo usa, lo que falta es backend.

### 2. Los protocolos de comercio agéntico se consolidaron en fundaciones, y eso cambió dónde está el repo

2026 movió los tres protocolos de pago para agentes de sus dueños corporativos a
cuerpos neutrales — y **el repo se mudó con ellos**:

| Protocolo | Capa | Movimiento institucional medido | Repo canónico hoy |
|---|---|---|---|
| **ACP** | Checkout | Creado por Stripe + OpenAI | [`agentic-commerce-protocol/…`](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol) (Apache-2.0, 10.947 B, **0 tags**) |
| **AP2** | Autorización | **Donado por Google a la FIDO Alliance** | [`google-agentic-commerce/AP2`](https://github.com/google-agentic-commerce/AP2) (Apache-2.0, 11.359 B) |
| **x402** | Movimiento del pago | **Fundación bajo la Linux Foundation** | [`x402-foundation/x402`](https://github.com/x402-foundation/x402) (Apache-2.0, 11.324 B, 1.538 tags) |

🔴 **La consecuencia práctica, que es un hallazgo de método (T75):** cuando un
protocolo se muda a una fundación, **el repo del vendor no se archiva — se
degrada a fork**. `coinbase/x402` sigue vivo, con la misma licencia y el mismo
byte-count, y es el que linkea casi toda la prensa; lo único que lo declara fork
son **las tres primeras líneas de su README**. El filtro `archived` que el cycle
19 declaró obligatorio **no atrapa este caso**.

⚠️ **Los tres son especificación, no producto.** ACP se versiona **por carpeta**
(`2026-04-17`) con **0 tags** ⇒ no se puede pinear como dependencia. Ninguno
publica SDK de Unity o Unreal.

### 3. 🔴 La deriva *source-available* llegó a LATAM, y se la llevó el único insignia regional

El cycle 19 midió la deriva con el canal 7 sobre Unleash (Apache → AGPL,
2026-05-26) y Flipt (tres grants en un repo). Este pass la encuentra **un paso más
lejos**: `LerianStudio/midaz` (Brasil), publicitado como *"el primer ledger open
source de América Latina"*, **ya no es legible públicamente** (401), y su SDK Go
sobreviviente es **Elastic License 2.0 (3.824 B)** — source-available, no open
source. Los índices de terceros siguen publicando `Apache-2.0`.

🟡 **Corrige la regla del cycle 19:** *"el servidor relicencia pero los SDKs se
quedan permisivos"* tiene contraejemplo. Pasa a: **el SDK suele quedarse
permisivo, y hay que medirlo repo por repo.** No se hereda del servidor, ni de la
organización, ni del patrón del pass anterior.

### 4. El byte-count de licencia: el catálogo se amplía y la regla se confirma

Medidos en este pass (todos leídos sobre la rama default real):

| Familia | Byte-counts medidos este pass | Nota |
|---|---|---|
| **Apache-2.0** | **10.174** · 10.749 · 10.947 · 11.324 · 11.325 · 11.357 · 11.358 · 11.359 | 8 valores nuevos o reconfirmados |
| **AGPL-3.0** | **34.520** · 34.523 | la diferencia visible: `http` vs `https` en la URL de la FSF |
| **GPL-3.0** | 35.149 | igual en `COPYING` y en `LICENSE.txt` |
| **MIT** | 1.067 · 1.082 · 1.083 · 1.091 · 1.104 · 1.402 | |
| **BSD-3-Clause** | 1.562 | |
| **Elastic-2.0** | 3.824 | primera medición de ELv2 en esta KB |

🔴 **Uniendo con los valores que la KB ya tenía (10.760 · 11.316 · 11.344 ·
11.352 · 11.356 · 11.357), Apache-2.0 acumula al menos 13 byte-counts distintos.**
Confirma el T70: **el checksum SEÑALA, nunca DECIDE.** Y este pass agrega una
**causa medida** del desvío, que antes era sospecha: `tigerbeetle` mide 10.174 B
porque **omite el APPENDIX** (*"How to apply the Apache License to your work"*) —
verificado leyendo el final del archivo, que termina en `END OF TERMS AND
CONDITIONS`. La licencia es Apache-2.0 íntegra en sus términos.

### 5. 🔴 Canal 8 (nuevo): el predictor de relicencia, y por qué falla en copyleft

El canal 7 **detecta** que una licencia empeoró. El canal 8 intenta **predecirlo**,
preguntando quién puede relicenciar: una empresa única titular puede hacerlo
unilateralmente; un colectivo o una fundación, no.

Resultado sobre 20 repos: **la sonda sólo es válida en MIT/BSD**. En Apache el
titular **no está en `LICENSE`** (el texto trae el placeholder
`Copyright [yyyy] [name of copyright owner]`) y sólo 1 de 8 repos Apache publica
`NOTICE`. En **GPL/AGPL la sonda devuelve la línea de copyright de la FSF sobre el
texto de la licencia** (`Copyright (C) 2007 Free Software Foundation`) — **no la
del proyecto** — con lo cual **miente con formato correcto en 5 de 5**. Regla
**T76**: el canal 8 se corre sobre MIT/BSD; sobre Apache contra `NOTICE`; sobre
GPL/AGPL **nunca** contra `LICENSE`.

Señales accionables que sí produjo:

- **`velopack/velopack`** — el predictor más limpio del pass: **dos líneas de
  copyright en el mismo MIT**, `Caelan Sayler (2021)` **y** `Velopack Ltd. (2024)`.
  La transición individuo → empresa capturada dentro del archivo de licencia.
- **`killbill/killbill`** — titular en cabecera de `pom.xml`, y es una **cadena de
  sucesión corporativa**: `Ning → Groupon → Equinix → The Billing Project, LLC`.
  Un Apache-2.0 de 689 tags cuyo dueño cambió dos veces, invisible desde `LICENSE`.
- **`heroiclabs/nakama`** — `Copyright 2026 The Nakama Authors`: titular
  **colectivo**, señal de durabilidad.
- **CLA vs DCO:** 1 repo con CLA (`Heroic-Games-Launcher`, con
  `.github/workflows/cla.yml`), 1 con DCO (`devaccuracy/ledgerforge`), **18 sin
  régimen declarado**.

### 6. ⚠️ Gap de vocabulario medido: las búsquedas regionales prescritas devuelven apuestas, no videojuegos

Las cuatro búsquedas `AI gaming {región} 2026 adoption regulation players`
devolvieron, en las cuatro regiones, **material de iGaming (apuestas online)**:
reguladores de casino, *player safety* de apostadores, cartas de AI de autoridades
de juego. Útil, pero **no es esta industria**.

Esto es un hallazgo de método, no una queja: **en esta capa la palabra "gaming"
colisiona con la industria del juego de azar**, y la colisión es más fuerte
precisamente en las búsquedas regionales, porque la regulación de apuestas **es**
regional. Lo que funcionó fue cambiar el vocabulario al **instrumento**
(`app store`, `external purchase`, `MSCA`, `CADE`, `DMA`, `anti-steering`) en vez
del sector. Mismo mecanismo que el cycle 19 usó para encontrar el plano de decisión
(cambiar de artefactos a decisiones).

**Lo que no se encontró, dicho explícitamente:** ningún estudio de adopción de AI
entre jugadores de **videojuegos** de LATAM; **cero** actividad regulatoria de AI
específica de gaming registrada en **APAC** (una fuente de benchmark lo afirma con
esas palabras); nada sobre reglas de divulgación de AI a jugadores en títulos de
APAC.

### 7. Contexto de industria del pass (AI en juegos, no camino del dinero)

- Adopción casi universal en desarrollo: **~90 %** de los desarrolladores usa AI
  en algún punto del pipeline; el GDC 2026 State of the Industry (>2.000
  desarrolladores) muestra uso extendido en ideación, prototipado y soporte a
  programación.
- **El back-end, no el contenido de cara al jugador**, es donde se esperan las
  integraciones más profundas (KPMG).
- **Sentimiento del jugador hostil:** ~**85 %** de actitudes negativas hacia la AI
  generativa en juegos (encuesta Quantic Foundry, ~2 M de jugadores).
- **30,8 %** de los lanzamientos de Steam de 2026 llevan divulgación de AI; Valve
  endureció las reglas de divulgación en **ene-2026**.
- Más de **10.000** despidos globales en el primer semestre.
- AlixPartners proyecta múltiplos de valuación **2-3x** para quienes integren AI
  sobre IP fuerte.

⚠️ Las cifras de este bloque **7** vienen de reportes de consultoras, encuestas de
industria y blogs de vendor, con muestras y definiciones distintas. Se publican
como indicativas y con su fuente nombrada, **no como dato medido por esta KB** —
a diferencia de todo lo de los bloques 1-6, que se probó repo por repo.

# Tendencias — Gaming AI

> Última actualización: **2026-11-22 (cycle 19)** (passes previos: 2026-11-22 cycles 18/17/16/15/14/13/12/11/10, 2026-11-21 cycles 9/8/7/6/5, 2026-11-20, 2026-10-26, 2026-07-02).
> Cada tendencia lleva la señal que la sostiene. Donde este pass **refuta** algo
> que el propio KB publicaba, se dice.

## 🆕 Tendencias nuevas y correcciones — pass cycle 19 (2026-11-22)

**Tema:** el **plano de decisión** (flags, *remote config*, experimentos,
recomendación, LTV) y el **canal 7** (censo del carve-out y su deriva). Cuatro
tendencias nuevas, una corrección a una tendencia de esta misma KB publicada hace
una hora, y dos gaps medidos.

### 🆕 Tendencia 35: **la licencia ya no se mueve de un valor a otro; se mueve de PROPORCIÓN**

Las tendencias 33 y 34 de esta página dicen que la superficie de licencia de un repo
es más grande que cualquier sonda y que el componente restringido nunca es el
titular. Este pass agrega el eje que faltaba: **el tamaño del pedazo restringido es
una variable de tiempo, y se mide sin leer una línea de licencia**.

| Repo | Carve-out 2023 | Carve-out 2026 | Dirección |
|---|---|---|---|
| `growthbook/growthbook` | 12 archivos (**0,73 %** del árbol) | **317** archivos (**4,55 %**) | ⬆️ ×26 en archivos, **×6,2 en proporción** |
| `PostHog/posthog` | 247 archivos (2023) | **836** archivos (**1,12 %**, desde 7,35 % en 2022) | ⬆️ en archivos, **⬇️ en proporción** |

**Señal:** dos productos de la misma capa, la misma técnica de licencia (núcleo
permisivo + directorio propietario), **tendencias opuestas** — y ninguno cambió
nunca de licencia, así que ningún canal de los 1-6 lo habría visto. El titular
*"el carve-out de PostHog casi se cuadruplicó"* es cierto y engañoso; el de
GrowthBook es cierto en los dos sentidos. **La proporción es el indicador; su
derivada es el riesgo.**

### 🆕 Tendencia 36: **la capa que decide qué ve el jugador tiene plataformas permisivas; lo que no tiene es SDK de engine**

Medido en este pass: **ocho** plataformas permisivas, auto-hospedables y vivas
(Flagsmith BSD-3, GrowthBook MIT-core, flagd/OpenFeature Apache-2.0,
go-feature-flag MIT, Featurevisor MIT, gorse Apache-2.0, y Flipt/Unleash con
reservas). **Cero** paquetes Unity. **Cero** Unreal. Las tres opciones con SDK de
engine (Unity Remote Config/Game Overrides, GameAnalytics, Joystick) son
propietarias.

**Por qué importa como tendencia y no como dato:** reproduce en una cuarta capa el
patrón que esta KB ya midió en engines, backends y assets — **el open source
permisivo llega hasta el borde del engine y se detiene ahí**, porque quien escribe
infraestructura de decisión trabaja en web/SaaS y quien trabaja en engines no
escribe infraestructura de decisión. Es el hueco donde una práctica de servicios
tiene ventaja estructural: no hay que construir la plataforma, hay que **tender el
último metro**.

### 🆕 Tendencia 37: **la regulación de 2026 pasó del CONTENIDO generado a la DECISIÓN tomada**

Los cycles 10-18 siguieron etiquetas, marcado y categorías de edad: regulación del
**output**. Este pass encuentra una familia distinta, en vigor en tres regiones,
que regula **la decisión**: *surveillance pricing* en EE.UU. (Maryland en vigor
desde 1-oct-2026), precios algorítmicos de plataforma en China (en vigor
10-abr-2026, con **registro de algoritmo** y divulgación de reglas de ranking),
loot boxes y oferta a menores en Brasil (Decreto 12.880/2026), y el Art. 5 del AI
Act en la lectura sectorial de *gambling* (apuntar ofertas de mayor riesgo a
jugadores financieramente vulnerables como práctica potencialmente **prohibida**,
no de alto riesgo).

**Lo que las cuatro piden en términos de ingeniería es lo mismo:** poder **apagar**
la personalización de oferta y precio por jugador y por jurisdicción, y poder
**probar** cuándo se apagó. Detalle por región en `intel/market.md`.

### 🆕 Tendencia 38: **el repo archivado se queda con las estrellas, y eso rompe cualquier ranking por tracción**

`hyochan/react-native-iap`: **3.219 ★**, `archived=true`, y en su propia descripción
declara el sucesor. `hyodotdev/openiap`: **171 ★**, MIT, push del 2026-11-21. **19×
de diferencia a favor del muerto.** Es el *runner-up promotion* del cycle 4, pero la
señal es distinta: en un rename el slug viejo devuelve `NOT_FOUND` por API y 200 en
raw; en un archivo **todo responde 200** y la única pista está en la prosa de la
descripción y en el campo `archived`. ⇒ **`archived` deja de ser un campo
informativo y pasa a ser un filtro obligatorio** en cualquier consulta de esta
familia de KBs.

### 📏 🔴 CORRECCIÓN a la T70 del cycle 18 (publicada hace ~1 hora): **Apache-2.0 no tiene cuatro byte-counts, tiene al menos SEIS**

Medidos hoy, los seis Apache-2.0 legítimos: **10.760** (`prebid/prebid-server`) ·
**11.316** (`Unleash/unleash` pre-v8) · **11.344**
(`OpenAttribution/open-attribution`) · **11.352** (`open-feature/spec`) ·
**11.356** (`gorse-io/gorse`) · **11.357** (`pymc-labs/pymc-marketing`). **600 B de
dispersión.** La mayor parte de la diferencia es el apéndice *"How to apply the
Apache License to your work"*, que algunos repos recortan.

⇒ **Para Apache-2.0 el checksum de bytes puede levantar la mano, nunca
dictaminar.** Confirmar con la primera línea no vacía y con la presencia del
apéndice. Para MIT (1.066-1.102 B observados en 5 repos) y GPL-3.0 (35.149 B en dos
repos independientes) el checksum sigue siendo fiable. **Esta es la tercera
corrección al checksum en tres passes: la técnica estrella de esta KB es un
detector, no un juez, y conviene dejar de tratarla como un juez.**

### ⛔ Gaps medidos este pass, con lo que se buscó

| Gap | Qué se buscó | Resultado medido |
|---|---|---|
| SDK de engine para flags/experimentos | paquetes Unity y Unreal de las 8 plataformas permisivas | **cero y cero** |
| MMP / atribución móvil open source | `open source MMP mobile attribution` | **uno** (`OpenAttribution/open-attribution`, Apache-2.0, 86 ★) y **sin push desde 2025-09-28** ⇒ existe y está parado |
| Mediación de publicidad open source | mediación entre redes | **cero**; la subasta sí (`prebid/prebid-server`, Apache-2.0) |
| Modelo de churn/LTV de juego | reconfirmación del gap del cycle 16 | **sigue en cero**, pero la base permisiva para construirlo es `pymc-labs/pymc-marketing` (Apache-2.0, CLV bayesiano) |
| Regla de juegos sobre oferta personalizada en China/Corea | búsqueda regional dirigida | **cero**: la exposición entra por reglas de **plataforma y precio**, no de juegos |
| Enmienda COPPA 2026 / acción FTC 2026 sobre ofertas personalizadas | búsqueda regional dirigida | **cero verificable**; sólo los casos de 2022-2023 (Epic, USD 520 M) |
| Fuente primaria de Unleash y de Bain | `getunleash.io`, `docs.getunleash.io`, `bain.com` | **ENOTFOUND** en esta sesión. Unleash **sustituido por completo** en el repo (historia + `package.json` + tags); Bain **sin sustituto** ⇒ cifras marcadas como secundarias |

## 🆕 Tendencias nuevas y correcciones — pass cycle 18 (2026-11-22)

**Tema:** el canal 6 (el árbol completo del repositorio). Dos tendencias nuevas, un
gap estructural cerrado y dos gaps reconfirmados con el motivo medido.

### 🟢 GAP CERRADO · **"Listado de directorios de un repo: sigue sin canal"** era falso

Esta sección publicó durante **5 passes**, sin cambios:

> *"**Listado de directorios de un repo**: sigue sin canal. Por eso el grant partido
> de Matrix-Game se verifica por sonda de rutas conocidas y no puede afirmarse que
> `Matrix-Game-4/` sea el único subdirectorio con licencia propia."*

**Hay canal, y es git, no la API de GitHub:**

```bash
git clone --filter=blob:none --no-checkout --depth 1 https://github.com/<slug> <dir>
git -C <dir> ls-tree -r --name-only HEAD      # árbol completo, cero blobs descargados
git -C <dir> cat-file -p HEAD:<ruta>          # el texto, a demanda
```

Medido este pass: **41 repos, 0 fallos, 329+ archivos de licencia**. `gh api
repos/<slug>/contents` sí está cerrado (**403** fuera de los repos de la sesión) —
**la creencia correcta sobre el instrumento equivocado**. Es el **segundo** gap de
canal de esta KB que resulta no existir, después del de octubre sobre
`search/repositories`.

🔴 **Y la lección se repite con una vuelta más:** en octubre, un canal declarado
bloqueado congeló una columna entera durante un mes. Esta vez, un canal declarado
**inexistente** hizo que esta KB publicara durante 5 passes que Matrix-Game tenía
7 archivos de licencia cuando tiene **28**, uno de ellos **no comercial**. La regla
de la rotación —*un canal declarado bloqueado se reintenta cada pass*— hay que
extenderla: **también se reintenta un canal declarado inexistente, y se reintenta
con una herramienta distinta, no con la misma**.

### 🆕 Tendencia 33: **la superficie de licencia de un repo de gaming AI es ~4× más grande que cualquier sonda, y el multiplicador lo fija el gestor de dependencias**

| Repo | Sonda | Árbol | Factor | Por qué |
|---|---|---|---|---|
| [agones-dev/agones](https://github.com/agones-dev/agones) | 3 | **149** | 50× | `vendor/` de Go, commiteado |
| [heroiclabs/nakama](https://github.com/heroiclabs/nakama) | 2 | **81** | 40× | idem |
| [SkyworkAI/Matrix-Game](https://github.com/SkyworkAI/Matrix-Game) | 7 | **28** | 4× | `third_party/` a mano, de papers |
| [node-pinus/pinus](https://github.com/node-pinus/pinus) | — | **14** | — | |
| [recastnavigation/recastnavigation](https://github.com/recastnavigation/recastnavigation) | 4 | **4** | **1×** | repo chico y plano |

**La señal:** el factor **no mide la calidad de la auditoría, mide cómo el proyecto
trae sus dependencias.** Un repo Go con `vendor/` commiteado y un repo de research
con `third_party/` copiado a mano son dos regímenes distintos de riesgo, y hoy esta
KB los trataba igual. Operativamente: **preguntar por el gestor de dependencias
antes de estimar el esfuerzo de una auditoría de licencias** — es la diferencia
entre 4 archivos y 149.

**Y el corolario que importa para vender:** los 146 archivos extra de agones son
**ruido permisivo**. El valor de enumerar no es el volumen, es que **el único grant
que cambia una decisión estaba entre ellos** en 1 de 41 repos. Una auditoría que no
enumera acierta ~97% de las veces y falla exactamente donde duele.

### 🆕 Tendencia 34: **el componente restringido nunca es el titular — y en gaming AI entra por el banco de pruebas**

41 repos, 329+ grants, **un solo** `NonCommercial`: `CC BY-NC 4.0` (18.208 B) a seis
niveles dentro de `GameWorldScore`, el **benchmark** de Matrix-Game. El modelo es
MIT/Apache; lo restringido es **la prueba de que el modelo funciona**.

No es un caso aislado: esta KB ya tenía el mismo patrón en otras dos formas —
`audiocraft` (código MIT, **pesos** `CC BY-NC 4.0`), `YuE` (código Apache-2.0,
**pesos** `CC BY-NC 4.0`), `minerl` (badge `NOASSERTION`, repo entero
`CC-BY-NC-SA`). **La constante es que lo no comercial vive en la periferia del
entregable:** los pesos, el benchmark, el dataset, el harness de evaluación. Nunca
en el `LICENSE` de la raíz, que es el único que mira el equipo legal.

⇒ **Inversión del orden de auditoría.** Primero `third_party/`, `vendor/`,
`external/`, `eval/`, `benchmark/`, `assets/`, `weights/`; **el grant de la raíz al
final**. Es exactamente al revés de como se audita hoy.

⇒ **Y la consecuencia de producto:** en un PoC de world model, *entregar el modelo*
y *reportar la métrica publicada del modelo* son **dos decisiones de licencia
distintas**. Ver Receta 47.

### 📏 7º y 8º defecto del *checksum de bytes* de esta KB

**7º — el filtro de nombre, no el archivo.** Sobre el árbol de Matrix-Game, un
filtro anclado al principio del basename (`^licen…`) encuentra **23** archivos; uno
con `contains`, **28**. Los 5 perdidos (`MG4CF-LICENSE`, `TAEHV-LICENSE`,
`SAL-LICENSE.txt`, `mediamtx-reader.LICENSE`, `THIRD_PARTY_NOTICES.md`) son
**justamente la forma que usa un proveedor para licenciar un componente**. Un canal
nuevo con un filtro malo pierde lo que el canal viejo ya tenía.

**8º — Apache-2.0 es íntegro en cuatro byte-counts distintos.** Medidos este pass:
**9.723** (AccessKit, cycle 16), **10.197 / 176 líneas / sin apéndice**
(Unity-MCP 🆕), **11.335 / 201 líneas** (c2pa-rs 🆕), **11.557 / 201 líneas** (o3de 🆕),
más el testigo histórico 11.357-11.362. Los cuatro leídos íntegros buscando
cláusulas añadidas: **ninguno adulterado**. 11.335 vs 11.557 **con las mismas 201
líneas** es relleno del placeholder `[name of copyright owner]`.

⇒ **Reemplazo:** `wc -l` (**201** con apéndice, **~176** sin) + el literal
`END OF TERMS AND CONDITIONS`. El byte-count es **triage**, no veredicto.

### ⛔ Gaps de canal reconfirmados este pass, con el código medido

| Canal | Test 2026-11-22 | Consecuencia |
|---|---|---|
| `huggingface.co/api/models/*` | **000** (sin conexión) | Licencia de los **pesos** de Matrix-Game, MineWorld y Audio2Face-3D **sin verificar**, 3er pass. En un world model los pesos son la mitad del entregable, y las tendencias 33-34 dicen que **es justamente donde vive lo no comercial** ⇒ este gap ahora es **el de mayor riesgo de la KB** |
| `eur-lex.europa.eu` | **000** | Fuente primaria del EU AI Act **sin leer, 6º pass y 2 KBs**. El 2-dic-2026 (Art. 111(4)) sigue sostenido por 4+ secundarias concordantes |
| `gh api repos/<slug>/contents` | **403** | Cerrado fuera de los repos de la sesión — **y ya no importa: el canal 6 lo reemplaza** |


## 🆕 Tendencias nuevas y correcciones — pass cycle 17 (2026-11-22)

**Tema:** la fábrica (versionado, CI, caché, tracking de producción). Cuatro
tendencias, una corrección a un gap estructural que esta KB declaró tres passes
seguidos, y un conflicto que se cierra.

### 🆕 T-A · **La licencia de un repo es una función del tiempo, y el archivo de hoy no lo dice**

El cycle 16 midió que una categoría entera salió del open source entre 2024 y 2026.
Este pass mide **el mecanismo individual**, y es invisible a los cuatro canales de
licencia de esta KB: `ynput/ayon-backend` pasó de Apache-2.0 a **FSL-1.1-ALv2** el
**2025-01-27** (commit `97627e27`), y el `LICENSE` de hoy es un FSL impecable, **sin
una sola línea que diga que ayer fue otra cosa**.

> **Señal operativa (canal 5):** `git log --follow -- LICENSE` sobre un clone sin
> blobs. Corrido sobre 4 repos de esta capa dio **1 relicenciamiento, 1 cambio de
> dueño sin cambio de grant (`Iterative, Inc.` → `Treeverse.`, 2025-12-01) y 2
> confirmaciones de estabilidad de 8 y 9 años**. Las confirmaciones valen tanto como
> los hallazgos: *"Apache desde el commit inicial, nunca tocado"* es una afirmación
> que esta KB no podía hacer hasta hoy. Receta 44.

### 🆕 T-B · **El corte del grant se mudó del repo al componente: servidor restringido, cliente permisivo**

Segundo caso medido en dos passes consecutivos. Cycle 16: Snowplow, badge Apache-2.0,
componentes desplegables que prohíben producción. Cycle 17: AYON, cliente Apache-2.0,
**servidor FSL**. En los dos la pieza restringida es **la que se despliega**, y en los
dos el badge del repo más visible dice "permisivo".

> **Señal operativa:** en una plataforma de varios repos, la pregunta no es *"¿qué
> licencia tiene?"* sino **"¿qué licencia tiene lo que se despliega?"**. Y la buena
> noticia para una consultora: tanto la FSL como la SSPL/SLULA permiten explícitamente
> **servicios profesionales sobre la instalación del licenciatario**; lo que prohíben
> es ofrecer el producto. Receta 45.

### 🆕 T-C · **La fábrica es el carril exento: ni disclosure, ni backlash, ni consentimiento del equipo**

Tres años de deterioro de sentimiento (18% → 30% → 52-55% de desarrolladores que ven
daño) y ~85% de rechazo declarado del jugador conviven con **~90% de adopción en el
pipeline**. La contradicción se explica sola cuando se separa **lo que el jugador ve**
de **lo que la fábrica hace**: Steam exime las herramientas de desarrollo, y el Art. 50
de la UE obliga sobre la salida, no sobre el build.

> **Señal operativa:** toda receta de esta KB que viva **antes** del artefacto
> jugable (10, 19, 36, 43, 44) se vende sin pelear la discusión cultural. Es, hoy, el
> único carril de crecimiento que no depende de que el equipo del cliente *quiera* IA.

### 🆕 T-D · **La capa de fábrica tiene geografía, y es inversa a la del resto de la KB**

22 repos medidos: la **EMEA** produce las plataformas de producción y las licencia a
la defensiva (FSL, AGPL, GPL); la **APAC** produce la única automatización de QA de
juego permisiva y a escala (Airtest/Poco, Apache-2.0, NetEase); **North America**
produce la infraestructura genérica permisiva (git-lfs, sccache, DVC) y la
arquitectura de referencia **no usable** (GPL + suscripción); **LATAM: 0 de 22**.

> **Señal operativa:** el argumento regional de esta capa no es de costo. En EMEA es
> **licencia**, en APAC es **composición de lo local**, en NA es **presupuesto de
> pipeline (Unreal/Perforce)** y en LATAM es **capacidad**, no producto.

### 🔧 Corrección a esta KB · el gap *"no existe un ERP vertical de gaming"* era **un artefacto de la consulta**

Declarado estructural en los cycles 14, 15 y 16. La consulta que lo produjo
—`open source platform gaming ERP CRM MIT Apache`, la del runbook— **no podía
encontrar la categoría porque la industria no la llama ERP, la llama pipeline /
production tracking**. Existen, están vivas y tienen commits de esta semana: AYON
(Ynput) y Kitsu (CGWire).

> **Regla T67:** antes de declarar un gap estructural, **buscar en el vocabulario del
> oficio, no en el del runbook**. Simétrica de la T64 de la familia.

### 🟢 Conflicto cerrado · las cifras de divulgación de Steam no se contradicen

49,9% (**mes** de octubre 2026) contra 40,1% (**año** acumulado) contra 30,8% (**año
hasta julio**): es una serie mensual creciente medida en ventanas distintas. Citar
como *"casi la mitad de los lanzamientos de octubre y ~40% del año"*. Ver
`intel/market.md`.

---

## 🆕 Tendencias nuevas y correcciones — pass cycle 16 (2026-11-22)

**Tema:** el plano de datos del jugador. Cinco tendencias, una corrección a una
regla de esta KB y un conflicto entre KBs que se declara sin cerrar.

### 🆕 T-A · Una categoría madura no se reemplaza: **se forkea en su último commit permisivo**

Medido en la analítica de producto: entre 2024 y 2026, Snowplow (core → SLULA,
8-ene-2024), Sentry (FSL-1.1), Countly (AGPL §7 modificada), Metabase
(AGPL + comercial), OpenReplay (carve-out `ee/`), Grafana (AGPL-3.0) y Rybbit
(AGPL-3.0) salieron de la ruta permisiva. **La respuesta de la comunidad no fue un
producto nuevo.** Fue `OpenSnowcat`: un fork del último commit Apache-2.0, con 81 ★
y commits de hace nueve días.

> **Señal operativa:** cuando una capa de infraestructura madura se relicencia, no
> busques "la alternativa". **Buscá el fork**, y buscalo por el nombre de la
> organización, no por el del componente. Un producto nuevo en una capa madura
> tarda años; un fork existe desde el día siguiente.

### 🆕 T-B · **El gradiente de estrellas invertido**, 2ª reproducción en esta KB

`snowplow/snowplow`: **7.051 ★**, Apache-2.0, **no se despliega**.
`stream-collector` y `enrich`: **40 ★ y 31 ★**, SLULA, **son el producto**.

La 1ª reproducción fue el clúster `godot-mcp` del cycle 10 (el líder por estrellas
llevaba 7 meses muerto). Con dos instancias independientes en capas distintas, la
regla se puede enunciar: **las estrellas miden atención acumulada sobre un NOMBRE,
y el nombre suele vivir en el repo paraguas.** El objeto que vas a instalar está
en otro repo, con otra licencia y dos órdenes de magnitud menos de estrellas.

### 🆕 T-C · El grant se parte **por el lado de la red**, no sólo por edición o directorio

Snowplow: tracker de cliente **BSD-3-Clause**, pipeline de servidor **SLULA**.
Esta KB ya tenía ejes de partición por edición, por directorio (`ee/`,
`enterprise/`), por territorio (los world models de Tencent), por pesos y por ruta
de importación. **Éste es nuevo: cliente contra servidor.**

> Si tu arquitectura cruza esa línea —y toda telemetría la cruza— **tu auditoría de
> licencia tiene que cruzarla también**. Verificar el SDK y dar por verificado el
> backend es exactamente el error que la tabla de arriba produce.

### 🆕 T-D · El **carve-out por directorio** dejó de ser anécdota: es la plantilla de la categoría

Tres vendors sin relación: PostHog `ee/`, Metabase `enterprise/`, OpenReplay `ee/`.
**Reflejo nuevo:** listar el árbol buscando `ee/` o `enterprise/` **antes** de leer
el `LICENSE` raíz. Y verificar puntero y destino por separado: el `LICENSE` raíz de
OpenReplay apunta a `ee/LICENSE`, que **da 404 en `main` y en `dev`**; el archivo
real es `ee/LICENSE.md`. **2º puntero de licencia roto** en esta KB (el 1º fue el
`LICENSE` que era un fragmento de diff, cycle 8).

### 🆕 T-E · La adopción de IA generativa y su aceptación **divergen**, tercer año seguido

GDC State of the Game Industry: uso **~27% (2024) → 36% (2026)**; "impacto
negativo para la industria" **18% (2024) → 30% (2025) → 52% (2026)**.

> **No es ruido de un año: son tres puntos en la misma dirección.** Toda propuesta
> cuyo valor dependa de que el equipo del cliente *quiera* IA generativa apuesta
> contra una tendencia de tres años. La capa de medición no pide esa adhesión, y es
> por eso que se vende primero.

### 🔧 Corrección a una regla de esta KB: el *tell* del "archivado pero recomendado" **es más débil de lo que se afirmó**

La regla heredada decía que, en los 7 casos registrados, el proyecto muerto venía
recomendado **siempre** por documentación del propio proyecto, y que ése era el
discriminante.

**`CrateBytes` es el 8º caso y lo rompe:** archivado desde el **2025-01-22**, y
quien lo recomienda es una **awesome-list de terceros**, actualizada el 2026-09-21.

> **Sobrevive:** una recomendación nunca es evidencia de vida. **Se cae:** que el
> origen *project-adjacent* fuera el discriminante — las listas de terceros se
> escriben una vez y tampoco se revisan. **El único instrumento de liveness sigue
> siendo `archived` + `pushed_at`, corrido sobre cada fila**, incluidas las que
> llegan por una fuente que parece independiente.

### 🆕 Regla T65 · Un slug **adivinado** que falla no mide ausencia

`repo:opensnowcat/enrich` → 422. `git ls-remote .../opensnowcat/enrich` →
NOT_FOUND. `.../opensnowcat/stream-collector` → NOT_FOUND. **El proyecto existía**:
la convención es `opensnowcat/opensnowcat-*`.

> Antes de declarar un cero, **buscá la organización por palabra clave**. Un 422 o
> un NOT_FOUND sobre un slug que inventaste mide tu suposición sobre el nombre.
> Complementa al cycle 10 (`422` no distingue borrado de renombrado): aquí no hubo
> ni borrado ni renombrado — el slug **nunca fue correcto**. Este pass estuvo a un
> paso de publicar un cero falso sobre la única ruta permisiva de la capa.

### 🆕 Regla T66 · Las sondas de esta KB **tienen forma de GitHub**, y eso puntúa a GitLab como muerto

`GlitchTip` es MIT (1.067 B) y vive en GitLab. En GitHub, `GlitchTip/GlitchTip` son
3 ★ con último push en 2022.

> **Un cero invita a seguir buscando; un cadáver cierra la pregunta.** Por eso este
> modo de fallo es peor que la ausencia. Sonda que funciona:
> `curl https://gitlab.com/<org>/<repo>/-/raw/<branch>/LICENSE`.

### 🔴 Conflicto entre KBs de la flota, **declarado y sin cerrar**: la fecha del Art. 50

La KB *enterprise* registra **dic-2027** y llama obsoleto al "2-ago-2026". Las
fuentes jurídicas de videojuegos de **nov-2026** recogidas en este pass dicen que
el Art. 50 **aplica desde el 2-ago-2026**, que **no fue aplazado por el Digital
Omnibus**, y que los sistemas ya en el mercado tienen hasta el **2-dic-2026** para
el marcado legible por máquina.

> **No se pudo resolver desde esta sesión**: `WebFetch` no resuelve hosts generales
> (2ª medición del gap del cycle 9), de modo que **no hubo acceso a EUR-Lex**.
> **Las dos fechas quedan registradas y ninguna debe citarse a un cliente sobre la
> base de esta KB.** Lo que no depende de cuál gane: el inventario de contenido
> generado hay que tenerlo igual.

### 🆕 T-F · La primera norma que apunta al **mecanismo**, no al medio

Las normas de IA que esta KB venía registrando regulan **lo que el juego muestra**
(divulgación, marcado, *deepfakes*). Las medidas chinas sobre IA antropomórfica
—en vigor desde el **15-jul-2026**— regulan **lo que el NPC hace con un menor**:
prohíben ofrecer a menores servicios de IA construidos sobre relaciones virtuales,
y —a diferencia de otras leyes de *companion AI*— **no excluyen los videojuegos**.

> ⚠️ **Secundaria y sin verificar en fuente** (análisis de Stanford CodeX,
> 19-nov-2026; el host no resuelve en esta sesión). Si se confirma, es la primera
> vez que una norma alcanza **el mecanismo que esta KB lleva 16 passes
> recomendando**, y la consecuencia de diseño es inmediata: los eventos de
> NPC-compañero son una categoría de dato aparte, con su propia tabla, su propio
> TTL y su propia partición por jurisdicción (Receta 42).

---

## 🆕 Tendencias nuevas y correcciones — pass cycle 15 (2026-11-22)

### 🔴 Tendencia 21: **la obligación llegó antes que la oferta — y es la primera vez que pasa en esta KB**

En catorce passes el patrón fue siempre el mismo: existe una oferta open source
abundante y la regulación corre detrás (etiquetado, edad, disclosure). En la capa
operativa de cara al jugador **el orden se invierte**.

| Señal | Medición |
|---|---|
| **CVAA** (EE. UU., FCC) | Obliga a accesibilidad del chat en juegos desde el **1-ene-2019** |
| **EAA** (UE) | **Aplicable desde el 28-jun-2025**; **primeras demandas en Francia, nov-2025** |
| Oferta OSS de accesibilidad | **2 repos vivos y permisivos** de 9 enumerados |
| Oferta OSS de anti-cheat ML | **0** de 8 |
| Oferta OSS de soporte al jugador | **0** de 5 |
| Oferta OSS de moderación de **voz** | **0** de 1 |

⇒ **Consecuencia para el pitch:** en todas las demás capas Globant compite por
integrar mejor. En ésta **no hay con qué integrar**, y el estudio que lo descubre
solo lo descubre tarde. **Es la capa con menor competencia open source y mayor
presión externa de esta KB.**

### 🔴 Tendencia 22: **la función se muda al motor, y eso mata al plugin antes de que nadie lo anuncie**

Godot 4.5 absorbió el lector de pantalla vía **AccessKit**. Resultado medido:

- [lightsoutgames/godot-accessibility](https://github.com/lightsoutgames/godot-accessibility) (MIT, 87 ★): **sin push desde 2021-05-19**, y sigue siendo el primer resultado de todo buscador.
- [mikrima/UnityAccessibilityPlugin](https://github.com/mikrima/UnityAccessibilityPlugin) (Apache-2.0, 85 ★): **sin push desde 2023-05-13**.

Es la **8ª instancia** del patrón *recomendado-pero-muerto* en esta KB, y
confirma la regla de la flota con una causa nueva: aquí el proyecto no murió por
abandono sino **por absorción**. ⇒ **Un plugin cuya función aparece en las notas
de release del engine entra en cuenta regresiva, aunque su repo siga abierto.**

### 🔴 Tendencia 23: **el camino de salida de una dependencia archivada no es neutral en licencia** (2ª instancia medida)

| Original | Sucesor | Movimiento |
|---|---|---|
| [m1maker/SRAL](https://github.com/m1maker/SRAL) — **MIT**, ⛔ archivado 2026-08-05 | [ethindp/prism](https://github.com/ethindp/prism) — **MPL-2.0**, vivo | permisiva pura → **copyleft por archivo** |
| `Piper` — **MIT**, repo archivado *(registrado en el cycle 13)* | repo sucesor — **GPL-3.0** | permisiva pura → **copyleft fuerte** |

Dos capas sin relación, mismo movimiento, y **en ninguno de los dos casos el
mantenedor menciona la licencia** al explicar la migración. ⇒ Deja de ser
anécdota: **al reemplazar un repo archivado, la licencia del sucesor se verifica
antes de aceptar la migración**, como paso explícito del gate.

### 📏 Tendencia 24 (método): **el checksum de bytes sólo vale contra texto plano** — 5º y 6º defectos medidos

| Defecto | Caso | Efecto |
|---|---|---|
| **5º — apéndice podado** | `AccessKit/accesskit`, `LICENSE-APACHE` = **9.723 B** | Apache-2.0 íntegro que termina en `END OF TERMS AND CONDITIONS` sin el bloque *"APPENDIX: How to apply…"*. **Falso negativo** |
| **6º — variante Markdown** | `ethindp/prism` MPL-2.0 = **15.504 B** vs `waldo-vision` MPL-2.0 = **16.725 B**; `ALE` GPL-2.0 `.md` = **17.879 B** | La misma licencia mide distinto según formato. **Falso negativo** |

**Regla operativa nueva:** si el archivo termina en `.md` o su primera línea
empieza con `#`, **el testigo no aplica**. Testigos nuevos para la tabla:
**GPL-2.0 Markdown ≈ 17.880 B**, **MPL-2.0 Markdown ≈ 15.500 B**. Y **tolerancia
±2 B** sobre el testigo canónico (caso `ForgeAX/forgeax-engine`, Apache-2.0 de
11.358 B = un salto de línea final).

### 🔵 Tendencia 25: **el badge puede nombrar una licencia REAL del repo equivocado**

`AccessKit/accesskit`: badge `BSD-3-Clause`; grant real **Apache-2.0 OR MIT**;
el BSD proviene de `LICENSE.chromium`, el código de terceros **vendorizado**.

Es el complemento de la regla T60 de la flota (*`NOASSERTION` no es un veredicto*).
Acá el badge **sí** afirma, y lo que afirma es un componente. ⇒ **Regla
completa: el badge nunca es el grant — ni cuando calla ni cuando habla.** El
grant está en los archivos y, cuando los archivos no bastan, en el `README`.

### 🔵 Tendencia 26 (confirmación cruzada entre KBs): **el preámbulo corporativo derrota al clasificador de licencias**

`SanderMertens/flecs` (8.854 ★): badge `NOASSERTION`, `LICENSE` = **MIT íntegro**
(1.131 B) con una línea extra —`Portions Copyright (c) Meta Platforms, Inc. and
affiliates`— bajo el título. **Mecanismo idéntico** al medido por la KB de
*enterprise* en `Tencent/LoopForge`.

Dos industrias, dos repos sin relación, mismo fallo. ⇒ **`NOASSERTION` + línea de
copyright corporativo adicional = permisiva intacta hasta prueba en contrario**,
y es una regla transferible a cualquier KB de la flota.

### 🟡 Corrección de registro: **la cifra de adopción de IA en desarrollo tiene dos valores en circulación**

~**90%** (Google Cloud, vía GamesBeat) frente a **97%** (*Google Cloud Games
Report 2025*, citado por un competidor directo). Mismo origen declarado,
números y años distintos. ⇒ **no citar el 97%**; usar *"en torno al 90%, fuente
secundaria"* y apoyar el argumento en el **desglose por tarea** del cycle 12.

### ⛔ Lo que este pass midió como CERO, por región

- **APAC:** cero fuentes que vinculen accesibilidad y videojuegos; cero datos de adopción regional.
- **LATAM:** cero fuentes sobre accesibilidad de videojuegos, **medido con el instrumento en español**.

⇒ **El gancho de accesibilidad es NA + EMEA únicamente.** En APAC y LATAM no hay
instrumento sectorial al que anclarlo, y venderlo igual sería vender una urgencia
que el cliente no reconoce.

## 🆕 Tendencias nuevas y correcciones — pass cycle 14 (2026-11-22)

### 🔴 Tendencia nº1 del pass: **la restricción de licencia se mudó fuera del repo** — y es el primer escalón que ninguna auditoría automática puede ver

Señal, no opinión. Esta KB lleva registrada la trayectoria completa, pass por
pass, de **dónde vive la restricción**:

| Pass | Dónde vivía | Caso medido | ¿La ve un escáner de licencias? |
|---|---|---|---|
| cycle 4 | archivo de licencia de la raíz | MonoGame = Ms-PL, no MIT | ✅ sí |
| cycle 6-7 | subdirectorios del repo | `Matrix-Game-4/` con grant propio | ⚠️ sólo si recorre el árbol |
| cycle 10 | **territorio** | Tencent: sin grant en EU / UK / Corea | ❌ no |
| cycle 12 | carve-out por entregable | `ee/`, PolyForm Shield | ❌ no |
| cycle 13 | **modelo de negocio del licenciatario** | YSPL 1.0; Fish Audio (umbral US$ 500k) | ❌ no |
| 🆕 **cycle 14** | **la clausura de dependencias, declarada sólo en prosa** | **TRELLIS + TRELLIS.2** | ❌ **no, y tampoco la enumeración de archivos** |

**El caso, con las cifras.** [microsoft/TRELLIS](https://github.com/microsoft/TRELLIS)
(14.370 ★) y [microsoft/TRELLIS.2](https://github.com/microsoft/TRELLIS.2)
(13.144 ★) — **27.514 ★ sumadas**, los líderes abiertos de imagen→3D — son MIT
en el badge, MIT en el archivo, y limpios en la enumeración. Su `README.md`
dice, en prosa, que sus renderers dependen de `diffoctreerast` /
`nvdiffrast` / `nvdiffrec`. Leídos en su propio repo: **Gaussian-Splatting
License (Inria+MPII)** y **NVIDIA Source Code License**, las dos con prohibición
comercial literal.

⇒ **La tendencia registrable: en la capa de modelos generativos, el grant del
repo ya no describe el artefacto.** Hay que resolver la clausura.

### 🔴 Tendencia nº2: **el peso es un objeto de licencia separado, y en audio está medido como no comercial en 3 de 3**

| Proyecto | ★ | Código | Pesos | Archivo donde vive el segundo grant |
|---|---|---|---|---|
| audiocraft | 23.943 | MIT | **CC BY-NC 4.0** | `LICENSE_weights` |
| YuE | 13.656 | Apache-2.0 | **CC BY-NC 4.0** | `MODEL_LICENSE` |
| soundbox | 239 | MIT | *"does not cover the machine-learning model weights"* | addendum dentro del propio `LICENSE` |

Tres proyectos, tres **nombres de archivo distintos**, ninguno llamado
`LICENSE`. Y la contraparte positiva, que es la que marca el estándar que un
proyecto serio debería seguir: [OpenMOSS/MOSS-TTS](https://github.com/OpenMOSS/MOSS-TTS)
(4.358 ★) escribe en su README *"Models in MOSS-TTS Family are licensed under
the Apache License 2.0"* — **extiende el grant a los pesos explícitamente**. Es
el único de la capa que lo hace.

### 🟡 Tendencia nº3: la **adopción** de esta capa todavía no llegó — y eso es información, no una objeción

Los números que este pass encontró sobre generación de assets (todos de
resúmenes secundarios de GDC 2026, 2.300+ respondentes; **el informe primario no
se recuperó y se declara como tal**):

- **~19%** del uso reportado de AI corresponde a **generación de assets**.
- **~81%** del uso de genAI es **investigación y brainstorming**, no assets
  terminados.
- Cifras de adopción total que **no coinciden entre sí** y miden cosas
  distintas: 36% (uso personal), 52% (estudios con genAI en producción), ~50%
  (BCG), 73% (a16z Games AI Survey), 88% (expectativa de que sea estándar).

⇒ **Lectura honesta: la capa de assets es futuro inmediato, no presente.** Y
eso hace *más* valiosas las tendencias nº1 y nº2: las licencias se están
cerrando **antes** de que llegue el volumen. Un estudio que elija su pipeline
este trimestre lo elige para los próximos tres años.

> ⚠️ **No se promueve ninguna de esas cifras a número de referencia de esta KB.**
> Provienen de resúmenes de terceros, miden cosas distintas y discrepan entre
> sí por un factor de 2,4×. Se publican **con su dispersión a la vista**, que es
> la regla de método nº… de esta página desde el cycle 5: *un número sin su
> muestra no es un número*.

### 🟡 Tendencia nº4: la **plantilla territorial de Tencent** se confirma fuera de los world models

El cycle 10 la encontró en los tres repos de world models y la llamó *"la
plantilla de la casa"*. Este pass agrega el **cuarto repo, en otra línea de
producto** ([Tencent-Hunyuan/Hunyuan3D-2.1](https://github.com/Tencent-Hunyuan/Hunyuan3D-2.1),
4.519 ★, generación de malla), con la **misma primera línea carácter por
carácter**: exclusión de **EU, Reino Unido y Corea del Sur**.

⇒ Con 4 observaciones y 0 contraejemplos, esta KB eleva el hallazgo de
*"peculiaridad de una línea"* a **política de proveedor**: asumir exclusión
territorial en cualquier release abierto de Tencent-Hunyuan y verificar la
primera línea del archivo antes de evaluar.

### 🔵 Señal regulatoria nueva del pass — y **la frontera que esta KB no cruza**

- **Malta, 18-sep-2026:** la **MGA** y la **MDIA** publicaron el *AI Gaming
  Charter*, **voluntario**, 48 páginas, seis áreas de guía, con clasificación de
  herramientas por impacto en el jugador y reporte de incidentes graves de
  sistemas de alto riesgo bajo el AI Act. Consulta previa lanzada el 8-may-2026.
  Un seguimiento de nov-2026 reporta **brechas de supervisión** entre
  licenciatarios.
- ⚠️ **Es carril casino, no videojuegos.** La frontera que esta KB estableció en
  el cycle 8 y revalidó en los cycles 9, 12 y 13 se vuelve a aplicar aquí: se
  cita **como mercado adyacente** y **no** se promueve a dato de gaming.
  Confundirlos quema la credibilidad en la primera reunión con un estudio.
- 🔵 Lo que sí es transferible del documento maltés: **la clasificación de
  herramientas AI por impacto en el jugador** es exactamente el inventario que
  el Art. 50 del AI Act va a exigir a un estudio. El entregable se parece; el
  régimen legal no.

### 🟢 Una práctica de producto que vale como tendencia temprana

[Bingeljell/image-to-3dlab](https://github.com/Bingeljell/image-to-3dlab)
(*AssetFurnace*, Apache-2.0, 1.244 ★) vende como **característica** que escribe
*"la licencia de cada asset"* que produce. En 14 passes es el primer proyecto
que esta KB encuentra tratando la **procedencia de licencia del artefacto
generado** como parte del producto. Si la capa de assets escala, esto deja de
ser una curiosidad y se convierte en requisito de compra — igual que pasó con
las SBOM en software.


## 🆕 Tendencias nuevas y correcciones — pass cycle 13 (2026-11-22)

### 🔴 Tendencia nº1 del pass: **la capa más cercana al jugador se está cerrando — y lo hace con licencias que un badge SPDX no detecta**

Señal, no opinión. Tres de los proyectos más grandes de diálogo y voz cambiaron
de régimen de licencia en los últimos 15 meses, **todos en la misma dirección**:

| Proyecto | ★ | Antes | Ahora | Fecha |
|----------|---|-------|-------|-------|
| **Piper** (TTS) | 11.336 → 6.347 | `rhasspy/piper`, **MIT** | `OHF-Voice/piper1-gpl`, **GPL-3.0** | repo MIT archivado 2025-08-26 |
| **fish-speech** (TTS) | 33.825 | permisiva | **Fish Audio Community License 1.0** — umbral US$ 500k **medido sobre el cliente** | **13-oct-2026** |
| **Yarn Spinner** (diálogo) | 2.876 (núcleo) | MIT en todo | **YSPL 1.0** en los ports Godot-GDScript y Unreal | vigente |

**Lo que define a esta generación de licencias: no prohíben, condicionan por
modelo de negocio.** Umbral de facturación, prohibición de producto competidor,
prohibición de hosting, prohibición de entrenar AI con el código. Y las tres son
**invisibles para una auditoría automática**: GitHub las reporta como
`NOASSERTION` (fish-speech, los ports YSPL) o directamente **no las ve**
(`inkle/inky` aparece como `null` teniendo MIT en el README).

⇒ **Consecuencia de método, y vale para todas las KBs de la familia: a partir de
2026 el badge de licencia pierde valor como filtro de triage.** La única entrada
confiable al inventario es la lectura del archivo en bytes, y el rechazo
automático por badge `NOASSERTION` descarta proyectos usables (NeMo Guardrails es
Apache) al mismo tiempo que deja pasar proyectos bloqueados (fish-speech sale
`NOASSERTION` igual que NeMo).

### 🔵 Tendencia nº2: **el eje regulatorio se mudó de la ETIQUETA a la EDAD**

Lo que esta página venía siguiendo —Art. 50, disclosure de Steam, etiquetado
chino— es **transparencia sobre el contenido**. Lo que entró en vigor y es
exigible **hoy** en Estados Unidos es otra cosa: **verificación de categoría de
edad por usuario**, con la tienda como punto de control (Texas SB 2420, exigible
desde el 4-jun-2026; cinco estados con leyes equivalentes) y con la plataforma
más grande del segmento ya alineada (Roblox: verificación de edad obligatoria
para acceder al chat, global, desde ene-2026).

Y la señal de que no es sólo norteamericano: **Linklaters** describe el escrutinio
regulatorio **europeo** como impulsado sobre todo por **seguridad infantil,
privacidad de datos de menores y explotación financiera** — los mismos tres ejes.

⇒ **La inversión en etiquetado sirve en una jurisdicción. La inversión en una
capa de edad + moderación sirve en las dos.** Si hay que elegir dónde poner el
sprint, el dato dice edad.

### 📏 Tendencia nº3: **tercera confirmación independiente de que la adopción está en el pipeline, no en la pantalla** — y la primera que viene de un censo

| Fuente | Tipo | Qué dice |
|--------|------|----------|
| KPMG (Scott Purdy), predicciones 2026 | opinión experta | las integraciones más profundas serán **en el back end** |
| Bird & Bird, *The Future of Play* 2026 (EMEA) | opinión legal sectorial | la adopción genAI es más fértil en **prototipado y concept art** |
| Steam disclosures (medición propia de esta KB) | **censo** | 49,9% de títulos declaran AI; **sólo ~3% es generación dentro del gameplay** |
| Encuesta Unity 2026 (ya en esta página) | encuesta | código 62% > narrativa 44% > **NPC 40%** |

Cuatro instrumentos distintos, misma conclusión. **Es de las afirmaciones mejor
sostenidas de esta KB** y conviene usarla como ancla comercial: *"AI en el
pipeline"* tiene cuatro fuentes; *"AI en la pantalla"* tiene marketing.

### 🟡 Tendencia nº4: el *backlash* ahora tiene acompañamiento económico en los dos sentidos

- **Omdia** lista el *"emerging AI backlash"* entre las tendencias clave de games
  tech 2026, junto con **costos crecientes de servidores e infraestructura**.
- **>10.000 puestos** eliminados globalmente en el período que cubre una revisión
  de mitad de 2026 — es el contexto en el que el jugador lee cualquier anuncio
  de AI de un estudio.
- **AlixPartners** (pronóstico, declarado como tal): las compañías de gaming que
  integren AI con IP fuerte tendrían múltiplos de valuación **2-3× superiores** a
  los rezagados.
- ~**90% de los desarrolladores** usan AI en algún punto del pipeline (encuesta
  citada por GamesBeat). Una encuesta de proveedor sobre 50+ líderes dice **94%**;
  se registra con el sesgo declarado.

⇒ **Las dos presiones son reales y apuntan a lo mismo:** el mercado de capitales
premia integrar AI, el mercado de jugadores castiga mostrarla. **La estrategia
que satisface a los dos es la del pipeline**, que es exactamente lo que miden las
cuatro fuentes de la tendencia nº3.

### ⛔ Hueco de oferta medido: no hay líder open source de moderación de chat de juego

Consultas `toxicity detection chat moderation` y `content moderation LLM
guardrails` sobre `search/repositories`, ordenadas por ★: **el máximo de toda la
lista es 8 ★**. Lo que existe son componentes **genéricos de LLM** (detoxify
1.320 ★ Apache-2.0, Guardrails 7.635 ★ Apache-2.0, NeMo Guardrails 7.544 ★
Apache-2.0, LlamaFirewall MIT). **Ninguno es de gaming**: no manejan jerga de
partida, ofuscación `l33t`, voz, ni el perfil multi-idioma de LATAM y APAC.

Es un hueco **medido**, y coincide en el tiempo con la ola regulatoria de la
tendencia nº2. Ver `intel/market.md` y `compose/patterns.md` (Receta 34).

### 🟡 Corrección a esta KB: el *throttle* de aprobaciones de China aflojó

Esta KB describía el régimen de la NPPA como cuello de botella creciente.
Medición de este pass: **aprobaciones de juegos +19% interanual en 2026 vs 2025**,
leído como normalización regulatoria. El **lead time** del registro previo de
servicios de AI generativa ante la CAC **sigue vigente** y sigue entrando en el
cronograma; lo que cambia de signo es el indicador de aprobaciones. Se corrige.

---

## 🆕 Tendencias nuevas y correcciones — pass cycle 12 (2026-11-22)

### 🔴 Corrección central de este pass: **el caso de uso nº1 de AI en gaming no es el NPC, y esta KB lo venía ordenando al revés**

La encuesta de Unity 2026 parte la adopción por tarea: **código 62% · narrativa
44% · comportamiento de NPC 40% · market research 37% · playtesting automatizado
35%**. El NPC generativo está **22 puntos por debajo** del asistente de código.

Esto no es un matiz de ranking. Reordena el riesgo de todo el portafolio:

| Caso de uso | % devs | Art. 50(2) EU | Obligación de runtime NA | Rechazo del jugador |
|---|---|---|---|---|
| Asistencia de código | **62%** | ❌ no aplica | ❌ no aplica | ❌ invisible |
| Narrativa / escritura | 44% | 🟡 si el output se embarca | ❌ | 🟡 |
| **NPC** | 40% | 🔴 **sí** | 🔴 **sí** | 🔴 **alto** |
| Market research | 37% | ❌ | ❌ | ❌ |
| **Playtesting automatizado** | 35% | ❌ **no aplica** | ❌ **no aplica** | ❌ **invisible** |

> 🟢 **La fila que hay que vender primero es la última, no la del medio.** El
> playtesting automatizado es el único caso de uso **sin exposición regulatoria
> ni de cara al jugador**, y ya lo usa un tercio de la industria. El NPC
> generativo es el que arrastra las tres columnas rojas a la vez.

Y hay una **segunda brecha** en los mismos datos: GDC 2026 (2.300+ respondentes)
mide **36%** de uso de genAI, pero **30% en estudios vs 58% en editoras, soporte
y marketing/PR**. 🔴 **La adopción es más alta fuera del estudio que dentro** —
el comprador más probable no es el director técnico del estudio.

---

### 🆕 Tendencia 29: **el NPC se está yendo del datacenter a la GPU del jugador, y eso apaga obligaciones, no sólo costos**

NVIDIA sumó **soporte de Qwen3-8B** (open-weight) como SLM **on-device** en ACE,
además de su línea propia (Nemotron 3 Nano 4B; Nemotron 3.5 Bolt E4B sobre RTX
Spark en cobertura de oct-2026). El primer título embarcado con NPC ACE —
**Mecha BREAK** — corre un Nemotron-4 4B **en la máquina local**.

> 🔴 **Por qué esto es regulatorio y no sólo técnico:** esta KB documentó en el
> cycle 10 que la obligación que rompe el patrón *"el NPC llama a un LLM
> hosteado"* es de **topología de datos**, no de transparencia. Un SLM on-device
> **no tiene topología de datos**: la conversación del jugador no sale del
> equipo. **La arquitectura apaga la obligación en vez de documentarla.**
>
> Es, por lejos, la mitigación más barata que esta KB puede ofrecer hoy a un
> cliente con jugadores en EE.UU.: no es un control, es la ausencia del hecho
> que dispara el control.

⚠️ **Contrapeso honesto:** dependencia de hardware (la ruta ACE es NVIDIA), un
SLM de 4–8B **no es un modelo frontera** y el jugador con GPU vieja queda fuera o
cae a una ruta hosteada — con lo cual la obligación **vuelve para ese segmento**.
Hay que diseñar las dos rutas, no una.

---

### 🆕 Tendencia 30: **la licencia dejó de vivir en el repositorio**

Tres casos medidos este pass, y los tres rompen la sonda que esta KB perfeccionó
durante once passes:

| Caso | Lo que ve la sonda | Dónde está realmente el grant |
|---|---|---|
| `QwenLM/Qwen3` (27.758 ★) | badge `NONE`, 9×404, **enumeración = 0** | **`README.md` §*License Agreement*** → Apache-2.0, archivos en **Hugging Face** |
| `TaloDev/hosting` (36 ★) | **enumeración = 0** | **en ningún lado** — el producto es MIT, el runbook no está otorgado |
| `Yuki001/game-dev-skills` (123 ★) | **enumeración = 0**, README sin licencia | **en ningún lado** |

> 📐 **La regla que sale de los tres juntos:** *enumeración = 0* es **ambiguo**,
> y los tres resultados posibles son muy distintos entre sí — grant alojado
> afuera / grant inexistente / repo auxiliar que nadie licenció. **Resolverlo
> exige leer el README antes de escribir el veredicto, en los dos sentidos.**
> Escribir "sin licencia" sin ese paso descartó por poco un fundacional de
> 27.758 ★; escribir "MIT" por herencia de la organización habría otorgado un
> runbook que nadie otorgó.

---

### 🆕 Tendencia 31: **aparece una familia de licencia que segmenta por TIPO DE ENTREGABLE, no por uso ni por región**

`LumioGames/LGUI` llega con **PolyForm Shield 1.0.0 + Additional Permission**
(8.363 B) — familia nueva para esta KB. Permite uso comercial dentro de *"tus
propios juegos, aplicaciones, sitios y productos"* y prohíbe que el producto
resultante sea *"un UI framework, UI toolkit, UI editor, UI asset pipeline **o
game development tool**"*.

> 🔴 **El eje es nuevo.** Esta KB ya tenía licencias que segmentan por **uso**
> (no comercial), por **región** (los world models de Tencent excluyen UE/UK/
> Corea), por **rama**, por **subdirectorio** y por **variante de modelo**.
> Ésta segmenta por **qué tipo de cosa construís**: el **mismo repo** es verde
> en el juego de un cliente y rojo dentro de un acelerador reutilizable de
> Globant.
>
> ⚠️ **Es el eje más peligroso para una consultora**, porque el modelo de
> negocio de un estudio de servicios es precisamente **convertir el trabajo de
> cuenta en activo reutilizable**. Una licencia que permite lo primero y prohíbe
> lo segundo **se rompe en el momento exacto en que el proyecto sale bien**.

🟢 **Y la contratendencia que la acompaña:** la enumeración encontró adentro de
LGUI al padre **FairyGUI, MIT** (2.978 ★). El envoltorio restrictivo vendoriza
al original permisivo. **Cuando un grant condicional aparece, enumerar antes de
rechazar o de aceptar: el componente que se necesita puede estar adentro con
mejor licencia** — al precio de menos mantenimiento (FairyGUI-unity lleva ~9
meses sin push y su **editor no tiene grant**).

---

### 🆕 Tendencia 32: **un AAA japonés negando AI generativa en público convierte el "backend-only" en una oferta vendible**

**Capcom negó oficialmente** haber implementado AI generativa en el contenido de
sus juegos, y el patrón japonés que describe el barrido es **AI sólo en el
backend**. Combinado con el desglose de Unity (código 62% / NPC 40%), dos
fuentes independientes apuntan al mismo lugar: **el valor está donde el jugador
no mira**.

> 🟢 En Japón el freno **no es legal** — la *AI Promotion Act* no tiene
> penalidades — **es reputacional**. Por eso una garantía contractual de *"cero
> AI de cara al jugador"*, con gate de CI que la sostenga, es un entregable con
> comprador identificado, y no una limitación del alcance.

---

### 🔴 Corrección a esta KB: **el argumento LATAM necesita sus cuatro cifras, no dos**

Esta KB vendía LATAM con el **48,8%** que compraría juegos hechos con AI. El
estudio PGB Latam 2026 (**n = 7.202**) tiene dos cifras más que esta KB no
usaba: **25,7%** lo pensaría antes de comprar, y 🆕 **35,8% teme caída de calidad
y productos "sin alma"** (cero menciones en once passes).

> 🔴 **El 48,8% y el 35,8% no son bandos opuestos: son la misma población
> poniendo una condición.** La tendencia real de LATAM no es *"aceptación de
> AI"*, es **"aceptación condicionada a que no se note"**. Eso mueve el
> entregable de *contenido generativo visible* a **gate de calidad percibida**
> — y vuelve a coincidir, por tercera vez en este pass, con pipeline y QA.

---

### ⛔ Gaps de canal y de dato declarados este pass (no silenciados)

| Gap | Medición |
|---|---|
| `huggingface.co` | **No resuelve**: `curl` → 000, WebFetch → `ENOTFOUND`. Dos sondas. **Ninguna licencia de pesos fue verificada este pass.** |
| `gh api repos/<owner>/<repo>` | **HTTP 403** en esta sesión (scope cubre `search/*` y `raw`). ★/`archived`/`pushed_at` se obtuvieron vía `search/repositories`, que **sí** funciona |
| `curl -sI https://github.com/<slug>` | Devuelve `200 Connection Established` **del proxy**. Sigue sin servir como prueba de existencia — 4ª confirmación |
| Adopción de AI por estudios **de EMEA** | ⛔ **Sin cifra.** La afirmación del cycle 11 (*"Europa integra más profundo que NA"*) sigue apoyada en **una sola fuente** |
| Adopción / sentimiento de jugadores **de APAC** | ⛔ **Sin cifra** en este barrido |
| Adopción de AI por estudios **de NA** | ⛔ **Sin cifra.** El *State of AI in Gaming 2026* de **KPMG** apareció **sólo como fragmento** y **no se cita** |
| Adopción de AI por estudios **de LATAM** | ⛔ **Sin cifra.** Pista nombrada para el próximo pass: **Abragames**, gamescom latam |
| Títulos embarcados con NPC **ACE** | 🟡 **Sólo Mecha BREAK corroborado.** PUBG/inZOI degradados a *"Krafton leveraging AI"*; NARAKA y MIR5 **sin corroborar y no publicados** |

---

## 🆕 Tendencias nuevas y correcciones — pass cycle 11 (2026-11-22)

### 🆕 Tendencia 25: **el regulador europeo empezó a escribir excepciones técnicas, no sólo obligaciones**

🔵 Las guías finales del Art. 50 (adoptadas **5-nov-2026**, `C(2026) 7702 final`)
**amplían** las excepciones al marcado del Art. 50(2), y dos de las ampliaciones
están redactadas en términos de esta industria: **contenido efímero generado en
tiempo real en videojuegos y VR**, y **pipelines de producción en circuito
cerrado** donde sólo la salida final se marca.

**Por qué es una tendencia y no un detalle:** durante dos años el movimiento
regulatorio europeo fue de **ampliación de deberes**. Este es el primer
instrumento del período que **devuelve alcance** reconociendo un límite técnico
—y lo condiciona a demostrarlo—. ⇒ cambia la forma del entregable de
cumplimiento: de *"marcar todo"* a **"clasificar, demostrar infactibilidad donde
la haya, y dar disclosure de sesión"**. El trabajo no desaparece: **se vuelve
argumentativo y documental**, que es trabajo de consultoría, no de pipeline.

⚠️ **Riesgo simétrico:** una excepción condicionada es una excepción que alguien
tiene que **sostener por escrito**. Un cliente que la invoque sin memo es un
cliente peor parado que uno que marcó de más.

### 🆕 Tendencia 26: **Europa integra más profundo que Norteamérica, con el régimen regulatorio más duro encima**

Vivrato VAIA (20-oct-2026, n=101 líderes): **Europa 4,2/10** vs **Norteamérica
3,5/10** de profundidad de integración en pipeline; **52%** del total ≤ 3/10.

Esto **refuta el supuesto por defecto** de que la regulación europea frena la
adopción. Los dos hechos conviven: EMEA tiene el calendario legal más exigente
**y** la integración más profunda. La lectura más económica es que **la presión de
cumplimiento obligó a formalizar pipelines**, y un pipeline formalizado es un
pipeline donde la AI se puede embeber. ⚠️ Es una **inferencia**, no un hallazgo
medido: el informe da los dos números, no la causa.

**Consecuencia de pitch:** en EMEA no se vende adopción, se vende profundidad.
En NA no se vende adopción tampoco —ya ocurrió, y es ancha y fina— se vende
**integración**. En las dos regiones el discurso de *"empiecen con AI"* llega
tarde.

### 🆕 Tendencia 27: **la capa de infraestructura de servidor de juego está casi íntegramente en APAC y es permisiva**

De los cinco frameworks de game server incorporados este pass
([skynet](https://github.com/cloudwu/skynet) 14.199 ★ MIT,
[nano](https://github.com/lonng/nano) 3.228 ★ MIT,
[pitaya](https://github.com/topfreegames/pitaya) 2.847 ★ MIT,
[zfoo](https://github.com/zfoo-project/zfoo) 2.005 ★ Apache-2.0,
[pinus](https://github.com/node-pinus/pinus) 1.933 ★ MIT), **el centro de
gravedad de la categoría —incluido el cadáver más estrellado,
[pomelo](https://github.com/NetEase/pomelo) de NetEase— está en APAC**, y la
categoría entera es permisiva.

Contrasta con la capa de **world models**, donde el liderazgo de APAC (Tencent)
vino con **exclusión territorial de EU/UK/Corea** (cycle 10). ⇒ **la
infraestructura de APAC es abierta; los modelos de APAC no necesariamente.**
Distinguir las dos capas evita tanto el rechazo en bloque como la confianza en
bloque.

### 🔴 Corrección a la propia KB: **"Farama = permisivo" era una inferencia, y es falsa**

Esta KB lista cuatro repos de la Farama Foundation y los dos que tienen badge
limpio son MIT (Gymnasium, PettingZoo; Minigrid devuelve `NOASSERTION`). De ahí
se seguía —sin estar escrito— que la fundación es permisiva.
[Arcade-Learning-Environment](https://github.com/Farama-Foundation/Arcade-Learning-Environment)
es **GPL-2.0** y [ViZDoom](https://github.com/Farama-Foundation/ViZDoom) **no
expone grant legible** (27 sondas en 404, API `null`), los dos vivos. ⇒
**regla nº18: la licencia es propiedad del repo, nunca de la cuenta.**

Es la misma clase de error que la KB ya cometió con **la popularidad** (0ad,
pomelo, SerpentAI: muchas estrellas, proyecto muerto) y con **el badge**
(NOASSERTION leído como negativo). Tres veces el mismo patrón: **una señal barata
usada como sustituto de una medición.**

### 🆕 Tendencia 28: **el juego como interfaz de los agentes, no el agente dentro del juego**

[Agentshire](https://github.com/Agentshire/Agentshire) (MIT, 1.317 ★) visualiza
**agentes AI como NPCs 3D en un pueblo**: el juego no es el producto, es la
**consola de observabilidad** de una flota de agentes. Es la inversión de la
dirección que toda esta KB venía registrando.

**Por qué mirarlo aunque no sea gaming:** para un cliente **no gaming**, la
objeción recurrente ante un sistema multi-agente es *"no veo qué está pasando"*.
Esta categoría convierte competencia de game dev —que es lo que Globant tiene en
esta KB— en **una capacidad vendible fuera de la industria**. Señal temprana:
un repo, 7 meses sin push. Se registra como emergente, no como tendencia
confirmada.

---

## 🆕 Tendencias nuevas y correcciones — pass cycle 10 (2026-11-22)

### 🔴 Corrección obligatoria: el **Digital Fairness Act ya no es un borrador esperado**

Esta página lo listaba como *"Borrador previsto para Q4-2026"*. **Se publicó el
11-nov-2026 como COM(2026) 1100.** La fila corregida:

| Tendencia | Qué dice | Estado |
|---|---|---|
| 🔴 **Digital Fairness Act** | **Publicado 11-nov-2026, COM(2026) 1100.** Art. 15: recompensa variable de pago (loot boxes, ruletas, sobres) debe **publicar ítems y probabilidades**, ofrecer **desactivación accesible** y queda **prohibida para menores**. Arts. 14–17: el diseño adictivo (push no disparados por el usuario, rachas) debe poder **apagarse**. Multa hasta **6% de facturación global**. Los juegos online quedan exentos de las reglas de diseño adictivo *para menores*, que pasan al **EU KIDS Act** | 🔴 **Propuesta publicada.** Pendiente de Parlamento y Consejo; aplicaría **12 meses después de entrar en vigor** |

⚠️ **El matiz que la prensa está perdiendo:** la prohibición de loot boxes de
pago **es para menores**. No hay prohibición general para adultos.

### 🆕 Tendencia 21: **la obligación regulatoria dejó de ser sólo de transparencia y pasó a ser de topología de datos**

Hasta este pass, todo lo regulatorio que medía esta KB pedía **declarar** algo
(Art. 50 del AI Act, disclosure de Steam, ratings). La **COPPA enmendada**
(90 FR 16970, cumplimiento obligatorio desde el **22-abr-2026**) pide otra cosa:
**consentimiento parental verificable separado y desagrupado** antes de divulgar
datos de un menor a un tercero **para entrenar IA**.

**Por qué es un cambio de categoría.** Una obligación de transparencia se cumple
escribiendo. Ésta se cumple **cambiando por dónde viajan los bytes**: inferencia
on-device, opt-out contractual de entrenamiento, o un NPC que nunca transmita
texto libre. ⇒ **El patrón por defecto de la industria —NPC conversacional
contra un LLM hosteado— no es implementable tal cual en un juego infantil
estadounidense.** Ver Receta 26.

**Predicción falsable, para que el próximo pass la mida:** si esta tendencia es
real, en los próximos 2–3 passes deberían aparecer repos de NPC con **inferencia
local como opción de primera clase**, no como fallback. `undreamai/LLMUnity`, ya
en esta KB, es el candidato a observar.

### 🆕 Tendencia 22: **la licencia se volvió un eje geográfico, y no viene de ningún regulador**

Las **tres** releases abiertas de world models de Tencent
(`HY-World-2.0` 2.901 ★, `HY-WorldPlay` 1.630 ★, `Hunyuan-GameCraft-1.0` 765 ★)
declaran, en la primera línea de su archivo de licencia, que **no se aplican en
la Unión Europea, el Reino Unido ni Corea del Sur**. Verificado byte a byte en
este pass, los tres.

**≈5.300 ★ de world models abiertos que un estudio europeo no puede usar** — y
el badge de GitHub para los tres dice `NOASSERTION`, que no se parece en nada a
*"prohibido en tu jurisdicción"*.

> **Lo que esto anuncia, y es más grande que estos tres repos: el *open weights*
> chino está adoptando la licencia comunitaria con exclusión territorial como
> estándar de facto, y el territorio excluido es exactamente el bloque
> regulatorio más exigente.** No es casualidad: una licencia que excluye la UE
> es una forma de no entrar al perímetro del AI Act. ⇒ **Para esta KB, la
> pregunta "¿es permisiva?" dejó de ser suficiente. La pregunta completa es
> "¿es permisiva, y dónde?"**

**Predicción falsable:** si el patrón es estructural y no de un proveedor, los
próximos releases abiertos de world models de origen chino —de cualquier casa—
deberían traer la misma cláusula territorial. El próximo pass puede medirlo con
`search/code` + byte-read, que es barato.

### 🆕 Tendencia 23: **el "mercado" de herramientas de agente por engine consolida en Unity y se fragmenta en Godot**

Mismo problema técnico, dos estructuras de mercado opuestas, medidas el mismo día:

| Engine | Nº de repos MCP relevantes | Líder | ¿Consolidado? |
|---|---|---|---|
| **Unity** | 6+ | `CoplayDev/unity-mcp` — **16.104 ★**, MIT, push 2026-11-17 | ✅ **sí**: un orden de magnitud sobre el 2.º |
| **Godot** | **7** | `Coding-Solo/godot-mcp` — 6.732 ★ pero **7 meses sin commit** | ⛔ **no**: ninguno de los vivos pasó de 700 ★ |

**Seis de los siete repos de Godot nacieron entre dic-2025 y jun-2026.** La
lectura: Unity ya tiene ganador y conviene construir encima; Godot está en
disputa y conviene **no casarse** — y elegir por `pushed_at` + licencia, nunca
por estrellas, porque ahí la estrella apunta a un repo muerto.

### 🆕 Tendencia 24: **APAC es la única región donde la IA en gaming tiene argumento de ingresos**

En **H1-2026**, los títulos con IA fueron **~15% del top-100 móvil de China** y
se llevaron **42,6% de los ingresos** de ese top-100: **≈2,8× de
sobre-representación**.

| Región | Con qué se vende hoy la IA en gaming |
|---|---|
| **NA** | Riesgo (COPPA, FTC §5) y eficiencia interna. Sentimiento del jugador hostil |
| **EMEA** | Cumplimiento (AI Act Art. 50, DFA) |
| **APAC** | 🟢 **Ingresos** |
| **LATAM** | Aceptación del jugador y ventana regulatoria abierta |

⚠️ **Correlación, no causalidad:** los títulos con IA del top-100 chino son
desproporcionadamente los de los estudios grandes, que ya facturaban más. **Lo
que el dato prueba es que la IA no es un lastre comercial en ese mercado**, que
sigue siendo más de lo que puede probarse en cualquier otra región.

### 🟡 Corrección nº3: las tres cifras de Steam de esta KB son **tres denominadores**, no un desacuerdo

| Cifra | Ventana | Denominador |
|---|---|---|
| **49,9%** (cross-check SteamDB **49,7%** = 1.821/3.661) | **octubre 2026** | releases del mes |
| **40,1%** | ene–oct 2026 | releases del año |
| **~30%** | ene-2025 → oct-2026 | **9.251 juegos de pago** (excluye F2P) |
| 61,1% / 43% | 1–3 nov / 1–2 nov | **275 juegos: ruido, no usar** |

**Y dos matices que bajan el número real:** Valve **quitó en enero de 2026** la
obligación de declarar herramientas de desarrollo (agentes de código incluidos),
así que el 49,9% mide **assets que ve el jugador**, no uso interno; y el 49,9%
**incluye juegos cuyo único uso de IA fue traducir una cadena de texto**.

**Desglose de usos entre los que declaran:** imagen **63%**, testing/traducción
30%, store page 26%, música 21%, programación 16%, **voice acting 7%**.

> **El 7% de voice acting ordena toda la conversación de riesgo reputacional: es
> el uso que más indigna y el menos frecuente.** Un cliente que teme el backlash
> está temiendo una práctica que hace uno de cada catorce juegos que declaran.

### ⛔ Gap de canal declarado, re-verificado este pass

`WebFetch` **no resuelve ningún host general en esta sesión**
(`getaddrinfo ENOTFOUND`). ⇒ **Ninguna afirmación regulatoria de esta página
está verificada contra fuente primaria**: ni COM(2026) 1100, ni 90 FR 16970, ni
el informe de GDC. Todo lo regulatorio viene de **resúmenes de búsqueda web
sobre fuentes secundarias** y se marca como tal. Los canales que sí funcionan
—`gh api search/repositories`, `gh api search/code`, `raw.githubusercontent.com`—
**sólo verifican repos**, y es ahí donde esta KB puede afirmar sin asteriscos.

---
## Tendencias confirmadas (alta confianza)

### 1. 🔴 El sentimiento del desarrollador se dio vuelta, y eso cambia cómo se vende
Es el cambio más importante de los últimos cuatro meses y no estaba en este archivo.

- GDC 2026 (2.300+ respondentes): **52% de los devs dice que la AI generativa está
  dañando la industria**; sólo **7%** ve impacto positivo.
- Uso declarado: **36%** de los profesionales, con **30%** entre devs de estudio
  contra **58%** en publishing/marketing/negocio. Una fuente registra caída
  interanual **36% → 29%**.
- Al mismo tiempo, el Google Cloud Games Report 2025 reporta **97%** de devs usando
  AI para assets, playtesting y localización.

**Lectura:** la capacidad no bajó; bajó la disposición a declararla. El pass de
julio de este KB abría con "87% de estudios usa AI agents" como señal de demanda
caliente. Esa frase, dicha en una sala donde la mitad de los devs piensa que la AI
daña su oficio, cierra la puerta. **El ángulo que sobrevive es AI en el pipeline
interno** (QA, build, localización, ops), no en el contenido que ve el jugador.

### 2. 🔴 La transparencia pasó de ser tema de ética a ser tema de fecha
Tres relojes corriendo a la vez, con distinta naturaleza:

| Jurisdicción | Obligación | Fecha | Naturaleza |
|--------------|-----------|-------|-----------|
| **UE** | Art. 50(1): disclosure al jugador que interactúa con AI | **vigente desde 2-ago-2026** | Ley |
| **UE** | Art. 50(2): marcado **machine-readable** en contenido generado, sistemas ya en mercado | 🔴 **2-dic-2026** | Ley — ~5 semanas |
| **UE** | Anexo III / alto riesgo | **2-dic-2027** (movido desde 2-ago-2027) | Ley |
| **Corea** | Notificación previa + etiquetado de salida (AI Basic Act) | **vigente desde ene-2026** | Ley, gracia ≥1 año |
| **China** | Etiquetado de contenido, reglas por tipo de servicio | vigente | Ley |
| **Japón** | Divulgación de datos de entrenamiento | propuesta **voluntaria** | De facto, por nombramiento público |
| **Steam (global)** | Disclosure de contenido AI entregado al jugador | **política reescrita 17-ene-2026** | Contrato de tienda |

El dato que une todo: **las tiendas llegaron antes que los reguladores**, y
contractualmente pegan más rápido. Cumplir Steam no cumple la UE, pero incumplir
Steam saca el juego de la tienda esta semana.

### 3. 🔴 El etiquetado en Steam llegó al 50% — un año antes de lo proyectado
**Actualizado 2026-11-20 y es el cambio cuantitativo más grande del pass.**
- Serie con censo ampliado (~63.600 releases; el de octubre era ~53.600):
  **13,2% (2024) → 24% (2025) → 40,1% (2026 hasta octubre)**, y **49,9% en el mes
  de octubre-2026** solo. Contra-conteo independiente sobre datos de SteamDB:
  **1.821 de 3.661 = 49,7%**. Dos métodos, misma conclusión.
- 🔴 **La proyección ">50% para 2027-2028" que este archivo citaba hace 25 días ya
  se cumplió.** Octubre-2025 estaba en 26,4%: el ritmo casi se duplicó en un año.
  **Consecuencia para el pitch:** etiquetar dejó de ser diferencial o señal de
  riesgo reputacional — es el default del catálogo. Lo diferencial ahora es *qué*
  se etiqueta.
- 🟢 **El desglose es la parte vendible, y desmiente el relato:** **63% de las
  declaraciones es artwork**, **30% texto y traducciones**, **8% sólo página de
  tienda**, y **apenas 3% declara generación durante el gameplay**. El mercado
  etiqueta *arte y texto de producción*; la AI en runtime es un nicho de 3%. Quien
  venda "NPCs con AI" no está entrando en una ola del 50%, está entrando en el 3%.
- 🟢 **Y es inversamente proporcional al tamaño:** **47%** de los títulos con <10
  reviews declaran AI, **27%** de los de 100-999, y sólo **14%** de los de 1.000+.
  Los estudios grandes declaran menos: o usan menos, o tienen más que perder.
- ⚠️ Una declaración **no** es una medida de uso, y la ausencia de etiqueta no
  prueba ausencia de AI. El 49,9% cuenta declaraciones.
- 🟢 **La reescritura del 17-ene-2026 exime explícitamente las herramientas de
  desarrollo** (GitHub Copilot citado por nombre) y sólo obliga sobre el contenido
  AI que llega al jugador. Hay por tanto un espacio limpio y grande para AI interna.
- ⚠️ El ~36% que circula para Next Fest **no es cifra de Valve**: es conteo de
  terceros (2.593 de 7.145). Valve nunca publicó disclosure por edición.
- Epic (Tim Sweeney) atacó la política como "abhorrent", argumentando que las
  etiquetas funcionan como declaración política. La industria **no** tiene consenso.

### 4. 🔴 MCP ganó en gaming — pero este KB tenía mal quién ganó
La tendencia se confirma; **el ranking estaba equivocado y se corrige aquí.**

| Repo | Licencia | ★ 2026-11-20 | `pushed_at` | Nota |
|------|----------|--------------|-------------|------|
| 🆕 [CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp) | **MIT** | **16.087** | 2026-11-17 | **El líder real, 3,4× el nº2.** ⚠️ rama default `beta` |
| 🆕 [Coding-Solo/godot-mcp](https://github.com/Coding-Solo/godot-mcp) | MIT | **6.713** | ⚠️ 2026-04-16 | 15× el godot-mcp que listábamos, **7 meses parado** |
| [IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP) | Apache-2.0 | 4.672 | 2026-11-19 | Nº2, no nº1. Opción si se exige Apache-2.0 |
| [hi-godot/godot-ai](https://github.com/hi-godot/godot-ai) | MIT | 3.627 | **hoy** | 46 tools, 120+ ops. El Godot MCP *mantenido* |
| 🆕 [CoderGamester/mcp-unity](https://github.com/CoderGamester/mcp-unity) | MIT | 2.018 | 2026-10-11 | Tercera opción Unity |
| [prajwalshettydev/UnrealGenAISupport](https://github.com/prajwalshettydev/UnrealGenAISupport) | MIT | 686 | 2026-04-28 | El único Unreal con grant limpio y volumen |
| [tomyud1/godot-mcp](https://github.com/tomyud1/godot-mcp) | MIT | 455 | 2026-10-17 | Lo que llamábamos "segunda opción real" |
| [IvanMurzak/Unreal-MCP](https://github.com/IvanMurzak/Unreal-MCP) | Apache-2.0 | 64 | 2026-11-19 | Temprano |
| [IvanMurzak/GameDev-MCP-Server](https://github.com/IvanMurzak/GameDev-MCP-Server) | Apache-2.0 | 21 | 2026-11-19 | Backend compartido, adopción mínima |

**Qué se sostiene:** la adopción está en el **plugin de engine**, no en la
abstracción compartida — la asimetría 16.087 / 64 / 21 lo dice más fuerte que
antes. Y `hi-godot/godot-ai` pasó de 805 (julio) a 3.627.

🔴 **Qué estaba mal y por qué, porque la causa es generalizable.** Este archivo
decía que `IvanMurzak/Unity-MCP` era el dominante. El líder es
`CoplayDev/unity-mcp`, y era **invisible** porque el repo **fue renombrado**
(`justinpbarnett/unity-mcp` → `CoplayDev/unity-mcp`): el slug viejo sigue indexado
en blogs y búsquedas, ya no resuelve en la API, y **el nº2 heredó el título de
"dominante" sin que nadie comparara**. Mismo mecanismo en Godot
(`Coding-Solo/godot-mcp`, 6.713★, nunca visto por esta KB).

> **Modo de fallo nombrado para la rotación: *el ascenso del subcampeón*.**
> Inverso del *name-collision cluster* que documentó la KB de enterprise: allí los
> resultados de búsqueda eran repos falsos; **aquí son todos reales** y el error
> está en cuál se corona. Discriminadores: (1) comparar **≥3 candidatos por ★**
> antes de escribir "dominante"; (2) si el nº1 esperado de una categoría no
> aparece, **sospechar rename** y probar `raw` del slug viejo — un 200 ahí con
> `NOT_FOUND` en la API es la firma de un redirect; (3) pedir siempre `pushed_at`
> junto a ★, porque el líder de Godot está **parado hace 7 meses** y eso cambia la
> recomendación aunque no cambie el ranking.

Se reporta además un plugin MCP first-party experimental en UE 5.8 — si Epic lo
estabiliza, la tier community de Unreal se consolida de golpe.

### 5. 🟢 Los world models jugables se volvieron ejecutables en hardware de consumidor
Pasó de categoría "a observar" en julio a entregable real.

- **Matrix-Game 4.0** (Skywork AI, **15-oct-2026**): arquitectura híbrida — el
  **engine ejecutable** mantiene lógica, física e input, y un **modelo causal de
  video** convierte frames low-poly en visuales. **67 FPS en RTX 5080** y 58 FPS en
  5070 Ti a 864×480 con SageAttention. Publicados inferencia, demo en browser y el
  código de tres juegos (Medieval Castle, Ancient Egypt, City Racing).
- **MineWorld** (Microsoft, MIT): Minecraft en tiempo real, transformer
  autoregresivo visual-acción, checkpoints 300M/700M/1200M, 4–7 fps con Diagonal
  Decoding.
- 🔴 **La línea Genie de DeepMind no es abierta**: reports y blog posts, sin código.
  Todo lo jugable abierto es de **Skywork, Microsoft o Tencent**.

**Lo que esto cambia en la práctica:** la arquitectura ganadora **no** reemplaza el
engine. Lo mantiene y le pone rendering neural encima. Eso hace el patrón adoptable
de forma incremental, que es la diferencia entre una demo y un roadmap.

### 6. 🟢 Ya existe un benchmark para agentes que juegan
En julio este KB recomendaba QA con RL pero no tenía con qué medirlo.

- [lmgame-org/GamingAgent](https://github.com/lmgame-org/GamingAgent) — **MIT, 998 ★,
  ICLR 2026**: LMGame Bench + harness de gaming + leaderboard público, y despliegue
  de computer-use agents sobre PC.
- Alrededor hay un ecosistema de evaluación naciente: **CUA-bench** (agentes jugando
  seis juegos comerciales con teclado y mouse), **RuneScape-Bench**,
  **GameLogicBench** (aserciones de estado a nivel de tick), **MineExplorer**.
- Roblox **OpenGameEval** (dic-2025, 47 escenarios) sigue siendo la referencia del
  lado plataforma.

### 7. El stack Behavior Tree + LLM sigue siendo el patrón de NPC que aguanta
Sin cambios estructurales desde julio, lo cual ya es información: no apareció nada
que lo desplace.
- **LimboAI** (MIT, **3.1k ★**, era 2.8k): BT + HSM.
- **Beehave** (MIT, 3.2k ★): BT componibles en el scene tree.
- **LLMUnity** (Apache-2.0, 1.7k ★) del lado Unity.
- **generative_agents** de Stanford (Apache-2.0, **22.399★**) sigue siendo la
  arquitectura de referencia: memory stream + reflection + planning. 🔴 **Matiz que
  faltaba: `pushed_at` 2024-08-05, 27 meses sin commits.** Es verdad del *patrón*
  y engañoso del *código*: artefacto de paper congelado, no base mantenida.
- 🆕 **La contraparte viva:** [a16z-infra/ai-town](https://github.com/a16z-infra/ai-town)
  (**MIT, 10.835★, commit de hoy**), starter kit desplegable de un pueblo de
  agentes. **Faltaba desde el primer pass de esta KB.** Si `generative_agents` es
  el destino arquitectónico, éste es el vehículo que se forkea.
- **Beehave** (MIT, 3.329★) ⚠️ rama default **`godot-4.x`**, ni `main` ni `master`.
- Patrón: **BT como esqueleto de control determinista, LLM como módulo de diálogo.**
  La razón ahora es también regulatoria: si el Anexo III llega a cubrir sistemas
  adaptativos, tener la decisión fuera del LLM es lo que mantiene el sistema
  auditable.

### 8. Nakama sigue siendo el backend por defecto, con una alternativa MIT nueva
- **Nakama**: Apache-2.0, **13.6k ★** (era 12.8k). Hooks en Go/TS/Lua: ahí entra la AI.
- 🆕 **Talo** ([TaloDev/backend](https://github.com/TaloDev/backend), **MIT**, 104 ★):
  auth, stats, leaderboards, saves, event tracking, canales realtime, live config,
  presencia, relaciones; integra Steamworks, Google Play Games y Apple Game Center.
  Paquetes Godot y Unity. Más permisivo que Nakama y de alcance menor.
- 🆕 **Pterodactyl** (MIT): la capa de **ops** que faltaba. Un backend de juego no se
  opera a mano.
- 🆕 **Agones** ([agones-dev/agones](https://github.com/agones-dev/agones),
  Apache-2.0, **7.106★**): orquestación y autoescalado de game servers sobre
  **Kubernetes**. **Faltaba desde el primer pass** — Pterodactyl opera hosts,
  Agones da capacidad elástica; no se reemplazan. 🔴 Y está **renombrado**:
  `googleforgames/agones` ya no es el slug canónico.
- 🟢 **Corrección: Elements no era AGPL-3.0, es MPL-2.0** — copyleft **por
  archivo**, hostearlo no obliga a liberar tu código. Este archivo lo listaba como
  "trampa de licencia" y **el descarte era erróneo**; sigue siendo chico (22★) pero
  no estaba descalificado. Nuevo slug: `NamazuStudios/elements`.
- 🔴 **Pelican** (fork de Pterodactyl) pasó de "sin archivo de licencia" a
  **inexistente**: `repo:` → `NOT_FOUND`, `raw` → 404.
- 🔴 **Nakama 13.757★, Talo 105★** (rama default `develop`), **Pterodactyl 9.533★**
  (rama default `1.0-develop`).

### 9. 🔴 El riesgo de licencia es sistemático, no anecdótico
En un solo pass, leyendo grants primera mano, aparecieron:
- Un **badge MIT sin ningún archivo de licencia** en el repo (`chongdashu/unreal-mcp`).
- Una fila que este KB publicaba como **MIT** y es **CC-BY-4.0** (`local-llm-npc`).
- **Grafana** publicada aquí como Apache-2.0 cuando es **AGPL-3.0**.
- Un world model cuyo **grant cambia según el subdirectorio**: Matrix-Game es MIT en
  la raíz y **Apache-2.0** en `Matrix-Game-4/`, con el sidebar de GitHub mostrando
  sólo MIT.
- Cuatro engines/juegos con **archivos de licencia agregados** donde el código y los
  assets tienen grants distintos (LÖVE, 0 A.D., SuperTuxKart, Luanti).
- Un repo **vivo, vacío y sin licencia** que estaba en el radar de este KB
  (`SKYHUBDev/Aivill`).

**Añadido 2026-11-20 — y ahora el riesgo se mide dentro de este propio KB:**
- **MonoGame** publicado aquí como MIT es **Ms-PL** (§3(D) obliga a que la
  distribución *en fuente* vaya bajo Ms-PL: no relicenciable a MIT/Apache).
- **PostHog** publicado aquí como MIT es **MIT Expat + carve-out `ee/`** — el mismo
  texto, palabra por palabra, que el carve-out de Onyx en la KB de enterprise.
  **El patrón "MIT Expat + `ee/`" es una plantilla de la industria OSS-comercial**,
  no una rareza: hay que buscarla activamente.
- **O3DE** publicado aquí como Apache-2.0 es **Apache-2.0 *o* MIT a elección**
  (más permisivo de lo que decíamos) **pero arrastra Qt bajo LGPL-3.0**, que no
  mencionábamos al venderlo como "enterprise sin royalties".
- **Elements** fue **descartado por una licencia que no tiene** (MPL-2.0, no
  AGPL-3.0). Un descarte erróneo es peor que una recomendación floja: saca al
  candidato del campo de visión y nadie vuelve a mirarlo.
- **0 A.D.** sigue recomendado como base de fork y está **`archived`** desde
  ago-2024 (el proyecto migró a Gitea propio).

🟢 **Y el hallazgo de método que lo hace sistemático: el *checksum de bytes*.**
Este KB venía anotando los bytes de cada archivo de licencia sin usarlos. Los
textos de licencia tienen longitud canónica (MIT ~1.100 B, Ms-PL ~3.900,
Apache-2.0 ~11.350, MPL-2.0 ~16.700, CC-BY-4.0 ~18.650, LGPL-2.1 ~26.400,
AGPL-3.0 ~34.500, GPL-3.0 ~35.200), así que **una fila cuyos bytes no caen en la
banda de su familia está mal clasificada**. Aplicado a las filas ya escritas, sin
una sola llamada de red nueva: **marcó 4 filas y las 4 estaban mal. Cero falsas
alarmas.** El caso de `elements` es la lección entera: decía "AGPL-3.0, 16726 B"
mientras este mismo KB anotaba "AGPL-3.0, 34523 B" para Grafana — **el dato que
refutaba la etiqueta estaba escrito al lado de la etiqueta.**

**La conclusión operativa:** en gaming, un badge o una tabla heredada no es
evidencia — **ni siquiera una tabla propia verificada el mes pasado**. El método
(dos canales + checksum de bytes) está documentado en `agents/top.md` y es
vendible como entregable propio.

### 10. 🆕 🟢 La demanda se movió del agente-en-el-editor al pipeline-de-estudio
Categoría que no existía en esta KB hace 25 días, y en estrellas ya es más grande
que toda la tier MCP.

- [Donchitos/Claude-Code-Game-Studios](https://github.com/Donchitos/Claude-Code-Game-Studios)
  — **MIT, 40.377★**: 49 agentes con rol + 72 skills de workflow. **Repo nº1 de
  game-development en el trending de GitHub** a principios de nov-2026, con
  trayectoria medida 34,7k (3-nov) → 40,2k → 40.377 (hoy).
- [EL4CTEO/blender-skills](https://github.com/EL4CTEO/blender-skills) — MIT, 2.529★:
  workflows de Blender **como skills de agente**. Cubre arte 3D, que el tooling de
  engine no toca.
- [WhiteTowerAI/ohmygame](https://github.com/WhiteTowerAI/ohmygame) — Apache-2.0,
  **2.486★, +107% en 25 días**: el mayor crecimiento relativo del pass.
- [genex-games/genex-desktop](https://github.com/genex-games/genex-desktop) — MIT,
  900★, multi-engine (Unreal/Unity/Godot/browser). ⚠️ rama default `dev`.

**La lectura, con su contrapeso.** 40.377★ en un repo de **prompts y definiciones
de agente** —sin engine, sin runtime, sin modelo— contra 16.087★ del plugin de
editor más usado. Las ★ miden **interés**, no producción, y un repo de prompts es
barato de marcar: no es evidencia de adopción en estudios. Pero el orden de
magnitud indica qué está pidiendo el mercado — **un pipeline con roles**, no un
agente en el editor — y eso es exactamente la forma de un engagement de servicios.

### 11. 🆕 🟢 Los NPCs con AI ya escalaron en producción, y el dato es de APAC
La objeción recurrente ("los NPC con LLM no son viables en producción") tiene ahora
una refutación con número.

- **Tencent, *Game for Peace*: 110 millones de usuarios acumulados** de features de
  NPC con AI y **pico de 17,7 millones de DAU**. Es, por dos órdenes de magnitud,
  el despliegue más grande del que esta KB tenga medición.
- **NetEase, *Justice Mobile*:** personaje con AI movido por **DeepSeek**, que
  conversa y reacciona fuera del diálogo escrito. 🔴 Y circuló que NetEase había
  **prohibido** la gen AI a sus devs: **lo desmintió**; no repetir el rumor.
- **Tencent "LAP":** herramienta interna de producción con AI para assets de fondo
  y personaje y generación de animación.
- ⚠️ **Límite del dato:** Tencent no desglosa cuánto del NPC es LLM y cuánto es
  sistema clásico con capa generativa. Sirve para matar la objeción de
  escalabilidad; **no** para afirmar "110M de usuarios de LLM en runtime".
- **Contraste que conviene llevar junto:** sólo **3%** de las declaraciones de AI
  en Steam son generación durante el gameplay (tendencia 3). La AI en runtime es
  masiva en APAC mobile y marginal en el catálogo occidental de PC.

### 12. 🆕 🔴 La AI como estrategia corporativa anunciada junto al despido
Riesgo reputacional del pass convertido en titular, con fecha.

- **Ubisoft, 19-nov-2026:** **166 puestos** eliminados (Montreal y Quebec City),
  conocidos el mismo día del reporte semestral, y **1.100 en seis meses**. El mismo
  día lanzó **Motherbrain**, subsidiaria dedicada a gen AI para *gameplay* — NPCs
  con memoria y "AI game masters" para mundos sistémicos, con el equipo del
  proyecto interno *Teammates* escindido ahí. **Nada ha salido: son planes.**
- **GDC, nov-2026: 54% de los despedidos sigue sin empleo** (era 48% en enero), y
  **sin evidencia de que la AI cause los despidos**.
- **Lectura para la sala:** la correlación es de calendario, no causal, pero el
  cliente —y su equipo— la va a escuchar como causal. Es el refuerzo empírico de
  la tendencia 1: el ángulo que sobrevive es **AI en pipeline interno y
  upskilling**, y conviene llevar el dato de GDC para desactivar la objeción sin
  negarla.

---

### 13. 🆕 🔴 La adopción de gen AI se partió por región, y la encuesta que todos citan no puede verla

La cifra que circula —**40% de los profesionales usa gen AI**, GDC *State of the
Game Industry*, publicada el **12-nov-2026**— esconde el hallazgo:

| Corte | 2025 → 2026 |
|-------|-------------|
| **EE.UU.** | 36% → **38%** (GDC lo declara dentro del margen de error) |
| **Fuera de EE.UU.** | 37% → **44%** (+7 pts) |
| **Japón solo** (CESA 2026, 1.349 respuestas) | — → **85,8%**, con **63% de uso diario** |

Y el detalle de instrumento que lo cierra: **la muestra de GDC es 63%
estadounidense, contra 54% en la edición anterior**, con **3% de Japón**. ⇒ **la
encuesta se volvió más estadounidense exactamente mientras lo que se movía era el
resto del mundo.** GDC lo reconoce a medias: atribuye el alza no-estadounidense a
compañías de free-to-play y móvil y advierte que el tamaño de muestra limita la
conclusión.

**Qué hacer con esto, concretamente:** dejar de usar una cifra de adopción
"global". Hay dos mercados: uno donde la adopción está estancada en ~38% y **55%
de la industria dice que la gen AI le hace daño**, y otro donde está en 85,8% con
63% de uso diario. **El mismo pitch no sirve para los dos**, y hasta este pass
esta KB tenía escrito sólo el primero. Ver `intel/market.md` → *un número sin su
muestra no es un número*.

### 14. 🆕 🔴 La transparencia se mueve del cartel al *runtime* (y eso es ingeniería, no legal)

Tres obligaciones distintas, tres lugares distintos donde se cumplen — y la
tercera es nueva en esta KB:

| Obligación | Dónde se cumple | Costo de implementación |
|------------|-----------------|------------------------|
| Ficha de AI en Steam | **Fuera del juego**, en la store | Un formulario |
| Art. 50(2) EU AI Act (marcado de salidas) | **En el build**: metadata/C2PA en los assets | Pipeline de firma |
| 🆕 **Leyes estatales de *companion chatbots* (NA)** | **Dentro de la sesión de juego** | 🔴 **Reloj de sesión + flag de minoría + canal de aviso + log** |

Connecticut (Public Act 26-15) pide recordar que se habla con una AI **≥1×/hora
a menores y ≥1×/3 horas a adultos**, con protocolo de crisis y derivación al
**988**; Washington (HB 2225, vigente **1-ene-2027**) pide recordatorios
periódicos con intervalo más corto para menores **y trae acción privada**; Idaho
(S 1297) opera desde **1-jul-2027**.

🟡 **Y la pregunta que nadie respondió todavía:** si un **NPC** cuenta como
*companion chatbot*. El *Harvard JSEL* (ago-2026) la plantea y la deja abierta.
⇒ **la tendencia no es "hay que cumplir": es que el costo de prepararse es de
días y el de equivocarse es el proyecto.** Receta 13.

### 15. 🆕 🟡 Los instrumentos de medición de AI en gaming cambiaron bajo nuestros pies

No es una tendencia de mercado; es una de **datos**, y envenena las otras si no
se dice:

1. **Valve reescribió el formulario de declaración de AI el 17-ene-2026**,
   restringiéndolo a contenido AI **que el jugador ve u oye** y sacando las
   herramientas de desarrollo. ⇒ el **49,9% de octubre-2026 mide menos cosas**
   que el 26,4% de octubre-2025: **la serie tiene un corte** y la subida real es
   mayor que la aparente.
2. **Dos censos, dos números**: ~49,9% (censo de releases de octubre) vs
   **30,8%** (censo de Sulka Haro sobre 53.597 releases, primeros 7 meses de
   2026). No se contradicen: **miden ventanas y universos distintos**.
3. **CESA cambió la unidad**: 51% (2025) era sobre **54 empresas**; 85,8% (2026)
   es sobre **1.349 personas**.

🔴 **Dato nuevo con filo comercial, del mismo censo de Steam:** los juegos que
**declaran** AI promedian **77% de reviews positivas** contra **83%** de los que
no declaran. Pero **los devs con ≥10 lanzamientos en 2026 declaran en 68% de sus
juegos, contra 37% de los devs de un solo lanzamiento**. ⇒ la penalización de 6
puntos **puede estar midiendo volumen y no AI**: quien publica en masa declara
mucho más. Sin control por género, precio ni presupuesto, **la lectura honesta
es "hay correlación, la causa no está establecida"** — y sirve igual, porque el
cliente la va a encontrar y conviene que la oiga bien contada primero.

### 16. 🆕 🔴 El copyleft se mudó hacia arriba del stack: del engine al **pack de skills**

Durante tres passes esta KB midió riesgo de licencia en **engines, backends y
librerías** — la capa donde el software se compila y se enlaza. El riesgo se
movió a la capa de arriba, que es **markdown y prompts**, y nadie la audita.

Medido sobre los packs de agentes/skills de gaming **nuevos** con más de 400★:

| Grant | Repos | Ejemplos |
|-------|-------|----------|
| 🟢 MIT / Apache-2.0 | 3 | `awesome-gamedev-agent-skills` (Apache-2.0 + `NOTICE`, 9.929★), `coop-anything` (MIT, 2.185★), `bot-crossing` (MIT, 906★) |
| 🔴 **Copyleft** | **3** | `Game-Dev-Agentic-Skills` (**LGPL-3.0**, 983★), `taOS` (**AGPL-3.0**, 609★), `guizang-game-skill` (**AGPL-3.0**, 431★) |

**Por qué es más peligroso que el mismo reparto en una librería.** Una
dependencia copyleft entra por el gestor de paquetes, aparece en el lockfile y
cualquier gate de CI la ve. **Un pack de skills entra copiando un directorio de
`.md` al repo del cliente** — no hay lockfile, no hay manifest, no hay import, y
el gate de licencias de la Receta 15 **miraba dependencias de build**. Un archivo
de instrucciones de agente bajo AGPL-3.0 pegado en un entregable arrastra la
obligación de red **sobre lo que lo ejecute**, y la ruta de contaminación es un
`cp -r`.

> **Dicho con honestidad sobre el límite de la afirmación:** que un prompt en
> markdown sea "software" a efectos de la AGPL-3.0 **no está resuelto en
> tribunales** y esta KB no va a afirmar que lo esté. El punto operativo no
> depende de eso: **un cliente corporativo con un gate de licencias va a
> rechazar un archivo AGPL-3.0 en su repo sin esperar jurisprudencia.** El riesgo
> es de aceptación del entregable, y ése es seguro.

**Consecuencia:** el gate de licencias tiene que cubrir `*.md` y los directorios
de skills, no sólo el árbol de dependencias. Ver **Receta 16**.

### 17. 🆕 🟢 La licencia de los **pesos** dejó de ser incognoscible, y la vía no era llegar a Hugging Face

El gap más viejo de esta KB (declarado cuatro passes seguidos: *"ninguna licencia
de pesos ha sido verificada nunca"*) se cerró en su primer caso sin acceder a
Hugging Face — que, además, **nunca va a estar accesible**: lo que esta KB
registraba como *"no resuelve DNS"* es en realidad un **403 en el CONNECT del
proxy de egreso**, es decir una **denegación de política**, no un fallo
transitorio.

**La vía que funciona:** un modelo *continued-pretrained* o *fine-tuned* **no
puede ser más permisivo que su backbone**, y el backbone está nombrado en el
README del repo de código. `THUSI-Lab/GameScaling` lo dice textual: *"The models
are continued pre-trained from **Qwen3.5**, which is also **Apache 2.0**"*.
⇒ **pesos Apache-2.0, verificado, sin salir de `raw.githubusercontent.com`.**

**Y el caso que no se cierra, ahora se cierra mejor de lo que estaba.**
`Matrix-Game-4/THIRD_PARTY_NOTICES.md` declara: *"**Model and asset
distributions carry their own accompanying license statements**"* y *"**Base-model
and codec weight licenses accompany their upstream distributions**"*. Pasamos de
*"no sabemos qué licencia tienen los pesos"* a *"**el publicador declara que los
pesos no están cubiertos por el Apache-2.0 del repo**"*. En una propuesta, eso es
la diferencia entre un riesgo desconocido y **un ítem con dueño**: hay que
pedirle el grant de los pesos al proveedor, y ahora se sabe que existe.

**Tendencia de fondo, no sólo de método:** los publicadores de world models
**están separando explícitamente código, pesos y assets en tres grants
distintos**. Tres de tres casos de esta KB lo hacen (Matrix-Game, HY-World con
exclusión territorial, GameScaling con 8 repos de checkpoints aparte). ⇒
**"el modelo es open source" dejó de ser una afirmación con significado.** Hay
que preguntar *qué parte*.

### 18. 🆕 🔴 La infraestructura de gaming se está revendiendo como infraestructura de agentes — y el nicho pierde proveedores

El *Open Game Backend* (`rivet-gg/opengb`) era, en la prensa de backends, la
alternativa Apache-2.0 a Nakama. Trazado hoy: su README dice *"Moved to
rivet-gg/toolchain"*, ese slug está **archivado** y apunta a `rivet-gg/rivet`, que a su vez es un
redirect de organización; el canónico es [rivet-dev/rivet](https://github.com/rivet-dev/rivet).

**Lo que hay al llegar ya no es un backend de juego.** Es *"the orchestrator for
agentic workloads"*, con *"4,99 ms p99 cold starts, 72 KB RAM per Actor"* y
*"create one for each **agent**, user, session, or tenant"* — 6.970★, commit de
hoy. La misma org publica `agentos` (4.912★, sandboxes WASM) y `agents` (1.652★).

**La empresa que construyó la orquestación de game servers abandonó gaming y
vendió la misma tecnología como infraestructura de agentes AI.** Tiene lógica
técnica: un actor con estado en memoria, ciclo de vida largo y sesión por
usuario **es** un game server, y resulta que también es un agente.

Es el **tercer** movimiento del mismo tipo que esta KB registra en 48 horas:

| Repo / empresa | Nicho de origen | Dónde está ahora | ★ |
|----------------|-----------------|------------------|---|
| `rivet-dev/rivet` | **Open Game Backend** | orquestador de workloads agénticos | 6.970 |
| `msitarzewski/agency-agents` | — | catálogo genérico con gaming como **1 de 18** divisiones | 169.646 |
| `Donchitos/Claude-Code-Game-Studios` | game dev | el nº1 de la categoría, **12 días sin commits** | 40.486 |

> **Lectura para AI Studios, y es la incómoda:** el mercado está **drenando
> especificidad de gaming hacia arriba**. Los repos que crecen son los
> horizontales; el específico de gaming más grande se quedó quieto; y un
> proveedor de infra de gaming se reposicionó fuera del nicho. **El diferencial
> defendible no es el componente —se está volviendo genérico— es el
> conocimiento del dominio: el engine del cliente, su pipeline de arte, su
> compliance por región y su audiencia por edad.** Es exactamente lo que ninguno
> de esos tres repos tiene y lo que esta KB sí está acumulando.
>
> **Consecuencia operativa inmediata:** la alternativa Apache-2.0 a Nakama en
> orquestación es **`agones-dev/agones`**, no OpenGB. OpenGB no está
> desmantenido: **dejó de existir como producto de gaming.**

### 19. 🆕 🔴 El riesgo de licencia dejó de ser copyleft y pasó a ser **ausencia de licencia**, y entra por el pack permisivo

La tendencia 16 de este archivo (el copyleft subiendo al pack de skills) describe
un riesgo **visible**: un `LICENSE` que dice LGPL o AGPL, que cualquiera puede
leer. Este pass encuentra la variante que **ningún gate detecta**, y en el pack
de skills de gaming **más estrellado que existe**:

```
htdt/godogen          MIT, 7.378 ★   ← lo que el cliente instala y lo que el gate audita
   │ README: "el agente instala godogen_assets en tu GPU"
   ▼
htdt/godogen_assets   2 ★            ← 69 archivos versionados, CERO licencia
   ▼
modelos gated de terceros (stable-audio-3-small-sfx exige aceptar su licencia)
```

🔴 **Un repo sin grant no es "permisivo por defecto": es "prohibido por
defecto".** Por derecho de autor, sin licencia **no hay permiso** para copiar,
modificar ni redistribuir. Y el vehículo es un repo **MIT con 7.378 estrellas**,
es decir el que **pasa limpio** cualquier auditoría que mire el repo que el
cliente nombró.

**Confirmado por dos canales independientes**, no por un 404 mal leído: `find`
sobre el clon → 0; `search/code repo:htdt/godogen_assets filename:LICENSE` →
`total_count: 0`.

> **Consecuencia de método (modifica la Receta 16):** un pack de skills es un
> **instalador**, y se audita como un instalador. El gate tiene que **seguir las
> instrucciones de instalación** y auditar lo que el agente va a clonar en
> tiempo de setup, no sólo el árbol del repo declarado. 🟢 Y hay un punto de
> control natural nuevo: [vercel-labs/skills](https://github.com/vercel-labs/skills)
> (MIT, 40.334 ★) es la CLI por la que estos packs se instalan.

---

### 20. 🆕 🔴 El sentimiento del jugador tiene dos cifras irreconciliables, y la diferencia **es** el producto

Este archivo y `intel/market.md` vienen sosteniendo **~85% de rechazo del
jugador** a la AI en juegos (Quantic Foundry, ~2M de jugadores). Este pass mide
lo contrario en dos muestras distintas:

| Fuente | Qué pregunta | Resultado |
|--------|--------------|-----------|
| Quantic Foundry (~2M jugadores) | Actitud hacia la AI en juegos, **en abstracto** | **~85% de rechazo** |
| GameDiscoverCo (~3.800 usuarios de Steam) | **Comprarías** un juego con disclosure de AI en Steam | **43% no tiene problema** |
| Misma familia de datos, otra lectura | ¿Tenés problema con la AI en juegos? | **sólo 31% sí** |

🔵 **No se elige una cifra, y no es un error de nadie: miden cosas distintas.**
El rechazo declarado en abstracto y la decisión de compra ante una etiqueta
concreta **son dos variables diferentes**, y la brecha entre ambas es grande y
sistemática.

🟢 **Y la brecha es el argumento comercial:** significa que **la etiqueta cuesta
mucho menos de lo que el discurso público sugiere**, que es precisamente la
objeción nº1 con la que un publisher frena la Receta 7. ⇒ **llevar las dos
cifras a la reunión y nombrar la diferencia** es más creíble que citar la
conveniente, y evita que el cliente aparezca con la otra. ⚠️ Ninguna de las
muestras está desagregada por región.

---

### 21. 🆕 🟡 La adopción declarada por developers va de 29% a 90% según cómo se pregunte

Cuatro cifras de 2026, todas defendibles, todas incompatibles:

| Fuente | Muestra | Cifra |
|--------|---------|-------|
| GDC *State of the Game Industry 2026* | 2.300+ profesionales | **≈36%** |
| Otra encuesta 2026 | n/d | **29%** (bajando desde 36%) |
| Sondeo de Google Cloud | 615 developers | **~90%** "usa AI de alguna forma" |
| CESA (Japón) | 1.349 respuestas | **85,8%**, 63% a diario |

🔴 **La horquilla 29–90% no es ruido de muestreo: es el redactado de la
pregunta.** *"¿Usás gen AI en tu trabajo?"* y *"¿hay AI en algún punto de tu
pipeline?"* describen dos mercados distintos — y el segundo incluye autocompletado
de código, que casi nadie llama "AI en el pipeline".

> 📐 **Regla: pedir el redactado antes de citar el número.** Esta KB ya tenía la
> regla *"un número sin su muestra no es un número"* (cycle 5). Este pass le
> agrega la segunda mitad: **un número sin su pregunta tampoco lo es.** Y la
> consecuencia de venta es concreta: la cifra alta sirve para argumentar
> *gobernanza* (ya está pasando, sin control), la baja para argumentar
> *habilitación* (todavía no escaló). **Elegir según lo que se vende, declarando
> cuál se usó.**

---

### 22. 🆕 🟢 APAC pasó de un país con cifra propia a dos, y aparece la práctica que se puede implementar

- **Japón**: CESA **85,8%** (1.349 respuestas), **63% a diario**; **JOGA: 100%**
  de las empresas de juego online encuestadas, Gemini la herramienta más común.
- 🆕 **Corea**: **77,5% de 120 empresas de juego** usan gen AI (**KOCCA**,
  Q2-2026), y **"AI skills" es la prioridad nº1 de contratación** del sector.

⇒ La tesis de esta KB —*"en APAC no se vende adopción, se vende gobernanza,
escala y evaluación de calidad"*— **dejó de apoyarse en un solo país**.

🟢 **Y lo nuevo y más vendible: los estudios japoneses ya declaran los controles
que esta KB sabe construir** — **restringen qué herramientas puede usar el
personal** y **prohíben que salidas crudas de AI lleguen al producto** sin
revisión. **Primera vez en esta KB que la práctica declarada del cliente coincide
exactamente con un entregable ya escrito** (Recetas 15 y 16, gates en el pipeline
de build). No hay que convencer de la necesidad: hay que implementar lo decidido.

⚫ **Y el hueco más caro de la región: China.** Con el instrumento corregido
(*"video game"* en vez de *"gaming"*), **ninguna cifra de adopción de estudios
chinos** — siendo el mercado con el régimen vinculante más desarrollado y el
origen de los world models punteros. **Cero de medición, no de mercado.**

---

### 23. 🆕 🔴 El archivo de licencia dejó de ser confiable **como documento**, no sólo como veredicto

Las tendencias 9 y 19 de esta página describieron el riesgo de licencia como
**sistemático** y después como **ausencia de grant**. Este pass agrega un tercer
modo, y es peor que los dos anteriores porque **pasa los controles existentes**:
**el archivo está dañado**.

`Farama-Foundation/Minigrid` (2.543 ★, benchmark RL estándar) distribuye un
`LICENSE` de **2.507 B** que dice *"Apache License, Version 2.0"*, contiene
**sólo las §§ 1 y 9**, **le faltan las §§ 2-8 — el grant de copyright, el de
patente y la redistribución** — y trae un **encabezado de hunk de diff literal
(`@@ -154,49 +194,23 @@`)** en la línea 18. Su `pyproject.toml` declara **MIT**.
El grant real está en **`docs/LICENSE` (12.201 B): MIT + Apache-2.0 completos**.

**Por qué es una tendencia y no una anécdota:** los tres controles que esta KB
construyó en siete passes **fallan cada uno a su manera** contra este caso.

| Control | Qué hace con este archivo |
|---------|---------------------------|
| **Checksum de bytes** (R10/R20) | **Lo clasifica mal, no lo caza.** 2.507 B con íncipit Apache cae en la categoría ya bendecida *"Apache sin apéndice"* (`libgdx`, 10.174 B). El tamaño distingue *recortado* de *canónico*; **no distingue *recortado* de *roto*** |
| **El íncipit dicta** (R20) | **Dice Apache-2.0** — y la línea 1 es correcta. El íncipit es válido; **el cuerpo no existe** |
| **Sonda por nombre** | Devuelve **el archivo equivocado** con `HTTP 200` y 2.507 B ⇒ **ni 404 ni byte-count de no-hallazgo** (umbral 50 B). La sonda **no omite el grant: lo sustituye** |

**Lo único que funcionó fue la enumeración (R17)**, y encontró el documento real
en una ruta **no adivinable**. ⇒ **Regla nueva (Receta 21): verificar la
INTEGRIDAD del archivo antes de interpretar su contenido** — ausencia de
artefactos de VCS y presencia de las cláusulas operativas.

**Y un corolario que vale para toda la flota:** cuando **el manifiesto y el
`LICENSE` discrepan en la *familia* de licencia** —acá MIT vs Apache-2.0, los dos
artefactos versionados, **ninguno un anuncio**— la T63 (*"leé el manifiesto, no
el anuncio"*) **no alcanza para desempatar**, porque ninguno de los dos es el
anuncio. **Esa discrepancia es una ambigüedad abierta, no un veredicto**, y se
resuelve buscando el tercer documento. 🟢 En este caso se resolvió: **Minigrid es
permisivo y usable, citando `docs/LICENSE`.** Hermanos sanos (`Gymnasium` 1.104 B,
`PettingZoo` 1.073 B, 0 artefactos) ⇒ **el daño es del repo, no del org.**

### 24. 🆕 🔴 *"Open-source"* en un paper revisado por pares **no es un grant** — y el mejor agente del nicho es inusable

La tendencia 17 celebró que la licencia de los **pesos** dejó de ser incognoscible.
Este pass encuentra el caso inverso y lo encuentra **en el tope técnico del
nicho**.

`MineDojo/NitroGen` (**2.251 ★**, **oral en CVPR 2026**, arXiv 2601.02427) es, por
capacidad, lo más relevante que apareció en gaming este año: behaviour cloning
sobre **40.000 horas de gameplay en más de 1.000 juegos**, wrapper compatible con
Gymnasium, pesos publicados en Hugging Face. El abstract dice
*"we **open-source** the NitroGen dataset, simulator, and pre-trained weights"*.

El archivo dice otra cosa: **`NVIDIA License`, 4.729 B.**

- **§ 3.3** — *"only may be used **or intended for use** non-commercially…
  non-commercial research purposes only"* ⇒ **un PoC pago ya está fuera**.
- **§ 3.2** — **viral**: ponerle tus términos a un derivado es legal **sólo si
  arrastran la § 3.3**.
- **§ 3.7** — arrastra componentes bajo otras licencias, nombrando el **Meta
  OPT-IML 175B License Agreement**.
- **§ 3.4** — **terminación del grant** ante un reclamo de patente.

⛔ **Exclusión dura para cualquier entregable de Globant.**

**La tendencia:** el canal por el que llega el estado del arte en agentes de
juego es **el paper**, y **un venue de primer nivel no audita licencias**. El
abstract **es el anuncio** en el sentido de la T63, aunque esté revisado por
pares — y la *model card* de Hugging Face, que sí dice *"Governing Terms: NVIDIA
License"*, es el manifiesto. ⇒ **Todo modelo que entre a esta KB por vía
académica necesita byte-read del `LICENSE` antes de la fila**, sin importar el
prestigio del venue. Es el espejo exacto del hallazgo del cycle 7 sobre
`GameScaling`: ahí el grant de pesos resultó **mejor** que el badge; acá, **mucho
peor** que el paper.

### 25. 🆕 🟡 *"Gaming"* es un homónimo, y el informe-faro de 2026 es del otro rubro

**Las cuatro búsquedas regionales de este pass devolvieron en primer lugar un
informe de casino.** *"The State of AI in Gaming 2026"* (**UNLV International
Gaming Institute + KPMG**) es el primer benchmark serio del rubro: *AI Maturity
Index* **≤ 45/100**, gobernanza rezagada frente a la adopción, y **divergencia
medida regulador-industria** (los reguladores ponen el juego problemático y los
resultados injustos arriba; la industria, último). 🔴 **Es de apuestas, no de
videojuegos.**

**Por qué entra como tendencia y no como nota al pie:** en inglés regulatorio y
de mercado *"gaming"* **significa juego de azar**, de modo que **toda búsqueda
de la forma *"AI gaming <región> <año> regulation"* —exactamente la que esta
rotación ejecuta cada pass— está sesgada hacia el rubro equivocado por
construcción**. El riesgo no es un dato malo: es **un dato bueno del vertical
ajeno**, cuantitativo y reciente, que se ingiere sin resistencia.

⇒ **Regla de ingesta para la flota: clasificar la fuente en `videojuegos` o
`azar/iGaming` ANTES de citar la cifra, y declararlo en la misma línea.** Es la
R14 (ejes de alcance de un grant) aplicada al **alcance de un número**.
🟢 **Y tiene su lado útil:** iGaming es un vertical real y grande —sobre todo en
**NA** y en **LATAM**, donde Brasil abrió mercado licenciado el 1-ene-2025— con
compradores y plataformas propias. **Se atiende como vertical separado**, con la
frontera escrita en `verticals/solutions.md`, no mezclándolo con videojuegos.

### 26. 🆕 🔴 La cifra insignia de una KB puede ser correcta y estar mal usada: **48,8% es hispanoamérica, no LATAM**

El dato más citado de este archivo sobre LATAM —48,8% de jugadores dispuestos a comprar
juegos hechos con IA— proviene de **PGB Latam 2026: 7.202 respuestas de Argentina, Chile,
Colombia, México y Perú**. **Brasil no está en ese panel.** Medido por separado
(**PGB Brasil 2026, >7.000 personas, campo 5–13 de marzo de 2026**), Brasil da **39,3%**.

- Brasil es el mercado más grande y maduro de la región (penetración >70%).
- La diferencia es de **~9,5 puntos**, y va en contra del mercado más valioso.
- En ambos paneles convive con **40,6% que teme despidos y precarización**.

**Lo que la tendencia dice, más allá de LATAM:** una cifra heredada de pass en pass
**se endurece sin ganar evidencia**. Esta llevaba cinco passes citada sin su panel.
El control que la cazó no fue una fuente nueva: fue **volver a pedirle la muestra a un
número que ya estaba adentro**.

> **Regla de método (nº16): auditar periódicamente las cifras propias, no sólo las
> entrantes.** El riesgo de una KB madura no es incorporar un dato falso, es
> **conservar un dato verdadero fuera de su alcance**.

### 27. 🆕 🔴 El sprint regulatorio europeo es **más angosto** de lo que la industria (y esta KB) venía diciendo

El 2-dic-2026 es real, pero cubre mucho menos de lo que sugiere el pánico de calendario:

| Afirmación corriente | Lo que dicen las fuentes de este pass |
|---|---|
| "Todo debe estar marcado el 2-dic-2026" | El plazo cubre **sólo el marcado legible por máquina del *proveedor* (Art. 50(2))** y **sólo** para sistemas puestos en el mercado **antes del 2-ago-2026** |
| "Lo nuevo tiene hasta diciembre" | ❌ Lo puesto en el mercado desde el **2-ago-2026 no tiene gracia alguna**: cumple desde el lanzamiento |
| "Los deployers también tienen prórroga" | ❌ **Los deberes del deployer no se pospusieron**: rigen desde 2-ago-2026 |
| "Hay que marcar los assets ya generados" | ❌ **El contenido sintético generado antes del 2-ago-2026 no se marca retroactivamente** |

Y por encima de todo: la Comisión sostiene que **en principio los videojuegos con IA no
tienen obligación bajo el AI Act** — pathfinding, balanceo y entrenamiento de NPC son
**riesgo mínimo**. Lo prohibido de verdad (Art. 5, no "alto riesgo") es el **NPC diseñado
para empujar gasto in-game**, con multas de hasta **35 M€ o 7%** de facturación global.

**Consecuencia de ingeniería:** la mayoría de los estudios son **deployers**, no
providers, del modelo generativo. Les aplica avisar que se interactúa con IA (50(1)) y
etiquetar deepfakes (50(4)), **no** el marcado del 50(2) que vence en diciembre.
**Triar provider vs deployer es ahora el paso 0** (ver Receta 22).

### 28. 🆕 🟢 La adopción declarada se partió por **jerarquía**, y los dos lados están medidos

| Instrumento | Quién responde | Pregunta | Resultado |
|---|---|---|---|
| Vivrato VAIA (n=**101**, abr–sep 2026, entrevistas) | **líderes senior de estudio** | ¿usa IA en ≥1 de 9 etapas? | **94 %** |
| idem | idem | profundidad de integración | **52 % puntúa ≤ 3/10** |
| Encuesta GDC (**13-nov-2026**) | **trabajadores** | ¿usás IA en tu trabajo? | **40 %** |
| idem | idem | ¿la genAI es mala noticia? | **55 % sí** |

**94 vs 40 no es una contradicción: es la distancia entre la empresa y la persona.** Y
el 52% en ≤3/10 dice que, del lado del líder, **el 94% es amplitud, no profundidad**.

Esto **cierra el rango 29%–90%** que la tendencia nº21 registraba sin explicar: el techo
lo produce preguntarle a líderes con umbral *"al menos una etapa"*; el piso, preguntarle
a trabajadores por su propio uso. **El rango no era ruido: era la jerarquía.**

⚠️ Vivrato cubre estudios con sede **en NA y Europa**, y reporta **Europa más profunda
que Norteamérica**. **No cubre LATAM ni APAC.**

### 29. 🆕 🔴 Un repo de 25.570 ★ puede faltar entero, y ninguna auditoría de licencias lo detecta

`clockworklabs/SpacetimeDB` —el competidor técnico serio de Nakama— estuvo **ausente de
esta KB durante ocho passes**. Las Recetas 10, 14, 17, 20 y 21 auditan grants cada vez
con más finura, y **ninguna podía encontrarlo**, porque todas auditan **lo que ya está
escrito**.

> **Un silencio no se contradice a sí mismo.**

Cuando por fin se lo abrió, el resultado justificaba el esfuerzo: **BSL 1.1**
(`LICENSE.txt`, 42.562 B), Change Date **2031-11-20** → AGPL-3.0 con excepción de
linking, y un Additional Use Grant que permite **una sola instancia en producción**.
GitHub lo reporta como `NOASSERTION`, que no es un veredicto sino su ausencia.

**La tendencia:** a medida que una KB madura, **el error se corre de la precisión hacia
la cobertura**. Hace falta un control de inventario —"¿qué repos grandes del nicho no
están en ninguna fila?"— distinto de los controles de licencia.

### 30. 🆕 🟡 Los homónimos son ahora un modo de falla **de dos canales**, no uno

| Homónimo | Canal | Caso medido |
|---|---|---|
| **`gaming`** = videojuego ≠ casino | prensa / informes | *"The State of AI in Gaming 2026"* (UNLV IGI + KPMG, 9-abr-2026) mide **83 empresas de juego de azar + 113 reguladores** |
| **`Unity`** = engine ≠ Unity Catalog | **GitHub** | `databrickslabs/ontobricks` (485 ★) aparece en la búsqueda `unity ai agent mcp` porque expone el **metastore de Databricks** por MCP |

El primero contamina la inteligencia de mercado; el segundo, el catálogo de repos.
**Ambos entran por búsqueda de texto y ninguno es detectable sin abrir el artefacto.**

> **Regla (nº14):** antes de aceptar una fila que llegó por búsqueda, verificar que el
> término que la trajo **significa ahí lo que significa acá**.

## En el radar (emergente, 2026-2027)

| Tendencia | Señal verificada | Estado |
|-----------|------------------|--------|
| **Plugin MCP first-party en Unreal** | Reportado como experimental en UE 5.8 | Sin confirmar en fuente primaria |
| **Rendering neural sobre engine existente** | Matrix-Game 4.0, 67 FPS en RTX 5080 | Ejecutable hoy en hardware de consumidor |
| **DLSS 5 generativo** | Presentado en GTC 2026 (mar-2026): transforma skins y modelos en juego | Producto NVIDIA, no OSS |
| **AI companions como feature de producto** | Krafton *Co-Playable Character* → "PUBG Ally", modo arcade, 1S-2026 | Anunciado |
| **Rechazo editorial explícito a gen AI** | Capcom negó usarla en contenido; CEO de Nintendo dijo que no la usarán por riesgo de IP | La división es pública |
| **Un solo estudio asistido por agentes** | [WhiteTowerAI/ohmygame](https://github.com/WhiteTowerAI/ohmygame), Apache-2.0, **2.486★ (+107% en 25 días)** | 🟢 **Promovido a tendencia confirmada** → ver tendencia 10 |
| **Modding asistido por AI** | [rehan-remade/universal-modder](https://github.com/rehan-remade/universal-modder), MIT, **9.809★ (★ ahora verificada)** | Categoría con volumen propio |
| 🆕 **Práctica PROHIBIDA, no alto riesgo, en la UE** | La Comisión Europea citó como ejemplo de práctica prohibida (Art. 5) los **NPCs generados por AI diseñados a empujar al jugador a gastar más**; comentario legal extiende a optimización de engagement sobre menores | 🔴 **Régimen distinto: lo prohibido no se gestiona, se cancela.** Primera conversación si el cliente tiene NPC conversacional conectado a la tienda |
| 🆕 **Digital Fairness Act** | Borrador previsto para **Q4-2026**: diseño adictivo y mecánicas aleatorizadas (loot boxes) | ⛔ **SUPERADO 2026-11-22 — publicado 11-nov-2026, COM(2026) 1100; ver cycle 10 arriba.** Convierte el monetization design en materia regulada |
| 🔴 **World model 3D con licencia *territorial*** — **corregido 2026-11-21** | [Tencent-Hunyuan/HY-World-2.0](https://github.com/Tencent-Hunyuan/HY-World-2.0), 2.900★. **Sí publica grant**: `License.txt` (⚠️ mayúscula L, minúscula t), 16.858 B, *Tencent HY-WORLD 2.0 Community License*, release 15-abr-2026 | 🔴 **No es "sin licencia" — es una licencia no-OSI que se cae por región:** *"THIS LICENSE AGREEMENT DOES NOT APPLY IN THE EUROPEAN UNION, UNITED KINGDOM AND SOUTH KOREA"*. Usable en NA/LATAM/APAC-ex-Corea, **inusable en EMEA y Corea**. **No** es el patrón de `unreal-mcp`: ahí no hay archivo. |
| 🆕 **Reescritura de engines en Rust** | [horizon-rust/horizon](https://github.com/horizon-rust/horizon), **11.515★**, Minecraft Java en Rust | 🔴 **LGPL-3.0**: aparecerá en listas de trending de gaming y **no es permisivo** |
| **Pesos con licencia distinta al código** | Matrix-Game y MineWorld publican pesos en HF | 🔴 **No verificable desde aquí** (ver gaps) |
| 🆕 **Catálogo horizontal de agentes con división de gaming** | [msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents), MIT, **169.646★ / 27.561 forks**, ~230 agentes en 18 divisiones, una de ellas Game Development (Unity/Unreal/Godot/Blender/Roblox) | 🟢 **4,2× el repo de orquestación específico de gaming** (40.485★). El inventario de agentes **ya es commodity**: el diferencial es harness, evaluación e integración con el engine |
| 🆕 **IP existente → juego, por pipeline de agentes** | [zenstory-ai/novel-to-game](https://github.com/zenstory-ai/novel-to-game), MIT, 1.175★: despiece de novela → diseño → build → *blind-play*, con salida a **WeChat/Douyin/TapTap** | 🟢 Primer repo de la KB cuyo *surface* de distribución es **APAC exclusivamente** |
| 🆕 **El mod como vía a producción** | [MinLL/SkyrimNet-GamePlugin](https://github.com/MinLL/SkyrimNet-GamePlugin), 386★: NPCs con voz, memoria y acción en Skyrim. Licencia propia **con su API Papyrus en MIT** | 🟡 **Integrable, no forkeable.** La implementación de NPC-LLM con más horas de jugador reales de esta KB |
| 🆕 **Engines diseñados para agentes, no adaptados** | [SummerEngine/summer](https://github.com/SummerEngine/summer) (MIT, 110★) y [aigengame/godot-agent](https://github.com/aigengame/godot-agent) (MIT, 58★): el engine expone MCP + CLI + skills de build/verify | 🟡 Muy temprano. Señal de dirección, no ruta recomendable todavía |

## 🔴 Retirado del radar
- **Aivill** (`SKYHUBDev/Aivill`) estaba listado en julio como "villanos adaptativos
  que aprenden del jugador — concepto emergente a observar". Verificado el
  2026-10-26: el repo está **vivo pero vacío**, sin README y sin licencia en HEAD. No
  hay proyecto que observar.
- **Aivill** reconfirmado el 2026-11-20: **0★**, sin licencia y sin README. Cerrado.
- **OpenGame** (`leigest519/OpenGame`, Apache-2.0, **3.077★**) se mantiene, con
  advertencia agravada: **sin commits desde 2026-09-03** y el README sin novedades
  desde abr-2026.
- 🆕 **LMGame Bench / GamingAgent** (MIT, 1.006★) baja de "instrumento nuevo" a
  **instrumento congelado**: `pushed_at` 2025-11-16, **un año exacto sin commits**.
  Sigue siendo el único benchmark publicado del nicho y la tendencia 6 se sostiene,
  pero no hay que presentarlo como un proyecto en desarrollo.

## Gaps declarados
- **Licencia de los pesos**: `huggingface.co` no resuelve DNS en este entorno
  (`getaddrinfo ENOTFOUND`), así que el grant de los pesos de Matrix-Game 4.0 y de
  los checkpoints de MineWorld **no está verificado**. En world models, los pesos
  son la mitad del entregable. Precedente: en `automotive-kb`, Alpamayo 1.5 tiene
  código Apache-2.0 y pesos **OpenMDW-1.1**.
- 🟢 **GAP CERRADO — y la lección es sobre la KB, no sobre el entorno.** El pass de
  octubre declaró `api.github.com/search/*` **bloqueado** ("sessions are bound to
  their configured repositories") y en consecuencia dejó ~20 filas con ★ heredadas
  de julio y **nunca midió `pushed_at` ni `archived`**. **No está bloqueado:**
  `gh api -X GET search/repositories -f q='repo:<slug>'` funciona y devuelve
  existencia + SPDX + ★ + `archived` + `pushed_at` + rama default en una llamada.
  Con ese canal este pass releyó **el 100% de las ★** y encontró `0ad` archivado,
  5 repos dormidos y 2 renames. **Regla para la rotación: un canal declarado
  bloqueado debe reintentarse cada pass, no heredarse como conclusión** — una
  creencia errónea sobre el instrumento congeló una columna entera de la KB
  durante casi un mes. (El canal que sigue sin servir es `curl -sI` sobre
  github.com, que devuelve respuestas del proxy y no del repo.)
- 🟢 **GAP CERRADO (cycle 18) — "listado de directorios: sigue sin canal" era
  falso.** El canal existe y es **git**, no la API de GitHub:
  `git clone --filter=blob:none --no-checkout --depth 1` + `git ls-tree -r HEAD`.
  41 repos, 0 fallos, 329+ archivos de licencia enumerados. **Matrix-Game tiene 28
  archivos de licencia, no 7**, y el nº 28 es **CC BY-NC 4.0** dentro del benchmark
  `GameWorldScore`. `Matrix-Game-2/` (88 archivos) no tiene licencia propia ⇒ cae
  bajo el MIT de la raíz. Lo que sí está cerrado es `gh api repos/<slug>/contents`
  (**403**): la creencia era correcta sobre el instrumento equivocado. **Regla
  extendida para la rotación: un canal declarado INEXISTENTE se reintenta cada pass
  igual que uno declarado bloqueado, y con una herramienta distinta.**
- **`eur-lex.europa.eu` no es alcanzable** desde este entorno, así que el registro
  primario del EU AI Act **sigue sin leer tras 5 passes y 2 KBs**. Las fechas de
  este archivo provienen de comentario legal secundario, consistente entre sí pero
  no verificado en la fuente oficial.
- **Oferta OSS de origen LATAM**: buscada explícitamente en español apuntando a
  estudios brasileños y argentinos. **Cero repos de gaming AI.** Detalle y
  denominador en `intel/market.md` → LATAM.
- 🆕 **Fuente primaria del EU AI Act: sigue sin leer, y ahora con el motivo
  medido.** `digital-strategy.ec.europa.eu`, `orrick.com`, `dlapiper.com` y
  `github.blog` dan **ENOTFOUND** en esta sesión (WebFetch no resuelve hosts
  arbitrarios); la API de `huggingface.co` responde **vacío**. 🟢 **Pero el
  2-dic-2026 quedó resuelto a nivel de artículo** (**Art. 111(4)**) por 4+
  secundarias independientes y concordantes — ver `intel/market.md` → EMEA.
  **Verificar contra el texto oficial antes de cualquier entregable firmado.**
- 🆕 **Adopción de gen AI por estudios LATAM: cero medido con dos instrumentos.**
  No existe encuesta por país ni por tamaño. El informe Xsolla/GamesBeat
  *State of the LATAM Games Industry 2026* está *gated*.
- 🆕 **Adopción de estudios chinos: cero medido.** Ver tendencia 22.
- 🆕 **Licencia de los *pesos* de NVIDIA Audio2Face-3D: no resoluble.** Los
  modelos están en Hugging Face y la API del host responde vacío. El **código**
  sí quedó leído en bytes (MIT / Apache-2.0 + 3 porciones MIT).
- 🆕 **Plugin de UE5 de Audio2Face-3D:** el README de NVIDIA lo declara **MIT**
  pero **no enlaza repo de GitHub**. Sin slug no hay byte-read ⇒ **gap, no fila**.

---
*Fuentes añadidas 2026-11-20: análisis independiente del censo Steam (~63.600
releases) y contra-conteo sobre SteamDB (49,9% / 49,7% oct-2026, desglose por tipo
y tamaño); GamesRadar, Analytics Insight, gamedev.net, vgtimes (Ubisoft Motherbrain
y 166 despidos, 19-nov-2026); techtimes (GDC nov-2026, 54% sin empleo); BigGo
Finance y webpronews (Tencent Game for Peace 110M usuarios de NPC con AI / 17,7M
DAU pico; Tencent LAP; NetEase Justice Mobile con DeepSeek); FRVR (desmentido del
supuesto veto de NetEase); Niko Partners y smartkarma (aprobaciones NPPA);
WH Partners, DLA Piper, twobirds, gamingtechlaw (Art. 5 NPCs que empujan al gasto,
Digital Fairness Act Q4-2026, Digital Omnibus); Malta Gaming Authority; UNLV/KPMG
(reguladores NA, Alberta y Colorado ene-2026); Quantic Foundry (~2M jugadores);
coddykit y gittrend (trending de GitHub nov-2026). **38 repos verificados en dos
canales** (`gh api search/repositories` con qualifier `repo:` exacto + lectura del
grant en `raw.../HEAD/`), con control negativo en ambos sentidos.*

*Fuentes 2026-10-26: GDC 2026 survey (vía ShaneTheGamer, Market.us); Sulka Haro
(censo Steam); digitalcitizen.life; notebookcheck (Sweeney); pcgamer;
gamingtechlaw.com, twobirds.com, aiandgames.com (EU AI Act); askajay.ai,
paglago.net, iagovernance.com, techpost.bsa.org (APAC); note.com/commodvs
(Krafton/Capcom/Nintendo); strayspark.studio y mcp-scorecard.ai (ecosistema MCP);
arxiv.org (GameLogicBench, MineExplorer); benchlm.ai; raiscui/gh-trends
(daily 2026-10-26). Repos verificados con `git ls-remote` + lectura de licencia
en HEAD, con control negativo.*

---
*Pass 2026-11-21 (cycle 6) — tendencias 16, 17 y 18 agregadas: el copyleft se
mudó a los packs de skills (3 de 6 packs nuevos >400★ son LGPL-3.0/AGPL-3.0, y
la ruta de contaminación es un `cp -r` que ningún gate de build ve); la licencia
de los pesos dejó de ser incognoscible vía la regla del modelo base
(GameScaling ← Qwen3.5, Apache-2.0), con la corrección de que Hugging Face es
una **denegación de política del proxy** y no un fallo de DNS; y la
infraestructura de gaming se está revendiendo como infraestructura de agentes
(Rivet/OpenGB → orquestador agéntico), tercer caso de drenaje de especificidad
hacia lo horizontal. Fuentes nuevas de este pass: AlixPartners (múltiplos 2-3×),
Omdia (*AI in Games: State of the Market 2H26* — identificado, de pago, no
leído, sin cifras atribuidas), BCG (*Video Gaming Report 2026* — idem),
KPMG/SIGMA (gobernanza 30/100, 58% de reguladores, 13% de operadores con
política), WH Partners (*Same NPC, Different Rules* — Art. 50 por edad de
audiencia), PGB Latam vía gamescom latam (Gen Z mexicana 51,8%), CAC China
(>550 servicios registrados a mediados de 2026), Gabinete de Japón (AI Basic
Plan del 14-jul-2026). 2026-11-21.*

---
*Pass 2026-11-21 (cycle 7) — tendencias 19, 20, 21 y 22 agregadas. Fuentes
nuevas: KOCCA (Korea Creative Content Agency, Q2-2026, 77,5% de 120 empresas);
JOGA vía prensa japonesa (100% de empresas de juego online, Gemini);
GameDiscoverCo (~3.800 usuarios de Steam, 43% sin problema de compra);
GDC *State of the Game Industry 2026* (2.300+ respondentes, ≈36%); sondeo de
Google Cloud (615 developers, ~90%); censo de Steam de 53.597 releases
(30,8% 2026-hasta-julio / 19,9% 2025 / 10,9% 2024, y 47% vs 14% por tamaño de
título); encuesta de 826 trabajadores (9/10 quieren más que un sí/no, 28,4%
conforme con sí/no); comentario legal concordante sobre **Art. 111(4)** y la
transición del Art. 50(2) al 2-dic-2026, guías no vinculantes de la Comisión del
20-jul-2026 (umbral de "obviedad" en NPCs single-player), sanción de 15 M€ / 3%,
Code of Practice on Transparency of AI-Generated Content; Video Games Europe
(escrito del 3-jun-2026) y EGDF (prioridades 2026); imarc y Xsolla/GamesBeat
(estructura de mercado LATAM: Brasil ~13.225 devs / ~1.042 estudios, México
72,6 M jugadores = 61% de 6+); PNUD vía prensa sectorial (rezago LAC).
**24 repos nuevos verificados en tres canales**, 23 licencias leídas en bytes,
4 badges `NOASSERTION` resueltos en ambas direcciones, 1 fila corregida
(`ml-agents`, grant partido por rama) y 1 gap declarado sin fila (GameReverie).
⚠️ El informe **UNLV/KPMG "State of AI in Gaming 2026" (83 empresas) es un
benchmark de *gambling***, no de videojuegos: esta KB ya lo etiquetaba así y se
confirma. 2026-11-21.*
