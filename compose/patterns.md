---
industry: gaming
region: Global
updated: 2026-10-26
---

# Patrones de composición — Gaming AI

> Recetas concretas: repos reales, licencias verificadas, cómo se cablean.
> Última actualización: **2026-10-26** (pass previo: 2026-07-02).
>
> Nueve recetas. Las seis primeras vienen del pass de julio con sus cifras y
> licencias corregidas; **las recetas 7, 8 y 9 son nuevas** y responden a cambios
> concretos de este ciclo: una fecha límite regulatoria en la UE, world models
> jugables ejecutables en hardware de consumidor, y la aparición del primer
> benchmark publicado de agentes que juegan.

## Patrón base

```
[Engine / Plataforma base (open source)]
          ↓
[Capa de integración AI (LLM / RL / ML)]
          ↓
[Agentes especializados de Gaming]
          ↓
[UI conversacional / API / juego en producción]
```

---

## Receta 1: NPC con LLM en Godot (recomendado — stack MIT/Apache)

**Caso de uso**: NPCs que hablan naturalmente, recuerdan al jugador, tienen personalidad persistente.

**Stack completo**:
```
Godot 4 (MIT)                          ← engine del juego
└── LimboAI (MIT)                       ← behavior tree del NPC
    └── BTState con trigger HTTP         ← dispara llamada al LLM
        └── Claude / GPT-4o / Ollama    ← LLM (cloud o local)
            └── pre-conversation.json   ← personalidad + conocimiento del personaje
                └── ChromaDB / Qdrant   ← vector store para memoria episódica
```

