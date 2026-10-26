---
industry: gaming
region: Global
updated: 2026-10-26
---

# Tendencias — Gaming AI

> Última actualización: **2026-10-26** (pass previo: 2026-07-02).
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

### 3. El etiquetado en Steam se normalizó — y la exención de herramientas es la grieta
- Penetración (censo de ~53.600 releases): **10.9% (2024) → 19.9% (2025) → 30.8%
  (2026 a julio)**. Proyección de prensa: >50% para 2027-2028.
- 🟢 **La reescritura del 17-ene-2026 exime explícitamente las herramientas de
  desarrollo** (GitHub Copilot citado por nombre) y sólo obliga sobre el contenido
  AI que llega al jugador. Hay por tanto un espacio limpio y grande para AI interna.
- ⚠️ El ~36% que circula para Next Fest **no es cifra de Valve**: es conteo de
  terceros (2.593 de 7.145). Valve nunca publicó disclosure por edición.
- Epic (Tim Sweeney) atacó la política como "abhorrent", argumentando que las
  etiquetas funcionan como declaración política. La industria **no** tiene consenso.

### 4. 🟢 MCP ganó en gaming, y el ganador es el plugin de engine — no el framework
La categoría que más creció desde julio. No es NPCs: son agentes conectados al editor.

| Repo | Licencia | ★ (2026-10-26) | Nota |
|------|----------|----------------|------|
| [IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP) | Apache-2.0 | **4.5k** | Cualquier método C# → tool en una línea |
| [hi-godot/godot-ai](https://github.com/hi-godot/godot-ai) | MIT | **3.2k** (era 805 en julio) | 46 tools, 120+ operaciones |
| [tomyud1/godot-mcp](https://github.com/tomyud1/godot-mcp) | MIT | 444 | Segunda opción Godot |
| [IvanMurzak/Unreal-MCP](https://github.com/IvanMurzak/Unreal-MCP) | Apache-2.0 | 51 | Temprano |
| [IvanMurzak/GameDev-MCP-Server](https://github.com/IvanMurzak/GameDev-MCP-Server) | Apache-2.0 | 16 | Backend compartido |

**El dato duro:** `godot-ai` multiplicó ★ por ~4 en cuatro meses. Y la asimetría
4.5k / 51 / 16 **dentro del mismo autor** dice dónde está la adopción: en el plugin
de Unity, no en la abstracción compartida. Se reporta además un plugin MCP
first-party experimental en UE 5.8 — si Epic lo estabiliza, la tier community de
Unreal se consolida de golpe.

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
- **generative_agents** de Stanford (Apache-2.0, 21.7k ★) sigue siendo la
  arquitectura de referencia: memory stream + reflection + planning.
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
- 🔴 Dos trampas de licencia verificadas este pass: **Elements** (namazustudios) es
  **AGPL-3.0**, y **Pelican** (fork de Pterodactyl) **no tiene archivo de licencia**
  en HEAD.

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

**La conclusión operativa:** en gaming, un badge o una tabla heredada no es
evidencia. El método está documentado en `agents/top.md` y es vendible como
entregable propio.

---

## En el radar (emergente, 2026-2027)

| Tendencia | Señal verificada | Estado |
|-----------|------------------|--------|
| **Plugin MCP first-party en Unreal** | Reportado como experimental en UE 5.8 | Sin confirmar en fuente primaria |
| **Rendering neural sobre engine existente** | Matrix-Game 4.0, 67 FPS en RTX 5080 | Ejecutable hoy en hardware de consumidor |
| **DLSS 5 generativo** | Presentado en GTC 2026 (mar-2026): transforma skins y modelos en juego | Producto NVIDIA, no OSS |
| **AI companions como feature de producto** | Krafton *Co-Playable Character* → "PUBG Ally", modo arcade, 1S-2026 | Anunciado |
| **Rechazo editorial explícito a gen AI** | Capcom negó usarla en contenido; CEO de Nintendo dijo que no la usarán por riesgo de IP | La división es pública |
| **Un solo estudio asistido por agentes** | [WhiteTowerAI/ohmygame](https://github.com/WhiteTowerAI/ohmygame), Apache-2.0, 1.2k ★, en trending de GitHub el 2026-10-26 | Tracción nueva |
| **Modding asistido por AI** | [rehan-remade/universal-modder](https://github.com/rehan-remade/universal-modder), MIT, ~8.9k ★ | Categoría con volumen propio |
| **Pesos con licencia distinta al código** | Matrix-Game y MineWorld publican pesos en HF | 🔴 **No verificable desde aquí** (ver gaps) |

## 🔴 Retirado del radar
- **Aivill** (`SKYHUBDev/Aivill`) estaba listado en julio como "villanos adaptativos
  que aprenden del jugador — concepto emergente a observar". Verificado el
  2026-10-26: el repo está **vivo pero vacío**, sin README y sin licencia en HEAD. No
  hay proyecto que observar.
- **OpenGame** (`leigest519/OpenGame`, Apache-2.0, 3.0k ★) se mantiene, con
  advertencia: **última novedad en su README es del 2026-04-21**, seis meses sin
  anuncios.

## Gaps declarados
- **Licencia de los pesos**: `huggingface.co` no resuelve DNS en este entorno
  (`getaddrinfo ENOTFOUND`), así que el grant de los pesos de Matrix-Game 4.0 y de
  los checkpoints de MineWorld **no está verificado**. En world models, los pesos
  son la mitad del entregable. Precedente: en `automotive-kb`, Alpamayo 1.5 tiene
  código Apache-2.0 y pesos **OpenMDW-1.1**.
- **Descubrimiento**: `api.github.com/search/*` está bloqueado para esta sesión
  ("sessions are bound to their configured repositories"), así que no hubo búsqueda
  por topic ni por estrellas. El descubrimiento dependió de búsqueda web, que sesga
  hacia lo indexado y escrito en inglés.
- **Oferta OSS de origen LATAM**: buscada explícitamente en español apuntando a
  estudios brasileños y argentinos. **Cero repos de gaming AI.** Detalle y
  denominador en `intel/market.md` → LATAM.

---
*Fuentes 2026-10-26: GDC 2026 survey (vía ShaneTheGamer, Market.us); Sulka Haro
(censo Steam); digitalcitizen.life; notebookcheck (Sweeney); pcgamer;
gamingtechlaw.com, twobirds.com, aiandgames.com (EU AI Act); askajay.ai,
paglago.net, iagovernance.com, techpost.bsa.org (APAC); note.com/commodvs
(Krafton/Capcom/Nintendo); strayspark.studio y mcp-scorecard.ai (ecosistema MCP);
arxiv.org (GameLogicBench, MineExplorer); benchlm.ai; raiscui/gh-trends
(daily 2026-10-26). Repos verificados con `git ls-remote` + lectura de licencia
en HEAD, con control negativo.*
