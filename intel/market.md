---
industry: gaming
region: Global
updated: 2026-11-21
---

# Mapa de mercado — Gaming AI

> Key players, market map y oportunidades **por región**.
> Última actualización: **2026-11-20** (passes previos: 2026-10-26, 2026-07-02).
>
> En el pass anterior las secciones de North America, EMEA y APAC decían
> literalmente `- To research` y sólo LATAM tenía contenido. Las cuatro regiones
> de este archivo ahora tienen hallazgos propios, y donde una región no devolvió
> nada se dice explícitamente qué se buscó y qué faltó.

## 🔴 Lo que cambió en 24 h — leer esto primero (2026-11-21)

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

#### 🆕 2026-11-21 — NA dejó de tener sólo políticas de tienda: ahora tiene una obligación de *runtime*

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

### EMEA

#### 🆕 2026-11-21 — quedan **11 días**, y aparece el matiz de la Comisión que hay que decir en la sala

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
  DSA sobre UGC y protección de menores, y reglas de loot boxes.

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
  digitales, juegos incluidos. Para un publisher europeo, el monetization design
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

### APAC

#### 🆕 2026-11-21 — APAC tiene ahora **su propia cifra de adopción**, y es 2,2× la "global"

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

### LATAM

#### 🆕 2026-11-21 — la ventaja de LATAM se sostiene, y ahora tiene su contrapeso medido

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
