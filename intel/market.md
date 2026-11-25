---
industry: gaming
region: Global
updated: 2026-11-25
---

## 🆕 Lo que cambió — pass cycle 20 (2026-11-25): **2026 es el año en que el camino del dinero se abrió por ley en las CUATRO regiones — y la oferta open source para juegos es cero**

Los cycles 10-18 siguieron la regulación del **contenido** generado. El cycle 19
siguió la de la **decisión** (precio, oferta, recomendación). Este pass sigue la
del **canal de cobro**: quién puede cobrarle al jugador, con qué comisión y desde
qué tienda. Es una familia distinta —derecho de competencia, no protección del
consumidor ni AI— con instrumentos distintos, y **ya cambió de estado en las
cuatro regiones en los últimos 12 meses**:

| Región | Instrumento que abre el canal de cobro | Estado medido al 2026-11-25 |
|---|---|---|
| **North America** | *Epic v. Apple* (anti-steering) + programa de *external offers* de Google Play (EE.UU.) | 🔴 **Comisión jurídicamente indeterminada**: el 9º Circuito (11-dic-2025) confirmó el desacato pero **anuló** la prohibición total de comisión y devolvió el caso para fijar una "atada a los costos reales". **Certiorari concedido 30-jun-2026** |
| **EMEA** | **DMA** — Apple unificó a todos los devs de la UE en **un solo modelo el 1-ene-2026** | ✅ **En vigor**: la *Core Technology Fee* (0,50 €/instalación) fue reemplazada por la **Core Technology Commission del 5 %** |
| **APAC** | **Japón: Mobile Software Competition Act (MSCA)** | ✅ **En vigor desde el 18-dic-2025**: tiendas de terceros y *billing* alternativo obligatorios |
| **LATAM** | **Brasil: acuerdo con el CADE** (aprobado dic-2025, plazo de 3 años) | ✅ **En vigor con iOS 26.5 (jun-2026)**: marketplaces alternativos y pagos fuera de IAP |

🔴 **La consecuencia de arquitectura, que es lo que esta KB tiene para vender:**
las cuatro regiones piden lo mismo en ingeniería aunque lo digan distinto —
**poder cobrar desde más de un canal, con comisión distinta por región, y entregar
el mismo derecho al jugador sin importar por dónde pagó**. Eso exige exactamente
tres cosas que esta KB nunca había inventariado: un **validador de recibo por
tienda** (enchufable, porque las tiendas nuevas aún no existen), un **ledger** que
registre el derecho una sola vez (idempotencia contra doble entrega), y un
**enrutador de pago** por región. Ver las Recetas 52-55.

⚠️ **La comisión NO es un número para hardcodear.** Al 2026-11-25, el porcentaje
que cobra Apple en EE.UU. está **ante la Corte Suprema**, y en Brasil **las
fuentes consultadas se contradicen** (10 %/21 % vs 25 %/10 % + 5 %). Cualquier
modelo financiero que ponga la comisión como constante de código es un modelo que
hay que recompilar cuando salga el fallo. **Va como configuración por región, con
fecha de vigencia.**

### 🔴 El gap de oferta, medido: la tienda D2C para juegos **no tiene ninguna opción open source**

Buscado explícitamente: *webshop* / tienda D2C para juegos móviles. **Todos** los
proveedores que aparecen son **SaaS propietario**: Appcharge, Aghanim, Tebex,
Xsolla, Coda Payments, Fungies, Neonpay. Y en backend de juego con catálogo +
entitlement, los pesos pesados también: **AccelByte, PlayFab, Metaplay, Pragma**,
y el *Ecom* de Epic (atado a una sola tienda).

La oferta open source de esta capa **no es de juegos**: es de fintech y de
e-commerce genérico (Medusa, Saleor, Kill Bill, Hyperswitch, TigerBeetle), más
**una sola pieza nativa de juegos** que sí cubre compra y suscripción:
`heroiclabs/nakama` (Apache-2.0).

> 🟡 **Corrección a una afirmación de búsqueda, verificada contra el código:** las
> comparativas de proveedores afirman que Nakama *"no tiene store ni catálogo ni
> entitlement"*. **Falso en la mitad que importa.** Clonado y listado el árbol:
> `iap/iap.go`, `iap/iap_samsung.go`, `server/api_purchase.go`,
> `server/core_subscription.go`, `server/console_purchase.go` y migraciones SQL
> dedicadas desde 2021 (`20210416090601-purchase.sql`,
> `20220426122825-subscription.sql`). Validadores nativos medidos: **Apple (JWS y
> recibo legacy), Google (compra y suscripción), Huawei, Facebook Instant**, más
> Samsung. Lo que Nakama **no** trae es el **catálogo/escaparate** (precios,
> ofertas, bundles) — y *ese* es el gap real, mucho más chico y mucho más barato
> que "construir el backend de comercio".

# Mapa de mercado — Gaming AI

> Key players, market map y oportunidades **por región**.
> Última actualización: **2026-11-22 (cycle 19)** (passes previos: 2026-11-22 cycles 18/17/16/15/14/13/12/11/10, 2026-11-21 cycles 9/8/7/6/5, 2026-11-20, 2026-10-26, 2026-07-02).
>
> En el pass anterior las secciones de North America, EMEA y APAC decían
> literalmente `- To research` y sólo LATAM tenía contenido. Las cuatro regiones
> de este archivo ahora tienen hallazgos propios, y donde una región no devolvió
> nada se dice explícitamente qué se buscó y qué faltó.

## 🆕 Lo que cambió — pass cycle 19 (2026-11-22): **la regulación de 2026 dejó de hablar de la IA y empezó a hablar del PRECIO y de la OFERTA**

Los cycles 10-18 siguieron la regulación del **contenido** generado (etiqueta,
marcado, categoría de edad). Este pass busca la regulación de la **decisión**
—qué oferta, qué precio, qué recomendación recibe cada jugador— y encuentra que es
una familia distinta, con instrumentos distintos, y **ya en vigor en tres de las
cuatro regiones**:

| Región | Instrumento que pega en la capa de decisión | Estado al 2026-11-22 |
|---|---|---|
| **North America** | Leyes estatales de *surveillance pricing* (Maryland, Connecticut, New Jersey, New York) | **Maryland en vigor desde 1-oct-2026**; California AB 2564 aún en el Senado |
| **EMEA** | AI Act Art. 50(2) (marcado legible por máquina) + Art. 5 (prácticas prohibidas) + Digital Fairness Act | **Art. 50(2): 2-dic-2026 ⇒ faltan 10 días** para sistemas anteriores al 2-ago-2026 |
| **APAC** | *Rules for Price Behavior on Internet Platforms* (NDRC/SAMR/CAC, China) | **En vigor desde 10-abr-2026**, por 5 años |
| **LATAM** | Brasil: Lei 15.211/2025 (ECA Digital) + **Decreto 12.880/2026** | **Ley en vigor 17-mar-2026**; decreto reglamentando loot boxes |

🔴 **La consecuencia de arquitectura, que es lo que esta KB tiene para vender:** las
cuatro familias piden lo mismo en términos de ingeniería aunque lo digan distinto —
**poder apagar, por jugador y por jurisdicción, la personalización de oferta y
precio, y poder probar cuándo se apagó**. Eso es un *feature flag* con el dato de
edad y el país como entradas, más un registro de cambios. Es la capa que esta KB no
tenía hasta hoy (ver `verticals/solutions.md` y la Receta 50).

⚠️ **Límite declarado de todo lo anterior:** las leyes estatales de EE.UU. están
redactadas para **comercio minorista**. **Que la oferta in-game y el precio de
moneda virtual caigan en su ámbito no está establecido**, y este pass no lo
resuelve: se publica como riesgo a dimensionar con el abogado del cliente, no como
obligación.

⚠️ **Hosts que no resuelven desde esta sesión** (ENOTFOUND, se agregan a la lista
muerta por política): `bain.com`, `getunleash.io`, `docs.getunleash.io`. Las cifras
de Bain de abajo quedan marcadas como **fuente secundaria sin verificar en la
fuente**, y el comunicado específico de LATAM **no pudo leerse**: es un gap, no un
dato.

## 🆕 Lo que cambió — pass cycle 18 (2026-11-22): **la geografía del riesgo de licencia**, y qué devolvió cada región este pass

El canal 6 (árbol completo del repositorio) permite por primera vez preguntar de
**dónde viene** cada grant restringido que termina dentro de un entregable. La
respuesta tiene geografía, y no es la de la regulación.

### 🔴 El riesgo de licencia que importa **no viene del repo que se elige, viene del paper que ese repo cita**

El único grant no comercial encontrado en **41 repos / 329+ archivos** es
`CC BY-NC 4.0` dentro de `SkyworkAI/Matrix-Game` (🇨🇳 Skywork AI, China), y **no es
de Skywork**: es de `amt`, un componente académico de interpolación de frames
copiado dentro del benchmark `GameWorldScore`. Lo mismo vale para Eigen (🇫🇷/🇩🇪,
MPL-2.0 con textos GPL/LGPL legacy) y DROID-SLAM / RAFT / lietorch (🇺🇸 Princeton,
BSD-3) en el mismo árbol.

⇒ **La cadena de contaminación es académica, no corporativa, y es transnacional.**
Un world model chino embarca un componente de un paper estadounidense con licencia
no comercial y una librería europea con copyleft débil. **Ninguna de las tres
jurisdicciones explica el riesgo**: lo explica que un repo de research vendoriza
`third_party/` a mano desde papers, y un paper rara vez elige una licencia pensando
en producto.

**Consecuencia para un engagement, en cualquier región:** la pregunta de *due
diligence* no es *"¿de qué país es el repo?"* sino **"¿cómo trae sus
dependencias?"**. `vendor/` de Go (agones: 149 grants, todos permisivos salvo 2
MPL) es un régimen **auditable y aburrido**; `third_party/` copiado de papers
(Matrix-Game: 28 grants, 1 no comercial) es el régimen donde aparece el problema.

### 🆕 Lo que devolvió cada región este pass (y lo que NO)

| Región | Qué se buscó | Qué devolvió |
|---|---|---|
| **North America** | `AI gaming North America 2026 adoption regulation players` | 🔁 **Nada nuevo, y la confirmación de un homónimo.** El informe-faro que devuelve el buscador sigue siendo **UNLV/KPMG *State of AI in Gaming 2026*** — que es **gambling**, no videojuegos (AI Maturity Index 45/100, abril 2026). Esta KB ya lo etiquetaba así desde el cycle 7: **tercera confirmación**. Lo único de videojuegos: Reed Smith citando la GDC 2026 (**52%** de devs ve daño) y la coalición de seguridad infantil de Roblox con la Attorney General Alliance |
| **EMEA** | `AI gaming EMEA 2026 AI Act Article 50 deadline` | 🟡 **Un refinamiento operativo del Art. 50, no una novedad.** La excepción para videojuegos es **conjuntiva**: el contenido efímero en tiempo real queda exento *"donde el marcado sea técnicamente inviable **Y** se informe al usuario dentro de la experiencia"*. Esta KB publicaba la excepción (Receta 27) **sin el "Y"** ⇒ **cumplir una sola de las dos condiciones no exime**. El 2-dic-2026 está a **10 días** |
| **APAC** | `AI gaming APAC 2026 China Japan Korea adoption regulation NPC` | 🟢 **Una cifra nueva y un conflicto de instrumento.** CADPA (vía cobertura de ChinaJoy 2026) mide **86,36%** de empresas chinas usando AI, contra el **~60%** de Niko Partners que esta KB publica. **No es un desacuerdo: son dos denominadores** (CADPA encuesta a sus asociadas, Niko estima el universo de estudios) ⇒ **llevar las dos o ninguna**, igual que con Steam. Y KOCCA aporta un tercer punto temporal: **41,7% en H1-2025** frente al **77,5% del Q2-2026** que esta KB ya tenía ⇒ **la serie coreana casi se duplicó en un año**, y es la única serie temporal de adopción por región que esta KB puede construir |
| **LATAM** | `AI videojuegos LATAM Brasil Argentina México 2026 adopción regulación` | 🟡 **Confirmaciones, un dato de denominador y un gap que no se mueve.** México: **>76 M** jugadores (Endeavor) junto al 72,6 M de Xsolla ⇒ dos instrumentos, misma magnitud. **Regulación de IA en videojuegos en Brasil, Argentina y México: cero instrumentos encontrados**, cuarto pass consecutivo. ⚠️ **Y una advertencia de higiene que esta KB debe mantener:** la cifra de **97%** de devs LATAM usando IA sale del *Google Cloud Games Report* **vía Globant** — es nuestra propia casa citándose. Se publica **marcada como tal**, nunca como dato independiente |

### ⛔ Gaps de este pass, declarados

- **Japón: cero.** La consulta de APAC no devolvió ni una fuente sobre reglas
  japonesas de IA en videojuegos. Esta KB tiene el *AI Basic Plan* del 14-jul-2026 y
  el pronunciamiento de CESA sobre propiedad intelectual, y **nada más**. No es que
  Japón no regule: es que **no se encontró con esta consulta**.
- **Política de modelos open source en gaming, por región: cero.** Buscado
  explícitamente en APAC. Ninguna jurisdicción aparece legislando sobre *qué modelos
  puede usar un juego*, sólo sobre *qué hay que declarar*.
- **Repos de gaming AI de origen LATAM: cero**, cuarto pass consecutivo, buscado en
  español. El canal 6 no cambia esto — enumera árboles de repos que ya existen.
- **Licencia de los pesos: sigue sin canal** (`huggingface.co` → 000). Dada la
  tendencia 34 (*lo no comercial vive en la periferia: pesos, benchmark, dataset*),
  **éste es ahora el gap de mayor riesgo comercial de esta KB**, en las cuatro
  regiones por igual.


## 🔴 Lo que cambió — pass cycle 17 (2026-11-22): **la capa de fábrica tiene una geografía propia, y no es la del resto de esta KB** — más la resolución del conflicto de Steam que este archivo dejó abierto

Los 16 passes anteriores mapearon este mercado por **regulación** (quién obliga a
etiquetar, a verificar edad, a marcar contenido). Este pass mapea otra cosa: **de
dónde sale el software con el que se fabrica un juego**, y el mapa no coincide.

### 🟢 1. Resolución del conflicto de Steam que el cycle 16 registró como NO resuelto

El cycle 16 publicó: *"49,9% (oct-2026) contra 30,8% de los lanzamientos de 2026 …
no se pueden promediar … no citar ninguno sin decir el denominador"*. **El
denominador está medido y los dos números son compatibles:**

| Cifra | Ventana | Qué cuenta |
|---|---|---|
| **1,7%** | enero 2024 | lanzamientos **del mes** (primer mes de la obligación) |
| **10,9%** | año 2024 | lanzamientos **del año** |
| **19,9% / ~24%** | año 2025 | lanzamientos **del año** |
| **26,4%** | octubre 2025 | lanzamientos **del mes** |
| **30,8%** | 2026 **hasta julio** | lanzamientos del año **a esa fecha** |
| **~40,1%** | 2026 hasta la fecha | lanzamientos del año **acumulado** |
| **49,9%** | **octubre 2026** | lanzamientos **del mes** |

⇒ **No hay contradicción: hay una tasa mensual que sube dentro del año.** El
acumulado anual (40,1%) es siempre menor que el mes más reciente (49,9%) **porque la
serie es creciente**. La forma correcta de citarlo: *"casi la mitad de los
lanzamientos de octubre de 2026 y ~40% del año acumulado"*. **Conflicto cerrado.**

### 🔵 2. La geografía de la capa de fábrica, medida sobre 22 repos

| Región de origen | Repos de esta capa | Licencia dominante |
|---|---|---|
| **EMEA** | AYON (Ynput, **Chequia**), Kitsu/Zou (CGWire, **Francia**), AltTester (Altom, **Rumanía**), plugin UE de TeamCity (JetBrains), aplib.net (**Países Bajos**) | 🔴 **FSL-1.1, AGPL-3.0, GPL-3.0** — la restrictiva |
| **APAC** | Airtest + Poco (**NetEase, China**) | 🟢 **Apache-2.0** |
| **North America** | git-lfs (GitHub), sccache (Mozilla), DVC/lakeFS (Treeverse), autopilot (Ramen), la arquitectura de referencia de AWS | 🟢 MIT/Apache **salvo** lakeFS (BSL) y la referencia de AWS (GPL + suscripción) |
| **Global / comunidad** | game-ci | 🟢 MIT |
| **LATAM** | **0 de 22** | — |

🔴 **El dato que no estaba en ningún pass anterior: Europa produce la capa de
pipeline de esta industria y la licencia a la defensiva.** Las dos plataformas de
seguimiento de producción del mundo open source son europeas y **ninguna de las dos
está en la ruta permisiva** (AYON parte el grant desde el 2025-01-27, Kitsu es AGPL
desde 2017). No es casualidad de licencia: son empresas chicas que viven del
producto, y la licencia es su modelo de negocio.

🟢 **Y el espejo: la única automatización de QA de juego permisiva y a escala es
china.** `AirtestProject/Airtest` (9.657★, Apache-2.0, NetEase) no tiene equivalente
occidental permisivo: el occidental equivalente es AltTester y es GPL-3.0 con
suscripción de pago para el modo batch.

### Datos nuevos por región — este pass

#### North America

- 🔴 **La arquitectura de referencia de AWS para QA con agentes no es usable tal
  como se publica**: embebe `AltTester-Unity-SDK` (**GPL-3.0**, 35.043 B medidos)
  dentro del build del cliente, el server en *batchmode* sobre Fargate **requiere
  suscripción Pro** (lo dice AWS), y **el repo prometido no está publicado**.
  ⇒ En una conversación con un cliente que llega citando ese blog, el argumento
  no es técnico: es que **la alternativa Apache-2.0 ya existe y cubre 4 engines**
  (Receta 19). ⚠️ *Fuente secundaria: `aws.amazon.com` no resuelve desde esta sesión.*
- 🔴 **Lock-in de pipeline en Unreal:** Horde, el sistema de build de Epic, publica
  fuente **bajo la EULA de UE** y su CI **no soporta Git ni Subversion — sólo
  Perforce, UE 5.4+**. Para un cliente AAA norteamericano esto es presupuesto, no
  preferencia. ⚠️ *Fuente secundaria: `dev.epicgames.com` no resuelve desde esta sesión.*
- 🟡 **Tercera reconfirmación de la frontera del cycle 8:** la consulta regional del
  runbook (`AI gaming North America 2026 adoption regulation`) **vuelve a devolver el
  informe UNLV/KPMG "State of AI in Gaming 2026" en primer lugar, y es de casino**
  (índice de madurez 45/100, foco en protección del jugador). Sigue sin ser de
  videojuegos. **La frontera no es una curiosidad: es el resultado nº1 de la consulta
  obligatoria, tres passes seguidos.**

#### EMEA

- 🔴 **Las dos plataformas de producción de esta industria son europeas y las dos
  salieron de la ruta permisiva** (ver el mapa de arriba). Consecuencia comercial
  concreta: **en EMEA la conversación de pipeline empieza por licencia, no por
  funcionalidad**, y el cliente europeo suele tener ya una de las dos instalada.
- 🟢 **El argumento de soberanía se refuerza, no se debilita:** AYON y Kitsu se
  despliegan on-prem por diseño; el competidor (ShotGrid/ftrack) es SaaS de un
  proveedor norteamericano. Para un estudio europeo con datos de producción bajo
  NDA, eso es el argumento, y es de ellos, no de Globant.

#### APAC

- 🟢 **La capa de QA de UI de juego es APAC-origen y permisiva**: Airtest (9.657★) +
  Poco (1.964★), ambos Apache-2.0 desde el commit inicial (verificado por canal 5),
  ambos de NetEase. ⚠️ **Poco lleva 34 meses sin commit y Airtest 8**: usar como
  librería estable, no como dependencia con roadmap.
- 🟡 **Corrobora el aflojamiento de aprobaciones que el cycle 13 registró en China:**
  las aprobaciones de juegos acumulan **+19% interanual** (Niko Partners, fuente
  secundaria). Es la tercera fuente independiente en la misma dirección.

#### LATAM

- 🆕 **Brasil tiene reglamento del Marco Legal, y esta KB no lo tenía:** el
  **Decreto nº 13.138/2026, firmado el 14-oct-2026**, reglamenta la **Lei nº
  14.852/2024** (Marco Legal da Indústria de Jogos Eletrônicos), con foco declarado
  en desarrollo de la industria, innovación, formación profesional e
  internacionalización, y con **desarrollo de juegos —co-desarrollo y QA— listado
  entre las áreas abiertas a empresas extranjeras**. ⚠️ *Fuente secundaria:
  `www.trade.gov` y `www.planalto.gov.br` no resuelven desde esta sesión; el número
  de decreto y la fecha se publican como dato a confirmar contra el DOU en el
  próximo pass.*
  ⇒ **Por qué importa para Globant y no es trivia regulatoria:** es el primer
  instrumento de la región que **nombra QA y co-desarrollo** como el carril de
  entrada, que es exactamente la capa que este pass acaba de mapear (Recetas 19 y 43).
- 🔴 **Cero medido, no silencio:** de los **22 repos** de la capa de fábrica
  verificados este pass, **ninguno tiene dueño LATAM**. La capa que el decreto
  brasileño incentiva **no se produce en la región**: se adopta. Para una propuesta
  eso es favorable —no hay competidor local de producto— y para el discurso de
  ecosistema es el gap que hay que nombrar.
- 🟡 **Tamaño, de este pass:** Brasil sigue siendo el mercado mayor y más maduro de la
  región, con penetración declarada **>70% de la población** y **más de 1.000
  desarrolladores activos**; estudios de Argentina, Chile y Colombia aportan cada vez
  más a lanzamientos internacionales (Xsolla/GamesBeat, fuente de industria).

---

## 🔴 Lo que cambió — pass cycle 15 (2026-11-22): **la primera obligación de este mercado que YA se está litigando — y no es de IA**

Los catorce passes anteriores de esta página siguieron la regulación **de la
IA**: quién obliga a etiquetar contenido generado, quién obliga a verificar la
edad, quién prohíbe qué práctica. Este pass mide un eje **anterior, vigente y
ya accionado en tribunales**: la **accesibilidad del producto**.

Importa por tres motivos que se refuerzan:

1. **Ya está en vigor en dos regiones.** El **CVAA** obliga en EE. UU. desde el
   **1-ene-2019** para el chat de los juegos; el **EAA** europeo es aplicable
   desde el **28-jun-2025**. No hay cuenta regresiva que vender: hay un
   incumplimiento que auditar.
2. **Ya se litigó.** Las **primeras demandas bajo el EAA se presentaron en
   Francia en nov-2025**, y Países Bajos y otros Estados miembro planificaron
   actividad de fiscalización para 2026. Es la primera obligación de esta KB con
   jurisprudencia en curso, no con fecha futura.
3. **No se puede comprar la solución.** Como mide `agents/top.md` en este mismo
   pass, la capa operativa de cara al jugador —accesibilidad, anti-cheat,
   soporte, moderación de voz— **no tiene oferta open source viva**. La función
   de accesibilidad se mudó **dentro del motor** (Godot 4.5 vía AccessKit) y los
   plugins que recomienda cualquier buscador llevan entre 3 y 5 años sin un commit.

> **El cruce que hace vendible esto:** el CVAA pide que el **chat de voz tenga
> equivalente en texto** y viceversa. La moderación de voz —que esta KB midió
> este pass como oferta **cero**— necesita exactamente lo mismo: transcribir el
> audio. **Un solo componente satisface una obligación legal y una necesidad de
> *trust & safety*.** Ése es el argumento, y no es de compliance: es de coste.

### 🟡 Corrección de registro: la adopción de IA en desarrollo tiene DOS cifras en circulación y no coinciden

| Cifra | Fuente citada | Cómo tratarla |
|---|---|---|
| **~90%** de los desarrolladores usa IA en algún punto del pipeline | Google Cloud, vía GamesBeat | Secundaria, redondeada |
| **97%** de los desarrolladores usa IA para escenarios, *playtesting* y **localización** | *Google Cloud Games Report 2025*, vía Globant | Secundaria, y **la cita es de un competidor directo en el mismo mercado** |

Ambas remiten al mismo origen (Google Cloud) con números distintos y años
distintos. ⇒ **No citar "97%" en una propuesta.** Si hace falta una cifra de
adopción, usar *"en torno al 90%, según Google Cloud vía fuentes secundarias"* y
poner el peso del argumento en el **desglose por tarea** que ya registra el
cycle 12, no en el total.

## 🔴 Lo que cambió — pass cycle 14 (2026-11-22): **la primera restricción de este mercado que se decide por JURISDICCIÓN DEL LICENCIANTE, no del cliente**

Los passes anteriores de esta página siguieron la regulación: quién obliga a
etiquetar, quién obliga a verificar edad, quién prohíbe qué. Este pass mide un
eje distinto y **anterior a la regulación**: en la capa de producción de assets,
**la licencia del software decide en qué región se puede vender el resultado** —
y lo decide antes de que ningún regulador opine.

### 🔵 1. El mapa de la capa de assets, puesto por región — y no es simétrico

| Opción de generación 3D | Licenciante | NA | EMEA | APAC | LATAM |
|---|---|---|---|---|---|
| `microsoft/TRELLIS` + `TRELLIS.2` (27.514 ★) — **generación** | Microsoft (NA) | ✅ MIT | ✅ MIT | ✅ MIT | ✅ MIT |
| …sus **renderers** (`nvdiffrast`, `nvdiffrec`) | **NVIDIA** (NA) | ⛔ no comercial | ⛔ | ⛔ | ⛔ |
| …su **renderer de splatting** (`diffoctreerast` ← `diff-gaussian-rasterization`) | **Inria + MPII** (FR/DE, **EMEA**) | ⛔ no comercial | ⛔ | ⛔ | ⛔ |
| `Tencent-Hunyuan/Hunyuan3D-2.1` (4.519 ★) | Tencent (APAC) | ✅ | ⛔ **sin grant** | ⛔ **sin grant en Corea** / ✅ resto | ✅ |
| `Bingeljell/image-to-3dlab`, `OpenDCAI/GameFactory-3A` | — | ✅ Apache-2.0 | ✅ | ✅ | ✅ |

**Las dos lecturas que salen del cuadro:**

1. 🔴 **EMEA es la región con menos opciones de esta capa, y por dos causas
   independientes que se suman.** La alternativa china **no tiene grant** en la
   UE ni el Reino Unido (exclusión territorial explícita de Tencent), y el
   renderer de splatting que el líder MIT necesita es de **Inria + MPII**, con
   prohibición comercial literal — una restricción **de origen europeo**. Lo que
   queda en pie para un engagement europeo es el stack Apache/MIT puro:
   `image-to-3dlab`, `GameFactory-3A`, `MOSS-TTS`, `ACE-Step`.
2. 🟡 **Corea del Sur recibe el golpe dos veces.** Queda fuera del grant de
   Tencent **y** es la jurisdicción de APAC con la ley de AI más amplia (AI
   Basic Act, vigente **22-ene-2026**).

### 🔵 2. Dónde está la adopción de esta capa — y por qué el número importa para el pitch

Resúmenes secundarios de **GDC 2026** (2.300+ respondentes; **el informe
primario no se recuperó en este pass y se declara**): la **generación de assets**
es ~**19%** del uso reportado de AI, contra ~**81%** en investigación y
brainstorming. Cifras de adopción total que **discrepan por un factor de 2,4×**
porque miden cosas distintas: 36% (uso personal), 52% (estudios con genAI en
producción), ~50% (BCG), 73% (a16z Games AI Survey), 88% (expectativa de que sea
estándar).

⇒ **El pitch correcto no es "todos ya lo usan".** Es: *"casi nadie lo usa
todavía en producción, y las licencias de las cuatro herramientas que vas a
elegir ya se están cerrando"*. Ninguna de estas cifras se promueve a número de
referencia de esta KB.

### 🟡 3. Carril casino — se cita, no se promueve

**Malta, 18-sep-2026:** MGA + MDIA publicaron el *AI Gaming Charter*,
**voluntario**, 48 páginas, seis áreas de guía, con clasificación de
herramientas por impacto en el jugador; consulta previa el 8-may-2026;
seguimiento de nov-2026 reporta brechas de supervisión entre licenciatarios.
⚠️ **Es gambling, no videojuegos** — frontera establecida en el cycle 8 y
revalidada por quinta vez. Lo único transferible es la **forma del entregable**
(inventario de herramientas AI clasificadas por impacto en el jugador), que es
el mismo artefacto que pide el Art. 50 del AI Act.

### Datos nuevos por región — este pass

#### North America

- 🔴 **Los dos licenciantes que bloquean la capa de assets son norteamericanos y
  tienen nombre.** `nvdiffrast` y `nvdiffrec` son de **NVIDIA**, bajo *"NVIDIA
  Source Code License"* — y la variante de `nvdiffrast` se llama literalmente
  **"1-Way Commercial"**, con la restricción escrita: *"only may be used or
  intended for use non-commercially … research or evaluation purposes only"*.
  ⇒ a diferencia de una exclusión territorial, **acá hay una contraparte
  corporativa identificable** a la que un cliente norteamericano puede
  plantearle una licencia comercial por los canales normales. **Esta KB no
  afirma que NVIDIA la otorgue**: afirma que existe el interlocutor, que es más
  de lo que hay en los otros bloqueos de este pass.
- Estimaciones de agregadores secundarios ubican a NA en ~**40%** del ingreso de
  AI-en-gaming. ⚠️ Fuentes terciarias con metodologías distintas: **no se
  promueve a cifra de referencia**.
- ⚠️ **El homónimo de casino volvió a dominar la consulta regional**, por sexto
  pass consecutivo. `AI gaming North America 2026 adoption regulation players`
  devuelve KPMG, SIGMA y AiR HUB — los tres de **gambling**. Sus datos (Alberta
  publicó en ene-2026 reglas para detectar software automatizado usado por
  jugadores; Colorado enmendó sus reglas de juego responsable el mismo mes;
  >80% de operadores usando genAI para contenido e insights, con adopción
  **agéntica limitada** por compliance) se registran **como mercado adyacente**.

#### EMEA

- 🔴 **La restricción comercial de origen europeo.** La *Gaussian-Splatting
  License* es de **Inria** (Francia) y el **Max Planck Institut für Informatik**
  (Alemania), con el software *"in the process of being registered with the
  Agence pour la Protection des Programmes (APP)"*. Texto literal: *"THE USER
  CANNOT USE, EXPLOIT OR DISTRIBUTE THE SOFTWARE FOR COMMERCIAL PURPOSES"*. Es
  el renderer sobre el que se construyó buena parte de la generación 3D abierta
  de los últimos dos años. ⇒ **el bloqueo no viene sólo de fuera de Europa.**
- ⛔ **La alternativa china no existe en EMEA.** `Hunyuan3D-2.1` (4.519 ★) suma
  el cuarto repo de Tencent sin grant en **UE y Reino Unido**. Para un
  engagement europeo no es "una licencia restrictiva": es **ausencia de
  licencia**, ni siquiera para evaluar.
- 🔵 **Malta, 18-sep-2026:** AI Gaming Charter voluntario (MGA + MDIA). Carril
  casino — ver arriba.
- 🔴 **EMEA no es un mercado, y esto se puede medir.** El informe *State of AI in
  Gaming 2026* (AiR HUB) registra actividad regulatoria sobre AI concentrada en
  **Norteamérica y Europa occidental**, y **ninguna** en **Europa Central y del
  Este** ni en **Medio Oriente y África**. ⇒ el gancho regulatorio que funciona
  en París o Estocolmo **no existe** en Varsovia, Bucarest, Dubái o
  Johannesburgo, y vender el mismo entregable ahí es vender aire. Para esa mitad
  de EMEA el argumento tiene que ser **coste y pipeline**, no compliance.
- 🔵 **Reconciliación de fechas del Art. 50, porque dos fuentes de este pass
  parecían contradecir a esta KB.** Un análisis de estudio jurídico (WH
  Partners) fecha la entrada en vigor del Art. 50 el **2-ago-2026**; esta página
  viene trabajando con el **2-dic-2026**. **No es una contradicción:** son las
  dos patas del mismo artículo — obligación para sistemas nuevos desde agosto,
  y plazo para los **ya puestos en el mercado** en diciembre. Las recetas 7, 13
  y 18 están dimensionadas para la segunda, que es la que le aplica a un
  catálogo existente. **Sin corrección.**

#### APAC

- ⛔ **Corea del Sur queda fuera del grant de Tencent** (`Hunyuan3D-2.1`, 4.519 ★
  — cuarto repo con la misma cláusula). Un estudio coreano no puede usar la
  línea abierta de Tencent **ni para evaluar**.
- 🔵 **Corea — AI Basic Act**, vigente **22-ene-2026**: transparencia y
  supervisión humana para sistemas de alto impacto. Dato nuevo de este pass:
  un período de gracia de **al menos un año antes de que empiecen las multas**,
  con tope reportado en **KRW 30 M**. ⚠️ Fuentes de compliance-vendor, no texto
  oficial: **verificar antes de poner una fecha en una propuesta**.
- 🔵 **China:** la enmienda a la Ley de Ciberseguridad agregó una cláusula de
  gobernanza de AI en **ene-2026**, con multas de hasta **RMB 10 M**, sobre el
  régimen ya existente de **registro obligatorio de servicios generativos y
  etiquetado de contenido**. ⇒ **el etiquetado cae exactamente sobre esta capa**:
  si el asset final que ve el jugador lo generó un modelo, hay obligación de
  etiqueta. Es el único régimen del mundo donde la capa de producción de assets,
  y no la de NPCs, es el centro de la obligación.
- 🔵 **Japón:** enfoque liviano — *AI Promotion Act* **sin sanciones directas**,
  guías voluntarias, y un **AI Basic Plan adoptado en jul-2026**.
- 🔵 Señales de producto: **Capcom** negó públicamente usar AI generativa en
  contenido de juego; la desarrolladora de **PUBG** anunció un modo de compañero
  AI para 2026. ⚠️ Vía resumen de industria, sin confirmación primaria.

#### LATAM

- 🔴 **Cero medido, no silencio: la consulta regional de videojuegos no existe
  como tal.** `AI gaming LATAM Latin America 2026 adoption regulation players
  Brazil` devolvió **sólo apuestas e iGaming** — ni un resultado de videojuegos.
  Esto ya es un hallazgo en sí: **para LATAM, el buscador no distingue "gaming"
  de "gambling" en absoluto**, mientras que en NA y EMEA al menos devuelve
  ambos. La frontera del cycle 8 es **más cara de sostener en LATAM** que en
  ninguna otra región.
- 🔵 Lo que sí devolvió, registrado **como mercado adyacente**: Brasil opera
  bajo la **Ley 14.790/2023** (apuestas deportivas dic-2023, casino online fines
  de 2024) y la **SPA pasó de recomendar a sancionar desde el 1-ene-2026**;
  las *selfies* estáticas ya **no satisfacen los estándares PAD** (Presentation
  Attack Detection) del regulador, explícitamente planteado como defensa contra
  deepfakes generativos. Perú combina transmisión certificada de datos con una
  dirección supervisora propia. ⚠️ Reuters reportó en sep-2026 una posible
  medida federal brasileña que prohibiría el casino online conservando las
  apuestas deportivas — **sin segunda fuente, no se da por hecho**.
