---
industry: gaming
region: Global
updated: 2026-11-22
---

# 🎮 Gaming KB

> Knowledge base de la industria **Gaming** para Globant AI Studios.
> NPC agents, procedural content, soporte in-game, anti-cheat, game analytics,
> y —desde el cycle 13— la capa de **diálogo autorado, voz, moderación de chat y
> localización**.
>
> **Último pass: 2026-11-22 (cycle 13).**

## Estructura

```
gaming-kb/
├── agents/        # Agentes AI open source de la industria
├── repos/         # Repos MIT/Apache como punto de partida
├── verticals/     # Plataformas verticales customizables con AI
├── intel/         # Mercado, players, tendencias
├── compose/       # Recetas: cómo componer soluciones
└── ingest/        # Scripts de actualización automática
```

## Uso

1. **Nuevo engagement**: leer `intel/market.md` + `repos/foundations.md`
2. **Proponer solución AI**: `agents/top.md` + `compose/patterns.md`
3. **Mantenerse al día**: correr `ingest/update.sh` semanalmente

## Cómo leer esta KB

- **`agents/trending.md` y `repos/trending.md` son APPEND-ONLY.** Cada pass
  agrega una sección fechada arriba y conserva la historia debajo: la historia
  es el dato.
- **Toda licencia de esta KB está leída en bytes del archivo del repo**, no
  tomada del badge de la API. Las reglas de método viven en `agents/top.md` y el
  procedimiento ejecutable en `compose/patterns.md` → **Receta 20**.
  🔴 **Cinco cosas que esta KB aprendió a no confiar: el badge no es el grant,
  el conteo de bytes no es el grant, la rama default no es el repo, *el repo no
  es la organización* (cycle 12), y —desde el cycle 13— **la raíz no es el
  repo**: `meta-llama/PurpleLlama` tiene **14 archivos de licencia y 5 grants**,
  con la raíz **menos** libre que cuatro de sus subcarpetas, que son MIT.
  Y el grant puede estar **en el `README.md`** (`inkle/inky`: 2.758 ★, badge
  `null`, MIT verbatim en la prosa).** Un producto MIT puede tener al
  lado un repo auxiliar sin ningún grant (`TaloDev/hosting`), y un repo con
  **cero** archivos de licencia puede tener el grant **declarado en el README y
  alojado afuera** (`QwenLM/Qwen3`). **`enumeración = 0` es ambiguo, no
  negativo:** el procedimiento para resolverlo es la **Receta 30**.
- **Las oportunidades de `intel/market.md` están organizadas bajo un único
  encabezado `## Opportunities by region`** con una subsección por región
  (North America / EMEA / APAC / LATAM). **Los gaps se escriben como gaps**: un
  cero medido es información, un silencio parece cobertura.
- ⚠️ **"Gaming" es ambiguo en fuentes regulatorias**: las consultas con esa
  palabra devuelven la industria del **juego de azar**. El material de *iGaming*
  de esta KB está etiquetado como carril separado y **no debe mezclarse con
  videojuegos**. Para investigar la industria, el instrumento correcto dice
  **"video game"**.

---
*Red de KBs Globant AI Studios → [globant-kb](../globant-kb/)*
