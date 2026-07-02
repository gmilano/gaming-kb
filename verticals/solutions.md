# Verticales de partida — Gaming AI

> Plataformas verticales open source customizables con AI.
> Modelo: partir de algo funcional y robusto, añadir capa agentica encima.
> Última actualización: 2026-07-02

## Plataformas base recomendadas

| Plataforma | Licencia | URL | Stack | Caso de uso principal |
|------------|----------|-----|-------|-----------------------|
| **Godot Engine** | MIT | [godotengine/godot](https://github.com/godotengine/godot) | C++/GDScript/C# | Engine cliente para juegos 2D/3D. Base ideal para integrar NPCs AI, PCG y sistemas de diálogo. |
| **Open 3D Engine (O3DE)** | Apache 2.0 | [o3de/o3de](https://github.com/o3de/o3de) | C++ | Engine AAA sin royalties. Para juegos AAA/AA con integración AWS Bedrock o Comprehend. |
| **Nakama** | Apache 2.0 | [heroiclabs/nakama](https://github.com/heroiclabs/nakama) | Go (server) + SDKs | Backend de juego: multiplayer, matchmaking, leaderboards, social. Extensible con módulos Go/TS/Lua. |
| **Supabase** | Apache 2.0 | [supabase/supabase](https://github.com/supabase/supabase) | PostgreSQL + APIs | BaaS para juegos asíncronos/persistentes: player profiles, inventarios, leaderboards, UGC, lobbies. |
| **Gymnasium** | MIT | [Farama-Foundation/Gymnasium](https://github.com/Farama-Foundation/Gymnasium) | Python | Definir entornos de juego como entornos RL. Base para entrenar cualquier agente con SB3, CleanRL. |

---

## Cómo customizar con AI

### Godot + AI

**Opción 1 — LLM local (sin API externa)**
```
Godot Engine (MIT)
    ↓ plugin
godot-ai / godot-ai-assistant-hub
    ↓ protocolo MCP / HTTP
Ollama (local) con Llama 3.1 / Gemma 3n
    ↓
NPC con diálogo dinámico, generación de código GDScript in-editor
```
- Repos: [hi-godot/godot-ai](https://github.com/hi-godot/godot-ai), [FlamxGames/godot-ai-assistant-hub](https://github.com/FlamxGames/godot-ai-assistant-hub)
- Caso Globant: estudio indie que quiere NPCs sin costos de API. Deployment offline posible.

**Opción 2 — RAG sobre lore del juego**
```
Godot Engine
    ↓ llamada HTTP a servidor FastAPI
LlamaIndex (MIT) + ChromaDB (Apache 2.0)
    ↓ retrieval de lore, quests, personajes
LLM (Claude / GPT / Llama local)
    ↓
NPC que "conoce" el universo del juego
```
- Referencia: [RAG with Godot — Fully Local Open Source Agent](https://igorcomune.medium.com/rag-with-godot-fully-local-open-source-agent-for-game-development-429266298e79)

---

### Nakama + AI (backend inteligente)

Nakama expone hooks server-side en Go, TypeScript y Lua. Se puede interceptar eventos y llamar a un LLM/modelo:

```
Nakama server (Apache 2.0)
    ↓ hook after_match_create
    ↓ after_authenticate
Función custom (Go/TS/Lua) que llama:
    → Modelo de matchmaking predictivo (sklearn/PyTorch exportado a ONNX)
    → LLM para generar notificaciones personalizadas
    → Clasificador de comportamiento para anti-cheat
    ↓
Resultado: matchmaking mejorado, anti-cheat, engagement personalizado
```

Módulos AI a añadir sobre Nakama:
- **Matchmaking con ML**: predecir partidas equilibradas basándose en historial (PyTorch ONNX → Go)
- **Detección de churn**: score por jugador, trigger de re-engagement automático
- **Anti-cheat conductual**: detectar patrones anómalos en velocidad, puntería, recursos

---

### Supabase para juegos asíncronos/persistentes + AI

Supabase es ideal para juegos sin tiempo real estricto (turnos, idle, RPGs, social):

```
Supabase (PostgreSQL + realtime + auth + storage)
    ↓ Edge Functions (Deno/TS)
    ↓ trigger en eventos de juego
LLM via API (Claude/GPT/Ollama)
    ↓
- Recomendación de siguiente misión basada en perfil del jugador
- Descripción generativa de ítems de inventario
- Soporte in-game vía chatbot con RAG sobre FAQs
```

Ventajas sobre Firebase: SQL nativo (PostgreSQL), pgvector para RAG sin servicio externo, open source self-hosteable.

---

### Unity + ML-Agents (pipeline completo de RL)

```
Unity Editor (engine propietario, gratis hasta $200k revenue)
    ↓ Unity ML-Agents (Apache 2.0)
    ↓ Python API
Stable-Baselines3 (MIT) — algoritmos: PPO, SAC, MA-POCA
    ↓ entrenamiento
Modelo exportado (ONNX)
    ↓ Unity Inference Engine (runtime)
NPC/bot entrenado corriendo en tiempo real
```

Aplicaciones:
- Bots de dificultad adaptativa que aprenden del jugador
- Entidades de testing automatizado (QA agent)
- Pathfinding + decision-making aprendido (más fluido que A*)

---

### O3DE + AWS AI Services (para estudios enterprise)

```
Open 3D Engine (Apache 2.0)
    ↓ AWS Gem (plugin nativo)
AWS Bedrock (Claude / Llama en la nube)
AWS GameLift (multiplayer servers)
AWS Comprehend (análisis de toxicidad en chat)
    ↓
Juego AAA con AI as a Service sin lock-in de engine
```

Perfil de cliente: estudio mid-size que quiere escalar con AWS sin pagar royalties de engine.

---

## Tabla resumen de fit por caso de uso

| Caso de uso | Plataforma base | Capa AI | Esfuerzo |
|-------------|----------------|---------|----------|
| NPC con LLM local | Godot | godot-ai + Ollama | Bajo (días) |
| NPC con memoria y personalidad | Godot / Unity | Generative Agents pattern + LLM API | Medio (semanas) |
| Multiplayer backend inteligente | Nakama | Custom hooks + modelo ONNX | Medio |
| Backend social/persistente + AI | Supabase | Edge Functions + LLM API | Bajo-Medio |
| Bots RL / QA automatizado | Unity + ML-Agents | Stable-Baselines3 + PPO | Medio-Alto |
| AAA con AI cloud | O3DE | AWS Bedrock / GameLift | Alto (meses) |

---
*Fuentes: heroiclabs.com, adilbouchnita.com, godotengine.org, github.com (verificado 2026-07-02)*