- 📏 **Presencia LATAM en la capa de assets: medida, con el instrumento corto en
  español.** Tres consultas en español sobre generación de assets, música y
  rigging devolvieron **`total_count: 0`** cada una. Una cuarta devolvió
  exactamente **un** repo:

  | Repo | ★ | Licencia (byte-read) | Push | Qué es |
  |---|---|---|---|---|
  | [facundoAlmon/AssetCrisol](https://github.com/facundoAlmon/AssetCrisol) | **1** | **GPL-3.0** (35.149 B — coincide con el checksum de la casa) | 2026-11-13 | *"De una imagen a assets de juego 2D y 3D, con IA local y open source"*; salida a **sprites, `.glb` y escenas de Godot 4**; Blender 5.1; Linux + NVIDIA 16 GB |

  ⚠️ **Límite del instrumento, declarado:** "repo en español" **no es** "repo de
  origen LATAM" — los canales de esta sesión (`search/*` + `raw`) **no permiten
  medir el país del dueño**. Lo que se afirma es lo medido: **1 repo
  hispanohablante, vivo, de 1 ★ y GPL-3.0**, contra 14 filas nuevas en el resto
  del mundo. El ecosistema de producción de assets **no tiene hoy una pieza
  LATAM sobre la que construir**, y decirlo con el número es más útil que no
  decir nada.
- 🟢 **El dato estructural de LATAM de esta KB sigue siendo el del cycle 9** (la
  aceptación de AI por parte de jugadores, con su corrección de muestra). Este
  pass **no lo toca** y no agrega un argumento LATAM que no haya medido.


## 🔴 Lo que cambió — pass cycle 13 (2026-11-22): **la regulación de 2026 dejó de pedir una etiqueta y pasó a pedir una CATEGORÍA DE EDAD por usuario — y el mismo dato tiene tres ciclos de vida incompatibles**

Los passes anteriores de esta página siguieron la transparencia (Art. 50,
disclosure de Steam, etiquetado chino). Este pass mide un desplazamiento: **el
instrumento con más fuerza ejecutoria hoy en gaming no es el de etiquetado de AI:
es el de verificación de edad**, y está vigente y exigible en Estados Unidos
desde junio de 2026.

### 🔵 1. El dato de edad: un solo campo, tres regímenes que piden cosas distintas

| Jurisdicción | Qué exige | Qué decide la edad | Retención del dato |
|--------------|-----------|--------------------|--------------------|
| **EU** (AI Act Art. 50) | etiquetar contenido generado por AI | **si hay obligación** (según la audiencia declarada del título, PEGI) | — (es clasificación de producto, no dato de usuario) |
| **Connecticut** (PA 26-15) | recordatorio de que se habla con una AI | **con qué frecuencia** cumplir (≥1/hora menores, ≥1/3 h adultos) | implica **conocer y conservar** la edad del usuario en sesión |
| **Texas** (SB 2420) | la tienda clasifica al usuario y el desarrollador debe usar esa clasificación | **si la descarga y la compra están permitidas** | 🔴 **obliga a BORRAR el dato de edad después de verificar** |

> 🔴 **La contradicción es operativa, no teórica: Connecticut empuja a conservar
> la edad del usuario para poder cumplir una cadencia; Texas obliga a borrarla
> después de verificar.** Un build que guarda `user.age` para satisfacer a CT
> incumple la obligación de borrado de TX. La salida está en el diseño, no en el
> abogado: conservar **la categoría derivada mínima** y el **reloj**, no la edad.
> Ver `compose/patterns.md`, **Receta 21**.

### 🔵 2. Qué dice exactamente SB 2420, porque cambia quién hace el trabajo

- **Vigente 1-ene-2026; exigible desde el 4-jun-2026.** El Quinto Circuito
  levantó la medida cautelar y la Corte Suprema dejó que Texas siga ejecutándola
  mientras avanza la apelación. ⇒ **no es derecho futuro: está en ejecución.**
- **Cuatro categorías** que fija la tienda: menor de 13; 13-15; 16-17; adulto.
- **Obligación del desarrollador** (no de la tienda): usar la categoría que la
  tienda le entrega, verificarla, y confirmar que hay consentimiento parental
  cuando corresponde. **Los padres aprueban cada descarga y cada compra in-app.**
- **Borrado** de la información de edad después de la verificación.
- **Puerto seguro** si se siguen estándares de industria ampliamente adoptados de
  buena fe.
- Violaciones tratadas como **prácticas comerciales engañosas** bajo la ley de
  Texas.
- **Estados con leyes de age assurance a nivel tienda/dispositivo: Texas, Utah,
  Louisiana, Alabama y California.**

### 🔵 3. El precedente de producto: Roblox

Desde **enero de 2026**, Roblox exige **verificación de edad para acceder al chat
a todos sus usuarios, globalmente** (estimación facial; los de 13+ pueden usar
documento). Las demandas de los fiscales generales de **Texas y Louisiana**
precedieron al cambio. Roblox además integró una coalición de seguridad infantil
con la **Attorney General Alliance**.

⇒ **El patrón para vender:** la obligación legal llega como requisito de tienda y
de fiscalía estatal, y la respuesta de la plataforma más grande del segmento fue
**poner verificación de edad delante de la comunicación, no del juego**. Ese es
el diseño de referencia: se restringe el **canal social**, no el acceso al
producto.

### 🟡 4. Corrección a esta KB: el *throttle* de aprobaciones de China se está AFLOJANDO

Esta página venía describiendo el régimen de la NPPA como un **cuello de botella
de aprobaciones**. La medición de este pass dice lo contrario: **las aprobaciones
de juegos subieron ~19% interanual en 2026 frente a 2025**, lo que se lee como
normalización regulatoria. **El lead time sigue existiendo** —y hay que meterlo en
el cronograma igual—, pero la dirección del indicador cambió de signo. Se corrige.

### 📏 5. Tres fuentes independientes dicen lo mismo sobre DÓNDE está la adopción

| Fuente | Medición | Dice |
|--------|----------|------|
| KPMG (Scott Purdy), predicciones 2026 | cualitativa | las integraciones más profundas serán **en el back end**, no en contenido generado de cara al jugador |
| Bird & Bird, *The Future of Play* 2026 | cualitativa, EMEA | la adopción de genAI es más fértil en **prototipado temprano y concept art** |
| Steam, disclosures (medición propia de esta KB) | censo, NA | 49,9% de los títulos declaran AI, pero **sólo ~3% es generación dentro del gameplay** |

⇒ **Tercera confirmación independiente, y la primera que viene de un censo y no de
una opinión.** El discurso comercial que se sostiene con datos es
**"AI en el pipeline"**, no "AI en la pantalla". Lo contrario es exactamente lo
que dispara el *backlash* que Omdia lista entre las tendencias de 2026.

### Datos nuevos por región — este pass

#### North America

- **Texas SB 2420** vigente 1-ene-2026, **exigible desde 4-jun-2026**; Quinto
  Circuito levantó la cautelar, la Corte Suprema permitió que la ejecución siga.
  Cuatro categorías de edad; deber de **borrado**; puerto seguro por estándares
  de industria; sanción por prácticas engañosas.
- **Cinco estados** con leyes de age assurance a nivel tienda o dispositivo:
  **TX, UT, LA, AL, CA**.
- **Roblox**: verificación de edad obligatoria para chat, global, desde ene-2026;
  precedida por demandas de los AG de **TX y LA**; coalición de seguridad infantil
  con la **Attorney General Alliance**.
- ⚠️ **El homónimo volvió a dominar la consulta regional.** `AI gaming North
  America 2026 adoption regulation players` devuelve, en los primeros puestos, el
  informe **KPMG/UNLV *State of AI in Gaming 2026*** — que es de **casino**, no de
  videojuegos (frontera establecida en el cycle 8 y **revalidada hoy**). Su dato
  —índice de madurez de AI promedio **45/100**, regulación concentrada en NA y
  Europa occidental, escepticismo regulatorio hacia la autorregulación— **se cita
  como mercado adyacente, no como gaming**.
- Encuesta de proveedor (declarada como tal): **94% de los estudios** usan AI de
  alguna forma, sobre una muestra de 50+ líderes. Sesgo de proveedor y muestra
  chica ⇒ **no se promueve a cifra de referencia**.

#### EMEA

- ⏱️ **Art. 50: faltan 10 días** (2-dic-2026) para los sistemas ya en mercado.
  Las recetas 7, 13 y 18 están dimensionadas para ese plazo.
- **EGDF** fija la agenda 2026 en tres ejes: **AI dirigida por humanos**, mercados
  de plataforma más justos y **soberanía tecnológica europea**. Posición explícita:
  la AI asiste al desarrollador, no reemplaza el control creativo. Malestar
  declarado dentro de la comunidad de desarrollo por el *"AI slop"*.
- **Video Games Europe**: sector de **€24.500 M**, **53% de la población** juega.
  Posición: enfoque regulatorio equilibrado que proteja a los creadores humanos.
- **Bird & Bird**: más regulación **y más fiscalización**; adopción de genAI
  concentrada en **prototipado y concept art**.
- **Linklaters**: el escrutinio regulatorio europeo está impulsado sobre todo por
  **seguridad infantil, privacidad de datos de menores y explotación financiera**
  — los mismos tres ejes de la ola norteamericana. ⇒ **la inversión en la capa de
  edad + moderación sirve en las dos jurisdicciones**; la de etiquetado, sólo en
  una.
- **Cuarta corroboración** del aplazamiento post-Omnibus: acuerdo provisional
  Parlamento-Consejo de **mayo de 2026** que posterga los requisitos de alto
  riesgo a **dic-2027 y ago-2028**. El Art. 50 **no** se mueve.

#### APAC

- **China**: ~**60% de los estudios** chinos ya usan AI en su pipeline (Niko
  Partners) — generación de contenido, comportamiento de NPC y live ops. Reglas de
  **etiquetado de contenido generado por AI vigentes desde septiembre de 2025**.
  **Aprobaciones de juegos +19% interanual en 2026** ⇒ normalización, no throttle
  (corrige a esta KB).
- **Japón**: **AI Promotion Act** (mayo de 2025) — ley marco de política nacional,
  **sin penalidades** y sin reglas específicas de gaming.
- **Corea del Sur**: el marco cubre sistemas de alto impacto, **disclosure y
  etiquetado de AI generativa**, y obligaciones para proveedores extranjeros vía
  **representante local** — este último punto es el que le pega a un estudio que
  publica desde fuera.
- **Disclosure obligatorio de contenido generado por AI**: comentario legal
  (Linklaters) señala a **China y Singapur** como los modelos que otras
  jurisdicciones están mirando.
- ⚠️ **Gap declarado:** no se encontró dato 2026 de adopción de AI por parte de
  **jugadores** japoneses o coreanos, ni regulación específica de AI en
  videojuegos en esos dos países más allá de los marcos generales. Se buscó con
  `AI gaming APAC Asia 2026 adoption regulation players China Japan Korea video
  games`.

#### LATAM

- **335 millones+ de jugadores** en la región (Xsolla/GamesBeat 2026), **Brasil el
  mercado más grande**, mobile-first y esports de base. **México: 72,6 millones de
  jugadores = 61% de la población de 6 años o más.**
- **Bain (ago-2026)**: el futuro del gaming en Latinoamérica estará definido por
  **adolescentes, compra directa y personalización con IA**. ⇒ la personalización
  con AI aparece como eje de crecimiento **del lado de la demanda**, que es
  coherente con el 48,8% de aceptación de jugadores que ya mide esta KB.
- **Brasil**: **sandbox regulatorio piloto de IA y protección de datos**, más
  consulta pública de **ANATEL** sobre uso ético de IA en telecomunicaciones. Son
  medidas generales de IA, **no específicas de videojuegos**.
- **PNUD**: Latinoamérica y el Caribe **rezagados en adopción de IA**.
- 📏 **Medición de oferta, no silencio: cero repos de gaming AI de origen
  hispano/lusoparlante con tracción.** Instrumento: consultas en español y
  portugués sobre `search/repositories`, ordenadas por ★:
  `videojuego agente IA NPC` → **total = 0**; `juego IA generativa godot` →
  **total = 0**; `jogo IA agente NPC` → **total = 0**;
  `videojuegos inteligencia artificial` → 128 resultados, **el máximo es 3 ★**
  (ejercicios de curso). ⇒ **La ventaja de LATAM en esta KB es de demanda
  (jugadores, aceptación, volumen), no de oferta (repos, herramientas).** Un
  discurso de "ecosistema open source local" no tiene respaldo medible; uno de
  "mercado de lanzamiento racional para AI de cara al jugador" sí lo tiene.
- ⚠️ **El homónimo también domina acá.** La consulta regional de LATAM devuelve
  casi en su totalidad **iGaming / apuestas** (Ley 14.790 de Brasil, nueva ley de
  juego de México). Medición cruzada de este pass: **el homónimo contamina las
  consultas regionales de NA y LATAM, y NO las de EMEA ni APAC** — útil para saber
  dónde hay que desambiguar la búsqueda y dónde no.

---

## 🔴 Lo que cambió — pass cycle 12 (2026-11-22): **la cifra de adopción que esta KB nunca tuvo desglosada, y una contradicción entre dos búsquedas sobre ACE que se resuelve hacia abajo**

### 🔵 1. La encuesta de Unity 2026 trae el **desglose por tarea** — esta KB tenía el total, no el reparto

Esta KB citaba porcentajes de adopción agregados. La encuesta de Unity de 2026
los parte por **para qué** se usa la AI, y el reparto cambia el discurso de venta:

| Uso | % de desarrolladores |
|---|---|
| Asistencia de **código** | **62%** |
| **Narrativa** y escritura | 44% |
| **Comportamiento de NPC** | 40% |
| *Market research* | 37% |
| **Playtesting automatizado** | 35% |

> 🔴 **Lo que esto obliga a corregir en la sala:** el uso nº1 **no es el NPC**.
> Es el código — exactamente la capa donde viven los packs de skills y el
> tooling MCP que esta KB lista desde el cycle 4. El NPC generativo está
> **22 puntos por debajo** del asistente de código. Vender "AI en gaming"
> empezando por el NPC es empezar por el **tercer** caso de uso, y además por el
> único que arrastra el Art. 50(2), la obligación federal de runtime en NA y el
> rechazo del jugador que esta KB ya mide.
>
> ✅ **La consecuencia comercial es directa:** el primer entregable vendible es
> **pipeline y QA**, no el NPC. Y el playtesting automatizado (35%) es la única
> de las cinco filas **sin exposición regulatoria de cara al jugador**.

Corroboración independiente de la dirección, no del número: la encuesta *State
of the Game Industry* de GDC 2026 (2.300+ respondentes) mide **36%** de uso de
genAI, con una brecha por empleador de **30% en estudios vs 58% en editoras,
soporte y marketing/PR** — es decir, la adopción es más alta **fuera** del
estudio que dentro.

---

### 🟡 2. NVIDIA ACE: **dos búsquedas de este mismo pass se contradicen, y esta KB publica la versión conservadora**

Esta KB cita ACE en 8 archivos. Nunca tuvo una lista de **títulos embarcados**.
Al construirla, las dos pasadas dieron resultados distintos:

| Afirmación | Pasada A (barrido de tendencias) | Pasada B (verificación dirigida) | Qué se publica |
|---|---|---|---|
| ACE *live* en **PUBG: BATTLEGROUNDS** e **inZOI** | ✅ afirmado | 🟡 degradado: NVIDIA describe a **Krafton** *"leveraging AI"* para compañeros interactivos en PUBG e historias personalizadas en inZOI — **no** confirma features de NPC ACE embarcadas | 🟡 **"Krafton usa AI en PUBG e inZOI"**, sin afirmar ACE embarcado |
| ACE *live* en **NARAKA: BLADEPOINT** y **MIR5** | ✅ afirmado | ⛔ **cero resultados** que mencionen ninguno de los dos junto a ACE | ⛔ **No se publica.** Sin corroborar |
| Primer juego embarcado con NPC ACE | — | ✅ **Mecha BREAK**, con un Nemotron-4 4B Instruct corriendo **on-device** | ✅ **Mecha BREAK** |

> 📐 **Regla de método nº16: cuando dos pasadas del mismo pass se contradicen, se
> publica la intersección y se nombra la diferencia.** Una lista de cuatro
> títulos embarcados es un activo comercial potente — y es exactamente por eso
> que no se puede escribir con una sola fuente. Esta KB ya se quemó con el
> informe-faro de "AI in gaming" que resultó ser de casino.

🆕 **Lo que sí es nuevo, verificado y accionable:** **ACE soporta Qwen3-8B**
(SLM open-weight) para despliegue **on-device**, además de la línea propia
Nemotron (Nemotron 3 Nano 4B; en cobertura de 2026-10-28, Nemotron 3.5 Bolt E4B
sobre RTX Spark). Ver `repos/foundations.md`: el repo de Qwen3 **no tiene archivo
de licencia** y declara Apache-2.0 en su README.

---

### Datos nuevos por región — este pass

#### North America

- 🆕 **Connecticut: el calendario de la PA 26-15 se resuelve como FASES, no como
  desacuerdo entre fuentes.** `compose/patterns.md` marcaba *"1-oct-2026 **o**
  1-ene-2027 — las fuentes discrepan"*. Un análisis de la *Harvard Journal of
  Sports & Entertainment Law* (ago-2026) describe la ley como de **entrada en
  vigor por etapas a partir del 1-oct-2026**. 🟢 **Las dos fechas pueden ser
  correctas: son etapas distintas de la misma ley.** La discrepancia señalada
  era de lectura, no de fuentes.
- 🔴 **Y la pregunta abierta que importa más que la fecha:** el mismo análisis
  deja explícito que **no está resuelto si un sistema de AI *dentro de un juego*
  cae en el alcance** de los estatutos de *companion chatbot*, y advierte que
  **no todas las leyes estatales de chatbot incluyen exclusión para
  videojuegos.** Para un NPC conversacional en un título con jugadores en EE.UU.,
  eso convierte el alcance en un **análisis estado por estado**, no en un
  interruptor.
- 🆕 **Privacidad biométrica como segundo frente, independiente de la AI:** el
  análisis de comportamiento de jugador — *eye tracking*, entrada facial,
  monitoreo anti-cheat — puede disparar obligaciones bajo leyes biométricas
  estatales (**Texas, Illinois, Washington**). Es un vector que no pasa por
  ninguna norma de AI y que esta KB sólo tenía mencionado al pasar.
- 🔵 **Carril casino, que esta KB mantiene separado del videojuego:** la
  **New York State Gaming Commission aprobó el 9-oct-2026** una propuesta para
  restringir el uso de AI y sistemas automatizados por parte de licenciatarios
  (prohibiría generar promociones personalizadas o apuestas sugeridas con AI);
  al **10-nov-2026** el aviso formal que abre el período de comentarios **aún no
  se había publicado** en el *NY State Register*. El mismo día, la
  **Massachusetts Gaming Commission** pidió a **todos** los operadores de
  apuestas deportivas del estado que informaran cómo usan AI y ML en marketing.
  ⚠️ **Ninguna de las dos aplica a videojuegos.** Se registran porque siguen
  alimentando el patrón que esta KB midió: *la actividad regulatoria de AI en
  "gaming" en NA está concentrada en el carril casino*, y por eso los informes
  que dominan las búsquedas no sirven para un cliente de videojuegos.
- ⛔ **Gap medido, no silenciado:** **no se encontró ninguna cifra de adopción de
  AI por estudios o jugadores específica de Norteamérica para 2026.** El informe
  *State of AI in Gaming 2026* de **KPMG** existe y aparece en los resultados
  **sólo como fragmento**: no se pudo leer y **no se cita**.

#### EMEA

- 🟢 **Cuarta y quinta corroboración del calendario del Art. 50, y una
  reconciliación:** fuentes de este pass sitúan las **Guías finales del Art. 50
  aprobadas el 20-jul-2026**, con las versiones lingüísticas *pendientes de
  adopción formal*. El cycle 11 registró la **adopción formal el 5-nov-2026**.
  🟢 **No hay conflicto: aprobación del contenido en julio, adopción formal de
  las versiones lingüísticas en noviembre.** Las dos fechas describen pasos
  distintos del mismo instrumento, y conviene decirlo así en la sala para no
  parecer impreciso.
- 🟢 Confirmado de nuevo, sexta vez: Art. 50 **aplica desde el 2-ago-2026**, con
  período de gracia hasta el **2-dic-2026** para el marcado de salidas
  generativas en sistemas **ya en el mercado**. La obligación de informar que se
  interactúa con AI **no aplica cuando el uso es evidente**.
- 🆕 **El vector *deepfake*, que esta KB tenía poco desarrollado:** el contenido
  generado que pueda pasar por una **persona, lugar o evento real** cae bajo el
  régimen de marcado, y **alcanza a estudios de fuera de la UE si el contenido se
  distribuye allí**. La Comisión sugiere juicio **caso por caso** según realismo,
  contexto y audiencia — no una regla general. Para un título con parecidos
  faciales, estadios o ciudades reales, es un frente aparte del NPC.
- 🆕 **Y el matiz de copyright que afecta al entregable, no al cumplimiento:**
  donde el contenido es **enteramente generado por AI**, la protección por
  derecho de autor puede ser limitada o inexistente. Un cliente que genera arte
  o narrativa al 100% con AI **puede no poder defender ese activo**. Es un
  argumento a favor del pipeline asistido (humano en el loop) y en contra del
  *fully generative*, y no viene de ninguna norma de AI.
- 🔵 **Las tiendas y plataformas exigen disclosure por contrato**, al margen de
  la ley — para muchos clientes eso llega antes y pega más fuerte que el AI Act.
- ⛔ **Gap medido:** **no se encontró ninguna cifra de adopción de AI por
  estudios europeos** en este barrido. La afirmación del cycle 11 (*"Europa
  integra más profundo que Norteamérica"*, vía el desglose regional de Vivrato)
  **no fue corroborada ni refutada este pass**: queda apoyada en una sola fuente.

#### APAC

- 🔴 **Capcom negó oficialmente haber implementado AI generativa en el contenido
  de sus juegos.** Es el primer **desmentido explícito de un AAA** que registra
  esta KB, y no es un detalle: el barrido describe un patrón japonés de
  **"AI sólo en el backend"** — herramientas internas, pipeline, QA — con el
  contenido de cara al jugador deliberadamente fuera.
  > 🟢 **Para la sala, es la mejor noticia del pass:** el desglose de Unity
  > (código 62% / NPC 40%) y la postura japonesa dicen **lo mismo desde dos
  > lados distintos**. En APAC-Japón, el entregable que se puede vender hoy es
  > **pipeline y QA**; el NPC generativo tiene un incumbente negándolo en
  > público.
- 🆕 **Corea — las dos cifras que le faltaban al AI Basic Act de esta KB:** las
  multas administrativas llegan a **KRW 30 millones** y hay un **período de
  gracia de al menos un año** antes de que empiecen a aplicarse, desde la
  vigencia del **22-ene-2026**. 🔵 Eso reencuadra Corea: la fecha ya pasó, **la
  sanción todavía no muerde**, y la ventana para llegar en regla sigue abierta.
- 🆕 **Corea, producto:** se describen funciones de **compañero AI** llegando al
  **modo arcade de PUBG en el 1er semestre de 2026** — consistente con el
  "Krafton leveraging AI" de la sección de ACE, y con el hecho de que **ninguna
  de las dos fuentes lo llama una feature de ACE**.
- 🆕 **China, escala del régimen:** más de **550 servicios de AI generativa
  registrados ante el regulador** a mediados de 2026, bajo el apilado
  PIPL + CSL + DSL + Medidas de Recomendación Algorítmica + Disposiciones de
  Síntesis Profunda + Medidas de AI Generativa. 🔵 **El filing ante la CAC es un
  requisito de entrada, no un trámite posterior**: para un cliente que publique
  en China, es trabajo de preproducción.
- 🔵 **Japón sigue siendo el extremo blando:** la *AI Promotion Act* **no tiene
  penalidades directas**; se apoya en guías voluntarias y autorregulación
  sectorial. Combinado con el desmentido de Capcom: en Japón **el freno no es
  legal, es reputacional**.
- ⛔ **Gap medido:** **no se encontró ninguna fuente sobre adopción o sentimiento
  de JUGADORES en APAC** en este barrido. La cifra de ingresos de APAC que esta
  KB usa desde el cycle 6 **no fue re-corroborada aquí**.

#### LATAM

- 🆕 **El estudio PGB Latam 2026 queda completo — esta KB usaba 2 de sus 4
  cifras.** Muestra: **7.202 personas** de **Argentina, Chile, Colombia, México y
  Perú**, presentado en **gamescom latam 2026 (São Paulo)**:

  | Cifra | Valor | Estado en esta KB |
  |---|---|---|
  | Podría comprar juegos hechos mayoritariamente con AI | **48,8%** | ✅ ya listada (y ya corregida: **no incluye a Brasil**) |
  | **Lo pensaría antes de comprar** | **25,7%** | 🟡 apenas mencionada |
  | Teme **despidos y precarización** del trabajo creativo | **40,6%** | ✅ ya listada |
  | 🆕 Teme **caída de calidad y productos "sin alma"** | **35,8%** | 🆕 **nueva: cero menciones en 11 passes** |

  > 🔴 **Por qué el 35,8% cambia el guion de venta en LATAM:** esta KB vendía
  > LATAM como *"el mercado de primer lanzamiento racional para AI de cara al
  > jugador"* apoyada en el 48,8%. Con las cuatro cifras juntas, el retrato es
  > otro: **la aceptación es alta y condicionada**. El 48,8% que compraría y el
  > 35,8% que teme un producto "sin alma" **no son grupos opuestos: son la misma
  > población pidiendo calidad**. La oferta correcta no es "LATAM acepta AI": es
  > **"LATAM acepta AI si no se nota en la calidad"** — lo que empuja, otra vez,
  > hacia pipeline/QA antes que hacia contenido generativo visible.

- 🆕 **Volumen regulatorio regional, con instrumento:** el informe *Desafíos
  regulatorios de la IA en Latam* (2ª edición, nov-2025) analiza **193
  iniciativas legislativas en 13 jurisdicciones**, con Argentina, Brasil, Chile,
  Colombia y México entre ellas, y describe la tensión entre **modelo AI-Act
  europeo** y **moratoria regulatoria**, con Chile, Brasil y Uruguay avanzando
  marcos. ⚠️ **El informe no contiene nada específico de videojuegos** — se cita
  como **argumento de volumen**, igual que la KB de enterprise usa los 716
  instrumentos para justificar OSCAL, y **no** como norma sectorial.
- ⛔ **Dos ceros medidos, con el instrumento que los midió — no son silencios:**
  1. **Adopción de AI por estudios latinoamericanos: no se encontró ninguna
     cifra.** Ni número de estudios, ni normas sectoriales. La pista para el
     próximo pass está nombrada: **Abragames** (Brasil) y la propia gamescom
     latam.
  2. **Repos de gaming-AI de origen LATAM: barrido en español**
     (`videojuegos IA agente godot`, ordenado por ★) **devuelve un único repo,
     con 0 ★** (`ricardoalmira89/lumeart`, Apache-2.0, push 2026-11-17).
     🔴 **Cero efectivo, medido con el instrumento corto en español** — el mismo
     método con el que la KB de enterprise midió su cero LATAM. **LATAM consume
     esta tecnología; no la publica.**

---

## 🔴 Lo que cambió — pass cycle 11 (2026-11-22): **las Guías del Art. 50 se adoptaron formalmente el 5-nov-2026, y traen una excepción escrita para el contenido generado EN TIEMPO REAL dentro de un juego**

Esta KB citaba las guías del Art. 50 como *"20-jul-2026, **no vinculantes**"* y
dejó registrado en dos archivos que **la fuente primaria no se pudo leer**
(`ENOTFOUND`). Esa limitación sigue vigente en esta sesión — WebFetch no resuelve
ningún host externo —, así que **todo lo de abajo es 🔵 secundario y hay que
contrastarlo contra el texto oficial antes de firmar**. Dicho eso, lo que las
fuentes secundarias coinciden en decir **cambia la Receta 7**, que es la receta
con reloj.

### 🔵 1. El instrumento tiene número y fecha de adopción formal

| Hito | Fecha | Documento |
|---|---|---|
| La Comisión **aprueba el contenido** de las guías del Art. 50 | **20-jul-2026** | `C(2026) 5054 final` |
| 🆕 La Comisión las **adopta formalmente**, *sin cambios de fondo* | **5-nov-2026** | 🆕 `C(2026) 7702 final` |
| 🆕 **Code of Practice on Transparency of AI-generated Content**, versión final | **10-jun-2026** | Voluntario; exige **≥ 2 técnicas legibles por máquina** |

⇒ La KB tenía la fecha del **borrador aprobado**, no la del **instrumento
adoptado**. Hay 17 días de diferencia y un número de documento que citar.
🔵 Una fuente fecha la publicación de las guías finales el 20-jul y no menciona
la adopción de noviembre: **las fuentes no coinciden**, y hasta leer el original
se citan **las dos fechas**, no una.

### 🟢 2. La excepción que cambia el alcance: **contenido efímero en tiempo real**

Las guías finales **amplían las excepciones** al deber de marcado del Art. 50(2).
La que importa en esta industria está escrita en términos de videojuegos:

> 🔵 El contenido **efímero generado en tiempo real** — *se citan explícitamente
> videojuegos y realidad virtual* — queda **exento del marcado** **sólo si** se
> cumplen **las dos** condiciones: (a) el marcado **no es técnicamente factible**
> y (b) se informa al usuario del origen AI **a nivel de experiencia o de sesión**.

Y la definición de *"técnicamente factible"* que traen las guías:

> 🔵 *Soluciones implementables con la tecnología y los métodos **actualmente
> disponibles**, dentro de **la arquitectura técnica y el entorno operativo
> específicos**.*

🔴 **Por qué esto es el hallazgo del pass y no una nota al pie.** Esta KB ya
había medido que **sólo el 3% de las declaraciones de AI en Steam corresponde a
generación durante el gameplay**. Ese 3% es exactamente el contenido efímero en
tiempo real. ⇒ **el caso más caro de resolver técnicamente — marcar cada línea de
diálogo que un LLM emite en runtime — es justo el que tiene una vía de excepción
escrita.** La conversación con el cliente deja de ser *"hay que marcar todo en 10
días"* y pasa a ser **dos entregables distintos**:

| Contenido | Obligación | Trabajo real |
|---|---|---|
| Assets generados **en pipeline** y embarcados en el build | **Marcado Art. 50(2)** (si es provider y el título es pre-2-ago-2026) | Receta 7: inventario + marcado |
| Contenido generado **en runtime, efímero, no grabado** | 🆕 Posible **excepción**, si se sostiene la infactibilidad **y** se da disclosure de sesión | 🆕 **Memo de factibilidad + disclosure de sesión** — ver Receta 23 |

### 🟢 3. Dos excepciones más, y la que sirve para pipelines de producción

| Excepción 🔵 | Qué dice | Lectura para gaming |
|---|---|---|
| **Pipelines cerrados de producción** | Las salidas usadas en **flujos de producción en circuito cerrado** para cine, animación, **juegos** y publicidad **no se marcan en etapas intermedias**: sólo **la salida final** debe ir marcada y ser detectable | 🟢 **Descarga de trabajo grande.** El arte intermedio, los *concepts*, las variantes descartadas y los pasos de un pipeline de assets **no son puntos de marcado**. La KB vendía inventario sobre todo el pipeline |
| **Productos cerrados / B2B técnico** | Sistemas embebidos en **productos físicos cerrados** cuyas salidas no se pueden exportar quedan fuera; y hay una excepción **B2B** acotada para salidas estrictamente técnicas dirigidas a un conjunto limitado y predefinido de profesionales | ⚠️ **No se aplica sola a un pipeline de estudio.** Ninguna fuente la extiende a assets de videojuego. Se menciona para que nadie la invoque de memoria |
| ⚠️ **El límite técnico declarado** | 🔵 Las guías **no resuelven** que las soluciones robustas de marcado, **en particular para TEXTO**, siguen siendo limitadas | 🟢 **Es el fundamento técnico del memo de infactibilidad** para diálogo de NPC generado por LLM. No es una opinión del proveedor: es una limitación que el propio comentario legal sobre las guías reconoce |

🔴 **Lo que NO cambia, y hay que decirlo en la misma frase:** la excepción es
**del marcado del Art. 50(2)**. El **Art. 50(1)** —avisar al jugador de que
interactúa con un sistema de AI— **no tiene excepción por eficiencia técnica** y
rige **para todos desde el 2-ago-2026**. De hecho la excepción de arriba
**exige** disclosure de sesión como condición: *el aviso no es lo que se ahorra,
es lo que se paga a cambio*.

### 🔴 4. El 94% de Vivrato ahora tiene desglose regional — y Europa está más adelante que Norteamérica

Esta KB ya publicaba *"Europa reporta adopción más profunda que Norteamérica"*
**sin la cifra**. El informe (**VAIA, publicado el 20-oct-2026**) la trae:

| Métrica | Valor | Nota |
|---|---|---|
| Líderes que usaron AI **en desarrollo, de alguna forma** | **97 %** | Es el titular que circula |
| Líderes que la usaron en **≥ 1 de 9 etapas de producción** | **94 %** | Es el dato que esta KB ya tenía |
| Profundidad de integración **≤ 3/10** | **52 %** | La adopción es ancha y fina |
| 🆕 Profundidad media — **Europa** | **4,2 / 10** | |
| 🆕 Profundidad media — **Norteamérica** | **3,5 / 10** | |
| Muestra | **101 líderes senior**, entrevistas estructuradas, **abr–sep 2026**, estudios de EU y NA | No es encuesta online |

🟢 **97% y 94% no son dos versiones del mismo número**: el 97% es *"usó AI en
desarrollo"* y el 94% es *"en al menos una de nueve etapas"*. Citar el 97% como
"94%" o viceversa es citar mal — y la prensa los mezcla.

🔴 **La inversión EU > NA es vendible y contraintuitiva.** El supuesto de sala es
que Norteamérica va delante. **Europa mide 0,7 puntos más de profundidad de
integración** — con el calendario regulatorio más duro del mundo encima. ⇒ en
EMEA **no se vende "empezar con AI"**: se vende *profundizar lo que ya corre*, y
**el argumento de cumplimiento es un acelerador, no un freno**.

### 🔵 5. Ubisoft Motherbrain: el mandato incluye explícitamente **clientes de afuera**

La KB registró Motherbrain (19-nov-2026) como señal de riesgo de marca junto a
los despidos. Las fuentes de este pass agregan **el dato comercial**:

- Alcance declarado: *"servir a creadores **globalmente, dentro del ecosistema de
  Ubisoft y fuera de él**"*, y **formar alianzas con empresas de tecnología
  seleccionadas**.
- Foco técnico: **NPCs inteligentes con memoria** y **AI game masters** que
  gestionan mundos sistémicos a gran escala. Formaliza el equipo del prototipo
  **Teammates** (compañeros de escuadra por voz).
- **Sin producto, sin fecha, sin detalle técnico anunciado.**
- ⚠️ **Conflicto de cifra en los despidos: 116 vs 166.** Esta KB publica **166**.
  Hay fuentes de la misma semana que dicen **116 en dos estudios**. ⇒ **llevar las
  dos** o no llevar ninguna; no hay forma de dirimirlo desde esta sesión.

🟢 **Lectura comercial:** un publisher AAA acaba de declarar que **va a vender
tecnología de NPC a terceros**. Eso lo convierte, a la vez, en **competidor** de
una propuesta de AI Studios y en **socio tecnológico posible**. Es la primera vez
que esta KB puede nombrar a un actor de la industria en las dos casillas.

---

## 🔴 Lo que cambió — pass cycle 10 (2026-11-22): **el Digital Fairness Act dejó de ser un borrador esperado: se publicó el 11-nov-2026, y esta KB lo seguía anunciando a futuro en 5 lugares**

> **Primer pass de un día nuevo.** El cycle 9 corrió el 2026-11-21. En 24 h no se
> mueve una cifra de encuesta; sí se mueven **hechos con fecha**, y este pass trae
> tres que el inventario anterior no tenía: la publicación del DFA, una
> obligación federal estadounidense que ya está en vigor y toca directamente a
> los NPCs con LLM, y la reconciliación de las tres cifras de Steam que esta KB
> venía citando como si fueran la misma.

#### 🔴 Corrección nº1 — regulación europea: el **Digital Fairness Act ya es una propuesta publicada** — COM(2026) 1100, del 11-nov-2026

Esta KB escribía *"propuesta esperada en Q4-2026"*, *"borrador Q4-2026"* y
*"propuesta formal prevista para Q4-2026"* en `intel/market.md` (×4) y
`intel/trends.md` (×2). **El Q4 llegó y el texto salió.** La diferencia no es
cosmética: *esperado* es una conversación de radar, *publicado* es una
conversación de roadmap con articulado citable.

| Campo | Valor medido |
|---|---|
| Fecha de publicación | **11-nov-2026** (una fuente secundaria dice 12-nov; la discrepancia se declara, no se resuelve) |
| Designación | **COM(2026) 1100** |
| Estado | Propuesta de la Comisión. **Todavía tiene que pasar por Parlamento y Consejo** — no es derecho aplicable |
| Entrada en aplicación prevista | **12 meses después de la entrada en vigor**, una vez acordado |
| Sanción máxima | **6% de la facturación anual global** |

**Qué dice el articulado que importa a un estudio:**

| Artículo(s) | Qué regula | Obligación concreta |
|---|---|---|
| **Art. 15** | **Sistemas de recompensa variable de pago**: loot boxes, ruletas, sobres de cartas coleccionables | Publicar **qué ítems hay y con qué probabilidad**; dar al jugador **una herramienta accesible para desactivarlos**; **prohibidos para menores** |
| **Arts. 14–17** | **Diseño adictivo** | El consumidor tiene que poder **apagar** notificaciones push no disparadas por su propia actividad, rachas de engagement y mecánicas equivalentes |
| Interacción con **EU KIDS Act** | Menores | Los juegos online quedarían **exentos de las reglas de diseño adictivo del DFA *para menores***, porque esa materia pasa al KIDS Act. **No es una exención general** |

> ⚠️ **Matiz que hay que llevar a la sala, porque la prensa lo está perdiendo:**
> la prohibición de loot boxes de pago **es para menores**. *No* hay prohibición
> general para jugadores adultos. Varios titulares de nov-2026 dicen lo
> contrario. Un cliente que llegue alarmado por "la UE prohibió las loot boxes"
> está citando un titular, no el texto.

⚠️ **Procedencia:** parte de la cobertura disponible (PocketGamer, Shattered,
Inven Global) se escribió sobre un **borrador filtrado antes de la publicación**
y puede no coincidir con el texto final. El análisis de Pinsent Masons es el
único de la muestra escrito sobre el texto publicado. **Ninguna fuente primaria
pudo leerse en esta sesión** (ver el gap de canal declarado abajo), así que
COM(2026) 1100 queda como referencia a verificar, no como cita verificada.

**Por qué esto cambia la conversación comercial en EMEA.** La KB venía vendiendo
Art. 50(2) del AI Act (transparencia de contenido generado) como el gancho
regulatorio europeo. El DFA agrega un gancho **de monetización**, que es donde
está el P&L del cliente, no su pipeline de arte. Y con **12 meses desde la
entrada en vigor**, el calendario de implementación cae justo en el horizonte de
un engagement que arranque en 2027.

#### 🔴 Corrección nº2 — regulación estadounidense: **ya hay una obligación federal en vigor que rompe el patrón "el NPC llama a un LLM hosteado"** — y esta KB no la tenía

Esta KB declaraba, correctamente, que **no existe** ley federal ni estatal
estadounidense específica de IA en videojuegos. Sigue siendo cierto. Pero existe
una regla federal **general** cuyo efecto sobre juegos infantiles es directo,
está **en vigor desde el 22-abr-2026**, y no aparecía en ninguno de los 8
archivos:

| Campo | Valor |
|---|---|
| Norma | **COPPA Rule enmendada** (FTC) — Federal Register **90 FR 16970** |
| Fecha de cumplimiento obligatorio | **22-abr-2026** (ya pasada) |
| Qué exige | **Consentimiento parental verificable (VPC) SEPARADO** antes de divulgar datos personales de un menor a **terceros** para publicidad dirigida **o para entrenar/desarrollar IA** |
| Prohibición de empaquetado | El padre tiene que poder **aceptar el servicio sin aceptar el entrenamiento de IA**. No vale enterrarlo en los ToS ni agruparlo con el consentimiento del juego |
| Fundamento de la FTC | Divulgar datos de un menor para entrenar IA **"no se considera, en general, integral al servicio"** |

**Traducción a arquitectura, que es lo que hace esta KB.** En un título dirigido
a menores en EE.UU.:

1. Mandar la frase que escribió un chico a una API de LLM de un tercero **es una
   divulgación a un tercero**.
2. Si ese tercero puede usarla para entrenar, hace falta **VPC separado y
   desagrupado** — un segundo flujo de consentimiento, no una casilla más.
3. ⇒ **El patrón "NPC conversacional contra un modelo hosteado" no es
   implementable tal cual en un juego infantil estadounidense.** Las salidas
   viables son: inferencia **local/on-device**, un contrato con **opt-out
   contractual de entrenamiento** verificable, o diseñar el NPC para que **nunca
   transmita entrada libre del jugador**.

Esto es más accionable que cualquier porcentaje de adopción: es un **requisito
de diseño**, con fecha, que decide la topología del sistema. Ver la **Receta 26**
en `compose/patterns.md`.

⚠️ **Procedencia:** todas las fuentes de la muestra son blogs legales y de
compliance, no el texto de la FTC. Discrepan además sobre si el 22-abr-2026 es
la fecha de *cumplimiento* de una regla efectiva desde el 23-jun-2025, o la
fecha de *entrada en vigor*. En sustancia da igual —hoy aplica— pero la cita
exacta debe sacarse de 90 FR 16970.

#### 🟡 Corrección nº3 — Steam: las **tres** cifras de Steam que esta KB cita son tres denominadores distintos, y mezclarlas produce contradicciones falsas

Esta KB publica *"10,9% (2024) → 19,9% (2025) → 30,8% (2026 a julio)"* sobre un
censo de 53.597 releases, y en otro lugar *"49,9% oct-2026"*. Las dos son
correctas y **no se contradicen**: miden cosas distintas. La tabla que faltaba:

| Cifra | Qué mide exactamente | Ventana | Denominador |
|---|---|---|---|
| **49,9%** | Share de releases **del mes** con disclosure | **octubre 2026** | Todos los releases de ese mes (cross-check SteamDB: **1.821 / 3.661 = 49,7%**) |
| **40,1%** | Share acumulado **del año en curso** | ene–oct 2026 | Todos los releases de 2026 |
| **~30%** | Share del **catálogo de juegos de pago** | ene-2025 → oct-2026 | **9.251** juegos de pago (excluye free-to-play) |
| 61,1% / 43% | Lecturas de **primeros días de noviembre** | 1–3 nov / 1–2 nov | **275 juegos. Ruido, no señal** — no usar |

> **Regla de método nº18 (hermana de la nº "un número sin su muestra no es un
> número" del cycle 5): dos cifras de disclosure de Steam sólo son comparables
> si comparten ventana Y denominador. La diferencia entre 49,9% y 30% no es
> desacuerdo entre fuentes: es free-to-play dentro o fuera.**

**Y dos matices que bajan el número real de "AI en gameplay":**
- Valve **cambió las reglas en enero de 2026**: ya **no** hay que declarar
  herramientas de desarrollo, agentes de código incluidos. El 49,9% mide
  **assets que ve el jugador**, no uso interno — y el uso interno, que es el 97%
  del mercado según el dato que esta KB ya tenía, **salió del numerador**.
- El 49,9% **incluye juegos cuyo único uso de IA fue traducir una cadena de
  texto**.

**Desglose de usos entre los juegos que sí declaran** (nuevo en esta KB):

| Uso declarado | Share |
|---|---|
| Generación de imagen | **63%** |
| Testing y traducción | 30% |
| Store page / marketing | 26% |
| Música | 21% |
| Programación | 16% |
| Voice acting | **7%** |

El *voice acting* en 7% es el dato que ordena la conversación de riesgo
reputacional: **es el uso que más indigna y el menos frecuente.**

### Datos nuevos por región — este pass

#### North America
- **COPPA enmendada**, en vigor desde el 22-abr-2026 (arriba). Es, hasta donde
  mide esta KB, **la primera obligación estadounidense con fecha que condiciona
  la arquitectura de un NPC con LLM**.
- **Sigue sin haber** ley federal ni estatal específica de IA en videojuegos.
  El gancho federal general es la **Sección 5 de la FTC Act**: presentar
  contenido generado por IA como hecho por humanos en material de marketing
  puede ser práctica desleal o engañosa. ⛔ **Gap declarado:** la afirmación de
  que *"la ESRB adoptó en 2024 una política de clasificación de contenido IA"*
  aparece en una sola fuente de baja calidad y **no pudo corroborarse**. No se
  incorpora como dato.
- **Regulación de *gambling* (el otro "gaming"):** KPMG mide que **58% de los
  reguladores duda de que la industria pueda autorregular su uso de IA** y que
  **sólo 13% conoce operadores licenciados con marcos internos de IA
  responsable**. ⚠️ Es del sector casino, no videojuegos — esta KB ya documentó
  ese homónimo en el cycle 8 y lo mantiene separado a propósito.

#### EMEA
- **Digital Fairness Act publicado el 11-nov-2026, COM(2026) 1100** (arriba).
  Es el cambio de estado más importante del pass para la región.
- **La línea de world models de Tencent no tiene licencia en EU ni UK** — las
  tres releases, verificado byte a byte este pass (ver `agents/top.md`,
  hallazgo nº1). **Es una restricción de EMEA que nace en un repo, no en un
  regulador**, y por eso es fácil de pasar por alto en un due diligence que sólo
  mira normativa.
- El AI Act sigue con **cronogramas en conflicto entre fuentes** (Art. 50 desde
  el 2-ago-2026 según varias; obligaciones de alto riesgo desplazadas a
  2027/2028 por el Digital Omnibus según otras). **Esta KB sigue llevando las
  dos y no elige** — el conflicto es el dato.

#### APAC
- **China, adopción a nivel estudio — dos cifras que no coinciden y hay que
  llevar las dos:**

  | Fuente | Fecha | Cifra | Matiz |
  |---|---|---|---|
  | Niko Partners | 2025 | **~60%** de los estudios chinos usan IA | — |
  | Game Publishing Committee (citada en **ChinaJoy 2026**) | 2026 | **86,4%** con IA en producción | **La mayoría en etapa temprana de "exploración asistida por IA"** |

  El salto de 60%→86,4% es casi con certeza **definición, no crecimiento**: la
  segunda cuenta "exploración" como producción.
- 🔴 **El dato que convierte la adopción en dinero (nuevo, el mejor del pass para
  APAC):** en **H1-2026**, los títulos con IA fueron **~15% del top-100 móvil de
  China** pero generaron **42,6% de los ingresos** de ese top-100. **≈2,8× de
  sobre-representación en revenue.** Es el primer dato de esta KB que liga IA en
  gaming a facturación y no a eficiencia de producción.
- **NetEase:** un ejecutivo reporta **+30% de eficiencia de I+D** con
  herramientas tipo DeepSeek, concentrado en **diseño de diálogo de NPC y
  testing**. Sin cifras de usuarios para la franquicia *Justice*.
- **Tencent** confirma lo que esta KB ya tenía (*Game for Peace*: **110 M**
  usuarios acumulados de gameplay con NPC IA, **17,7 M** de pico DAU) y agrega
  **Roco Kingdom: World**, donde los combates de NPC son mayoritariamente
  dirigidos por IA. Nuevo también: Tencent lanzó un **compañero de IA que mira
  la pantalla del jugador y aconseja** — categoría distinta al NPC, y sin
  números públicos.
- **Japón:** foco declarado en **protección de IP** y modelos *backend-specialized*,
  no en features visibles al jugador. Encaja con el 85,8% de adopción entre
  desarrolladores japoneses (CESA) que esta KB ya tenía: **adoptan mucho y
  muestran poco.**
- **Corea:** la **AI Basic Act** rige desde el 22-ene-2026 con **al menos un año
  de gracia antes de multas**, y Corea **rechazó** introducir excepción de TDM
  en su ley de copyright (feb-2026). 🔴 **Y Corea es, junto a EU y UK, uno de los
  tres territorios excluidos de la licencia Tencent** — para un engagement
  coreano, esa línea de modelos está doblemente cerrada.

#### LATAM
- 🔴 **El 48,8% insignia de esta KB ahora tiene su ficha técnica completa, y
  confirma la corrección del cycle 9:** el estudio es **PGB Latam 2026**
  (Pesquisa Game Brasil, presentado en **gamescom latam**), **más de 7.200
  encuestados**, y el panel es de **cinco países hispanoamericanos: Argentina,
  Chile, Colombia, México y Perú**. **Brasil no está en la muestra.**
- 🆕 Del mismo estudio: **40,6% de los encuestados está preocupado por los
  despidos y la precarización del trabajo creativo**. Es el contrapeso del
  48,8%, y faltaba: **aceptación de compra y preocupación laboral conviven en la
  misma población.** Vender el 48,8% sin el 40,6% es vender medio dato.
- 🆕 **Gen Z = 47,5% del total de jugadores** de la región.
- 🆕 **Trampa de denominador detectada este pass:** varios medios regionales
  presentan el **Gaming Report 2026 de Bain** (*42% de los gamers se siente más
  cómodo con IA que hace un año; 59% entre adolescentes*) **como si fuera un
  estudio de LATAM. No lo es: es una encuesta global de 5.300+ jugadores.** Es
  la misma clase de error que el de los denominadores de Steam, y acá produce un
  número **inflado a favor** del pitch regional — el tipo de error que no se
  corrige solo porque nadie se queja de una cifra que le conviene.
- **Regulación, Brasil:** el **PL 2338/2023** (Marco Legal da IA) **sigue sin ser
  ley**. Aprobado por el Senado en dic-2024, está en la Cámara; el relator
  anunció el **24-ago-2026** que la votación quedaba **para después de las
  elecciones de octubre de 2026**. A nov-2026 no hay fecha de votación.
  ⇒ **Para un engagement brasileño que arranque hoy, el riesgo regulatorio de IA
  es bajo y conocido. Esa es una ventaja estructural real de la región frente a
  EMEA**, y es honesta: no se sostiene en que LATAM sea más barata.
- ⛔ **Gap medido, no silenciado — tercera vez consecutiva que se mide y tercera
  vez que da el mismo resultado:** **no existe dato publicado de adopción de IA
  por estudios de desarrollo latinoamericanos.** Se buscó en español y en
  portugués (`industria videojuegos América Latina 2026 inteligencia artificial
  estudios adopción`, `Brasil PL 2338 inteligência artificial jogos
  desenvolvedores 2026`). Lo más cercano que existe es **IBGE 2026: 41,9% de las
  empresas brasileñas de 100+ empleados usan IA** — de toda la economía, **no del
  sector juegos**. Se registra como proxy explícitamente etiquetado, no como
  dato sectorial.
- 🆕 **Medición de origen LATAM en el canal de repos (instrumento nuevo este
  pass):** la enumeración por estrellas devolvió **un** repo de gaming de origen
  latinoamericano con tracción — `corosolto/client` (263 ★, FPS de navegador con
  facciones brasileñas, **AGPL-3.0**, push 2026-11-22). **No es un agente ni usa
  IA**, así que no entra en `agents/top.md`. Pero el cero de passes anteriores
  deja de ser cero: **hay producción de juegos open source en la región, no hay
  producción de *agentes* de gaming.** Es un gap de categoría, no de volumen.

#### ⛔ Gap de canal declarado (no silenciado) — se re-verificó este pass

`WebFetch` **no resuelve ningún host general en esta sesión** (`getaddrinfo
ENOTFOUND` contra `www.pinsentmasons.com`). ⇒ **Ninguna afirmación regulatoria de
este pass está verificada contra fuente primaria**: ni COM(2026) 1100, ni
90 FR 16970, ni el informe de GDC. Todo lo regulatorio de esta sección viene de
**resúmenes de búsqueda web de fuentes secundarias**, y se marca como tal. Los
canales que **sí** funcionan y sí verificaron datos este pass son
`gh api search/repositories`, `gh api search/code` y `raw.githubusercontent.com`
— los tres sobre repos, ninguno sobre normativa.

---


## 🔴 Lo que cambió — pass cycle 9 (2026-11-21): **la cifra insignia de LATAM de esta KB se midió sobre un panel que excluye a Brasil**

Este pass no agrega un dato a LATAM: **corrige el dato con el que esta KB venía
vendiendo LATAM**. También le pone muestra al 94% que circula en NA/EMEA, y le pone
números al informe-faro que el cycle 8 identificó como del *otro* gaming.

### 🔴 La corrección: el 48,8% es de **cinco países hispanoamericanos**, y Brasil no es uno de ellos

Desde el cycle 4 esta KB afirma que LATAM acepta la IA jugador-facing mucho más que
el resto del mundo (**48,8%** vs ~85% de rechazo global), y de ahí deriva la tesis
comercial más fuerte del archivo: *LATAM es el mercado racional de primer lanzamiento*.
La cifra es real. **Su alcance, no.**

| | PGB **Latam** 2026 | PGB **Brasil** 2026 |
|---|---|---|
| Muestra | **7.202 respuestas** | **>7.000 personas** |
| Países | **Argentina, Chile, Colombia, México, Perú** | Brasil |
| Campo | presentado en gamescom latam 2026 (São Paulo) | **5–13 de marzo de 2026** |
| Autor | Pesquisa Game Brasil / Go Gamers | idem |
| Compraría un juego hecho con IA | **48,8 %** | **39,3 %** |
| Teme despidos / precarización por IA | **40,6 %** | — |

**Brasil está fuera del panel del 48,8 %** — y Brasil es el mercado más grande y más
maduro de la región, con penetración de videojuegos por encima del 70% de la
población. Cuando se le pregunta a Brasil por separado, la aceptación cae a **39,3 %**:
**unos 9,5 puntos menos**.

**Qué hay que cambiar en el discurso comercial:**

- ❌ Ya no: *"LATAM acepta la IA en juegos"*.
- ✅ Ahora: *"**Hispanoamérica** (AR/CL/CO/MX/PE, n=7.202) muestra 48,8% de intención
  de compra; **Brasil, medido aparte (n>7.000), muestra 39,3%**. La ventaja regional
  es real pero **no es homogénea, y el mercado más grande puntúa más bajo**."*
- ✅ Para un engagement **brasileño**, la cifra que corresponde citar es 39,3 %, no 48,8 %.
- ⚠️ Y en ambos paneles convive con **40,6 % que teme despidos**: aceptación de compra
  y ansiedad laboral **no se cancelan**, coexisten en la misma muestra.

> **La regla de método del cycle 5 —"un número sin su muestra no es un número"— se
> aplicó por primera vez contra la cifra favorita de esta propia KB, y la cifra no
> pasó entera.** El error no fue inventar el dato: fue heredarlo sin su panel.

### 🔴 El 94% ahora tiene instrumento, y **no es una encuesta**

El dato que domina los titulares de 2026 (*"el 94% de los estudios usa IA"*) viene del
**Vivrato AI Adoption Report (VAIA)**:

- **101 conversaciones confidenciales con líderes senior de estudio**, abril–septiembre 2026.
- Estudios con sede **mayormente en Norteamérica y Europa**.
- Es un **estudio de entrevistas estructuradas, no una encuesta online**.
- El 94% = *"usa IA en **al menos una** de nueve etapas de producción"*.
- **52% puntúa la profundidad de integración en ≤ 3/10.**
- **Europa reporta adopción más profunda que Norteamérica** (inversión de lo que se
  suele asumir).

**Esto cierra el rango 29%–90% que esta KB venía registrando** (tendencia nº21): el
techo es alto porque **pregunta a líderes, no a developers**, y porque el umbral es
*"al menos una etapa"*. Y choca de frente con el dato fresco de esta semana:

| Fuente | Fecha | Pregunta | Cifra |
|---|---|---|---|
| Vivrato VAIA (n=101 **líderes**) | abr–sep 2026 | ¿usa IA en ≥1 de 9 etapas? | **94 %** |
| Encuesta GDC (**trabajadores**) | **13-nov-2026** | ¿usás IA en tu trabajo? | **40 %** |
| idem | idem | ¿la genAI es mala noticia? | **55 % sí** |

**La brecha 94 vs 40 no es contradicción: es la distancia entre lo que la empresa hace
y lo que la persona promedio hace.** Para una venta, importa porque el patrocinador
(líder) y el usuario (equipo) **no están en el mismo punto**, y el 55% dice que el
usuario además **no quiere estar ahí**.

### 🔴 El informe-faro de "AI in Gaming 2026" ahora tiene números — y sigue siendo de casino

El cycle 8 declaró la frontera del homónimo (*gaming* = videojuego ≠ casino). Este pass
la confirma **con la muestra del informe**: *"The State of AI in Gaming 2026"* es del
**UNLV International Gaming Institute (AiR Hub) + KPMG**, publicado el **9 de abril de
2026**, y mide **empresas de juego de azar**:

| Métrica | Valor |
|---|---|
| Muestra | **83 empresas de juego + 113 reguladores** |
| AI Maturity Index global | **45 / 100** |
| Operadores *land-based* vs *online* | **39** vs **54** |
| **Gobernanza de IA** (la más baja de todas las categorías) | **30 / 100** |
| Brecha estrategia − gobernanza | **27 puntos** — los autores la llaman *"the most notable finding"* |
| Uso de genAI | **80 % o 81,5 %** según la fuente — ⚠️ **no se pudo dirimir** |

**Uso correcto en Globant:** este informe **no** describe el mercado de videojuegos y
no debe citarse en un pitch de game studio. **Sí** es munición de primer nivel para la
práctica de **iGaming / casino**, y la brecha de 27 puntos entre estrategia y gobernanza
es, literalmente, una oferta de consultoría descrita por sus propios clientes.

⛔ **Gap de canal declarado:** `WebFetch` no resuelve hosts generales en esta sesión
(`ENOTFOUND` en 3 de 3). **Todas las cifras de prensa e informes de este pass vienen de
snippets de búsqueda, no de la fuente primaria.** Donde las fuentes discrepan (80% vs
81,5% de genAI en casino; 60% vs ~90% de estudios chinos) **se registran ambas y se
declara que no se dirimió**, en lugar de elegir la más conveniente.

## 🔴 Lo que cambió — pass cycle 7 (2026-11-21)

> Tercer pass del día. **Ninguna cifra heredada se reescribe por antigüedad.** Lo
> que este pass aporta en mercado son **tres cosas**: (1) la base legal exacta del
> único deadline que esta KB vende, (2) un **defecto del instrumento regional**
> que explica por qué EMEA y LATAM venían devolviendo material de *gambling*, y
> (3) **cifras propias nuevas para APAC** y un **cero medido** para LATAM.

| Qué | Dato | Por qué cambia la conversación |
|-----|------|-------------------------------|
| 🔴 **El 2-dic-2026 tiene base legal, y un alcance mucho más chico del que vendíamos** | El Art. 50 **aplica desde el 2-ago-2026**. El 2-dic-2026 es una **transición del Art. 50(2) (marcado *machine-readable*) y SÓLO para sistemas puestos en el mercado ANTES del 2-ago-2026**; la base legal que citan las fuentes es el **Art. 111(4)**. **No extiende el Art. 50(1)** (disclosure al jugador), que vence desde agosto | ⇒ **La Receta 7 necesita una columna nueva y decisiva: ¿el título se puso en el mercado antes o después del 2-ago-2026?** Si es **después, nunca tuvo período de gracia** y el cliente **ya está en incumplimiento** — el encuadre "quedan 11 días" se lo oculta. Ver el reencuadre nº3 en `compose/patterns.md` |
| 🔴 **El AI Act no impone ninguna tecnología de marcado** | Las fuentes coinciden en que el Acta **no nombra un estándar técnico** | ⇒ **C2PA es una elección defendible, no un requisito legal.** Decir "hay que usar C2PA para cumplir" es vender una preferencia como obligación. Se propone *porque* es auditable e interoperable, y así hay que argumentarlo |
| 🟡 **Las guías de la Comisión son del 20-jul-2026 y NO son vinculantes** | Y citan como ejemplo que la interacción con un **NPC de un juego single-player** puede alcanzar el umbral de **"obviedad"** y **no requerir disclosure adicional** | ⇒ Refuerza y precisa el hallazgo del cycle 6 (la edad de la audiencia): el alcance del Art. 50(1) se decide por **modo de juego + audiencia**, y hay base para **no etiquetar** en el caso más común del catálogo |
| 🔴 **Sanción con número** | Hasta **15 M€ o 3% de la facturación mundial anual**, el mayor de los dos | Es la cifra que mueve la conversación del equipo legal al comité. Esta KB no la tenía |
| 🧪 **Defecto del instrumento regional, y explica 2 passes de ruido** | La consulta obligatoria `AI {industry} {region} 2026 adoption regulation players` con `industry=gaming` devuelve **la industria del juego de azar**, no videojuegos: el informe **UNLV/KPMG "State of AI in Gaming 2026" (83 empresas) es un benchmark de *gambling*** | 🟢 Esta KB **ya etiquetaba bien** ese material como carril separado — no hay contaminación que corregir. Pero **sustituir "gaming" por "video game" en la consulta regional cambió el conjunto de resultados por completo** y produjo los hallazgos de Corea, JOGA y el censo de Steam de abajo. **Regla nº17: en esta industria, el instrumento regional debe decir "video game".** |
| 🟢 **APAC gana dos cifras propias que no estaban** | **Corea: 77,5% de 120 empresas de juego usan gen AI** (KOCCA, Q2-2026), y *"AI skills"* como **prioridad nº1 de contratación**. **JOGA: 100% de las empresas japonesas de juego online encuestadas** usan gen AI, Gemini la herramienta más común | Japón ya tenía su 85,8% (CESA). **Corea es el segundo mercado de APAC con cifra propia** ⇒ la tesis "APAC no se vende adopción, se vende gobernanza" ahora tiene **dos países**, no uno |
| ⚫ **LATAM: cero medido en adopción por estudios** | Buscado con **dos instrumentos** (`AI gaming LATAM...` y `Latin America video game developers AI 2026 Brazil Mexico Argentina studios adoption`): **ninguna encuesta de adopción de gen AI por estudios LATAM**, ni por país ni por tamaño | Es un **cero medido, no un silencio**. Lo que sí hay es **estructura de mercado** (abajo). La cifra vendible de LATAM sigue siendo de **jugadores** (48,8% / 51,8% Gen Z), no de estudios |

---

## 🔴 Lo que cambió en ~1 h — pass cycle 6 (2026-11-21)

> ⏱️ Este pass corre ~1 hora después del cycle 5. **Ninguna cifra de mercado
> cambió en una hora** y ninguna de las de abajo reemplaza a las de las
> secciones siguientes: son **instrumentos y datos que faltaban**, no
> actualizaciones. Las cuatro regiones ganan material propio; donde una región
> midió cero, se dice que midió cero.

| Cambio | Dato | Consecuencia |
|--------|------|--------------|
| 🟢 **Aparece el argumento de valuación, que es el único que habla el idioma del CFO** | **AlixPartners** (*Media & Entertainment Industry Predictions 2026*): las compañías de gaming que integren AI con éxito **sobre IP fuerte** cotizarán a múltiplos **2-3× más altos** que los rezagados **para fin de 2026**. | Es la primera cifra de esta KB que **no** es de adopción ni de compliance. Toda la página venía argumentando *costo* y *riesgo*; esto argumenta **valor de empresa**, y es el argumento que abre una conversación de board, no de equipo técnico. ⚠️ Es una **proyección de consultora**, no una medición: se cita como tesis con atribución, nunca como hecho. |
| 🟡 **Dimensión del mercado de gen-AI-en-gaming, y NA es ~40% de él** | ~**US$ 2.210 M** el mercado de gen AI en gaming en 2026, con **North America ≈ 40%** (≈ US$ 880 M). Las fuentes **no coinciden** en la participación exacta. | Pone techo a la conversación: el gasto en gen AI de toda la industria es **~0,8% de los ~US$ 190-200 MM de la industria**. ⇒ **la oportunidad de AI Studios no es vender AI: es vender la integración de AI en el pipeline existente**, que es la parte grande. Coherente con KPMG: las integraciones más profundas están **en el back-end**, no en contenido generado de cara al jugador. |
| 🔴 **El costo laboral de 2026 tiene cifra agregada** | **más de 10.000 despidos globales en la industria en el primer semestre de 2026** (recuento de prensa sectorial). | La KB tenía casos (Ubisoft 166 el 19-nov) y la cifra de GDC (34% despedidos en 2 años, 54% aún sin empleo). **Ahora tiene el agregado del año**, que es el número que va a estar en la sala antes de que uno hable. Se usa igual que los otros: **no hay evidencia publicada de causalidad con la AI**, y decirlo desactiva la objeción sin negarla. |
| 🟡 **Hay un instrumento nuevo que esta KB no estaba siguiendo** | **Omdia**, *AI in Games: State of the Market 2H26* y *Trends to Watch 2026: Games Tech*. Omdia lista el **backlash de AI** como tendencia clave del año y cubre la emergencia de **herramientas de creación de juegos con AI para consumidores** y las decisiones **build-vs-buy**. | Es la primera casa de research de la industria (no legal, no encuesta de devs) que entra a esta KB. **Es de pago y no se pudo leer**: se anota como instrumento a conseguir, con lo que su resumen público dice, y **no se le atribuye ninguna cifra**. |
| 🟡 **BCG puso número al crecimiento de la industria** | **BCG**, *Video Gaming Report 2026: The Next Era of Growth*. | Mismo tratamiento: instrumento identificado, no leído, sin cifras atribuidas. Sirve para saber **qué deck va a tener el cliente sobre la mesa**. |

---

## 🔴 Lo que cambió en 24 h — pass cycle 5 (2026-11-21)

| Cambio | Dato | Consecuencia |
|--------|------|--------------|
| 🔴 **El deadline de EMEA: quedan 11 días** | Art. 50(2) del EU AI Act: fin del período de gracia para sistemas **ya en el mercado** el **2-dic-2026**. Hoy es 2026-11-21. | El reencuadre de ayer (triage, no proyecto) **se mantiene y se endurece**. Ver sección EMEA. |
| 🔴 **El "40% global" de adopción es, en los hechos, un número de North America** | GDC *State of the Game Industry* (edición **12-nov-2026**): 40% usa gen AI. Desagregado: **EE.UU. 36%→38%** (dentro del margen de error, según GDC) vs **fuera de EE.UU. 37%→44%** (+7 pts). Y la muestra es **63% residentes en EE.UU.**, contra **54% en la edición anterior**. | **La muestra se volvió MÁS estadounidense justo mientras lo que se movía era el resto del mundo.** Citar "40%" en una sala de APAC es citarles un número ajeno. Ver la sección nueva abajo. |
| 🟢 **El contraste APAC tiene cifra propia, y es 2,2×** | CESA (Japón), preview en Tokyo Game Show 2026: **85,8% de los desarrolladores japoneses usa gen AI**, de ellos **63% a diario** y 22,8% ocasionalmente. **1.349 respuestas individuales**, encuesta mayo–agosto 2026; informe completo a principios de **dic-2026**. | Japón solo está **a más del doble** del número "global". GDC tiene **3% de respondentes en Japón**: estructuralmente **no puede ver** este mercado. |
| 🟡 **El etiquetado en Steam tiene una ruptura de serie, y es de instrumento** | **Valve reescribió el formulario de declaración el 17-ene-2026** para restringirlo a contenido AI **que el jugador ve u oye**, excluyendo herramientas usadas en desarrollo. | Las cifras pre y post enero-2026 **no son comparables**. El 49,9% de octubre mide **menos cosas** que el 26,4% de octubre-2025: la subida real es **mayor** que la que muestra la serie. |
| 🔴 **Declarar AI en Steam correlaciona con peor recepción** | Juegos que declaran AI: **77% de reviews positivas** promedio. Los que no declaran: **83%**. Y los devs con **≥10 lanzamientos en 2026 declaran en el 68%** de sus juegos, contra **37%** de los devs con un solo lanzamiento. | 6 puntos de review score es dinero. **No está controlado por género, precio ni presupuesto** — y la segunda cifra sugiere la confusión: quien publica mucho y rápido declara mucho más, así que el castigo puede ser al *asset-flip*, no a la AI. Hay que decir las dos cosas. |
| 🔴 **NA tiene por primera vez una obligación de diseño, no de disclosure estático** | Leyes estatales de *companion chatbots*: **Washington HB 2225** (sancionada 24-mar-2026, vigente **1-ene-2027**), **Connecticut SB 5 / Public Act 26-15** (firmada 27-may-2026), **Idaho S 1297** (firmada 31-mar-2026, operativa **1-jul-2027**). Connecticut exige recordatorio de que se habla con una AI **al menos 1×/hora para menores y 1×/3 horas para adultos**, más protocolo de crisis con derivación al **988**. | Un recordatorio periódico **dentro de la sesión** es un **requisito de ingeniería y de UX**, no una línea en la tienda. Es la primera obligación de NA que toca el *runtime* de un NPC con LLM. Ver Receta 13. |

---

## 🔴 El hallazgo de método de este pass: **un número sin su muestra no es un número**

Tres mediciones de este pass fallan del mismo modo, y el modo es el mismo que
rompe las licencias en `agents/top.md`: **el valor se publica sin su alcance.**

| Medición | Valor citado como global | Alcance real del instrumento | Qué pasa si se cita sin el alcance |
|----------|--------------------------|------------------------------|-----------------------------------|
| Adopción de gen AI, GDC nov-2026 | **40%** | **63% de la muestra vive en EE.UU.**; 3% en Japón | Se presenta una cifra **de NA** como cifra mundial. En APAC es ~2× más alta |
| Adopción de gen AI, CESA 2026 | **85,8%** | **Japón**, 1.349 respuestas individuales, mayo–ago 2026 | El baseline 2025 que la prensa compara (**51%**) era de **54 empresas**, no de individuos ⇒ **no son la misma unidad de medida**: el salto 51→85,8 mezcla empresa con persona |
| Etiquetado en Steam | **49,9%** (oct-2026) | Censo de releases **con el formulario reescrito el 17-ene-2026**; un censo independiente (Sulka Haro, 53.597 releases) da **30,8%** para 2026 | Dos instrumentos, dos números, y **la serie temporal tiene un corte en enero-2026**. "49,9% vs 1,7% en enero-2024" compara dos definiciones distintas |

**Regla para las slides de AI Studios:** toda cifra de adopción viaja con
**(a) la casa, (b) la geografía de la muestra y (c) la unidad medida** —persona o
empresa. Las tres cifras de arriba son correctas y **las tres se vuelven falsas
al perder una de esas tres cosas**. Es el mismo error que publicar "MonoGame es
MIT": el dato existe, el alcance no viajó con él.

> **Lo que esto habilita comercialmente, y es concreto:** permite **presupuestar
> la resistencia por región**. Un engagement en Japón empieza con 85,8% de
> adopción declarada y 63% de uso diario: la conversación es de *scaling y
> gobernanza*. Uno en EE.UU. empieza con 38% y **55% de la industria diciendo que
> la gen AI le hace daño** (GDC nov-2026, 34% despedidos en 2 años, 54% de ellos
> aún sin empleo): la conversación es de *pipeline interno y riesgo*. **Es el
> mismo deck con dos primeras diapositivas distintas**, y hasta hoy esta KB tenía
> sólo la segunda.

---

## 🔴 Lo que cambió en 25 días — pass 2026-11-20

| Cambio | Dato | Consecuencia |
|--------|------|--------------|
| 🔴 **El deadline de EMEA está encima** | Art. 50(2) del EU AI Act: fin del período de gracia el **2-dic-2026**. Hoy es 2026-11-20: **quedan 12 días.** | En el pass anterior eran ~5 semanas y se vendía como "sprint". A 12 días **ya no es vendible como proyecto**: es triage. Ver nota de reencuadre en la sección EMEA. |
| 🔴 **Steam cruzó el 50%** | **49,9% de los lanzamientos de octubre-2026** declararon contenido AI (censo ~63.600 releases). Contra-conteo independiente sobre SteamDB: **1.821 de 3.661 = 49,7%**. | La proyección de prensa de octubre ("más de la mitad para 2027-2028") se cumplió **más de un año antes**. El etiquetado dejó de ser señal diferencial: es el default. |
| 🟢 **Primera cifra de producción real de NPCs con AI** | Tencent reporta, en *Game for Peace*, **110 M de usuarios acumulados** de features de NPC con AI y **17,7 M de DAU pico**. | Es, con diferencia, el despliegue de NPCs con AI más grande del que esta KB tenga medida. Mata el argumento "los NPCs con LLM no escalan": escalan, en APAC, hoy. |
| 🔴 **Un AAA institucionalizó la AI el mismo día que despidió** | Ubisoft: **166 puestos** eliminados (Montreal y Quebec City, conocidos el **19-nov-2026**, día de su reporte semestral) y **1.100 en seis meses**; a la vez lanzó **Motherbrain**, subsidiaria dedicada a AI generativa para *gameplay* (NPCs con memoria y "AI game masters"). | Es el riesgo reputacional del pass hecho titular. Ningún producto de Motherbrain ha salido: son planes. Y GDC (nov-2026) mide que **54% de los despedidos sigue sin empleo** (era 48% en enero) **sin evidencia de que la AI cause los despidos**. La correlación es de calendario, no causal — pero el cliente la va a escuchar como causal. |

---

## Tamaño de mercado — los estimados no coinciden, y la diferencia importa

No existe "el" número de AI in gaming: hay dos mercados distintos medidos por
casas distintas. Presentarlos como uno solo es lo que produce slides que un
cliente puede refutar.

| Alcance | Casa | 2026 | Horizonte | CAGR |
|---------|------|------|-----------|------|
| **Generative AI in Gaming** (estrecho) | Research and Markets | **$2.21B** | $5.09B en 2030 | 23.2% |
| Generative AI in Gaming (estrecho) | Market.us | ~$1.7B (base 2025) | ~$17.0B en 2035 | 25.6% |
| **AI in Games** (amplio) | The Business Research Company | **$3.4B** | $6.73B en 2030 | — |
| AI in Games (amplio) | Precedence Research | **$8.50B** | ~$37.89B en 2034 | — |
| AI in Gaming (amplio) | Technavio | — | 2025–2030 | **40.7%** |

**Cómo usarlo:** el rango defendible es **$2–2.2B para generative AI** y
**$3–8.5B para AI in games en sentido amplio**. La horquilla 3.4 → 8.5 para el
mismo año y el mismo nombre es la prueba de que el alcance, no el crecimiento, es
lo que mueve el número. Citar la casa y el alcance, nunca el número solo.

### Adopción — y la corrección que este pass hace a su propio KB

El pass de julio publicó, en cuatro archivos, que **"87% de estudios usa AI
agents en sus workflows"** (Google Cloud survey, 615 devs, jun-jul 2025). Esa
cifra sigue existiendo pero **no significa lo que el KB la hacía significar**, y
la encuesta de GDC 2026 (2.300+ respondentes) mide lo contrario:

| Medición | Valor | Fuente |
|----------|-------|--------|
| Profesionales de la industria que usan gen AI en su trabajo | **36%** | GDC 2026 survey (2.300+) |
| Devs *en estudio* que usan gen AI | **30%** | GDC 2026 |
| Roles de publishing / marketing / negocio que usan gen AI | **58%** | GDC 2026 |
| Devs que dicen que gen AI está **dañando** la industria | **52%** | GDC 2026 |
| Devs que ven impacto **positivo** | **7%** | GDC 2026 |
| Uso declarado por dev, variación interanual | **36% → 29%** (baja) | Market.us citando GDC |
| Desarrolladores que usan AI para acelerar assets / playtesting / localización | **97%** | Google Cloud Games Report 2025 |
| 🆕 Profesionales que usan gen AI — **edición nov-2026** | **40%** (desde 36%) | GDC SOTGI, publicado **12-nov-2026** |
| 🆕 De los que la usan, uso **diario** | **57%** | GDC nov-2026 |
| 🆕 Usuarios en **EE.UU.** | **36% → 38%** (GDC lo llama dentro del margen de error) | GDC nov-2026 |
| 🆕 Usuarios **fuera de EE.UU.** | **37% → 44%** (+7 pts) | GDC nov-2026 |
| 🆕 Dicen que gen AI **daña** la industria | **55%** (era 52%) · mixto 35% · positivo **8%** | GDC nov-2026 |
| 🆕 Despedidos en los últimos 2 años | **34%**, de ellos **54% aún sin empleo** | GDC nov-2026 |
| 🆕 **Desarrolladores japoneses** que usan gen AI | **85,8%**, **63% a diario** (1.349 respuestas, may–ago 2026) | **CESA 2026** (preview en TGS 2026) |
| 🆕 Picos sectoriales dentro de GDC | **58%** en juegos free-to-play · **70%** en estudios sólo-móvil · sólo **8%** la usó para **arte de producción final** | GDC nov-2026 |

🔴 **Las dos cifras extremas (97% y 29%) son compatibles y hay que venderlas
juntas:** casi todo el mundo toca una herramienta con AI dentro, y una minoría
decreciente declara "usar gen AI". Lo que cambió entre 2025 y 2026 no es la
capacidad: es que **declararlo tiene costo reputacional**. Un pitch que abre con
"87% de los estudios ya usa agentes" se choca con que 52% de los devs del cliente
piensa que la AI está dañando su industria. El ángulo que sobrevive a esa sala es
**AI en el pipeline interno** (QA, localización, build, ops), no AI en el
contenido que ve el jugador.

🆕 **2026-11-21 — y el dato que lo confirma por dentro de la propia encuesta:**
GDC mide **70% de adopción en estudios sólo-móvil** y **58% en free-to-play**,
contra **8% que la usó para arte de producción final**. La adopción no está
distribuida por geografía solamente: está distribuida **por modelo de negocio**.
F2P y móvil —donde el contenido es operación continua y el arte es
reemplazable— adoptan al doble del promedio; el arte final, que es donde el
jugador y el artista se encuentran, es el último bastión con 8%. **Ésa es la
frontera comercial real**, y es la misma en las dos regiones: vender pipeline y
operación, no vender el arte.

---

## Players globales — engines y plataformas

| Empresa | Tipo | Fortaleza AI | Debilidad | Modelo |
|---------|------|-------------|-----------|--------|
| **Unity Technologies** | Engine propietario | Unity AI 6.2 integrado (Assistant, Generators, Inference Engine). ml-agents (Apache-2.0). El ecosistema MCP más adoptado corre sobre Unity: 🔴 **corregido este pass — el líder es `CoplayDev/unity-mcp` (MIT, 16.087★), no `IvanMurzak/Unity-MCP` (4.672★)**; el primero estaba invisible para esta KB por un rename. | Controversia de pricing 2023 aún en la memoria del mercado. | Revenue share + suscripción |
| **Epic Games (Unreal)** | Engine propietario | MetaHuman AI. Aura agent. Plugin MCP first-party experimental reportado en UE 5.8. | Royalty 5% > $1M. **Tim Sweeney atacó públicamente la política de disclosure de AI de Steam** como "abhorrent" — Epic está posicionado contra el etiquetado, lo que es un riesgo de marca para un partner. | Royalty |
| **Valve / Steam** | Distribución | **Es el regulador de facto del AI en juegos.** Política de disclosure desde 9-ene-2024, **reescrita el 17-ene-2026**. | No produce tooling AI. | Revenue share |
| **NVIDIA** | GPU + SDK | ACE (ASR+NLP+TTS+animación NPC). **DLSS 5 presentado en GTC 2026 (mar-2026)**: modelo generativo capaz de transformar skins y modelos de personaje en juego. | Dependencia de hardware NVIDIA. | Hardware + SDK |
| **Skywork AI** | Research (APAC) | 🆕 **Matrix-Game 4.0 (15-oct-2026)**: el world model jugable abierto más avanzado. 67 FPS en RTX 5080. Código y juegos publicados. | Licencia partida (MIT raíz / Apache-2.0 en v4); pesos en HF sin verificar. | Open weights |
| **Microsoft** | Cloud + research | 🆕 **MineWorld** (MIT): world model Minecraft en tiempo real. Azure AI para gaming. Sponsor de O3DE. | Sin engine propio competitivo. | Cloud usage |
| **Tencent** | Publisher + research (APAC) | Hunyuan-GameCraft: video de juego interactivo desde teclado y mouse. | 🔴 Licencia community propia, **no OSI**. No es base construible. | Propietario |
| **Roblox** | Plataforma UGC | AI Studio con cliente MCP. CUBE 3D. OpenGameEval OSS. | Audiencia mayormente juvenil. | Revenue share |
| **Google DeepMind** | Research + cloud | Concordia (Apache-2.0). Genie 2 / Genie 3. | 🔴 **La línea Genie no es abierta**: reports y blog posts, sin código. Todo lo jugable abierto es de Skywork, Microsoft o Tencent. | Cloud API |
| **Heroic Labs** | Backend OSS | Nakama (Apache-2.0, **13.757★**). | Heroic Cloud desde ~$600/mes. | OSS + managed |
| **Inworld AI / Convai** | AI NPC (cerrado) | NPCs con voz, memoria, emoción. | Pricing por personaje o por llamada, cerrado. | SaaS |

---

## Opportunities by region

Una regla de este archivo: **cada región tiene su propio gancho regulatorio y su
propia oferta local**. No se reutiliza el argumento de LATAM en EMEA.

### North America

#### 🆕 cycle 20 (2026-11-25) — el canal de cobro: **la comisión está indeterminada, y eso es la oportunidad**

Estado medido del litigio que fija cuánto cuesta cobrar fuera de la App Store en
EE.UU.:

| Hito | Fecha | Qué dejó establecido |
|---|---|---|
| Desacato de Apple (N.D. Cal.) | **abr-2025** | Prohibió a Apple cobrar comisión sobre compras por link externo |
| 9º Circuito | **11-dic-2025** | Confirmó **el desacato** pero **anuló la prohibición total** de comisión; devolvió el caso para fijar una comisión "razonablemente atada a los costos reales de Apple" |
| Certiorari concedido | **30-jun-2026** | Limitado a si un tribunal puede declarar desacato por violar **el "espíritu"** de una orden judicial |
| Escrito de Epic a la Corte | **13-nov-2026** | Sostiene que el **27 %** de Apple sobre compras hechas dentro de los 7 días del tap desafía la orden |
| Posición del Solicitor General | nov-2026 | Hay *"al menos duda razonable"* de que la orden prohibiera el 27 % |

**Lo que Apple pide:** 15 % para apps estándar (vs 30 % de IAP). **Google Play
(EE.UU.):** su programa de *external offers* tuvo fecha límite de inscripción el
**28-ene-2026**; si Google efectivamente cobra esas tarifas, el incentivo
económico del webstore se desvanece.

⚠️ **Límite declarado:** la comisión de remand **no tiene fecha en ningún
expediente público** y una de las fuentes consultadas la proyecta para 2027. **No
se publica acá una fecha de resolución.**

🟢 **Oportunidad NA, y es de ingeniería, no de lobby:** el cliente que ya tiene
webshop necesita **poder cambiar la comisión por región y por fecha de vigencia
sin recompilar, y poder reconciliar hacia atrás cuando el porcentaje cambie
retroactivamente**. Eso es tabla de comisiones versionada + ledger idempotente
(`tigerbeetle` o `formancehq/ledger`) + enrutamiento (`juspay/hyperswitch`). Es un
engagement de arquitectura financiera, y **el disparador es una sentencia que
todavía no salió** — se vende antes, no después.

#### 🆕 Oportunidad del pass cycle 19 (2026-11-22): **la primera prohibición de EE.UU. que apunta al precio personalizado ya está en vigor, y es de un estado**

| Jurisdicción | Instrumento | Estado medido | Qué prohíbe / exige |
|---|---|---|---|
| **Maryland** | *Predatory Pricing Act* | **en vigor desde 1-oct-2026** | Primera prohibición expresa del país sobre prácticas de *surveillance pricing* en venta minorista |
| **Connecticut** | ley de *surveillance pricing* | sancionada | Segunda del país; alcanza vendedores minoristas y plataformas de delivery |
| **New Jersey** | ley de *surveillance pricing* | sancionada | **La primera con acción privada** (*private right of action*) ⇒ litigio sin regulador de por medio |
| **New York** | régimen de divulgación | vigente | Exige **divulgar** el precio algorítmico personalizado, no lo prohíbe |
| **California** | **AB 2564** (Ward, presentada 20-feb-2026) | **avanzó en la Asamblea el 27-may-2026**, en comisión del Senado en jun-2026 | Prohibiría fijar precios personalizados con información personal. **No es ley todavía** |
| **California (enforcement)** | barrido investigativo del Fiscal General | anunciado en enero (2026) | Uso de la ley de privacidad estatal contra precios individualizados en viajes y retail |

Según una de las alertas consultadas, **cuatro estados** ya tienen alguna forma de
ley de *surveillance pricing*. 🔴 **Pero las seis filas están escritas para
retail**, y ninguna fuente de este pass afirma que una tienda in-game o el precio
de un paquete de moneda virtual estén dentro del ámbito.

**El ángulo comercial, que es el que un estudio puede comprar hoy:** el diseño
defendible para un lanzamiento en EE.UU. es **personalizar el contenido y no el
precio** —misma lista de precios para todos, la variación va en qué se muestra y
cuándo— más un registro que pruebe esa separación. Eso es exactamente lo que
distingue un *feature flag* auditable de un modelo de *pricing* opaco, y es un
entregable de dos semanas sobre
[Flagsmith](https://github.com/Flagsmith/flagsmith) (BSD-3) o
[flagd](https://github.com/open-feature/flagd) (Apache-2.0).

**Antecedente de enforcement que ya existe y nadie discute** (y que sigue siendo la
referencia más citada): la acción de la FTC contra **Epic Games** (2022-2023,
**USD 520 M** en total — USD 275 M por COPPA y USD 245 M en reembolsos por *dark
patterns*), cuya orden exige **consentimiento expreso e informado antes de cobrar**
una compra in-game. ⚠️ Un resultado de este pass menciona además una acción de la
FTC contra el publisher de *Genshin Impact* (USD 20 M y prohibición de vender loot
boxes a menores): **no se pudo verificar** más allá del titular, se publica como
pendiente.

📉 **Gap medido en esta región:** se buscó una enmienda de COPPA de 2026 y acción de
la FTC específicamente sobre **ofertas personalizadas**; los resultados sólo
devolvieron los casos de 2022-2023. **Nada nuevo verificable en 2026 sobre este
eje en NA**, y conviene decirlo antes de que alguien lo asuma.


> 🆕 **cycle 18 (2026-11-22).** Nada nuevo de videojuegos, y **tercera confirmación
> del homónimo**: el informe que el buscador devuelve como estado del arte
> —UNLV/KPMG *State of AI in Gaming 2026*, AI Maturity Index **45/100**, abril
> 2026— es de **gambling**, no de videojuegos. Citarlo en una propuesta de gaming
> es un error de categoría, y es el error por defecto de cualquiera que busque este
> tema sin saberlo. De videojuegos sólo apareció la GDC 2026 vía Reed Smith (**52%**
> de devs ve daño) y la coalición de seguridad infantil de Roblox con la Attorney
> General Alliance. 🔴 **Riesgo de licencia aplicable acá:** el stack de los papers
> estadounidenses (DROID-SLAM, RAFT, lietorch — Princeton, BSD-3) es permisivo, pero
> llega **vendorizado dentro de repos de terceros** ⇒ la obligación de atribución se
> hereda sin elegirla. Ver Receta 46.

#### 🆕 Oportunidad del pass cycle 17 (2026-11-22): **la fábrica, que es lo único que Steam exime y el backlash no toca**

**El gancho, en una línea:** *"No les proponemos IA en el juego. Les proponemos que
el build salga solo todas las noches, que un agente lo juegue antes que ustedes y que
el crash llegue con el stack del cliente — y nada de eso dispara un disclosure en
Steam ni una discusión con el equipo de arte."*

- **Steam exime las herramientas de desarrollo.** La Receta 43 cae entera del lado
  exento, igual que la 19 y la 10.
- **El contra-argumento ya tiene nombre:** si el cliente llega con el blog de QA con
  Bedrock de AWS, la respuesta es que esa referencia embebe **GPL-3.0 en su build** y
  pide **suscripción Pro**, y que la alternativa Apache-2.0 cubre UE, Unity, Godot y
  Roblox.
- **Si el cliente es Unreal, hay una línea de presupuesto que nadie puso:** Perforce +
  Horde bajo EULA, o TeamCity. Llevarlo a la estimación inicial.

#### 🆕 Oportunidad del pass cycle 16 (2026-11-22): **la adopción sube y la aceptación baja, tercer año seguido** — y el plano de datos es lo único que se vende sin tocar esa herida

**El gancho, en una línea:** *"El 36% de la industria ya usa IA generativa y el
52% de los desarrolladores dice que le hace mal al sector. Lo único que podés
construir sin pelear esa discusión es la capa que mide, no la que genera."*

**Las cifras, de la GDC State of the Game Industry 2026** (encuesta de industria,
la fuente más repetida y la más fiable del conjunto de este pass):

| Señal | 2024 | 2025 | 2026 |
|---|---|---|---|
| Trabajadores que usan IA generativa | ~27% | — | **36%** |
| Dicen que el impacto en la industria es **negativo** | 18% | 30% | **52%** |

> **Divergen, no convergen.** Es el tercer año consecutivo de deterioro del
> sentimiento **mientras la adopción crece**. Cualquier propuesta cuyo valor
> dependa de que el equipo del cliente *quiera* IA generativa está apostando contra
> una tendencia de tres años. La capa de datos no pide esa adhesión: mide lo que el
> juego ya hace.

**⚠️ Conflicto registrado y NO resuelto — divulgación en Steam.** Esta KB publica
**49,9% (oct-2026)**. Las fuentes de este pass dan **30,8% de los lanzamientos de
2026** (19,9% en 2025; 10,9% en 2024), sobre ~53.600 lanzamientos. Los dos números
no se pueden promediar: casi con seguridad tienen **denominadores distintos**
(catálogo entero contra lanzamientos del año). **No citar ninguno de los dos como
"el" dato de Steam sin decir el denominador.** La dirección —sube fuerte año a
año— es lo único que las dos series comparten.

**Regulación, con la etiqueta puesta:** el informe de referencia del año
(*State of AI in Gaming 2026*, KPMG con UNLV) es del sector **GAMBLING**, no de
videojuegos — 83 empresas de juego de azar y 113 reguladores. Dice que el **58% de
los reguladores cree que la industria no puede autorregularse** en IA y que el 70%
de las empresas declara alguna práctica de IA responsable. **Es trasladable como
clima regulatorio, no como obligación para un estudio de videojuegos.** Lo mismo
con la guía de Alberta (ene-2026) sobre detección de bots: es de azar.

Sin verificar: que la ESRB haya empezado a tratar el diálogo de NPC generado como
contenido sujeto a revisión de clasificación. **Aparece en una guía de terceros y
no se pudo confirmar en fuente ESRB.** Si fuera cierto cambia el calendario de
certificación de cualquier juego con NPC generativo, así que **vale la pena
confirmarlo antes de una propuesta**, no citarlo.


#### 🆕 Oportunidad del pass cycle 15 (2026-11-22): **auditoría CVAA del chat — la obligación que lleva siete años en vigor y que ningún pass de esta KB había registrado**

**El gancho, en una línea:** *"Tu juego tiene chat de voz. Desde 2019 la FCC
exige que ese canal tenga equivalente en texto. Y el componente que lo resuelve
es el mismo que necesitás para moderar la voz."*

| Qué | Detalle |
|---|---|
| **Disparador** | El **CVAA** (*Twenty-First Century Communications and Video Accessibility Act*), que fiscaliza la **FCC**, extiende los requisitos de accesibilidad a consolas, juegos y servicios de comunicación avanzada (**ACS**). Según guías de accesibilidad del sector, aplica a títulos publicados **desde el 1-ene-2019** y a títulos anteriores que reciban ***substantial upgrades*** ⇒ **una remasterización o un port reactiva la obligación** |
| **La regla concreta** | Resumen del IGDA: el **chat de texto** debe ofrecer entrada y salida **de voz**; el **chat de voz** debe ofrecer entrada y salida **de texto**. Es bidireccional, y es exactamente la pieza que un *live service* da por resuelta |
| **Entregable 1** | **Inventario ACS**: qué canales de comunicación expone el juego (voz, texto, *party chat*, emotes, chat de clan), cuáles tienen equivalente en la otra modalidad y cuáles no. Es un artefacto de dos semanas y no existe en ningún estudio |
| **Entregable 2** | **Capa STT/TTS bidireccional** construida sobre lo que esta KB ya tiene verificado como permisivo en la capa de voz (cycle 13) + `AccessKit`/`prism` para la salida a lector de pantalla. **No hay producto que comprar**: la única librería de abstracción viva es `ethindp/prism` (**MPL-2.0**) y la archivada es `m1maker/SRAL` |
| **Entregable 3** | **Doble uso declarado**: la misma transcripción alimenta la moderación de voz. La moderación de **texto** sí tiene oferta permisiva real (`unitaryai/detoxify`, `guardrails-ai`, ambos ya en esta KB) ⇒ **transcribir convierte un problema sin oferta en uno que ya está resuelto** |
| **Cifras de contexto** | **33%** de los desarrolladores declara estar construyendo funciones de accesibilidad (GDC 2026) y **74%** de los adultos estadounidenses considera que los juegos ofrecen experiencias accesibles (ESA 2026). *Ambas son secundarias, de un resumen estadístico de 2026, sin verificar contra el informe original* |
| **Lo que NO se promete** | Que esto cubra el EAA europeo. **Son regímenes distintos** y el europeo está en disputa de alcance (ver EMEA) |

> ⚠️ **Antes del contrato:** la fecha del 1-ene-2019 y la regla de *substantial
> upgrades* provienen de guías de proveedores de accesibilidad, **no del texto de
> la FCC**. Verificar contra el `47 CFR` con el abogado del cliente. El gancho es
> sólido; la cláusula contractual necesita fuente primaria.

#### 🆕 Hallazgo regulatorio nº2 del cycle 15 (NA): **doce estados ya legislaron sobre *AI companions*, y nadie sabe si un NPC generativo entra**

| Qué | Detalle |
|---|---|
| **El dato** | Según un artículo del *Harvard Journal of Sports and Entertainment Law* (ago-2026), a **julio de 2026 doce estados** habían promulgado legislación dirigida específicamente a **chatbots de IA, *AI companions* y sistemas conversacionales** |
| **Por qué es un problema para gaming** | Ninguna de esas leyes se escribió pensando en videojuegos, pero **un NPC con LLM es literalmente un sistema conversacional**. El propio artículo deja la pregunta abierta |
| **El matiz que define el riesgo** | **Connecticut excluye** expresamente los chatbots de juego **limitados a temas del juego**. ⇒ el carve-out existe pero es **estrecho**: un NPC que se mantiene en la ficción está fuera; uno que conversa de cualquier cosa —que es el argumento de venta de los NPCs generativos— **cae adentro** |
| **Entregable** | **Clasificación de NPCs por amplitud conversacional**, no por tecnología: por cada NPC, si su dominio está acotado por diseño (y demostrable con los *guardrails* en el código) o si es abierto. Es el mismo artefacto del cycle 12 (alcance estado por estado) con un eje nuevo, y es **barato ahora y carísimo después del lanzamiento** |
| **Adyacente, no es gaming** | En el carril **gambling**: Alberta y Ontario introducen requisitos para **detectar y prevenir bots** en juegos *peer-to-peer*, y Carolina del Norte obliga a **documentar y divulgar** el uso de IA. Se cita como señal de dirección regulatoria. **No se promueve: es apuestas** |

#### 🆕 Oportunidad del pass cycle 14 (2026-11-22): **auditoría de clausura de licencias del pipeline de assets — y la negociación que sí tiene contraparte**

**El gancho, en una línea:** *"Tu pipeline de generación de assets dice MIT en
los cuatro repos. Tres de los cuatro renderers que instalaste prohíben el uso
comercial por escrito, y lo dice el README, no la licencia."*

| Qué | Detalle |
|---|---|
| **Disparador** | `microsoft/TRELLIS` + `TRELLIS.2` suman **27.514 ★** y son el punto de partida por defecto de cualquier equipo que arranque imagen→3D en 2026. Los dos declaran dependencias no comerciales **sólo en prosa del README** |
| **Entregable 1** | **Clausura de licencias del pipeline**: por cada repo, resolver toda dependencia nombrada en su sección de licencia como un repo más (SPDX + byte-read). Es el **canal 4** documentado en `repos/foundations.md` |
| **Entregable 2** | **Separación generación / visualización.** El camino de generación de TRELLIS es MIT **incluidos los pesos**; `setup.sh` instala los renderers sólo con flags explícitas y `renderers/__init__.py` es perezoso ⇒ se puede generar sin importarlos. Lo que hay que reescribir es el **preview**. Sustituto medido de extracción de malla: `nv-tlabs/FlexiCubes`, **Apache-2.0** |
| **Entregable 3** | 🆕 **La vía que sólo existe en NA:** los licenciantes que bloquean (**NVIDIA**, `nvdiffrast` / `nvdiffrec`) son una empresa con la que el cliente ya tiene relación comercial. A diferencia de una exclusión territorial, acá hay **a quién preguntarle**. Entregable: el expediente técnico para pedir licencia comercial, con los archivos y versiones exactas |
| **Por qué ahora** | La adopción de esta capa es **~19%** del uso de AI (GDC 2026, vía resúmenes secundarios): el cliente está eligiendo pipeline **este trimestre**, y lo elige para tres años |
| **Lo que NO se vende** | Nada del carril casino (KPMG/SIGMA/AiR HUB, Alberta, Colorado). **Es gambling.** |

#### 🆕 Oportunidad del pass cycle 12 (2026-11-22): **mapa de alcance estado-por-estado para NPC conversacionales + barrido biométrico**

**El gancho, en una línea:** *"No está resuelto si tu NPC es un companion chatbot
bajo la ley de tu estado — y no todas las leyes estatales excluyen videojuegos."*

| Qué | Detalle |
|---|---|
| **Disparador** | Connecticut PA 26-15 entra **por etapas desde el 1-oct-2026**; el análisis de la *Harvard JSEL* (ago-2026) deja **explícitamente abierto** si un sistema de AI dentro de un juego cae en alcance, y advierte que **no todas** las leyes estatales de chatbot traen exclusión para videojuegos |
| **Entregable 1** | **Matriz de alcance por estado** para el título: ¿el NPC califica como *companion*? ¿hay exclusión de videojuego en el texto? ¿qué etapa de la ley aplica a la fecha de lanzamiento? |
| **Entregable 2** | 🆕 **Barrido biométrico, que no pasa por ninguna norma de AI:** inventario de *eye tracking*, entrada facial y telemetría anti-cheat contra las leyes de **Texas, Illinois y Washington**. Vector independiente y casi siempre ignorado en el *kickoff* |
| **Entregable 3** | 🆕 **Opción de arquitectura que apaga el problema en vez de documentarlo:** mover el NPC a **SLM on-device** (ACE + Qwen3-8B / Nemotron). Si la conversación no sale de la máquina del jugador, la topología de datos que dispara la obligación **deja de existir** — ver `compose/patterns.md`, Receta 29 |
| **Por qué ahora** | La fecha de la etapa 1 **ya pasó** y el alcance sigue sin estar resuelto: el cliente está expuesto *hoy* y no tiene cómo saberlo |
| **Lo que NO se vende** | Nada del carril casino (NYSGC 9-oct-2026, Massachusetts 9-oct-2026). **No aplica a videojuegos** y confundirlos quema la credibilidad en la primera reunión |


#### 🆕 Cycle 11 (2026-11-22) — Norteamérica mide **menos profundidad de integración que Europa**, y Ubisoft abre su tecnología de NPC a terceros

| Hallazgo | Dato | Qué hacer con él |
|---|---|---|
| 🔴 **NA está por DEBAJO de Europa en profundidad** | Vivrato VAIA (20-oct-2026, n=101 líderes, abr–sep 2026): **Norteamérica 3,5/10** vs **Europa 4,2/10** de profundidad de integración en pipeline. **52%** de la muestra total puntúa ≤ 3/10 | Contradice el supuesto de sala. En NA el pitch correcto es **"la adopción ya ocurrió, la integración no"**: hay herramientas en todas partes y **casi ningún pipeline** que las tenga embebidas. Eso es trabajo de integración — exactamente lo que AI Studios vende — y **no** requiere convencer a nadie de adoptar AI |
| 🟢 **Ubisoft Motherbrain va a vender a terceros** | Mandato declarado: servir a creadores *"dentro del ecosistema de Ubisoft **y fuera de él**"* + alianzas con empresas de tecnología seleccionadas. Foco: **NPC con memoria** y **AI game masters** de mundos sistémicos. Formaliza el prototipo **Teammates**. Sin producto ni fecha | **Competidor y socio posible a la vez.** Es el primer actor de esta KB que ocupa las dos casillas. Para una cuenta AAA norteamericana, la pregunta *"¿y si Ubisoft les vende esto?"* **va a aparecer**: la respuesta es que Motherbrain no tiene producto anunciado y que la integración con el pipeline del cliente no es lo que un publisher rival va a hacer por él |
| ⚠️ **Cifra de despidos en conflicto** | Esta KB publica **166**. Fuentes de la misma semana dicen **116 en dos estudios** | **Llevar las dos o ninguna.** No se puede dirimir desde esta sesión |


#### 🆕 cycle 9 (2026-11-21) — el informe que todos citan es de casino, y la brecha líder/trabajador es de 54 puntos

**1. *"The State of AI in Gaming 2026"* (UNLV IGI AiR Hub + KPMG, 9-abr-2026) es de
juego de azar, no de videojuegos.** Muestra: **83 empresas de juego + 113 reguladores**.
AI Maturity Index **45/100**; *land-based* **39** vs *online* **54**; **gobernanza
30/100**, la categoría más baja; **brecha estrategia−gobernanza de 27 puntos**, que los
autores llaman el hallazgo más notable. Uso de genAI: **80% u 81,5%** según fuente
(⚠️ no dirimido).
→ **Oportunidad NA, pero para la práctica de iGaming/casino de Globant, no para game
studios.** Un sector que se autodeclara con estrategia muy por delante de su gobernanza
está describiendo una oferta de *AI governance* en sus propios términos. El desajuste
reportado entre reguladores (que priorizan riesgo de juego problemático y resultados
injustos) y operadores (que lo ponen último) es el gancho de conversación.

**2. La brecha entre quien firma y quien usa está medida.** Vivrato VAIA (**101
líderes senior**, abr–sep 2026, sedes mayormente NA y Europa): **94%** usa IA en ≥1 de
9 etapas, pero **52% puntúa la integración en ≤3/10**. Encuesta GDC del **13-nov-2026**:
**40%** de los trabajadores usa IA y **55% la considera mala noticia**.
→ **Cómo vender en NA:** el patrocinador ya dijo que sí (94%) y ya sabe que no está
integrado (52% en ≤3/10). **La venta no es "adoptá IA", es "convertí el piloto en
pipeline"** — y tiene que incluir cómo se trata al 55% que no la quiere, o el proyecto
se gana al comité y se pierde en el equipo.

**3. Señal de riesgo de marca, medida:** Take-Two **despidió a su head of AI y a parte
del equipo de IA** meses después de que su CEO declarara que abrazaba la IA generativa,
y la compañía afirma que la genAI tiene *"cero participación"* en GTA 6.
→ **Lectura para un pitch:** en NA, anunciar IA **y** recortar personal en la misma
ventana es un riesgo reputacional conocido por los compradores. La propuesta de Globant
debe separar explícitamente **capacidad** de **reducción de headcount**, y conviene que
lo haga por escrito en la propuesta.

**4. Dato de adopción que este pass NO pudo confirmar:** no se encontró una cifra 2026
de adopción de IA por *jugadores* en NA comparable a la de LATAM (PGB) o a la de China
(Niko). **Gap declarado**, no silencio: el argumento NA se sostiene con datos de
*industria*, no de *jugador*.

#### 🆕 2026-11-21 (cycle 8) — el informe-faro de "AI in gaming" de NA **es de casino**, y un incumbente muerto que sigue primero en las búsquedas

🔴 **Hallazgo de ingesta, no de mercado.** Las cuatro búsquedas regionales de
este pass devolvieron en primer lugar **"The State of AI in Gaming 2026"**
(**UNLV International Gaming Institute + KPMG**, primer informe de referencia del
rubro, con sede en Las Vegas): *AI Maturity Index* que **no pasa de 45 sobre
100**, adopción amplia con **gobernanza rezagada**, y una **divergencia medida
entre reguladores e industria** — los reguladores ubican el riesgo de **juego
problemático** y de **resultados injustos para el jugador** entre los más altos;
la industria, **al final de su propia lista**. El informe además expresa
**escepticismo sobre la autorregulación** del sector.

⛔ **Ese informe es del rubro APUESTAS/CASINO, no de videojuegos**, y por eso
ninguna de sus cifras entra al tamaño de mercado ni a la adopción de esta KB.
En inglés regulatorio y de mercado, *"gaming"* significa **juego de azar**; es la
razón por la que domina cualquier búsqueda de *"AI gaming regulation 2026"*.
**El riesgo es alto justamente porque el informe es bueno:** cuantitativo,
reciente y de una institución seria. Una KB de videojuegos que lo ingiera como
propio **se corrompe con material de primera calidad**.

🟢 **Dónde sí sirve, dicho con su alcance:** si Globant atiende a un cliente de
**iGaming/casino en NA** —un vertical real y grande en la región— esas tres
cifras son el mejor artefacto de venta disponible, y la **brecha
regulador-industria sobre quién considera grave el daño al jugador** es
literalmente el riesgo que el cliente no está mirando. **Pero es otro vertical,
con otros compradores y otra lista de plataformas** (ver la frontera declarada en
`verticals/solutions.md`).

🔵 **Nota de regulación, sin novedad respecto del cycle 7:** no apareció en este
pass ninguna obligación **federal** nueva específica de AI en videojuegos. Lo que
el cycle 5 documentó (obligación de *runtime*) y el cycle 7 (censo de Steam con
su muestra) **sigue siendo la base**. **Un pass sin novedad regulatoria se
escribe como tal**, no se rellena.

⛔ **`openai/gym` está ARCHIVADO** (37.237 ★, el repo de RL más estrellado de
origen NA). Sucesor mantenido: `Farama-Foundation/Gymnasium`, ya en la KB. Se
anota **porque con 37 k ★ encabeza cualquier búsqueda de "RL environments"** y es
la clase de propuesta que llega sola a una sala. **11º caso en esta KB de
"muerto pero recomendado".**

#### 🆕 2026-11-21 (cycle 7) — el censo de Steam se puede citar con su muestra, y el sentimiento del jugador entra en conflicto

- 🟢 **El 49,9% de octubre ahora tiene censo detrás, con numerador y
  denominador.** Un censo de **53.597 releases** da **30,8% de los juegos
  publicados en 2026 hasta julio** con disclosure de AI, contra **19,9% (2025)**
  y **10,9% (2024)**. El **49,9% de octubre-2026** es el dato mensual (≈2× el
  26,4% del mismo mes del año anterior). ⇒ **la serie es la venta, no el pico**:
  10,9 → 19,9 → 30,8 es una pendiente que un comité entiende sin discusión.
- 🔴 **Y el sesgo por tamaño de título está medido, lo que invierte el pitch:**
  **47%** de los títulos con **menos de 10 reviews** declaran AI, contra **14%**
  de los que tienen **1.000 o más**. ⇒ **el disclosure es inversamente
  proporcional al tamaño del estudio**, confirmando por tercera vez el hallazgo
  de esta KB. Para un cliente AAA, declarar lo pone en el 14%: **la conversación
  no es técnica, es de riesgo de marca**, y hay que llevarla preparada.
- ⚠️ **Dato de método que hay que decir en la sala: el disclosure de Steam es
  autodeclarado y no auditado.** Mide **lo que los estudios eligieron declarar**,
  no uso real de AI. Cualquier proyección de mercado construida sobre esa serie
  hereda ese sesgo.
- 🔴 **CONFLICTO DECLARADO, no resuelto — el rechazo del jugador.** Esta KB
  publica **~85% de rechazo** (Quantic Foundry) como dato central de NA. Este
  pass encuentra lo contrario en dos muestras distintas: **43% de ~3.800
  usuarios de Steam no tiene problema en comprar un juego con disclosure de AI**
  (GameDiscoverCo), y otra lectura del mismo material dice que **sólo 31% sí
  tiene problema**. 🔵 **No se elige una cifra**: miden cosas distintas
  (rechazo declarado en abstracto vs. decisión de compra ante una etiqueta
  concreta) y es exactamente la distinción que importa comercialmente.
  ⇒ **Para la propuesta: el rechazo abstracto es alto, la fricción de compra
  medida es mucho menor.** Usar las dos cifras y nombrar la diferencia es más
  creíble que citar la que convenga — y evita que el cliente llegue con la otra.
- 🟡 **La adopción declarada por los devs tiene tres números y el conflicto es de
  pregunta, no de realidad:** GDC 2026 (**2.300+** respondentes) **≈36%**; otra
  encuesta 2026 **29%** (bajando desde 36%); y un sondeo de **Google Cloud a 615
  developers** da **~90%** "usa AI de alguna forma". ⇒ **La horquilla 29–90% no
  es ruido: es el redactado de la pregunta.** "¿Usás gen AI en tu trabajo?" y
  "¿hay AI en algún punto de tu pipeline?" son dos mercados distintos. **Pedir
  siempre el redactado antes de citar el número.**
- 🟢 **9 de cada 10 trabajadores de la industria quieren MÁS disclosure que un
  sí/no** (muestra de **826**; sólo **28,4%** cree que un sí/no alcanza). ⇒ el
  **reporte de cobertura fechado** de la Receta 7 no es sólo un entregable
  regulatorio: es lo que la propia plantilla del cliente pide.

#### 🆕 2026-11-21 (cycle 6) — tres piezas que faltaban: el múltiplo, el tamaño del mercado y la brecha regulador-industria

- 🟢 **El argumento de valuación es un argumento de North America.** La tesis de
  **AlixPartners** (múltiplos **2-3×** para quien integre AI sobre IP fuerte, a
  fin de 2026) se dirige a un mercado de capitales que está acá: los publishers
  cotizantes y el venture. **Es el único argumento de esta página que se puede
  llevar a un board y no a un equipo.** Atribuido como proyección, no como dato.
- 🟡 **Tamaño: NA ≈ 40% de un mercado de gen AI en gaming de ~US$ 2.210 M en
  2026** (≈ US$ 880 M; las fuentes difieren en la participación exacta). Leído
  junto a KPMG —*"las integraciones más profundas están en el back-end, no en
  contenido generado de cara al jugador"* (Scott Purdy)— da la forma de la
  oferta: **el dinero de NA está en pipeline, herramientas y ops, no en NPCs
  generativos**. Que es exactamente lo contrario de donde está el entusiasmo de
  producto, y coherente con el 3% de declaraciones de Steam que corresponden a
  generación **en runtime**.
- 🔴 **La brecha regulador-industria tiene cifra, y es grande** (sector *gambling*
  regulado de NA, no videojuegos): **58% de los reguladores duda de que la
  industria pueda autorregular su uso de AI**, y **sólo 13% conoce algún operador
  licenciado con una política interna de AI responsable** (KPMG). **Índice de
  madurez de gobernanza de AI: 30 sobre 100** (SIGMA, *State of AI in Gaming
  2026*, informe benchmark inaugural). ⇒ **la gobernanza es el producto
  faltante del mercado, no la capacidad técnica**, y hay un número para decirlo:
  87% de los operadores no tiene política escrita. Para AI Studios es una oferta
  de *AI governance as a deliverable* con demanda medida.
- 🟡 **Adopción declarada, con su alcance:** **90%** de los desarrolladores
  integra AI en su workflow según la encuesta **Google Cloud / Harris Poll**
  (615 desarrolladores) — ⚠️ **sólo cinco países en la muestra**, así que es
  comparable con el 40% de GDC únicamente si se dice eso. Globant ya cita
  públicamente el **97%** del *Google Cloud Games Report* para assets, testing y
  localización: **las dos cifras son de la misma casa y de la misma familia de
  instrumentos**, y conviene usar una sola para no competir consigo mismo.
- ⚫ **Gap medido, no silencio:** no se encontró ninguna regulación **federal**
  de EE.UU. específica de AI en videojuegos. Lo que hay es (a) leyes **estatales**
  de *companion chatbots* —ver abajo— y (b) regulación de **gambling**, que es
  otro sector. Buscado: `AI gaming North America 2026 adoption regulation
  players`, más seguimiento de ESRB y FTC. **La capa federal no existe; la
  estatal sí y es la que obliga.**

#### 🆕 2026-11-21 (cycle 5) — NA dejó de tener sólo políticas de tienda: ahora tiene una obligación de *runtime*

Hasta ayer esta sección decía, con razón, que en NA *"lo que manda no es una ley:
es la política de una tienda"*. **Eso cambió, y no vino de una ley de AI sino de
las leyes estatales de *companion chatbots*.**

| Estado | Instrumento | Firmada | Vigente | Qué obliga que afecta a un NPC con LLM |
|--------|-------------|---------|---------|----------------------------------------|
| **Washington** | HB 2225 | 24-mar-2026 | **1-ene-2027** | Recordatorios periódicos de estatus AI en sesiones largas, **con intervalo más corto para menores**. Incluye **acción privada** (modelada sobre *My Health My Data*), sin daños estatutarios |
| **Connecticut** | SB 5 → **Public Act 26-15** | 27-may-2026 | 🟡 fuentes discrepan: 1-oct-2026 vs 1-ene-2027 | Recordatorio **≥1×/hora para menores y ≥1×/3 horas para adultos**, más **protocolo de crisis** (riesgo de suicidio, autolesión o violencia) con derivación al **988** |
| **Idaho** | S 1297 *Conversational AI Safety Act* | 31-mar-2026 | **1-jul-2027** | Marco de seguridad para AI conversacional |

**Por qué esto es distinto de todo lo anterior en esta KB.** El Art. 50 europeo y
la ficha de Steam se resuelven **fuera del juego**: un cartel, un campo en la
store, un manifiesto de procedencia. Un recordatorio **cada hora dentro de la
sesión** se resuelve **en el loop del juego**: hay que llevar reloj de sesión,
distinguir si el jugador es menor, elegir un canal de presentación que no rompa
la inmersión, y registrar que se mostró. **Es la primera obligación de NA que
toca el runtime del NPC.** Receta 13 en `compose/patterns.md`.

🟡 **El gap honesto, y es grande:** ninguna de las tres leyes dice
explícitamente que un **NPC de videojuego** sea un *companion chatbot*. El
artículo del *Harvard Journal of Sports & Entertainment Law* (ago-2026) plantea
exactamente esa pregunta y **la deja abierta**. ⇒ Lo vendible **no es
"cumplimiento"**: es **preparación barata** — construir el reloj de sesión y el
flag de minoría ahora cuesta días, y litigar la definición después cuesta el
proyecto. Decirlo así, sin inflarlo.

🆕 **Contexto regulatorio adyacente (sector gambling, no videojuegos):** KPMG
*AI in Gaming 2026* reporta que la actividad regulatoria se concentra en NA y
Europa Occidental y que **58% de los reguladores cree que la industria no puede
autorregular su uso de AI**. Aplica a iGaming, **no** a videojuegos; se anota
porque es el clima en el que se legisla lo que sí aplica.

🆕 **Tamaño relativo:** un estimado sitúa a NA en **~39,8% del mercado de AI in
gaming** — la región más grande. Es un estimado de una sola casa, sin alcance
declarado; usar con la regla de la muestra de arriba.

---

**Lo que manda aquí (todavía, y sigue siendo cierto): la política de una tienda.**

- **Steam reescribió su política de disclosure el 17-ene-2026** y separó dos cosas
  que antes iban juntas: las **herramientas AI usadas durante el desarrollo** (p.ej.
  GitHub Copilot) quedan **explícitamente exentas** de disclosure, y el **contenido
  generado por AI que se le entrega al jugador** debe declararse. Hay además un
  reporte in-game, pero sólo cubre contenido *live-generated*.
- 🔴 **Penetración del etiquetado — serie actualizada y mucho más alta.** Censo
  ampliado (~63.600 releases, el de octubre era ~53.600): **13,2% (2024) → 24%
  (2025) → 40,1% (2026 hasta octubre)**, y **49,9% en el mes de octubre-2026**
  solo. Contra-conteo independiente con datos de SteamDB: **1.821 de 3.661
  releases = 49,7%** — dos métodos, misma conclusión. Hace un año el mismo mes
  estaba en **26,4%**: el ritmo casi se duplicó en doce meses. **La proyección
  "más de la mitad para 2027-2028" que este archivo citaba en octubre se cumplió
  con más de un año de anticipación.**
- 🟢 **Y la composición del etiquetado es la parte vendible:** de las
  declaraciones, **63% es artwork**, **30% texto y traducciones**, **8% sólo
  página de tienda** y **apenas 3% declara generación durante el gameplay**. O
  sea: el mercado etiqueta *arte y texto de producción*, no AI en runtime. Quien
  venda "AI en el juego" está en el 3%, no en el 50%.
- 🟢 **El etiquetado es inversamente proporcional al tamaño del título:** **47%**
  de los juegos con <10 reviews declaran AI, **27%** de los de 100-999 y sólo
  **14%** de los de 1.000+. Los estudios grandes declaran menos — porque usan
  menos, o porque tienen más que perder. Dato de negociación con un publisher.
- ⚠️ **Una declaración no es una medida de uso**, y una ausencia de etiqueta no
  prueba ausencia de AI. El 49,9% cuenta **declaraciones**.
- ⚠️ **La cifra de ~36% que circula para Steam Next Fest NO es de Valve**: es un
  conteo de terceros (2.593 de 7.145 títulos filtrados por base de datos). Valve
  nunca publicó un conteo de disclosure para ninguna edición. No usarla como dato
  oficial en un deck.
- **iGaming / gambling es un carril regulatorio separado y más maduro:** varios
  estados de EE.UU. y provincias canadienses **incentivan** el monitoreo con AI
  para detectar daño al jugador. Pero una encuesta a reguladores encuentra que la
  mayoría **duda de que la industria pueda autorregular su uso de AI**, y sólo una
  fracción conocía operadores licenciados con políticas internas de AI responsable.
- **Barrera citada por analistas de mercado:** el estatus de copyright del contenido
  generado por AI sigue sin resolverse, y eso frena el uso en assets publicables.
- 🆕 **Reguladores de juego: actividad concentrada aquí y en Europa occidental**
  (informe UNLV/KPMG). **Alberta** publicó en **enero de 2026** guías de detección
  de bots y de ventaja desleal, y **Colorado** cambió reglas el mismo mes. Bélgica,
  Finlandia e Italia, junto con varios estados y provincias de NA, **incentivan**
  el monitoreo con AI para proteger al jugador. Pero los reguladores declaran
  **visibilidad limitada** sobre cómo se despliega la AI y poca confianza en poder
  supervisarla: hay mandato y no hay instrumento. Ése es el hueco de consultoría.
- 🆕 **Adopción corporativa vs agéntica, medida aparte:** **>80%** de las empresas
  del sector usan AI generativa para contenido e insights, mientras **la adopción
  agéntica sigue siendo limitada**, frenada por compliance y seguridad del jugador.
  La brecha entre "usamos genAI" y "tenemos agentes en producción" es el mercado.
- 🔴 **Sentimiento del jugador, y es el número más duro de la región:** una encuesta
  de Quantic Foundry sobre **~2 millones de jugadores** reporta **~85% con actitud
  negativa** hacia la gen AI en juegos. Contrastar con LATAM (48,8% de aceptación)
  más abajo: **no es el mismo mercado y no se le vende igual.**

| Oportunidad NA | Driver | Fit Globant |
|----------------|--------|-------------|
| **Pipeline AI "no declarable"** | La reescritura de 17-ene-2026 exime las herramientas de desarrollo. Hay un espacio limpio: acelerar build, QA, localización y ops sin disparar disclosure ni backlash. | Es exactamente la capa interna donde Globant ya opera. Ver `compose/patterns.md` → Receta 2 y 6. |
| **Auditoría de disclosure pre-lanzamiento** | 30.8% de los releases ya etiqueta y el criterio cambió hace 9 meses. Muchos estudios no saben de qué lado cae su propio pipeline. | Servicio corto y vendible: inventario de assets por procedencia + mapeo contra el survey de Steam. |
| **AI responsable para operadores iGaming** | Los reguladores dudan de la autorregulación y pocos operadores tienen política interna. | Marco de AI responsable + monitoreo de daño al jugador, auditable. |
| **Anti-fraude conductual** | Carril iGaming NA con supervisión activa. | GNNs sobre red de jugadores + detección server-side. |


#### 🆕 Oportunidad del pass 2026-11-22: **auditoría COPPA de rutas de datos de menores** (la única del pass con obligación ya vencida)

Desde el **22-abr-2026** un juego estadounidense dirigido a menores necesita
**consentimiento parental verificable separado y no empaquetado** antes de
mandar datos de un chico a un tercero **para entrenar IA** (COPPA enmendada,
90 FR 16970). Casi ningún NPC conversacional en producción fue diseñado contra
ese requisito, porque el requisito es posterior al patrón.

- **Entregable:** inventario de rutas de datos del título (qué sale del
  dispositivo, hacia qué proveedor, bajo qué términos de entrenamiento) +
  veredicto por ruta + plan de remediación.
- **Las tres salidas técnicas, y hay que presentarlas juntas:** (1) inferencia
  **on-device** — elimina la divulgación, no el consentimiento; (2) contrato con
  **opt-out de entrenamiento verificable**; (3) rediseño para que el NPC **nunca
  transmita entrada libre** del jugador (intents cerrados en vez de texto).
- **Por qué vende:** no es una mejora, es una exposición legal con fecha pasada.
  Y el comprador no es el director creativo: es **legal + privacy**, que tiene
  presupuesto propio.
- **Composición:** Receta 26 (`compose/patterns.md`).

### EMEA

#### 🆕 cycle 20 (2026-11-25) — el canal de cobro: la DMA ya cerró su modelo, y **una corrección al cycle 19 sobre el AI Act**

**Apple unificó a todos los desarrolladores de la UE en un solo modelo el
1-ene-2026.** Lo medido:

- La **Core Technology Fee** (0,50 € por instalación arriba del millón) fue
  **reemplazada por la Core Technology Commission (CTC) del 5 %** sobre ventas de
  bienes y servicios digitales.
- Se suma una **Initial Acquisition Fee del 2 %** para usuarios nuevos en los
  primeros 6 meses ⇒ la tasa efectiva más baja queda en **~7 %**, no en 5 %.
- Los *Store Services* quedaron en **dos niveles** (el Tier 1 con menos
  servicios, p. ej. sin actualizaciones automáticas).
- 🔴 **Pregunta regulatoria abierta:** la Comisión Europea anunció que revisaría
  si la estructura nueva cumple la DMA. **No se encontró una decisión final
  publicada**, y no se afirma acá que la haya.

🔴 **Corrección al cycle 20 de lo que publicó el cycle 19 sobre el AI Act Art. 50(2).**
El pass anterior escribió *"Art. 50(2): 2-dic-2026 ⇒ faltan 10 días"* sin su
alcance, y el alcance cambia a quién le aplica:

- Las obligaciones de transparencia del Art. 50 **ya están en vigor desde el
  2-ago-2026**, no en diciembre.
- El **2-dic-2026** es una **ventana de transición acotada**: aplica **sólo** a
  sistemas que generan contenido sintético y que **ya estaban en el mercado antes
  del 2-ago-2026**, para agregarles el **marcado legible por máquina**.
- La Comisión publicó guías **no vinculantes** el **20-jul-2026**.
- Sanciones: hasta **15 M€ o 3 %** de la facturación mundial, el mayor.
- 🟢 **El carve-out que más le sirve a un juego:** cuando el contenido deepfake
  forma parte de una obra **evidentemente artística, creativa, satírica o
  ficcional**, la obligación de divulgación **se reduce a declarar que el
  contenido existe**. Un juego es el caso de libro de "obra ficcional".

Así que para un juego ya publicado antes de agosto de 2026 con voces o arte
generados, **la fecha del 2-dic-2026 sí muerde** (marcado legible por máquina),
pero para un título nuevo la obligación ya venía corriendo desde agosto. **El
cycle 19 tenía la fecha bien y el alcance incompleto.**

🟢 **Oportunidad EMEA:** la DMA es la única región donde el modelo de cobro ya
está **cerrado y cuantificado** (5 % CTC + 2 % IAF + Store Services por nivel).
Es la región donde se puede construir el caso de negocio del D2C **con números
firmes**, y donde `f-droid/fdroidserver` (AGPL-3.0) es la única pieza open source
que permite **operar un marketplace propio** — legal desde la DMA, y sin
equivalente permisivo.

#### 🔴 Oportunidad del pass cycle 19 (2026-11-22): **faltan 10 días para el 2-dic-2026**, y la Comisión ya dijo que apuntar ofertas a jugadores vulnerables puede ser práctica **prohibida**

**1. El reloj concreto (y es el dato más perecedero de esta KB).** Los sistemas de
IA generativa **puestos en el mercado antes del 2-ago-2026** tienen hasta el
**2-dic-2026** para cumplir el **marcado legible por máquina + mecanismo de
detección** del Art. 50(2). Los puestos en el mercado **el 2-ago-2026 o después
no tienen período de gracia alguno**. El resto de las obligaciones del Art. 50 rige
sin transición desde el 2-ago-2026, **también para sistemas heredados**.

> **Al cierre de este pass (2026-11-22) quedan 10 días.** La Receta 7 de esta KB
> (inventario + plan fechado) ya no alcanza como entregable: para un título vivo con
> generación en runtime, lo único que se puede prometer en 10 días es el
> **inventario cerrado + el marcado donde técnicamente se pueda + el memo que
> documente lo que no**. Una de las notas consultadas (Cloud Security Alliance,
> sep-2026) dice con todas las letras que un marcado robusto que sobreviva a
> edición, truncado, paráfrasis y copiar-pegar es *"an open technical question"*, y
> que **retro-encajarlo es más difícil que diseñarlo desde el principio**. Eso es
> una frase para poner en el memo, no para esconder.

**2. Lo nuevo de este pass, y es el gancho de la capa de decisión.** Un análisis
sectorial (DLA Piper, 2026) sobre el **Art. 5** —prácticas prohibidas, no de alto
riesgo— enumera como potencialmente prohibido *"AI-driven dynamic targeting of
financially vulnerable players with higher-risk offers or bonus structures"* y los
juegos diseñados para producir resultados de *"near-miss"*.

⚠️ **Frontera de esta KB, que el cycle 8 declaró y acá hay que repetir:** esa
fuente es del **sector de apuestas** (*gambling*), no de videojuegos. Se publica
porque es **el análogo legal más cercano** que existe hoy a lo que hace la capa de
decisión de un F2P, y porque la redacción del Art. 5 no distingue por sector — no
porque un videojuego sea un casino.

**3. Lo que ya estaba y sigue en pie.** El **Digital Fairness Act** como regulación
de consumo que alcanza el *diseño adictivo* y las *mecánicas aleatorizadas*
(⚠️ **fuentes en conflicto sobre su estado**: el cycle 10 de esta KB lo registró
como publicado el 11-nov-2026; una fuente consultada hoy lo describe todavía como
borrador previsto para el Q4-2026. **No se resuelve acá**), y la agenda 2026 de la
**EGDF** —IA conducida por humanos, mercados justos, soberanía europea— que es el
lenguaje con el que un estudio europeo compra: la capa de decisión
**auto-hospedada** es un argumento de soberanía antes que de costo.

**Qué se vende en EMEA con esto:** el *flag* de personalización no es una feature de
producto, es **el punto de control que hace demostrable** que no se está apuntando
ofertas a jugadores vulnerables. Sin él, la respuesta a un regulador es una
declaración; con él, es un registro.


> 🆕 **cycle 18 (2026-11-22).** Refinamiento operativo del Art. 50, a **10 días** del
> 2-dic-2026: la excepción de videojuegos es **conjuntiva**. El contenido efímero
> generado en tiempo real queda exento del marcado *"donde el marcado sea
> técnicamente inviable **Y** se informe al usuario dentro de la experiencia"*.
> Esta KB publicaba la excepción (Receta 27) **sin el "Y"**. ⇒ **Cumplir una sola de
> las dos condiciones no exime**, y la segunda —informar en la experiencia— es
> trabajo de producto, no de legales: es la Receta 13 (disclosure por reloj de
> sesión). Fuente: comentario legal concordante (Travers Smith, DLA Piper, William
> Fry) sobre las guías de la Comisión del 20-jul-2026 + el escrito de Video Games
> Europe del 3-jun-2026. ⚠️ `eur-lex.europa.eu` **sigue sin resolver** (código 000):
> verificar contra el texto oficial antes de cualquier entregable firmado.

#### 🆕 Oportunidad del pass cycle 17 (2026-11-22): **el pipeline europeo ya está instalado y su licencia es el problema** — entrar por ahí, no por el engine

**El gancho, en una línea:** *"Su AYON es FSL desde enero de 2025 y su Kitsu es AGPL:
nosotros construimos del lado Apache-2.0 del corte, y el entregable queda de ustedes,
redistribuible, sin tocar el servidor."*

- **Entrada técnica**: addons sobre `ayon-core` / `ayon-python-api` (Apache-2.0),
  nunca parches al servidor (Receta 45).
- **Entrada legal**: la cláusula de *professional services* de la FSL está escrita y
  permite operar el servidor del cliente. Decirlo **antes** de que lo pregunten.
- **Entrada de soberanía**: las dos plataformas se despliegan on-prem; el competidor
  es SaaS norteamericano. El dato de producción no sale del estudio.
- **Fecha para la propuesta**: si exigen grant permisivo sobre el servidor,
  **2027-01-30** es el día en que AYON 1.7.0 convierte a Apache-2.0.

#### 🆕 Oportunidad del pass cycle 16 (2026-11-22): **el marcado legible por máquina del Art. 50 vence el 2 de diciembre de 2026 — en diez días** — y hay un conflicto de fecha entre KBs que no se puede cerrar desde esta sesión

**El gancho, en una línea:** *"Si tu juego ya estaba en el mercado, el plazo para
marcar de forma legible por máquina el contenido generado vence el 2 de diciembre.
Son diez días, y el entregable realista ya no es cumplir: es el inventario y el
plan fechado."*

**Lo medido este pass (fuentes jurídicas secundarias, nov-2026, concordantes
entre sí):**

- El **Art. 50 aplica desde el 2-ago-2026** y **no fue aplazado por el Digital
  Omnibus**.
- **Transición para sistemas ya en el mercado: hasta el 2-dic-2026** para el
  marcado legible por máquina. **Hoy faltan 10 días.**
- Guidelines finales de la Comisión sobre el Art. 50: **Comunicación C(2026) 5054,
  aprobada el 20-jul-2026**, adopción formal pendiente.
- Multas de hasta **15 M€ o el 3%** del volumen de negocio mundial.
- Alcance extraterritorial: proveedores, *deployers*, importadores y distribuidores
  que pongan sistemas en el mercado de la UE **o cuya salida se use en la UE** →
  **un estudio de UK o de EE. UU. que sirve a jugadores europeos está dentro.**

> 🔴 **CONFLICTO ENTRE KBs DE ESTA FLOTA, DECLARADO Y SIN RESOLVER.** La KB
> *enterprise* registra **dic-2027** como fecha corregida del Art. 50 y califica el
> "2-ago-2026" de dato obsoleto que reaparece pass tras pass. Las fuentes de
> videojuegos de **noviembre de 2026** recogidas aquí dicen lo contrario, y son
> varias e independientes entre sí.
>
> **No se puede cerrar desde esta sesión:** `WebFetch` no resuelve hosts generales
> (2ª medición del gap), así que **no hubo acceso al texto primario en EUR-Lex**.
> **Acción obligatoria antes de cualquier entregable a un cliente EMEA: leer el
> texto del Reglamento y el estado del Omnibus en fuente oficial.** Las dos fechas
> están registradas; **ninguna de las dos debe citarse a un cliente sobre la base
> de esta KB.** Lo que no depende de cuál gane: el inventario de contenido generado
> hay que tenerlo igual, y hoy no lo tiene nadie.

> 🟢 **Corroboración INTERNA, que inclina la balanza sin cerrarla:** el análisis
> del cycle 15 **de esta misma página** ya había establecido, con más precisión que
> cualquier fuente de este pass, que el **2-dic-2026 cubre sólo el Art. 50(2) y
> sólo los títulos puestos en el mercado antes del 2-ago-2026** —uno lanzado
> después **nunca tuvo período de gracia**— y que **el Art. 50(1) vence para todos
> desde agosto**. Es decir: **la KB de gaming es internamente consistente con
> agosto de 2026 desde antes de este pass**, y la discrepancia es con la KB
> *enterprise*, no dentro de ésta. Sigue sin leerse el texto primario, así que la
> instrucción de arriba no cambia: **verificar en EUR-Lex antes de un entregable.**

**Dónde sigue el desacuerdo sustantivo** (no de fecha): si el diálogo de NPC
generado requiere etiqueta. Un análisis jurídico polaco sostiene que el test
depende de **la diversidad etaria real del público del juego** — es decir, *el
mismo NPC tiene obligaciones distintas según quién juegue*. Otra lectura sostiene
que el creativo estilizado de un videojuego raramente califica como *deepfake*.
**Es exactamente el tipo de pregunta que un inventario resuelve y una opinión no.**

**Por qué el plano de datos de este pass es el instrumento:** la obligación es
demostrar **qué se generó, con qué sistema y cuándo**. Eso es un registro de
eventos con retención, no un documento. ClickHouse + el registro de esquemas de
Iglu dan la traza; C2PA (ya en esta KB) da el marcado del artefacto. Las dos
mitades, y ninguna sirve sola.


#### 🆕 Oportunidad del pass cycle 15 (2026-11-22): **el EAA ya se está litigando — y el alcance para videojuegos está EN DISPUTA, que es precisamente el entregable**

**El gancho, en una línea:** *"La Ley Europea de Accesibilidad es aplicable desde
junio de 2025, las primeras demandas se presentaron en Francia en noviembre de
2025, y tus abogados y los nuestros no se ponen de acuerdo sobre si tu juego
está cubierto. Eso se resuelve con un dictamen, no con una suposición."*

| Qué | Detalle |
|---|---|
| **Estado real** | El **EAA es aplicable en toda la UE desde el 28-jun-2025**. Los Estados miembro ya pueden fiscalizar: derecho de reclamo del consumidor, sanciones y multas. **Primeras demandas presentadas en Francia en nov-2025**; Países Bajos y otros planificaron actividad para 2026 |
| 🔴 **El alcance está CONTESTADO — se publica el desacuerdo, no una conclusión** | **(a)** *Deque* (proveedor de accesibilidad) sostiene que **EN 301 549 aplica a los videojuegos online igual que a web y móvil**. **(b)** El **IGDA Game Accessibility SIG** sostiene que los juegos **en general no están cubiertos**, y que para consola y PC el EAA exige sobre todo **chat de texto junto al chat de voz**, o conversación total en videochat. **(c)** *Sprung Studios* sostiene que el EAA **no regula el contenido del juego**, pero **sí las plataformas y lanzadores** desde los que se accede |
| **Cómo tratarlo** | (a) es un proveedor con incentivo a ampliar el alcance; (b) es el cuerpo del sector; (c) es intermedia y compatible con (b). ⇒ **Posición de esta KB: tratar (b)+(c) como la lectura de trabajo y (a) como el escenario adverso.** Nunca presentar ninguna como "lo que dice la ley" |
| **Entregable 1 — el que se vende** | **Dictamen de alcance país por país.** El EAA se transpuso **nacionalmente** y las leyes de implementación difieren entre mercados (Bird & Bird). No existe "cumplimiento EAA" europeo: existe cumplimiento alemán, francés, español. **Es exactamente la forma de entregable que el carácter fragmentado vuelve defendible por horas** |
| **Entregable 2** | **Auditoría contra EN 301 549 v3.2.1**, que es la versión contra la que hay que medir **hoy**. Priorizar los requisitos de **comunicación bidireccional**, que son los que la lectura (b) sí considera cubiertos y los que coinciden con el CVAA de NA ⇒ **un solo trabajo técnico, dos jurisdicciones** |
| **Entregable 3** | **Plan de transición a v4.1.1**, que mueve el estándar a **WCAG 2.2** y añade **seis requisitos nuevos**. Es la primera versión escrita pensando en el EAA |
| **Carve-out a verificar** | Según el IGDA, los juegos **ya en el mercado antes de la fecha límite estarían exentos**. La redacción es ambigua y la fecha exacta no se pudo confirmar. ⇒ **si el cliente tiene catálogo, esto vale más que toda la auditoría junta**: verificarlo es la primera pregunta, no la última |

> 🔴 **GAP DECLARADO, no se vende como resuelto:** **no se pudo confirmar que
> EN 301 549 v4.1.1 haya sido citada en el Diario Oficial de la UE.** Su efecto
> legal depende de esa cita; un proveedor la pronostica para **nov-2026** (este
> mes) y no hay confirmación. **Hasta verificarlo en el DOUE, la versión con
> efecto es v3.2.1** y v4.1.1 no puede ser un requisito contractual.

> ⚠️ **Contradicción heredada, otra vez presente en los resultados de este pass:**
> una fuente legal vuelve a fechar las obligaciones del **Art. 50** del AI Act el
> **2-ago-2026** y otra el **2-ago-2027**. Esta KB ya corrigió esa fecha en passes
> anteriores. **Séptimo pass consecutivo en que la versión obsoleta reaparece**:
> no está retrocediendo, y hay que asumir que el cliente la tiene.

#### 🆕 Oportunidad del pass cycle 14 (2026-11-22): **el pipeline de assets viable en Europa es un conjunto más chico, y hay que demostrarlo con la licencia en la mano**

**El gancho, en una línea:** *"En Europa, dos de las tres rutas estándar de
generación 3D están cerradas — y una de las dos está cerrada por una licencia
europea."*

| Qué | Detalle |
|---|---|
| **Disparador 1** | ⛔ **La ruta china no existe acá.** `Tencent-Hunyuan/Hunyuan3D-2.1` (4.519 ★) es el **cuarto** repo de Tencent cuya licencia declara en su primera línea que **no aplica en la UE ni el Reino Unido**. No es restrictiva: es **ausencia de grant**, ni siquiera para evaluar |
| **Disparador 2** | 🔴 **La ruta MIT está cerrada por Inria + MPII.** El renderer de splatting sobre el que se apoya la generación 3D abierta lleva la *Gaussian-Splatting License* (Francia + Alemania, en registro ante la **APP**): *"THE USER CANNOT USE, EXPLOIT OR DISTRIBUTE THE SOFTWARE FOR COMMERCIAL PURPOSES"* |
| **Entregable 1** | **Mapa de rutas viables en UE/UK**, con el archivo y los bytes de cada grant: `Bingeljell/image-to-3dlab` (Apache-2.0), `OpenDCAI/GameFactory-3A` (Apache-2.0), `OpenMOSS/MOSS-TTS` (Apache-2.0 **incl. pesos**), `ace-step/ACE-Step` (Apache-2.0), `nv-tlabs/FlexiCubes` (Apache-2.0) |
| **Entregable 2** | **Inventario de herramientas AI clasificadas por impacto en el jugador** — el mismo artefacto que pide el Art. 50 y que el *AI Gaming Charter* maltés (18-sep-2026) formaliza para el carril casino. Se reusa la **forma**, no el régimen |
| **Entregable 3** | 🆕 **Segmentación interna de EMEA.** El *State of AI in Gaming 2026* registra **cero** actividad regulatoria de AI en **Europa Central y del Este** y en **Medio Oriente y África**. Para esa mitad de EMEA el gancho de compliance **no aplica** y el pitch tiene que ser coste y pipeline. Entregable: dos versiones de la propuesta, no una |
| **Por qué ahora** | El plazo del Art. 50 para sistemas **ya en el mercado** (2-dic-2026) está a días; la obligación para sistemas nuevos corre desde ago-2026 |

#### 🆕 Oportunidad del pass cycle 12 (2026-11-22): **auditoría de parecido real (*deepfake*) y de defendibilidad del activo generado**

**El gancho:** *"El Art. 50 que estás resolviendo cubre tu NPC. No cubre el
estadio, la ciudad ni la cara que tu pipeline generó — y eso alcanza a tu estudio
aunque no esté en la UE."*

| Qué | Detalle |
|---|---|
| **Disparador 1** | El régimen de marcado alcanza contenido generado que pueda pasar por **persona, lugar o evento real**, con **alcance extraterritorial** si se distribuye en la UE. La Comisión pide juicio **caso por caso** (realismo, contexto, audiencia): no hay regla general que un cliente pueda aplicar solo |
| **Disparador 2** | 🆕 **Donde el contenido es enteramente generado por AI, la protección de copyright puede ser limitada o inexistente.** No es cumplimiento: es **el activo del cliente**. Un estudio que generó su arte al 100% **puede no poder defenderlo** |
| **Entregable** | Inventario de assets con parecido real + clasificación por riesgo + **dictamen de defendibilidad** por activo, con la recomendación de arquitectura que sale de ahí: **humano en el loop documentado** convierte un activo indefendible en uno defendible |
| **Por qué vende** | Las dos piezas se venden al mismo comprador (*legal* + *studio head*) y la segunda **no es un costo de cumplimiento: es protección de propiedad intelectual**. Es el único argumento EMEA de este pass que no es defensivo |
| **Reconciliación de fechas, para la sala** | Guías del Art. 50 **aprobadas el 20-jul-2026**, **adoptadas formalmente** (versiones lingüísticas) el **5-nov-2026**. 🟢 No es contradicción: son dos pasos del mismo instrumento. Decirlo así evita parecer impreciso |
| **Y el canal que llega antes que la ley** | Tiendas y plataformas ya exigen disclosure **por contrato**. Para muchos clientes es la obligación que muerde primero |


#### 🆕 Cycle 11 (2026-11-22) — las Guías del Art. 50 **se adoptaron formalmente** y abren una excepción para el contenido en tiempo real; Malta propone una Carta de AI (carril casino)

🔵 **Todo lo regulatorio de esta subsección es secundario**: WebFetch da
`ENOTFOUND` en todos los hosts externos de esta sesión. Contrastar contra el
texto oficial antes de firmar. Desarrollo completo arriba, en *"Lo que cambió —
pass cycle 11"*.

| Hallazgo | Dato | Qué cambia |
|---|---|---|
| 🔵 **Las guías tienen adopción formal y número** | Contenido aprobado **20-jul-2026** (`C(2026) 5054 final`); **adoptadas formalmente el 5-nov-2026** como **`C(2026) 7702 final`**, sin cambios de fondo. **Code of Practice on Transparency of AI-generated Content**, final el **10-jun-2026**, voluntario, **≥ 2 técnicas legibles por máquina**. ⚠️ Una fuente sólo reconoce el 20-jul ⇒ **citar las dos fechas** | La KB citaba el borrador. Ahora hay un instrumento adoptado que nombrar |
| 🟢 **Excepción escrita para contenido EFÍMERO EN TIEMPO REAL** | Las guías finales **amplían** las excepciones al marcado del Art. 50(2). El contenido efímero generado en tiempo real —**se citan videojuegos y VR explícitamente**— queda exento **sólo si** (a) el marcado **no es técnicamente factible** **y** (b) hay disclosure **de experiencia o de sesión** | 🔴 **El cambio de alcance más grande del pass.** El 3% de declaraciones de Steam que es generación en gameplay — el caso técnicamente más caro — **tiene vía de excepción**. Se convierte en **memo de factibilidad + disclosure de sesión** (Receta 23), no en sprint de marcado |
| 🟢 **Pipelines cerrados: sólo se marca la salida final** | Salidas en flujos de producción **en circuito cerrado** para cine, animación, **juegos** y publicidad **no se marcan en etapas intermedias** | Reduce el inventario de la Receta 7: *concepts*, variantes y pasos intermedios **no son puntos de marcado** |
| 🟢 **El marcado de TEXTO sigue siendo limitado, y las guías lo admiten** | 🔵 El comentario legal sobre las guías señala que **no resuelven** la limitación de las soluciones robustas de marcado, **en particular para texto** | Es el **fundamento técnico** del memo de infactibilidad para diálogo de NPC por LLM. No es alegato de parte |
| 🔴 **Lo que NO tiene excepción** | El **Art. 50(1)** (avisar que se interactúa con AI) **rige para todos desde el 2-ago-2026** y no admite excepción por infactibilidad técnica — **al contrario: la excepción de marcado lo EXIGE** como condición | El aviso **no es lo que se ahorra: es lo que se paga a cambio** |
| 🎰 **Carril casino (≠ videojuego)** | **Malta (MGA)** abrió consulta en **sept-2026** sobre una **AI Gaming Charter voluntaria**: disclosure de uso de AI, chequeos de equidad y **supervisión humana** de decisiones automatizadas que afecten acceso o gasto. Documento público: `mga.org.mt/app/uploads/AI-Gaming-Charter-2026.pdf` | **No aplica a videojuegos.** Se registra porque es el primer regulador de EMEA con un instrumento de AI específico del sector, y porque un cliente de iGaming **sí** lo va a preguntar |


#### 🆕 cycle 9 (2026-11-21) — **el sprint del Art. 50(2) es MÁS ANGOSTO de lo que esta KB venía diciendo**

Corrección de alcance, no de fecha. El **2-dic-2026 (quedan 11 días)** sigue siendo
real, pero esta KB lo venía presentando como un sprint que alcanza a casi todo el
catálogo. Las fuentes de este pass acotan el alcance en cuatro puntos:

1. **El plazo del 2-dic-2026 cubre sólo el deber de *marcado legible por máquina* del
   PROVEEDOR (Art. 50(2))**, y sólo para sistemas **puestos en el mercado antes del
   2-ago-2026**.
2. **Sistemas puestos en el mercado desde el 2-ago-2026: sin período de gracia.**
   Cumplen desde el lanzamiento. No hay "ventana de diciembre" para lo nuevo.
3. **Los deberes del *deployer* NO fueron pospuestos**: rigen desde el 2-ago-2026.
4. 🔑 **El contenido sintético ya generado antes del 2-ago-2026 no debe marcarse
   retroactivamente.** Esto **saca trabajo del sprint**: no hay que volver sobre los
   assets viejos ya generados.

**La consecuencia que cambia la Receta 7:** un estudio de videojuegos es, casi siempre,
**deployer** de un modelo generativo de un tercero, **no provider**. Lo que le aplica
entonces es avisar que se interactúa con IA (Art. 50(1)) y etiquetar deepfakes
(Art. 50(4)) — **no** el marcado técnico del proveedor del 50(2), que es el que vence
el 2 de diciembre. **Triar provider vs deployer ANTES de correr el sprint** evita
vender once días de pánico por un deber que puede no aplicar al cliente.

**Contexto adicional medido este pass:**

- Guidelines finales de la Comisión sobre transparencia: ~**30-jul-2026**. Código de
  Buenas Prácticas de Transparencia (junio 2026) declarado **adecuado**.
- Los firmantes del Código deben montar un **mecanismo interoperable después de
  diciembre** → marcar a las apuradas ahora puede tener que rehacerse. Conviene elegir
  el mecanismo mirando ese requisito.
- La Comisión sostiene que **en principio los videojuegos con IA no tienen obligación
  bajo el AI Act**: la mayoría de la IA de juego (pathfinding, balanceo, entrenamiento
  de NPC) es **riesgo mínimo**.
- Lo que **sí** está prohibido (Art. 5, no "alto riesgo"): **NPCs diseñados para empujar
  gasto in-game**. Multas hasta **35 M€ o 7% de la facturación global**.
- Pendiente: **Digital Fairness Act**, propuesta esperada en **Q4-2026**, apuntando a ⛔ **SUPERADO el 2026-11-22: el DFA se publicó el 11-nov-2026 como COM(2026) 1100. Ver la sección del cycle 10 al inicio de este archivo.**
  diseño adictivo y mecánicas aleatorias, con protecciones específicas para menores.
- Lado casino (frontera del homónimo): la **UKGC subió el rating de riesgo de los
  proveedores de software de juego de bajo a medio el 30-jul-2026**, citando documentos
  falsos y deepfakes usados para eludir KYC.

**Oportunidad EMEA reencuadrada:** el entregable que vende no es "cumplí con el Art. 50
antes del 2 de diciembre" sino **"te digo en qué artículos caés y en cuáles no"**. Para
la mayoría de los estudios la respuesta honesta reduce el trabajo — y es justamente por
eso que genera confianza para el proyecto siguiente.

**Dato de adopción EMEA:** Vivrato VAIA encuentra que **Europa reporta adopción más
profunda que Norteamérica** (n=101 líderes, abr–sep 2026). Es el único dato comparativo
NA/EMEA que esta KB tiene, y apunta en contra de la intuición habitual.

#### 🆕 2026-11-21 (cycle 8) — **4ª confirmación independiente del 2-dic-2026**, y el host primario **no resuelve**: lo que este pass puede y no puede afirmar

⛔ **Gap de canal declarado antes del dato.** `www.dlapiper.com` y `chambers.com`
—las dos fuentes de las que los cycles 5, 6 y 7 tomaron el andamiaje legal del
Art. 50— **no resuelven por el proxy de esta sesión** (`ENOTFOUND`), igual que
`www.unlv.edu` y `github.blog`. **Este pass no leyó ninguna fuente primaria de
EMEA.**

🟡 **Lo que sí vale, dicho con su límite.** Los snippets de búsqueda de este pass
reproducen, de forma independiente de los tres passes anteriores:

- **Art. 50 aplicable desde el 2-ago-2026.**
- **Los sistemas ya en el mercado antes del 2-ago-2026 tienen hasta el
  2-dic-2026** para el **marcado legible por máquina del Art. 50(2)**.
- **Digital Omnibus** publicado el **24-jul-2026**, con parte de las
  obligaciones corridas a **2027** y otra parte cayendo el 2-ago-2026.
- **Guidelines del Art. 50 de la Comisión aprobadas el 20-jul-2026**, y la
  lectura de que **para un single-player de audiencia adulta el diálogo con NPCs
  puede quedar sin etiquetado adicional**.
- **Digital Fairness Act**: propuesta formal prevista para **Q4-2026**. ⛔ **SUPERADO el 2026-11-22: el DFA se publicó el 11-nov-2026 como COM(2026) 1100. Ver la sección del cycle 10 al inicio de este archivo.**

⇒ **Cuenta como la 4ª confirmación independiente del 2-dic-2026** (cycles 5, 6, 7
y éste, con fuentes distintas cada vez). **No cuenta como verificación
primaria**, y nadie debe citar este pass como lectura de fuente. **Quedan 11
días** desde hoy. Las precisiones de alcance que ya tiene la KB —**Art. 111(4)**
como base legal, el **2-dic-2026 cubre sólo el Art. 50(2) y sólo el catálogo
pre-2-ago-2026**, el **Art. 50(1) vence para todos desde agosto**, y la
**partición por edad de la audiencia** del cycle 6— **se sostienen sin cambios**.

🔧 **Lo único que cambia de raíz en EMEA este pass es de licencia, y toca el
componente que firma el marcado.** `compose/patterns.md` describía
`contentauth/c2pa-rs` y `c2pa-python` como **"Apache-2.0"** a secas, mientras
`agents/top.md` ya publicaba desde el cycle 7 —y este pass **re-verificó en
bytes: `LICENSE-MIT` 1.083 B + `LICENSE-APACHE` 11.335 B, idénticos**— que son
**MIT *o* Apache-2.0 a elección del integrador**. **Corregido.**

**Por qué importa comercialmente y no es cosmético:** en un grant dual **elige
quien integra**, y para un entregable de cliente **se elige Apache-2.0 por su
grant expreso de patente** — MIT no lo tiene. Es decir: en el único componente
que materialmente **firma** el marcado legible por máquina del Art. 50(2), la KB
estaba publicando una licencia que **no permitía elegir la protección de
patente**. Una fila que dice "Apache-2.0" **pierde la elección**; una que dice
"MIT" **pierde la patente**.

🔵 **Recordatorio de alcance que esta KB ya estableció y este pass no cambia:**
el Acta **no nombra ninguna tecnología de marcado**. **C2PA es elección de
ingeniería, no requisito legal**, y presentarlo como lo segundo es vender un
substrato como un certificado — el mismo error que la KB de enterprise documentó
con OSCAL y el Anexo IV.

#### 🆕 2026-11-21 (cycle 7) — el deadline que esta KB vende tiene por fin su base legal, y un alcance mucho más chico

- 🔴 **Resuelto el 2-dic-2026, con artículo.** El **Art. 50 aplica desde el
  2-ago-2026**. El **2-dic-2026** es una **transición acotada**: cubre
  **únicamente el Art. 50(2)** —el marcado *machine-readable*— y **sólo para
  sistemas puestos en el mercado o en servicio ANTES del 2-ago-2026**. La base
  legal que citan las fuentes de forma concordante es el **Art. 111(4)**.
  🔵 **Cuatro+ fuentes secundarias independientes coinciden y nombran el mismo
  artículo**; la **fuente primaria no se pudo leer** (`digital-strategy.ec.europa.eu`
  y las firmas legales dan **ENOTFOUND** en esta sesión). ⇒ **se cita el
  Art. 111(4) como lo que las fuentes dicen, y se verifica contra el texto
  oficial antes de cualquier entregable firmado.**
- 🔴 **La consecuencia reencuadra la Receta 7 por tercera vez, y en contra del
  cliente:** un título **lanzado después del 2-ago-2026 NUNCA tuvo período de
  gracia** — debía marcado **desde el día de su puesta en el mercado**. El
  encuadre "quedan 11 días" **sólo aplica al catálogo viejo** y, aplicado a un
  lanzamiento reciente, **oculta un incumplimiento que ya ocurrió**. ⇒ **el
  inventario necesita una columna nueva: fecha de puesta en el mercado, antes o
  después del 2-ago-2026.** Es la columna que decide si el cliente tiene 11 días
  o una brecha abierta hace meses.
- 🟢 **Y la transición no toca el Art. 50(1):** la obligación de avisar al
  jugador que habla con un sistema de AI **vence desde agosto para todos**. Un
  cliente que creía tener hasta diciembre para su NPC conversacional **está
  mirando el deadline equivocado**.
- 🟢 **El AI Act no impone ninguna tecnología de marcado.** ⇒ **C2PA es una
  elección de ingeniería defendible, no un requisito legal.** Esta KB debe
  proponerlo por auditabilidad e interoperabilidad, **nunca** como "lo que la ley
  exige". Hay además una vía formal alternativa: el **Code of Practice on
  Transparency of AI-Generated Content**; quien no lo suscriba debe demostrar
  cumplimiento por *"alternative equivalently adequate means"*.
- 🟡 **Las guías de la Comisión son del 20-jul-2026 y NO son vinculantes**, y dan
  un ejemplo que **reduce alcance en el caso más común del catálogo**: la
  interacción con **NPCs de un juego single-player** puede alcanzar el umbral de
  **"obviedad"** y **no requerir disclosure adicional**. Combinado con el
  hallazgo del cycle 6 (la edad de la audiencia), el alcance del Art. 50(1) se
  decide por **modo de juego × audiencia**, no por título.
- 🔴 **Sanción, con número:** hasta **15 M€ o 3% de la facturación mundial anual**,
  el mayor de los dos. Es la cifra que esta KB no tenía y la que convierte la
  Receta 7 de proyecto técnico en punto de comité.
- 🟢 **La industria europea ya está en la mesa:** **Video Games Europe** presentó
  su escrito a la consulta de las guías del Art. 50 el **3-jun-2026**, y la
  **EGDF** lleva tres prioridades 2026 —*human-led AI*, mercados justos y
  **soberanía europea**— incluyendo **etiquetado más claro del contenido
  asistido por AI**. ⇒ para un cliente europeo, el etiquetado **no es una
  imposición externa**: su propia federación lo está pidiendo. Cambia el tono de
  la reunión.
- ⚫ **Gap que sigue abierto, y es el mismo del cycle 6:** ninguna cifra de
  **adopción o sentimiento de jugadores europeos**. Buscado otra vez, ahora con
  el instrumento corregido (*"video game"* en lugar de *"gaming"*). **EMEA sigue
  siendo la región con el gancho regulatorio más fuerte y la evidencia de
  demanda más débil de esta KB.**

#### 🆕 2026-11-21 (cycle 6) — el matiz que cambia **quién** tiene que etiquetar: la **edad de la audiencia**

- 🔴 **El Art. 50 no obliga a todos por igual: el alcance depende de a quién
  apunta el juego.** Análisis de **WH Partners** (*"Same NPC, Different Rules:
  How a Player's Age Changes Their Responsibilities Under the AI Act"*): un
  juego **single-player dirigido a adultos** puede **no** necesitar etiquetar el
  diálogo de un NPC, mientras el mismo NPC en un título con audiencia de menores
  sí. ⇒ **El entregable del sprint del 2-dic-2026 no es "etiquetar todo": es
  determinar, por título, la audiencia y de ahí la obligación.** Esto
  **reencuadra la Receta 7 por segunda vez**: el inventario necesita una columna
  más —**clasificación de audiencia por edad**— y esa columna es la que decide
  si una fila necesita trabajo de ingeniería o sólo un registro. Es también la
  tercera vez que esta KB se encuentra con lo mismo: **el valor (aquí, "hay que
  etiquetar") no significa nada sin su alcance (aquí, "a quién le vendés el
  juego").**
- 🟡 **El reloj del Omnibus sigue en disputa, y ahora con dos fuentes
  enfrentadas.** La enmienda **AI Omnibus** habría entrado en vigor el
  **27-jul-2026**, con Anexo III (alto riesgo) corrido a **2-dic-2027** y Anexo I
  a **2-ago-2028** (fuente sectorial de iGaming). Otra fuente del mismo sector
  sostiene que el grueso del Acta, **incluido el Anexo III**, aplica desde
  **2-ago-2026**. 🔵 **El conflicto que la KB de enterprise registró se
  mantiene y ahora también aparece dentro de gaming** — el pass del 2026-11-20
  había anotado que las fuentes de gaming *corroboraban* el calendario
  post-Omnibus; con esta segunda fuente ya **no hay corroboración limpia**.
  ⚠️ **Nada de esto mueve el 2-dic-2026 del Art. 50(2)**, que es el deadline que
  importa y sobre el que no hay disputa. **Para la propuesta: ejecutar contra el
  2-dic-2026 y tratar el calendario de alto riesgo como abierto**, con la fecha
  conservadora (2-ago-2026) como supuesto hasta tener fuente primaria.
- 🟢 **Tres Estados miembro ya obligan o fomentan monitoreo automatizado de
  jugador: Bélgica, Finlandia e Italia** (KPMG; sector *gambling*). Es el
  **primer caso de esta KB donde la AI es lo que el regulador PIDE**, no lo que
  restringe: modelos que marcan *markers of harm* y disparan límites o contacto
  humano, con efectos medibles reportados por operadores europeos. ⇒ **oferta
  invertida para EMEA**: en videojuegos la conversación es *cómo cumplir pese a
  la AI*; en el adyacente regulado es *cómo cumplir usando AI*. Mismo stack de
  detección conductual que la Receta 3.
- 🟡 **Malta: la AI Gaming Charter voluntaria tiene fecha disputada.** Una fuente
  la data como lanzada el **18-sep-2026**; otra la describe aún como
  **consulta**. La KB ya registraba "consulta abierta, sin guía publicada a
  sept-2026". **Se deja el conflicto declarado, no se elige una fecha.**
- ⚫ **Gap medido:** no se encontró **ninguna** cifra de adopción o sentimiento
  **de jugadores europeos** sobre AI en juegos. Buscado: `AI gaming EMEA Europe
  2026 adoption regulation players`. Lo que devuelve EMEA es **regulación y
  comentario legal**, casi nada de demanda. ⇒ **EMEA es, en esta KB, la región
  con el gancho regulatorio más fuerte y la evidencia de mercado más débil**, y
  hay que decirlo así en vez de rellenar con el ~85% global de Quantic Foundry,
  cuya muestra tampoco está desagregada por región.

#### 🆕 2026-11-21 (cycle 5) — quedan **11 días**, y aparece el matiz de la Comisión que hay que decir en la sala

- 🔴 **11 días** al 2-dic-2026. Nada que se empiece hoy como proyecto termina
  antes. **Triage, inventario y plan fechado** (Receta 7, ya reencuadrada).
- 🟡 **Matiz nuevo, y es de la Comisión Europea:** según el análisis de
  *AI and Games* citando a la Comisión, **un videojuego habilitado con AI no
  tiene, por sí mismo, obligación bajo el AI Act**; lo que cae bajo el Acta son
  **casos específicos** —contenido sintético, interacción declarada con una AI— y
  se invita a códigos de conducta voluntarios. Bird & Bird añade que el
  **contenido efímero en tiempo real** (en juego o en VR) queda exento del
  marcado **sólo cuando marcarlo no es técnicamente factible**, con disclosure
  **a nivel de sesión** en su lugar.
  ⚠️ **Esto convive en tensión con lo que esta KB publicó ayer** (la Comisión
  señalando NPCs que empujan gasto in-game como práctica prohibida del Art. 5).
  Las dos lecturas son compatibles —**Art. 5 prohíbe una conducta concreta;
  Art. 50 impone transparencia a casos concretos; no hay un régimen "videojuegos"**—
  pero **presentarlas como una sola es lo que hace refutable el deck.** Decir:
  *"no hay obligación general; hay obligaciones por caso, y dos de esos casos son
  habituales en un juego con AI"*.
- 🔴 **Exclusión territorial que bloquea una pieza del stack, descubierta hoy:**
  la licencia de `Tencent-Hunyuan/HY-World-2.0` (`License.txt`, 16.858 B) dice
  *"THIS LICENSE AGREEMENT DOES NOT APPLY IN THE EUROPEAN UNION, UNITED KINGDOM
  AND SOUTH KOREA"*. ⇒ **el world model abierto más grande de Tencent no es
  usable en un engagement EMEA**, y el sustituto es `SkyworkAI/Matrix-Game`
  (MIT en la raíz, **Apache-2.0** en `Matrix-Game-4/`). Es la primera vez que
  esta KB encuentra un **grant que se cae por región**, y es exactamente el tipo
  de cosa que no aparece en ningún comparativo de world models.
- 🟢 **Lo que sí es vendible en EMEA con 11 días:** el **inventario de
  procedencia** y el pipeline de firma C2PA — que hoy sabemos que es **MIT *o*
  Apache-2.0**, no sólo Apache-2.0 (ver `repos/foundations.md`), así que entra
  incluso en clientes con política de sólo-MIT.
- 🟡 **Riesgo legal adyacente:** Bird & Bird señala que el contenido **generado
  enteramente por AI** puede tener **protección de copyright limitada o nula**.
  Para un estudio, el riesgo no es sólo la multa: es **no poder defender su
  propio asset**. Y en el radar: **Digital Fairness Act** (borrador Q4-2026),
  DSA sobre UGC y protección de menores, y reglas de loot boxes. ⛔ **SUPERADO el 2026-11-22: el DFA se publicó el 11-nov-2026 como COM(2026) 1100. Ver la sección del cycle 10 al inicio de este archivo.**

---

🔴 **La única región con una fecha límite dura (texto del pass 2026-11-20, a 12 días).**

- **EU AI Act, Artículo 50 (transparencia): aplicable desde el 2-ago-2026.** Quedó
  **fuera** del aplazamiento general, así que ya está vigente.
- 🔴 **2 de diciembre de 2026: fin del período de gracia para el marcado
  *machine-readable* del Art. 50(2)** en sistemas ya puestos en el mercado. Hoy es
  **2026-11-20: quedan 12 días.**
- 🔴 **REENCUADRE OBLIGATORIO DE LA OFERTA.** En octubre este archivo vendía un
  "sprint de marcado" de 3-5 semanas contra este deadline. **A 12 días, ese
  entregable ya no cabe y proponerlo es vender un incumplimiento.** Lo que sí cabe,
  y es honesto: (a) **inventario de exposición** — qué SKUs del catálogo publicado
  llevan contenido generado y qué metadata tienen hoy, entregable en días, no
  semanas; (b) **marcado del subconjunto crítico** priorizado por riesgo y
  visibilidad; (c) **plan fechado post-deadline** con la brecha documentada, que
  es lo que un regulador y un auditor valoran frente a un cumplimiento fingido.
  Decirle al cliente que llega completo al 2-dic es el error, no la oferta.
- **Alto riesgo / Anexo III se movió a 2 de diciembre de 2027** (se esperaba
  2-ago-2027). Hay por tanto **dos relojes distintos** y confundirlos es el error
  que se ve en el mercado.
- 🟢 **Corroboración cruzada entre KBs de esta familia:** el aplazamiento de alto
  riesgo vía *Digital Omnibus* (a 2027 y 2028) aparece ahora en fuentes legales
  independientes consultadas para gaming. La KB de **enterprise** registró el
  2026-11-20 un conflicto no resuelto porque sus fuentes daban 2-ago-2026 para
  alto riesgo; **las fuentes de gaming son consistentes con el calendario
  post-enmienda** (Art. 50 vigente desde 2-ago-2026, alto riesgo a 2027/2028), que
  es la lectura que esta KB ya sostenía. El conflicto de enterprise se explica por
  fuentes pre-enmienda. ⚠️ **`eur-lex.europa.eu` sigue sin ser alcanzable desde
  este entorno, así que el registro primario sigue SIN LEER en 5 passes y 2 KBs.**
  No poner ninguna de las dos fechas en una propuesta sin verificar el texto oficial.
- **Qué obliga concretamente en un juego:** disclosure al jugador cuando interactúa
  con un sistema de AI, salvo que sea obvio (Art. 50(1)); y marcadores **visibles y
  legibles por máquina** en copy de marketing, arte promocional e imágenes de juego
  generadas por AI.
- 🆕 🔴 **HALLAZGO NUEVO Y MÁS GRAVE QUE LA TRANSPARENCIA: hay un caso de uso de
  gaming en las prácticas PROHIBIDAS (Art. 5), no en alto riesgo.** La Comisión
  Europea citó como ejemplo de práctica prohibida los **NPCs generados por AI
  diseñados para empujar al jugador a gastar más dentro del juego**. Y comentario
  legal sostiene que la **AI de optimización de engagement aplicada a jugadores
  menores de edad** caería también en Art. 5. La diferencia de régimen es total:
  **alto riesgo se gestiona con documentación; prohibido no se gestiona, se
  cancela.** Si un cliente tiene un NPC conversacional conectado a la tienda o a la
  economía del juego, ésa es la primera conversación, antes de cualquier marcado.
- 🆕 **Y viene una segunda norma, específica de diseño de juego:** la Comisión
  planea un borrador de **Digital Fairness Act para el Q4 de 2026**, apuntando a
  **diseño adictivo y mecánicas aleatorizadas** (loot boxes) en productos
  digitales, juegos incluidos. ⛔ **SUPERADO el 2026-11-22: el DFA se publicó el 11-nov-2026 como COM(2026) 1100. Ver la sección del cycle 10 al inicio de este archivo.** Para un publisher europeo, el monetization design
  pasa a ser materia regulada, no sólo materia de ética.
- 🆕 **iGaming EMEA:** la **Malta Gaming Authority** abrió consulta sobre una
  **AI Gaming Charter voluntaria**; a septiembre de 2026 no había publicado guía
  específica de AI. Voluntario hoy, piso de expectativa mañana.
- ⚠️ **Desacuerdo no resuelto entre fuentes legales:** si la dificultad adaptativa,
  la reacción al comportamiento del jugador y las historias personalizadas caen en
  "alto riesgo". Una guía dice que las herramientas generales de generación de
  assets no caen automáticamente; otra sostiene que los sistemas adaptativos sí.
  **Se registra como abierto, no se resuelve a favor del cliente.**
- **Las plataformas llegaron antes que el regulador:** las reglas de transparencia
  de AI ya estaban en los contratos de las tiendas antes de que la norma aplicara.
  El cumplimiento contractual es la ruta rápida; el legal es el piso.
- **Exposición comercial real:** los jugadores reaccionan críticamente al contenido
  AI que pueden identificar, así que el riesgo se concentra en **assets creativos
  que ve el jugador**, no en el pipeline.

| Oportunidad EMEA | Driver | Fit Globant |
|------------------|--------|-------------|
| 🔴 **Sprint de marcado Art. 50(2)** | Deadline 2-dic-2026, catálogo ya publicado, requiere marcado machine-readable. Ventana de 5 semanas. | Entregable acotado y fechado. Ver `compose/patterns.md` → **Receta 7**. |
| **Separación de relojes 50 / Anexo III** | Dos fechas (2-dic-2026 y 2-dic-2027) y fuentes que las confunden. | Assessment de alcance: qué sistema cae en transparencia y qué en alto riesgo, con la ambigüedad declarada. |
| **Arquitectura "sin perfilado" por diseño** | Si los sistemas adaptativos caen en Anexo III, el dynamic difficulty basado en perfil del jugador se vuelve caro. | Sacar la decisión de dificultad del LLM y llevarla a un modelo auditable y determinista. |


#### 🆕 Oportunidad del pass 2026-11-22 (a): **readiness de monetización frente al Digital Fairness Act** — ya hay texto publicado que citar

COM(2026) 1100 se publicó el **11-nov-2026**. Todavía tiene que pasar Parlamento
y Consejo, y aplicaría **12 meses después de entrar en vigor** — es decir, cae de
lleno en el horizonte de un proyecto que arranque en 2027.

- **Entregable:** auditoría del diseño de monetización contra **Art. 15**
  (recompensa variable de pago: inventario de ítems + **probabilidades
  publicadas** + **switch de desactivación accesible** + **bloqueo para
  menores**) y contra **Arts. 14–17** (diseño adictivo: push no disparados por
  el usuario y rachas, **apagables**).
- **El ángulo que lo diferencia de un trabajo legal:** las probabilidades y el
  switch son **features de producto**, no cláusulas. Alguien tiene que construir
  el panel de odds, el toggle, el gate por edad y la telemetría que prueba que
  funcionan. Eso es ingeniería, y es lo que vende AI Studios.
- ⚠️ **Honestidad obligatoria en la sala:** la prohibición de loot boxes de pago
  **es para menores**; no hay prohibición general para adultos. Si el cliente
  llega con el titular equivocado, corregirlo **es** el primer entregable.
- **Riesgo máximo citable:** **6% de facturación global anual**.

#### 🆕 Oportunidad del pass 2026-11-22 (b): **sustitución de world models bloqueados por territorio**

Verificado byte a byte este pass: **las tres releases abiertas de world models
de Tencent** (`HY-World-2.0`, `Hunyuan-GameCraft-1.0`, `HY-WorldPlay`) llevan la
misma cláusula — *"THIS LICENSE AGREEMENT DOES NOT APPLY IN THE EUROPEAN UNION,
UNITED KINGDOM AND SOUTH KOREA"*. En EMEA **no hay grant**: no es una licencia
incómoda, es ninguna licencia.

- **Entregable:** barrido del stack del cliente buscando pesos y código con
  exclusión territorial, y **plan de sustitución medido** —
  `SkyworkAI/Matrix-Game` (MIT en raíz + Apache-2.0 en `Matrix-Game-4/`),
  `nv-tlabs/lyra` (Apache-2.0), `deeplethe/utopia` (Apache-2.0), ninguno con
  cláusula de territorio.
- **Por qué nadie lo detecta solo:** el badge de GitHub dice `NOASSERTION`, no
  "prohibido en tu país". La cláusula está en la **primera línea de un
  `License.txt` con L mayúscula y t minúscula**, que la sonda habitual
  (`LICENSE`, `LICENSE.txt`) **no pide**. Esta KB le erró con la sonda estándar
  en este mismo pass antes de enumerar.
- **Composición:** Receta 25 (`compose/patterns.md`).

### APAC

#### 🆕 cycle 20 (2026-11-25) — el canal de cobro: **Japón ya lo abrió, y el 90 % de los desarrolladores no lo usó**

La **Mobile Software Competition Act (MSCA)** japonesa está **en vigor desde el
18-dic-2025** (una fuente da 17-dic-2025; la diferencia es zona horaria). Obliga a
Apple y Google a permitir tiendas de terceros, *billing* alternativo y *link-outs*.
Tarifas medidas que las plataformas pusieron en respuesta:

| Plataforma | Tarifa específica de Japón | Condición |
|---|---|---|
| **Apple** | **21 %** | si el dev usa pago in-app de terceros |
| **Apple** | **15 %** | sobre transacciones vía web linkeada por el dev |
| **Apple** | — | el IAP de Apple **debe mostrarse junto** a las alternativas |
| **Google Play** | **4 %** de *service fee* reducido | si el usuario elige la opción no-Google; se muestran lado a lado |

🔴 **El dato que vale más que las tarifas: más del 90 % de los desarrolladores
japoneses NO adoptó ni los pagos alternativos ni los marketplaces**, y
agrupaciones de la industria alegan que los términos tarifarios de Apple y Google
**podrían violar la propia MSCA**. ⚠️ Esa alegación **no se pudo verificar contra
fuente primaria** (no se accedió al texto de la JFTC) y se publica como alegación,
no como hecho.

🟢 **Y ahí está la oportunidad APAC, que es la más clara de las cuatro:** el canal
está **legalmente abierto y comercialmente vacío**. La barrera no es el permiso —
ya existe— es que **integrar un segundo canal de cobro, con su validador de recibo
y su reconciliación, es trabajo de backend que el estudio japonés no hizo**. Es
exactamente el entregable de las Recetas 52 y 53. Un mercado con permiso
concedido, 90 % de no-adopción y una razón técnica identificada es el mejor perfil
de demanda de este pass.

**Regulación AI de la región (contexto, no canal de cobro):** Corea — *AI Basic
Act* **en vigor 22-ene-2026**; Vietnam — ley de AI **en vigor 1-mar-2026**; Taiwán
— *AI Basic Act* aprobado **dic-2025**; China — reglas específicas sobre
recomendación algorítmica, deepfakes y AI generativa (el cycle 19 fechó las
*Rules for Price Behavior* en vigor 10-abr-2026).

⚠️ **Gap informado de APAC, dicho explícitamente:** las búsquedas de regulación
de **AI específica de gaming** en APAC devuelven **cero actividad registrada** —
una de las fuentes de benchmark lo dice con esas palabras ("no recorded activity
in Asia-Pacific"). Lo que sí hay es regulación de AI **general** (arriba) y de
**competencia móvil** (MSCA). El silencio es de la regulación sectorial de AI en
juegos, no de la cobertura de este pass.

#### 🔴 Oportunidad del pass cycle 19 (2026-11-22): **China reguló el PRECIO algorítmico en abril, y el requisito operativo es "poder apagarlo y poder documentarlo"**

**Lo que entró en vigor el 10-abr-2026** (y rige por 5 años): las *Rules for Price
Behavior on Internet Platforms*, emitidas por **NDRC + SAMR + CAC**. Lo medido en
las fuentes de este pass:

- Prohíben a las plataformas usar **datos, algoritmos o reglas de plataforma** para
  cobrar **precios distintos por el mismo bien en condiciones similares** — lo que
  en China se llama *"discriminación por big data"*.
- Los reguladores pueden exigir **sistemas internos de cumplimiento de precios**,
  **retención de datos de precios y transacciones**, cooperación en investigaciones
  y **registro (filing) del algoritmo**.
- Las plataformas deben **divulgar sus reglas de ranking y de recomendación** a los
  comercios participantes.
- Lectura de un analista citado en las fuentes: se espera que la práctica **se
  vuelva encubierta** —cupones diferenciados en lugar de precios diferentes—, lo que
  es exactamente el riesgo que un *flag* auditable ayuda a acotar.

**Lo que ya existía y ahora cobra sentido junto con lo anterior:** las
*Algorithm Recommendation Provisions* (2022) reconocen el derecho del usuario a
**desactivar la recomendación personalizada** y exigen protección reforzada para
**menores** y otros grupos vulnerables. Y las reglas de 2026 sobre servicios de IA
**antropomórfica / de compañía** (en vigor **15-jul-2026**) agregan protecciones
reforzadas para menores y personas mayores: eso toca directamente a los **NPC
conversacionales** de esta KB, no sólo a la recomendación.

**Corea:** hay **proyectos de ley** —no ley— para restringir la recomendación
personalizada por IA a menores, con la KMCC como autoridad; una nota de prensa de
ago-2026 observa que la función de recomendación se usa *"no sólo en redes sociales
sino también en noticias, juegos y compras"*, y advierte que un alcance vago las
haría inaplicables. Se registra como **señal, no como obligación**.

📉 **Gap medido, explícito:** **no se encontró ninguna regla específica de juegos**
sobre ofertas personalizadas ni en China ni en Corea. La exposición llega por la vía
de **plataformas y precios**, no por la vía de la regulación de juegos — y ésa es la
diferencia que hace que un estudio la descubra tarde.

🟢 **El requisito de arquitectura para un lanzamiento en China, que se deduce de las
tres filas de arriba:** el motor de recomendación tiene que poder (a) **apagarse por
usuario**, (b) **exportar sus reglas de ranking** como documento presentable y
(c) convivir con **precio uniforme**. Un recomendador SaaS de tercero no puede
cumplir (b) de forma creíble; [gorse](https://github.com/gorse-io/gorse)
(Apache-2.0, 9.927 ★) auto-hospedado sí, porque las reglas son archivos del cliente.


> 🆕 **cycle 18 (2026-11-22).** Dos cifras nuevas, y las dos son **de instrumento**,
> no de tendencia. **China:** CADPA (vía cobertura de ChinaJoy 2026) mide **86,36%**
> de empresas usando AI, contra el **~60%** de Niko Partners que esta KB publica. No
> se contradicen: CADPA encuesta **a sus asociadas** y Niko estima **el universo de
> estudios** ⇒ dos denominadores, **llevar las dos o ninguna** (misma regla que con
> las tres cifras de Steam). **Corea:** KOCCA da **41,7% en H1-2025** frente al
> **77,5% del Q2-2026** que esta KB ya tenía ⇒ **la adopción coreana casi se duplicó
> en un año**, y es **la única serie temporal de adopción por región** que esta KB
> puede construir con dos puntos del mismo instrumento. Ése es el argumento APAC más
> fuerte disponible hoy: no el nivel, **la pendiente**. ⛔ **Japón: cero fuentes** en
> esta consulta (ver gaps del cycle 18).

#### 🆕 Oportunidad del pass cycle 17 (2026-11-22): **la automatización de QA permisiva de esta industria es de la región** — se compone, no se importa

**El gancho, en una línea:** *"La base de automatización que ya usan los estudios de
la región es Apache-2.0 y es local; lo que falta no es la herramienta, es el agente
que decide qué probar."*

- **Airtest (9.657★, Apache-2.0, NetEase)** como capa de ejecución + `ramenvr/autopilot`
  (Apache-2.0) como capa de decisión = Receta 19 + Receta 43 sin una sola licencia
  restrictiva.
- ⚠️ **Advertencia que hay que dar, no esconder**: Poco lleva **34 meses** sin commit
  y Airtest **8**. Son bases estables, no dependencias con roadmap: presupuestar el
  mantenimiento del *fork* propio.

#### 🆕 Oportunidad del pass cycle 16 (2026-11-22): **China publicó la primera norma del mundo que apunta al MECANISMO de NPC-compañero, no al contenido generado** — y toca directamente lo que esta KB lleva 16 passes vendiendo

**El gancho, en una línea:** *"Todas las normas de IA que mirabas regulan lo que el
juego MUESTRA. Ésta regula lo que el NPC HACE con un menor, y los videojuegos no
están excluidos."*

**Lo medido (fuente secundaria, ver la advertencia):** las medidas chinas sobre
**IA antropomórfica**, en vigor desde el **15-jul-2026**, incluyen una disposición
que **prohíbe ofrecer a menores servicios de IA construidos sobre relaciones
virtuales**. Según el análisis, **otras jurisdicciones con leyes de *companion AI*
excluyen explícitamente los videojuegos, y el enfoque chino no lo hace**: la
**interacción emocional sostenida** con el jugador es precisamente lo que puede
arrastrar a un NPC con IA dentro del alcance.

> ⚠️ **Procedencia y límite:** el análisis es de **Stanford CodeX, 19-nov-2026**
> —tres días antes de este pass—. **`law.stanford.edu` no resuelve en esta sesión**
> (`ENOTFOUND`), así que esto viene de *snippets* de búsqueda y **no fue leído en la
> fuente**. Es un dato **secundario y sin verificar**. Dicho eso: si se confirma, es
> el hallazgo regulatorio más relevante que esta KB registró para su propio
> producto estrella, porque **las recetas de NPC-compañero de esta KB son
> exactamente el objeto regulado**.

**Lo demás de la región, con su matiz:**

- **Corea — AI Basic Act en vigor desde el 22-ene-2026** (ya registrado). **Matiz
  nuevo de este pass:** el regulador señaló que **2026 funciona como período
  piloto con aplicación limitada**. Cambia la conversación comercial: el año que
  viene no es el año de la multa, es **el año de construir la arquitectura antes de
  que la aplicación empiece**.
- **Vietnam — ley en vigor desde el 1-mar-2026** (ya registrado).
- **NPPA:** su supervisión se extendió **más allá de la publicación de juegos hacia
  la IA y el gobierno del dato**, y mostró flexibilidad tras el rechazo de la
  industria a varios borradores.
- ⛔ **Las 3 horas semanales de menores NO deben citarse como vigentes.** La cifra
  circula en los *trackers* de 2026 pero su fuente se remonta a **2021**. Dato
  caducado hasta que se verifique en fuente NPPA.
- **Japón:** no se encontró norma de IA específica para videojuegos. La verificación
  de edad existe (vía comités de ética en Japón, vía datos de operadora móvil en
  Corea) pero **no es regulación de IA**. **No vender trabajo japonés como
  cumplimiento de IA.**
- ⛔ El informe que afirma **"ninguna actividad regulatoria sobre IA registrada en
  Asia-Pacífico"** es del sector **gambling**, de fuente única, y **se contradice
  con la norma china de arriba**. No usarlo.

**Consecuencia para el plano de datos:** si los eventos de NPC-compañero son la
categoría señalada, tienen que vivir en **su propia tabla con su propio TTL y su
propia partición por jurisdicción** — no mezclados en el flujo general de
telemetría. Eso es la Receta 42, y es una decisión de esquema que cuesta barata
hoy y carísima después.


#### 🆕 Pass cycle 15 (2026-11-22): **GAP MEDIDO — la accesibilidad de videojuegos no aparece en APAC, y eso cambia el pitch**

**Qué se buscó, literalmente:** `AI gaming APAC 2026 adoption regulation players
accessibility Japan Korea China`, en modo extendido.

**Qué devolvió:** regulación general de IA país por país (abajo) y **esports**.
**Cero** resultados sobre accesibilidad de videojuegos, **cero** sobre adopción
de IA por desarrolladores de la región y **cero** sobre adopción por jugadores.

⇒ **Esto se escribe como gap, no como silencio.** No significa que APAC no tenga
normas de accesibilidad —Japón tiene la *Act on Elimination of Disability
Discrimination* y Corea su propia ley— sino que **no están conectadas con el
sector de videojuegos en ninguna fuente que esta sesión pueda alcanzar**. La
consecuencia operativa es directa: **en APAC el gancho de accesibilidad no se
usa**, porque no hay un instrumento sectorial al que anclarlo ni una obligación
que el cliente reconozca.

**Lo que sí está medido en APAC y sigue vigente para el pitch:**

| País | Instrumento | Estado medido | Consecuencia |
|---|---|---|---|
| **Corea del Sur** | *AI Basic Act* | **En vigor desde el 22-ene-2026**. Período de gracia para sanciones de **al menos 1 año**; tope de multa **KRW 30 millones** | La obligación existe hoy; **el riesgo económico empieza en 2027**. Vende preparación, no urgencia de multa |
| **China** | Sin ley general de IA, pero **normas específicas obligatorias**: algoritmos de recomendación, *deepfakes*, IA generativa y **etiquetado de contenido generado** | Vigente. Sanciones de hasta **CNY 50 millones** según una fuente secundaria | El régimen más exigente de la región, y el **etiquetado** toca directamente a los NPCs generativos y a los assets generados |
| **Japón** | *AI Promotion Act* | **Marco de promoción sin sanciones.** El **2º Plan Básico de IA** fue aprobado por el Gabinete el **14-jul-2026**, revisando el de dic-2025 | 🔴 **No se escopea trabajo japonés como compliance.** Es estrategia, no deber exigible. Repetido de passes anteriores y vuelto a confirmar |

**Señal de mercado del período:** los **esports** fueron disciplina en los
*Asian Games 2026* (**22-sep → 2-oct-2026**), con la Japan Esports Union como
coorganizadora. Es relevancia de categoría, no un dato de adopción de IA.

> **Regla que se mantiene:** en APAC, **país por país y nunca una plataforma
> regional**. Los instrumentos son nacionales y no transfieren.

#### 🆕 Oportunidad del pass cycle 14 (2026-11-22): **etiquetado de assets generados para el mercado chino, y el doble bloqueo coreano**

**El gancho, en una línea:** *"China es la única jurisdicción donde la obligación
cae sobre el ASSET, no sobre el NPC — y tu pipeline no sabe qué assets generó."*

| Qué | Detalle |
|---|---|
| **Disparador 1** | **China:** registro obligatorio de servicios generativos + **etiquetado de contenido**, ahora con la cláusula de gobernanza de AI agregada a la Ley de Ciberseguridad en **ene-2026** y multas de hasta **RMB 10 M**. La obligación de etiqueta cae **exactamente sobre el artefacto final** que ve el jugador |
| **Disparador 2** | ⛔ **Corea, doble golpe.** `Hunyuan3D-2.1` **excluye Corea del Sur** del grant, y el **AI Basic Act** está vigente desde el **22-ene-2026** (transparencia + supervisión humana para sistemas de alto impacto; período de gracia reportado de ≥1 año antes de multas, tope KRW 30 M — ⚠️ fuente de compliance-vendor, verificar) |
| **Entregable 1** | **Procedencia por asset**: registro, por cada malla/textura/pista, de qué modelo la generó, con qué licencia y qué grado de intervención humana tuvo. Es el insumo del etiquetado chino y, de paso, del Art. 50 europeo. Precedente de producto: `Bingeljell/image-to-3dlab` ya **escribe la licencia de cada asset que genera** |
| **Entregable 2** | **Matriz proveedor × jurisdicción** para el cliente coreano o con publicación en Corea: qué modelos tiene permitido siquiera evaluar |
| **Entregable 3** | Stack de generación sin dependencia china ni de renderers no comerciales, para clientes que publican en los tres mercados de APAC a la vez |
| **Contexto de mercado** | **Japón** va liviano: *AI Promotion Act* sin sanciones directas y *AI Basic Plan* adoptado en jul-2026 ⇒ el gancho en Japón **no es regulatorio**. Señales de producto: Capcom negó públicamente usar AI generativa en contenido; la desarrolladora de PUBG anunció modo de compañero AI para 2026 |

#### 🆕 Oportunidad del pass cycle 12 (2026-11-22): **"AI sólo en el backend" como oferta explícita — con un AAA japonés respaldándola en público**

**El gancho, y es el más fuerte del pass:** *"Capcom negó públicamente usar AI
generativa en el contenido de sus juegos. Nosotros te entregamos exactamente eso:
toda la productividad, cero AI de cara al jugador."*

| Qué | Detalle |
|---|---|
| **Disparador** | 🔴 **Capcom negó oficialmente** haber implementado AI generativa en el contenido de sus juegos, y el barrido describe un patrón japonés de **AI sólo en el backend** (herramientas internas, pipeline, QA) |
| **Por qué esto es una oferta y no una renuncia** | Converge con el desglose de Unity desde el otro lado: **código 62% vs NPC 40%**. El caso de uso nº1 de la industria **no toca al jugador**. En Japón eso además **elimina el riesgo reputacional**, que es el freno real (la *AI Promotion Act* **no tiene penalidades**) |
| **Entregable** | Pipeline + QA automatizado + tooling de engine, con una **garantía contractual de "cero AI de cara al jugador"** y la evidencia técnica para sostenerla (gate de CI que falla si una ruta generativa llega a runtime) |
| **Corea — reencuadre con cifras nuevas** | AI Basic Act vigente desde el **22-ene-2026**, multas hasta **KRW 30 M**, **período de gracia ≥ 1 año** antes de sancionar. 🟢 **La fecha pasó pero la sanción todavía no muerde: la ventana para llegar en regla sigue abierta**, y ese es el argumento de urgencia honesto |
| **China — trabajo de preproducción, no posterior** | **+550 servicios de AI generativa registrados** ante el regulador a mediados de 2026. El **filing ante la CAC es requisito de entrada**: si el cliente publica en China, entra en el cronograma desde el día uno |
| **Gap declarado** | **No se encontró ninguna fuente de adopción o sentimiento de JUGADORES en APAC** este pass |


#### 🆕 Cycle 11 (2026-11-22) — China acelera aprobaciones y NetEase embarca narrativa generativa; **cero actividad regulatoria de AI en el carril casino de APAC**

| Hallazgo | Dato | Qué hacer con él |
|---|---|---|
| 🟢 **El embudo de aprobación chino se está abriendo, no cerrando** | 🔵 NPPA: **+19% interanual** a mayo-2026 y **~1.571 títulos aprobados hasta septiembre, +23%** sobre 2025 (Niko Partners / prensa sectorial; **sin tabla primaria de NPPA verificable desde esta sesión**, y las fuentes **no coinciden** en el agregado — otra da ~1.766 hasta octubre) | Esta KB registraba la aprobación como **freno**. El dato de 2026 dice que el freno **se está soltando**. Para una cuenta con ambición en China, el cuello de botella ya no es la licencia: es el contenido |
| 🟢 **Narrativa generativa EMBARCADA, no en demo** | 🔵 **NetEase** anunció para *Justice Online Mobile* un **"sistema de narrativa infinita dirigido por AI"**: el jugador crea y juega historias generadas dinámicamente | Se suma a Tencent (Game for Peace, 110 M de usuarios acumulados de NPC-AI) como **segunda prueba de que APAC pone generación de cara al jugador en producción**, no en beta. Es el contraargumento a *"esto no escala"* y a *"los jugadores lo rechazan"* |
| 🔵 **Corea: la referencia de producto tiene fechas** | **Krafton**, personaje **Ella** en *PUBG: Battlegrounds*, **beta limitada del 17-jun al 1-jul-2026**, restringida a jugadores con hardware **NVIDIA RTX** compatible | Caso con ventana cerrada y **gate de hardware**: sirve para dimensionar expectativas de cobertura de jugadores, no sólo de capacidad técnica |
| 🎰 **Carril casino: hueco declarado** | 🔵 El benchmark de *gambling* (n=**83 empresas** y **113 reguladores**) registra actividad regulatoria sobre AI **concentrada en Norteamérica y Europa Occidental**, y **ninguna registrada en Asia-Pacífico** (ni en Medio Oriente/África). En APAC, mesas de juego inteligentes desplegándose en Macao, Manila, Singapur, Auckland y Australia; **Macao más conservador** por incertidumbre sobre privacidad en reconocimiento facial | **Un cero medido, no un silencio.** Para iGaming en APAC **no hay regulador de AI que invocar**, lo que invierte el pitch: no se vende cumplimiento, se vende **gobernanza anticipada** |


#### 🆕 cycle 9 (2026-11-21) — dos cifras chinas que no se pueden conciliar, y un regulador que acelera

**1. La adopción china tiene dos números incompatibles, de la misma casa.** Niko
Partners aparece citado con **60% de los estudios chinos usando IA en sus pipelines**
(informe 2025) y, por otro lado, con **"casi el 90% de los developers en China usando
herramientas de genAI"**. Las definiciones (estudio vs developer, "IA" vs "genAI") y
los años probablemente expliquen la diferencia, **pero con lo disponible en esta sesión
no se pudo dirimir**.
→ **Se registran las dos.** Para un pitch: usar **60%** si se habla de estudios y se
quiere una cifra defendible; **no** usar el ~90% sin la aclaración de que mide
developers y herramientas genAI.

**2. El regulador chino está acelerando, no frenando.** Aprobaciones de la NPPA **+19%
interanual YTD**; Niko proyecta **más de 2.000 juegos aprobados en el año**. Lotes
mensuales medidos: **154 en abril**, **158 en mayo**, **197 en julio** (⚠️ el dato de
julio es de fuente secundaria). Mercado chino proyectado **+4% en 2026**, hacia
**59.800 M USD en 2030**.
→ La narrativa de "China traba los juegos" está **desactualizada**; el cuello de botella
de 2026 es la saturación de IP, no la licencia.

**3. Etiquetado obligatorio, ya vigente.** Las *Measures for Labeling AI-Generated
Content* se publicaron en **marzo de 2025** y rigen **desde septiembre de 2025**. China
lleva **más de un año** con etiquetado de contenido generado por IA en vigor — **antes
que la UE**.
→ **Reutilización directa:** un estudio que ya publica en China **ya tiene** maquinaria
de etiquetado. Esa maquinaria es el punto de partida más barato para el Art. 50 europeo.
**Vender el camino China → UE, no dos proyectos separados.**

**4. Producto de referencia:** Tencent abrió la **beta abierta de *Honor of Kings:
World* en China el 10-abr-2026**, extendiendo una IP saturada hacia un género nuevo. El
analista de Niko apunta además **fatiga de open-world/anime** en el mercado.
→ Para un cliente APAC: la presión competitiva no está en *si* usar IA, está en
**diferenciar una IP madura** — que es un problema de contenido y de live-ops, los dos
lugares donde los agentes rinden.

**5. Marco regulatorio por país (lo medido este pass):**

| País | Instrumento | Estado |
|---|---|---|
| China | *Measures for Labeling AI-Generated Content* | **Vigente desde sep-2025** |
| Japón | *Act on Promotion of R&D and Utilization of AI-Related Technologies* | **Mayo 2025** — ley **promotora**, de marco, no restrictiva ni específica de gaming |
| Corea | Marco de IA: sistemas de alto impacto, **divulgación y etiquetado de genAI**, supervisión humana | Vigente; **no se encontró dato de adopción específico de gaming** |

⛔ **Gap declarado:** no se encontraron cifras 2026 de adopción de IA en gaming para
**Japón ni Corea**. Es ausencia de dato medida, no cobertura.

#### 🆕 2026-11-21 (cycle 8) — Corea pone cifra a lo que pierde, y China deja de ser un caso de adopción para ser un caso de **certidumbre regulatoria**

🔵 **Corea, dato nuevo y vendible por su signo:** la participación coreana en el
mercado global de juegos **cayó a 7,2%, su nivel más bajo desde 2020**. Puesto
junto a lo que esta KB ya tiene —**AI Basic Act con alcance extraterritorial
desde el 22-ene-2026** (cycle 4) y la **excepción de TDM rechazada**— el perfil
coreano es **el más difícil de la región: obligación nueva y presupuesto que se
encoge**. ⇒ **No es el mercado de entrada de APAC.** Es, en cambio, donde la
oferta que vende *cumplimiento a costo acotado* tiene el argumento más fuerte,
porque el cliente **no puede** responder con más gasto.

🔵 **China: el eje correcto no es la adopción, es la certidumbre.** Las fuentes
de este pass coinciden en que la adopción **ya no se discute** —*"AI adoption is
not much of a debate in China; it's already in use across much of the games
industry"*— lo que **confirma** sin añadir el dato duro de Tencent que el cycle 4
ya tiene (**110 M de usuarios acumulados de NPCs con AI / 17,7 M de DAU pico**,
que sigue siendo la mejor refutación del *"los NPCs con LLM no escalan"*).

Lo **nuevo** es el encuadre: el **etiquetado obligatorio de contenido generado
por AI rige desde septiembre de 2025**, y el marco chino se describe como **el
más completo y restrictivo de APAC** — pero las mismas fuentes señalan que eso
**provee certidumbre regulatoria que muchas empresas valoran**. 🟢 **Consecuencia
comercial, y es contraintuitiva:** China es la región donde **el requisito de
etiquetado es más viejo y más claro**, así que es la única donde el trabajo de
marcado se vende **como práctica establecida y no como preparación para una
fecha**. Para un estudio de APAC que exporta a la UE, **el pipeline de marcado
que China ya obliga es, en lo sustancial, el que el Art. 50(2) pide el
2-dic-2026** ⇒ **un solo trabajo, dos jurisdicciones**. Es el argumento
geográficamente más eficiente de esta página.

🟡 **Japón: conflicto de fuentes, declarado y no resuelto.** Una fuente describe
una **ley marco de 2025** que ordena la política nacional de AI; otra sostiene que
Japón **prioriza el cumplimiento voluntario y los principios centrados en la
persona** por sobre marcos obligatorios. **Las dos pueden ser ciertas a la vez**
(ley marco sin obligaciones duras), pero este pass **no pudo verificarlo en
fuente primaria** ⇒ se registra como **conflicto abierto**, no como hallazgo.
El movimiento de plan de Estado que el cycle 6 documentó sigue siendo la
anotación vigente.

🔵 **Regulación de fondo pendiente en la región**, sin cambio este pass: el
análisis sectorial de AI en gaming asiático sigue pidiendo **actualizar el marco
de copyright para contenido generado por AI** y **definir umbrales de control
creativo humano** en obras asistidas por AI — relevante porque **Asia domina
mobile y online**, donde el volumen de assets generados es mayor.

⚠️ **Barreras que las fuentes nombran y que afectan el tamaño del ticket:**
costos de integración altos, **datos de jugador fragmentados entre plataformas**,
y estudios chicos sin infraestructura ni expertise para desplegar AI a escala
comercial. ⇒ **En APAC, la primera venta es la capa de datos**, no el agente.
Es la misma conclusión que la KB de enterprise sacó para la región.

#### 🆕 2026-11-21 (cycle 7) — Corea entra con cifra propia, y aparece la práctica de gobernanza que se puede vender

- 🟢 **COREA TIENE SU PROPIA CIFRA Y ES ALTA: 77,5% de 120 empresas de juego
  usan gen AI** (**KOCCA** — Korea Creative Content Agency —, informe del
  **Q2-2026**). Y el dato que la acompaña es el comercialmente útil: **"AI
  skills" es ya la prioridad nº1 de contratación** del sector. ⇒ Hasta hoy la
  tesis "en APAC no se vende adopción, se vende gobernanza y escala" **se
  apoyaba en un solo país** (Japón, CESA 85,8%). **Ahora tiene dos mercados
  independientes**, con muestras y organismos distintos.
- 🟢 **JOGA: el 100% de las empresas japonesas de juego online encuestadas usa
  gen AI**, y **Gemini es la herramienta más común**. Es una tercera medición
  japonesa, con una población distinta (juego online) a la de CESA.
  ⚠️ **Muestra chica y no publicada en detalle**: se cita como confirmación de
  dirección, no como porcentaje de precisión.
- 🟢 **🆕 LA PRÁCTICA DE GOBERNANZA QUE SE PUEDE VENDER, Y ES DE APAC:** los
  estudios japoneses reportan que **restringen qué herramientas puede usar el
  personal** y **prohíben que salidas crudas de AI lleguen al producto sin
  pasar por revisión**. ⇒ **Esto no es una política, es un control implementable**
  — y es exactamente el entregable que las Recetas 15 y 16 (gates en el pipeline
  de build) producen. **Primer caso en esta KB donde la práctica declarada del
  cliente coincide con un entregable que ya tenemos escrito**: no hay que
  convencerlo de la necesidad, hay que implementarle lo que ya decidió.
- ⚠️ **Comparabilidad: la base de CESA 2025 eran 54 empresas y la cifra 2026 de
  developers son 1.349 respuestas individuales.** La prensa presenta
  "51% → 85,8%" como crecimiento interanual; **son poblaciones e instrumentos
  distintos**. Citar el 85,8% con su muestra, **nunca el salto**.
- 🔴 **El freno estructural de APAC es regulatorio y es de aprobación de
  producto, no de AI:** el *patchwork* entre los mercados grandes —aprobación de
  títulos, límites de monetización y reglas de menores— es el constraint
  principal, agravado por reglas de privacidad y datos de jugador fragmentados;
  **los estudios chicos son los más afectados**. ⇒ sostiene la regla heredada:
  **país por país, nunca una plataforma regional**.
- ⚫ **Gap medido: China.** Con el instrumento corregido (*"video game"*), las
  consultas **no devolvieron ninguna cifra de adopción de estudios chinos**, aun
  siendo el mercado con el régimen vinculante más desarrollado y el origen de
  los world models punteros (Hunyuan, Skywork). **Es un cero de medición, no de
  mercado**, y es el hueco más caro de APAC en esta KB.

#### 🆕 2026-11-21 (cycle 6) — Japón mueve su plan de Estado, y aparece el primer repo con un estudio AAA de APAC detrás

- 🟡 **Japón: el Gabinete adoptó un AI Basic Plan nuevo el 14-jul-2026**, que
  **reemplaza** el plan de diciembre de 2025. Japón mantiene el enfoque por
  principios, sin penalidades, apoyado en leyes existentes. ⇒ para un título que
  se publica en Japón el riesgo **no es la multa, es el cambio de política**: el
  plan de Estado se reescribió en 7 meses. **Cláusula de revisión semestral en
  la propuesta, no un anexo de compliance cerrado.**
- 🟡 **Corea, el matiz que faltaba:** el AI Basic Act vigente desde el
  **22-ene-2026** tiene **período de gracia de al menos un año antes de multas**,
  y el techo de multa es **KRW 30 millones** cubriendo **sólo tres tipos de
  incumplimiento**. ⇒ **la exposición económica coreana es baja; la exposición
  reputacional y de publicación, no.** Para dimensionar: el techo coreano es
  ~US$ 20 mil contra los **€35 M o 7% de facturación global** del Art. 5 europeo.
  **Seis órdenes de magnitud de diferencia entre dos jurisdicciones que un mismo
  título publica el mismo día** — es el argumento más fuerte de esta KB a favor
  de una **capa de disclosure configurable por jurisdicción** y no de tres
  implementaciones (Receta 15).
- 🟡 **Etiquetado de contenido generado: China, Corea, India y Vietnam ya lo
  exigen.** Vietnam: Ley de AI **vigente desde el 1-mar-2026**. **China: se
  requiere registro ante la CAC para servicios de AI generativa, y a mediados de
  2026 había más de 550 servicios registrados.** ⇒ **publicar un NPC con LLM en
  China no es un problema de etiquetado: es un trámite de registro previo**, y
  550 registros dicen que el trámite es transitable, no prohibitivo. Es la
  diferencia entre "no se puede" y "lleva tiempo y papeles": **hay que
  presupuestarlo como *lead time* regulatorio, no como bloqueo.**
- 🟢 **Señal de industria nueva, y viene con repo:**
  [THUSI-Lab/GameScaling](https://github.com/THUSI-Lab/GameScaling) (Apache-2.0,
  604★, **pesos Apache-2.0 verificados vía Qwen3.5**) acredita trabajo realizado
  *"while interning at **miHoYo**"*. Es la primera vez que esta KB encuentra un
  **estudio AAA de APAC** (Genshin Impact, Honkai) detrás de un repo de agentes
  de juego **permisivo y publicado**. Junto a `op7418/guizang-game-skill`
  (origen APAC, aunque AGPL-3.0) y `Calix-L/DanKS` (Guandan por RL), **APAC pasó
  de ser la región con la mejor cifra de adopción a ser también la que
  contribuye código**. Eso cambia la propuesta: en APAC AI Studios compite con
  capacidad local real, no llega a llenar un vacío.
- ⚫ **Gap medido, y es el mismo que el pass anterior, ahora con una segunda
  confirmación independiente:** no existe regulación de AI **específica de
  videojuegos** en ningún país de APAC. Todo lo aplicable es **AI horizontal**
  (China/Corea/Japón/Vietnam) más el *throttle* de aprobación del NPPA chino.
  🆕 Y en el sector adyacente: un informe sectorial registra actividad
  regulatoria sobre AI en *gambling* concentrada en **North America y Europa
  Occidental**, con **CERO registrada en Asia-Pacífico, Europa Central y del
  Este, y Medio Oriente/África**. **Un cero medido en tres regiones** —se anota
  como tal y no como falta de datos.

#### 🆕 2026-11-21 (cycle 5) — APAC tiene ahora **su propia cifra de adopción**, y es 2,2× la "global"

| Dato | Valor | Instrumento |
|------|-------|-------------|
| Desarrolladores japoneses que usan gen AI | **85,8%** | CESA 2026, **1.349 respuestas individuales**, encuesta may–ago 2026, preview en **TGS 2026**, informe completo a principios de **dic-2026** |
| De ellos, uso **diario** | **63%** · ocasional 22,8% | idem |
| Beneficios esperados declarados | eficiencia y productividad, **acortar ciclos de producción**, bajar costos de producción y operación | idem |
| Contraste con la cifra "global" | GDC nov-2026: **40%**, con **3% de muestra japonesa** | GDC SOTGI 12-nov-2026 |

🔴 **Dos cuidados obligatorios antes de usar el 85,8%:**
1. El baseline 2025 que la prensa usa (**51%**) venía de preguntar a **54
   empresas**; el 85,8% es de **1.349 personas**. **No es la misma unidad**: el
   "salto" mezcla empresa con individuo.
2. CESA aclara que su preview **no cubre** AI de pathfinding, matchmaking,
   anti-cheat ni comportamiento de NPC — mide **gen AI en el trabajo**, no AI en
   el juego. Para NPCs, la cifra de producción sigue siendo la de Tencent
   (110 M de usuarios acumulados, 17,7 M DAU pico).

**Consecuencia comercial directa:** en APAC la objeción *"la AI todavía no se
usa"* **no existe**, y tampoco la objeción reputacional con la fuerza que tiene
en NA. La conversación entra en **gobernanza, escala y costo por token**, no en
*awareness*. Es la única región de esta KB donde el pitch puede abrir con
*"ustedes ya están en el 85,8%; hablemos de lo que no está gobernado"*.

🆕 **China — oferta y regulación en el mismo movimiento:** más de **550
servicios de gen AI registrados** a mediados de 2026 bajo el régimen de filings
de algoritmos + etiquetado obligatorio de contenido AI + revisiones de
seguridad. Y la oferta open source sigue naciendo acá: 🆕
[zenstory-ai/novel-to-game](https://github.com/zenstory-ai/novel-to-game) (MIT,
1.175★) es un pipeline **novela → juego** cuyos targets de salida son
**WeChat / Douyin / TapTap mini-games** — es decir, un repo open source cuyo
*distribution surface* es exclusivamente APAC. Para una práctica regional es un
punto de partida, no una curiosidad.

🆕 **Japón — marco promocional, no punitivo:** *AI Promotion Act* sin sanciones,
y **2º AI Basic Plan aprobado en julio-2026** (documento de estrategia, no
estatuto). Combinado con el 85,8%: **alta adopción y baja obligación** ⇒ el
freno no es legal, es de calidad. La preocupación nº1 declarada por los devs
japoneses en una encuesta doméstica distinta (Cloud Ace) es **"inestabilidad en
la precisión y calidad de los resultados generados" (51,9%)**. ⇒ **en Japón se
vende evaluación y QA de salidas, no adopción.**

🔴 **Corea — doble bloqueo que conviene tener junto:** AI Basic Act vigente
desde **ene-2026** con alcance **extraterritorial**, **rechazo** de la excepción
de TDM en copyright (feb-2026) **y** la exclusión territorial de la licencia de
HY-World 2.0 (Corea queda fuera junto con EU y UK). ⇒ Corea es, de las cuatro
regiones, **la de mayor fricción simultánea legal + de licencias**.

---

**Tres regímenes distintos en una región (texto del pass 2026-11-20), y la oferta
open source más fuerte del ciclo.**

- **Corea del Sur — AI Basic Act en vigor desde el 22-ene-2026**, con aplicación
  **extraterritorial** cuando el sistema afecta a usuarios coreanos (un estudio
  LATAM o europeo que publique en Corea queda alcanzado). Es el régimen más
  completo de la región: los proveedores de AI generativa deben **notificar al
  usuario por adelantado y etiquetar la salida**. Techo de sanción modesto
  (KRW 30 millones) y período de gracia de al menos un año antes de multas.
- **China — regula por tipo de servicio**, no por riesgo: reglas vinculantes
  separadas para algoritmos de recomendación, *deep synthesis*, AI generativa y
  etiquetado de contenido. Para un juego con contenido generado, el etiquetado es
  obligación directa.
- **Japón — promoción sin sanciones.** La **AI Promotion Act** rige desde el
  4-jun-2025 **sin penalidades directas**, y se reporta un **AI Basic Plan
  adoptado en julio de 2026**. Hay propuesta de divulgación **voluntaria**
  de datos de entrenamiento que en práctica opera como obligatoria, porque el
  incumplidor queda nombrado públicamente.
- 🆕 🟢 **LA CIFRA DE PRODUCCIÓN QUE FALTABA EN TODA ESTA KB.** Tencent reporta
  para **Game for Peace** (la versión china de PUBG Mobile) **110 millones de
  usuarios acumulados** de las features de NPC con AI y un **pico de 17,7 millones
  de DAU**. Es el despliegue de NPCs con AI más grande del que esta KB tenga
  medición, y por dos órdenes de magnitud. **Consecuencia directa para el pitch:**
  la objeción "los NPCs con LLM no escalan / no son viables en producción" es
  falsa y hay un número con el cual refutarla. Lo que no dice el dato: cuánto de
  ese NPC es un LLM y cuánto es un sistema clásico con capa generativa — Tencent
  no desglosa, y conviene no sobreafirmarlo.
- 🆕 **NetEase — *Justice Mobile* tiene un personaje con AI movido por DeepSeek**,
  que conversa y reacciona fuera del diálogo escrito. 🔴 **Y una corrección de
  rumor:** circuló que NetEase había **prohibido** la AI generativa a sus
  desarrolladores; **lo desmintió** y hay títulos suyos usándola. No repetir el
  rumor en una sala.
- 🆕 **Tencent mostró "LAP"**, herramienta de producción con AI para assets de
  fondo y de personaje y para generación de animación. La AI de pipeline interno
  en APAC no es piloto: es herramienta de casa de un publisher top-3 global.
- 🆕 **El cuello de botella APAC no es técnico, es de aprobación:** la **NPPA**
  china emitió ISBNs para **130 juegos domésticos** el 25-mar-2026, y
  **Tencent y NetEase recibieron una aprobación doméstica cada uno en abril de
  2026 tras dos meses de cero aprobaciones**. ⚠️ Las cifras de los lotes de
  agosto/octubre-2026 (195 vs 215 títulos) **no concilian entre fuentes** y se
  registran como no verificadas. Para un engagement: el riesgo de calendario en
  China es regulatorio-administrativo, no de ingeniería, y **no hay evidencia de
  un trámite de aprobación separado para NPCs con AI** dentro del régimen NPPA —
  se buscó específicamente y no se encontró.
- **Industria, y es una división abierta:**
  - **Krafton** empuja AI companions: el concepto *Co-Playable Character* iba a
    debutar como **"PUBG Ally"** en el modo arcade de PUBG en el **1er semestre de
    2026**.
  - **Capcom negó públicamente** haber metido AI generativa en el contenido de sus
    juegos.
  - El **CEO de Nintendo** dijo que no usarán AI generativa en títulos futuros, por
    riesgo de propiedad intelectual.
- 🟢 **Hallazgo de oferta, no de demanda:** los dos world models jugables abiertos
  más avanzados de este ciclo son de APAC — **Matrix-Game 4.0 (Skywork AI, China,
  15-oct-2026)** y **Hunyuan-GameCraft (Tencent, China)**. Microsoft MineWorld es el
  único contrapeso no-APAC. Quien quiera construir con world models abiertos está,
  de hecho, construyendo sobre I+D china.
- **Esports entró al programa de los Juegos Asiáticos 2026**: demanda de broadcast,
  anti-cheat y analítica competitiva con fecha de calendario.

| Oportunidad APAC | Driver | Fit Globant |
|------------------|--------|-------------|
| **Etiquetado multi-jurisdicción** | Corea obliga notificación previa + etiquetado; China etiquetado por tipo de servicio; Japón divulgación "voluntaria". Un mismo título publica en los tres. | Una capa de disclosure configurable por jurisdicción, no tres implementaciones. |
| **Integración de world models abiertos** | Matrix-Game 4.0 es MIT/Apache-2.0 y corre en una RTX 5080. La barrera ya no es el modelo: es integrarlo a un pipeline de producción. | Ver `compose/patterns.md` → **Receta 8**. |
| **Due diligence de procedencia de pesos** | Construir sobre I+D china implica revisar licencia de pesos y de assets de entrenamiento, y Hunyuan ya es no-OSI. | Screening de grants (el método está en `agents/top.md`). |
| **Analítica de esports** | Juegos Asiáticos 2026. | Anti-cheat conductual + analítica de broadcast. |


#### 🆕 Oportunidad del pass 2026-11-22: **el argumento de ingresos, que en APAC ya existe y en ninguna otra región**

Dato nuevo de este pass: en **H1-2026** los títulos con IA fueron **~15% del
top-100 móvil chino** y se llevaron **42,6% de los ingresos** de ese top-100
(≈**2,8×** de sobre-representación). Es el primer dato del inventario que liga
IA en gaming a **facturación**, no a eficiencia de producción.

- **Cómo se usa:** en NA y EMEA la conversación es *coste* y *riesgo*; en APAC
  se puede abrir por **ingreso**. Es la misma tecnología con un caso de negocio
  distinto, y el caso de ingreso es el único que financia un proyecto grande.
- ⚠️ **Decir el sesgo antes de que lo digan:** correlación, no causalidad. Los
  títulos con IA del top-100 chino son desproporcionadamente los de los grandes
  estudios, que ya facturaban más. **Lo que el dato prueba es que la IA no es un
  lastre comercial en ese mercado** — que ya es más de lo que puede probarse en
  cualquier otra región.
- **Adopción a nivel estudio, con las dos cifras:** ~60% (Niko, 2025) vs **86,4%**
  (Game Publishing Committee, ChinaJoy 2026, *mayoría en etapa temprana*). La
  diferencia es de definición. ⇒ **Vender integración y puesta en producción, no
  evangelización**: el mercado ya probó, le falta operar.
- 🔴 **Corea, advertencia doble:** AI Basic Act desde el **22-ene-2026** (≥1 año
  de gracia antes de multas) **y** territorio excluido de la licencia Tencent.
  La línea de world models de Tencent **no se puede usar en un engagement
  coreano**, igual que en EMEA.

### LATAM

#### 🆕 cycle 20 (2026-11-25) — el canal de cobro: Brasil lo abrió en junio, y **el ledger insignia de la región dejó de ser open source**

**Brasil, acuerdo con el CADE:** aprobado en **dic-2025**, con plazo de **3 años**,
y **en vigor con iOS 26.5 en jun-2026**. Lo que habilita:

- Desarrolladores brasileños pueden operar **marketplaces alternativos** y cobrar
  **fuera de In-App Purchase**.
- Las apps distribuidas fuera de la App Store pasan por **Notarization**
  (chequeos básicos de seguridad e integridad).
- Distribución fuera de la tienda: **Core Technology Commission del 5 %** sobre
  bienes y servicios digitales.
- 🔴 **Las comisiones no coinciden entre fuentes:** una reporta **10 %** reducida
  para la mayoría y **21 %** estándar para bienes digitales; otra reporta **25 %**,
  o **10 %** para los que califican, **más 5 %** por usar el procesamiento de pago
  de Apple. **No se resuelve acá** — se marca como número a confirmar contra la
  documentación de Apple antes de ponerlo en un modelo financiero.

**Brasil / Android, y es una fecha ya pasada que hay que chequear:** desde el
**30-sep-2026**, una app necesita estar registrada por un **desarrollador
verificado** para instalarse en dispositivos Android certificados en **Brasil,
Indonesia, Singapur y Tailandia**. Google afirma que más del 99 % de las apps de
Play quedaron registradas automáticamente — **el riesgo está en lo que se
distribuye fuera de Play**, que es justamente el canal que el acuerdo CADE abrió.
Las dos medidas empujan en direcciones opuestas y **las dos aplican en Brasil al
mismo tiempo**.

#### 🔴 El hallazgo LATAM del pass: `midaz` (Brasil) ya no es open source, y **todos los índices siguen diciendo Apache-2.0**

`LerianStudio/midaz` se publicita como *"el primer ledger open source de América
Latina"*, de una empresa brasileña fundada en 2024. Medido hoy, repo por repo:

| Artefacto | Estado medido 2026-11-25 | Licencia medida |
|---|---|---|
| `LerianStudio/midaz` (core) | 🔴 **no legible públicamente** — `git ls-remote` **pide credenciales (401, no 404)**; `raw` da 404 en `LICENSE`, `README.md` y `go.mod`, tanto en `develop` como en `main` | — |
| `LerianStudio/midaz-console` | 🔴 no legible públicamente | — |
| `LerianStudio/lib-commons` | 🔴 no legible públicamente | — |
| [`LerianStudio/midaz-sdk-golang`](https://github.com/LerianStudio/midaz-sdk-golang) | ✅ vivo, **164 tags** | 🔴 **Elastic License 2.0** (3.824 B) — **source-available, no open source** |

Mientras tanto, los índices de terceros siguen publicando `Apache-2.0`
(`pkg.go.dev`, agregadores de ecosistema con "Apache-2.0 / 72 forks / ~364 ★").
🔴 **La distancia entre el metadato de tercero y el archivo de licencia real es de
una licencia entera, y en este caso incluso de la existencia del repo.**

⚠️ **Lo que NO se afirma:** no se midió **cuándo** ni **por qué** cambió, ni si
fue relicencia, repo privado o borrado. `git ls-remote` devolviendo pedido de
credenciales es consistente con las tres cosas. **Se publica el estado, no la
causa.**

🟡 **Y corrige una regla del cycle 19:** con Unleash se concluyó que *"el servidor
relicencia pero los SDKs se quedan permisivos"*. Acá el servidor **salió de
internet público** y el SDK sobreviviente es **ELv2**. La regla correcta es: *el
SDK suele quedarse permisivo, y hay que medirlo repo por repo.*

🟢 **Oportunidad LATAM:** Brasil es la **única** región donde el canal de cobro se
abrió **y** el mercado tiene escala de juegos móviles propia **y** la pieza de
infraestructura financiera regional acaba de dejar de ser utilizable como base
open source. El reemplazo permisivo existe y está verificado
(`tigerbeetle` Apache-2.0, `formancehq/ledger` MIT) — es un engagement de
migración con disparador fechado, no una propuesta especulativa.

⚠️ **Gap informado de LATAM, dicho explícitamente:** las búsquedas prescritas de
*"AI gaming LATAM adoption regulation players"* devuelven **casi exclusivamente
iGaming (apuestas online)**, no videojuegos — y lo mismo pasó en las otras tres
regiones (ver `intel/trends.md`). **No se encontró ningún estudio de adopción de
AI entre jugadores de videojuegos de LATAM.** Lo que sí se encontró, y se marca
como material de apuestas y no de videojuegos: LATAM se describe como líder global
en adopción de AI para *player safety* porque son mercados abiertos en la última
década sin marco heredado que defender, y Colombia tiene reglas de juego
responsable en plena vigencia.

#### 🔴 Oportunidad del pass cycle 19 (2026-11-22): **Brasil es la única jurisdicción de esta KB donde la ley ya trae un INTERRUPTOR escrito — y apagarlo exime de verificar edad**

**El instrumento:** la **Lei 15.211/2025** (*ECA Digital*), **en vigor desde el
17-mar-2026**, con capítulo propio de juegos electrónicos y obligaciones para
desarrolladores, distribuidoras y tiendas de aplicaciones. La reglamenta el
**Decreto 12.880/2026**, que según el análisis consultado definió una lista de
productos y servicios **prohibidos para menores** que incluye juegos de azar,
apuestas, loterías **y caixas de recompensa (loot boxes) en juegos electrónicos**.

🟢 **Y la cláusula que convierte esto en una decisión de ingeniería y no sólo de
compliance:** el decreto prevé que un juego puede **ofrecer versiones sin loot
boxes o bloquear esa funcionalidad por defecto**, y **en ese caso la verificación
de edad queda dispensada**. Es decir: la ley **pone precio a la personalización de
oferta** y el precio es *implementar verificación de edad*. Apagarla por defecto
sale más barato que verificar, y es un *flag*.

**Enforcement, ya en marcha:** una **Nota Técnica del Ministerio de Justicia**
(proceso **08250.000382/2026-89**), que analiza los primeros seis meses de
vigencia, **pide a la ANPD que monitoree** los juegos con caixas de recompensa que
**no implementaron verificación de edad**.

**Lo que ya existía:** la **Lei 14.852/2024** (*Marco Legal dos Jogos
Eletrônicos*) exigía consentimiento de los padres para microtransacciones y loot
boxes en juegos accesibles a menores. El ECA Digital refuerza eso y, según el
análisis de ConJur, **va más allá al prohibir**.

⚠️ **Conflicto de fuentes declarado y NO resuelto en este pass** (tres lecturas del
mismo texto): la Nota Técnica del MJ afirma que la ley **veda la oferta de la
funcionalidad** de caixas de recompensa; Agência Gov dice que están prohibidas **en
juegos dirigidos a niños**; un análisis académico (UFJF) sostiene que la ley **no
las prohíbe** y sólo exige transparencia absoluta. La diferencia práctica es enorme
y depende de la definición de *"juego dirigido a niños"*, que **no se pudo leer en
la fuente primaria en esta sesión**. Para una propuesta: se diseña para el escenario
más restrictivo (**por defecto apagado**), que además es el más barato por la
dispensa de verificación.

**El lado comercial, que empuja en sentido contrario y por eso importa.** ⚠️ Cifras
de **fuente secundaria** (*Bain & Company, Gaming Report 2026*, encuesta de jun-2026
sobre **+5.300** jugadores, publicado ago-2026): **42 %** de los jugadores está más
cómodo con IA en juegos que un año antes, y **59 %** entre adolescentes; **86 %** de
los adolescentes gasta dinero en juegos cada mes; **84 %** de quienes ya compraron
directo más de una vez dice que **una oferta personalizada** los haría volver a
comprar; y cada venta movida de una tienda de terceros al canal directo mejora el
margen **15-30 puntos porcentuales**. ⚠️ **`bain.com` no resuelve desde esta
sesión**: hay un comunicado específico de LATAM —titulado alrededor de
adolescentes, compra directa y personalización con IA— que **no pudo leerse**. Es un
**gap declarado**, no un dato: las cifras de arriba son **globales**, no regionales.
Complemento, también secundario: *Xsolla/GamesBeat, State of the LATAM Games
Industry 2026* reporta **+335 M** de jugadores en la región y **72,6 M** en México.

🔴 **La síntesis que hace de LATAM el mejor caso de esta capa:** es la región donde
el **argumento comercial** de la personalización es más fuerte (adolescentes +
compra directa + aceptación de IA, coherente con el 48,8 % de aceptación que esta KB
ya tenía desde el cycle 4) **y** donde existe la **restricción legal más dura**
contra ella. Las dos cosas se resuelven con la **misma** arquitectura: la
personalización como *flag* con edad y jurisdicción como entrada, apagado por
defecto y encendido con evidencia. Vender eso no es vender cumplimiento: es vender
**la única forma de capturar el margen sin quedar del lado equivocado del Decreto
12.880**.


> 🆕 **cycle 18 (2026-11-22).** Confirmaciones y una advertencia de higiene.
> **México:** **>76 M** jugadores (Endeavor) junto al 72,6 M de Xsolla ⇒ dos
> instrumentos independientes, misma magnitud: la cifra es sólida. **Regulación de
> IA en videojuegos en Brasil, Argentina y México: cero instrumentos encontrados**,
> cuarto pass consecutivo — y eso es un dato vendible, no un vacío: **LATAM es hoy
> la región donde una decisión de producto sobre AI no tiene un reloj regulatorio
> encima**, al revés que EMEA (10 días) y NA (1-ene-2027). ⚠️ **Higiene de fuente:**
> el **97%** de devs LATAM usando IA sale del *Google Cloud Games Report* **vía
> Globant** — es nuestra propia casa citándose, y así debe publicarse. 🔴 **Repos de
> gaming AI de origen LATAM: cero**, cuarto pass, buscado en español. El canal 6 no
> mueve esta aguja: enumera árboles de repos que ya existen, no los crea.

#### 🆕 Oportunidad del pass cycle 17 (2026-11-22): **el decreto brasileño nombra QA y co-desarrollo, y la capa que nombra no se produce en la región**

**El gancho, en una línea:** *"El reglamento del Marco Legal abre justamente
co-desarrollo y QA, y en esa capa no hay proveedor local de producto: hay 22 repos
medidos y ninguno es de la región. Lo que se arma acá es capacidad, no licencia."*

- **Instrumento**: Decreto nº 13.138/2026 (14-oct-2026), que reglamenta la Lei nº
  14.852/2024. ⚠️ *Dato de fuente secundaria — confirmar contra el DOU.*
- **Oferta concreta y barata de arrancar**: Receta 43 variante A (100% MIT/Apache)
  sobre un estudio de 10-30 personas; reemplaza una persona de release y deja
  trazabilidad de build, que es lo que después pide cualquier certificación.
- **Cero medido**: 0 de 22 repos de la capa con dueño LATAM. No es silencio, es
  medición, y es el argumento de ecosistema para una conversación con Abragames o un
  programa público.

#### 🆕 Oportunidad del pass cycle 16 (2026-11-22): **la aceptación de IA convive con una objeción LABORAL explícita** — y el problema nº1 que declaran los estudios de la región es el que Globant vende

**El gancho, en una línea:** *"En Latinoamérica el jugador acepta la IA más que en
ningún otro mercado, y aun así cuatro de cada diez están preocupados por el empleo
de los desarrolladores. No es contradicción: es el margen exacto en el que se
comunica un lanzamiento."*

**PGB Latam 2026** — 7.202 encuestados en Argentina, Chile, Colombia, México y
Perú:

| Señal | Valor |
|---|---|
| Consideraría comprar juegos hechos con IA | **~50%** |
| Preocupados por el efecto en el empleo de desarrolladores | **40,6%** |
| Lo pensaría dos veces si el juego fue hecho **mayormente** con IA | **25,7%** |
| Generación Z | ya es **el grupo mayoritario** del público gamer |

> **Esto MATIZA, no contradice, el 48,8% de aceptación que esta KB publica.** La
> aceptación regional se sostiene. Lo que el pass agrega es que **la objeción no es
> estética sino laboral**, y que una cuarta parte del mercado castiga
> específicamente el *"hecho mayormente con IA"*. La conclusión operativa es de
> comunicación, no de producto: **la IA como herramienta del equipo se tolera; la
> IA como sustituto del equipo se penaliza**, y la diferencia está en cómo se
> cuenta.

**Mercado:** la región genera ~**US$ 8.300 M al año**; **México es el mayor mercado
de Latinoamérica**. Bain (2026) identifica como motores de crecimiento a los
**adolescentes** y a la **compra directa al desarrollador**, y señala que la IA
acelera el prototipado y abarata la iteración. (Los 372 M de jugadores que esta
página ya registra siguen en pie.)

**🔴 El dato más accionable del pass para Globant, y lo dicen los propios estudios
de la región:** su **problema número uno es la retención de talento** — los
desarrolladores reciben ofertas de trabajo remoto de empresas internacionales **con
salarios en dólares**. No es una observación de un analista sobre el mercado: es la
queja declarada por el lado comprador.

> **Es la variable del modelo nearshore dicha por el cliente.** Un estudio
> latinoamericano no compra "capacidad de desarrollo barata" —esa es la oferta que
> le está vaciando el equipo—; compra **capacidad que no se va**. La propuesta que
> funciona en la región es la que se mide en **continuidad y transferencia de
> conocimiento**, no en *rate*. Y es, además, el argumento que convierte el plano
> de datos de este pass en una venta: **un pipeline de telemetría documentado y con
> esquemas versionados es conocimiento que sobrevive a la rotación**; un modelo de
> churn entrenado por alguien que se fue, no.

**Regulación:** incipiente. Los propios reguladores de la región **admiten poca
visibilidad** sobre cómo las empresas usan la IA. No hay instrumento regional de IA
aplicable a videojuegos que este pass haya podido medir.

> ⛔ **Frontera del homónimo, otra vez:** la prohibición brasileña de **iGaming**
> (nov-2026) y el endurecimiento regulatorio de Colombia y Perú son del sector
> **juego de azar**. Aparecen en todas las búsquedas regionales de "gaming" y **no
> pertenecen a esta página**. Se nombran aquí únicamente para que nadie los importe
> por error a una propuesta de videojuegos.


#### 🆕 Oportunidad del pass cycle 15 (2026-11-22): **el 48,8% queda CONFIRMADO con fuente nombrada — y la accesibilidad es un cero medido también acá**

**El dato más valioso de esta KB en LATAM queda corroborado por segunda vez y
ahora con su fuente identificada.** El cycle 4 registró *"48,8% de aceptación del
jugador latinoamericano"* sin instrumento nombrado. Este pass lo encuentra:

| Dato | Valor | Fuente |
|---|---|---|
| Jugadores que **considerarían comprar** un videojuego desarrollado con IA | **48,8%** | **Estudio PGB Latam 2026**, presentado en **gamescom latam** |
| Temen **despidos** en la industria | **40,6%** | ídem |
| Temen que los juegos **pierdan calidad** | **35,8%** | ídem |

⇒ **El argumento estructural de LATAM se sostiene y se afina.** Frente al ~85%
de rechazo global que esta KB registra (Quantic Foundry), LATAM sigue siendo
**el mercado racional de primer lanzamiento para IA de cara al jugador**. Y
ahora se sabe que **la objeción latinoamericana no es la calidad del producto
sino el empleo** (40,6% vs 35,8%) ⇒ el contraargumento no es una demo: es
**dónde queda el equipo**. Un lanzamiento LATAM con IA visible y sin recorte de
plantilla anunciado es vendible; uno acompañado de layoffs reproduce acá el
rechazo del que se estaba escapando.

**Tamaño del mercado, actualizado:**

| Dato | Valor | Fuente |
|---|---|---|
| México, jugadores activos | **> 76 millones** | Endeavor, vía prensa mexicana |
| México, mercado anual | **≈ USD 2.300 millones** | ídem |
| Posición | **#1 de la región** | ídem |

> 🔴 **GAP MEDIDO — accesibilidad:** se corrió el instrumento **en español**
> (`AI videojuegos LATAM 2026 adopción regulación accesibilidad industria`), que
> es la regla de la flota para no confundir ausencia de oferta con ausencia de
> búsqueda. **Ninguna fuente de la región aborda accesibilidad de videojuegos.**
> Lo que devolvió fue adopción, percepción del jugador y tamaño de mercado. ⇒
> **el gancho de accesibilidad no se usa en LATAM**, igual que en APAC, y por la
> misma razón: no hay instrumento sectorial al que anclarlo.

**Lo que LATAM sí origina y hay que vender en su lugar — la capa de aptitud
lingüística.** Es la conclusión que la flota ya alcanzó en otras industrias y que
este pass confirma en gaming con un repo concreto:

| Repo | ★ | Licencia | Para qué |
|---|---|---|---|
| [TohmaN233/YN-translation-workshop](https://github.com/TohmaN233/YN-translation-workshop) | 235 | **MIT** | Banco de trabajo de traducción y revisión con IA para ficción larga y **localización de juegos**. Vivo 2026-11-11 |

La **localización** es la tarea donde el propio informe de adopción citado por la
industria pone a la IA junto a escenarios y *playtesting*, y es la única de las
tres donde la especificidad regional —es-419 vs es-ES, pt-BR, voseo, variantes
de doblaje— **es una ventaja estructural y no un descuento por nearshore**.

> ⚠️ **No citar "97% de los desarrolladores usa IA"** en material LATAM: la cifra
> circula atribuida al *Google Cloud Games Report 2025* **a través de un
> competidor directo**, y otra fuente da ~90% para el mismo origen. Ver la
> corrección de registro al inicio de este archivo.

> ⚫ **Regulación LATAM de IA en gaming:** las fuentes de la región sólo registran
> **mayor supervisión de compras dentro del juego y de funciones de IA**, y
> *loot boxes*, en términos genéricos y sin instrumento citable. **No hay norma
> de IA específica de videojuegos medida en LATAM este pass.** Los instrumentos
> generales (Perú SD 115-2025-PCM en vigor desde el 22-ene-2026, firma uruguaya
> del Convenio del Consejo de Europa) siguen siendo el marco, y no son sectoriales.

#### 🆕 Oportunidad del pass cycle 14 (2026-11-22): **la capa de assets no tiene pieza local — y eso es exactamente el espacio**

**El gancho, en una línea:** *"En producción de assets con AI, el ecosistema
abierto en español es un repo de 1 ★ bajo GPL-3.0. No hay nada que adoptar: hay
que construirlo."*

| Qué | Detalle |
|---|---|
| **Disparador (medido, no supuesto)** | Tres consultas en español sobre generación de assets 3D, música de videojuego y rigging automático devolvieron **`total_count: 0`** cada una. Una cuarta devolvió **un solo repo**: [facundoAlmon/AssetCrisol](https://github.com/facundoAlmon/AssetCrisol) — **1 ★**, **GPL-3.0** (35.149 B), vivo (push 2026-11-13), con salida a **Godot 4** y Blender 5.1 |
| ⚠️ **Límite del instrumento** | "Repo en español" **no es** "repo de origen LATAM": los canales de esta sesión no permiten medir el país del dueño. Se afirma lo medido, no más |
| **Entregable 1** | **Pipeline de assets 100% permisivo y auditado**, armado y documentado para el cliente: `image-to-3dlab` + `GameFactory-3A` + `MOSS-TTS` + `ACE-Step`, con la clausura de dependencias resuelta. Ninguna de las cuatro tiene restricción territorial ⇒ **LATAM es la única región de este cuadro sin bloqueo de ningún tipo** |
| **Entregable 2** | **Salida a Godot como criterio de selección.** Tanto `AssetCrisol` como `GameFactory-3A` exportan a Godot 4, que es la base que esta KB recomienda desde el primer pass. Es la fila donde el stack LATAM y el stack recomendado coinciden sin esfuerzo |
| **Entregable 3** | Si el cliente quiere contribuir: `AssetCrisol` es **GPL-3.0**, así que contribuir ahí **no** es compatible con un producto propietario. El camino limpio es contribuir upstream a los Apache-2.0 |
| ⚠️ **Lo que NO se vende** | Nada del carril apuestas. La consulta regional de este pass devolvió **exclusivamente** iGaming (Brasil Ley 14.790/2023, SPA sancionando desde 1-ene-2026, estándares PAD contra deepfakes; Perú con dirección supervisora propia). En LATAM el buscador **no distingue gaming de gambling en absoluto**, y por eso acá la frontera hay que enunciarla en la primera reunión |

#### 🆕 Oportunidad del pass cycle 12 (2026-11-22): **"AI que no se note" — las cuatro cifras del PGB juntas cambian la oferta**

**El gancho, corregido respecto de lo que esta KB venía diciendo:** *"El 48,8%
de los jugadores de la región compraría un juego hecho con AI. El 35,8% teme
que salga un producto sin alma. No son dos públicos: es el mismo, y te está
diciendo el precio de entrada."*

| Qué | Detalle |
|---|---|
| **Base** | PGB Latam 2026, **n = 7.202**, Argentina/Chile/Colombia/México/Perú, presentado en gamescom latam 2026 (São Paulo). ⚠️ **Brasil NO está en el panel** — corrección del cycle 9, que se mantiene |
| **Las cuatro cifras, dichas enteras** | **48,8%** compraría · **25,7%** lo pensaría antes · **40,6%** teme despidos y precarización · 🆕 **35,8%** teme caída de calidad y productos *"sin alma"* |
| 🔴 **El reencuadre** | Esta KB vendía LATAM como *"mercado racional de primer lanzamiento para AI de cara al jugador"*. Con las cuatro cifras: **la aceptación es alta y CONDICIONADA**. La oferta correcta no es *"LATAM acepta AI"*, es **"LATAM acepta AI si no se nota en la calidad"** |
| **Entregable que sale de ahí** | **Gate de calidad percibida** en el pipeline generativo: revisión humana obligatoria sobre todo contenido de cara al jugador, con métrica y evidencia. Se vende como **gestión de riesgo de marca**, no como cumplimiento — en LATAM no hay obligación que citar |
| **Argumento de volumen regulatorio** | **193 iniciativas legislativas en 13 jurisdicciones** (*Desafíos regulatorios de la IA en Latam*, 2ª ed., nov-2025). ⚠️ **Nada específico de videojuegos**: se usa como volumen, no como norma sectorial |
| **Dos ceros medidos** | ⛔ **Sin cifra de adopción por estudios latinoamericanos** (pista para el próximo pass: Abragames, gamescom latam). ⛔ **Un solo repo de gaming-AI de origen LATAM, con 0 ★** (barrido en español ordenado por estrellas). **LATAM consume esta tecnología; no la publica** |


#### 🆕 Cycle 11 (2026-11-22) — LATAM: **no existe una cifra de adopción de AI en videojuegos** para la región en las fuentes de 2026. Es un hueco medido, no una omisión

Este pass buscó específicamente una medición de adopción de AI entre estudios de
videojuegos latinoamericanos —en inglés y acotando a Brasil y México— y **no la
encontró**. Es la **tercera confirmación** del mismo cero, con un instrumento
distinto de los dos anteriores. Lo que hay es cualitativo. Se escribe porque un vacío no declarado se
lee como cobertura:

| Lo que SÍ hay | Dato | Calidad |
|---|---|---|
| **Brasil, volumen de industria** | 🔵 **más de 1.000 estudios** operando en el país; los estudios *"ya exploran AI para acelerar prototipado y experimentar con flujos de localización"* (pieza ligada a **ABRAGAMES**, may-2026) | Cualitativo. **Sin porcentaje** |
| **LATAM, informe regional** | 🔵 *State of the LATAM Games Industry 2026* (GamesBeat): el talento de ingeniería regional *"utiliza cada vez más AI para mejorar la eficiencia de producción"* | Cualitativo. **Sin porcentaje** |
| **México** | 🔵 La única cifra localizable es de **2023** (crecimiento del desarrollo indie), y **no habla de AI** | No utilizable para 2026 |
| **Señal de ecosistema** | 🔵 **Xbox LATAM Game Camp / ID@Xbox**, oct-2026: programa activo de desarrollo de creadores latinoamericanos | Contexto, no medición |

🔴 **Lo que esto significa para una propuesta.** La KB tiene para LATAM una cifra
**de jugadores** (48,8% de aceptación en AR/CL/CO/MX/PE, n=7.202; Brasil aparte
con 39,3%) y **ninguna de estudios**. ⇒ **el argumento LATAM de esta KB sigue
siendo de demanda, no de oferta**, y hay que decirlo así: *"el mercado acepta AI
de cara al jugador mucho más que el promedio global; cuánto la usan los estudios
locales no está medido públicamente"*. Inventar un número ahí es el error que
esta KB pasó dos ciclos corrigiendo.

| Otro hallazgo | Dato | Lectura |
|---|---|---|
| ⚠️ **Un repo permisivo de la categoría backend, con origen SIN confirmar** | [topfreegames/pitaya](https://github.com/topfreegames/pitaya) (MIT, 2.847 ★, push 2026-11-17, SDKs Unity/iOS/Android) suele asociarse a un estudio brasileño. **Esta sesión no pudo confirmarlo**: `gh api users/<org>` está bloqueado y la búsqueda web no lo corrobora (una fuente advierte que las dos empresas que se asocian son distintas) | 🔴 **No se publica como "repo de origen LATAM".** Queda como **candidato a verificar** en el próximo pass con un canal que lea la ficha de la organización. La casilla *repos de agentes/infra de origen LATAM* sigue **en cero medido** |
| 🎰 **Carril casino: Brasil enfría la inversión regional** | 🔵 **Nov-2026**: la prohibición brasileña del iGaming regulado empuja jugadores a operadores sin licencia y lleva a los proveedores a compromisos **más chicos y más cortos en toda LATAM**; México, Perú y Argentina siguen en foco pero con cautela. Colombia ya aplica reglas actualizadas de juego responsable | **No aplica a videojuegos.** Importa porque un cliente de iGaming en LATAM va a estar recortando presupuesto justo ahora |
| 🎰 **Y el contrapunto** | 🔵 Un análisis sectorial posiciona a LATAM como **líder global en adopción de AI para seguridad del jugador**, con el argumento de que los mercados abrieron hace poco y la supervisión por AI quedó **en el reglamento inicial** en vez de ser un retrofit | Mismo carril casino. Es el único eje donde LATAM aparece **estructuralmente adelante**, no más barato |


#### 🔴 cycle 9 (2026-11-21) — **CORRECCIÓN: el 48,8% que esta KB usa para vender LATAM no incluye a Brasil**

Es la corrección más importante de este pass y toca el argumento comercial más fuerte
del archivo. Desde el cycle 4 esta página sostiene que LATAM es **el mercado racional
de primer lanzamiento** para IA jugador-facing, apoyada en un 48,8% de aceptación
frente a ~85% de rechazo global. **La cifra es real; su alcance estaba mal.**

| | PGB **Latam** 2026 | PGB **Brasil** 2026 |
|---|---|---|
| Muestra | **7.202 respuestas** | **>7.000 personas** |
| Países | **Argentina, Chile, Colombia, México, Perú** | **Brasil** |
| Campo | presentado en **gamescom latam 2026** (São Paulo) | **5–13 de marzo de 2026** |
| Autor | Pesquisa Game Brasil / Go Gamers | idem |
| **Compraría un juego hecho con IA** | **48,8 %** | **39,3 %** |
| Teme despidos / precarización por IA | **40,6 %** | — |

**Brasil —el mercado más grande y más maduro de la región, con penetración >70% de la
población— está fuera del panel del 48,8%.** Medido aparte, da **39,3%**: unos **9,5
puntos menos**.

**Cómo corregir el pitch:**

- ❌ **No decir más:** *"LATAM acepta la IA en juegos (48,8%)"*.
- ✅ **Decir:** *"**Hispanoamérica** (AR/CL/CO/MX/PE, n=7.202): **48,8%** de intención
  de compra. **Brasil, medido aparte (n>7.000): 39,3%**."*
- ✅ **En un engagement brasileño la cifra correcta es 39,3%.** Citar 48,8% en São Paulo
  es citar un panel que excluye a ese cliente — y es verificable en un minuto.
- ⚠️ **La aceptación convive con ansiedad laboral en la misma muestra: 40,6% teme
  despidos.** No son dos públicos distintos; son las mismas personas. Un lanzamiento
  LATAM con IA necesita mensaje de empleo, no sólo de producto.

**La ventaja regional sigue existiendo, y sigue siendo estructural:** incluso 39,3% en
Brasil está muy por encima del rechazo ~85% que esta KB registra a nivel global. Lo que
se cae no es la tesis, es **la homogeneidad de la tesis**. LATAM deja de ser un bloque
y pasa a ser **dos mercados con números propios**.

> Esta corrección la produjo la regla de método del cycle 5 —*"un número sin su muestra
> no es un número"*— aplicada **a la cifra favorita de esta propia KB**. El dato llevaba
> cinco passes citado sin su panel.

**Contexto regional adicional (medido este pass):**

- **Brasil y México lideran la adopción general de IA en LATAM (76% y ~70%)**, pero
  ⚠️ esa cifra mide **usuarios digitales en general, no developers ni estudios**. No
  usarla como proxy de adopción en gaming.
- **Consolas 19,5% / PC 17,5%** de preferencia regional (PGB Latam, vs 16,4% en 2025).
- **México: 51,8% de sus jugadores son Generación Z**, segundo detrás de Perú.
- Decreto brasileño firmado el **14-oct-2026** mencionado en fuentes de comercio
  exterior ⚠️ **sin que se pudiera confirmar su contenido** en esta sesión.

⛔ **Gaps LATAM declarados (medidos, no silenciados):**

1. **No existe cifra de adopción de IA por *estudios* de Brasil, México o Argentina.**
   Se buscó explícitamente en español; lo que vuelve es consumo y percepción de
   jugadores. El dato de 94% (Vivrato) es de estudios **con sede en NA y Europa** y
   **no cubre LATAM**.
2. **Cero repos de gaming AI con origen LATAM** entre los 11 nuevos de este pass. Es un
   cero **medido** sobre el universo buscado, no una ausencia por no haber mirado.
3. La UNDP advierte que LATAM y el Caribe **van rezagados en adopción de IA** en general.

→ **Lo que estos tres gaps significan comercialmente:** en LATAM la demanda del
*jugador* está medida y es favorable, pero la capacidad del *estudio* no está medida
por nadie. Globant, que opera desde la región, está en posición de ser **quien produce
ese dato** — un informe de adopción de IA en estudios latinoamericanos no existe hoy y
sería el único activo de su tipo en la conversación.

#### 🆕 2026-11-21 (cycle 8) — la brecha de adopción de LATAM **por fin tiene su cifra**, y es una razón para vender, no una excusa

🟢 **El dato más fuerte que esta página consiguió para LATAM, y es una brecha
cuantificada.** Según el **PNUD/UNDP** (vía SiGMA), la AI podría agregar **5,4%
al PIB regional de LATAM hacia 2030**, contra **14,5% en América del Norte** —
atribuido a **baja inversión pública en ciencia y tecnología** y a **una economía
informal grande**. ⇒ **Una brecha de 9,1 puntos.**

**Cómo se usa, y cómo NO.** No es un argumento de *"LATAM está atrasada"*: es
**el tamaño del excedente sin capturar**, y la razón por la que en LATAM el
comprador correcto es el que **todavía no tiene pipeline**, mientras en NA es el
que **ya tiene uno y necesita gobernarlo**. La misma oferta se vende con dos
relatos distintos según el lado de esa brecha.

🔵 **Estructura de mercado, con fuente y con conflicto declarado.** El informe
**Xsolla + GamesBeat 2026** (previo a gamescom latam) da:
**+335 M de jugadores** en la región; **Brasil** como el mercado más grande y
maduro, con **penetración por encima del 70% de la población**; **México** como
principal puerta comercial de entrada, con **72,6 M de jugadores**.

⚠️ **Conflicto con el propio histórico de esta KB, y se deja a la vista:** el
**cycle 1 (jul-2026) publicó "372 M de gamers"** en LATAM; este informe dice
**+335 M**. **No se reescribe ninguna de las dos cifras** — son instrumentos
distintos con metodologías que no publicamos, y la regla de la casa desde el
cycle 5 es que **un número sin su muestra no es un número**. ⇒ En una propuesta
se cita **335 M con fuente Xsolla/GamesBeat 2026** (la más reciente y la más
atribuible) y **se evita el rango** salvo que el cliente pregunte, caso en el que
la honestidad sobre la dispersión **es** el argumento de credibilidad.

🟡 **Regulación: sin obligación de AI en videojuegos, y eso es el hallazgo.**
Este pass buscó específicamente regulación de AI aplicada a videojuegos en
Brasil y México y **no encontró ninguna**. Lo que hay es **regulación de
apuestas**: Brasil terminó 80 años de prohibición federal y abrió mercado
licenciado el **1-ene-2025**; a fines de 2025 la **SPA del Ministerio de
Hacienda** abrió consulta pública por su **agenda regulatoria 2026-2027**; en
México la reforma federal **sigue pendiente**. Una fuente de agencia comercial
menciona un **decreto brasileño de video y entretenimiento del 14-oct-2026** cuyo
contenido **no se pudo confirmar** (snippet truncado) ⇒ **se registra como pista,
no como dato**.

⇒ 🟢 **Consecuencia, y es la ventaja estructural de la región:** en LATAM **no
hay deadline regulatorio que forzar**, así que la venta **no puede ser
cumplimiento** — tiene que ser **producto**. Eso **coincide** con la única
ventaja estructural (no de precio) que esta KB le encontró a LATAM y que sigue
vigente desde el cycle 4: **48,8% de aceptación del jugador frente a ~85% de
rechazo global** ⇒ **LATAM sigue siendo el primer mercado racional para lanzar AI
de cara al jugador**. Y el contrapeso también sigue vigente: **Ubisoft, 166
despidos + Motherbrain, 19-nov-2026**.

🔵 **Medición de origen, repetida con el instrumento corto en español:** este
pass volvió a buscar repos de agentes de gaming **con origen LATAM** y el
resultado vuelve a ser **cero medido, no cero supuesto** — consistente con los
cycles 4-7. **Es un cero informado y se escribe como tal**: la región consume y
adapta, no publica base instalada en este nicho.

#### 🆕 2026-11-21 (cycle 7) — el cero medido en adopción, y la estructura de mercado que sí se puede citar

- ⚫ **CERO MEDIDO, con los dos instrumentos declarados.** Buscado como pide la
  rotación (`AI gaming LATAM 2026 adoption regulation players`) **y** con el
  instrumento corregido (`Latin America video game developers AI 2026 Brazil
  Mexico Argentina studios adoption`): **no existe ninguna encuesta de adopción
  de gen AI por estudios latinoamericanos**, ni por país ni por tamaño.
  🔵 **Es la segunda vez que esta KB mide este cero** y hay que escribirlo así:
  **un cero medido es información; un silencio parece cobertura.** El informe
  **Xsolla/GamesBeat *State of the LATAM Games Industry 2026*** existe y está
  organizado por Brasil / México / resto, pero **está *gated***: si tiene sección
  de AI, no es legible desde acá.
- 🟢 **Lo que SÍ se puede citar es estructura de mercado, y sirve para dimensionar
  un engagement** (fuentes secundarias de mercado — **atribuir, no adoptar**):
  **Brasil ~13.225 developers y ~1.042 estudios activos** (el mayor pool de la
  región y el líder en revenue); **México 72,6 M de jugadores = 61% de la
  población de 6+** (la puerta comercial); **Argentina y México con pools de
  developers de varios miles**. ⇒ **Brasil es dónde está el talento, México es
  dónde está el jugador.** Son dos conversaciones distintas y esta KB las
  mezclaba.
- 🔴 **El constraint de LATAM está nombrado por la propia industria y NO es
  tecnológico: es financiamiento.** Entrevistas de prensa especializada sitúan la
  falta de **funding** como lo que falta en Brasil. ⇒ **consecuencia directa para
  la oferta: en LATAM el entregable tiene que pagarse solo dentro del trimestre.**
  Un programa de transformación a 12 meses no tiene de dónde financiarse; una
  Receta 1 o 3 acotada, sí. **Esto refuerza, con causa económica, la elección de
  recetas que esta KB ya hacía para la región.**
- 🟡 **Contrapeso macro:** el **PNUD advierte que América Latina y el Caribe van
  rezagados en adopción de AI** (vía prensa sectorial). Es el marco en el que hay
  que leer el 48,8% / 51,8% de **jugadores** dispuestos a comprar juegos hechos
  con AI: **la apertura del jugador latinoamericano sigue siendo la ventaja
  estructural de la región** (y la única ventaja *estructural*, no de precio, de
  toda esta KB) **pero conviene no confundirla con capacidad de producción
  instalada**, que es la que el funding limita.
- 🔵 **Y el carril regulatorio sigue como estaba:** el único compliance con fecha
  en LATAM es **publicitario** (guía CONAR de Brasil, en vigor desde el
  1-jun-2026); **no hay ley general de AI en vigor** (PL 2338/2023 trabado).
  Nada de este pass lo mueve.

#### 🆕 2026-11-21 (cycle 6) — la cifra LATAM se parte por generación, y es el dato más vendible de la región

- 🟢 **México, Generación Z: 51,8% compraría un juego desarrollado con AI**,
  contra el **48,8% regional** (PGB Latam 2026, presentado en **gamescom latam**).
  Es la **primera desagregación por cohorte** que tiene esta KB en cualquier
  región. ⇒ el argumento de "mercado de primer lanzamiento" deja de ser
  geográfico y se vuelve **geográfico + demográfico**: el segmento más receptivo
  del mundo a AI de cara al jugador, con dato publicado, es **Gen Z mexicana**.
  Para un rollout eso es una **cohorte de prueba A/B definible**, no un país
  entero.
- 🟡 **Contrapeso, en la misma encuesta y por eso creíble: 40,6% se alarma por la
  precariedad laboral y los despidos.** ⇒ 51,8% compra y 40,6% se alarma **no se
  contradicen**: describen un mercado donde la AI en el **producto** se tolera y
  la AI como **narrativa de reemplazo** no. Es la misma lectura que ya tenía la
  KB, ahora con la cifra de aceptación más alta y la de alarma al lado.
- 🟡 **Contexto de adopción general (no de gaming), para la sala:** Brasil y
  México lideran la adopción de AI en la región con **76% y 70%**
  respectivamente. ⚠️ Es adopción **empresarial general**, no de estudios de
  juego: se usa para argumentar *madurez del mercado comprador*, nunca como
  cifra de la industria. **Un número sin su sector tampoco es un número.**
- 🟡 **Regulación, estado consolidado y conflicto declarado:** **Brasil** —
  PL 2338/2023 aprobado por el Senado y **trabado en la Cámara**; una fuente
  sectorial lo describe ya *"en fase de implementación con la ANPD como
  autoridad"*, lo cual **contradice** el estado legislativo y se anota como
  conflicto, no como hecho. **México** — a julio de 2026 **no hay ley general de
  AI**; una consultora sostiene que la **LFPDPPP** ya cubre el caso, fuente menos
  confiable y sin cita oficial. **Deloitte (jul-2026):** Colombia, Brasil, Chile
  y México en **distintas etapas de trámite**, ninguna con ley en vigor. ⇒ **la
  oferta de compliance en LATAM sigue siendo LGPD + derecho de autor + CONAR, no
  AI Act.** Sin cambios respecto del pass anterior, ahora con la fuente
  conflictiva identificada por nombre.
- ⚫ **Gap medido, tercer pass consecutivo y con instrumento en español:** no se
  encontró **ninguna** regulación de AI **específica de videojuegos** en LATAM
  (loot boxes con AI, copyright de assets generados), ni **ningún estudio LATAM
  concreto** con adopción de AI documentada públicamente, ni **ningún repo de
  agentes de gaming de origen LATAM** entre los 16 candidatos nuevos de este
  pass. Buscado con instrumento en español —`AI videojuegos LATAM Brasil México
  2026 adopción regulación estudios`— precisamente para no medir el sesgo del
  inglés. **El cero es del mercado, no del instrumento.** Lo que sí existe es
  demanda medida (PGB Latam) y estudios locales con nombre propio
  (p. ej. GameCoder Studios, México); lo que no existe es **producción open
  source de agentes**. ⇒ **LATAM es, en esta KB, consumidor y mercado de prueba,
  no productor** — y ésa es la oportunidad: **el hueco de producción es
  exactamente el espacio de un AI Studio local.**

#### 🆕 2026-11-21 (cycle 5) — la ventaja de LATAM se sostiene, y ahora tiene su contrapeso medido

| Dato | Valor | Fuente / instrumento |
|------|-------|---------------------|
| Gamers de LATAM que **comprarían** un juego hecho con AI | **48,8%** | **PGB Latam 2026**, presentado en **gamescom latam** |
| 🆕 Gamers de LATAM a quienes **alarma la precariedad laboral y los despidos** de la industria | **40,6%** | idem |
| Contraste global | **~85% de rechazo** entre jugadores (Quantic Foundry, ~2 M de jugadores) | — |

🟢 **La tesis sobrevive, y es la única ventaja *estructural* (no de precio) que
esta KB puede atribuir a LATAM:** con 48,8% de aceptación contra ~85% de rechazo
global, **LATAM es el mercado racional para un primer lanzamiento de AI visible
al jugador**. Eso no cambió hoy.

🔴 **Pero el 40,6% es el contrapeso, y hay que llevarlo en la misma slide.** La
aceptación latinoamericana **no es entusiasmo**: casi la mitad de esos mismos
jugadores declara alarma por el empleo. ⇒ la AI **visible** se puede lanzar en
LATAM; la **narrativa de "AI que reemplaza al equipo" no**. El mensaje que
funciona es *"AI en el pipeline, equipo local ampliado"*, con el caso Ubisoft
(166 despidos + Motherbrain el 19-nov-2026) como ejemplo de **lo que no hay que
anunciar junto**.

🆕 **Regulación: medido, no silencio — y el resultado es que no hay ley de AI
vigente que ate a un estudio de la región.**

| País | Estado al 2026-11-21 | Lo que **sí** ata hoy |
|------|---------------------|----------------------|
| **Brasil** | **PL 2338/2023 aprobado en el Senado el 10-dic-2024 y TRABADO en la Cámara** — sin voto, y sin parecer del relator aprobado en la comisión especial (set/oct-2026). Si la Cámara cambia el texto, **vuelve al Senado**. ⇒ **no hay ley general de AI en vigor en Brasil.** En may-2026 el sector creativo objetó el posible recorte del capítulo de **copyright**, que es justamente el que afecta a assets generados | **LGPD** (datos) y la **guía de publicidad de CONAR**, en vigor desde el **1-jun-2026**, para contenido publicitario hecho con AI ⇒ **el trailer y la campaña están regulados antes que el juego** |
| **México** | Sin ley general. Iniciativa federal de 2024 con autoridad de supervisión, no aprobada. **México lidera el mercado de videojuegos de Latinoamérica** | Parches sectoriales (regulación reactiva) |
| **Argentina** | Sin ley general de AI; adhesión a los **Principios de AI de la OCDE** (también Brasil y México) | Nada específico de AI en juegos |

**Lectura para engagement, y es la inversa de EMEA:** en EMEA la fecha manda y
el gancho es compliance; **en LATAM el gancho es time-to-market** —se puede
lanzar antes, con AI visible, en el único mercado que la acepta— **y el riesgo no
es la multa: es la marca**. Y el único compliance que hay que resolver ya, si hay
campaña, es el **publicitario brasileño (CONAR, 1-jun-2026)**, no el del
producto.

🟡 **Gap declarado de LATAM, medido con el instrumento corto en español y
portugués (4ª vez consecutiva que se mide y 4ª vez que sale lo mismo):** no
existe **ningún dato de adopción de gen AI por estudios latinoamericanos**, ni
por país ni agregado. Las búsquedas en español y portugués devuelven (a) la cifra
global de GDC replicada por prensa regional, (b) el **97% del Google Cloud Games
Report 2025, que es global y circula en medios regionales como si fuera de la
región** —error concreto a no repetir— y (c) iGaming, que es otro negocio.
**Tampoco hay repos de agentes de gaming con origen LATAM** en ninguna de las
búsquedas de este pass. Es un **cero medido**, no un silencio: el instrumento se
corrió y no encontró nada.

---

**El gancho NO es compliance de AI (texto del pass 2026-11-20).**

- **Regulación: no hay ley general de AI donde importa.**
  - **México no tiene ley general de AI.** Opera en lo que los analistas llaman
    *regulación reactiva*: parches sectoriales. Hubo reformas publicadas en
    **mayo de 2026** a la **Ley Federal del Trabajo** y a la **Ley Federal del
    Derecho de Autor**. El proyecto de Ley Federal de IA (**"Ley Nahui"**) sigue
    en debate.
  - **Brasil: el PL 2338/2023 fue aprobado por el Senado en diciembre de 2024 y
    sigue pendiente de votación en la Cámara de Diputados.** El pass de
    `automotive-kb` del 2026-10-26 midió su última movimiento el 2-sep-2026. **No
    es ley.**
  - 🔴 **Consecuencia para el pitch:** vender "cumplimiento de AI Act" en LATAM es
    vender algo que no existe. El gancho real es **LGPD / protección de datos,
    derecho de autor (la reforma mexicana de mayo-2026) y supervisión de compras
    in-game**, que los gobiernos de la región sí están aumentando.
- **Demanda del jugador — el dato más accionable de la región.** Estudio **PGB Latam
  2026** (presentado en gamescom latam, São Paulo): **48.8% de los gamers
  latinoamericanos compraría juegos desarrollados con AI**, y **40.6% le preocupa la
  precariedad laboral y los despidos**. Es la única región de este archivo con una
  medición de *aceptación del jugador*, y dice que casi la mitad del mercado no
  castiga el uso de AI mientras no implique despidos visibles.
- 🆕 🟢 **LA DIVERGENCIA REGIONAL MÁS VENDIBLE DE ESTE ARCHIVO, ahora que hay con
  qué contrastar.** El 48,8% de aceptación de LATAM (PGB Latam 2026) se lee contra
  el **~85% de actitud negativa** que Quantic Foundry mide sobre ~2 millones de
  jugadores a nivel global (muestra con fuerte peso NA). **Son dos mercados
  distintos frente al mismo producto.** Consecuencia operativa concreta: una
  feature con AI visible al jugador que sería un riesgo de marca en un lanzamiento
  NA es **comercialmente viable en LATAM**, y por tanto **LATAM es el mercado de
  primer lanzamiento racional para producto con AI de cara al jugador**, no el
  último. Es el único argumento de esta KB en el que LATAM no es "más barato" sino
  **estructuralmente mejor**. ⚠️ Honestidad metodológica: las dos encuestas no son
  comparables cabeza a cabeza (distinta muestra, pregunta y escala); lo defendible
  es **la dirección y el tamaño de la brecha**, no la resta de los porcentajes.
- 🆕 **Ecosistema: hay más estudios de los que esta KB asumía.** Microsoft declara
  haber trabajado con **más de 60 estudios** de México, Brasil, Argentina, Chile,
  Colombia, Perú, Honduras y Uruguay vía **ID@Xbox**. **Gamescom Latam 2026** (São
  Paulo, 30-abr a 3-may-2026) tuvo una delegación argentina de **más de 70 estudios
  y empresas**. Estimación periodística, marcada como tal: **>70% de los estudios
  indie de la región** se concentran en México, Argentina y Brasil (Infobae, 2025).
- 🆕 **Lectura de Bain sobre la región:** la AI acelera prototipos, abarata la
  iteración y mejora el conocimiento del jugador, y su impacto **depende de a quién
  beneficie el uso** — que es exactamente el eje del 40,6% de preocupación laboral
  de abajo. Las dos mitades del dato LATAM son una sola conversación.
- **Mobile es la plataforma, no una plataforma:** **86.9% de los gamers en México
  usa el celular como pantalla principal**. México lidera el mercado gaming de LATAM.
- **Adopción general de AI:** Brasil ~76% y México ~70%, de las más altas de la
  región (coloquio, agosto 2026).
- **Mercado gaming LATAM:** $25.7B (2025) → $28.04B (2026), CAGR 9.12%
  *(heredado del pass 2026-07-02, no reverificado en este pass)*.
- 🔴 **Ausencia informada, con denominador:** se buscó específicamente oferta open
  source de gaming AI de origen latinoamericano (en español, apuntando a estudios
  brasileños y argentinos). **No se encontró ningún repositorio de AI para NPC o
  para game dev originado en un estudio LATAM.** Lo más cercano es **Argentum
  Online** — primer MMORPG open source desarrollado en Argentina, liberado en 2018,
  **GPL-3.0-or-later y sin AI de NPC**. Esto no es un fallo de búsqueda: es la
  estructura del mercado. LATAM en gaming AI es **demanda y talento de delivery, no
  oferta de repos**, y ése es precisamente el espacio de un integrador.

| Oportunidad LATAM | Driver | Fit Globant |
|-------------------|--------|-------------|
| **AI NPC asequible** | Los estudios locales no pagan Inworld/Convai por personaje. 48.8% de los jugadores no rechaza AI. | Godot + LimboAI + Ollama. Ver `compose/patterns.md` → Receta 1. |
| **Mobile-first analytics y retención** | 86.9% mobile en México; F2P donde retención > adquisición. | PostHog + GNNs para churn. Receta 5. |
| **iGaming anti-fraude** | Brasil reguló iGaming; supervisión de compras in-game en aumento. | Detección conductual server-side sobre Nakama. Receta 3. |
| **Compliance real: LGPD + derecho de autor** | No hay AI Act, **sí** hay LGPD y la reforma mexicana de derecho de autor de mayo-2026. | Reencuadrar la oferta de compliance; no vender AI Act. |
| **Narrativa de "AI sin despidos"** | 40.6% de preocupación por precariedad es el riesgo de marca local, y **Ubisoft acaba de darle titular** (166 despidos el 19-nov-2026 el mismo día que abrió una subsidiaria de AI generativa). | Posicionar AI en pipeline interno y upskilling, no en reemplazo de artistas. Llevar el dato de GDC: **54% de los despedidos sigue sin empleo y no hay evidencia de causalidad con la AI** — desactiva la objeción sin negarla. |
| 🆕 **Mercado de primer lanzamiento para AI de cara al jugador** | 48,8% de aceptación LATAM vs ~85% de rechazo global. La feature que no se puede lanzar en NA **sí** se puede lanzar acá. | Diseñar el rollout regional: LATAM primero, telemetría de aceptación, y recién después NA/EMEA con evidencia propia en la mano. |
| **Exportación con diferencial AI** | Estudios LATAM compitiendo globalmente sin oferta OSS local. | Globant como acelerador de integración AI. |

---


#### 🆕 Oportunidad del pass 2026-11-22: **la ventana regulatoria brasileña**, y el 48,8% dicho entero

El **PL 2338/2023** sigue sin ser ley: aprobado por el Senado en dic-2024, está
en la Cámara, y el relator anunció el **24-ago-2026** que la votación quedaba
**para después de las elecciones de octubre de 2026**. A nov-2026 no hay fecha.

- **El argumento, y es estructural, no de precio:** un piloto de IA en juego
  lanzado hoy en Brasil corre bajo un régimen **conocido y estable**, mientras
  que el equivalente europeo tiene que presupuestar DFA + AI Act con cronogramas
  en conflicto. **LATAM como mercado de primer lanzamiento para features de IA
  visibles al jugador** es la única tesis de esta KB que no se apoya en que la
  región sea más barata.
- **Reforzado por aceptación medida:** **48,8%** compraría títulos hechos con IA
  (PGB Latam 2026, >7.200 encuestados, **Argentina/Chile/Colombia/México/Perú —
  Brasil NO está en el panel**), contra ~85% de rechazo declarado a nivel global.
- 🔴 **Y hay que decir la otra mitad del mismo estudio: 40,6% está preocupado por
  los despidos y la precarización del trabajo creativo.** Aceptación de compra y
  ansiedad laboral conviven en la misma población encuestada. Un pitch que cite
  48,8% y omita 40,6% se cae cuando el cliente lee el estudio.
- ⚠️ **No confundir con Bain:** el *Gaming Report 2026* (42% más cómodo con IA
  que hace un año; 59% entre adolescentes) es **global, 5.300+ jugadores**,
  aunque varios medios regionales lo presenten como latinoamericano.
- ⛔ **Lo que sigue sin existir, medido tres passes seguidos:** cifras de adopción
  de IA por **estudios** de desarrollo latinoamericanos. El mejor proxy
  disponible es **IBGE 2026: 41,9% de las empresas brasileñas de 100+ empleados**
  — de toda la economía, no del sector. **Si un pitch necesita ese número, hay
  que generarlo, no citarlo**: ahí hay un entregable de investigación vendible.

## Posicionamiento Globant AI Studios

### Propuesta de valor diferenciada
1. **Stack OSS end-to-end verificado**: Godot + Nakama/Talo + SB3 + LimboAI + MCP,
   con los grants leídos uno por uno (ver `agents/top.md`) — sin lock-in de
   plataforma cerrada y sin sorpresas de licencia.
2. **Screening de licencias como entregable propio, y ahora con método
   reproducible.** Acumulado de dos passes sobre repos reales: un badge MIT sin
   archivo de licencia (`chongdashu/unreal-mcp`, 2.120★), una fila MIT que es
   CC-BY-4.0 (`local-llm-npc`), Grafana como Apache-2.0 cuando es AGPL-3.0, un
   world model cuyo grant cambia según el subdirectorio (Matrix-Game), **MonoGame
   publicado como MIT cuando es Ms-PL**, **PostHog como MIT cuando tiene carve-out
   `ee/`**, **O3DE como Apache-2.0 cuando es dual Apache/MIT y arrastra Qt bajo
   LGPL-3.0**, y **un backend descartado por AGPL-3.0 que en realidad es MPL-2.0**.
   Lo diferencial no es la lista: es que el método es **mecánico y auditable** —
   dos canales independientes más un **checksum de bytes** del texto de licencia
   que, aplicado a las filas ya escritas de esta KB, marcó 4 y acertó 4 (ver
   `agents/top.md`). Eso se le entrega a un cliente como procedimiento, no como
   opinión.
3. **Delivery LATAM** con costo y velocidad frente a los grandes integradores.
4. **Patterns listos**: recipes de `compose/patterns.md` → PoC en 2-6 semanas.

### Cuentas objetivo (perfil)
- Publishers con catálogo en la UE y **deadline del 2-dic-2026 a 12 días** sin
  resolver → entrada por inventario de exposición, no por "sprint de marcado".
- 🆕 Publishers o estudios con **NPC conversacional conectado a la tienda o a la
  economía del juego** y publicación en la UE: posible práctica **prohibida** por
  Art. 5, no alto riesgo. Es la conversación más urgente del archivo.
- Estudios AA/AAA que quieren AI en el pipeline interno sin exposición de disclosure
  en NA.
- Operadores iGaming en Brasil/México con supervisión regulatoria creciente.
- Estudios que publican simultáneamente en Corea, China y Japón.
- Estudios indie/mid-size LATAM compitiendo con AAA.

### Competencia
- **Accenture, Wipro, Globant-peers**: presencia enterprise, poco foco gaming.
- **Inworld / Convai**: plataformas cerradas y caras → alternativa OSS consultiva.
- **Estudios locales** (Aquiris Brasil, Luderia México): clientes, no competidores.

---
*Fuentes añadidas 2026-11-20: análisis independiente del censo Steam (~63.600
releases) y contra-conteo sobre SteamDB (49,9% / 49,7% en oct-2026, desglose por
tipo y por tamaño de título); UNLV/KPMG (reguladores de juego NA, Alberta y
Colorado ene-2026); Quantic Foundry (~2M jugadores, ~85% negativo); GDC nov-2026
(54% de despedidos sin empleo); GamesRadar / Analytics Insight / gamedev.net
(Ubisoft Motherbrain y 166 despidos, 19-nov-2026); Niko Partners y smartkarma
(aprobaciones NPPA); BigGo Finance y webpronews (Tencent Game for Peace 110M
usuarios de NPC con AI / 17,7M DAU pico, NetEase Justice Mobile con DeepSeek,
Tencent LAP); FRVR (desmentido del supuesto veto de NetEase); WH Partners,
DLA Piper, twobirds, gamingtechlaw (Art. 5 NPCs que empujan al gasto, Digital
Fairness Act Q4-2026, Digital Omnibus); Malta Gaming Authority (AI Gaming
Charter); xenoss / aimadetools (Corea AI Basic Act 22-ene-2026, Japón AI
Promotion Act y AI Basic Plan jul-2026); Microsoft LATAM (ID@Xbox, 60+ estudios),
Bain vía anda.cl, Gamescom Latam 2026 (delegación argentina 70+); Globant "Game
On" (publicado 18-nov-2025, $189B de revenue global — **outlook de hace un año,
probable nueva edición pendiente**). Repos verificados en dos canales
(`gh api search/repositories` + lectura del grant en `raw.../HEAD/`).*

*Fuentes consultadas 2026-10-26: Research and Markets, Market.us, The Business
Research Company, Precedence Research, Technavio (tamaño de mercado, en conflicto);
GDC 2026 survey vía ShaneTheGamer y Market.us; Sulka Haro (censo Steam ~53.600
releases); digitalcitizen.life (conteo Next Fest de terceros); KPMG y SIGMA/Vixio
(reguladores de gambling); gamingtechlaw.com, twobirds.com, aiandgames.com
(EU AI Act Art. 50 / Anexo III); askajay.ai, paglago.net, iagovernance.com,
techpost.bsa.org (APAC); note.com/commodvs (Krafton, Capcom, Nintendo);
geekzilla.tech (PGB Latam 2026), mediotiempo, infobae, academiadeia
(LATAM regulación y adopción); Globant/Google Cloud Games Report 2025.
Repos citados verificados con `git ls-remote` + lectura de licencia en HEAD.*
