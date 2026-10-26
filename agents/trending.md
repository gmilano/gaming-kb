---
industry: gaming
region: Global
updated: 2026-10-26
---

# Agentes trending — Gaming AI

> **Archivo APPEND-ONLY.** Cada pass agrega una sección fechada arriba y conserva
> la historia completa debajo. La historia es el dato: sin ella no hay tendencia,
> sólo una foto.

---

## 🆕 2026-10-26

Primer pass sobre esta KB desde el 2026-07-02 (116 días). Todo lo de abajo se
verificó primera mano: `git ls-remote` para existencia y lectura del archivo de
licencia en `raw.githubusercontent.com/<slug>/HEAD/`, con control negativo
(`gmilano/this-repo-does-not-exist-control-xyz` → correctamente UNREACHABLE). Las
★ se leyeron de la página del repo el 2026-10-26.

### 🟢 Lo nuevo con tracción real

| Repo | Licencia (archivo, bytes) | ★ 2026-10-26 | Señal |
|------|--------------------------|--------------|-------|
| [IvanMurzak/Unity-MCP](https://github.com/IvanMurzak/Unity-MCP) | Apache-2.0 (`LICENSE`, 10197) | **4.5k** | El MCP de engine más adoptado del ecosistema, y **no estaba en esta KB**. Cualquier método C# se convierte en tool con una línea. |
| [SkyworkAI/Matrix-Game](https://github.com/SkyworkAI/Matrix-Game) | ⚠️ partida: raíz MIT (1118) / `Matrix-Game-4/` Apache-2.0 (11357) | **2.8k** | **Matrix-Game 4.0 liberado el 15-oct-2026.** Engine ejecutable + rendering neural causal. **67 FPS en RTX 5080** a 864×480. Tres juegos nativos publicados. |
| [lmgame-org/GamingAgent](https://github.com/lmgame-org/GamingAgent) | MIT (`LICENSE`, 1065) | **998** | **ICLR 2026.** LMGame Bench + harness + leaderboard. El benchmark de agentes que juegan que esta KB declaraba faltante en julio. |
| [microsoft/MineWorld](https://github.com/microsoft/MineWorld) | MIT (`LICENSE`, 1141) | 493 | World model Minecraft en tiempo real. Checkpoints 300M/700M/1200M, 4-7 fps con Diagonal Decoding. Contrapeso no-APAC a Matrix-Game. |
| [tomyud1/godot-mcp](https://github.com/tomyud1/godot-mcp) | MIT (`LICENSE`, 1071) | 444 | Segunda opción MCP real del ecosistema Godot. |
| [WhiteTowerAI/ohmygame](https://github.com/WhiteTowerAI/ohmygame) | Apache-2.0 (`LICENSE`, 11357) | **1.2k** | "Estudio de un solo desarrollador": canvas de juego + BYO key. **En el trending de GitHub del 2026-10-26** (release `daily-2026-10-26` de `raiscui/gh-trends`). |
| [IvanMurzak/Unreal-MCP](https://github.com/IvanMurzak/Unreal-MCP) | Apache-2.0 (`LICENSE`, 11341) | 51 | Plugin C++ de editor + bridge .NET. Temprano. |
| [IvanMurzak/GameDev-MCP-Server](https://github.com/IvanMurzak/GameDev-MCP-Server) | Apache-2.0 (`LICENSE`, 10197) | 16 | Backend MCP engine-agnostic de los tres anteriores. |
| [rehan-remade/universal-modder](https://github.com/rehan-remade/universal-modder) | MIT (`LICENSE`, 1096) | ~8.9k* | Modding asistido por AI: categoría con volumen propio que esta KB no cubría. |

### 📈 Crecimiento medido sobre filas ya existentes

Lo que cambió en 116 días, releído uno por uno:

| Repo | julio 2026 | **2026-10-26** | Δ |
|------|-----------|----------------|---|
| `hi-godot/godot-ai` | 805 ★, "~41 tools" | **3.2k ★, 46 tools, 120+ ops** | **×4** |
| `limbonaut/limboai` | 2.8k ★ | **3.1k ★** | +11% |
| `heroiclabs/nakama` | 12.8k ★ | **13.6k ★** | +6% |
| `undreamai/LLMUnity` | 1.7k ★ | **1.7k ★** | plano |

**La tendencia, en una línea:** el crecimiento se concentró en **tooling MCP de
editor**, no en NPCs. La tier de NPC con LLM está plana; la tier de agente
conectado al editor se multiplicó por cuatro en el líder.

### 🔴 Retirado del radar con motivo

| Repo | Qué decía esta KB en julio | Qué se verificó el 2026-10-26 |
|------|---------------------------|-------------------------------|
| [SKYHUBDev/Aivill](https://github.com/SKYHUBDev/Aivill) | "Villanos adaptativos que aprenden del comportamiento del jugador. Concepto muy relevante, observar." Listado en *Proyectos a seguir* y en el radar de `intel/trends.md`. | Repo **vivo pero vacío**: `README.md` → 404, ningún archivo de licencia en HEAD, 0 ★. **No hay proyecto que observar.** Se observó durante cuatro meses una carpeta vacía. |
| [leigest519/OpenGame](https://github.com/leigest519/OpenGame) | "Primer framework agentico OSS para generación de juegos web, abr 2026" | Sigue vivo y creció (3.0k ★, Apache-2.0 verificado), pero su **última novedad en README es del 2026-04-21**: seis meses sin anuncios. Se mantiene *con* advertencia de estancamiento. |

### ⛔ Candidatos rechazados (para no re-gastar la búsqueda)

| Candidato | Licencia | Por qué no entra |
|-----------|----------|------------------|
| [balaraj74/openNPC](https://github.com/balaraj74/openNPC) | MIT (1077 B, real) | Se presenta como "framework open source para NPCs autónomos". Primera mano: **5 ★, 4 commits, 1 fork**, se autodescribe "production-ready foundation" pero el comando de clonado de su README sigue diciendo `yourusername`, y Actions no muestra tests. El grant es real; el proyecto no. |
| [lschiweck/LLM-NPC-Agents](https://github.com/lschiweck/LLM-NPC-Agents) | Apache-2.0 (11319 B) | **Proyecto real** (55 commits, backend + cliente Unity, módulos STT/TTS/LLM, cita un paper) y el único candidato Apache-2.0 de NPC con voz en tiempo real. Pero **11 ★** y se autodeclara "Early Release": queda como señal a seguir, no en la ruta recomendada. |
| [chongdashu/unreal-mcp](https://github.com/chongdashu/unreal-mcp) | **ninguna** | Citado en guías 2026 como "MIT, ~70 tools". El README muestra un **badge** `License: MIT`, pero **no hay archivo de licencia en HEAD** (`LICENSE`, `LICENSE.md`, `LICENSE.txt`, `COPYING` → todos 404) y el repo se autodeclara `Status: Experimental`. **Un badge no es un grant.** |
| [Tencent-Hunyuan/Hunyuan-GameCraft-1.0](https://github.com/Tencent-Hunyuan/Hunyuan-GameCraft-1.0) | Tencent community (16386 B), **no OSI** | Aparece en listas como "open source". No es Apache/MIT; trae restricciones de uso. |

### 🧭 Señal de ecosistema de este pass

- **MCP ganó en gaming, y el premio se lo llevó el plugin de engine.** La asimetría
  **4.5k / 51 / 16** dentro del *mismo autor* (Unity / Unreal / backend compartido)
  localiza la adopción con precisión: está en Unity, no en la abstracción. Se reporta
  además un plugin MCP **first-party experimental en UE 5.8**; si Epic lo estabiliza,
  la tier community de Unreal se consolida de golpe.
- **Los world models jugables dejaron de ser "a observar".** Matrix-Game 4.0 corre en
  una **RTX 5080**, y su arquitectura **conserva el engine** en lugar de reemplazarlo:
  por eso es adoptable por etapas. La línea **Genie de DeepMind no es abierta** (reports
  y blog posts, sin código): todo lo jugable abierto es de **Skywork, Microsoft o
  Tencent**, o sea mayoritariamente I+D de APAC.
- **El sentimiento se dio vuelta y eso cambia el pitch.** GDC 2026 (2.300+
  respondentes): **52%** de los devs dice que la gen AI está dañando la industria y
  sólo **7%** ve impacto positivo; uso declarado **36%** global, **30%** en estudio
  contra **58%** en publishing/marketing. El "87% de estudios usa AI agents" que esta
  KB venía repitiendo mide otra cosa y **no se puede usar como apertura**.

### 🔍 Gaps declarados de este pass

- **`huggingface.co` no resuelve DNS** aquí (`getaddrinfo ENOTFOUND`): la licencia de
  los **pesos** de Matrix-Game 4.0 y de los checkpoints de MineWorld **queda sin
  verificar**. En un world model los pesos son la mitad del entregable.
- **`api.github.com/search/*` rechazado** ("sessions are bound to their configured
  repositories"): cero descubrimiento por topic o por estrellas. Todo salió de
  búsqueda web + lectura de páginas de repo, lo que sesga hacia lo indexado en inglés.
- **Oferta OSS de origen LATAM: ausencia medida.** Se buscó en español apuntando a
  estudios brasileños y argentinos. **Cero repos de gaming AI de origen LATAM.** Lo más
  cercano es **Argentum Online** (primer MMORPG open source argentino, liberado 2018,
  **GPL-3.0-or-later, sin AI de NPC**). No es fallo de búsqueda: es la estructura del
  mercado.
- **★ marcadas con `*`** no se releyeron en este pass.

---

# Historial

Todo lo que sigue es el contenido de los passes anteriores, sin modificar.

---

# Agentes trending — Gaming AI

> Tendencias activas en AI gaming. Última actualización: 2026-07-02
> Investigación manual curada — complementa el pipeline automático de top.md

## Tendencias principales en 2026

### 1. NPCs con LLM + Memoria persistente — en producción
La adopción de LLMs para NPCs pasó de prototipos a producción en 2026. El patrón dominante combina:
- **LLM base** (GPT-4o, Claude 3.5, Llama 3.1 local) para generación de diálogo
- **Vector store** (ChromaDB, Qdrant) para memoria episódica del personaje
- **TTS/voz** (ElevenLabs, Kokoro TTS open source) para output de audio
- **Reconocimiento de emoción** del jugador vía análisis de texto o cámara

Impacto medido: NPCs AI aumentaron immersion scores 40% y session times 28% en RPGs.
Segmento NPCs + Digital Humans: 28.6% del mercado total AI gaming en 2026.

### 2. NVIDIA ACE — Digital Humans en tiempo real
NVIDIA ACE (Avatar Cloud Engine): ASR + NLP + TTS + animación facial para NPCs en tiempo real.
Demo "Covert Protocol" (Inworld + ACE, GDC 2025): jugadores interrogan NPCs adaptativos.
SDK disponible para Unreal Engine y Unity. Tendencia: NPCs con voz generada on-the-fly sin latencia perceptible.
NVIDIA también lanzó un fork de Godot con path tracing en GDC 2026 (MIT).

### 3. Generative Agents — de paper a producción
El paper de Stanford "Generative Agents: Interactive Simulacra of Human Behavior" (2023, 21.7k stars) se convirtió en el estándar de arquitectura NPC. Las 3 capas:
- **Memory stream**: log de eventos con timestamp + relevancia + recencia
- **Reflection**: síntesis de recuerdos en insights de alto nivel
- **Planning**: planes horarios basados en reflexiones

Repos derivados activos: Concordia (Google DeepMind, Apache 2.0), implementaciones para RPGs y MMOs.

### 4. AI integrado en engines — Unity 6.2, Godot MCP, Unreal Aura
- **Unity AI** (v6.2, mid-2025): suite en editor sin suscripción separada. Módulos: Assistant (docs/code), Generators (texturas/sprites con difusión), Inference Engine (ML on-device).
- **Godot MCP** ([hi-godot/godot-ai](https://github.com/hi-godot/godot-ai), abr 2026, MIT, 805 stars): 120+ operaciones, 41 tools MCP. Conecta Godot a Claude Code, Cursor, Codex.
- **Unreal Engine — Aura** (Ramen VR, ene 2026): agente con modos editor y coding. Caso Sinn Studio: lanzó *Zombonks* en 5 meses (~mitad del tiempo normal).
- **Roblox AI Studio** (dic 2025): MCP client integrado + OpenGameEval OSS (47 escenarios de benchmark).

### 5. PCG con LLMs, Diffusion y World Models
- **OpenGame** ([leigest519/OpenGame](https://github.com/leigest519/OpenGame), MIT): framework agentico OSS para generar juegos web end-to-end con GameCoder-27B (RL-entrenado para ejecución de código de juego). Abr 2026.
- **Roblox CUBE 3D**: modelo 1.8B params para generación de objetos 3D on-platform desde texto (mar 2025).
- **PANGeA** (paper 2024): Procedural Artificial Narrative using Generative AI para juegos de turnos.
- World models (Genie 2, etc.): modelos que generan entornos de juego completos e interactivos desde texto/imagen.

### 6. Behavior Trees + LLM — el stack ganador para NPCs
El ecosistema Godot consolidó el patrón Behavior Tree + LLM:
- **Beehave** (MIT, 3.2k stars): BTs componibles en el scene tree de Godot. Lógica reactiva estructurada.
- **LimboAI** (MIT, 2.8k stars): BTs + HSMs (Hierarchical State Machines) combinados.
- **godot_rl_agents** (MIT, 900+ stars): RL sobre juegos Godot con SB3, Sample Factory, RLLib, CleanRL.

Patrón emergente: BT como esqueleto de control → LLM como módulo de diálogo/decisión contextual.

### 7. AI QA automatizado — testing con agentes
- Bots RL que juegan el juego para detectar bugs en casos edge-case (especialmente level design)
- **GameStudio Subagents** ([pamirtuna/gamestudio-subagents](https://github.com/pamirtuna/gamestudio-subagents)): equipo de sub-agentes IA (dev, QA, artist, game designer) en terminal
- LLMs + Playwright + CrewAI para testing semántico de flujos de UI
- Roblox OpenGameEval: benchmark que incluye escenarios de testing AI automatizado

### 8. Player analytics con GNNs — churn prediction
- Graph Neural Networks sobre comportamiento + red social del jugador
- 75.83 AUROC con GNN (PyTorch Geometric) vs 62.44 de LightGBM flat-table (benchmark RelBench)
- Predicción de churn 14 días en adelante con señales: amigos que se van, difficulty walls, session length declining
- F2P mobile en LATAM: driver crítico (retención > adquisición en costo)

## Proyectos a seguir

| Proyecto | URL | Señal |
|----------|-----|-------|
| **godot_rl_agents** | [edbeeching/godot_rl_agents](https://github.com/edbeeching/godot_rl_agents) | RL sobre Godot con 4 frameworks, comunidad activa |
| **LLMUnity** | [undreamai/LLMUnity](https://github.com/undreamai/LLMUnity) | LLMs locales en Unity, 1.7k stars, creciendo |
| **npcpy** | [NPC-Worldwide/npcpy](https://github.com/NPC-Worldwide/npcpy) | Python para NPCs multimodales, 1.4k stars |
| **OpenGame** | [leigest519/OpenGame](https://github.com/leigest519/OpenGame) | Primer framework agentico OSS para generación de juegos web |
| **Aivill** | [SKYHUBDev/Aivill](https://github.com/SKYHUBDev/Aivill) | Villanos adaptativos que aprenden del comportamiento del jugador |

## Señal de mercado — adopción

- 87% de estudios usa AI agents en workflows (Google Cloud survey, 615 devs, jun-jul 2025)
- 50%+ de estudios AAA usa AI en alguna capacidad
- Generative AI in Gaming: $1.79B en 2026, CAGR 23.2%
- NPCs y Digital Humans: mayor segmento (28.6% del total AI gaming)

---
*Fuentes: agentmarketcap.ai, solidaitech.com, aivexify.com, wanderfolk.ai, GitHub (verificado 2026-07-02)*
