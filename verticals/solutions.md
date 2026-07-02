# 🏭 Verticales de partida — Gaming

> Plataformas completas con licencia abierta. Modelo: fork + AI encima.
> Última actualización: 2026-07-02

## Stack recomendado: Godot + Nakama

La combinación más potente disponible en open source hoy:

```
Godot (MIT) — motor del juego
    + LimboAI / Beehave — NPC behavior trees
    + godot_rl_agents — entrenamiento RL de agentes
    + godot-ai — MCP server para AI-assisted dev
    ↕
Nakama (Apache-2.0) — backend multijugador
    + Lua/TS extensions — lógica server-side
    + Open Match — matchmaking con AI
```

## Plataformas verticales completas

| Plataforma | Licencia | URL | Descripción | Cómo añadir AI |
|------------|----------|-----|-------------|----------------|
| Godot Engine | MIT | [godotengine.org](https://godotengine.org) | Motor 2D/3D completo. Editor, exportador, scripting | LimboAI para NPCs; godot_rl_agents para RL; godot-ai para MCP |
| Nakama | Apache-2.0 | [heroiclabs.com/nakama](https://heroiclabs.com/nakama-opensource) | Backend gaming: auth, social, realtime, storage | Extender con TS/Go: matchmaking AI, anti-cheat, player analytics |
| Open 3D Engine | Apache-2.0 | [o3de.org](https://o3de.org) | Motor 3D AAA con PhysX, raytracing, atom renderer | Gems (plugins) para AI; ideal para proyectos enterprise |
| Colyseus | MIT | [colyseus.io](https://colyseus.io) | Server Node.js multijugador con rooms y state sync | Middleware para AI decisions; agentes server-side |

## Juegos open source para derivar

Proyectos completos MIT/Apache que se pueden fork+AI:

| Juego | Licencia | Stack | Por qué útil |
|-------|----------|-------|--------------|
| [Luanti (Minetest)](https://github.com/minetest/minetest) | LGPL-2.1 | C++/Lua | Voxel world completo. Base ideal para añadir AI de generación de mundos y NPCs | 10.5k |
| [0 A.D.](https://github.com/0ad/0ad) | GPL-2.0/CC-BY-SA | C++ | RTS completo. AI de unidades mejorable con ML | 2.5k |
| [SuperTuxKart](https://github.com/supertuxkart/stk-code) | GPL-3.0 | C++ | Juego de carreras completo. AI de oponentes mejorable | 5k |
