---
industry: gaming
region: Global
updated: 2026-10-26
---

# Mapa de mercado — Gaming AI

> Key players, market map y oportunidades **por región**.
> Última actualización: **2026-10-26** (pass previo: 2026-07-02).
>
> En el pass anterior las secciones de North America, EMEA y APAC decían
> literalmente `- To research` y sólo LATAM tenía contenido. Las cuatro regiones
> de este archivo ahora tienen hallazgos propios, y donde una región no devolvió
> nada se dice explícitamente qué se buscó y qué faltó.

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

🔴 **Las dos cifras extremas (97% y 29%) son compatibles y hay que venderlas
juntas:** casi todo el mundo toca una herramienta con AI dentro, y una minoría
decreciente declara "usar gen AI". Lo que cambió entre 2025 y 2026 no es la
capacidad: es que **declararlo tiene costo reputacional**. Un pitch que abre con
"87% de los estudios ya usa agentes" se choca con que 52% de los devs del cliente
piensa que la AI está dañando su industria. El ángulo que sobrevive a esa sala es
**AI en el pipeline interno** (QA, localización, build, ops), no AI en el
contenido que ve el jugador.

---

## Players globales — engines y plataformas

| Empresa | Tipo | Fortaleza AI | Debilidad | Modelo |
|---------|------|-------------|-----------|--------|
| **Unity Technologies** | Engine propietario | Unity AI 6.2 integrado (Assistant, Generators, Inference Engine). ml-agents (Apache-2.0). El ecosistema MCP más adoptado corre sobre Unity: `IvanMurzak/Unity-MCP` 4.5k ★. | Controversia de pricing 2023 aún en la memoria del mercado. | Revenue share + suscripción |
| **Epic Games (Unreal)** | Engine propietario | MetaHuman AI. Aura agent. Plugin MCP first-party experimental reportado en UE 5.8. | Royalty 5% > $1M. **Tim Sweeney atacó públicamente la política de disclosure de AI de Steam** como "abhorrent" — Epic está posicionado contra el etiquetado, lo que es un riesgo de marca para un partner. | Royalty |
| **Valve / Steam** | Distribución | **Es el regulador de facto del AI en juegos.** Política de disclosure desde 9-ene-2024, **reescrita el 17-ene-2026**. | No produce tooling AI. | Revenue share |
| **NVIDIA** | GPU + SDK | ACE (ASR+NLP+TTS+animación NPC). **DLSS 5 presentado en GTC 2026 (mar-2026)**: modelo generativo capaz de transformar skins y modelos de personaje en juego. | Dependencia de hardware NVIDIA. | Hardware + SDK |
| **Skywork AI** | Research (APAC) | 🆕 **Matrix-Game 4.0 (15-oct-2026)**: el world model jugable abierto más avanzado. 67 FPS en RTX 5080. Código y juegos publicados. | Licencia partida (MIT raíz / Apache-2.0 en v4); pesos en HF sin verificar. | Open weights |
| **Microsoft** | Cloud + research | 🆕 **MineWorld** (MIT): world model Minecraft en tiempo real. Azure AI para gaming. Sponsor de O3DE. | Sin engine propio competitivo. | Cloud usage |
| **Tencent** | Publisher + research (APAC) | Hunyuan-GameCraft: video de juego interactivo desde teclado y mouse. | 🔴 Licencia community propia, **no OSI**. No es base construible. | Propietario |
| **Roblox** | Plataforma UGC | AI Studio con cliente MCP. CUBE 3D. OpenGameEval OSS. | Audiencia mayormente juvenil. | Revenue share |
| **Google DeepMind** | Research + cloud | Concordia (Apache-2.0). Genie 2 / Genie 3. | 🔴 **La línea Genie no es abierta**: reports y blog posts, sin código. Todo lo jugable abierto es de Skywork, Microsoft o Tencent. | Cloud API |
| **Heroic Labs** | Backend OSS | Nakama (Apache-2.0, **13.6k ★**). | Heroic Cloud desde ~$600/mes. | OSS + managed |
| **Inworld AI / Convai** | AI NPC (cerrado) | NPCs con voz, memoria, emoción. | Pricing por personaje o por llamada, cerrado. | SaaS |

---

## Opportunities by region

Una regla de este archivo: **cada región tiene su propio gancho regulatorio y su
propia oferta local**. No se reutiliza el argumento de LATAM en EMEA.

### North America

**Lo que manda aquí no es una ley: es la política de una tienda.**

- **Steam reescribió su política de disclosure el 17-ene-2026** y separó dos cosas
  que antes iban juntas: las **herramientas AI usadas durante el desarrollo** (p.ej.
  GitHub Copilot) quedan **explícitamente exentas** de disclosure, y el **contenido
  generado por AI que se le entrega al jugador** debe declararse. Hay además un
  reporte in-game, pero sólo cubre contenido *live-generated*.
