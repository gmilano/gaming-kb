---
industry: gaming
region: Global
updated: 2026-11-21
---

# Tendencias — Gaming AI

> Última actualización: **2026-11-21 (cycle 6)** (passes previos: 2026-11-21 cycle 5, 2026-11-20, 2026-10-26, 2026-07-02).
> Cada tendencia lleva la señal que la sostiene. Donde este pass **refuta** algo
> que el propio KB publicaba, se dice.

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
| 🆕 **Digital Fairness Act** | Borrador previsto para **Q4-2026**: diseño adictivo y mecánicas aleatorizadas (loot boxes) | Anunciado; convierte el monetization design en materia regulada |
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
- **Listado de directorios de un repo**: sigue sin canal. Por eso el grant partido
  de Matrix-Game se verifica por sonda de rutas conocidas y no puede afirmarse que
  `Matrix-Game-4/` sea el único subdirectorio con licencia propia.
- **`eur-lex.europa.eu` no es alcanzable** desde este entorno, así que el registro
  primario del EU AI Act **sigue sin leer tras 5 passes y 2 KBs**. Las fechas de
  este archivo provienen de comentario legal secundario, consistente entre sí pero
  no verificado en la fuente oficial.
- **Oferta OSS de origen LATAM**: buscada explícitamente en español apuntando a
  estudios brasileños y argentinos. **Cero repos de gaming AI.** Detalle y
  denominador en `intel/market.md` → LATAM.

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
