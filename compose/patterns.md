# 🧩 Patrones de composición — Gaming AI

> Recetas concretas y verificadas. Última actualización: 2026-07-02

---

## Patrón 1: NPC con diálogo LLM en Godot

**Caso de uso**: NPCs que hablan naturalmente, recuerdan al jugador, tienen personalidad

**Stack**:
```
Godot 4 (MIT)
└── LimboAI (MIT) — behavior tree del NPC
    └── BTState con trigger → llama API LLM
        └── Claude / GPT-4o / LLM local via Ollama
            └── pre-conversation.json — personalidad, estilo, conocimiento del personaje
```

**Repos**:
- [limbonaut/limboai](https://github.com/limbonaut/limboai) — behavior trees
- [AkshitIreddy/Interactive-LLM-Powered-NPCs](https://github.com/AkshitIreddy/Interactive-LLM-Powered-NPCs) — referencia de implementación

**Tiempo estimado**: 2-3 semanas para MVP funcional

---

## Patrón 2: Agente de juego entrenado con RL

**Caso de uso**: Oponentes que aprenden, testing automatizado de balance, playtesting

**Stack**:
```
Godot 4 (MIT)
└── godot_rl_agents (MIT) — bridge Python↔Godot
    └── StableBaselines3 o Ray RLLib
        └── Entorno custom de entrenamiento en Godot
            └── Agente exportado → integrado como oponente en el juego
```

**Repos**:
- [edbeeching/godot_rl_agents](https://github.com/edbeeching/godot_rl_agents)

**Tiempo estimado**: 1-2 semanas para primer agente entrenado

---

## Patrón 3: Backend multijugador + AI features

**Caso de uso**: Juego multijugador con matchmaking inteligente, anti-cheat, analytics

**Stack**:
```
Godot 4 (MIT) — cliente
└── Nakama (Apache-2.0) — servidor
    ├── Matchmaking personalizado (TypeScript/Go) con AI scoring
    ├── Server-side events → PostHog para analytics
    └── Open Match — algoritmo de matchmaking enchufable
```

**Repos**:
- [heroiclabs/nakama](https://github.com/heroiclabs/nakama)
- [heroiclabs/nakama-godot](https://github.com/heroiclabs/nakama-godot) — SDK Godot oficial
- [googleforgames/open-match](https://github.com/googleforgames/open-match)

**Tiempo estimado**: 3-4 semanas para stack completo

---

## Patrón 4: AI-assisted game dev (developer tooling)

**Caso de uso**: Estudio que quiere usar AI para accelerar el desarrollo en Godot

**Stack**:
```
godot-ai (MIT) — servidor MCP
└── Claude Code / Codex conectado al editor Godot
    ├── Genera escenas desde descripción natural
    ├── Edita scripts GDScript
    ├── Wirea signals automáticamente
    └── Configura materiales, animaciones, UI
```

**Repos**:
- [hi-godot/godot-ai](https://github.com/hi-godot/godot-ai) — 120+ operaciones MCP

**Tiempo estimado**: Setup en 1 día, ROI inmediato en velocidad de desarrollo

---

## Patrón 5: Mundo procedural con AI

**Caso de uso**: Generación infinita de contenido de juego (niveles, quests, diálogos)

**Stack**:
```
Luanti/Minetest (LGPL-2.1) — base voxel world
    ├── WorldX (MIT) — procedural world gen con AI
    ├── LLM para descripción → parámetros de generación
    └── Texture generation con Stable Diffusion (MIT)
```

**Repos**:
- [minetest/minetest](https://github.com/minetest/minetest)
- [YGYOOO/WorldX](https://github.com/YGYOOO/WorldX)

**Tiempo estimado**: 4-6 semanas para sistema básico funcional

---

## Patrón 6: Game QA Automatizado con RL

**Caso de uso**: Reemplazar QA manual con agentes que exploran el juego buscando bugs

**Stack**:
```
Godot 4 — juego a testear
└── godot_rl_agents — agentes exploradores
    └── Entrenados para maximizar: coverage, crash frequency, exploit discovery
        └── Reporter de bugs automático → GitHub Issues
```

**Tiempo estimado**: 2-3 semanas para framework de QA básico