**Repos**:
- [limbonaut/limboai](https://github.com/limbonaut/limboai) — MIT, **3.1k** stars (releído 2026-10-26). BTs + HSMs para Godot.
- [bitbrain/beehave](https://github.com/bitbrain/beehave) — MIT, 3.2k stars. BTs componibles alternativo.
- [AkshitIreddy/Interactive-LLM-Powered-NPCs](https://github.com/AkshitIreddy/Interactive-LLM-Powered-NPCs) — MIT, 716 stars. Referencia de implementación con voz + memoria.
- [undreamai/LLMUnity](https://github.com/undreamai/LLMUnity) — Apache-2.0, 1.7k stars. Para Unity en vez de Godot.

**Cómo conectar**:
1. Instalar LimboAI como plugin en Godot 4.
2. Crear un BTAction `LLMDialogue` que envía POST al endpoint LLM con: `{character: "...", player_input: "...", memory: [...últimos N eventos...]}`.
3. La respuesta del LLM vuelve como texto → animación de lip sync + texto en UI.
4. Guardar el intercambio en el memory store (ChromaDB vía API HTTP o SQLite local).
5. Opción local sin internet: usar Ollama con Llama 3.1 8B o Gemma 3n on-device.

**Tiempo estimado**: 2-3 semanas para MVP funcional con voz.
**Costo cloud**: ~$0.01-0.05 por conversación con Claude Haiku / GPT-4o-mini.
**Costo local**: $0 con Ollama, requiere GPU en el cliente.

---

## Receta 2: Agente RL para QA automatizado

**Caso de uso**: Reemplazar QA manual con agentes que exploran el juego detectando bugs, exploits de nivel, y estrategias dominantes.

**Stack**:
```
Godot 4 (MIT)                           ← juego a testear como entorno RL
└── godot_rl_agents (MIT)               ← bridge Python ↔ Godot (obs/actions/rewards)
    └── Stable-Baselines3 (MIT)         ← algoritmo PPO / SAC
        └── Entrenamiento con reward:    ← maximiza: coverage + crashes encontrados + exploits
            → Agente explorador
        └── Reporter automático          ← logs → GitHub Issues / Slack
```

**Repos**:
- [edbeeching/godot_rl_agents](https://github.com/edbeeching/godot_rl_agents) — MIT, 900+ stars. Wrappers para SB3, Sample Factory, Ray RLLib, CleanRL.
- [DLR-RM/stable-baselines3](https://github.com/DLR-RM/stable-baselines3) — MIT, 13.5k stars. PPO, SAC, A2C, DQN en PyTorch.
- [Farama-Foundation/Gymnasium](https://github.com/Farama-Foundation/Gymnasium) — MIT, 12.1k stars. API estándar del entorno.

**Cómo conectar**:
1. Definir el espacio de observación: posición del jugador, estado del nivel, items disponibles.
2. Definir las acciones: movimiento, interacciones disponibles.
3. Definir la reward function: +reward por área nueva explorada, +reward por encontrar estados inválidos (out-of-bounds, NaN values), -reward por acciones repetitivas.
4. Entrenar durante 1-5M steps con PPO vía SB3.
5. El agente entrenado se convierte en un "explorador" que corre el juego 24/7 detectando regresiones.

**Tiempo estimado**: 1-2 semanas para primer agente; 3-4 semanas para sistema de QA completo.
**ROI**: reducción de 60-70% en QA manual según benchmarks de la industria.

---

## Receta 3: Backend multijugador + AI features

**Caso de uso**: Juego multijugador con matchmaking inteligente, anti-cheat conductual, y analytics de retención.

**Stack**:
```
Godot 4 (MIT) — cliente
└── SDK nakama-godot (Apache-2.0)
    ↕ WebSocket / HTTP
Nakama server (Apache-2.0)              ← backend multiplayer
    ├── Matchmaking hook (TypeScript)
    │   └── Modelo ONNX de matching     ← PyTorch → exportado como ONNX
    ├── Server-side events               ← cada acción del jugador logeada
    │   └── PostHog (MIT)               ← analytics de comportamiento
    ├── Anti-cheat hook (Go)
    │   └── Detección de anomalías      ← velocidad, puntería, recursos anómalos
    └── Open Match (Apache-2.0)         ← matchmaking enchufable de Google
```

**Repos**:
- [heroiclabs/nakama](https://github.com/heroiclabs/nakama) — Apache-2.0, **13.6k** stars (releído 2026-10-26). Backend completo.
- [heroiclabs/nakama-godot](https://github.com/heroiclabs/nakama-godot) — Apache-2.0. SDK oficial Godot.
- [googleforgames/open-match](https://github.com/googleforgames/open-match) — Apache-2.0. Matchmaking framework de Google.
- [PostHog/posthog](https://github.com/PostHog/posthog) — MIT, 23k stars. Product analytics self-hosted.

**Cómo conectar**:
1. Instalar Nakama via Docker. Configurar SDK en Godot.
2. Crear server-side hooks en TypeScript que interceptan eventos: `after_match_create`, `before_authenticate`.
3. En el hook de matchmaking: llamar al modelo ONNX que predice la "calidad" de un partido (skill balance, latencia, historial de jugadores).
4. Anti-cheat: loguear posición + velocidad + kills por segundo. Si supera umbral estadístico (z-score > 3), flag para revisión.
5. PostHog recibe eventos → dashboards de retención, churn prediction, session analysis.

**Tiempo estimado**: 3-4 semanas para stack completo.

---

## Receta 4: Mundo procedural con AI

**Caso de uso**: Generación infinita de contenido: niveles, quests, diálogos, texturas.

**Stack**:
```
Godot 4 (MIT) — runtime del juego
    ├── PCG de niveles:
    │   └── Wave Function Collapse (WFC) en GDScript
    │       └── LLM para describir constraints en lenguaje natural → parámetros WFC
    ├── PCG de narrativa:
    │   └── LLM (Claude / Llama) para generar quests + diálogos dinámicos
    │       └── Lore base en ChromaDB (RAG) → coherencia narrativa
    └── PCG de assets:
        └── Stable Diffusion (API) para texturas procedurales
            └── [o opcional] AI-generated music con MusicGen (MIT)

```

**Repos**:
- [godotengine/godot](https://github.com/godotengine/godot) — MIT. WFC implementable nativamente.
- [YGYOOO/WorldX](https://github.com/YGYOOO/WorldX) — MIT, 1.1k stars. Generación procedural de mundos con AI (TypeScript).
- [joonspk-research/generative_agents](https://github.com/joonspk-research/generative_agents) — Apache-2.0, 21.7k stars. Para quests y narrativa con agentes que recuerdan.
- [google-deepmind/concordia](https://github.com/google-deepmind/concordia) — Apache-2.0, 1.5k stars. Simulación social para worlds persistentes.

**Cómo conectar**:
1. Definir el "grammar" del nivel: tipos de habitaciones, conexiones posibles, reglas de juego.
2. LLM recibe descripción del jugador y contexto del mundo → genera constraints para el generador.
3. WFC aplica constraints → produce layout de nivel que respeta coherencia.
4. Para quests: agente con memoria (generative_agents pattern) genera objetivos basados en historial del jugador.
5. Assets: llamada a Stable Diffusion API con prompt generado por LLM → texturas coherentes con el bioma.

**Tiempo estimado**: 4-6 semanas para sistema básico; 3-4 meses para calidad AAA-like.

---

## Receta 5: Game Support Agent (in-game chatbot)

**Caso de uso**: Agente de soporte conversacional dentro del juego. Responde preguntas sobre mecánicas, ayuda a jugadores atascados, reemplaza FAQ estático.

**Stack**:
```
Godot 4 UI (MIT) — chat overlay en el juego
    ↕ HTTP
FastAPI server (Python)
    ├── LlamaIndex (MIT) — RAG sobre documentación del juego
    │   ├── game_manual.md → chunks embedidos en ChromaDB
    │   ├── patchnotes.md
    │   └── community_faq.md
    └── LLM (Claude Haiku / GPT-4o-mini / Llama local)
        └── System prompt: "Eres el asistente de [nombre del juego]. Solo respondes sobre mecánicas y soporte. No spoilers."
```

**Repos**:
- [run-llama/llama_index](https://github.com/run-llama/llama_index) — MIT, 40k stars. RAG + agentes sobre datos propios.
- [chroma-core/chroma](https://github.com/chroma-core/chroma) — Apache-2.0. Vector database para embeddings.

**Cómo conectar**:
1. Preparar el corpus del juego: manual, patch notes, FAQs, descripciones de ítems → convertir a Markdown.
2. Indexar en ChromaDB con LlamaIndex usando embeddings de OpenAI o Sentence Transformers (MIT).
3. FastAPI expone endpoint POST `/ask` que recibe pregunta del jugador.
4. LlamaIndex recupera chunks relevantes (top-5 por similitud coseno).
5. LLM recibe chunks + pregunta → genera respuesta natural en el tono del juego.
6. Godot muestra respuesta en UI in-game con animación.

**Costo**: ~$0.001-0.003 por pregunta con Claude Haiku. Para volumen alto: Llama 3.1 8B local en servidor.
**Tiempo estimado**: 1-2 semanas para MVP funcional.

---

## Receta 6: AI Dev Tooling — Godot MCP

**Caso de uso**: Estudio que quiere usar AI para acelerar desarrollo en Godot (generación de scenes, scripts, assets).

**Stack**:
```
Claude Code / Cursor / Codex (cualquier cliente MCP)
    ↕ Model Context Protocol
godot-ai (MIT)                          ← MCP server local
    ↕ Godot Editor API
Godot 4 Editor (MIT)                   ← editor abierto
    ├── Scene building desde descripción natural
    ├── Script GDScript generado y editado
    ├── Signals wired automáticamente
    └── Materials, animations, UI configurados
```

**Repos**:
- [hi-godot/godot-ai](https://github.com/hi-godot/godot-ai) — MIT, **3.2k** stars (releído 2026-10-26; eran 805 en julio). **46 tools** y 120+ operaciones.
- [FlamxGames/godot-ai-assistant-hub](https://github.com/FlamxGames/godot-ai-assistant-hub) — MIT. Alternativa con Ollama/Gemini/OpenRouter.

**Cómo configurar**:
1. Instalar godot-ai desde Godot Asset Library o GitHub.
2. Configurar Claude Code para conectarse al MCP server local (puerto configurable).
3. En Claude Code: `/add-context-window --mcp godot` y empezar a usar lenguaje natural.
4. Ejemplos de prompts: "Crea una escena de un jugador con física de plataformer", "Añade un sistema de inventario al player.gd", "Wire el signal `body_entered` del Area2D al método `on_enemy_hit`".

**Tiempo estimado**: Setup en 1 día. ROI inmediato.

---


---

## 🆕 Receta 7: Sprint de marcado Art. 50(2) del EU AI Act — **vence el 2-dic-2026**

**Caso de uso**: un publisher con catálogo en la UE que usó AI generativa en arte
promocional, copy de marketing o imágenes de juego, y tiene que emitir marcadores
**visibles y legibles por máquina** sobre contenido ya publicado.

**Por qué ahora**: el Art. 50 del EU AI Act **está vigente desde el 2-ago-2026** (quedó
fuera del aplazamiento general). Para sistemas **ya puestos en el mercado**, el
período de gracia del marcado *machine-readable* del **Art. 50(2) termina el 2 de
diciembre de 2026**. Hoy es 2026-10-26: **quedan unas 5 semanas**. El reloj de
alto riesgo (Anexo III) es **otro** y vence el **2-dic-2027**: confundirlos es el
error más común del mercado.

**Stack**:
```
Inventario de assets (fuente de verdad propia del estudio)
    ↓ clasificación por procedencia
    ├── generado por AI y entregado al jugador ──→ EN ALCANCE
    ├── generado por AI sólo en desarrollo     ──→ fuera de alcance (ver nota Steam)
    └── creado por humano                      ──→ fuera de alcance
    ↓ para lo que está en alcance
c2pa-rs (Apache-2.0)  /  c2pa-python (Apache-2.0)
    ↓ firma Content Credentials sobre el asset
Manifiesto C2PA embebido  ← la parte "machine-readable"
    +
Disclosure visible en store page / in-game  ← la parte "visible", Art. 50(1)
    ↓ auditoría
Reporte de cobertura: assets en alcance / marcados / pendientes, con fecha
```

**Repos**:
- [contentauth/c2pa-rs](https://github.com/contentauth/c2pa-rs) — **Apache-2.0**
  (`LICENSE-APACHE`, 11335 B). SDK de referencia de Content Credentials.
- [contentauth/c2pa-python](https://github.com/contentauth/c2pa-python) —
  **Apache-2.0** (`LICENSE-APACHE`, 11335 B). Binding Python: lo práctico para
  correrlo sobre un pipeline de assets existente.

**Cómo conectar**:
1. **Inventariar por procedencia, no por tipo.** La pregunta que define el alcance
   es "¿este asset lo ve el jugador y lo generó un sistema de AI?". La mayoría de
   los estudios no tiene esa columna en ninguna parte: construirla *es* la primera
   semana del trabajo.
2. Clasificar contra las dos obligaciones separadas: **Art. 50(1)** = disclosure al
   jugador cuando interactúa con un sistema de AI, salvo que sea obvio;
   **Art. 50(2)** = marcado machine-readable del contenido generado.
3. Firmar los assets en alcance con `c2pa-python` en el paso de build, no a mano.
4. Emitir el disclosure visible en la store page y, si hay NPCs conversacionales,
   también in-game.
5. Entregar un **reporte de cobertura fechado**: es lo que un regulador o un
   partner contractual va a pedir, y es el entregable que el cliente puede mostrar.

🟢 **Nota que cambia el alcance y ahorra trabajo:** Steam reescribió su política el
**17-ene-2026** y **exime explícitamente las herramientas AI de desarrollo**
(GitHub Copilot citado por nombre), obligando sólo sobre el contenido AI que llega
al jugador. El criterio contractual de la tienda y el criterio legal de la UE
coinciden en la línea "¿lo ve el jugador?", así que un solo inventario sirve para
los dos.

⚠️ **Ambigüedad que se declara, no se resuelve a favor del cliente:** las fuentes
legales **no coinciden** en si la dificultad adaptativa, la reacción al
comportamiento del jugador y las historias personalizadas caen bajo alto riesgo
(Anexo III). Una guía sostiene que las herramientas generales de generación de
assets no caen automáticamente; otra, que los sistemas adaptativos sí. Se vende
como alcance estatutario a monitorear hasta el **2-dic-2027**, no como "no aplica".

**Tiempo estimado**: 3-5 semanas para un catálogo mediano. **Es la única receta de
este archivo con fecha de caducidad externa.**

---

## 🆕 Receta 8: World model encima de un engine existente

**Caso de uso**: prototipado visual de alta fidelidad sin pipeline de arte, o
exploración de dirección visual antes de comprometer presupuesto de assets.

**Por qué ahora**: **Matrix-Game 4.0** salió el **15-oct-2026** y su arquitectura
**no reemplaza el engine**: lo mantiene. El engine ejecutable conserva lógica,
física e input; el modelo causal de video convierte frames low-poly en visuales.
Eso lo hace adoptable de forma incremental — la diferencia entre una demo y un
roadmap. Corre a **67 FPS en una RTX 5080** y 58 FPS en una 5070 Ti a 864×480 con
SageAttention; también hay rutas para A100 y RTX 5090 con FP8, kernels fusionados y
CUDA graphs.

**Stack**:
```
Godot (MIT) o Unity — el engine sigue siendo tuyo
    │   lógica de juego, física, input, estado: NO se delega al modelo
    ↓ frames low-poly RGB + prompt de texto
Matrix-Game 4.0  — Apache-2.0 en Matrix-Game-4/
    ↓ streaming neural rendering, causal, por chunks
Salida visual de alta fidelidad
    ↓ comparación
MineWorld (MIT) — alternativa Microsoft si el dominio es voxel/Minecraft
    checkpoints 300M / 700M / 1200M, 4-7 fps con Diagonal Decoding
```

**Repos**:
- [SkyworkAI/Matrix-Game](https://github.com/SkyworkAI/Matrix-Game) — ⚠️ **grant
  partido**: `LICENSE` raíz = **MIT** (1118 B); `Matrix-Game-4/LICENSE` =
  **Apache-2.0** (11357 B). El sidebar de GitHub muestra sólo MIT. La versión que se
  usa es v4, o sea **Apache-2.0**: hay que preservar `NOTICE` y viene con grant de
  patente. Incluye los tres juegos nativos (Medieval Castle, Ancient Egypt, City
  Racing), inferencia y demo en browser.
- [microsoft/MineWorld](https://github.com/microsoft/MineWorld) — **MIT**
  (`LICENSE`, 1141 B). Transformer autoregresivo visual-acción.

**Cómo conectar**:
1. Construir la escena en el engine con geometría low-poly y materiales planos. El
   presupuesto de arte se mueve de "modelar y texturizar" a "bloquear la escena".
2. Exportar el framebuffer RGB low-poly como entrada del modelo.
3. Definir los prompts de estilo por escena o por bioma. Es el control artístico
   real: el modelo no inventa la geometría, la reviste.
4. Correr la inferencia por la ruta de la GPU disponible (A100 / 5090 / 5080 / 5070 Ti).
5. Mantener **toda** la lógica de juego en el engine: el modelo no es autoridad de
   estado y no se le pide que lo sea.

🔴 **Verificación pendiente, bloqueante para uso comercial:** la licencia de los
**pesos** (publicados en Hugging Face) **no se pudo leer en este pass** —
`huggingface.co` no resuelve DNS en este entorno. Precedente concreto de este mismo
programa: en `automotive-kb`, Alpamayo 1.5 tiene **código Apache-2.0 y pesos
OpenMDW-1.1**. **No asumir que el grant del código cubre los pesos.** Primer paso de
cualquier engagement con esta receta: leer la licencia de los pesos.

⚠️ Para MineWorld, una nota de mayo-2025 en su README dice que los checkpoints de HF
fueron "temporally taken down". Confirmar disponibilidad antes de planificar.

**Tiempo estimado**: 4-6 semanas para prototipo jugable. Requiere GPU dedicada.

---

## 🆕 Receta 9: QA con agentes, **medido** — LMGame Bench + MCP

**Caso de uso**: reemplazar o complementar QA manual con agentes que juegan, y
poder **demostrar** que juegan bien. La Receta 2 entrena un explorador RL; esta
mide a cualquier agente contra un estándar publicado y conecta los hallazgos al
editor.

**Por qué ahora**: en julio este KB recomendaba QA con RL sin ningún instrumento de
medición. **GamingAgent / LMGame Bench** (MIT, 998 ★, **ICLR 2026**) es ese
instrumento: harness de gaming, leaderboard público y despliegue de computer-use
agents sobre PC.

**Stack**:
```
El juego (Godot / Unity, build de QA)
    ↓
GamingAgent (MIT) — harness + computer-use agent
    ├── modo vanilla: VLM solo, sin harness → baseline del modelo
    └── modo harness: GamingAgent workflow → techo alcanzable
    ↓ hallazgos (crash, estado inválido, estrategia dominante)
godot-ai (MIT, 46 tools) o Unity-MCP (Apache-2.0)
    ↓ el agente abre el editor y reproduce el caso
Issue con escena de repro adjunta, no sólo un log
    ↓ regresión
godot_rl_agents (MIT) + Stable-Baselines3 (MIT) — explorador entrenado, 24/7
```

**Repos**:
- [lmgame-org/GamingAgent](https://github.com/lmgame-org/GamingAgent) — **MIT**
  (`LICENSE`, 1065 B), 998 ★, ICLR 2026.
- [hi-godot/godot-ai](https://github.com/hi-godot/godot-ai) — **MIT**, 3.2k ★,
  46 tools y 120+ operaciones.
- [IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP) — **Apache-2.0**,
  4.5k ★. Cualquier método C# se convierte en tool con una línea: así se expone el
  estado interno del juego al agente sin escribir un puente.
- [edbeeching/godot_rl_agents](https://github.com/edbeeching/godot_rl_agents) — **MIT**.
- [DLR-RM/stable-baselines3](https://github.com/DLR-RM/stable-baselines3) — **MIT**.

**Cómo conectar**:
1. Medir **primero el baseline**: el modelo jugando sin harness. Sin eso no se sabe
   si la mejora vino del agente o del modelo, y es la métrica que el cliente va a
   cuestionar.
2. Activar el harness de GamingAgent y volver a medir sobre las mismas tareas.
3. Exponer el estado interno del juego como tools MCP (en Unity, un atributo por
   método; en Godot, vía `godot-ai`), para que el agente observe estado y no sólo
   píxeles.
4. Cuando el agente encuentra un fallo, **usar el MCP para reproducirlo en el
   editor** y adjuntar la escena de repro al issue. Un repro cuesta menos de
   triagear que un log.
5. Dejar el explorador RL (Receta 2) corriendo como red de regresión continua.

**Ecosistema de evaluación alrededor** (contexto, no dependencias): **CUA-bench**
(agentes jugando seis juegos comerciales con teclado y mouse), **RuneScape-Bench**,
**GameLogicBench** (aserciones de estado a nivel de tick), **MineExplorer**, y
**Roblox OpenGameEval** (47 escenarios) del lado plataforma.

**Tiempo estimado**: 1-2 semanas para el baseline medido; 3-4 semanas con MCP y
repro automático.

---

## Tabla resumen

| Patrón | Stack principal | Esfuerzo | ROI esperado |
|--------|----------------|---------|--------------|
| 1 · NPC con LLM | Godot + LimboAI (MIT) + Ollama/Claude | 2-3 semanas | +40% immersion (dato de industria, no medido aquí) |
| 2 · QA automatizado con RL | Godot + godot_rl_agents + SB3 | 3-4 semanas | -60% QA manual (benchmark de industria) |
| 3 · Multiplayer backend inteligente | Nakama (13.6k★) o **Talo (MIT)** + Open Match + PostHog | 3-4 semanas | Mejor matchmaking → retención |
| 4 · Mundo procedural | Godot + WFC + LLM + Concordia | 4-8 semanas | Contenido infinito, replayability |
| 5 · Game Support Agent | LlamaIndex (MIT) + FastAPI + Godot UI | 1-2 semanas | -70% tickets de soporte |
| 6 · AI dev tooling | **godot-ai (3.2k★) / Unity-MCP (4.5k★)** + Claude Code | 1 día de setup | 2-3x velocidad de desarrollo |
| 🆕 7 · **Marcado Art. 50(2) EU AI Act** | c2pa-python (Apache-2.0) + inventario de procedencia | **3-5 semanas** | 🔴 **Deadline externo: 2-dic-2026** |
| 🆕 8 · World model sobre engine | Godot/Unity + Matrix-Game 4.0 (Apache-2.0 en v4) | 4-6 semanas | Prototipo visual sin pipeline de arte |
| 🆕 9 · **QA con agentes, medido** | GamingAgent/LMGame Bench (MIT) + MCP | 1-4 semanas | QA con número defendible, no anécdota |

### Cuál proponer según la región del engagement

| Región | Receta de entrada | Por qué |
|--------|-------------------|---------|
| **EMEA** | **7**, luego 6 | Es la única región con una fecha legal dura (2-dic-2026) y un segundo reloj en 2-dic-2027. |
| **North America** | **6** y **9**, luego 2 | La reescritura de Steam del 17-ene-2026 exime las herramientas de desarrollo: la AI interna no dispara disclosure ni backlash, y 52% de los devs ve la gen AI como daño. |
| **APAC** | **8**, luego 7 adaptada | Los dos world models abiertos punteros son de APAC (Skywork, Tencent), y Corea/China ya obligan etiquetado. |
| **LATAM** | **1**, **3** y **5** | No hay AI Act que vender; sí hay 48.8% de jugadores que compraría juegos hechos con AI, 86.9% de mobile en México y operadores iGaming con supervisión creciente. |

---
*Repos y licencias verificados el 2026-10-26 con `git ls-remote` + lectura del
archivo de grant en HEAD, con control negativo en ambos sentidos. Cifras de ★ de
`godot-ai`, `Unity-MCP`, `limboai`, `nakama`, `GamingAgent`, `Matrix-Game`,
`MineWorld` y `TaloDev/backend` leídas de la página del repo el 2026-10-26; el
resto se hereda del pass 2026-07-02 sin reverificar.*
