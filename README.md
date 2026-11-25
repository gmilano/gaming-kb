---
industry: gaming
region: Global
updated: 2026-11-25
---

# 🎮 Gaming KB

> Knowledge base de la industria **Gaming** para Globant AI Studios.
> NPC agents, procedural content, soporte in-game, anti-cheat, game analytics,
> desde el cycle 13 la capa de **diálogo autorado, voz, moderación de chat y
> localización**, desde el cycle 14 la de **producción de assets** (malla, textura,
> rig, movimiento, música y SFX), desde el cycle 15 la **operativa de cara al
> jugador**, desde el cycle 16 el **plano de datos del jugador** y —desde el cycle
> 17— la **fábrica**: versionado de binarios, CI de build, caché de compilación,
> seguimiento de producción y automatización de QA. **Desde el cycle 19**, el
> **plano de decisión**: *remote config*, *feature flags*, asignación de
> experimentos, recomendación y valor del jugador — la capa que decide qué ve y
> qué paga cada jugador, y la que la regulación de 2026 persigue por su nombre.
>
>
> **Desde el cycle 20**, el **camino del dinero**: tienda y webshop D2C,
> validación de recibo, otorgamiento del derecho (*entitlement*), libro mayor de
> moneda virtual, y la entrega de lo comprado (parcheo diferencial, actualizador,
> tienda propia). Es la capa que **2026 abrió por ley en las cuatro regiones** —
> Japón MSCA, DMA, acuerdo CADE de Brasil, *Epic v. Apple*— y la primera capa de
> esta KB donde el entregable es **transaccional**: si falla, no se ve mal, se
> pierde dinero.
>
>
> **Desde el cycle 21**, el **plano del espectador**: cómo el juego llega a quien
> **no** lo está jugando — captura, encode, transporte, servidor de medios, cloud
> gaming y pixel streaming, espectador, clip automático, relato (*caster*) y
> archivo. Es la **primera capa de esta KB donde el copyleft es el default y no la
> excepción** (12 repos del lado píxel medidos, **cero permisivos**), y la primera
> donde el **gate de licencia y el gate regulatorio son la misma línea de
> pipeline**: la etiqueta de contenido sintético que exige China va embebida en el
> archivo, y el metadato se escribe **al encodear**.
>
> **Desde el cycle 22**, el **plano de la puerta**: quién puede jugar, con qué
> identidad, a qué **edad verificada** y con qué consentimiento. Es la capa que
> **las cuatro regiones legislaron en cuatro puntos distintos del stack** —NA en
> el sistema operativo, EMEA en el wallet del jugador, APAC en un registro
> estatal, LATAM en la plataforma con cuenta parental—, de modo que el *age gate*
> **no es un componente sino cuatro adapters detrás de una interfaz** (Receta
> 61). Y es la primera capa de esta KB donde **una norma nombra un repositorio**:
> el Annex A §A.8 del perfil europeo manda `longfellow-libzk-v1` y declara que
> cualquier otro sistema de prueba de conocimiento cero **no es conformidad**.
> Trae tres regímenes legales en un mismo stack obligatorio —spec **CC-BY-4.0**,
> bibliotecas **Apache-2.0/MPL-2.0**, apps **EUPL-1.2**— y el modo de falla
> cambia: si esta capa falla, no se ve mal, **nadie entra**.

> **Último pass: 2026-11-25 (cycle 22).**

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
  🔴 **Y la sexta, del cycle 14: el repo no es su clausura.** La restricción
  puede vivir **fuera del repo**, en una dependencia que sólo se nombra en la
  prosa del `README.md` — `microsoft/TRELLIS` y `TRELLIS.2` (**27.514 ★
  sumadas**) pasan los tres canales de esta casa como MIT limpias y sus tres
  renderers importan paquetes **no comerciales**. El procedimiento es el
  **canal 4** y la **Receta 36**.
  🔴 **Y la séptima, del cycle 22: el archivo de licencia no es la licencia.**
  Puede ser un **PUNTERO**: `paemuri/brdoc` (**206 B**) dice "ver
  `LICENSES/MPL-2.0.txt`" y además que **un aviso por archivo sobrescribe la
  raíz**; la spec de verificación de edad de la UE (**227 B**) sólo apunta a una
  URL de Creative Commons. Un lector que cuente bytes concluye "licencia corta
  tipo MIT" y se equivoca las dos veces. El procedimiento es el **canal 9** y la
  **Receta 63**. Corolario medido: **la confiabilidad del byte-count es por
  FAMILIA** — Apache-2.0 midió **8 pesos distintos en un solo pass** (9.452 a
  11.606), mientras **GPL-3.0 midió 35.149 B en tres repos independientes**.
- **En la capa de modelos generativos, código y pesos son dos licencias
  distintas, en archivos con nombres distintos** (`LICENSE_weights`,
  `MODEL_LICENSE`, o un addendum dentro del propio `LICENSE`). Medición del
  cycle 14: **3 de 3** proyectos de audio con pesos licenciados aparte los
  tienen en **modo no comercial**.
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
