---
industry: gaming
region: Global
updated: 2026-11-21
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

**Repos** (★ y `pushed_at` releídos 2026-11-20):
- [hi-godot/godot-ai](https://github.com/hi-godot/godot-ai) — MIT, **3.627★**,
  **commit de hoy**. 46 tools y 120+ operaciones. **Ésta es la recomendación para
  trabajo de cliente**, por mantenimiento activo.
- 🆕 [Coding-Solo/godot-mcp](https://github.com/Coding-Solo/godot-mcp) — MIT,
  **6.713★**, el más adoptado del ecosistema Godot (**15×** el `tomyud1/godot-mcp`
  que esta KB listaba como "segunda opción") ⚠️ **pero sin commits desde
  2026-04-16, 7 meses.** Trade-off que hay que declarar al cliente: adopción vs
  mantenimiento. Si pesa el ecosistema y los issues resueltos, éste; si pesa el
  soporte, `godot-ai`.
- [FlamxGames/godot-ai-assistant-hub](https://github.com/FlamxGames/godot-ai-assistant-hub) — MIT. Alternativa con Ollama/Gemini/OpenRouter.

🔴 **Si el engine es Unity, corregido este pass:** usar
**[CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp)** (**MIT,
16.087★**, ⚠️ rama default `beta`), no `IvanMurzak/Unity-MCP` (Apache-2.0,
4.672★) que esta KB recomendaba como "dominante". El líder era invisible por un
rename (`justinpbarnett/unity-mcp` → `CoplayDev/unity-mcp`). `IvanMurzak` sigue
siendo la opción correcta si el cliente **exige Apache-2.0** por política.
Si el engine es Unreal: `prajwalshettydev/UnrealGenAISupport` (MIT, 686★) es el
único con grant limpio y volumen — **no** `chongdashu/unreal-mcp`, que tiene
2.120★ y **ningún archivo de licencia**.

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
diciembre de 2026**. El reloj de alto riesgo (Anexo III) es **otro** y vence el
**2-dic-2027**: confundirlos es el error más común del mercado.

---

### 🔴 REENCUADRE OBLIGATORIO — actualizado **2026-11-21: quedan 11 DÍAS**

Esta receta se escribió el 2026-10-26 como un **sprint de 3-5 semanas** contra el
2-dic-2026. **Hoy es 2026-11-20 y ese entregable ya no cabe.** Proponerlo tal cual
es vender un incumplimiento con fecha de entrega posterior a la obligación. La
receta técnica de abajo **sigue siendo correcta y se conserva íntegra** — es lo que
hay que construir —, pero **el encuadre comercial cambia**:

| Fase | Alcance | Plazo real | Para qué sirve |
|------|---------|-----------|----------------|
| **A. Inventario de exposición** | Qué SKUs del catálogo publicado llevan contenido generado, y qué metadata tienen hoy. Nada de marcado todavía. | **Días, no semanas.** Cabe antes del 2-dic. | Es el entregable que convierte "no sabemos" en "sabemos y está acotado". Sin esto no hay priorización posible. |
| **B. Marcado del subconjunto crítico** | Sólo lo de mayor riesgo y visibilidad: arte promocional y copy de marketing de los títulos activos. | Parcial al 2-dic. | Reduce exposición real donde más pega. |
| **C. Plan fechado post-deadline** | El resto del catálogo, con fechas, responsables y la brecha **documentada**. | Posterior al 2-dic. | **Un plan con brecha declarada es lo que un regulador y un auditor valoran**; un cumplimiento fingido es lo que los hunde. |

**La regla honesta:** decirle al cliente que llega completo al 2-dic es el error,
no la oferta. Y si el cliente llega hoy con el catálogo sin inventariar, la
conversación correcta empieza por A, no por C2PA.

🔴 **Y antes de cualquier marcado, un chequeo que puede cambiar el proyecto
entero:** la Comisión Europea citó como ejemplo de **práctica PROHIBIDA (Art. 5)**
—no de alto riesgo— los **NPCs generados por AI diseñados para empujar al jugador
a gastar más dentro del juego**, y comentario legal extiende el criterio a la
optimización de engagement aplicada a **jugadores menores de edad**. Si el cliente
tiene un NPC conversacional conectado a la tienda o a la economía del juego, **eso
no se marca: se rediseña o se apaga.** Marcar un sistema prohibido no lo legaliza.
Ésa es la primera pregunta de la reunión, antes del inventario.

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

## 🆕 Receta 10: Auditoría de licencias de un stack de gaming — **método mecánico**

**Caso de uso**: due diligence antes de adoptar OSS en un proyecto de cliente, o
saneamiento de un inventario heredado. Es el entregable más vendible de esta KB
porque **el error que corrige es el default del mercado** y porque el método es
auditable en vez de opinable.

**Por qué es una receta y no un consejo**: en dos passes, aplicado a repos reales,
este procedimiento encontró 4 licencias mal clasificadas **en las propias tablas de
esta KB**, un repo descartado por una licencia que no tiene, y un repo recomendado
que estaba archivado. Ninguno era un repo oscuro: MonoGame tiene 14.663★, PostHog
40.990★, O3DE 9.894★.

**Stack** (sin dependencias, sólo dos canales y aritmética):
```
Para cada repo del inventario:

CANAL 1 — gh api -X GET search/repositories -f q='repo:<slug>'
    → existencia | SPDX | ★ | archived | pushed_at | default_branch
    en UNA llamada

CANAL 2 — curl -s raw.githubusercontent.com/<slug>/HEAD/<archivo>
    → el texto del grant, con BYTES
    (HEAD resuelve la rama default: NO hardcodear main/master)

CHECKSUM — ¿los bytes caen en la banda canónica de la familia declarada?
    MIT/Expat   ~1.050-1.150      Ms-PL      ~3.900
    Apache-2.0  ~11.300-11.400    MPL-2.0    ~16.700
    CC-BY-4.0   ~18.650           LGPL-2.1   ~26.400
    AGPL-3.0    ~34.500           GPL-3.0    ~35.200

DECISIÓN
    bytes POR ENCIMA de la banda  → licencia compuesta o familia equivocada: LEER
    bytes MUY POR DEBAJO          → archivo PUNTERO: seguir el puntero, NO "sin licencia"
    SPDX = NOASSERTION            → NO es "sin licencia": es "no matcheó un SPDX único"
                                    ⇒ detector de licencia compuesta: LEER SIEMPRE
```

**Los seis modos de fallo que este procedimiento caza, con el caso real de cada uno:**

| Modo de fallo | Caso verificado | Qué canal lo detecta |
|---------------|-----------------|---------------------|
| **Badge sin grant** | `chongdashu/unreal-mcp`: README con badge "License: MIT", **ningún archivo de licencia**, 2.120★ | Canal 2 (404 en todas las rutas) + SPDX `NONE` |
| **Familia equivocada** | `MonoGame`: publicado MIT, es **Ms-PL** (3882 B) | **Checksum** |
| **Carve-out por directorio** | `PostHog`: MIT Expat **+ `ee/` bajo licencia enterprise** (1563 B) | **Checksum** + lectura |
| **Grant que cambia por subdirectorio** | `Matrix-Game`: raíz **MIT** (1118 B), `Matrix-Game-4/` **Apache-2.0** (11357 B); el sidebar de GitHub muestra sólo MIT | Canal 2 sobre **rutas sondeadas**, no sobre la raíz |
| **Archivo puntero / licencia dual** | `o3de`: 1475 B porque es un puntero a "**Apache-2.0 *o* MIT a elección**", y advierte **Qt bajo LGPL-3.0** | **Checksum** (muy por debajo de la banda) |
| **Licencia agregada código+assets** | `love2d/love` (74927 B), `luanti`, `stk-code`, `0ad`: un solo archivo cubre engine y assets con grants distintos | Lectura de **cabecera**, nunca `grep` del cuerpo |

**Pasos**:
1. **Listar el inventario real** — lo que está en `package.json`, `.csproj`,
   `addons/`, submódulos y plugins del editor. No lo que el equipo cree que usa.
2. **Correr los dos canales** por cada slug. Guardar `(slug, SPDX, ★, archived, pushed_at, default_branch, archivo, bytes, primera línea)`.
3. **Aplicar el checksum.** Toda fila marcada se lee a mano. **Anotar los bytes en
   el entregable**: es lo que permite auditar la auditoría el mes siguiente.
4. **Clasificar por consecuencia, no por nombre de licencia**, que es lo que el
   cliente necesita decidir:
   - **Permisivo limpio** (MIT, Apache-2.0, BSD, Zlib) → construible.
   - **Permisivo con obligación** (Apache-2.0 ⇒ `NOTICE`; **Ms-PL ⇒ la fuente
     redistribuida sigue siendo Ms-PL**) → construible **con tarea asignada**.
   - **Copyleft por archivo** (MPL-2.0) → hostear no contamina; modificar el
     archivo sí.
   - **Copyleft de red / fuerte** (AGPL-3.0, GPL, LGPL) → decisión de arquitectura.
   - **Carve-out** (`ee/`) → la feature que el cliente quiere puede estar
     justamente del lado cerrado. **Verificar la feature, no el repo.**
   - **No OSI / sin grant** → fuera, o licencia comercial.
5. **Chequear vida, no sólo licencia:** `archived` es un booleano que ni ★ ni
   `pushed_at` revelan — **`0ad/0ad` tiene 2.827★ y está archivado** desde 2024, y
   el proyecto migró a un Gitea propio: **el repo está muerto y el juego vivo.**
6. **Revalidar los rechazos, no sólo las recomendaciones.** Un descarte mal
   fundado es peor que una recomendación floja: saca al candidato del campo de
   visión y nadie vuelve a mirarlo. `NamazuStudios/elements` estuvo descartado un
   mes por una AGPL-3.0 que no tiene.

**Control negativo obligatorio**: probar un slug inexistente
(`gmilano/this-repo-does-not-exist-control-xyz` → `NOT_FOUND`) y un slug conocido
vivo. Sin los dos, no se sabe si el instrumento distingue algo. ⚠️ **`curl -sI
https://github.com/<slug>` NO sirve como prueba de existencia** en este entorno:
devuelve respuestas del proxy, no del repo.

**Tiempo estimado**: 2-4 días para un stack de 30-50 dependencias, incluyendo la
lectura a mano de lo que marque el checksum. **Entregable**: tabla por consecuencia
+ las filas marcadas con su texto citado + lista de tareas (NOTICE, atribuciones,
decisiones de arquitectura) + las que requieren opinión legal.

**Límite honesto que hay que declarar**: esto es una **auditoría de procedencia
declarada**, no una opinión legal ni un escaneo de código copiado. Dice qué dicen
los archivos de licencia; no dice si el repo respeta las licencias de *sus*
dependencias, ni resuelve la pregunta de los **pesos de modelos**, que esta KB
**nunca ha podido verificar** (`huggingface.co` no resuelve DNS aquí, 3 passes).

---

## 🆕 Receta 11: Pipeline de estudio con agentes por rol

**Caso de uso**: estudio que ya usa un agente en el editor y quiere pasar de
"acelerar tareas" a "cubrir roles del pipeline" — la demanda que el mercado está
expresando con más fuerza que ninguna otra en este ciclo (ver `intel/trends.md` → 10).

**Stack**:
```
Claude Code (u otro cliente de agentes)
    ├── Donchitos/Claude-Code-Game-Studios (MIT, 40.377★)
    │       49 agentes con rol + 72 skills de workflow
    │       ↳ definiciones de agente y workflow: ES PROMPTS, no runtime
    ├── MCP del engine  ← la capa que SÍ toca el proyecto
    │       Unity  → CoplayDev/unity-mcp   (MIT, 16.087★, rama `beta`)
    │       Godot  → hi-godot/godot-ai     (MIT, 3.627★, mantenido)
    │       Unreal → UnrealGenAISupport    (MIT, 686★)
    ├── EL4CTEO/blender-skills (MIT, 2.529★)   ← arte 3D, que el MCP de engine no cubre
    └── godot_rl_agents (MIT) + SB3 (MIT)      ← QA, vía Receta 2
            ↕
      GamingAgent / LMGame Bench (MIT)         ← medición, vía Receta 9
```

**Cómo se arma**:
1. **Empezar por el MCP del engine, no por la flota de agentes.** El MCP es lo que
   da acceso real al proyecto; los 49 agentes son definiciones de rol que sin un
   canal al editor no tocan nada.
2. Instalar `Claude-Code-Game-Studios` y **desactivar todo menos 2-3 roles**
   (p. ej. *level design*, *gameplay scripting*, *QA*). 49 agentes activos a la vez
   es ruido, no pipeline.
3. Añadir `blender-skills` sólo si hay pipeline de arte 3D propio.
4. **Poner la puerta de calidad antes de ampliar:** Receta 9 (LMGame Bench) para
   que "el agente ayuda" sea un número y no una impresión.
5. Recién entonces activar más roles, de a uno, midiendo.

**Lo que hay que decirle al cliente, y es la mitad del valor de esta receta**:
- **40.377★ miden interés, no producción.** Es un repo de **prompts**: barato de
  marcar como favorito, barato de publicar, y **sin evidencia pública de adopción
  en estudios**. No presentarlo como estándar de industria.
- **El cuello de botella no es el agente, es la revisión.** Un pipeline con 49
  roles genera más output del que un equipo chico puede revisar, y output no
  revisado en un proyecto de juego es deuda de arte y de código.
- **El trabajo real es la calibración**, no la instalación: el setup es 1-2 días y
  la calibración semanas. Vender el setup como el proyecto es el error.

**Tiempo estimado**: 1-2 días de setup, **3-6 semanas de calibración** con la
puerta de medición de la Receta 9 puesta desde el día uno.

---

## 🆕 Receta 12: Capacidad elástica de game servers — la capa de ops que faltaba

**Caso de uso**: un juego multijugador de sesión que ya funciona y necesita escalar
sin sobreaprovisionar, o que opera servidores a mano desde un panel.

**Por qué está aquí**: esta KB tenía `Pterodactyl` (panel para hosts) y **nada para
Kubernetes** desde su primer pass. Agones es el estándar del nicho y faltaba.

**Stack**:
```
Nakama (Apache-2.0, 13.757★)  o  Talo (MIT, 105★)     ← backend de juego / meta
        ↕
agones-dev/agones (Apache-2.0, 7.106★)                 ← 🆕 orquestación en K8s
    ├── GameServer       — un proceso de servidor = un recurso de K8s
    ├── Fleet            — pool de servidores listos (warm capacity)
    └── FleetAutoscaler  — escala por GameServers libres, NO por CPU
        ↕
googleforgames/open-match (Apache-2.0, 3.427★)         ← matchmaking enchufable
        ↕
PostHog (MIT Expat + carve-out ee/) + Grafana ⚠️ AGPL-3.0
```

**Pasos**:
1. Empaquetar el binario de servidor con el **SDK de Agones** (`Ready()`,
   `Health()`, `Shutdown()`): sin eso el ciclo de vida no es observable y el
   autoscaler no puede decidir.
2. Definir un `Fleet` con capacidad caliente mínima. **El autoscaler de Agones
   escala por *GameServers libres*, no por CPU** — es la diferencia con un HPA
   genérico y la razón de usarlo: un servidor de partida está ocupado aunque su CPU
   esté baja.
3. Conectar matchmaking: Open Match asigna, Agones provee la instancia.
4. Capa AI, vía Receta 3: hooks de Nakama para scoring de matchmaking, detección
   de botfarm en `after_authenticate`, anti-cheat conductual sobre el stream.
5. Observabilidad: PostHog para producto. ⚠️ **Si el dashboard se expone a
   terceros, Grafana es AGPL-3.0** — usar Apache Superset o mantenerlo interno.

**Trampas verificadas**:
- 🔴 **El slug cambió:** `googleforgames/agones` **ya no es canónico**
  (`NOT_FOUND` en la API; el `raw` viejo responde 200 por redirect). Es
  `agones-dev/agones`. Toda la documentación y los blogs siguen citando el viejo.
- **Agones no reemplaza a Pterodactyl**: panel para operar hosts vs capacidad
  elástica en K8s. Son capas distintas y un cliente con 3 servidores fijos **no
  necesita Agones** — introducir Kubernetes donde no hay elasticidad que gestionar
  es el anti-patrón de esta receta.

**Tiempo estimado**: 2-3 semanas con K8s ya operativo; 5-7 si hay que montar el
cluster y la observabilidad.

---

## 🆕 Receta 13: Capa de disclosure por reloj de sesión para NPCs con LLM (NA, vigente 1-ene-2027)

**Para qué.** Las leyes estatales de *companion chatbots* de EE.UU. obligan a
**recordar periódicamente, dentro de la sesión**, que se está hablando con una
AI. Es la primera obligación que toca el **runtime** de un NPC, y no se resuelve
con un cartel en la store ni con metadata en el build.

**Las fechas, que son el motor del proyecto:**

| Estado | Instrumento | Vigencia | Cadencia exigida |
|--------|-------------|----------|------------------|
| Washington | HB 2225 (24-mar-2026) | **1-ene-2027** | Recordatorios periódicos en sesiones largas, **intervalo más corto para menores**. Trae **acción privada** |
| Connecticut | SB 5 → Public Act 26-15 (27-may-2026) | 🟡 1-oct-2026 *o* 1-ene-2027 (las fuentes discrepan — **verificar en el texto antes de comprometer fecha**) | **≥1×/hora a menores, ≥1×/3 h a adultos** + protocolo de crisis con derivación al **988** |
| Idaho | S 1297 *Conversational AI Safety Act* (31-mar-2026) | **1-jul-2027** | Marco de seguridad de AI conversacional |

**Stack (todo MIT/Apache-2.0, nada nuevo que comprar):**

```
Godot 4.x (MIT)
  └── LimboAI (MIT) o Beehave (MIT, rama godot-4.x)   ← el BT que ya gobierna el NPC
        └── nodo BTCondition "DisclosureDue"           ← lee el reloj de sesión
  └── autoload SessionClock.gd                         ← monotónico, persiste en save
  └── autoload AudienceFlag.gd                         ← menor / adulto / desconocido
  └── canal de presentación (3 opciones, ver abajo)
  └── log append-only de disclosures mostradas         ← la evidencia
NPC: npcpy (MIT) · LLMUnity (Apache-2.0) · NPCTalk (MIT, si hay voz)
```

**Cómo se cablea, en orden:**

1. **`SessionClock`**: tiempo acumulado de *interacción con el NPC*, no de
   partida. Reloj **monotónico** (no `Time.get_unix_time`, que el jugador puede
   mover), persistido en el save para que cerrar y reabrir no reinicie la cuenta.
2. **`AudienceFlag`**: tres estados, no dos. **`desconocido` se trata como
   menor** — es la opción barata y es la que defiende. Fuente: edad de la cuenta
   de plataforma si está disponible, declaración propia si no.
3. **`DisclosureDue`** como **condición del Behavior Tree**, no como timer
   suelto: así el aviso se dispara **entre turnos de diálogo** y no corta una
   línea a mitad. Umbral: 60 min si `menor|desconocido`, 180 min si `adulto`.
4. **Canal de presentación**, de menos a más invasivo —elegir uno y documentar
   por qué:
   - **diegético**: el NPC lo dice en personaje (*"recordá que soy una
     construcción"*). Preserva inmersión; ⚠️ **riesgo**: si el texto lo genera el
     LLM, el aviso puede no salir. **Debe ser texto fijo**, nunca generado.
   - **banner no modal** de 3–5 s. Neutro y auditable. **Default recomendado.**
   - **modal con confirmación**. Sólo si el cliente quiere la defensa más fuerte.
5. **Log append-only** (timestamp, id de sesión, flag de audiencia, canal,
   versión del texto). Es lo único que, llegado el caso, **prueba** el
   cumplimiento. Sin log, no hay defensa.
6. **Protocolo de crisis (Connecticut)**: clasificador de intención sobre la
   entrada del jugador → si hay señal de riesgo de suicidio, autolesión o
   violencia, **cortar el personaje** y mostrar recurso con **988**. Esto **no**
   lo decide el LLM del NPC: es una ruta separada, determinista y registrada.

**Esfuerzo:** 1–2 semanas sobre un NPC que ya existe. 3–5 días si el NPC ya corre
sobre un Behavior Tree (Receta 1).

🟡 **Lo que hay que decirle al cliente, textual, porque es la parte honesta:**
**ninguna de las tres leyes dice que un NPC de videojuego sea un *companion
chatbot*.** El *Harvard Journal of Sports & Entertainment Law* (ago-2026)
plantea la pregunta y la deja abierta. ⇒ esto **no se vende como "cumplimiento"**:
se vende como **preparación de bajo costo frente a una definición que todavía se
está escribiendo**. El reloj, el flag y el log cuestan días; rehacer el sistema
de diálogo de un juego ya lanzado, no.

🟢 **Reutilización:** el `SessionClock` + `AudienceFlag` son exactamente lo que
pide el **disclosure a nivel de sesión** que Bird & Bird describe como
alternativa europea cuando marcar contenido efímero en tiempo real **no es
técnicamente factible**. **La misma pieza sirve en NA y en EMEA**, por razones
legales distintas. Construirla una vez.

---

## 🆕 Receta 14: Auditoría de **alcance** de licencias — los 6 ejes (extiende la Receta 10)

**Para qué.** La Receta 10 responde *"¿esta fila dice la licencia correcta?"*.
Esta responde la pregunta que de verdad decide un entregable: ***"¿qué licencia
tiene esta parte, en este formato, en este territorio, para este uso?"***. Un
inventario con un solo campo `license` por repo **no puede representar 6 de los
16 repos `NOASSERTION` de esta KB**.

**Los 6 ejes, con el repo testigo verificado y la pregunta que hay que hacer:**

| # | Eje | Testigo | Pregunta al cliente |
|---|-----|---------|---------------------|
| 1 | **Directorio** | `PostHog/posthog` (`ee/`), `SkyworkAI/Matrix-Game` (`Matrix-Game-4/` Apache-2.0 vs raíz MIT) | ¿Qué subárbol vamos a usar y a distribuir? |
| 2 | **Tipo de medio** | `luanti-org/luanti`: código LGPL-2.1+, texturas/sonidos CC-BY-SA 3.0, Lua MIT — **en un solo archivo** | ¿Nos llevamos los assets o sólo el código? |
| 3 | **Archivo** | `MinLL/SkyrimNet-GamePlugin`: mod propietario con `SkyrimNetApi.psc` en **MIT** | ¿Integramos contra una API o empaquetamos el producto? |
| 4 | **Territorio** | `Tencent-Hunyuan/HY-World-2.0`: *no aplica en EU, UK ni Corea del Sur* | ¿En qué jurisdicciones se publica el juego? |
| 5 | **Campo de uso** | `AnkleBreaker-Studio/unity-mcp-server`: prohibida la reventa como herramienta/producto/servicio; permitido el consulting alrededor | ¿El software es el valor principal del entregable o un componente? |
| 6 | **Binario vs fuente** | `MonoGame/MonoGame` Ms-PL §3(D): binario libre, **fuente sólo bajo Ms-PL** | ¿Entregamos binario o código fuente al cliente? |

**Procedimiento (medio día sobre un stack ya inventariado):**

```bash
# 1. Badge + metadatos en una llamada (NO es prueba de licencia)
gh api -X GET search/repositories -f q="repo:$SLUG"   --jq '.items[]|"\(.full_name)|\(.stargazers_count)|\(.license.spdx_id)|\(.archived)|\(.pushed_at)|\(.default_branch)"'

# 2. Lectura de bytes con las 11 variantes de nombre (HEAD resuelve la rama default solo)
for f in LICENSE LICENSE.txt LICENSE.md License.txt license.txt LICENCE          COPYING COPYING.txt LICENSE-MIT LICENSE-APACHE NOTICE; do
  curl -s -o /tmp/l -w "$f %{http_code} %{size_download}\n"     "https://raw.githubusercontent.com/$SLUG/HEAD/$f"
  head -c 200 /tmp/l   # ← la cabecera, para identificar la familia
done
```

**Los tres discriminadores que convierten bytes en veredicto:**

1. **Bytes dentro de una banda canónica** (MIT ~1.100 · Ms-PL ~3.900 · Apache
   ~11.350 · MPL ~16.700 · CC-BY ~18.650 · LGPL-2.1 ~26.400 · AGPL ~34.500 ·
   GPL-3.0 ~35.200) ⇒ familia confirmada.
2. **Bytes muy por debajo** de la banda ⇒ **archivo puntero**: seguir el puntero
   (`ml-agents` 572 B, `UnrealMLAgents` 609 B, `o3de` 1475 B — los tres son
   permisivos, ninguno contiene su licencia).
3. 🆕 **Bytes fuera de TODA banda** ⇒ **archivo compuesto**: buscar los
   **encabezados de sección**, no leer la cabecera del archivo
   (`luanti` 8793 B, `love2d` 74.927 B, `0ad` 3049 B, `AnkleBreaker` 3068 B).
   ⚠️ **Éste es el eje que atrapa al lector apurado:** las primeras líneas del
   `LICENSE.txt` de Luanti hablan de **CC-BY-SA** porque la sección de arte va
   primera; el código, LGPL-2.1+, está en la línea 114.

**Salida del ejercicio:** no una columna `license`, sino
`license · scope · formato · territorio · campo_de_uso · notas`. Es la forma
mínima que puede representar el stack real de un juego.

🟢 **Por qué es entregable facturable y no higiene interna:** sobre las tablas de
esta propia KB, el método produjo en dos passes **4 reclasificaciones correctas
sobre 4 alertas**, **1 rechazo revertido** (`elements` era MPL-2.0, no AGPL-3.0),
**3 repos más permisivos de lo publicado** (O3DE, `elements`, C2PA) y **2
licencias propias escondidas detrás de un badge** (AnkleBreaker, HY-World 2.0).
Sin una sola dependencia nueva y sin acceso privilegiado.

---

## 🆕 Receta 15: *Gate* de licencia por región en el pipeline de build

**Para qué.** Descubrimos este pass que **una licencia puede caerse por
jurisdicción**: la de `HY-World 2.0` declara que **no se aplica en la Unión
Europea, el Reino Unido ni Corea del Sur**. Un estudio con un solo build y varias
tiendas **no tiene hoy ninguna forma de notar eso** hasta que lo nota legal.

**Patrón, 2–3 días:**

1. **Manifiesto por componente** (YAML junto al lockfile), un registro por
   dependencia con los 6 campos de la Receta 14. Mínimo viable:
   ```yaml
   - slug: Tencent-Hunyuan/HY-World-2.0
     license: LicenseRef-Tencent-HY-World-2.0-Community
     osi: false
     territory_excluded: [EU, UK, KR]
     scope: weights+code
     deliverable: binary-only
   ```
2. **Gate en CI**: el job de release recibe la lista de **mercados de
   lanzamiento** y falla si algún componente excluye uno de ellos. Es un `for`
   sobre dos listas; no hace falta herramienta.
3. **Sustituto declarado por adelantado**, no buscado en la crisis. Para world
   models: `SkyworkAI/Matrix-Game` (MIT en raíz, **Apache-2.0 en
   `Matrix-Game-4/`**, que es la versión que un estudio usaría) y
   `microsoft/mineworld` (MIT). ⚠️ **la licencia de los *pesos* sigue sin
   verificar** en los dos casos (`huggingface.co` no resuelve en este entorno,
   4º pass): el gate cubre el código, **no los pesos**, y eso hay que escribirlo
   en el manifiesto como riesgo abierto, no omitirlo.
4. **Lo mismo para el campo de uso**: si el entregable al cliente es la
   herramienta misma, `AnkleBreaker Open License` §3 la bloquea; si es consulting
   alrededor, está expresamente permitida. **El gate necesita saber qué se
   factura**, y por eso este paso se decide con el equipo comercial, no con el
   de ingeniería.

🟢 **Dónde encaja en la rotación:** este gate es la pieza que faltaba entre la
Receta 10 (¿la etiqueta es correcta?) y la Receta 14 (¿cuál es el alcance?). La
secuencia completa es **10 → 14 → 15**: verificar, acotar, **y hacer que el
pipeline lo recuerde solo**.

---

## Tabla resumen

| Patrón | Stack principal | Esfuerzo | ROI esperado |
|--------|----------------|---------|--------------|
| 1 · NPC con LLM | Godot + LimboAI (MIT) + Ollama/Claude | 2-3 semanas | +40% immersion (dato de industria, no medido aquí) |
| 2 · QA automatizado con RL | Godot + godot_rl_agents + SB3 | 3-4 semanas | -60% QA manual (benchmark de industria) |
| 3 · Multiplayer backend inteligente | Nakama (13.757★) o **Talo (MIT)** + Open Match + PostHog | 3-4 semanas | Mejor matchmaking → retención |
| 4 · Mundo procedural | Godot + WFC + LLM + Concordia | 4-8 semanas | Contenido infinito, replayability |
| 5 · Game Support Agent | LlamaIndex (MIT) + FastAPI + Godot UI | 1-2 semanas | -70% tickets de soporte |
| 6 · AI dev tooling | 🔴 **corregido:** godot-ai (3.627★) / **CoplayDev/unity-mcp (MIT, 16.087★)** + Claude Code | 1 día de setup | 2-3x velocidad de desarrollo |
| 7 · **Marcado Art. 50(2) EU AI Act** | c2pa-python (Apache-2.0) + inventario de procedencia | 🔴 **reencuadrada: inventario en días, marcado parcial, plan fechado** | 🔴 **Deadline externo a 12 DÍAS: 2-dic-2026** |
| 🆕 8 · World model sobre engine | Godot/Unity + Matrix-Game 4.0 (Apache-2.0 en v4) | 4-6 semanas | Prototipo visual sin pipeline de arte |
| 9 · **QA con agentes, medido** | GamingAgent/LMGame Bench (MIT, ⚠️ congelado hace 1 año) + MCP | 1-4 semanas | QA con número defendible, no anécdota |
| 🆕 10 · **Auditoría de licencias** | 2 canales + checksum de bytes (sin dependencias) | **2-4 días** | 🟢 **4/4 de acierto sobre las tablas de esta KB.** Entregable propio y auditable |
| 🆕 11 · **Pipeline de estudio con agentes** | Claude-Code-Game-Studios (MIT, 40.377★) + MCP de engine + blender-skills | 1-2 días setup, **3-6 sem de calibración** | Cubrir roles, no acelerar tareas |
| 🆕 12 · **Capacidad elástica de game servers** | Agones (Apache-2.0, 7.107★) + Nakama/Talo + Open Match | 2-3 semanas (K8s listo) | Escalar sin sobreaprovisionar |
| 🆕 13 · **Disclosure por reloj de sesión (NPC con LLM)** | Godot + LimboAI/Beehave (MIT) + `SessionClock` + log append-only + NPCTalk (MIT) | **1-2 semanas** (3-5 días si ya hay BT) | 🔴 **Vigencia externa 1-ene-2027 (WA) · cadencia 1 h/3 h (CT)**. Preparación, no cumplimiento: la definición de "companion chatbot" para NPCs sigue abierta |
| 🆕 14 · **Auditoría de ALCANCE de licencias (6 ejes)** | 2 canales + 11 nombres de archivo + 3 discriminadores de bytes | **medio día** sobre stack inventariado | 🟢 Sobre esta KB: 4/4 reclasificaciones, 1 rechazo revertido, 3 repos **más** permisivos, 2 licencias propias destapadas |
| 🆕 15 · **Gate de licencia por región en CI** | Manifiesto YAML de 6 campos + job de release + sustitutos predeclarados | **2-3 días** | 🟢 Evita descubrir en el lanzamiento que un componente **no se licencia** en el mercado de destino |

### Cuál proponer según la región del engagement

| Región | Receta de entrada | Por qué |
|--------|-------------------|---------|
| **EMEA** | 🔴 **primero el chequeo Art. 5, luego 7 reencuadrada (inventario), luego 10, luego 15** · 🆕 **11 días** | Fecha legal dura **a 12 días** y un segundo reloj en 2-dic-2027. **Y antes del marcado: si hay NPC conversacional conectado a la tienda, puede ser práctica PROHIBIDA (Art. 5) — eso no se marca, se rediseña.** |
| **North America** | 🆕 **13** primero si hay NPC conversacional, luego **6** y **9**, luego 2 y **10** | Steam exime las herramientas de desarrollo: la AI interna no dispara disclosure ni backlash, y 🆕 **55%** de los devs ve la gen AI como daño (GDC nov-2026; era 52%) y ~85% de los jugadores la rechaza. 🆕 **Adopción estadounidense estancada: 36%→38%, dentro del margen de error** ⇒ acá se vende *primer paso*, no escala. 🆕 **Y ya hay obligación de runtime: Receta 13.** **Dato nuevo: sólo 3% de las declaraciones de AI en Steam son generación en gameplay** — el pipeline interno es el 97% del mercado real. La Receta 10 vende bien donde los reguladores declaran "visibilidad limitada". |
| **APAC** | **8** y **11**, luego 7 adaptada, **10** y 🆕 **15** | Los world models abiertos punteros son de APAC (Skywork, Tencent) y Corea/China ya obligan etiquetado (Corea **extraterritorial** desde 22-ene-2026). **Y es la única región con NPCs con AI a escala de producción probada: Tencent reporta 110M de usuarios acumulados y 17,7M de DAU pico en *Game for Peace*** — acá la conversación no es "si funciona", es integración. La Receta 10 es obligatoria si se construye sobre I+D china (Hunyuan no es OSI; 🔴 **HY-World 2.0 sí publica licencia — `License.txt` — y excluye EU, UK y Corea**). 🆕 **Adopción japonesa 85,8% con 63% de uso diario (CESA 2026, 1.349 respuestas)** ⇒ acá no se vende adopción: se vende **gobernanza, escala y evaluación de calidad de salidas** (la preocupación nº1 declarada es precisión/calidad, 51,9%). |
| **LATAM** | **1**, **3**, **5** y **12** (y **13** sólo si se publica en EE.UU.) | No hay AI Act que vender; sí hay **48,8% de jugadores que compraría juegos hechos con AI** —contra ~85% de rechazo global— lo que hace de LATAM **el mercado de primer lanzamiento racional para AI de cara al jugador**, no el último. Más 86,9% de mobile en México y operadores iGaming con supervisión creciente. ⚠️ Cuidar la narrativa laboral: 40,6% de preocupación por precariedad (PGB Latam 2026), y Ubisoft acaba de ponerle titular. 🆕 **El único compliance con fecha en LATAM es publicitario, no de producto: guía CONAR de Brasil en vigor desde el 1-jun-2026** (el PL 2338/2023 sigue trabado en la Cámara ⇒ **no hay ley general de AI en vigor**). |

---
*Pass **2026-11-21**: recetas nuevas **13** (capa de disclosure por reloj de
sesión para NPCs con LLM — la primera obligación de NA que toca el runtime),
**14** (auditoría de **alcance** de licencias por 6 ejes, con repo testigo
verificado por eje) y **15** (gate de licencia **por región** en CI, nacido del
hallazgo de que la licencia de HY-World 2.0 no se aplica en EU/UK/Corea). Receta
**7** actualizada a **11 días**. Receta **10** extendida con el tercer
discriminador de bytes (*fuera de toda banda ⇒ archivo compuesto*). Tabla por
región actualizada con los cortes de adopción de GDC nov-2026 y CESA 2026.*

---
*Repos y licencias verificados el **2026-11-20 en dos canales**
(`gh api search/repositories` con qualifier `repo:` exacto → existencia + SPDX + ★
+ `archived` + `pushed_at` + rama default; `raw.../HEAD/<archivo>` → texto del
grant con bytes), con control negativo en ambos sentidos. **Todas las ★ de este
archivo son del 2026-11-20**: no queda ninguna heredada sin verificar. Recetas
nuevas de este pass: **10** (auditoría de licencias con checksum de bytes),
**11** (pipeline de estudio con agentes por rol), **12** (capacidad elástica con
Agones). Receta **7 reencuadrada** porque su deadline pasó de 5 semanas a 12 días.
Receta **6 corregida** en la elección de MCP para Unity y Godot.*