- **Penetración real del etiquetado** (censo de ~53.600 releases): **10.9% (2024) →
  19.9% (2025) → 30.8% (2026 hasta julio)**. Proyección de prensa: más de la mitad
  de los lanzamientos llevarían disclosure para 2027-2028.
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

| Oportunidad NA | Driver | Fit Globant |
|----------------|--------|-------------|
| **Pipeline AI "no declarable"** | La reescritura de 17-ene-2026 exime las herramientas de desarrollo. Hay un espacio limpio: acelerar build, QA, localización y ops sin disparar disclosure ni backlash. | Es exactamente la capa interna donde Globant ya opera. Ver `compose/patterns.md` → Receta 2 y 6. |
| **Auditoría de disclosure pre-lanzamiento** | 30.8% de los releases ya etiqueta y el criterio cambió hace 9 meses. Muchos estudios no saben de qué lado cae su propio pipeline. | Servicio corto y vendible: inventario de assets por procedencia + mapeo contra el survey de Steam. |
| **AI responsable para operadores iGaming** | Los reguladores dudan de la autorregulación y pocos operadores tienen política interna. | Marco de AI responsable + monitoreo de daño al jugador, auditable. |
| **Anti-fraude conductual** | Carril iGaming NA con supervisión activa. | GNNs sobre red de jugadores + detección server-side. |

### EMEA

**La única región con una fecha límite dura, y cae en 5 semanas.**

- **EU AI Act, Artículo 50 (transparencia): aplicable desde el 2-ago-2026.** Quedó
  **fuera** del aplazamiento general, así que ya está vigente.
- 🔴 **2 de diciembre de 2026: fin del período de gracia para el marcado
  *machine-readable* del Art. 50(2)** en sistemas ya puestos en el mercado. Hoy es
  2026-10-26: **quedan ~5 semanas.**
- **Alto riesgo / Anexo III se movió a 2 de diciembre de 2027** (se esperaba
  2-ago-2027). Hay por tanto **dos relojes distintos** y confundirlos es el error
  que se ve en el mercado.
- **Qué obliga concretamente en un juego:** disclosure al jugador cuando interactúa
  con un sistema de AI, salvo que sea obvio (Art. 50(1)); y marcadores **visibles y
  legibles por máquina** en copy de marketing, arte promocional e imágenes de juego
  generadas por AI.
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

### APAC

**Tres regímenes distintos en una región, y la oferta open source más fuerte del
ciclo.**

- **Corea del Sur — AI Basic Act en vigor desde enero de 2026.** Es el régimen más
  completo de la región: los proveedores de AI generativa deben **notificar al
  usuario por adelantado y etiquetar la salida**. Techo de sanción modesto
  (KRW 30 millones) y período de gracia de al menos un año antes de multas.
- **China — regula por tipo de servicio**, no por riesgo: reglas vinculantes
  separadas para algoritmos de recomendación, *deep synthesis*, AI generativa y
  etiquetado de contenido. Para un juego con contenido generado, el etiquetado es
  obligación directa.
- **Japón — promoción sin sanciones.** Hay propuesta de divulgación **voluntaria**
  de datos de entrenamiento que en práctica opera como obligatoria, porque el
  incumplidor queda nombrado públicamente.
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

### LATAM

**El gancho NO es compliance de AI, y este pass lo corrige.**

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
| **Narrativa de "AI sin despidos"** | 40.6% de preocupación por precariedad es el riesgo de marca local. | Posicionar AI en pipeline interno y upskilling, no en reemplazo de artistas. |
| **Exportación con diferencial AI** | Estudios LATAM compitiendo globalmente sin oferta OSS local. | Globant como acelerador de integración AI. |

---

## Posicionamiento Globant AI Studios

### Propuesta de valor diferenciada
1. **Stack OSS end-to-end verificado**: Godot + Nakama/Talo + SB3 + LimboAI + MCP,
   con los grants leídos uno por uno (ver `agents/top.md`) — sin lock-in de
   plataforma cerrada y sin sorpresas de licencia.
2. **Screening de licencias como entregable propio.** Este KB encontró, en un solo
   pass: un badge MIT sin archivo de licencia, una fila publicada como MIT que es
   CC-BY-4.0, Grafana publicada como Apache-2.0 cuando es AGPL-3.0, y un world model
   cuyo grant cambia según el subdirectorio. Ese error es el default del mercado.
3. **Delivery LATAM** con costo y velocidad frente a los grandes integradores.
4. **Patterns listos**: recipes de `compose/patterns.md` → PoC en 2-6 semanas.

### Cuentas objetivo (perfil)
- Publishers con catálogo en la UE y **deadline del 2-dic-2026** sin resolver.
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
