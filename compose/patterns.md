---
industry: gaming
region: Global
updated: 2026-11-25
---

## 🆕 Recetas del pass cycle 23 (2026-11-25) — el plano del dispositivo

### 🧩 Receta 64: **Canal 10 — medir el grant de un repo cuyo host es inalcanzable, por censo de forks**

**Problema.** La pieza fundacional de una capa vive en un host que el entorno no
alcanza (aquí: `gitlab.freedesktop.org`, **curl 000 / `connect_rejected`**). Sin
licencia leída, la pieza no puede entrar al catálogo — y es **el runtime de
referencia de la capa**, así que omitirla vacía el pass.

**Procedimiento ejecutable.**

```bash
# 1. Confirmar que el host está muerto y NO el recurso. Tres hosts de la familia:
for u in https://gitlab.freedesktop.org/ https://monado.freedesktop.org/ \
         https://monado.pages.freedesktop.org/monado/; do
  printf "%-50s " "$u"; curl -s -o /dev/null -w "%{http_code}\n" --max-time 20 "$u"
done
# 000 en los tres => host policy-dead, no 404 de recurso.

# 2. Censar forks en el host que SÍ responde.
gh api -X GET search/repositories -f q="monado in:name" -f sort=stars -f order=desc \
  --jq '.items[] | "\(.stargazers_count)\t\(.full_name)\t\(.license.spdx_id)\t\(.pushed_at[0:10])"'

# 3. Leer el LICENSE de CADA fork en bytes y exigir convergencia.
for r in mateosss/monado shinyquagsire23/monado SimulaVR/monado \
         konsti219/frame-monado alvr-org/Monado-ALVR; do
  printf "%-32s " "$r"
  curl -s "https://raw.githubusercontent.com/$r/main/LICENSE" | wc -c
done
```

**Resultado medido:** `1280` cinco veces, Boost Software License 1.0 las cinco.

**Criterio de aceptación.** **n ≥ 3 forks no coordinados con byte-count idéntico**
⇒ el grant se publica, citando el método. **n = 1** ⇒ no se publica (ese fork pudo
relicenciar). Divergencia entre forks ⇒ **no se publica nada**, se escribe la
divergencia como hallazgo.

**Límite que hay que declarar.** El canal mide **el grant que el fork
distribuye** —que es exactamente lo que importa para integrar— y **no** prueba qué
dice el `HEAD` del upstream hoy. Se escribe así en la ficha.

⇒ 🔵 **Copiar a toda KB cuya capa tenga upstream fuera de GitHub** (freedesktop,
Savannah, Gitea propio, SourceHut).

---

### 🧩 Receta 65: **Distinguir un canal muerto de un recurso muerto, antes de borrar un catálogo**

**Problema.** El procedimiento de ingesta manda `curl -sI` sobre cada URL. En este
entorno eso devuelve **403 para las 46 direcciones del pass**, incluidas las que
acababan de entregar su `LICENSE` byte a byte. Un pass que lea ese 403 como un 404
**descarta el catálogo completo** y escribe "no se encontraron repos".

**Procedimiento: la prueba de los dos canales.**

```bash
r="libsdl-org/SDL"
# Canal mandado (HTML de github.com) -- aquí muerto:
curl -sI -o /dev/null -w "HTML: %{http_code}\n" "https://github.com/$r"
# Canal A: raw => prueba repo + rama + archivo + CONTENIDO
curl -s -o /tmp/l -w "RAW:  %{http_code} " "https://raw.githubusercontent.com/$r/main/LICENSE.txt"; wc -c </tmp/l
# Canal B: índice de GitHub resuelve el slug exacto
gh api -X GET search/repositories -f q="repo:$r" --jq '.total_count'
```

**Salida medida:** `HTML: 403` · `RAW: 200 884` · `1`.

**Regla de decisión.**

| Patrón | Diagnóstico | Acción |
|---|---|---|
| **Todas** las URLs fallan con el **mismo** código | **Canal muerto** | Sustituir el canal. **No tocar el catálogo** |
| Unas pasan y otras dan 404 | **Recurso muerto** | Investigar cada 404: rename, borrado o conjetura propia |
| 403 en un canal + 200 en otro para el mismo repo | **Canal muerto, probado** | Documentar los dos canales en la auditoría |

> 🔴 **Un 403 uniforme es un diagnóstico del canal; un 404 es un diagnóstico del
> recurso. Confundirlos borra catálogos.** La prueba fue poder leer 884 B de
> `SDL/LICENSE.txt` **en el mismo minuto** en que `curl -sI` daba 403 sobre el
> mismo repo. ⇒ **Copiar a todas las KBs.**

---

### 🧩 Receta 66: **Experiencia XR 100 % permisiva, multi-headset, sin lock-in de engine**

**Problema.** Un cliente quiere una experiencia XR que corra en Galaxy XR, Quest y
PC, sin atarse al runtime de un fabricante ni a un engine propietario, y con el
expediente de licencias listo para auditoría.

**Stack, todo con licencia leída en bytes:**

| Rol | Pieza | Licencia **medida** |
|---|---|---|
| Estándar / loader | https://github.com/KhronosGroup/OpenXR-SDK | **Apache-2.0** (11.358 B) |
| Engine | **Godot 4** + https://github.com/GodotVR/godot_openxr_vendors | **MIT** (1.108 B) |
| Entrada / mando / háptica | https://github.com/libsdl-org/SDL | **Zlib** (884 B) |
| Runtime propio (si hay hardware raro) | fork de Monado, vía Receta 64 | **BSL-1.0** (1.280 B) |
| Transporte a standalone | https://github.com/alvr-org/ALVR | **MIT** (1.093 B) |
| **Prueba de conformidad** | https://github.com/KhronosGroup/OpenXR-CTS | **Apache-2.0** (11.358 B) |

**Cableado.**

1. El juego enlaza **sólo el loader de OpenXR**. Ningún `#include` de un SDK de
   fabricante. Esa es la línea que compra la portabilidad.
2. **Godot, no Unity**, por una razón medible y no estética: en Unity la capa de
   entrada (`InputSystem`, **491 B, Unity Companion License**) **no es open
   source** y su grant está condicionado a *"Unity-dependent projects"*. En Godot
   la cadena entera es MIT.
3. **SDL** para mando y háptica en el modo escritorio; dentro de XR, las acciones
   van por los *action sets* de OpenXR.
4. **Correr el CTS como paso de CI**, no como auditoría final. Es Apache-2.0 y
   ejecutable: pasa a ser un *gate* de pipeline.
5. Si el cliente embarca hardware propio ⇒ fork de Monado y driver `xrt_device`.
   **BSL-1.0 exime al código objeto del aviso de atribución** ⇒ menos carga de
   cumplimiento en el binario que MIT o BSD-3.

**Trampas, todas medidas en este pass.**

- 🔴 **No usar WiVRn** si el entregable es cerrado: **GPL-3.0, 35.149 B**.
- 🔴 **No partir de `godot_openxr_for_godot_3.x`**: dice *"(Maintenance mode)"* y
  no tiene push **desde 2022-12-18**. El repo vivo es `godot_openxr_vendors`.
- 🔴 **No proponer `PadForge`** para nada de mandos: **CC-BY-NC-SA-4.0**,
  NonCommercial, por mucho que tenga push de ayer.
- ⚠️ **`spatial containers`**: Khronos publicó dos extensiones el **19-oct-2026**
  (Android XR, Spatial API Level 4+), pero **Unity y Godot ponen la escena entera
  en un único contenedor primario**. Si el diseño necesita varios contenedores
  simultáneos, hoy eso es **OpenXR nativo**, no engine.

---

### 🧩 Receta 67: **Expediente CRA + RED para un producto XR vendido en la UE — con el reloj ya vencido**

**Problema.** El estudio embarca un producto XR en la UE. Dos relojes ya corren y
uno de ellos venció hace meses; el cliente cree que el CRA es "para 2027".

**El reparto de sujetos, que es el núcleo de la receta.**

| Sujeto | Quién es aquí | Obligación | Desde |
|---|---|---|---|
| **Fabricante** | **El estudio** que publica el producto | Reportar vulnerabilidades **activamente explotadas** e incidentes graves vía *Single Reporting Platform* de ENISA | 🔴 **11-sep-2026 — vencido** |
| **Open-source steward** | Khronos, el proyecto Monado, SDL | Art. 24(3) vía Art. 71(2), **sin sanciones** | 11-dic-2027 |

> 🔴 **La obligación no sube por la cadena: aterriza en quien empaqueta.** Que
> Monado y SDL sean *stewards* con fecha 2027 y sin multa **no protege al estudio**:
> el estudio es fabricante desde el día que vende.

**Pasos.**

1. **SBOM del stack XR** con la licencia **leída en bytes**, no tomada del badge:
   loader OpenXR (11.358 B Apache), runtime (1.280 B BSL-1.0), entrada (884 B
   Zlib), transporte (1.093 B MIT ALVR **o** 35.149 B GPL-3.0 WiVRn).
2. **Decidir ALVR vs WiVRn por escrito**, con el byte-count como evidencia. Es la
   única decisión de esta capa que puede obligar a liberar el producto.
3. **Proceso de reporte** contra la plataforma de ENISA, con responsable nombrado.
   Ya es exigible.
4. **Si hay radio en el objeto** (mando o headset inalámbrico): RED +
   **ciberseguridad del Reg. Delegado 2022/30, aplicable desde 1-ago-2025**, con
   las normas **EN 18031** (DOUE 30-ene-2025, **publicadas con restricciones** ⇒
   leer la guía de la Comisión sobre condiciones de uso). Tres partes: red, datos
   personales/privacidad, fraude.
5. **Cargador:** USB-C obligatorio desde **28-dic-2024** y **los headsets/
   auriculares están en alcance**. 🔵 **Los mandos de videojuego NO lo están
   todavía** — la Comisión encargó un estudio sobre mandos y periféricos ⇒ **es un
   alcance que puede cambiar; se audita, no se asume.**
6. **Si el producto se vende también fuera de la UE**, abrir **expediente por
   región**: FCC (NA), **Giteki + PSE** (Japón; y leer la **ordenanza en vigor
   24-jul-2026** sobre autoconfirmación, **cuyo alcance para mandos y headsets las
   fuentes no establecen**), **ANATEL** (Brasil, **Ato 14.448/2017**, validez ~2
   años). **Ninguna acepta el certificado de otra**: KC no vale en Japón, y CE/FCC
   no tienen efecto legal allí.

**Lo que esta receta NO resuelve, y hay que decirlo al cliente.** ⚠️ El reparto
fabricante/steward **tiene un conflicto de fuentes sin resolver**: Red Hat y
Greenbone sostienen que los *stewards* reportan desde sep-2026; OpenSSF y la
Comisión, que no. **La página primaria de la Comisión no resuelve DNS en este
entorno**, así que no se pudo verificar contra el original. La receta adopta la
lectura Comisión/OpenSSF **porque es la más cercana al texto citado del
Art. 71(2)**, y marca el punto como abierto. **Esto afecta sólo a la fecha del
upstream — la del estudio como fabricante no está en disputa.**

---

## 🆕 Recetas del pass cycle 22 (2026-11-25) — el plano de la puerta

### 🧩 Receta 60: **Puerta 100 % permisiva** — age gate real sin un solo componente restringido

**Problema.** Un título tiene que clasificar al jugador por edad en EE.UU. (SB
2420), probar edad en la UE y no retener el dato. Los vendors (k-ID, Yoti) lo
resuelven como servicio; el cliente quiere **control del dato y sin lock-in**.

**Stack, todo Apache-2.0/MIT medido en bytes:**

```
Cliente del juego (Unity/Unreal)
   │  ① el ÚNICO componente a construir: no existe con grant (ver §7 de agents/top.md)
   ▼
Keycloak  (Apache-2.0, 11.358 B)        ← github.com/keycloak/keycloak
   │  ② SPI de authenticator en Java: un nodo nuevo en el flujo de login
   ▼
zk-age-verifier (Apache-2.0, 11.391 B)  ← github.com/pipe23-org/zk-age-verifier
   │  ③ verifica la prueba Longfellow sobre mdoc por W3C Digital Credentials API
   ▼
longfellow-zk (Apache-2.0, 11.386 B)    ← github.com/google/longfellow-zk
       el esquema que el Annex A §A.8 de la UE declara ÚNICO conforme
```

Del lado del jugador, la credencial viaja en **ISO mdoc**, con
`openwallet-foundation-labs/identity-credential` o `davidz25/multipaz` (ambos
Apache-2.0, 11.358 B) como implementación de referencia.

**Cómo se cablea, concretamente:**

1. **Keycloak** como IdP del título. Se escribe un `Authenticator` SPI que, para
   los flujos marcados, inserta un paso `age-proof-required`.
2. Ese paso **no** pide fecha de nacimiento: emite un *presentation request* de
   credencial con un predicado `age_over_18` y delega en `zk-age-verifier`
   como sidecar HTTP.
3. `zk-age-verifier` valida contra `longfellow-zk`. **Se pinea el commit**, como
   hace `longfellow-go`: `61a8a735964d1b22bccf79bf14ef6767249cdf92` (`v0.9-177`).
4. Keycloak recibe **un booleano y una marca de tiempo**, y escribe en el token
   un claim `age_category` — **no** la edad. Esto satisface de un tiro el deber
   de **borrado** de SB 2420 y la contradicción conservar-vs-borrar de la
   **Receta 21** (cycle 13): se conserva la categoría derivada y el reloj.
5. El juego lee `age_category` del token. **Nunca ve un documento.**

🔴 **Lo que esta receta NO resuelve, y hay que decirlo en la propuesta:** el paso
① . No existe un cliente OIDC + presentación de credencial con licencia usable
para Unity o Unreal (medido: `Funtaptic/UnityOIDCAuth` **sin grant**, AccelByte
propietario). Es trabajo de construcción, y es **el entregable con más valor** de
un engagement de esta capa.

---

### 🧩 Receta 61: **Un deber, cuatro arquitecturas** — el gate que no se puede unificar

**El hallazgo que la hace necesaria.** Las cuatro regiones piden "verificá la
edad" y **cada una lo pide en un punto distinto del stack**. Un diseño único
falla en tres de las cuatro.

| Región | Dónde vive la verificación | Pieza que la implementa | Qué recibe el juego |
|---|---|---|---|
| **North America** | En el **sistema operativo / la tienda** | AB 1043 (CA), SB 26-051 (CO), SB 2420 (TX), Utah | Una **señal de categoría** que el OS entrega. El juego **consume**, no verifica |
| **EMEA** | En el **wallet del jugador** | Perfil AV de la UE + `longfellow-libzk-v1` | Una **prueba criptográfica** de un predicado |
| **APAC** | En un **registro estatal** | Real-name system del NPPA (China): integración obligatoria para aprobación | Un **veredicto remoto** y, en la práctica, una ventana horaria |
| **LATAM** | En la **plataforma**, con cuenta parental vinculada | ECA Digital (Lei 15.211/2025), supervisa ANPD | Un **vínculo de cuenta** y, para loot boxes, un interruptor de producto |

**Receta de arquitectura:** una sola **interfaz interna** (`AgeDecision`) con
cuatro *adapters*, y la regla de que **el juego sólo habla con la interfaz**.

```
                       ┌── NA  adapter: lee la señal del OS/tienda (consume)
AgeDecision ───────────┼── EMEA adapter: zk-age-verifier + longfellow-zk (prueba)
 { category, clock,    ├── APAC adapter: cliente del registro estatal (veredicto + ventana)
   source, expires }   └── LATAM adapter: vínculo de cuenta parental + switch de producto
```

🔴 **El campo que casi siempre falta y es el que importa: `source`.** Las cuatro
rutas producen la misma categoría con **fuerza probatoria distinta**. Un puerto
seguro de TX se apoya en "estándares de industria ampliamente adoptados"; una
prueba ZK de la UE es criptográfica; una ventana horaria china es un veredicto de
tercero. **Si el juego no sabe de dónde vino la decisión, no puede defenderla.**

**Interruptor de producto, no de legal (y ya está medido en esta KB):** el cycle
19 encontró que en Brasil **bloquear loot boxes por default exime de verificar
edad**. ⇒ El adapter LATAM debería poder devolver `category=unknown` **y que el
juego siga siendo compliant**, porque la feature está apagada. Es la ruta más
barata de las cuatro, y la única que no necesita identidad.

---

### 🧩 Receta 62: **No usar estimación facial como control primario** — y qué hacer si ya se usa

**El dato que la motiva.** `serengil/deepface` (MIT, 1.077 B) es la opción que
cualquier equipo elige primero. Su base **VGG-Face** tiene un **sesgo positivo
documentado de ±7–15 años en rostros jóvenes** (fuente: el README de
`matefs/adult-age-face-check`, que lo cita como la razón de haberla abandonado).
El sesgo es **hacia arriba**: estima de más ⇒ **deja pasar al menor**. Es la
dirección exactamente equivocada para un control de protección infantil.

**La receta:**

1. **Estimación facial = señal de *triage*, nunca veredicto.** Sirve para decidir
   *a quién pedirle* una prueba fuerte, no para conceder acceso.
2. **Ordenar las señales por costo para el jugador**, de menos a más:
   | Orden | Señal | Costo para el jugador | Fuerza |
   |---|---|---|---|
   | 1 | **Antigüedad/actividad de la cuenta** | Cero | Media — y es el precedente de Xbox |
   | 2 | **Señal del OS/tienda** (AB 1043, SB 2420) | Cero: ya la dio una vez | Alta en NA, con puerto seguro |
   | 3 | **Prueba ZK desde wallet** | Baja, una vez | Alta y sin entrega de datos |
   | 4 | **Estimación facial** | Media (cámara) | 🔴 **Sesgada en menores** |
   | 5 | **Documento / tarjeta de crédito** | Alta; excluye | Alta, pero ver abajo |
3. **Y la 5 tiene un veredicto regulatorio reciente, no una opinión:** eSafety
   (Australia) consideró que pedir **una tarjeta de crédito australiana** como
   método único **difícilmente cumpla el requisito de "fairness"**. ⇒ Un método
   único, por fuerte que sea, puede ser no conforme por **excluyente**.
4. Si ya hay DeepFace en producción: **medir el sesgo sobre la población propia**
   antes de tocar el umbral, y mover el umbral **hacia arriba** (pedir prueba
   fuerte a más gente), no hacia abajo.

---

### 🧩 Receta 63: **Resolver una licencia-PUNTERO** (canal 9 del método de esta casa)

**Cuándo se dispara.** El archivo de licencia de la raíz mide **menos de ~500
bytes**. Eso ya no significa "MIT o similar": puede ser un **redirect**.

**Procedimiento, en orden:**

1. **Leer el archivo completo**, no su tamaño. Si nombra otra ruta o una URL,
   **no es el grant**: es un puntero.
   - `paemuri/brdoc` → 206 B → `"See LICENSES/MPL-2.0.txt"`.
   - `eu-digital-identity-wallet/av-doc-technical-specification` → 227 B → URL a
     `creativecommons.org/licenses/by/4.0/` (**CC-BY-4.0**, licencia de
     documento, no de software).
2. **Buscar la cláusula de precedencia por archivo.** `brdoc` declara: *"If a
   file contains its own license notice, that notice applies to that file
   instead."* ⇒ **el aviso por archivo le gana a la raíz**. Sin leer esa frase,
   un censo de licencias de ese repo es incorrecto por construcción.
3. **Seguir el puntero y medir el destino** (`LICENSES/MPL-2.0.txt`), y recién
   entonces registrar la licencia.
4. **Distinguir grant de software de licencia de documento.** Una spec CC-BY se
   puede implementar libremente, pero **CC-BY no otorga licencia de patentes**.
   En este stack la protección de patentes **la da Apache-2.0 §3 de la
   biblioteca** (`longfellow-zk`), **no la norma**. Para un cliente que pregunta
   "¿esto me cubre?", esa distinción es la respuesta.

**Resultado del canal 9 en este pass:** 2 punteros detectados de 38 direcciones.
Las dos veces, un lector que contara bytes habría concluído "licencia permisiva
corta" y **las dos veces se habría equivocado**.

## 🆕 Recetas del pass cycle 21 (2026-11-25) — el plano del espectador

Las cuatro recetas usan **sólo repos verificados en este pass**: existencia y
**rama default real** por `git ls-remote --symref`, licencia y byte-count leídos
con `curl` sobre `raw.githubusercontent.com` **en esa rama**. Donde una pieza no
existe, se dice que no existe.

🔴 **Las cuatro comparten una premisa que esta KB no había tenido antes:** en esta
capa **no hay stack permisivo de captura**. El lado píxel es copyleft sin
excepción (12 repos medidos, 0 permisivos). Por eso la Receta 56 es el
prerrequisito de las otras tres: define **cómo se toca el copyleft** sin que
alcance al producto del cliente.

---

### Receta 56 — Separación por proceso: usar OBS/FFmpeg/Sunshine sin heredar su copyleft

**Cuándo:** siempre, en cuanto el proyecto capture o encodee video. Es el paso 0
de las Recetas 57-59 y del plano del espectador entero.

**El problema concreto:** el cliente quiere un producto cerrado y la única capa de
captura que existe es GPL. Si se enlaza `moonlight-common-c` (GPL-3.0) o se
escribe un plugin de OBS (GPL-2.0, como `obs-backgroundremoval`), **el producto
hereda**. Si se invoca el binario, no.

**La línea que decide, y no es opinión:** la obligación viaja por **enlace y obra
derivada**, no por **ejecución**. Entonces:

| ❌ Lo que contagia | ✅ Lo que no |
|---|---|
| Enlazar `moonlight-common-c` (GPL-3.0) en el cliente | Hablar el **protocolo** contra `Sunshine` desde un binario propio |
| Escribir un plugin dentro de OBS (hereda GPL-2.0) | Manejar OBS desde afuera por [`obsproject/obs-websocket`](https://github.com/obsproject/obs-websocket) (GPL-2.0, 4.400 ★) |
| Enlazar `libav*` en el ejecutable propietario | Invocar el **ejecutable** `ffmpeg` y leer su salida |

**Piezas y licencias medidas:**

```
PRODUCTO DEL CLIENTE (cerrado)
   │  (socket / CLI / cola — NUNCA enlace)
   ├─► obs-websocket (GPL-2.0) ──► OBS Studio (GPL-2.0, 80.061★, 2026-11-23)
   ├─► ejecutable `ffmpeg`  (LGPL-2.1+ por default — ver Receta 57)
   └─► protocolo Moonlight ──► Sunshine (GPL-3.0, 35.149 B) | Wolf (MIT, 2.322★)
TRANSPORTE Y SERVIDOR — todo permisivo, acá sí se enlaza libremente
   ├─ pion/webrtc (MIT, 1.093 B, 16.922★)
   ├─ livekit/livekit (Apache-2.0, 11.358 B, 22.381★)
   ├─ ossrs/srs (MIT, 1.086 B, 29.454★)   [rama default `develop`]
   └─ coturn/coturn (BSD-3, 1.622 B, 14.590★)   ← obligatorio para NAT
```

**Pasos:**

1. Dibujar la frontera: todo lo que toca píxeles corre **en su propio proceso o
   contenedor**, con su licencia declarada en el aviso del producto.
2. El producto del cliente habla con esos procesos por **socket, CLI o cola**.
3. Si hay que tocar píxeles **desde código propio**, usar
   [`selkies-project/selkies`](https://github.com/selkies-project/selkies)
   (**MPL-2.0**, 16.725 B, 2.637 ★): copyleft **por archivo**, así que los
   archivos nuevos quedan propietarios. Es **la única puerta permisiva-compatible
   del lado píxel** medida en este pass.
4. Si hay que **modificar** el host de streaming, la base correcta es
   [`games-on-whales/wolf`](https://github.com/games-on-whales/wolf) (**MIT**,
   1.113 B, rama **`beta`**), **no** Sunshine (GPL-3.0).
5. Dejar fuera
   [`AirenSoft/OvenMediaEngine`](https://github.com/AirenSoft/OvenMediaEngine)
   (**AGPL-3.0**, 34.453 B): es servidor y el AGPL se dispara **por uso en red**.
   Sustitutos con más ★: `srs` (MIT), `mediamtx` (MIT), `livekit` (Apache-2.0).

**Entregable:** diagrama de procesos con la licencia de cada uno + el aviso de
terceros del producto + la justificación escrita de por qué cada frontera es
ejecución y no enlace.

---

### Receta 57 — Gate de licencia **por flag de compilación**: el control que ningún gate anterior de esta KB tiene

**Cuándo:** el proyecto embarca FFmpeg (propio, de la distro, o dentro de una
imagen de Docker). Es decir: casi siempre.

**El problema:** las Recetas 10, 14, 17, 20, 21 y 46 auditan **repos y árboles**.
Para FFmpeg eso **da la respuesta equivocada**, porque la licencia la decide
`./configure`:

| Flag | Licencia efectiva del binario |
|---|---|
| *(ninguno)* | **LGPL-2.1-or-later** |
| `--enable-gpl` | **GPL-2.0-or-later** |
| `--enable-version3` | **(L)GPL-3.0** |
| `--enable-nonfree` | 🔴 **NO REDISTRIBUIBLE** |

**Pasos:**

1. **No auditar el repo: auditar el binario.** Sobre el que realmente se embarca:
   ```
   ffmpeg -hide_banner -buildconf | tr ' ' '\n' | grep -E 'enable-(gpl|version3|nonfree)'
   ```
2. **Regla de corte, dura:** si aparece `--enable-nonfree`, **el build se
   detiene**. No es una advertencia de licencia: ese binario **no se puede
   entregar** (texto literal del `LICENSE.md` de FFmpeg). El caso que lo pisa en
   esta capa es encodear audio **AAC con FDK**.
3. Si aparece `--enable-gpl` y el producto es cerrado, hay dos salidas:
   recompilar sin el flag (se pierden filtros GPL — la lista exacta está en el
   `LICENSE.md`), o consumir el binario **por proceso** (Receta 56).
4. **Auditar también el filtro invocado, no sólo el flag:**
   `libavfilter/vf_lensfun.c` está documentado como **GPL-3 o posterior** dentro
   de una lista que por lo demás es GPL-2+. Un solo filtro mueve la versión.
5. Pinchar el resultado en CI: el `buildconf` del binario se compara contra una
   lista blanca y el pipeline falla si cambia. **Porque quien cambia los flags no
   es el cliente: es el empaquetador de la imagen base.**
6. Extender el mismo gate a `GStreamer`: su grant se parte **por conjunto de
   plugins** (`good`/`bad`/`ugly`) y `ugly` trae elementos GPL — el núcleo es
   **LGPL-2.1** (`LICENSE`, 26.431 B).

**Entregable:** el `buildconf` de cada binario de medios embarcado, con su
licencia efectiva derivada, y el gate de CI que lo congela.

---

### Receta 58 — Procedencia embebida al encodear: **un solo paso que cumple China y el Art. 50(2) de la UE**

**Cuándo:** el producto publica al público **cualquier** contenido generado por AI
—casteo sintético, overlay, clip automático, escena generada— y opera en EMEA o
APAC. Las dos obligaciones caen en el mismo punto del pipeline, así que es **una
receta, no dos**.

**Por qué funciona una sola vez:** China exige etiqueta **visible + metadato
embebido** (norma obligatoria **GB 45438-2025**, en vigor desde el 1-sep-2025,
**con enforcement activo**); la UE exige marcado legible por máquina del contenido
generado (**Art. 50, en vigor 2-ago-2026; marcado del 50(2) vence el
2-dic-2026**). **El metadato se escribe donde se escribe el contenedor: en el
encoder.**

**Piezas (todas ya verificadas en esta KB):**

```
GENERACIÓN            → marca de origen por activo (voz AI / overlay AI / clip AI)
   │
ENCODE  (el punto de control, y es el lado copyleft — ver Receta 56)
   ├─ ejecutable `ffmpeg`  → escribe metadato de procedencia en el contenedor
   └─ C2PA (MIT *o* Apache-2.0, ya en esta KB) → firma la procedencia
   │
OVERLAY VISIBLE  → obs-browser (GPL-2.0) o composición propia = la etiqueta visible
   │
TRANSPORTE (permisivo) → srs (MIT) | livekit (Apache-2.0) | owncast (MIT)
   │
ARCHIVO → el VOD conserva el metadato ⇒ auditable después del directo
```

**Pasos:**

1. **Inventario primero** (es lo único que se cierra en 7 días, y el Art. 50(2)
   vence en 7): qué superficies del catálogo emiten contenido generado al público.
2. Marcar **por activo** en el momento de generarlo, no al final: la voz
   sintética, el overlay y el clip llevan cada uno su marca.
3. En el encode, escribir el metadato **y** firmarlo con C2PA. El visible va por
   overlay.
4. **Separar el segmento patrocinado del de comentario** con marca distinta: en
   NA la excepción de comentario **no cubre** lo patrocinado (ver
   `intel/market.md`, NA).
5. Donde no se llega a tiempo, **apagar por defecto** la superficie generativa
   (patrón del "interruptor de oferta", Receta 50). Un flag apagado es cumplimiento
   y es más barato que un sprint.
6. Para publicar en tienda en China: verificar la **capacidad de etiquetado antes
   de listar** — es un **bloqueante de lanzamiento**, no un riesgo de multa.

⚠️ **Límite declarado:** no encontré provisión específica de videojuegos o
live-streaming dentro de las Medidas chinas más allá de la verificación de escenas
virtuales generadas; las fuentes son compliance comercial con fechas inconsistentes.
**El texto del CAC y GB 45438-2025 se leen en original** antes de comprometer
cumplimiento contractual.

**Entregable:** inventario fechado + pipeline de procedencia + plan con fechas
para lo que no entra. **No** un certificado de cumplimiento.

---

### Receta 59 — Clipper y co-caster AI: la tier que **hay que construir** porque no existe para comprar

**Cuándo:** cliente de esports, publisher con programa de creadores, o UA basada
en clips. Hoy evalúa Medal.tv / Allstar / Overwolf / Opus Clip, **todos SaaS
propietario**.

**Por qué se construye:** medido en este pass, la tier open source de highlights y
casteo **no tiene nada adoptable**: 6 repos, **4 con 0 ★**, **3 sin archivo de
licencia** (4 nombres probados cada uno), el líder
([`showlab/livecc`](https://github.com/showlab/livecc), 485 ★) **sin licencia y 13
meses dormido**, y el único MIT
([`zicojiao/worldcupvoice`](https://github.com/zicojiao/worldcupvoice), 111 ★) atado
al SDK **propietario** de Agora. Pero **las piezas con que se construye sí son
permisivas**.

**La decisión de arquitectura que define la receta:** no detectar el momento
mirando **píxeles**. Un clipper que mira píxeles adivina; uno que lee el **replay**
sabe. Esa es la ventaja que los SaaS propietarios **no** tienen en títulos con
replay abierto.

```
ENTRADA
 ├─ replay / demo del título  ──► evento ESTRUCTURADO  (la señal buena)
 │    ├─ LaihoE/demoparser (MIT, 746★, CS2, bindings Python)
 │    ├─ markus-wa/demoinfocs-golang (MIT, 1.077★, CS)
 │    ├─ skadistats/clarity (BSD-3, 775★, Dota 2)
 │    ├─ pnxenopoulos/awpy (MIT, 640★, métricas ya calculadas)
 │    └─ odota/core (MIT, 1.640★, la plataforma de dato completa)
 ├─ audio del casteo ──► ggml-org/whisper.cpp (MIT, 55.496★) → timestamp + texto
 └─ video ──► streamlink (BSD-2, 11.998★) | yt-dlp (Unlicense, 210.483★)
FUSIÓN  → ranking de momentos: evento estructurado + pico de audio + palabra clave
CORTE   → ejecutable `ffmpeg` (Receta 57: auditar los flags) ⇒ corte sin recodificar
CO-CASTER (opcional) → LLM sobre el evento estructurado → TTS
          → livekit (Apache-2.0, 22.381★): el agente es PARTICIPANTE de la sala
SALIDA  → Receta 58 (procedencia firmada) → owncast (MIT) | srs (MIT)
```

**Pasos:**

1. Parser de replay del título → stream de eventos con timestamp. **Si el título
   no tiene replay abierto, esta receta pierde su ventaja** y conviene decirlo
   antes de venderla.
2. `whisper.cpp` sobre el audio del caster: el pico de emoción y la palabra clave
   son la segunda señal, independiente de la primera.
3. Ranking por fusión de las dos señales. Recién acá entra un modelo, y entra
   sobre **features**, no sobre píxeles: es barato y es explicable ante el cliente.
4. **La voz, con su registro de consentimiento.** Voz sintética genérica para
   comentario; para clonar una voz real hace falta consentimiento documentado, y
   el **segmento patrocinado va con marca distinta** porque queda afuera de la
   excepción de comentario en NA (ver `intel/market.md`).
5. Corte con `ffmpeg` por proceso (Receta 56) y **sin recodificar** donde el corte
   caiga en keyframe: es lo que hace viable el costo por clip.
6. Salida por Receta 58, **siempre**: la procedencia no se puede retrofitear a un
   catálogo de clips ya publicado.

**Dónde estrenarlo:** LATAM (pt-BR / es). Es el único lugar con **48,8 % de
aceptación de AI por el jugador contra ~85 % de rechazo global** y, hoy, **sin
obligación de etiquetado que condicione el diseño** — con la procedencia firmada
igual desde el día uno.

🔴 **Lo que NO se debe usar en esta receta, y por qué:**
[`Aseiel/VideoHighlighter`](https://github.com/Aseiel/VideoHighlighter) (507 ★) es
**AGPL-3.0**: ofrecerlo como servicio obliga a liberar el servicio entero. Sirve
como **referencia de diseño** de scoring multi-señal, **no** como dependencia.
Y los 3 repos de casteo **sin archivo de licencia** no son "de licencia
desconocida": sin grant son **todos los derechos reservados**, es decir código
mirable y no usable.


## 🆕 Recetas del pass cycle 20 (2026-11-25) — el camino del dinero

Las cuatro recetas de abajo usan **sólo repos verificados en este pass** (existencia
por `git ls-remote --symref`, licencia y byte-count leídos por
`raw.githubusercontent.com` sobre la rama default real). Donde una pieza no existe,
se dice que no existe.

---

### Receta 52 — Webshop D2C para un juego móvil, stack 100 % permisivo

**Cuándo:** el cliente quiere cobrar fuera de la tienda (habilitado en EE.UU., UE,
Japón y Brasil — ver `intel/market.md`) y hoy evalúa Appcharge / Xsolla / Tebex,
todos SaaS propietario.

**Piezas, todas permisivas:**

| Rol | Repo | Licencia (bytes) |
|---|---|---|
| Escaparate: catálogo, bundles, precios por región, promos | [`medusajs/medusa`](https://github.com/medusajs/medusa) (rama **`develop`**) | MIT (1.402 B) |
| Enrutado de pago entre PSPs por región | [`juspay/hyperswitch`](https://github.com/juspay/hyperswitch) | Apache-2.0 (11.358 B) |
| Otorgar el derecho al jugador | [`heroiclabs/nakama`](https://github.com/heroiclabs/nakama) | Apache-2.0 (11.358 B) |
| Libro mayor de moneda virtual | [`tigerbeetle/tigerbeetle`](https://github.com/tigerbeetle/tigerbeetle) | Apache-2.0 (10.174 B) |

**El cableado, y el punto exacto donde se enchufa:**

```
jugador → Medusa (catálogo + carrito)
            │
            ├─ pago → Hyperswitch → PSP por región (reintento + fallback)
            │                           │
            │                      webhook de PSP
            │                           ▼
            │   Nakama: proveedor IAP REGISTRADO POR EL RUNTIME
            │   server/iap_provider_registry.go
            │     RuntimeIAPProviderNotificationFunction  ← el webhook entra acá
            │     RuntimeIAPProviderPurchaseFunction      ← valida y otorga
            │                           ▼
            └────────────────── TigerBeetle: asiento de doble entrada
                                (idempotente por transaction id)
```

🔴 **Por qué esta receta es viable y no era hace un año:** Nakama incorporó en
2026 el **registro de proveedores IAP enchufables**
(`server/iap_provider_registry.go`, cabecera `Copyright 2026 The Nakama Authors`),
con `RuntimeIAPProviderPurchaseFunction`, `…SubscriptionFunction` y
`…NotificationFunction`. Un webshop propio **no es una tienda conocida**, así que
antes había que parchear el servidor; ahora **se registra como proveedor** desde el
runtime. Restricción medida del nombre del proveedor:
`^[a-zA-Z0-9_-]{1,128}$` (regex `iapProviderNameRegex`) — sin puntos ni espacios.

⚠️ **Dos cosas que hay que construir** (no las trae ninguna pieza): el **mapeo
SKU de Medusa → ítem de Nakama**, y la **idempotencia de extremo a extremo**. El
webhook del PSP **se reintenta**: si el asiento en TigerBeetle no está clavado al
id de transacción del PSP, se entrega dos veces. Es el riesgo nº1 de la receta y
se prueba con un test que reenvía el mismo webhook 3 veces.

---

### Receta 53 — Validar recibos de las tiendas que la ley acaba de crear

**Cuándo:** el cliente publica en un marketplace alternativo (UE por DMA, Japón por
MSCA, Brasil por el acuerdo CADE) y necesita validar compras de una tienda para la
que **no existe SDK oficial**.

**Lo que Nakama ya trae nativo, verificado leyendo `iap/iap.go`:**
`ValidateAppleJwsSignature`, `ValidateLegacyReceiptApple`,
`ValidateLegacyReceiptAppleWithUrl`, `ValidateReceiptGoogle`,
`ValidateSubscriptionReceiptGoogle`, `ValidateReceiptHuawei`,
`ValidateReceiptFacebookInstant`, más `iap/iap_samsung.go`.

**Lo que hay que escribir, y con qué base:**

| Si el backend es… | Base de validación | Licencia (bytes) |
|---|---|---|
| Go (o Nakama como runtime) | [`awa/go-iap`](https://github.com/awa/go-iap) — Apple, Google, Amazon, Huawei | MIT (1.082 B) |
| Servicio aparte, auto-hospedado | [`imariman/iapstack`](https://github.com/imariman/iapstack) — verificación **+ entitlement**, Go + PostgreSQL, modelo neutral de tienda | Apache-2.0 (11.357 B) |
| PHP | [`aporat/store-receipt-validator`](https://github.com/aporat/store-receipt-validator) | Apache-2.0 (11.325 B) |
| Laravel (con ciclo de vida de suscripción) | [`imdhemy/laravel-in-app-purchases`](https://github.com/imdhemy/laravel-in-app-purchases) (rama **`1.x`**) | MIT (1.067 B) |

**Patrón:** un validador por tienda, todos detrás de la **misma interfaz de
proveedor de Nakama**, de modo que agregar una tienda sea agregar un proveedor
registrado y no tocar el servidor.

⚠️ **`imariman/iapstack` se declara en desarrollo temprano (4 tags).** Para un
entregable de cliente va como **referencia de modelo de datos** —su separación
transacción/entitlement neutral de tienda es buena— no como dependencia de
producción. `awa/go-iap` tiene **192 tags** y es la base conservadora.

---

### Receta 54 — Autodistribución: operar tienda propia y entregar parches

**Cuándo:** post-DMA / MSCA / CADE, el cliente quiere distribuir fuera de la tienda
de plataforma (en Brasil, además, con **Notarization** de Apple, y en Android con
**desarrollador verificado obligatorio desde el 30-sep-2026**).

| Rol | Repo | Licencia (bytes) |
|---|---|---|
| Repositorio de apps propio (construir, firmar, publicar) | [`f-droid/fdroidserver`](https://github.com/f-droid/fdroidserver) | 🔴 **AGPL-3.0** (34.520 B) |
| Parcheo diferencial del build | [`itchio/butler`](https://github.com/itchio/butler) | MIT (1.104 B) |
| Protocolo de diff/patch subyacente | [`itchio/wharf`](https://github.com/itchio/wharf) | MIT (1.104 B) |
| Auto-updater del cliente de escritorio | [`velopack/velopack`](https://github.com/velopack/velopack) (rama **`develop`**) | MIT (1.091 B) |

🔴 **La frontera de licencia, que es lo que decide si esta receta se puede
vender:** `fdroidserver` es AGPL-3.0, pero es **infraestructura de publicación del
estudio**: corre en los servidores del cliente, **no se distribuye a los jugadores
y no se linkea en el juego**. El juego publicado a través de un repo F-Droid **no
se vuelve AGPL**. Lo que queda bajo AGPL es **el servidor de repositorio y las
modificaciones que se le hagan** — y si el cliente lo expone como servicio, debe
ofrecer su fuente. **Esa es la conversación con legales, y se tiene antes de
escribir código, no después.**

⚠️ **`itchio/wharf` tiene 0 tags** (butler tiene 347): el protocolo no se releasea.
**Pinear por SHA**, no por tag.

⚠️ **Lo que esta receta NO resuelve:** iOS. `SideStore/SideStore` (AGPL-3.0) y
`ImranR98/Obtainium` (GPL-3.0) son **clientes de usuario final**, no herramientas
de publicación, y sirven de referencia —no de base— para un entregable. **No existe
equivalente open source de `fdroidserver` para iOS**, y los *alternative
marketplaces* de la DMA y de Brasil son justamente de iOS. **Es el gap más caro de
este pass.**

---

### Receta 55 — Checkout agéntico de bienes in-game (exploratoria, con su riesgo escrito)

**Cuándo:** el cliente pregunta por "comercio agéntico" — que un agente AI compre
ítems o monedas en nombre del jugador.

| Capa | Spec / repo | Licencia (bytes) |
|---|---|---|
| Checkout | [`agentic-commerce-protocol/agentic-commerce-protocol`](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol) (Stripe + OpenAI) | Apache-2.0 (10.947 B) |
| Autorización del pago | [`google-agentic-commerce/AP2`](https://github.com/google-agentic-commerce/AP2) (donado a FIDO) | Apache-2.0 (11.359 B) |
| Movimiento del pago | [`x402-foundation/x402`](https://github.com/x402-foundation/x402) ⚠️ **no** `coinbase/x402` | Apache-2.0 (11.324 B) |
| Límites de gasto y aprobaciones del agente + MCP | [`devaccuracy/ledgerforge`](https://github.com/devaccuracy/ledgerforge) | Apache-2.0 (10.749 B) |

**Composición:** AP2 autoriza → x402 mueve → ACP cierra el checkout → LedgerForge
impone **límites de gasto del lado del servidor** y expone un **servidor MCP** para
que el agente consulte saldo y pida aprobación. El derecho comprado se otorga por
la misma vía de la Receta 52 (proveedor IAP registrado en Nakama).

🔴 **Riesgo declarado, y es grande:** las tres especificaciones son **capa de
protocolo, no producto**. ACP tiene **0 tags** y se versiona **por carpeta**
(`2026-04-17`) ⇒ **no se puede pinear como dependencia**; AP2 tiene 2 tags;
`ledgerforge` tiene 2 tags. **Ninguno publica SDK de Unity ni de Unreal.** Esta
receta se propone como **prueba de concepto con fecha de revisión**, no como
arquitectura de producción, y el cliente tiene que oír esa frase.

⚠️ **Trampa de recomendación (T75):** casi toda la prensa linkea `coinbase/x402`,
que **su propio README declara "development fork"** desde que el protocolo pasó a
la Linux Foundation. No está archivado, conserva nombre, licencia y byte-count
idéntico (11.324 B) y tiene 53 tags contra 1.538 del canónico. **Usar
`x402-foundation/x402`.**

---

### 🔴 Dos reglas de esta capa que valen para todas las recetas de arriba

1. **La comisión es configuración, no constante.** Al 2026-11-25 el porcentaje en
   EE.UU. está ante la Corte Suprema y en Brasil las fuentes se contradicen
   (10 %/21 % vs 25 %/10 % + 5 %). Tabla de comisiones **por región y con fecha de
   vigencia**, y capacidad de **reconciliar hacia atrás**.
2. **La idempotencia es el requisito, no una optimización.** Todo este camino está
   hecho de webhooks que se reintentan. El asiento contable se clava al id de
   transacción del proveedor y se prueba reenviando el mismo evento. Es la
   diferencia entre un ledger y una pérdida.

# Patrones de composición — Gaming AI

> Recetas concretas: repos reales, licencias verificadas, cómo se cablean.
> Última actualización: **2026-11-22 (cycle 19)** (passes previos: 2026-11-22
> cycles 18/17/16/15/14/13/12/11/10, 2026-11-21 cycles 9/8/7/6/5, 2026-11-20, 2026-10-26, 2026-07-02).
>
> 🆕 **Las del cycle 19 son la 48** (plano de decisión 100 % permisivo: flags,
> experimentos, recomendación y LTV sin un componente restringido), **la 49**
> (*gate de carve-out* — canal 7: medir qué **proporción** del repo no es libre y
> si crece), **la 50** (el **interruptor de oferta**: un flag, cuatro jurisdicciones
> que piden cosas incompatibles, apagado por defecto) y **la 51** (salir de Unleash
> sin heredar la AGPL).
>
> 🆕 **Las del cycle 18 son la 46** (auditoría de licencias **por árbol completo** —
> el canal 6 reemplaza el paso de enumeración de las Recetas 10/14/17/20, que probaba
> rutas y por construcción no podía saber si faltaban) **y la 47** (contratar un world
> model separando **el modelo** del **banco de pruebas**: en Matrix-Game el modelo es
> MIT/Apache y su benchmark contiene un componente **CC BY-NC 4.0**).
>
> 🆕 **Las del cycle 16 son la 41** (plano de datos del jugador sin una sola
> licencia restringida — y el motivo por el que se vende antes que las demás capas)
> **y la 42** (partición de telemetría por jurisdicción y banda etaria: la mitad de
> almacenamiento que le faltaba a la Receta 33).
>
> ⚠️ **Deuda conocida de esta página:** la *Tabla resumen* del final llega sólo
> hasta la receta 28. Las recetas 29-42 existen como secciones completas y **no
> están en esa tabla**. No se backfilleó en este pass; queda anotado para que nadie
> lea la tabla como un índice completo.
>
> 🆕 **Las del cycle 15 son la 39** (equivalencia voz↔texto como un solo
> componente: cumple el CVAA, habilita la accesibilidad y resuelve la moderación
> de voz, que no tiene oferta open source) **y la 40** (gate de sucesión: la
> licencia del reemplazo se verifica ANTES de aceptar la migración de un repo
> archivado).
>
> 🆕 **Las del cycle 14 son la 36** (auditoría de **clausura** de licencias: el
> gate que corre ANTES de los otros, porque la restricción que detecta no vive
> dentro del repo — dos repos de 27.514 ★ sumadas pasan los tres canales de esta
> casa y no son comerciales), la **37** (fábrica de assets 100% permisiva, con
> **procedencia por artefacto** — el manifiesto que sirve a la vez para el
> etiquetado chino, el Art. 50 europeo y la due diligence de un publisher) y la
> **38** (elegir generador 3D **por jurisdicción de publicación**, con la causa
> medida en cada ⛔).
>
> 🆕 **Las del cycle 13 son la 33** (el dato de edad como parámetro de runtime:
> un solo campo y tres jurisdicciones incompatibles — Texas obliga a borrar lo
> que Connecticut obliga a conservar), la **34** (capa de moderación de chat y
> NPC 100% MIT/Apache, que hay que **construir** porque está medido que no existe
> para comprar) y la **35** (diálogo → voz → idioma con la licencia decidida
> **antes** del motor: con Yarn Spinner, el port que elegís decide si tu licencia
> es MIT o YSPL).
>
> 🆕 **Las del cycle 12 son la 29** (NPC on-device: la arquitectura que *apaga*
> la obligación de topología de datos en vez de documentarla), la **30** (triage
> cuando la enumeración devuelve **cero** — el resultado ambiguo que la Receta 17
> no cubría), la **31** (stack indie 100% MIT con Talo, y el repo del propio
> proyecto que hay que dejar afuera) y la **32** (gate por **tipo de entregable**:
> el eje que PolyForm Shield introduce y que ningún gate anterior detecta).
>
> **Dieciocho recetas.** Las del cycle 6 son la **16** (gate de licencia para
> packs de skills y prompts — la ruta de contaminación por `cp -r` que el gate
> de build no ve), la **17** (auditoría de grant **por enumeración** en vez de
> por sonda, con el inventario de componentes vendorizados y el cierre de la
> licencia de pesos vía el modelo base) y la **18** (clasificación de audiencia
> **por edad** antes del sprint del Art. 50, porque la obligación europea
> depende de a quién apunta el juego).
>
> Nueve recetas originales. Las seis primeras vienen del pass de julio con sus cifras y
> licencias corregidas; **las recetas 7, 8 y 9 son nuevas** y responden a cambios
> concretos de este ciclo: una fecha límite regulatoria en la UE, world models
> jugables ejecutables en hardware de consumidor, y la aparición del primer
> benchmark publicado de agentes que juegan.

## 🆕 Receta 48: **Plano de decisión 100 % permisivo** — el *flag* como punto de control auditable, sin un solo componente restringido

**Para qué:** un juego live necesita cambiar configuración, contenido y ofertas sin
sacar un build, y necesita **poder probar** qué vio cada jugador. Esta receta arma
esa capa sin ningún grant con reservas y sin SaaS.

**Piezas, todas verificadas el 2026-11-22:**

| Rol | Repo | ★ | Licencia (byte-read) | Dónde corre |
|---|---|---|---|---|
| Contrato de API (el que te des-casa del vendor) | [open-feature/spec](https://github.com/open-feature/spec) | 1.299 | **Apache-2.0** (11.352 B) | especificación |
| Evaluador de flags | [open-feature/flagd](https://github.com/open-feature/flagd) | 1.029 | **Apache-2.0** | *sidecar* junto al backend |
| SDK servidor (Go: el lenguaje de Nakama) | [open-feature/go-sdk](https://github.com/open-feature/go-sdk) | 257 | **Apache-2.0** | backend de juego |
| SDK .NET (vía más corta a Unity) | [open-feature/dotnet-sdk](https://github.com/open-feature/dotnet-sdk) | 143 | **Apache-2.0** | cliente / servicios .NET |
| Estado de los flags sin base de datos | [thomaspoignant/go-feature-flag](https://github.com/thomaspoignant/go-feature-flag) | 2.145 | **MIT** (1.072 B) | lee S3/Git/HTTP |
| Consola + segmentos (si el cliente la quiere) | [Flagsmith/flagsmith](https://github.com/Flagsmith/flagsmith) | 8.533 | **BSD-3-Clause** (1.556 B) | self-host |
| Estadística del experimento | [growthbook/growthbook](https://github.com/growthbook/growthbook) | 8.645 | **MIT Expat**, ⚠️ **excluyendo** `packages/{back-end/src,front-end,shared/src}/enterprise` | self-host |
| Recomendación | [gorse-io/gorse](https://github.com/gorse-io/gorse) | 9.927 | **Apache-2.0** (11.356 B) | servicio |
| Uplift (a quién le cambia la conducta) | [uber/causalml](https://github.com/uber/causalml) | 6.047 | **Apache-2.0** (561 B) | batch |
| LTV / CLV bayesiano | [pymc-labs/pymc-marketing](https://github.com/pymc-labs/pymc-marketing) | 1.314 | **Apache-2.0** (11.357 B) | batch |

**Cableado:**

```
Godot/Unity ──(evaluación de flag con contexto: player_id, banda_etaria, país)──┐
                                                                               ▼
                                            flagd  (Apache-2.0, sidecar)  ◀── flags.json en Git
                                                 │        (el cambio entra por PR y queda firmado)
                        ┌────────────────────────┴───────────────────┐
                        ▼                                            ▼
      GrowthBook (MIT core): asignación + estadística      gorse (Apache-2.0): ranking
                        │                                            │
                        └──── eventos ──▶ Nakama / plano de datos (cycle 16) ──▶ causalml + pymc-marketing (batch)
```

**Pasos:**

1. Programá **contra OpenFeature**, nunca contra el SDK del proveedor. Es lo que
   hace que cambiar de evaluador más adelante sea una línea de configuración.
2. El estado de los flags vive en **Git**, no en una consola: el *quién, cuándo y
   por qué* lo da el historial del repo, que es la evidencia que piden las cuatro
   regiones de `intel/market.md`.
3. Si el cliente exige consola, **Flagsmith** (BSD-3 sin carve-out) antes que
   GrowthBook, y GrowthBook sólo si hace falta el motor de estadística.
4. **Pasá la Receta 49 (gate de carve-out) antes de fijar GrowthBook en el
   contrato**, y dejá por escrito que los tres directorios `enterprise/` quedan
   fuera del entregable.
5. ⚠️ **El último metro hay que escribirlo:** no existe SDK Unity ni Unreal
   permisivo para esta capa (gap medido, cycle 19). El camino es un cliente delgado
   sobre el protocolo OpenFeature; `dotnet-sdk` es el punto de partida para Unity.

**Licencia del entregable:** 100 % Apache-2.0/MIT/BSD-3 si se respetan las dos
exclusiones (los `enterprise/` de GrowthBook y **nada de Unleash v8**).

---

## 🆕 Receta 49: **Gate de carve-out (canal 7)** — medir qué proporción del repo NO es libre, y si está creciendo

**Para qué:** las Recetas 36, 44 y 46 verifican *qué dice* la licencia. Ninguna
verifica **cuánto del repo** cubre el pedazo propietario ni **hacia dónde va**. Un
núcleo *"MIT con un directorio enterprise"* puede ser 0,7 % o 4,6 % del árbol, y la
diferencia decide si el componente es adoptable.

**Los comandos (sobre el clon *blobless* que ya pide la Receta 46):**

```bash
git clone --filter=blob:none --no-checkout https://github.com/<owner>/<repo> r && cd r

# 1) ¿hay carve-out? (enumerar TODOS los archivos de licencia, no sólo la raíz)
git ls-tree -r --name-only HEAD | grep -iE '(^|/)(licen[cs]e|copying)[^/]*$'

# 2) censo de hoy: archivos bajo el carve-out / archivos totales
git ls-tree -r --name-only HEAD | grep -cE '(^|/)(enterprise|ee)/'
git ls-tree -r --name-only HEAD | wc -l

# 3) DERIVA: lo mismo año por año, mismo clon
for y in 2022 2023 2024 2025 2026; do
  c=$(git rev-list -1 --before=$y-01-01 HEAD) || continue
  [ -n "$c" ] && echo "$y $(git ls-tree -r --name-only $c | grep -cE '(^|/)(enterprise|ee)/') / $(git ls-tree -r --name-only $c | wc -l)"
done

# 4) fechar la decisión que lo movió
git log --follow --format='%h %ad %s' --date=short -- LICENSE
```

**Criterio de decisión (y es el aporte de esta receta):**

| Lectura | Veredicto |
|---|---|
| Carve-out **plano o decreciente en proporción** | Adoptable: el proveedor está ampliando el núcleo libre más rápido que el cerrado (**PostHog**: 7,35 % → 1,12 %) |
| Carve-out **creciente en proporción** | 🟡 Adoptable **con exclusión escrita** y revisión en cada renovación (**GrowthBook**: 0,73 % → 4,55 %) |
| Carve-out que toca la **ruta crítica** del entregable | ⛔ No adoptable: la exclusión no es implementable |
| **Sin** archivos de licencia fuera de la raíz | ✅ Limpio (**Flagsmith**: BSD-3, cero carve-out medido) |

**Lo que hay que leer además del conteo:** el texto del carve-out. El de GrowthBook
(2.333 B) dice que las modificaciones y parches que escribas sobre ese código
**quedan en propiedad del licenciante** — una cláusula que no aparece en ningún
conteo y que decide si el equipo del cliente puede contribuir.

**Regla para copiar a toda la familia de KBs:** *el conteo absoluto de archivos
propietarios no es un indicador; la proporción sí, y la derivada de la proporción
es el riesgo.* Un informe que dice "el carve-out se cuadruplicó" sin la proporción
puede estar describiendo un repo que se volvió **más** libre.

---

## 🆕 Receta 50: **El interruptor de oferta** — un flag, tres jurisdicciones que piden cosas incompatibles, apagado por defecto

**Para qué:** Brasil (Decreto 12.880/2026) puede exigir que la funcionalidad de
loot box esté bloqueada o ausente para menores —y **si está apagada por defecto,
exime de verificar edad**—; China (reglas de precios de plataforma, 10-abr-2026)
prohíbe precio distinto por el mismo bien y exige poder **documentar las reglas de
ranking**; EE.UU. tiene cuatro estados con leyes de *surveillance pricing*
(Maryland en vigor desde 1-oct-2026) escritas para retail y de alcance incierto
sobre juegos; la UE pone la oferta dirigida a jugadores vulnerables bajo el Art. 5
en la lectura sectorial de *gambling*. **Las cuatro se satisfacen con la misma
pieza.** Es la Receta 33 (el dato de edad como parámetro de runtime) llevada de la
telemetría a la **monetización**.

**El contrato del flag (una sola decisión, tres salidas):**

```
entradas:  banda_etaria (de la Receta 33)  +  jurisdicción_de_publicación  +  consentimiento
          │
          ▼
offer_personalization = {
   content_variation : bool   # qué se muestra  (lo permitido casi en todas partes)
   price_variation   : bool   # cuánto cuesta   (DEFAULT FALSE SIEMPRE)
   randomised_items  : bool   # loot box        (DEFAULT FALSE si menor o si jurisdicción=BR)
}
```

**Matriz de configuración por defecto:**

| Jurisdicción | `content_variation` | `price_variation` | `randomised_items` | Por qué |
|---|---|---|---|---|
| **Brasil** (menor o edad desconocida) | ✅ | ⛔ | ⛔ **apagado** | Decreto 12.880/2026; apagado por defecto **dispensa la verificación de edad** ⇒ es la opción barata *y* la conforme |
| **China** | ✅ | ⛔ **uniforme** | según aprobación | Reglas de 10-abr-2026: mismo precio en condiciones similares + reglas de ranking exportables + *filing* del algoritmo |
| **UE** | ✅ con registro | ⛔ para perfiles de vulnerabilidad | con divulgación | Art. 5 (lectura sectorial) + Digital Fairness Act sobre diseño adictivo y mecánicas aleatorizadas |
| **EE.UU.** | ✅ | ⛔ **por diseño** | con consentimiento expreso (orden FTC/Epic) | Cuatro estados con leyes de *surveillance pricing*; alcance sobre juegos **no establecido** ⇒ no variar precio es la postura defendible |

**Implementación concreta:**

1. `flagd` (Apache-2.0) evalúa el flag con el contexto; el archivo de reglas vive en
   **Git** (Receta 48, paso 2) ⇒ cada cambio tiene autor, fecha y revisor.
2. `price_variation` **no existe como capacidad** en el cliente: el precio viene del
   catálogo, no del motor de decisión. Lo que no se puede encender no hay que
   defenderlo.
3. La recomendación (`gorse`, Apache-2.0) corre **auto-hospedada** para poder
   exportar las reglas de ranking como documento — requisito explícito de China que
   un recomendador SaaS no puede cumplir de forma creíble.
4. **Salida del proyecto (el entregable que se cobra):** la matriz de arriba
   instanciada para el título, el `flags.json` versionado, y un registro de cambios
   exportable. Eso es lo que se le muestra a un regulador: un registro, no una
   declaración.

⚠️ **Conflicto de fuentes que esta receta NO resuelve** (declarado en
`intel/market.md`): tres lecturas distintas de si el ECA Digital brasileño **prohíbe**
las loot boxes o sólo las somete a transparencia, y la definición de *"juego dirigido
a niños"* no pudo leerse en la fuente primaria en esta sesión. La receta diseña para
el escenario más restrictivo porque además es **el más barato** (dispensa de
verificación).

---

## 🆕 Receta 51: **Salir de Unleash sin heredar la AGPL** — el caso concreto de un cliente que ya lo tiene instalado

**El problema:** `Unleash/unleash` (14.337 ★) pasó de Apache-2.0 a **AGPL-3.0** el
**2026-05-26** (commit `34b5661eb`, PR #12086) y el cambio rige **desde v8**; el
paquete `unleash-server` 8.3.1 declara `"license": "AGPL-3.0-or-later"`. Un cliente
que lo instaló en 2025 lo tiene como Apache-2.0 y no lo sabe; el día que actualice,
cambia de régimen.

**Árbol de decisión:**

| Situación | Acción | Por qué |
|---|---|---|
| Está en **v7.x** y no hay que actualizar | **Quedarse**: v7.6.4 (2026-05-14) es el último release Apache-2.0 | El grant de la versión que ya tenés no cambia retroactivamente |
| Está en v7.x y hay que actualizar | **Migrar a flagd o go-feature-flag** antes de subir a v8 | La actualización es lo que importa el copyleft de red |
| Ya está en **v8** y el servidor es interno | 🟡 Evaluar: AGPL + uso interno **no** dispara distribución | Pero sí la dispara si el servidor se entrega al cliente como parte del producto |
| Ya está en v8 y el servidor va **dentro del entregable** | ⛔ Reemplazar | Es copyleft de red en un componente que se distribuye |
| Lo que está enlazado son **los SDKs** | ✅ **Nada que hacer** | `unleash-node-sdk`, `-go-sdk`, `-java-sdk`, `-dotnet-sdk`, `-ruby-sdk` son **Apache-2.0**; `-python-sdk` y `-php-sdk`, **MIT** |

**Pasos de la migración (una tarde, no un trimestre):**

1. Introducir **OpenFeature** como fachada en el código del cliente (`go-sdk`,
   `dotnet-sdk`, `java-sdk`: Apache-2.0). A partir de acá el proveedor es
   reemplazable.
2. Exportar las definiciones de flags de Unleash y traducirlas a **`flagd`**
   (Apache-2.0) o a **`go-feature-flag`** (MIT) — ambos leen un archivo, así que la
   migración es un script, no una plataforma.
3. Verificar con la **Receta 44** (canal 5) la historia de licencia del reemplazo
   **antes** de firmarlo, y con la **Receta 49** si trae carve-out.
4. ⚠️ Corregir las referencias: los SDKs **se renombraron** a
   `unleash-<lenguaje>-sdk`. Los slugs viejos (`unleash-client-go`,
   `unleash-proxy-client-js`) devuelven **422** y sobreviven sólo como redirect.
5. ⚠️ Un dato que circula y que **no se pudo verificar en la fuente primaria**: que
   *"la versión open source se apaga el 31-dic-2026"*. Esa fecha corresponde a
   **Unleash Edge OSS**, no al servidor, y viene de la comparativa de un
   **competidor**; `getunleash.io` y `docs.getunleash.io` no resuelven desde esta
   sesión. **No usar ese dato en una propuesta sin confirmarlo.**


## 🆕 Receta 43: **Fábrica de build 100% permisiva** — la capa que estaba entre la Receta 37 (assets) y la 19 (QA) y no estaba escrita

**Caso de uso**: un estudio donde el arte ya se genera (R37), los NPC ya hablan
(R35) y el agente ya juega (R19), pero **el build sale a mano**: alguien abre el
engine, exporta, sube el zip. Es el punto donde los pilotos de IA mueren en el mes 3,
porque nada de lo anterior entra en una cadencia.

**Por qué ahora**: esta KB midió este pass que las seis categorías del carril —
versionado binario, CI, caché, tracking, QA de UI, publicación— **tenían cero filas
en 16 passes**, y que una de ellas (CI de Unreal) **no tiene opción permisiva**.

### Variante A — cliente Unity (ruta 100% MIT/Apache)

```
repo del juego (Git)
  ├── binarios      git-lfs/git-lfs            MIT (2.818 B)          ← assets no fusionables
  ├── datasets      treeverse/dvc              Apache-2.0 (11.340 B)  ← sólo si hay ML propio
  └── CI            GitHub Actions
        ├── game-ci/docker            MIT (1.071 B)   imagen base del runner
        ├── game-ci/unity-builder     MIT (1.097 B)   build por plataforma
        ├── game-ci/unity-test-runner MIT (1.097 B)   Unity Test Framework
        └── mozilla/sccache           Apache-2.0      caché de compilación (nativo/IL2CPP)
              ↓ artefacto jugable
        ramenvr/autopilot (Apache-2.0)   ← Receta 19, corrida nocturna sobre el build de la noche
              ↓ crash + telemetría
        GlitchTip (MIT)                  ← Receta 41, cycle 16
```

🔴 **Los dos pasos que no se pueden saltar:**

1. **`game-ci` es MIT; Unity no.** Hay que inyectar la licencia de Unity como secreto
   del runner. Decirlo en la estimación: no es un bloqueo, es una línea de costo y una
   conversación con el cliente sobre su contrato de Unity.
2. **Nunca empezar por `game-ci/unity-actions`** (1.097★, **último commit 2023-03-04**),
   que es el que devuelve la mayoría de las guías. El vivo es `unity-builder`.

### Variante B — cliente Unreal (y por qué cuesta más)

```
Perforce (propietario)  ──►  Horde (fuente bajo EULA de UE, CI sin soporte de Git/SVN, UE 5.4+)
        ó
Git + TeamCity (propietario, free tier) + JetBrains/teamcity-unreal-engine-plugin (Apache-2.0, 21★)
        ó
Git + Actions a mano con BuildGraph/UAT  ← sin plugin permisivo que lo envuelva
```

⇒ **El presupuesto de pipeline de un proyecto Unreal no es igual al de uno Unity, y
la diferencia no es de horas: es de licencias de terceros.** Esta receta existe para
que eso aparezca en la estimación y no en el mes 3.

**Esfuerzo**: 1-2 semanas (Unity) · 3-5 semanas (Unreal, incluyendo el server de CI).
**Entregable**: cadencia nocturna build → test → playtest con agente → reporte, con
cada pieza del lado permisivo salvo las dos declaradas.

**Por región**: en **EMEA** la variante A es además la que da trazabilidad para el
inventario del Art. 50(2) (la R7 necesita saber qué build contiene qué asset generado).
En **LATAM** y **APAC**, donde el cliente típico tiene equipo chico, la variante A
reemplaza una persona de release. En **North America**, Steam exime las herramientas
de desarrollo: **esta receta no dispara disclosure ni backlash**.

---

## 🆕 Receta 44: **Gate de canal 5** — verificar la HISTORIA de la licencia antes de adoptar, y recuperar la última versión permisiva

**Caso de uso**: una dependencia pasa el gate de la Receta 10/20 (badge + archivo +
checksum), entra al stack, y **seis meses después el área legal del cliente descubre
que el proyecto se relicenció el año pasado** y que la versión que corre en producción
ya no es la que se auditó.

**Por qué ninguna receta anterior lo detecta**: las Recetas 10, 14, 17, 20 y 21 leen
**el archivo de hoy**. Un relicenciamiento **no deja rastro en el archivo resultante**.

**El gate, mecánico (2 minutos por repo, sin dependencias):**

```bash
slug=ynput/ayon-backend
git clone -q --filter=blob:none --single-branch https://github.com/$slug /tmp/ch5
cd /tmp/ch5

# 1. ¿cambió la licencia alguna vez?
git log --follow --date=short --pretty='%h %ad %s' -- LICENSE LICENSE.md LICENSE.txt COPYING

# 2. si cambió: ¿cuál fue el último tag ANTES del cambio? (= última versión permisiva)
for t in $(git tag --sort=creatordate); do echo "$(git log -1 --date=short --pretty=%ad $t) $t"; done

# 3. ¿qué dice la cláusula de conversión? (FSL y BSL la tienen; MIT/Apache no)
grep -iE "future license|change date|second anniversary" LICENSE
```

**Los cuatro resultados posibles y qué hacer con cada uno:**

| Resultado | Lectura | Acción |
|---|---|---|
| Un solo commit, inicial | licencia estable desde el día 1 | ✅ adoptar; **y poder afirmarlo** en el entregable |
| Cambios sólo de año/copyright | estable; **puede haber cambio de dueño** | 🟡 leer el diff: `Iterative, Inc.` → `Treeverse.` es un cambio de dueño sin cambio de grant |
| Cambio de familia de licencia | relicenciamiento | 🔴 fechar el corte, identificar la última versión permisiva, decidir congelar o esperar |
| Hay cláusula de conversión | FSL / BSL | 🔴 calcular la fecha de conversión **por versión**, no por proyecto |

**Caso trabajado (medido este pass):** `ynput/ayon-backend` — Apache-2.0 desde
2022-02-28, **FSL-1.1-ALv2 desde el commit `97627e27` del 2025-01-27**; última release
Apache-2.0 **1.6.1 (2025-01-09)**; primera FSL **1.7.0 (2025-01-30)**, que **convierte
a Apache-2.0 el 2027-01-30**. Las dos salidas son congelar en 1.6.1 o esperar; no hay
una tercera, y las dos se dicen en una línea de propuesta.

**Dónde conecta**: es el paso 0 de la Receta 23 (gate de BSL/Change Date) — la FSL es
la misma estructura con conversión a Apache-2.0 a los 2 años — y el complemento de la
Receta 40 (gate de sucesión): la 40 verifica al **reemplazo**, la 44 verifica al
**pasado del titular**.

**Copiar a toda la familia de KBs.** El costo es un clone sin blobs; el ahorro es una
renegociación de contrato.

---

## 🆕 Receta 45: **Adoptar una plataforma con grant PARTIDO** — servidor restringido, cliente permisivo

**Caso de uso**: la mejor plataforma de la categoría tiene el servidor bajo licencia
source-available y los clientes bajo Apache-2.0 (AYON es el caso medido; el patrón se
repitió este mismo año en el plano de datos con Snowplow, Receta 41).

**El reparto que hay que escribir en la propuesta:**

| Pieza | Licencia | Quién la toca | Qué se puede prometer |
|---|---|---|---|
| Servidor (`ayon-backend`, `ayon-frontend`) | **FSL-1.1-ALv2** | se **despliega** en la infra del cliente, sin modificar | uso interno ✅ · servicios profesionales a un licenciatario ✅ *(Permitted Purpose nº4, textual)* · ofrecerlo como producto gestionado ⛔ |
| Clientes y SDK (`ayon-core`, `ayon-launcher`, `ayon-python-api`) | **Apache-2.0** | acá vive **todo** lo que escribe Globant | entregable limpio, del cliente, sin restricción |
| Addons propios | **la que elija el cliente** | repo del cliente | — |

**Regla de diseño:** *si una funcionalidad obliga a parchear el servidor, está del lado
equivocado del corte.* Rediseñarla como addon sobre la API Apache-2.0 o no hacerla.

**Pregunta para la reunión de arranque (una sola):** *"¿el entregable tiene que poder
redistribuirse bajo licencia permisiva, o alcanza con que se pueda operar y extender?"*
La primera respuesta obliga a congelar en la última versión permisiva (AYON 1.6.1) o a
esperar la conversión (2027-01-30); la segunda no obliga a nada.

**Por región**: en **EMEA** —donde nacieron las dos plataformas de esta capa, Ynput
(Chequia) y CGWire (Francia)— es además la conversación de soberanía: las dos se
despliegan on-prem. En **North America** el reflejo del cliente va a ser comprar
ShotGrid/ftrack; el argumento no es precio, es **que el pipeline no se lleva el dato
del estudio a un SaaS de un competidor**.

---

## 🆕 Receta 41: **Plano de datos del jugador sin una sola licencia restringida** — y el motivo por el que se vende primero

**Problema.** Todas las capas que esta KB fue añadiendo —NPC, assets, voz,
accesibilidad, QA— **producen eventos o consumen datos del jugador**, y ninguna se
puede medir, facturar ni defender ante un regulador sin un plano de datos debajo.
El cliente ya tiene uno. La pregunta nunca es si se construye: es **bajo qué
licencia corre el que ya tiene**.

🔴 **Y hay una probabilidad alta de que esté fuera de términos hoy.** Si usa el
pipeline de Snowplow, `stream-collector` y `enrich` están bajo **SLULA 1.1**, cuyo
§1.1 concede la licencia *"only for Non-Production Use or Non-Commercial Use"*, y
cuyo §1.2.2 define *Non-Production Use* como procesar **datos de prueba o
sintéticos para evaluar** el software. No es una cláusula anti-SaaS: **es la
prohibición de usarlo en producción.** Relicencia del core: **8-ene-2024** — hay
estudios corriéndolo desde antes que nunca releyeron el archivo.

### El stack, componente a componente, con la licencia leída byte a byte

```
  SDK en el juego                 ⟶  snowplow-javascript-tracker  BSD-3-Clause (1.546 B)
        │                             (el tracker SÍ es permisivo — ver nota del eje cliente/servidor)
        ▼
  Ingesta HTTP                    ⟶  opensnowcat/opensnowcat-collector   Apache-2.0 (11.358 B)  81★
        │
        ▼
  Validación + enriquecido        ⟶  opensnowcat/opensnowcat-enrich      Apache-2.0 (11.359 B)  40★
        │   └─ esquemas de evento ⟶  opensnowcat/opensnowcat-iglu-server Apache-2.0  ⚠️ dormido 2024-08
        ▼
  Almacén de eventos              ⟶  ClickHouse/ClickHouse               Apache-2.0        51.060★
        │
        ├─ BI / exploración       ⟶  apache/superset                     Apache-2.0        75.615★
        ├─ Trazas del game server ⟶  open-telemetry/opentelemetry-collector Apache-2.0      7.793★
        ├─ Observabilidad unif.   ⟶  ClickHouse/ClickStack               MIT (1.077 B)     10.061★
        └─ Crash / errores        ⟶  GlitchTip (GitLab)                  MIT (1.067 B)
                                      · protocolo compatible con los SDK de Sentry
```

**Ruta ligera**, si el volumen no justifica el pipeline completo:
[`tx7do/go-wind-uba`](https://github.com/tx7do/go-wind-uba) (MIT, 1.061 B, 51 ★ —
25 modelos de analítica con telemetría de juego nombrada explícitamente, Kafka →
ClickHouse/Doris) o [`mirlan-irokez/rawbbit`](https://github.com/mirlan-irokez/rawbbit)
(Apache-2.0, 11.349 B, 11 ★ — eventos → NATS JetStream → Parquet en tu object
storage, con un MCP de sólo lectura que deja a un agente consultar la telemetría).

**Modelo de eventos de juego:** [`GameTuner`](https://github.com/GameTuner/collector)
(Apache-2.0, 10 repos) como **referencia** de qué instrumentar en un juego móvil
—ID único multi-dispositivo, latencia de datos offline, eventos de progresión—.
⚠️ **No como dependencia:** 0 ★ y sin commits desde feb-2024, sólo GCP/pubsub, y
**antes de forkearlo hay que verificar si el snapshot de Snowplow que lleva dentro
es anterior al 8-ene-2024** (ver el límite declarado en `repos/foundations.md`).

### ⛔ Lo que NO se pone en este stack, y qué se rompe si se pone

| Tentación | Licencia medida | Qué se rompe |
|---|---|---|
| `getsentry/sentry` | **FSL-1.1-Apache-2.0** (3.785 B) | El grant Apache-2.0 es *"effective on the second anniversary"* de **cada versión**: la que querés desplegar es, por construcción, la que aún no convirtió. **Usar GlitchTip, que habla el mismo protocolo.** |
| `grafana/grafana` · `rybbit-io/rybbit` | **AGPL-3.0** | Copyleft de red. Si el entregable se expone al cliente o a terceros, arrastra. **Usar Superset.** |
| `metabase/metabase` | **AGPL + Metabase Commercial** (855 B) | Carve-out en `enterprise/`. |
| `openreplay/openreplay` | multi-licencia, `ee/` propietario (36.511 B) | Carve-out en `ee/`. |
| `Countly/countly-server` | **AGPL §7 modificada** (1.025 B) | AGPL **más** prohibición de quitar el logo: afecta al *white-label*, que es justo lo que un estudio quiere. |
| `snowplow/stream-collector` · `snowplow/enrich` | **SLULA 1.1** | Producción prohibida. |

### El checkpoint que convierte esto en una propuesta defendible

**Una cláusula de licencia en la propuesta, por componente, que responda una sola
pregunta: *¿qué pasa si el cliente redistribuye el entregable?*** Es la diferencia
entre un stack permisivo y uno que **parece** permisivo, y es barata de escribir
ahora y carísima de descubrir en la revisión legal del cliente.

⚠️ **Y una advertencia honesta sobre `iglu-server`:** es el registro de esquemas
—la pieza que define qué es un evento válido— y lleva **más de dos años sin un
commit**. Se usa, se fija la versión y **se asume el mantenimiento en el
presupuesto**. Decirlo en la propuesta; no descubrirlo en el mes cuatro.

### Por qué se vende antes que las otras capas

Es la única donde el argumento **no es de oportunidad sino de riesgo ya
contraído**. No hay que convencer a nadie de adoptar IA generativa —y según la GDC
2026 el 52% de los desarrolladores ya cree que le hace mal a la industria—: hay que
mostrar que el pipeline que corre hoy está fuera de los términos de su licencia, y
que la migración a OpenSnowcat es **un reemplazo componente a componente**, no una
reescritura.

---

## 🆕 Receta 42: **Partición de telemetría por jurisdicción y banda etaria** — la mitad de almacenamiento que le falta a la Receta 33

**Problema.** La [Receta 33](#) resolvió el dato de edad **en runtime**: no
persistir `age`, persistir `ageBand` + `isMinor` + la cadencia como parámetro, y
descartar la fuente. Correcto, y **incompleto**: esos eventos se escriben en algún
lado, y ahí vuelven a ser un problema, porque el **borrado** que exige Texas
SB 2420 y la **retención** que exigen otros regímenes se ejecutan sobre **tablas**,
no sobre variables de sesión.

Y hay un tercer régimen nuevo que cae exactamente sobre esta decisión: las medidas
chinas sobre **IA antropomórfica**, en vigor desde el **15-jul-2026**, que
prohíben ofrecer a menores servicios de IA construidos sobre **relaciones
virtuales** y que —a diferencia de otras leyes de *companion AI*— **no excluyen
los videojuegos**. *(Fuente secundaria, Stanford CodeX 19-nov-2026, sin verificar
en origen — ver `intel/market.md` → APAC.)*

### La decisión de esquema, que cuesta barata hoy y carísima después

**Paso 1 — particionar por los dos ejes que mandan los reguladores, no por fecha.**

```sql
-- ClickHouse (Apache-2.0)
CREATE TABLE player_events (
    event_time    DateTime,
    jurisdiction  LowCardinality(String),  -- 'EU','US-TX','US-CT','CN','BR',...
    age_band      LowCardinality(String),  -- '<13','13_15','16_17','adult'  (R33: categoría, NUNCA la edad)
    player_key    String,                  -- seudónimo rotable, no el ID de cuenta
    event_name    LowCardinality(String),
    payload       String
)
ENGINE = MergeTree
PARTITION BY (jurisdiction, age_band, toYYYYMM(event_time))
ORDER BY (event_name, event_time);
```

> **`PARTITION BY (jurisdiction, age_band, …)` es la receta entera en una línea.**
> Un `DROP PARTITION` borra exactamente el conjunto que un regulador pide y **nada
> más**. Con una partición global por fecha, atender ese mismo pedido es un
> `DELETE` masivo que bloquea la tabla y que nadie puede demostrar que fue
> completo.

**Paso 2 — TTL por partición, nunca global.** Los regímenes piden plazos distintos
y uno de ellos obliga a **borrar** lo que otro obliga a **conservar** (el conflicto
que la R33 ya documentó). Un TTL único de tabla no puede satisfacer a los dos:

```sql
ALTER TABLE player_events
  MODIFY TTL event_time + INTERVAL 30 DAY  DELETE WHERE age_band = '<13',
             event_time + INTERVAL 90 DAY  DELETE WHERE age_band IN ('13_15','16_17'),
             event_time + INTERVAL 730 DAY DELETE WHERE age_band = 'adult';
```

**Paso 3 — los eventos de NPC-compañero van a su PROPIA tabla.** Diálogo emocional
sostenido, memoria del NPC, vínculo persistente: es la categoría que la norma china
señala y **la primera que un regulador va a pedir**. Mezclada en el flujo general
es imposible de aislar después; separada, tiene su propio TTL, su propia partición
y su propia respuesta.

**Paso 4 — la banda etaria es un campo del ESQUEMA, no una convención de nombres.**
Se declara en `opensnowcat/opensnowcat-iglu-server` (Apache-2.0), de modo que un
evento sin `age_band` **no valida y no entra**. Es la única forma de que la
partición no se degrade en seis meses.

**Paso 5 — el consentimiento y el opt-out se resuelven en el SDK**, antes de la
ingesta. Un evento que no debió recolectarse no se arregla en el almacén.

**Paso 6 — la salida que compra el cliente:** un informe por jurisdicción desde
**Apache Superset** (Apache-2.0) que un DPO pueda leer sin saber SQL: qué
categorías de evento existen, de qué bandas etarias, con qué retención y cuándo se
ejecutó el último borrado.

### ⚠️ El límite honesto, y va en la propuesta

**No existe un estándar de auditoría para telemetría de menores en videojuegos.**
Los plazos del paso 2 son un punto de partida razonable, **no** un umbral
normativo: lo fija el regulador aplicable o el asesor legal del cliente. **Decirlo
por escrito en la propuesta** es lo que separa esta receta de una promesa de
cumplimiento que nadie puede sostener — y es lo que hace que el equipo legal del
cliente pase de bloqueante a patrocinador.

**Esfuerzo:** 1-2 semanas sobre un plano de datos que ya existe (Receta 41);
3-4 semanas si hay que migrarlo primero.

---

## 🆕 Receta 39: **Equivalencia voz ↔ texto como un solo componente** — la pieza que cumple el CVAA, habilita la accesibilidad y resuelve la moderación de voz

**El problema que resuelve de verdad:** tres necesidades que todo estudio trata
como tres proyectos distintos —accesibilidad del chat, cumplimiento del CVAA y
moderación del canal de voz— **se satisfacen con el mismo componente**. Y la
tercera no tiene oferta open source: medida este pass, la moderación de voz
arroja **1 candidato de 0 ★**. Transcribir convierte un problema sin oferta en
uno que esta KB ya tiene resuelto desde el cycle 13.

**Disparador:** el juego tiene chat de voz, o chat de texto, y se distribuye en
EE. UU. (CVAA, vigente desde el 1-ene-2019) o en la UE (EAA, aplicable desde el
28-jun-2025, **ya litigado en Francia**).

### Cableado

```
  Canal de voz del juego
        │
        ├─► STT  ──► transcripción ──┬─► 1. SALIDA DE TEXTO del chat de voz   [CVAA]
        │                            │
        │                            ├─► 2. unitaryai/detoxify (Apache-2.0)   [moderación]
        │                            │      + guardrails-ai (Apache-2.0)
        │                            │
        │                            └─► 3. captions en pantalla              [EAA / EN 301 549]
        │
  Canal de texto del juego
        │
        └─► TTS  ──► audio ──────────► 4. SALIDA DE VOZ del chat de texto      [CVAA]
                                          │
                                          └─► AccessKit (Apache-2.0 OR MIT) en Godot ≥ 4.5
                                              o ethindp/prism (MPL-2.0) fuera de Godot
```

### Piezas, con licencia leída byte a byte

| Pieza | Repo | Licencia | Nota |
|---|---|---|---|
| Lector de pantalla (Godot ≥ 4.5) | [AccessKit/accesskit](https://github.com/AccessKit/accesskit) | **Apache-2.0 OR MIT** (9.723 B + 1.023 B) | **Ya viaja en el binario.** Elegir Apache-2.0 por la concesión expresa de patentes |
| Abstracción TTS / lector, fuera de Godot | [ethindp/prism](https://github.com/ethindp/prism) | **MPL-2.0** (15.504 B) | Copyleft **por archivo**: usable en juego propietario, obliga a publicar las modificaciones de los archivos MPL |
| Accesibilidad de UI en Unity | [mikrima/UnityAccessibilityPlugin](https://github.com/mikrima/UnityAccessibilityPlugin) | **Apache-2.0** (11.357 B ✅) | ⚠️ 3,5 años sin push: adoptar con los ojos abiertos o construir sobre la API oficial de Unity |
| Moderación de texto | [unitaryai/detoxify](https://github.com/unitaryai/detoxify) | **Apache-2.0** | Ya en esta KB. Corre local, sin API |
| Validadores de salida | [guardrails-ai/guardrails](https://github.com/guardrails-ai/guardrails) | **Apache-2.0** | Ya en esta KB |
| STT / TTS | *capa de voz del cycle 13* | ver `agents/top.md` | Las licencias de esa capa ya están verificadas y **particionadas por pesos**: el código permisivo no implica pesos permisivos |
| Captions direccionales — referencia de diseño | [jumpingmushroom/Earshot](https://github.com/jumpingmushroom/Earshot) | **MIT** (1.069 B) | Mod de Valheim: captions de sonidos **con flecha de dirección**. Es el patrón a copiar, no la dependencia a importar |

### Pasos

1. **Inventario ACS** — enumerar cada canal de comunicación (voz, texto, *party*,
   clan, emotes) y marcar cuál tiene equivalente en la otra modalidad. Dos semanas.
2. **Elegir el eje por engine, no por librería** — Godot ≥ 4.5 ya trae la
   primitiva; Unity obliga a decidir entre UAP estancado y la API oficial cuyo
   sample **no es open source** (Unity Companion License).
3. **Etiquetado semántico y exclusiones explícitas.** Sin esto la primitiva
   **lee de más**: hay un caso reportado de un `TextureRect` usado como cursor
   que el Narrador de Windows 11 leía en voz alta.
4. **Declarar la atribución de Chromium.** AccessKit incorpora porciones
   derivadas de Chromium bajo **BSD-3-Clause**: el aviso y el disclaimer deben
   reproducirse en la distribución binaria.
5. **Enchufar la transcripción a la moderación** antes de construir nada propio
   de moderación de voz.

### ⛔ Lo que esta receta NO promete

- **No cubre el EAA entero.** El alcance del EAA para videojuegos está en disputa
  (ver `intel/market.md`): el IGDA sostiene que los juegos en general no están
  cubiertos y que lo exigido es sobre todo el chat de texto junto al de voz —que
  es justamente lo que esta receta entrega—, mientras un proveedor sostiene que
  aplica todo EN 301 549. **Se entrega la intersección, y se dice.**
- **No usar v4.1.1 como requisito contractual** hasta verificar su cita en el DOUE.
- **No cubre exportación web de Godot**: comportamiento no medido, riesgo abierto.

---

## 🆕 Receta 40: **Gate de sucesión** — verificar la licencia del reemplazo ANTES de aceptar la migración de una dependencia archivada

**El problema que resuelve:** esta KB midió **dos** casos en los que el sucesor
de un repo archivado es **menos libre** que el original, y en ninguno el
mantenedor menciona la licencia al explicar el cambio:

| Original | Sucesor | Movimiento |
|---|---|---|
| [m1maker/SRAL](https://github.com/m1maker/SRAL) — **MIT**, archivado 2026-08-05 | [ethindp/prism](https://github.com/ethindp/prism) — **MPL-2.0** | permisiva → copyleft **por archivo** |
| `Piper` — **MIT**, archivado *(cycle 13)* | sucesor — **GPL-3.0** | permisiva → copyleft **fuerte** |

El riesgo es de **secuencia**: la migración se decide por razones técnicas (el
repo está muerto, hay que salir), el reemplazo lo nombra el propio README del
muerto, y **la licencia se descubre después de que el código ya está integrado**.

### El gate, cinco comprobaciones mecánicas

```bash
# 1. ¿El repo actual está archivado o muerto?
gh api -X GET search/repositories -f q="repo:$SLUG" \
  --jq '.items[] | "archived=\(.archived) pushed=\(.pushed_at)"'

# 2. ¿A quién remite? (el README del archivado nombra al sucesor)
curl -s "https://raw.githubusercontent.com/$SLUG/main/README.md" | head -40

# 3. Licencia del SUCESOR, leída byte a byte — NO el badge
curl -s "https://raw.githubusercontent.com/$NEW_SLUG/main/LICENSE" | head -5
curl -s -o /tmp/l -w '%{size_download}\n' \
     "https://raw.githubusercontent.com/$NEW_SLUG/main/LICENSE"

# 4. ¿Es Markdown? Entonces el checksum de bytes NO aplica (defecto 6º)
#    .md o primera línea con '#' ⇒ leer el encabezado, no comparar bytes

# 5. ¿El grant del sucesor es compatible con el tipo de entregable?
#    (reusa el eje de la Receta 32: librería interna ≠ binario embarcado)
```

### Criterio de decisión

| Sucesor | Veredicto | Acción |
|---|---|---|
| Permisiva (MIT / Apache-2.0 / BSD) | 🟢 Migrar | Registrar la atribución si aplica |
| **MPL-2.0** | 🟡 Migrar **declarando** | Usable en producto propietario; obliga a publicar las modificaciones de los **archivos MPL**. Ponerlo en la propuesta, no en la integración |
| **GPL-2.0 / GPL-3.0 / AGPL** | 🔴 No embarcar | Sólo herramienta interna. Si el original archivado era permisivo, **evaluar hacer fork del archivado** antes que migrar |
| Sin archivo de licencia | ⛔ No migrar | Un sucesor sin grant es peor que un original muerto con grant |

> **La pregunta que hay que hacerse y casi nadie hace:** si el original es
> permisivo y está archivado pero **funciona**, un fork mantenido puede costar
> menos que la obligación que impone el sucesor. **Archivado no es lo mismo que
> inservible** — esta KB ya lo midió con ViZDoom en el cycle 11.

## 🆕 Receta 46: **Auditoría de licencias por árbol completo (canal 6)** — la que reemplaza a las Recetas 10, 14, 17 y 20 en el paso de enumeración

**Qué resuelve.** Las Recetas 10/14/17/20 enumeran grants **probando rutas**. Una
sonda no distingue *"no existe"* de *"no se me ocurrió"*: aplicada a
`SkyworkAI/Matrix-Game` encontró **7** archivos de licencia y declaró que no podía
saber si había más. **Hay 28**, y el nº 28 es `CC BY-NC 4.0`. Esta receta hace el
paso de enumeración **completo y mecánico**.

**Cuándo se corre.** Antes de poner cualquier repo en una propuesta, y **de nuevo**
antes del primer entregable (un `third_party/` nuevo entra con un `git pull`).

### Implementación

```bash
#!/bin/bash
# canal 6 — enumeración completa de grants. Uso: ./c6.sh owner/repo
slug="$1"; d="/tmp/c6/$(echo "$slug" | tr '/' '_')"
rm -rf "$d"

# 1. clon BLOBLESS: baja el árbol, cero contenido de archivos.
#    Matrix-Game = 2.765 archivos listados sin descargar el repo.
git clone --filter=blob:none --no-checkout --depth 1 \
    "https://github.com/$slug" "$d" || { echo "NO CLONE: $slug"; exit 1; }
cd "$d" || exit 1

# 2. enumerar. El filtro va sobre el BASENAME COMPLETO con `contains`:
#    un ancla inicial (^licen...) pierde MG4CF-LICENSE, TAEHV-LICENSE,
#    SAL-LICENSE.txt, mediamtx-reader.LICENSE -> 23 en vez de 28. (T68)
git ls-tree -r --name-only HEAD \
  | grep -iE '[^/]*(licen[cs]e|copying|notice|legal)[^/]*$' \
  | grep -viE '\.(py|js|ts|go|cs|h|cpp|rs|java|meta|png|jpg)$' \
  | while read -r p; do
      bytes=$(git cat-file -p "HEAD:$p" | wc -c)
      lines=$(git cat-file -p "HEAD:$p" | wc -l)
      inc=$(git cat-file -p "HEAD:$p" | grep -m1 -vE '^\s*$' | cut -c1-70)
      printf '%-8s %-6s %-70s %s\n' "$bytes" "$lines" "$p" "$inc"
    done
```

### Triage del resultado — **en este orden**, que es el inverso del habitual (T69)

| # | Dónde mirar | Por qué primero |
|---|---|---|
| 1 | `third_party/` · `external/` · `vendor/` | Es donde está el 95% del volumen **y el 100% de las sorpresas** |
| 2 | `eval/` · `benchmark/` · `test*/` · `*Score/` | 🔴 **Donde vivía el único `NonCommercial` de 41 repos.** El benchmark no es el producto, pero se entrega igual |
| 3 | `assets/` · `weights/` · `models/` · `demo/` | Grants por **tipo de medio** (`limboai`: código MIT, logo CC BY 4.0) |
| 4 | Subdirectorios de producto (`Matrix-Game-4/`, `com.unity.*/`) | Grant partido por versión o por paquete |
| 5 | La raíz | **Último.** Es el único que el equipo legal ya miró |

### Clasificar cada archivo encontrado

```bash
# ¿NO COMERCIAL? -> exclusión dura, no hay mitigación
grep -ci 'noncommercial\|non-commercial' "$f"

# ¿copyleft fuerte? -> GPL/AGPL. Mirar si está en el build o sólo en bench/
grep -m1 -i 'GNU GENERAL PUBLIC\|AFFERO' "$f"

# ¿Apache-2.0 íntegro? NO por bytes (son >=4 válidos: 9723/10197/11335/11557).
# Por LÍNEAS + el literal de cierre: (T70)
wc -l < "$f"                                   # 201 con apéndice, ~176 sin
grep -c 'END OF TERMS AND CONDITIONS' "$f"     # debe ser 1

# ¿adulterado? -> cláusula añadida sobre un texto estándar
grep -inE 'shall not be used|trademark of|notwithstanding the foregoing|amend' "$f"
```

### Salida para el cliente

Una tabla de **5 columnas** que **separa el producto del andamio**, porque esa
separación es la que cambia el precio. 🔴 **Corregido en el cycle 19:** el cycle 18
dejó acá el **encabezado sin filas** —el defecto exacto que la regla de la casa
prohíbe, porque un encabezado suelto se compila como entidad— y además decía *"3
columnas"* sobre una tabla de 5. Va como **plantilla en bloque de código**, que es
lo que es, y no como tabla vacía:

```
| Componente | Ruta | Grant | ¿Se embarca? | ¿Bloquea? |
|---|---|---|---|---|
| <nombre>   | <ruta en el árbol> | <SPDX + bytes> | sí/no | sí/no |
```

**Ejemplo real, Matrix-Game (medido 2026-11-22):** 28 archivos → 24 permisivos
embarcables, 3 textos legacy de Eigen que no aplican al uso, **1 `CC BY-NC 4.0`
que no se embarca pero sí se ejecuta** ⇒ la propuesta sale, con una línea de
alcance explícita. Ver Receta 47.

### Lo que esta receta **no** hace

- **No ve la licencia de los pesos.** `huggingface.co` no resuelve en este entorno
  (código 000) y los pesos no están en el árbol de git. En un world model eso es
  **la mitad del entregable sin verificar** — declararlo en la propuesta.
- **No ve el futuro ni el pasado del archivo**: eso es el canal 5
  (`git log --follow -- LICENSE`, Receta 44).
- **No ve dependencias no vendorizadas** (`requirements.txt`, `package.json`,
  `go.mod`): ésas son las Recetas 14 y 17.

---

## 🆕 Receta 47: **Contratar un world model separando el modelo del banco de pruebas**

**Qué resuelve.** El hallazgo del cycle 18: en `SkyworkAI/Matrix-Game` el **modelo**
es MIT/Apache-2.0 y el **benchmark con el que el modelo se valida**
(`GameWorldScore`) contiene un componente `CC BY-NC 4.0`. *Entregar el modelo* y
*reportar la métrica publicada del modelo* son **dos decisiones de licencia
distintas**, y hasta este pass esta KB las trataba como una sola.

**El error que evita:** un PoC pago que entrega un prototipo de world model **y en
el informe final reporta GameWorldScore** ha ejecutado un componente no comercial
para producir un entregable comercial. Es barato de evitar antes y caro de
descubrir en la revisión legal del cliente.

### El mapa, medido

| Capa | Rutas | Grant | ¿Comercial? |
|---|---|---|---|
| 🟢 **Modelo** | `LICENSE` (MIT, 1.118) · `Matrix-Game-1/3/4/LICENSE` (Apache-2.0) · `licenses/MG4CF-LICENSE` · `licenses/TAEHV-LICENSE` · `mediamtx-reader.LICENSE` | MIT + Apache-2.0 | ✅ **sí** |
| 🟢 **Runtime del juego** | `Matrix-Game-4/game/third_party/{JoltPhysics,DirectXMath,stb,recastnavigation}` | MIT ×3 · zlib · dual PD/MIT | ✅ **sí**, con avisos de atribución |
| ⛔ **Benchmark** | `Matrix-Game-1/GameWorldScore/GameWorld/third_party/amt/` | **CC BY-NC 4.0** (18.208 B) | ❌ **no** |
| 🟡 Benchmark (resto) | `DROID-SLAM` · `RAFT` · `lietorch` (BSD-3) · `tartanair_tools` (CMU) · Eigen (MPL-2.0; textos GPL-3.0/LGPL-2.1/GPL-2.0 legacy) | permisivo + copyleft débil por archivo | 🟡 sí, con cuidado |
| ❓ **Pesos** | Hugging Face | **sin verificar** (host no resuelve) | ❓ **declarar como riesgo abierto** |

### Las tres salidas, por orden de costo

1. **Extirpar `amt` y no reportar la métrica.** Se entrega el modelo; la validación
   se hace con métricas propias (FVD/PSNR/LPIPS sobre un set del cliente). **Costo:
   días.** Es la opción por defecto.
2. **Reimplementar la métrica** sin el componente NC, si el cliente exige
   comparabilidad con el paper. **Costo: semanas**, y el número **no** es citable
   como "GameWorldScore".
3. **Correr GameWorldScore tal cual**, sólo en investigación interna no facturada y
   sin que el resultado entre en ningún entregable. **Frágil**: depende de una
   frontera que nadie audita en el momento. No recomendada en engagement pago.

### Cláusula de alcance para la propuesta

> *El modelo base se entrega bajo MIT/Apache-2.0. La validación se realiza con
> métricas propias acordadas con el Cliente; **no** se ejecuta ni se reporta el
> benchmark publicado por el proveedor del modelo, que incorpora componentes de
> terceros con licencia no comercial. **La licencia de los pesos preentrenados no
> ha podido verificarse y se trata como riesgo abierto**: si el Cliente requiere
> certeza, se sustituyen por pesos entrenados sobre datos del Cliente.*

### Generalización — **aplicar a todo world model, no sólo a éste**

La tendencia 34 dice que lo no comercial vive en la periferia del entregable. Antes
de proponer cualquier modelo, correr la **Receta 46** sobre su repo y responder
tres preguntas, **separadas**:

1. ¿Puedo **embarcar** el modelo? → grant de las rutas de producto.
2. ¿Puedo **ejecutar** su evaluación? → grant de `eval/`, `benchmark/`, `*Score/`.
3. ¿Puedo **usar los pesos**? → grant del host de pesos. **Hoy esta KB no puede
   contestar la nº 3 para ningún modelo**, y es la más cara de las tres.

---

## 🆕 Receta 36: **Auditoría de CLAUSURA de licencias** — el gate que ningún gate anterior de esta KB detecta

**Caso de uso**: cualquier pipeline que instale modelos generativos. Es el gate
que hay que correr **antes** de los otros cinco de esta página, porque los cinco
operan dentro del repo y esta clase de restricción **no está dentro del repo**.

**El caso que la motiva, con números**: `microsoft/TRELLIS` (14.370 ★) y
`microsoft/TRELLIS.2` (13.144 ★) pasan **los tres canales** de esta KB —SPDX
`MIT`, `LICENSE` MIT estándar byte a byte, enumeración limpia— y **no son
usables comercialmente tal como se envían**. El dato está en prosa, en la
sección `## ⚖️ License` del `README.md`.

### El procedimiento, 4 pasos y ~1 minuto por repo

```bash
SLUG=microsoft/TRELLIS

# 1. Los tres canales de siempre (siguen siendo obligatorios)
gh api -X GET search/repositories -f q="repo:$SLUG" \
  --jq '.items[] | "\(.stargazers_count)★ \(.license.spdx_id) arch=\(.archived) \(.pushed_at)"'
curl -s "https://raw.githubusercontent.com/$SLUG/HEAD/LICENSE" | wc -c
gh api -X GET search/code -f q="repo:$SLUG filename:LICENSE" --jq '.items[].path'

# 2. 🆕 CANAL 4 — la clausura declarada en prosa
curl -s "https://raw.githubusercontent.com/$SLUG/HEAD/README.md" | grep -n -i "licen" 

# 3. Por cada dependencia que el paso 2 nombre, repetir 1 y 2 sobre ELLA
#    (no sobre el repo original)

# 4. 🆕 Verificar DÓNDE entra: top-level vs lazy decide si se puede evitar
curl -s "https://raw.githubusercontent.com/$SLUG/HEAD/<modulo>.py" | grep -n "^import\|^from\|    from"
```

### Lo que devolvió aplicado a los dos repos de este pass

| Paso | TRELLIS | TRELLIS.2 |
|---|---|---|
| 1. tres canales | ✅ limpio | ✅ limpio (+ `assets/hdri/license.txt` = **CC0**) |
| 2. canal 4 | *"submodules may have different licenses"* → `diffoctreerast` | *"certain dependencies operate under separate license terms"* → `nvdiffrast`, `nvdiffrec` |
| 3. resolución | **Gaussian-Splatting License** (Inria+MPII): *"CANNOT USE, EXPLOIT OR DISTRIBUTE … FOR COMMERCIAL PURPOSES"* | **NVIDIA Source Code License**: *"only may be used … non-commercially"* |
| 4. dónde entra | `mesh_renderer.py:2` **top-level**; `gaussian_render.py:58` **lazy**; `octree_renderer.py` módulo | idem |

### El veredicto operativo, que es lo que hace útil al paso 4

🟢 **Se puede usar la generación.** El README dice *"TRELLIS **models** and the
majority of the code are licensed under the MIT License"* ⇒ **los pesos son
MIT**. `setup.sh` expone `--nvdiffrast`, `--diffoctreerast`, `--kaolin` como
**flags opcionales** (no se instalan por defecto) y `trellis/renderers/__init__.py`
importa con `importlib` dentro de `__getattr__` ⇒ **nada del subárbol de
renderers se carga hasta que se nombra un renderer.**

⛔ **No se puede enviar la visualización.** Los **tres** renderers del repo
importan un paquete no comercial. No hay un cuarto.

🟢 **El sustituto de extracción de malla está medido**: `nv-tlabs/FlexiCubes`,
**Apache-2.0** (`LICENSE.txt`, 11.357 B).

### Gate de CI (copiar a toda la familia de KBs)

```bash
# falla el build si un README de dependencia nombra una licencia que no está
# en la allowlist, o si aparece una frase de prohibición comercial
for slug in $(cat pipeline-repos.txt); do
  curl -s "https://raw.githubusercontent.com/$slug/HEAD/README.md" \
  | grep -i -E "non-commercial|not for commercial|research (purposes )?only|separate license|different licenses" \
  && { echo "REVISAR CLAUSURA: $slug"; exit 1; }
done
```

> **La regla, en una línea: un proyecto no es más libre que la dependencia menos
> libre que te manda a instalar — y esa dependencia casi nunca está en un
> archivo de licencia.**

---

## 🆕 Receta 37: **Fábrica de assets 100% permisiva, con procedencia por artefacto**

**Caso de uso**: estudio que quiere generar mallas, texturas, rigs y audio con
AI y **vender el resultado**, en cualquier región, sin un abogado por asset.

**El criterio de selección es la licencia, no la calidad**, porque en esta capa
la calidad converge rápido y el grant no. Las cuatro piezas, todas verificadas
byte a byte en el cycle 14:

| Etapa | Repo | ★ | Licencia (byte-read) | Por qué ésta y no la grande |
|---|---|---|---|---|
| **Pipeline local completo** | [Bingeljell/image-to-3dlab](https://github.com/Bingeljell/image-to-3dlab) *(AssetFurnace)* | 1.244 | **Apache-2.0** (11.358 B) + `NOTICE` (1.162 B) | ⚠️ ojo al slug: el proyecto se llama *AssetFurnace* y el repo **no**. `matteodante/AssetFurnace` **no existe** (422 + 404) |
| **Orquestación → engine** | [OpenDCAI/GameFactory-3A](https://github.com/OpenDCAI/GameFactory-3A) | 1.344 | **Apache-2.0** (11.338 B) | única con salida ya resuelta a **UE5, Unity, Godot 4, Blender, three.js** |
| **SFX** | [OpenMOSS/MOSS-TTS](https://github.com/OpenMOSS/MOSS-TTS) | 4.358 | **Apache-2.0** (11.375 B) — **y el README extiende el grant a los pesos**: *"Models in MOSS-TTS Family are licensed under the Apache License 2.0"* | ⛔ `audiocraft` (23.943 ★) y ⛔ `YuE` (13.656 ★) tienen **pesos CC BY-NC 4.0** |
| **Música** | [ace-step/ACE-Step](https://github.com/ace-step/ACE-Step) | 5.095 | **Apache-2.0** | idem |
| **Extracción de malla** | [nv-tlabs/FlexiCubes](https://github.com/nv-tlabs/FlexiCubes) | 871 | **Apache-2.0** (11.357 B) | sustituye el camino de render no comercial de TRELLIS |
| **Interfaz de agente** | [HamSanders/soundbox](https://github.com/HamSanders/soundbox) | 239 | **MIT + addendum de pesos** (1.713 B) | **21 herramientas MCP** para Claude Code / Cursor; **no redistribuye pesos**: los baja con la cuenta HF del operador |

### ⚠️ Las dos piezas que NO se pueden cerrar hoy, dicho explícitamente

- **Auto-rigging.** [UniRig](https://github.com/VAST-AI-Research/UniRig) (2.189 ★)
  es MIT en la raíz y lleva **GPL-3.0** en `src/model/michelangelo/` (35.148 B),
  importado **sin condición** por `src/model/parse_encoder.py`, que construye
  los encoders de sus dos modelos. [MDM](https://github.com/GuyTevet/motion-diffusion-model)
  (4.154 ★) es MIT pero exige los body models **SMPL**, bajados de un Google
  Drive **sin autenticación** en lugar del portal de MPI con aceptación de
  licencia.
  ⇒ **Acción**: usar el auto-rig de `image-to-3dlab` y **verificar en el pass
  siguiente qué usa internamente** — si usa UniRig, hereda la GPL-3.0.
- **Los checkpoints de Stability** (`stable-audio-tools`, MIT en el código) van
  por licencia aparte y **no se leyeron**. Gap declarado.

### El entregable que distingue esta receta: **procedencia por asset**

`image-to-3dlab` ya escribe *"la licencia de cada asset"* que produce. Eso se
extiende a un manifiesto por artefacto —modelo, versión de pesos, licencia de
esos pesos, prompt, grado de intervención humana— que sirve **tres veces**:

1. **China**: el etiquetado de contenido generado cae sobre el artefacto final.
2. **UE**: es el insumo del inventario del Art. 50.
3. **Due diligence**: es lo que pregunta un comprador o un publisher.

```json
{
  "asset": "props/barrel_01.glb",
  "generator": {"repo": "Bingeljell/image-to-3dlab", "license": "Apache-2.0"},
  "model": {"name": "<modelo>", "weights_license": "<leída, no inferida>"},
  "human_in_the_loop": "retopo + UV manual",
  "territory_restrictions": "ninguna"
}
```

---

## 🆕 Receta 38: **Elegir generador 3D por jurisdicción de publicación, no por benchmark**

**Caso de uso**: el cliente ya decidió generar assets con AI. La pregunta no es
cuál es mejor — es **cuál tiene grant donde el cliente vende**.

### La tabla de decisión, con la causa medida en cada ⛔

| Opción | Licenciante | NA | EMEA | APAC | LATAM | Causa del bloqueo (byte-read) |
|---|---|---|---|---|---|---|
| `TRELLIS` / `TRELLIS.2` — **generación** | Microsoft | ✅ | ✅ | ✅ | ✅ | MIT, pesos incluidos |
| …sus **renderers** | NVIDIA / Inria+MPII | ⛔ | ⛔ | ⛔ | ⛔ | *"non-commercially … research or evaluation purposes only"* / *"CANNOT USE … FOR COMMERCIAL PURPOSES"* |
| `Hunyuan3D-2.1` | Tencent | ✅ | ⛔ | ⛔ **sólo Corea** | ✅ | *"THIS LICENSE AGREEMENT DOES NOT APPLY IN THE EUROPEAN UNION, UNITED KINGDOM AND SOUTH KOREA"* (17.914 B) |
| `stable-fast-3d` | Stability AI | ⚠️ | ⚠️ | ⚠️ | ⚠️ | Stability Community License (11.727 B) + **22 meses sin push** |
| `image-to-3dlab`, `GameFactory-3A` | — | ✅ | ✅ | ✅ | ✅ | Apache-2.0, sin cláusula territorial |

### Las tres reglas que salen de la tabla

1. **El territorio es un eje de partición del grant** (regla nº14 del cycle 10),
   y es el único que **no se puede resolver leyendo el repo**: hay que saber
   dónde publica el cliente. Con `Hunyuan3D-2.1` son ya **4 repos de Tencent /
   2 líneas de producto / 0 contraejemplos** ⇒ **asumir exclusión territorial
   en cualquier release abierto de Tencent-Hunyuan** y verificar la primera
   línea del archivo antes de evaluar nada.
2. **EMEA pierde las dos rutas no-Apache a la vez**, y una de las dos la pierde
   por una licencia **de origen europeo** (Inria + MPII, en registro ante la
   APP). Para un engagement europeo, el stack Apache/MIT de la Receta 37 no es
   una preferencia: es el conjunto completo.
3. **LATAM es la única región sin bloqueo de ningún tipo en este cuadro.** No
   por mérito local —la medición de este pass encontró **un** repo
   hispanohablante de generación de assets, de **1 ★ y GPL-3.0**
   ([facundoAlmon/AssetCrisol](https://github.com/facundoAlmon/AssetCrisol))—
   sino porque ninguna de las cláusulas territoriales la nombra. Es un
   argumento **estructural y verificable**, no de coste.

### Checklist de kickoff (5 preguntas, antes de escribir código)

1. ¿En qué jurisdicciones publica el cliente? → descarta filas enteras.
2. ¿El entregable es el asset o la herramienta? → decide si el gate de la
   Receta 32 (tipo de entregable) también aplica.
3. ¿Se envía código de visualización? → si sí, **Receta 36 obligatoria**.
4. ¿Los pesos se redistribuyen o los baja el operador? → el patrón `soundbox`
   traslada la obligación por escrito.
5. ¿Hay manifiesto de procedencia por asset? → si no, se construye ahora, no
   cuando lo pida el publisher.

---

## 🆕 Receta 33: **El dato de edad como parámetro de runtime** — un solo campo, tres jurisdicciones que piden cosas incompatibles

**Problema.** La Receta 18 clasifica audiencia por **título** (PEGI/ESRB) para
decidir si aplica el Art. 50, y cruza con Connecticut PA 26-15, que usa la edad
del **usuario** para decidir la **cadencia** del recordatorio. Falta el tercer
régimen, que es el que hoy tiene más fuerza ejecutoria: **Texas SB 2420**,
vigente desde el 1-ene-2026 y **exigible desde el 4-jun-2026** (el Quinto
Circuito levantó la cautelar; la Corte Suprema dejó que Texas siga ejecutando).

Y los tres **no se apilan**: uno de ellos obliga a **borrar** lo que otro obliga
a **conservar**.

| Régimen | La edad decide… | Fuente del dato | Retención |
|---------|-----------------|-----------------|-----------|
| **EU — AI Act Art. 50** | **si** hay obligación de etiquetar | clasificación del **título** (PEGI) | no aplica (no es dato de usuario) |
| **Connecticut — PA 26-15** | **con qué frecuencia** recordar que es una AI (≥1/h menores, ≥1/3 h adultos) | el **usuario** | implica conocerla **durante la sesión** |
| **Texas — SB 2420** | **si** se permite descarga y compra in-app | la **tienda**, en 4 categorías (<13 · 13-15 · 16-17 · adulto) | 🔴 **obligación de BORRAR tras verificar** |

🔴 **El conflicto, en una línea: un build que persiste `user.age` para cumplir
Connecticut incumple el deber de borrado de Texas.**

### Implementación

**Paso 1 — no persistir la edad. Persistir dos derivados y un reloj.**

```
// ❌ MAL — viola el deber de borrado de TX y guarda más de lo necesario
player.age = 14;
player.birthdate = "2012-03-01";

// ✅ BIEN — se conserva lo mínimo derivado, se descarta la fuente
session.ageBand       = "13_15";   // categoría de SB 2420, no la edad
session.isMinor       = true;      // único booleano que usa la lógica de producto
session.reminderEvery = 3600;      // segundos — Connecticut como PARÁMETRO
// el dato de edad recibido de la tienda se descarta en el mismo flujo
```

La categoría de SB 2420 es **ya** una generalización: el régimen que exige
borrarla no exige borrar la decisión que se tomó con ella. Lo que no puede
quedar guardado es la **edad o la fecha de nacimiento**.

**Paso 2 — la cadencia es configuración, no constante de build.** Es el mismo
punto de la Receta 18 (paso 2) llevado a su consecuencia: `reminderEvery` y los
flags de gating viajan en configuración por jurisdicción, no compilados. Un
título que publica el mismo día en EU, Texas y Connecticut hace **un build** y
**tres configuraciones**.

**Paso 3 — poner la verificación delante del CANAL SOCIAL, no del producto.**
Es el diseño de referencia que ya validó la plataforma más grande del segmento:
Roblox exige verificación de edad **para acceder al chat**, globalmente, desde
enero de 2026 — el acceso al juego sigue abierto. Traducido a arquitectura:

```
juego           -> abierto
chat / voz      -> requiere ageBand verificada
NPC con LLM     -> requiere ageBand (decide persona, filtros y cadencia de aviso)
compras in-app  -> requiere ageBand + consentimiento parental (SB 2420)
```

**Paso 4 — reclamar el puerto seguro.** SB 2420 prevé **safe harbor** para quien
siga de buena fe estándares de industria ampliamente adoptados de verificación de
edad y consentimiento parental. ⇒ **el entregable incluye el documento que dice
qué estándar se siguió y desde cuándo**, no sólo el código. Es el mismo principio
de la Receta 18, paso 4: lo que protege al cliente es haber decidido con
fundamento en una fecha.

**Paso 5 — dimensionar dónde aplica.** Leyes de age assurance a nivel
tienda/dispositivo: **Texas, Utah, Louisiana, Alabama, California**. Sanción en
TX: **prácticas comerciales engañosas** bajo ley estatal.

**Esfuerzo:** 3-5 días si la edad ya entra al sistema; 2-3 semanas si hay que
abrir el flujo de la tienda. **Encaja con:** va **después** de la Receta 18 y
**antes** de la 13. **Reutiliza** el mismo parámetro que la 18 promovió a entrada.

---

## 🆕 Receta 34: **Capa de moderación de chat y NPC, 100% MIT/Apache** — la que hay que construir porque no existe para comprar

**Caso de uso**: juego con chat entre jugadores y/o NPCs conversacionales que
debe operar en NA y EU después de las fechas de la Receta 33, sin mandar la voz
ni el texto de menores a una API de terceros.

**Por qué se construye y no se integra, con la medición al lado:** la consulta
`toxicity detection chat moderation` ordenada por ★ devuelve, completa, repos de
**0 a 8 ★**. **No hay plataforma open source líder de moderación de chat de
juego.** Hay cuatro primitivas buenas, genéricas de LLM, y el trabajo es
ensamblarlas y agregarles lo específico de juego.

### Las piezas, con licencia leída byte a byte

| Pieza | Repo | ★ | Licencia | Rol en la cadena |
|-------|------|---|----------|------------------|
| Clasificador de toxicidad | [unitaryai/detoxify](https://github.com/unitaryai/detoxify) | 1.320 | **Apache-2.0** (11.357 B) | corre **local**, multilingüe, sin API. Primer filtro, barato. |
| Rieles conversacionales | [NVIDIA-NeMo/Guardrails](https://github.com/NVIDIA-NeMo/Guardrails) | 7.544 | **Apache-2.0** (SPDX, 654 B) | define de qué habla el NPC y qué hace si lo sacan del riel |
| Validadores de salida | [guardrails-ai/guardrails](https://github.com/guardrails-ai/guardrails) | 7.635 | **Apache-2.0** (11.357 B) | esquema, PII, política propia sobre lo que devuelve el LLM |
| Guardrail de agente | [meta-llama/PurpleLlama](https://github.com/meta-llama/PurpleLlama) → **`LlamaFirewall/`** | 4.488 (repo) | 🔴 **MIT** (1.088 B) — **la raíz NO**, ver abajo | prompt injection y acciones inseguras cuando el NPC tiene herramientas |
| STT para moderar voz | [SYSTRAN/faster-whisper](https://github.com/SYSTRAN/faster-whisper) | 26.929 | **MIT** (1.064 B) | servidor; o [whisper.cpp](https://github.com/ggml-org/whisper.cpp) (55.409 ★, MIT) **en el cliente** si la voz del menor no debe salir del dispositivo |

🔴 **La trampa de licencia de esta receta, que cuesta el proyecto si se lee mal:**
`meta-llama/PurpleLlama` tiene en la **raíz** un `LICENSE` que es la **Llama 3.2
Community License (7.669 B) — no open source**. Enumerado, el repo tiene **14
archivos de licencia y 5 grants**: `LlamaFirewall/`, `CodeShield/`,
`CybersecurityBenchmarks/` y `SensitiveDocClassification/` son **MIT verbatim**,
y **todos los pesos** (`Llama-Guard*`, `Llama-Prompt-Guard-2*`) son Llama 2 / 3.2
/ 4 Community. ⇒ **se usa el framework MIT con un modelo propio o abierto; no se
usan los pesos de Meta.**

### Cableado

```
          jugador escribe                    jugador habla
                |                                  |
                |                      whisper.cpp (cliente, MIT)
                |                      o faster-whisper (servidor, MIT)
                v                                  v
        +---------------------------------------------------+
        | 1. detoxify (Apache-2.0) — local, ~ms, multilingüe |
        |    toxicidad > umbral  -> cortar aca, no gastar LLM|
        +---------------------------------------------------+
                |  pasa
                v
        +---------------------------------------------------+
        | 2. NeMo Guardrails (Apache-2.0) — rieles Colang    |
        |    tema fuera de alcance -> respuesta de desvio    |
        |    ageBand (Receta 33) -> elige el riel            |
        +---------------------------------------------------+
                |  pasa
                v
             LLM del NPC  (+ LlamaFirewall MIT si el NPC usa herramientas)
                |
                v
        +---------------------------------------------------+
        | 3. guardrails-ai (Apache-2.0) — valida la SALIDA   |
        |    PII, esquema, politica propia del titulo        |
        +---------------------------------------------------+
                |
                v
        texto final -> (Receta 35: voz y localizacion)
```

**Las dos reglas de diseño que hacen que esto sea barato:**

1. **El filtro barato va primero.** detoxify corre local en milisegundos; el LLM
   cuesta. Cortar en el paso 1 ahorra la llamada completa, y en un chat de juego
   la mayoría de lo que hay que cortar es obvio.
2. **`ageBand` de la Receta 33 selecciona el riel**, no se reimplementa acá. Un
   solo parámetro recorre las dos recetas.

### Lo que hay que construir encima (no viene con ninguna pieza)

- **Jerga y ofuscación de juego**: `l33t`, separadores, abreviaturas de partida.
  detoxify no las cubre: hay que normalizar antes de clasificar.
- **Contexto de partida**: *"te voy a matar"* en un shooter no es lo mismo que en
  el lobby. El riel necesita el estado del juego como entrada.
- **Multi-idioma con el perfil real del título**: para LATAM y APAC el inglés es
  minoría; detoxify es multilingüe pero hay que medir por idioma, no asumirlo.

**Esfuerzo:** 2-3 semanas para la cadena funcionando con telemetría; el trabajo
largo es el corpus de jerga por título y por idioma. **Encaja con:** **después**
de la Receta 33 (necesita `ageBand`), **antes** de la 35.

---

## 🆕 Receta 35: **Diálogo → voz → idioma, con la licencia decidida ANTES del motor**

**Caso de uso**: un título con NPCs conversacionales que hay que escribir, decir
en voz alta y entregar en varios idiomas — sin que una licencia aparezca a
mitad del sprint.

**Por qué esta receta existe:** en esta capa, **la elección de motor arrastra la
licencia**. No es una consideración legal posterior: es la primera decisión
técnica.

### Paso 1 — Autoría: el LLM no escribe al juego, escribe a un formato

```
LLM  ->  .ink / .yarn  ->  compilador  ->  runtime en el juego
```

Meter la salida del LLM en un formato de diálogo autorado **antes** del runtime
da tres cosas gratis: es **revisable por un narrative designer**, es
**diffeable en git**, y es **determinista en ejecución** — lo que a su vez
simplifica el registro del Art. 50, porque el contenido queda fijado en build y
no generado en vivo.

| Formato | Repo | ★ | Licencia | Cuándo |
|---------|------|---|----------|--------|
| **ink** | [inkle/ink](https://github.com/inkle/ink) | 5.029 | **MIT** (1.067 B) | narrativa ramificada con estado; compila a JSON ⇒ el LLM genera JSON válido directamente |
| **Yarn** | [YarnSpinnerTool/YarnSpinner](https://github.com/YarnSpinnerTool/YarnSpinner) | 2.876 | **MIT** (1.138 B) | diálogo tipo guion con comandos al juego; más legible para escritores |

### Paso 2 — 🔴 El gate de motor: **con Yarn Spinner, el port decide tu licencia**

| Motor | Repo del port | ★ | Licencia | Veredicto |
|-------|---------------|---|----------|-----------|
| Unity | `YarnSpinner-Unity` | 854 | **MIT** | ✅ |
| Godot **C#** | `YarnSpinner-Godot` | 218 | **MIT** (en rama `master`) | ✅ |
| Rust / Bevy | `YarnSpinner-Rust` | 299 | **MIT** (dual con Apache) | ✅ |
| Godot **GDScript** | `YarnSpinner-Godot-GDScript` | 99 | 🔴 **YSPL 1.0** | ⚠️ condicionado |
| Unreal | `YarnSpinner-UnrealEngine` | 52 | 🔴 **YSPL 1.0** | ⚠️ condicionado |

**Qué permite y qué prohíbe la YSPL 1.0, para decidir sin abogado:**

- ✅ **Hacer y vender el juego del cliente: permitido.**
- ✅ **Globant como contratista: permitido y nombrado** — la licencia define
  `"Contractor"` incluyendo *contractor, freelancer, or porting service provider*.
- ⛔ **Construir un acelerador de diálogo reutilizable entre cuentas: prohibido.**
  Define `"Competing Product"` como toda herramienta cuyo propósito primario sea
  habilitar a terceros a crear diálogo ramificado o narrativa no lineal.
  **Excepción escrita:** herramienta **interna de una sola Legal Entity, no
  distribuida** ⇒ un tool interno de un estudio sí se puede.
- 🔴 **Prohíbe alimentar el código a sistemas de entrenamiento de AI.** Permite
  usar herramientas asistidas para construir el juego, pero exige esfuerzo
  razonable para que **no envíen el fuente a entrenamiento**.

> 🔴 **Regla operativa, y es la primera de esta KB sobre el tooling propio:** si
> el proyecto toca un árbol con YSPL, **la configuración de retención de los
> agentes de código es parte del alcance técnico**, y se verifica al inicio
> (settings del proveedor), no al cierre. Si el programa de AI-assisted
> development del cliente no permite desactivar retención, **se elige `ink` (MIT)
> o el port C# de Godot**, y la decisión se registra.

### Paso 3 — Voz, con la lista de descartes explícita

| Rol | Elección | Licencia | Por qué no el más grande |
|-----|----------|----------|--------------------------|
| STT en cliente | [whisper.cpp](https://github.com/ggml-org/whisper.cpp) (55.409 ★) | **MIT** | — |
| STT en servidor | [faster-whisper](https://github.com/SYSTRAN/faster-whisper) (26.929 ★) | **MIT** | — |
| **TTS** | [SWivid/F5-TTS](https://github.com/SWivid/F5-TTS) (15.633 ★) | **MIT** (1.068 B) | `fish-speech` (33.825 ★) **bloquea por facturación del CLIENTE**; `rhasspy/piper` MIT está **archivado**; `piper1-gpl` es **GPL-3.0**; `coqui-ai/TTS` lleva **27 meses sin commits**. |

🔴 **La cláusula de fish-speech que hay que saber de memoria**, porque define el
modelo de entrega y no sólo la licencia: la Fish Audio Community License 1.0
(13-oct-2026) permite uso comercial sólo con `Annual Revenue` < **US$ 500.000**,
y **si operás el software para un tercero como contratista o integrador, el
umbral se mide sobre ESE tercero**. Dos salidas escritas:

- ✅ **entregar sólo el output** (archivos de audio: doblaje, voiceover) **no**
  queda sujeto al umbral del cliente ⇒ **modelo de servicio de producción de voz**;
- ⛔ **entregar el software** a un estudio que factura > US$ 500k ⇒ **sin licencia**.

Y el carve-out útil: *"game or app characters"* está **explícitamente excluido**
de la definición de `Hosted Service` ⇒ para un juego el problema nunca es el
hosting; es la facturación.

### Paso 4 — Idioma, con el límite del binario como criterio

| Pieza | Repo | Licencia | Dónde vive |
|-------|------|----------|------------|
| Runtime de i18n | [i18next](https://github.com/i18next/i18next) (8.666 ★) | **MIT** | **dentro** del juego ⇒ tiene que ser permisiva |
| Plataforma de traducción | [Weblate](https://github.com/WeblateOrg/weblate) (6.178 ★) | **GPL-3.0** | **fuera**: servicio del equipo de loc ⇒ no contamina el entregable |
| Traducción en vivo / análisis | [LunaTranslator](https://github.com/HIllya51/LunaTranslator) (14.258 ★) | **GPL-3.0** | **fuera**: herramienta de escritorio del equipo |

> **📐 La pregunta correcta no es "¿qué licencia tiene?" sino "¿cruza el límite
> del binario que se le entrega al jugador?".** Una GPL del lado del equipo es
> gratis; la misma GPL linkeada en el build es un bloqueo. Es el criterio que
> salva Weblate y LunaTranslator, y el que hunde a `piper1-gpl`.

### El stack completo, 100% permisivo

```
ink (MIT) o YarnSpinner núcleo (MIT)
   + port MIT del motor  [Unity | Godot C# | Rust]     <- Paso 2
   + whisper.cpp / faster-whisper (MIT)                <- STT
   + F5-TTS (MIT)                                      <- TTS
   + detoxify + NeMo Guardrails + guardrails-ai (Apache-2.0)  <- Receta 34
   + LlamaFirewall (MIT, subcarpeta)                   <- Receta 34
   + i18next (MIT) en runtime, Weblate (GPL, aparte)   <- Paso 4
   + ageBand como parámetro                            <- Receta 33
```

**Cero licencias condicionadas, cero copyleft cruzando el límite del binario,
cero pesos bajo licencia bespoke.** Todas las filas leídas byte a byte en este
pass.

**Esfuerzo:** 1 día el gate de licencia (pasos 1-2 y 4), que es lo que evita el
rediseño; 3-4 semanas el pipeline completo. **Encaja con:** **después** de la 33
y la 34.

---

## 🆕 Receta 29: **NPC conversacional sin topología de datos** — SLM on-device que apaga la obligación en vez de documentarla

**El problema que resuelve.** Las Recetas 7 y 13 enseñan a *cumplir*: marcar el
contenido (Art. 50(2) UE) y recordar al jugador que habla con una AI por reloj
de sesión (NA). Las dos asumen el patrón *"el NPC llama a un LLM hosteado"*. El
cycle 10 midió que la obligación que más duele en NA no es de transparencia sino
de **topología de datos** — a dónde va la conversación del jugador. **Esta receta
elimina el hecho en vez de controlarlo: si nada sale del equipo, no hay ruta de
datos que auditar.**

**Piezas, todas verificadas este pass:**

| Pieza | Repo / producto | Licencia (medida) | Rol |
|---|---|---|---|
| SLM on-device | [QwenLM/Qwen3](https://github.com/QwenLM/Qwen3) (27.758 ★) | 🟡 **badge `NONE`, enumeración = 0** → Apache-2.0 **declarado en `README.md` §*License Agreement*** | El modelo que corre en la GPU del jugador. **NVIDIA publicó soporte de Qwen3-8B en ACE para despliegue on-device** |
| Runtime de NPC | NVIDIA **ACE** (+ línea Nemotron: 3 Nano 4B; 3.5 Bolt E4B sobre RTX Spark) | Apache-2.0 en el SDK abierto — ⚠️ ver `agents/top.md`, los grants de ACE están partidos por componente | ASR + decisión + TTS + animación |
| Engine | Godot (MIT) / Unreal / Unity | ver tabla de engines | Host |
| Memoria de NPC | SQLite local / `beehave` (MIT, 3.330 ★) | MIT | Estado persistente **sin servidor** |
| Marcado | `contentauth/c2pa-rs` (MIT **o** Apache-2.0) | permisiva | Sólo para el contenido que **se embarca**, no para lo efímero en runtime (ver Receta 27) |

**Cableado:**

```
Jugador ──voz/texto──> [ ASR local (ACE) ]
                              │
                              v
                  [ SLM 4–8B on-device  ]  ← Qwen3-8B o Nemotron Nano
                   (contexto + persona)      ningún byte sale del equipo
                              │
                              v
          [ memoria local SQLite ] ──> [ árbol de comportamiento (beehave) ]
                              │
                              v
                      [ TTS local (ACE) ] ──> animación facial
```

**Qué verificar antes de prometerlo (y esto es la mitad de la receta):**

1. ⚠️ **Leer la *model card* del tamaño exacto que se embarca.** Esta KB ya midió
   que **el grant se parte por variante de modelo**. 8B ≠ 235B ≠ los `-Instruct`.
   ⛔ **Este pass NO pudo leerlas: `huggingface.co` no resuelve en esta sesión**
   (dos sondas). Lo verificado es que el propietario declara Apache-2.0 en su
   propio README.
2. ⚠️ **Diseñar las DOS rutas.** Un jugador con GPU vieja cae a ruta hosteada —
   y para ese segmento **la obligación de topología de datos vuelve entera**. El
   *fallback* no es un detalle de performance: es el 50% del análisis legal.
3. ⚠️ **Dependencia de hardware:** la ruta ACE es NVIDIA. Para consola o móvil,
   esta receta no aplica tal cual.
4. 🟢 **Lo que sí sigue aplicando:** el Art. 50(2) europeo **no** desaparece por
   correr local — la obligación de *informar que se habla con una AI* es de
   transparencia, no de datos. On-device apaga la obligación **de NA**, no la
   **de la UE**. Decir lo contrario en una sala es un error caro.

**Para quién se vende:** cliente con jugadores en EE.UU. + NPC conversacional en
el roadmap. Es la mitigación **más barata** del catálogo, porque no es un control
que haya que operar: es la ausencia del hecho que dispara el control.

---

## 🆕 Receta 30: **Triage de licencia cuando la enumeración devuelve CERO** — el árbol de decisión que faltaba

**Por qué existe.** La Receta 17 enseñó a enumerar en vez de sondear. Este pass
encontró que **la enumeración tiene un resultado ambiguo que la receta no
cubría**: `total_count = 0`. Tres repos lo devolvieron, y los tres veredictos
son distintos. Resolverlo mal, en cualquiera de las dos direcciones, es caro.

**El árbol, con los tres casos medidos:**

```
gh api -X GET search/code -f q='repo:<slug> filename:LICENSE' --jq '.total_count'
│
├─ > 0 ──> leer TODOS los archivos por raw (Receta 17). Ojo: el de la raíz
│          puede ser MÁS restrictivo que los vendorizados.
│          → LumioGames/LGUI: 8 archivos. Raíz PolyForm Shield (condicional),
│            adentro FairyGUI MIT ×3 + puerts BSD-3. LA SALIDA ESTABA ADENTRO.
│
└─ = 0 ──> ⚠️ NO escribir "sin licencia" todavía. Leer el README:
           curl -s https://raw.githubusercontent.com/<slug>/HEAD/README.md \
             | grep -n -i -A3 'license\|licence\|apache\|MIT'
           │
           ├─ el README DECLARA un grant ──> 🟢 el grant existe, alojado afuera.
           │   → QwenLM/Qwen3 (27.758 ★): README §"License Agreement" →
           │     Apache-2.0, archivos en Hugging Face.
           │     ⚠️ Anotar DÓNDE vive y verificarlo ahí antes de producción.
           │
           └─ el README NO dice nada ──> 🔴 sin grant = todos los derechos
               reservados. NO USAR.
               → Yuki001/game-dev-skills (123 ★): README 200 OK, 5.774 B,
                 cero menciones de licencia.
               → TaloDev/hosting (36 ★): el producto es MIT, ESTE repo no.
```

> 📐 **La regla, para copiar a toda la familia de KBs:** *enumeración = 0* es
> **ambiguo, no negativo**. Tiene tres desenlaces —grant alojado afuera / grant
> inexistente / repo auxiliar que nadie licenció— y **sólo el README los
> distingue**. Un falso negativo acá cuesta un fundacional de 27.758 ★; un falso
> positivo otorga lo que nadie otorgó.

**Control de la organización, y por qué es obligatorio.** El mismo pass midió
que **la licencia no se hereda de la organización** (regla del cycle 11, ahora
confirmada en un segundo caso): en `TaloDev`, **cuatro repos son MIT y el quinto
no tiene grant** — y el quinto es justamente el *self-hosting starter pack*.

```bash
# enumerar la ORGANIZACIÓN, no sólo el repo que te pasaron
gh api -X GET search/repositories -f q='user:<org>' -f per_page=30 \
  --jq '.items[]|.full_name+" ★"+(.stargazers_count|tostring)+" "+(.license.spdx_id//"NONE")'
# luego, para cada uno con NONE: el árbol de arriba
```

> 🔴 **El sesgo que esto corrige:** se enumera el repo **que te mandaron**, que
> casi siempre es el producto. El gate suele estar en el **repo auxiliar** —
> el runbook, el instalador, los fixtures — que nadie mira y nadie licenció.

---

## 🆕 Receta 31: **Stack indie 100% MIT con Talo** — y el repo que hay que dejar afuera

**Para quién.** Indie / mid-core que necesita cuentas, saves en la nube,
leaderboards, stats y analytics, quiere **self-hosting real** y no puede aceptar
Apache-2.0 ni un tier cerrado. Es el escalón por debajo de Nakama
(Apache-2.0, 13.763 ★), con menos superficie y **licencia más permisiva**.

| Pieza | Repo | Licencia (bytes, enumerada) | ★ |
|---|---|---|---|
| Servidor | [TaloDev/backend](https://github.com/TaloDev/backend) | 🟢 **MIT** (1.074) — 1 archivo | 105 |
| Cliente Godot | [TaloDev/godot](https://github.com/TaloDev/godot) | 🟢 **MIT** (1.074) — **4 archivos, 4 MIT**: `addons/talo` 1.074, `addons/gdUnit4` 1.069, `addons/GDQuest_GDScript_formatter` 1.072 | **251** |
| Cliente Unity | [TaloDev/unity](https://github.com/TaloDev/unity) | 🟢 **MIT** (1.074) — 2 archivos | 43 |
| Consola | [TaloDev/frontend](https://github.com/TaloDev/frontend) | 🟢 **MIT** (1.074) | 21 |
| ⛔ **Despliegue** | [TaloDev/hosting](https://github.com/TaloDev/hosting) | 🔴 **SIN GRANT — enumeración = 0** | 36 |
| Engine | Godot | 🟢 MIT | — |
| Orquestación (si hace falta escalar) | [agones-dev/agones](https://github.com/agones-dev/agones) | 🟢 Apache-2.0, **gobernanza CNCF desde mar-2026** | 7.1k |

**Cableado:**

```
Godot (MIT) ──addons/talo (MIT)──> TaloDev/backend (MIT, self-hosted)
                                           │
                        ┌──────────────────┼──────────────────┐
                        v                  v                  v
                 auth + players      leaderboards        event tracking
                 cloud saves         stats/live config   → export CSV
                                           │
                                   TaloDev/frontend (MIT): consola
```

> 🔴 **La instrucción operativa que esta receta existe para dar:**
> **NO copiar `TaloDev/hosting` al repo del cliente.** Es el *"self-hosting
> starter pack"* — exactamente lo que un equipo clonaría primero — y **no tiene
> ningún archivo de licencia** (cero por enumeración, no por sonda fallida).
> Desplegar `TaloDev/backend`, que **sí** es MIT, con IaC propia (Compose o
> Helm escritos por el equipo). El producto es MIT; **el runbook no está
> otorgado.**

**Corrección publicada respecto de lo que esta KB decía:** *"Talo: MIT completo,
sin tier cerrado"* es cierto **del producto** y **falso de la organización**. Y
el repo más estrellado de Talo **no es el backend: es el plugin de Godot**
(251 ★ vs 105 ★) — la puerta de entrada real, que esta KB no listaba.

**Cuándo NO usar Talo:** si hace falta matchmaking, realtime authoritative o
chat social a escala, el escalón correcto sigue siendo **Nakama** (Apache-2.0).
Talo cubre *metadata del jugador*, no *simulación multijugador*.

---

## 🆕 Receta 32: **Gate de "tipo de entregable"** — el eje de licencia que ningún gate anterior de esta KB detecta

**Por qué ninguna receta previa lo ve.** Los gates de las Recetas 15 (por
región) y 16 (packs de skills) preguntan *qué licencia tiene* y *a dónde va*.
**PolyForm Shield pregunta algo que ningún gate mira: QUÉ TIPO DE PRODUCTO estás
construyendo.** `LumioGames/LGUI` permite uso comercial dentro de *"tus propios
juegos, aplicaciones, sitios y productos"* y prohíbe que el resultado sea
*"un UI framework, UI toolkit, UI editor, UI asset pipeline **o game development
tool**"*.

> 🔴 **Por qué esto es específicamente peligroso para Globant:** el modelo de un
> estudio de servicios es **convertir el trabajo de cuenta en activo
> reutilizable**. Una licencia que permite lo primero y prohíbe lo segundo **se
> rompe en el momento exacto en que el proyecto sale bien** — cuando alguien
> propone empaquetar lo que funcionó y revenderlo. Y para entonces el código ya
> está adentro.

**Implementación — el campo nuevo es `deliverable_type`:**

```bash
# gate-deliverable.sh — corre en CI junto a los gates de las Recetas 15 y 16
# El repo declara qué ES, no sólo a dónde va.
#   DELIVERABLE_TYPE = client_game | internal_accelerator | product | tool

CONDITIONAL_LICENSES="PolyForm|Shield|Business Source|BUSL|Commons Clause|Elastic License"

hits=$(grep -rilE "$CONDITIONAL_LICENSES" --include='LICENSE*' --include='*.md' . || true)

if [ -n "$hits" ]; then
  case "$DELIVERABLE_TYPE" in
    client_game)
      echo "🟡 AVISO: licencia condicional presente. Permitida en un juego de cliente."
      echo "$hits"
      echo "   → registrar en el BOM del entregable: NO puede migrar a un acelerador."
      ;;
    internal_accelerator|product|tool)
      echo "🔴 BLOQUEO: licencia condicional en un entregable de tipo '$DELIVERABLE_TYPE'."
      echo "$hits"
      echo "   → PolyForm Shield nombra 'game development tool' como producto competidor."
      echo "   → Opción A: pedir licencia separada al titular."
      echo "   → Opción B: sustituir por el PADRE permisivo si está vendorizado (ver abajo)."
      exit 1
      ;;
  esac
fi
```

**Opción B, que es la que casi siempre resuelve — buscar el padre adentro:**

```bash
# enumerar TODOS los archivos de licencia del repo condicional
gh api -X GET search/code -f q='repo:<slug> filename:LICENSE' --jq '.items[].path'
# LumioGames/LGUI → 8 archivos. La raíz es PolyForm Shield.
# Adentro: FairyGUI MIT (1.086 / 1.092 / 1.099 B) y puerts BSD-3 (3.473 B).
```

| Si necesitás | Tomá | Licencia | Costo del cambio |
|---|---|---|---|
| UI de juego **dentro de un acelerador Globant** | [fairygui/FairyGUI-unity](https://github.com/fairygui/FairyGUI-unity) (2.978 ★) | 🟢 **MIT**, sin condiciones | ⚠️ ~9 meses sin push, y el **editor** ([fairygui/FairyGUI-Editor](https://github.com/fairygui/FairyGUI-Editor), 802 ★) **no tiene grant** |
| UI de juego **en un juego de cliente** | [LumioGames/LGUI](https://github.com/LumioGames/LGUI) (148 ★) | 🟡 PolyForm Shield — **permitido acá** | Mantenido (push 2026-11-19), pero queda marcado en el BOM |

> 📐 **Regla general: un grant condicional en la raíz no es un rechazo — es una
> instrucción para enumerar.** El componente permisivo que de verdad se necesita
> puede estar vendorizado adentro. El trade-off es casi siempre el mismo: **más
> libre, menos mantenido.**

---

## 🆕 Receta 27: **Memo de factibilidad del Art. 50(2) para contenido generado en runtime** — la salida que la Receta 7 no tenía

**Problema que resuelve:** la Receta 7 manda a marcar. La Receta 22 tria provider
vs deployer. Ninguna de las dos resuelve el caso **más caro y más común en un
juego con LLM**: ¿cómo se marca de forma legible por máquina **cada línea de
diálogo que un NPC genera en tiempo real y que nadie graba**?

🔵 **La base (secundaria — ver `intel/market.md` cycle 11):** las guías finales
del Art. 50 (adoptadas **5-nov-2026**, `C(2026) 7702 final`) **amplían las
excepciones** al marcado del 50(2). El contenido **efímero generado en tiempo
real** —**videojuegos y VR citados explícitamente**— queda exento **sólo si se
cumplen las dos condiciones**:

| Condición | Quién la demuestra | Forma del entregable |
|---|---|---|
| (a) El marcado **no es técnicamente factible** | **El cliente**, por escrito | **Este memo** |
| (b) El usuario es informado del origen AI **a nivel de experiencia o de sesión** | El cliente, en producto | **Cambio de UI/UX**, no de pipeline |

⚠️ **Es una excepción condicionada: si falla (a) o falla (b), no hay excepción.**
Y (b) **no es gratis** — el aviso al jugador no es lo que se ahorra, es lo que se
paga a cambio del marcado.

### Implementación

**Paso 1 — Separar el catálogo en dos pilas** (una consulta al equipo técnico):

| Pila | Definición operativa | Ruta |
|---|---|---|
| **Pipeline** | El contenido AI **se genera antes del build y se embarca** (arte, voces, textos, variantes) | **Receta 7** (marcado), con el recorte del paso 2 |
| **Runtime efímero** | Se genera **durante la sesión**, **no se persiste** y no se redistribuye | **Esta receta** |

🔴 **El caso borde que decide el proyecto:** si el juego **guarda** lo generado
(log de chat, screenshot compartible, clip, UGC exportable), **deja de ser
efímero** y vuelve a la pila de pipeline. **La pregunta es "¿sale del proceso?",
no "¿se generó en vivo?".**

**Paso 2 — Recortar la pila de pipeline con la excepción de circuito cerrado.**
🔵 Las salidas de **flujos de producción en circuito cerrado** (cine, animación,
**juegos**, publicidad) **no se marcan en etapas intermedias**: sólo **la salida
final** debe ir marcada y ser detectable. ⇒ los *concepts*, las variantes
descartadas y los pasos intermedios **salen del inventario**. En un pipeline de
assets real eso es la mayor parte de los artefactos.

**Paso 3 — Redactar el memo de infactibilidad.** Estructura mínima, una página
por sistema:

```
1. Sistema y salida      NPC conversacional X; texto generado por LLM, por turno
2. Volumen y vida útil   ~N turnos/sesión; no persistido; no exportable
3. Técnicas evaluadas    watermark de texto | metadata C2PA | marca en render
4. Por qué no son factibles
   "dentro de la arquitectura técnica y el entorno operativo específicos"
   (definición de las guías)
5. Evidencia externa     las guías NO resuelven la limitación de las
                         soluciones robustas de marcado, EN PARTICULAR DE TEXTO
6. Medida compensatoria  disclosure de sesión: dónde, con qué texto, desde
                         qué build
7. Fecha, responsable, y revisión al cambiar el motor de generación
```

🟢 **El punto 5 es el que hace que el memo no sea alegato de parte:** la
limitación del marcado de texto está reconocida en el comentario legal sobre las
propias guías, no la alega el proveedor.

⚠️ **Lo que NO puede decir el memo:** *"es caro"*, *"no hay tiempo"* o *"nadie lo
hace"*. La prueba es de **factibilidad técnica dentro de la arquitectura
específica**, no de conveniencia. Un memo con argumentos de costo **debilita** el
caso en vez de sostenerlo.

**Paso 4 — Implementar el disclosure de sesión (condición (b)).** Es trabajo de
producto, no de pipeline, y **es la parte que no se puede saltear**:

| Dónde | Qué |
|---|---|
| Primera sesión / pantalla de inicio | Aviso de que el título usa AI generativa en contenido en vivo |
| Entrada al modo/zona con generación | Aviso de experiencia, persistente y no modal |
| Ajustes | Dónde se explica qué sistema genera qué |

🔗 **Encaja con lo que la KB ya tiene:** la clasificación por edad de la
**Receta 18** decide si además hay deberes reforzados; la **Receta 22** decide si
el cliente es provider (único que mira el reloj del 2-dic) o deployer; la
**Receta 13** cubre la obligación de runtime del Art. 50(1), que **esta receta no
reemplaza**.

**Tiempo estimado:** **2–3 días** por título ya inventariado (1 día de
clasificación de pilas, 1–2 de memo y texto de disclosure). Comparado con las
**3–5 semanas** de un sprint de marcado completo, **y con la ventaja de que
termina en una decisión defendible en vez de en un marcado parcial**.

🔵 **Advertencia que va en el entregable, no en la nota al pie:** toda la base
legal de esta receta es **secundaria**. WebFetch no resuelve
`digital-strategy.ec.europa.eu` desde esta sesión. **Antes de firmar, leer
`C(2026) 7702 final` y el Code of Practice (final 10-jun-2026) directamente.**

---

## 🆕 Receta 28: **Elegir servidor de juego sin heredar un cadáver** — framework vs backend terminado

**Problema que resuelve:** el cliente pide *"backend multijugador con AI"*, el
equipo busca por estrellas, y los dos primeros resultados de la categoría
(`NetEase/pomelo`, 11.855 ★ y `SerpentAI`, 6.987 ★) **están archivados**.
La KB tenía la capa de *backends terminados* y **no** la de *frameworks*.

**Paso 1 — Decidir la clase antes del repo** (es la pregunta que ordena todo):

| Si el cliente... | Clase | Opción | Licencia verificada |
|---|---|---|---|
| Quiere features ya hechas (auth, leaderboards, chat, storage) + capa AI encima | **Backend terminado** | [heroiclabs/nakama](https://github.com/heroiclabs/nakama) · [TaloDev/backend](https://github.com/TaloDev/backend) | Apache-2.0 (11.358) · MIT (1.074) |
| Tiene protocolo propio y necesita control de sharding | **Framework** | [cloudwu/skynet](https://github.com/cloudwu/skynet) (C+Lua) · [topfreegames/pitaya](https://github.com/topfreegames/pitaya) (Go, SDKs Unity/iOS/Android) | MIT (1.085) · MIT (1.067) |
| Es una casa **JVM** | **Framework** | [zfoo-project/zfoo](https://github.com/zfoo-project/zfoo) | Apache-2.0 (11.357) |
| Es web / Node / TypeScript | **Framework o backend** | [colyseus/colyseus](https://github.com/colyseus/colyseus) · [node-pinus/pinus](https://github.com/node-pinus/pinus) | MIT (1.075) · MIT (1.067) |
| Necesita escalar **instancias de partida** en Kubernetes | **Ortogonal — se suma** | [agones-dev/agones](https://github.com/agones-dev/agones) | Apache-2.0 |

**Paso 2 — Filtro de vigencia, antes que el de licencia.** Para cada candidato,
`gh api search/repositories -f q='repo:<slug>'` y mirar **`archived` y
`pushed_at`**, en ese orden. Un MIT impecable sobre un repo archivado es
**peor** que un Apache-2.0 vivo, porque el badge invita a confiar.

```bash
for s in cloudwu/skynet topfreegames/pitaya NetEase/pomelo; do
  gh api -X GET search/repositories -f q="repo:$s" \
    --jq '.items[]|"\(.full_name) arch=\(.archived) push=\(.pushed_at[0:10]) \(.license.spdx_id)"'
done
```

⛔ **Descartes que esta receta hace explícitos:**
[NetEase/pomelo](https://github.com/NetEase/pomelo) (**archivado 2023-08-10** —
su continuación viva es `pinus`) y
[clockworklabs/SpacetimeDB](https://github.com/clockworklabs/SpacetimeDB)
(**BSL 1.1**, tope de una instancia en producción — cycle 9).

**Paso 3 — Declarar el linaje antes de comparar.** 🔴
**pomelo (archivado) → nano → pitaya.** El `LICENSE` de pitaya dice
*"© 2018 TFG Co **and nano Authors**"*. **Poner nano y pitaya como dos
alternativas independientes en un cuadro comparativo es un error técnico**: son
tronco y rama. Y nano lleva **9 meses sin push** mientras pitaya commiteó hace
días.

**Paso 4 — Dónde entra la AI.** En las dos clases, la capa de agentes va en el
**borde de lógica de servidor**, no dentro del transporte: handlers/remotes en
pitaya o nano, servicios Lua en skynet, módulos Go/Lua/TS en Nakama. Ahí se
enchufan matchmaking con modelo propio ([open-match](https://github.com/googleforgames/open-match),
Apache-2.0), telemetría de comportamiento ([PostHog](https://github.com/PostHog/posthog)
— ⚠️ MIT Expat **con carve-out `ee/`**) y el NPC server-side de la **Receta 1**.

**Tiempo estimado:** **medio día** la selección documentada; el filtro de vigencia
del paso 2 son **minutos** y es el que evita el error caro.

---

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

## 🆕 Receta 7: Sprint de marcado Art. 50(2) del EU AI Act — **vence el 2-dic-2026 (sólo para el catálogo pre-2-ago-2026)**

**Caso de uso**: un publisher con catálogo en la UE que usó AI generativa en arte
promocional, copy de marketing o imágenes de juego, y tiene que emitir marcadores
**visibles y legibles por máquina** sobre contenido ya publicado.

**Por qué ahora**: el Art. 50 del EU AI Act **está vigente desde el 2-ago-2026** (quedó
fuera del aplazamiento general). Para sistemas **ya puestos en el mercado antes de
esa fecha**, el período de gracia del marcado *machine-readable* del
**Art. 50(2) termina el 2 de diciembre de 2026**. El reloj de alto riesgo
(Anexo III) es **otro** y vence el **2-dic-2027**: confundirlos es el error más
común del mercado.

🆕 **Base legal, resuelta el 2026-11-21 (cycle 7):** la transición del 2-dic-2026
se apoya en el **Art. 111(4)**, según 4+ fuentes secundarias independientes y
concordantes. 🔵 **La fuente primaria no se pudo leer** en esta sesión
(`digital-strategy.ec.europa.eu` da ENOTFOUND) ⇒ **citar el Art. 111(4) como lo
que las fuentes dicen y verificarlo contra el texto oficial antes de firmar un
entregable.**

🔴 **Y el alcance es mucho más chico de lo que esta receta sugería. Tres límites
que cambian a quién se le vende qué:**

| Límite | Qué significa |
|--------|---------------|
| **Sólo el Art. 50(2)** | La transición cubre **únicamente el marcado machine-readable**. **No extiende el Art. 50(1)** —el aviso al jugador de que interactúa con un sistema de AI—, que **vence para todos desde el 2-ago-2026**. Un cliente que creía tener hasta diciembre para su NPC conversacional **está mirando el deadline equivocado** |
| **Sólo el catálogo pre-2-ago-2026** | Un título **puesto en el mercado después del 2-ago-2026 NUNCA tuvo gracia**: debía marcado **desde el día de su lanzamiento**. ⇒ para un lanzamiento reciente, "quedan 11 días" **oculta un incumplimiento que ya ocurrió** |
| **Ninguna tecnología obligatoria** | El Acta **no nombra un estándar técnico de marcado**. ⇒ 🔴 **C2PA es una elección de ingeniería defendible, NO un requisito legal.** Se propone por auditabilidad e interoperabilidad y así hay que argumentarlo. Vía formal alternativa: el **Code of Practice on Transparency of AI-Generated Content**; quien no lo suscriba debe demostrar cumplimiento por *"alternative equivalently adequate means"* |

🔴 **La columna que el inventario de la Fase A necesita y no tenía: FECHA DE
PUESTA EN EL MERCADO.** Es la que decide, por SKU, si el cliente tiene 11 días o
una brecha abierta desde agosto. Sin esa columna la priorización de la Fase B
está mal ordenada.

🟢 **Reductor de alcance que ahorra trabajo real:** las guías de la Comisión
(**20-jul-2026**, **no vinculantes**) dan como ejemplo que la interacción con
**NPCs de un juego single-player** puede alcanzar el umbral de **"obviedad"** y
**no requerir disclosure adicional**. Combinado con la clasificación por edad de
audiencia (Receta 18), el alcance del Art. 50(1) se decide por **modo de juego ×
audiencia**, no por título.

🔴 **La cifra que mueve la reunión al comité:** sanción de hasta **15 M€ o 3% de
la facturación mundial anual**, el mayor de los dos.

---

### 🔴 REENCUADRE OBLIGATORIO — actualizado **2026-11-21: quedan 11 DÍAS**

Esta receta se escribió el 2026-10-26 como un **sprint de 3-5 semanas** contra el
2-dic-2026. **Hoy es 2026-11-22, quedan 10 días, y ese entregable ya no cabe.** Proponerlo tal cual
es vender un incumplimiento con fecha de entrega posterior a la obligación. La
receta técnica de abajo **sigue siendo correcta y se conserva íntegra** — es lo que
hay que construir —, pero **el encuadre comercial cambia**:

| Fase | Alcance | Plazo real | Para qué sirve |
|------|---------|-----------|----------------|
| 🆕 **0. Partición por fecha de puesta en el mercado** | Separar el catálogo en **pre-2-ago-2026** (tiene hasta el 2-dic) y **post-2-ago-2026** (**debía marcado desde el lanzamiento**). Una consulta a la propia base de releases del cliente. | **Horas.** | 🔴 **Es ahora el primer paso, antes del inventario de assets.** Decide si la conversación es "sprint contra el 2-dic" o "brecha abierta y plan de remediación". Proponer un sprint a un cliente del segundo grupo es diagnosticar mal. |
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
c2pa-rs (MIT *o* Apache-2.0)  /  c2pa-python (MIT *o* Apache-2.0)
    ↑ grant DUAL: elegís. Para entregable de cliente -> Apache-2.0 (patente)
    ↓ firma Content Credentials sobre el asset
Manifiesto C2PA embebido  ← la parte "machine-readable"
    +
Disclosure visible en store page / in-game  ← la parte "visible", Art. 50(1)
    ↓ auditoría
Reporte de cobertura: assets en alcance / marcados / pendientes, con fecha
```

**Repos**:
- [contentauth/c2pa-rs](https://github.com/contentauth/c2pa-rs) — 🔧 **MIT *o*
  Apache-2.0, a elección del integrador** (`LICENSE-MIT` 1083 B +
  `LICENSE-APACHE` 11335 B; **re-verificado en bytes el 2026-11-21, cycle 8**).
  SDK de referencia de Content Credentials. **Corregido el 2026-11-21 (cycle 8):**
  esta página decía *"Apache-2.0"* a secas desde el cycle 7, contra el
  *"MIT o Apache-2.0"* que `agents/top.md` ya publicaba — **primera deriva
  interna medida entre páginas de esta KB**.
- [contentauth/c2pa-python](https://github.com/contentauth/c2pa-python) —
  🔧 **MIT *o* Apache-2.0** (mismos dos archivos). Binding Python: lo práctico
  para correrlo sobre un pipeline de assets existente.

> 🟢 **Qué elegir, y por qué no es cosmético.** En un grant dual **elige quien
> integra**. Para un entregable de cliente se elige **Apache-2.0 por su grant
> expreso de patente** (MIT no lo tiene) — y es justamente el componente que
> **firma** el marcado legible por máquina del Art. 50(2). Decir sólo
> "Apache-2.0" **pierde la elección**; decir sólo "MIT" **pierde la patente**.

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

## 🆕 Receta 16: *Gate* de licencia para **packs de skills y prompts** — la ruta de contaminación que la Receta 15 no ve

**Problema, y es nuevo.** La Receta 15 pone un gate de licencia en CI sobre el
**árbol de dependencias**: lee lockfiles, manifests y SPDX de paquetes. Es
correcta y no alcanza. El activo que se volvió viral en gaming es el **pack de
skills** — directorios de `.md` con instrucciones de agente — y **entra al repo
del cliente con un `cp -r`**: sin lockfile, sin manifest, sin import, invisible
para todo gate que mire dependencias.

Medido en este pass sobre los packs nuevos de gaming con >400★: **3 de 6 son
copyleft** (`thedivergentai/Game-Dev-Agentic-Skills` LGPL-3.0 983★,
`jaylfc/taOS` AGPL-3.0 609★, `op7418/guizang-game-skill` AGPL-3.0 431★).

**Honestidad sobre el alcance de la afirmación:** que un prompt en markdown sea
"software" a efectos de la AGPL-3.0 **no está resuelto**, y esta receta no lo
asume. El riesgo que mitiga es el **de aceptación del entregable**: un cliente
corporativo con gate de licencias rechaza un archivo AGPL-3.0 en su repo sin
esperar jurisprudencia.

### Implementación

**Paso 1 — inventario de packs, no de paquetes.** Todo directorio que contenga
`SKILL.md`, `skills/`, `.claude/`, `AGENTS.md` o `*.skill.md` es un pack, venga
de donde venga.

```bash
#!/usr/bin/env bash
# gate-skills.sh — corre en CI, junto al gate de la Receta 15
set -euo pipefail
PROHIBIDAS='AGPL-3.0|LGPL-3.0|LGPL-2.1|GPL-2.0|GPL-3.0|CC-BY-NC|SSPL|BUSL'
fail=0
# 1) packs de skills que viven en el repo
while IFS= read -r d; do
  # el grant de un pack es el LICENSE más cercano hacia arriba
  lic=""
  p="$d"
  while [ "$p" != "." ] && [ -n "$p" ]; do
    for f in LICENSE LICENSE.txt LICENSE.md License.txt license.txt LICENCE COPYING; do
      [ -f "$p/$f" ] && { lic="$p/$f"; break 2; }
    done
    p=$(dirname "$p")
  done
  if [ -z "$lic" ]; then
    echo "🔴 SIN GRANT: $d  (ningún LICENSE en la cadena de directorios)"; fail=1; continue
  fi
  # firma por bytes + encabezado (ver agents/top.md)
  b=$(wc -c < "$lic")
  if [ "$b" -lt 50 ]; then
    echo "🔴 ARCHIVO FANTASMA: $lic son $b B — ver el trap de los 14 bytes"; fail=1; continue
  fi
  if head -40 "$lic" | grep -qiE 'AFFERO|LESSER GENERAL PUBLIC|GNU GENERAL PUBLIC'; then
    echo "🔴 COPYLEFT en pack de skills: $d → $lic ($b B)"; fail=1
  fi
done < <(find . -type d \( -name skills -o -name '.claude' \) -not -path './.git/*')

# 2) ningún .md del repo debe declarar una licencia prohibida en su propio header
grep -rlEi "SPDX-License-Identifier: *($PROHIBIDAS)" --include='*.md' . 2>/dev/null \
  | sed 's/^/🔴 SPDX prohibido en markdown: /' | tee /dev/stderr | grep -q . && fail=1

exit $fail
```

**Paso 2 — la regla de los 3 bytes que hace falta conocer.** El gate de arriba
usa dos discriminadores derivados en este pass y en el anterior:

| Señal | Qué significa | Por qué el gate la necesita |
|-------|---------------|----------------------------|
| **bytes < 50** | el cuerpo de un 404 de `raw.githubusercontent.com` son **14 B** (`404: Not Found`) | sin esto, un archivo inexistente pasa como licencia válida |
| **bytes ≈ 7.650 + encabezado "LESSER"** | **LGPL-3.0**, que es un **suplemento** y **no embarca la GPL-3.0** que incorpora | un gate que sólo mire tamaño ve 7.650 B, lo compara con la banda de copyleft (≥26.400) y **lo deja pasar** |
| **bytes idénticos al `LICENSE` de la raíz** | copia de packaging, **no** partición del grant | evita 2 falsas alarmas de cada 4 (tasa medida en este pass) |

**Paso 3 — el pack que se recomienda usar, y por qué.**
[gamedev-skills/awesome-gamedev-agent-skills](https://github.com/gamedev-skills/awesome-gamedev-agent-skills)
(**Apache-2.0**, `LICENSE` 11.395 B + `NOTICE` 883 B, **9.929★, 1.013 forks**):
79 skills + router para Godot, Unity, Unreal, Roblox, three.js, Phaser y 6
engines más, *"checked on the real engines"*. **Su `NOTICE` es parte del valor
del entregable**, no un costo: declara que todas las skills son obra original
escrita desde documentación primaria y que *"no text or code was copied"* de
otras colecciones, más disclaimer de marcas de los 11 engines. Apache-2.0 §4(d)
obliga a propagarlo ⇒ **se entrega la atestación de procedencia junto con el
código**, que es justo lo que un cliente con ansiedad de IP quiere firmar.

**Paso 4 — alternativa si el cliente exige MIT puro:** `coop-anything` (MIT) y
`bot-crossing` (MIT) para las piezas que cubren; para profundidad de Godot,
`Game-Dev-Agentic-Skills` es técnicamente el mejor (99 skills, 27 blueprints de
género) y está **fuera de la ruta permisiva** por su LGPL-3.0: hay que decirlo y
no usarlo, no usarlo y no decirlo.

**Esfuerzo:** 1-2 días para el gate; 0,5 día por repo de cliente para el barrido
inicial. **Encaja con:** Recetas 10, 14 y 15 — la secuencia completa es
**10 (verificar) → 14 (acotar) → 15 (gate de build) → 16 (gate de prompts)**.

---

## 🆕 Receta 17: Auditoría de grant **por enumeración**, no por sonda — y el inventario de componentes vendorizados

**Problema.** Las Recetas 10 y 14 verifican licencias **sondeando nombres de
archivo**: se pide `LICENSE`, `LICENSE.txt`, `License.txt`… de una lista. Esa
lista tiene un sesgo invisible desde adentro: **es exactamente tan completa como
la imaginación de quien la escribió.** El pass anterior auditó
`SkyworkAI/Matrix-Game` con sondas, encontró 2 archivos de licencia y escribió
que *no podía afirmar* que no hubiera más. Había **7**, y **3 de las rutas no
eran adivinables** (nadie iba a sondear `Matrix-Game-4/licenses/MG4CF-LICENSE`).

> **Una sonda sólo puede confirmar lo que ya sospechabas. Sólo la enumeración te
> dice lo que te faltaba.**

### Implementación

**Paso 1 — enumerar, no adivinar.**

```bash
SLUG=SkyworkAI/Matrix-Game
gh api -X GET search/code -f q="repo:$SLUG filename:LICENSE" --jq '.items[].path'
# Matrix-Game-4/licenses/MG4CF-LICENSE
# Matrix-Game-4/licenses/TAEHV-LICENSE
# Matrix-Game-4/matrix_game/web/mediamtx-reader.LICENSE
# LICENSE
# Matrix-Game-4/LICENSE
# Matrix-Game-3/LICENSE.txt
# Matrix-Game-1/GameWorldScore/LICENSE
```

⚠️ **Dos trampas del canal, medidas:** (a) `filename:LICENSE` **matchea código
también** (`tools/ci_unity_license.py`, `build/extract-licenses.sh`) — filtrar
por ruta/extensión antes de contar; (b) repetir la consulta con
`filename:LICENCE`, `filename:COPYING` y `filename:NOTICE`, porque el índice
matchea el nombre que se le pide.

**Paso 2 — buscar el inventario de terceros, que es el eje que ninguna sonda de
licencia encuentra.**

```bash
for f in THIRD_PARTY_NOTICES.md THIRD_PARTY_NOTICES NOTICE CREDITS.md; do
  curl -sS -w "|%{http_code}" "https://raw.githubusercontent.com/$SLUG/HEAD/$f"
done   # descartar todo lo que no sea 200 (ver el trap de los 14 bytes)
```

Para Matrix-Game-4 ese archivo lista **11 componentes vendorizados** y encabeza
con *"The project-level Apache-2.0 license does not replace the licenses
below"*. Entre ellos: `recastnavigation` (**zlib**), Jolt Physics (MIT),
[madebyollin/taehv](https://github.com/madebyollin/taehv) (MIT), stb
(dual-license) y —el que muerde— **audio de racing bajo CC0 y CC BY 3.0 con
obligación de retener `audio/CREDITS.md`**.

**Paso 3 — resolver cada archivo con el discriminador de byte-identidad.**

```bash
root_bytes=$(curl -sS "https://raw.githubusercontent.com/$SLUG/HEAD/LICENSE" | wc -c)
# para cada ruta enumerada:
#   bytes == root_bytes  -> copia de packaging, NO es partición. No leer.
#   bytes != root_bytes  -> leer el encabezado. Es un grant distinto.
#   bytes < 50           -> 404 fantasma. Descartar.
```

**Tasa medida en este pass** sobre los 12 repos más recomendados de esta KB:
**4 de 12** tenían un grant que la ficha plana no representaba, y **2 de esos 4
hallazgos eran falsas alarmas** que el discriminador de byte-identidad separó sin
una lectura extra. El hallazgo real más caro: **`limbonaut/limboai`, publicado
como "MIT" plano en dos archivos de esta KB, es MIT para el código y
CC BY 4.0 para el logo y para los assets gráficos del proyecto demo** — y el
demo es justamente lo que un estudio forkea como punto de partida.

**Paso 4 — cerrar la licencia de los pesos sin acceder a Hugging Face.** Para
cualquier modelo del entregable:

```bash
curl -sS "https://raw.githubusercontent.com/$SLUG/HEAD/README.md" \
  | grep -iE "pre-trained from|base model|backbone|fine-tuned from|initialized from"
```

Porque **un modelo derivado no puede ser más permisivo que su backbone**.
Aplicado a `THUSI-Lab/GameScaling` devuelve, literal: *"The models are continued
pre-trained from **Qwen3.5**, which is also **Apache 2.0**"* ⇒ **pesos
Apache-2.0, verificado**, el primer grant de pesos cerrado en la historia de
esta KB. Si el README **no** lo declara y el modelo es entrenado desde cero, el
resultado correcto es *"grant de pesos no verificable por este canal"* escrito
en el entregable — y no una inferencia.

**Entregable:** una tabla por repo con `ruta | bytes | grant | ¿se distribuye?`,
más una fila por componente vendorizado y una fila por artefacto de modelo. Es
la Receta 14 (6 ejes, hoy **7** con *por dependencia vendorizada*) pero
**completa en vez de muestreada**.

**Esfuerzo:** 1 día por stack. **Encaja con:** 10 → 14 → **17** → 15/16. La 17
va **antes** de los gates, porque es la que produce la lista contra la que los
gates corren.

---

## 🆕 Receta 18: Clasificación de audiencia por edad **antes** del sprint de disclosure del Art. 50

**Problema.** La Receta 7 trata el deadline del **2-dic-2026** como un sprint de
inventario-y-etiquetado sobre todos los sistemas AI del catálogo. El análisis de
**WH Partners** (*"Same NPC, Different Rules: How a Player's Age Changes Their
Responsibilities Under the AI Act"*) agrega el discriminador que faltaba: **la
obligación depende de la audiencia.** Un juego **single-player dirigido a
adultos** puede **no** necesitar etiquetar el diálogo de un NPC; el **mismo NPC**
en un título con audiencia de menores sí.

⇒ Etiquetar todo no es "ir a lo seguro": es **gastar ingeniería en filas que no
la necesitan** mientras quedan sin resolver las que sí, con 11 días de plazo.

### Implementación

**Paso 1 — agregar una columna al inventario de la Receta 7**, antes de tocar
una línea de código:

| Título | Sistema AI | Audiencia declarada (PEGI/ESRB) | ¿Interacción con el jugador? | Obligación Art. 50 | Trabajo |
|--------|-----------|--------------------------------|------------------------------|--------------------|---------|
| *(ejemplo)* RPG single-player | NPC con LLM | PEGI 18 | sí, sólo texto | **a evaluar** — WH Partners sugiere que puede no requerir etiqueta | registro + decisión fechada y firmada |
| *(ejemplo)* mobile F2P | NPC con LLM | PEGI 7 | sí | **sí** | etiqueta + capa de runtime |
| *(ejemplo)* cualquiera | upscaling de texturas en build | — | **no** | **no** (herramienta de desarrollo) | sólo registro |

**Paso 2 — cruzar con la obligación de NA, que es por reloj y no por edad de
clasificación.** Connecticut (PA 26-15) exige recordatorio de que se habla con
una AI **≥1 vez/hora para menores y ≥1 vez/3 horas para adultos**. ⇒ **las dos
jurisdicciones piden cosas distintas del mismo dato**: EMEA usa la edad para
decidir **si** hay obligación; NA la usa para decidir **con qué frecuencia**
cumplirla. Una sola implementación sirve para las dos **sólo si la edad es un
parámetro de configuración y no una constante de build**. Es exactamente la capa
de la Receta 13, con la edad promovida a entrada.

**Paso 3 — dimensionar la exposición por jurisdicción antes de decidir cuánto
invertir.** Las cifras no son comparables y por eso la decisión es económica:

| Jurisdicción | Techo de sanción | Estado |
|--------------|------------------|--------|
| **EU** (Art. 5, prácticas prohibidas — p. ej. NPC que empuja al gasto) | **€35 M o 7% de facturación global** | vigente |
| **EU** (Art. 50, transparencia) | régimen de multas del Acta | **2-dic-2026** para sistemas ya en mercado |
| **Corea del Sur** (AI Basic Act) | **KRW 30 M** (~US$ 20 mil), sólo 3 tipos de incumplimiento, **gracia ≥1 año** | vigente 22-ene-2026 |
| **China** (CAC) | registro **previo** del servicio de AI generativa | vigente; **>550 servicios registrados** a mediados de 2026 |
| **Japón** | sin penalidades (enfoque por principios) | AI Basic Plan nuevo del **14-jul-2026** |

**Seis órdenes de magnitud entre el techo europeo y el coreano, para un mismo
título que publica el mismo día en los dos mercados.** La conclusión no es
ignorar Corea: es que **el presupuesto de compliance se asigna por exposición
medida**, y China no es una multa sino un **lead time** que hay que meter en el
cronograma de lanzamiento.

**Paso 4 — documentar la decisión, no sólo la implementación.** Para cada fila
donde se concluye "no hay obligación", el entregable es un **registro fechado
con el razonamiento y la fuente**. Si el criterio de la Comisión se endurece, lo
que protege al cliente es haber decidido con fundamento en una fecha, no haber
acertado.

**Esfuerzo:** 2-3 días de clasificación para un catálogo mediano; es lo que
libera semanas de ingeniería. **Encaja con:** va **antes** de la 7 y de la 13.

---

## 🆕 Receta 19: QA autónomo con un agente de playtesting open source — **multi-engine, Apache-2.0**

**Caso de uso**: un estudio con un build jugable que necesita cobertura de
regresión sobre niveles y progresión, y no tiene equipo de QA para repetirlo en
cada build.

**Por qué ahora**: hasta este pass, la Receta 9 (QA medido) dependía de
`lmgame-org/GamingAgent`, **congelado desde 2025-11-16**, y de entornos RL que
hay que entrenar por juego. **Apareció la pieza que faltaba**:
[ramenvr/autopilot](https://github.com/ramenvr/autopilot) — **Apache-2.0**
(`LICENSE`, 11358 B), 881★, commit de hoy — un agente de playtesting que
**aprende el juego y mejora corrida a corrida**, con soporte **UE 5.4–5.8,
Unity 6, Godot 4 y Roblox Studio** en Windows y macOS.

🟢 **Verificado que el grant cubre fuente real, no un envoltorio de descarga:**
651 archivos versionados, `plugin/` de 10 MB, **50 `.cpp` / 24 `.h` / 81 `.cs` /
47 `.gd` / 156 `.py`** y un `.uplugin`.

**Stack**:
```
build jugable del cliente (UE5 / Unity 6 / Godot 4 / Roblox Studio)
    ↓ plugin Autopilot instalado en el proyecto
ramenvr/autopilot (Apache-2.0)
    ├── modo plugin        → exploración autónoma, aprende entre corridas
    └── modo MCP           → el agente de codegen (Claude Code / Codex) PIDE
                              una sesión de playtest y LEE el resultado
    ↓ salida
repro steps + estado al fallo
    ↓
LMGame Bench / escenarios propios   ← la capa de MEDICIÓN de la Receta 9
    ↓
gate en CI: regresión de progresión por build
```

**Cómo conectar**:
1. **Instalar de fuente, no el binario, si el cliente exige auditabilidad.**
   ⚠️ El camino por defecto del README **descarga builds precompilados de
   `tryaura.dev`**; el repo soporta compilar desde fuente para Unreal
   explícitamente. Para un entregable con revisión de seguridad, **compilar**:
   el grant Apache-2.0 cubre la fuente, no las condiciones del sitio de descarga.
2. **Empezar por el MCP, no por el plugin.** El valor inmediato no es "QA
   automático": es que **el agente que escribe el código pueda probar su propio
   cambio** antes de abrir el PR. Eso cierra el loop de las Recetas 6 y 11.
3. **Medir con la Receta 9, no con el vendor.** ⚠️ Las cifras de Ramen (**43%
   más preciso**, **2× más rápido**, **7/7 vs 5/7** en un benchmark de 7 tareas
   de Unreal) **son del vendor y no están verificadas independientemente**. Se
   citan como lo que el proyecto declara; el número que va a la propuesta es el
   medido sobre el juego del cliente.
4. **Definir qué es un fallo antes de correrlo.** Un agente que explora encuentra
   *comportamiento inesperado*, no *bugs*. Sin criterio de aceptación escrito, la
   salida es una lista que nadie tríaga y el proyecto muere en la semana 3.

**Tiempo estimado**: **3-5 días** para el primer loop MCP sobre un build
existente; **2-3 semanas** para el gate de regresión en CI.

**Encaje regional**: 🟢 **Fuerte en APAC y NA.** En APAC conecta directo con la
práctica ya declarada por los estudios japoneses (**prohibido que salidas crudas
de AI lleguen al producto sin revisión**): un agente de QA es AI **que no toca el
contenido del jugador**, así que **no dispara ni disclosure ni backlash**. En NA
cae del lado exento de la política de Steam (herramienta de desarrollo).

---

## 🆕 Receta 20: Auditoría de licencias, **versión corregida** — el checksum tríaga, el íncipit dicta

**Caso de uso**: el mismo de las Recetas 10, 14 y 17 — auditar el grant de un
stack de gaming. **Esta receta no las reemplaza: les corrige tres defectos
medidos** que producen falsos negativos.

**Por qué existe**: el *checksum de bytes* es la técnica estrella de esta KB y
este pass la rompió en tres puntos distintos, todos reproducibles.

### Los tres defectos, con el repo donde se midió cada uno

| Defecto | Medición | Falso resultado que produce |
|---------|----------|-----------------------------|
| **Apache-2.0 tiene DOS valores válidos** | **11358 B** con `APPENDIX: How to apply` (`open_spiel`, `mujoco`, `autopilot`) vs **10173/10174 B** sin apéndice (`bevy`, `libgdx`). **Δ = 1.185 B = el bloque del apéndice** | 🔴 **Falso negativo**: el checksum heredado (`Apache~11350`) marca a **bevy y libgdx** como "no Apache" — dos de los engines más usados del mundo |
| **Un conteo POR ENCIMA del canónico es un hallazgo** | **15057 B** en `NVIDIA/Audio2Face-3D-training-framework` = Apache-2.0 **+** un inventario `PORTIONS LICENSED AS FOLLOWS` con **3 dependencias vendorizadas MIT y su ruta** (`audio2face/deps/charsiu/`, `.../motion_diffusion_model/`) | 🔴 **La enumeración de la Receta 17 no lo caza**: encuentra **UN** archivo de licencia y concluye **un** grant. El inventario vive **dentro** del `LICENSE`, no en archivos separados |
| **El checksum no distingue un grant hecho a medida** | **9977 B** = `Defold License 1.0`, a **196 B** del Apache sin apéndice, y es **Apache verbatim + una restricción de campo de uso** | 🔴 **Falso positivo**: "pesa como Apache" ⇒ se aprueba un grant **que no es open source** |

### El 8º eje de partición: **por rama**

```
Unity-Technologies/ml-agents — el MISMO archivo, el MISMO repo:
  develop (rama DEFAULT) → LICENSE.md =    572 B  (aviso corto de Apache)
  main                   → LICENSE.md = 11.324 B  (texto completo)
```
🔴 Leer sólo `HEAD` (que resuelve la default) y concluir *"cabecera-puntero, no
el texto completo"* es lo que esta KB publicaba. **El texto completo existe, en
otra rama.** Y esos 572 B **no son un grant deficiente**: son el **boilerplate
que Apache-2.0 recomienda en su propio apéndice** — el mismo bloque que `bevy` y
`libgdx` omiten.

### Implementación

```bash
# audit-grant.sh — reemplaza el núcleo de la Receta 10
SLUG="$1"

# 1) CANAL 1: existencia + badge + ★ + archived + pushed_at
gh api -X GET search/repositories -f q="repo:$SLUG"
#    NOASSERTION / null  -> NO es un veredicto: es la AUSENCIA de uno.
#    Resolver SIEMPRE por byte-read, y EN LAS DOS DIRECCIONES.

# 2) CANAL 3 primero (enumerar, no sondear)
gh api -X GET search/code -f q="repo:$SLUG filename:LICENSE"
#    bevy -> 164 archivos (doble grant replicado por crate)
#    defold -> 37 (incl. COMPLYING_WITH_LICENSES.md, TOOLS_LICENSES.md)
#    total_count: 0 -> ver el paso 5

# 3) CANAL 2 por rama, NO sólo HEAD  <-- EL CAMBIO DE ESTA RECETA
for BR in HEAD main master develop; do
  for F in LICENSE LICENSE.md LICENSE.txt LICENCE LICENSE-MIT LICENSE-APACHE \
           COPYING COPYING.txt license.txt LICENSE.rst NOTICE; do
    curl -sL -o /tmp/l -w "$BR/$F %{http_code} " \
      "https://raw.githubusercontent.com/$SLUG/$BR/$F"
    wc -c < /tmp/l; sha256sum /tmp/l    # <-- dedupe POR HASH, no por rama
  done
done
#    < 50 B           -> 404 fantasma (el cuerpo "404: Not Found" son 14 B)
#    tomar el texto MAS LARGO como el operativo

# 4) Los tres discriminadores que el conteo NO resuelve
head -c 200 /tmp/l                      # a) EL INCIPIT MANDA. "Defold License"? "Ms-PL"?
grep -c "APPENDIX: How to apply" /tmp/l # b) explica 11358 vs 10173 en Apache
awk '/END OF TERMS AND CONDITIONS/{f=1} f' /tmp/l   # c) TODO lo que viene despues
                                        #    son CLAUSULAS ANADIDAS. Leerlas.

# 5) Si no hay NINGUN archivo de licencia: NO es permisivo por defecto
#    htdt/godogen_assets -> 69 archivos versionados, 0 licencias, 2 canales de acuerdo
#    SIN GRANT = PROHIBIDO. Exclusion dura, no "a revisar".
```

### ⚠️ Defecto del instrumento que hay que conocer: `raw` aliasea `master` → `main`

`ml-agents` **no tiene** `refs/heads/master` y aun así
`raw/.../master/LICENSE.md` devuelve **200 con los bytes de `main`**; una rama
inventada devuelve **404**. ⇒ el bucle de arriba **cuenta el mismo archivo dos
veces**, y *"lo encontré en master"* **no prueba que `master` exista**. Por eso
el `sha256sum`: **deduplicar por contenido, no por nombre de rama.**

### El resultado que esta receta produce y las anteriores no

| Repo | Badge | Veredicto real por byte-read | Dirección |
|------|-------|------------------------------|-----------|
| `cocos/cocos-engine` | `NOASSERTION` | **MIT** (1164 B = 2 líneas de copyright) | 🟢 permisivo detrás del badge |
| `panda3d/panda3d` | `NOASSERTION` | **BSD-3-Clause** (1569 B, se auto-declara en prosa al final) | 🟢 permisivo detrás del badge |
| `bevyengine/bevy` | `Apache-2.0` | **MIT *o* Apache-2.0** (164 archivos de licencia) | 🟢 **el badge SUB-declara el grant** |
| `defold/defold` | `NOASSERTION` | 🔴 **`Defold License 1.0`**, field-of-use | 🔴 restrictivo |
| `minerllabs/minerl` | `NOASSERTION` | 🔴 **CC-BY-NC-SA 4.0** | 🔴 **NonCommercial** |
| `htdt/godogen_assets` | `null` | 🔴 **ningún grant** | 🔴 prohibido |

> 📐 **La regla de una línea, para copiar a toda la familia de KBs:**
> **el badge no es el grant, el conteo de bytes no es el grant, y la rama default
> no es el repo.** El grant es el texto, y hay que leerlo — en todas las ramas y
> en todos los archivos que la enumeración devuelva.

**Tiempo estimado**: **medio día** sobre un stack ya inventariado; **1 día** si
hay que enumerar por rama en más de 20 repos.

---

## 🆕 Receta 22: **Triaje provider vs deployer** — el paso 0 que le falta a la Receta 7

**Problema que resuelve:** la Receta 7 arranca un sprint de marcado del Art. 50(2) con
el reloj del 2-dic-2026 encima. Las fuentes del cycle 9 muestran que ese plazo es
**mucho más angosto** de lo que el pánico de calendario sugiere, y correr el sprint sin
triar antes puede significar **vender once días de urgencia por un deber que no le
aplica al cliente**.

**Los cuatro hechos que acotan el alcance:**

1. El plazo 2-dic-2026 cubre **sólo el marcado legible por máquina del PROVEEDOR
   (Art. 50(2))**, y **sólo** para sistemas puestos en el mercado **antes del 2-ago-2026**.
2. Sistemas puestos en el mercado **desde el 2-ago-2026: sin gracia**, cumplen al lanzar.
3. Los deberes del **deployer NO se pospusieron**: rigen desde el 2-ago-2026.
4. El **contenido sintético generado antes del 2-ago-2026 NO se marca retroactivamente**.

### Implementación

**Fase 0 — ¿El cliente es provider o deployer?** (una pregunta, cambia todo el proyecto)

| Si el cliente... | Entonces es | Y le aplica |
|---|---|---|
| Entrena/ajusta y **pone en el mercado** un modelo generativo propio | **Provider** | Art. 50(2): marcado legible por máquina. **Es el único que mira el reloj del 2-dic.** |
| Usa un modelo de un tercero (API o pesos) dentro de su juego | **Deployer** | Art. 50(1) avisar que se interactúa con IA; Art. 50(4) etiquetar deepfakes. **No** el 50(2) del proveedor. |
| Ambas cosas en distintos componentes | **Las dos** | Triar **por componente**, no por empresa |

> La mayoría de los estudios de videojuegos cae en la **segunda fila**. Decirlo de
> entrada reduce el alcance del proyecto — y es exactamente por eso que gana el proyecto
> siguiente.

**Fase 1 — Inventario con fecha de puesta en el mercado.** Para cada sistema con IA:
`nombre | provider o deployer | fecha de puesta en el mercado | ¿antes del 2-ago-2026? | artículo aplicable`.
La columna de fecha decide si hay prórroga o si la obligación ya venció al lanzar.

**Fase 2 — Clasificación de riesgo (y la buena noticia).** La Comisión sostiene que
**en principio los videojuegos con IA no tienen obligación** bajo el AI Act: pathfinding,
balanceo y entrenamiento de NPC son **riesgo mínimo**. Marcarlos como tales, por escrito
y con fundamento, **es parte del entregable**.

**Fase 3 — El control que sí es urgente y no es de transparencia:** buscar **NPCs
diseñados para empujar gasto in-game**. Eso no es "alto riesgo", es **prohibido por el
Art. 5**, con multas de hasta **35 M€ o 7%** de facturación global, y **no tiene plazo de
gracia ninguno**. Si aparece algo así en el inventario, **sube de prioridad por encima
de todo el sprint de marcado**.

**Fase 4 — Elegir el mecanismo de marcado mirando a 2027, no a diciembre.** Los
firmantes del Código de Buenas Prácticas de Transparencia (junio 2026, declarado
adecuado por la Comisión; guidelines finales ~30-jul-2026) deben montar un **mecanismo
interoperable después de diciembre**. Un marcado elegido a las apuradas ahora puede
tener que rehacerse: conviene verificarlo contra ese requisito **antes** de implementarlo.

**Qué entregar:** una tabla de una página por sistema — *artículo aplicable, fecha que
manda, qué hay que hacer, qué explícitamente no hay que hacer*. La última columna
(incluido *"los assets generados antes del 2-ago-2026 no se re-marcan"*) suele ser la
que más tiempo ahorra.

---

## 🆕 Receta 23: *Gate* de **Business Source License y Change Date** en la selección de infraestructura

**Problema que resuelve:** las Recetas 10/14/15/20/21 cazan copyleft y ausencia de
licencia. **Ninguna caza una licencia *source-available* con tope de uso**, que es el
modo en que hoy se distribuye la infraestructura de juego más atractiva técnicamente.
El caso medido: `clockworklabs/SpacetimeDB`, **25.570 ★**, que GitHub reporta como
`NOASSERTION` y que en realidad es **BSL 1.1 con tope de UNA instancia en producción**.

**Por qué los gates anteriores no lo ven:** BSL no es copyleft (no obliga a abrir nada),
no es ausencia de licencia (el archivo está completo y es explícito), y su byte-count
(42.562 B) no coincide con ninguna firma conocida. Pasa los tres filtros existentes.

### Implementación

**Paso 1 — Disparadores.** Marcar para lectura manual **cualquier** dependencia de
infraestructura cuyo `spdx_id` sea `NOASSERTION`, o cuyo `LICENSE` contenga alguna de:

```
Business Source License
Change Date
Additional Use Grant
Licensed Work
Change License
Functional Source License
Elastic License
Server Side Public License
```

**Paso 2 — Extraer los tres parámetros que deciden** (no el nombre de la licencia):

| Parámetro | Pregunta | SpacetimeDB (medido 2026-11-21) |
|---|---|---|
| **Additional Use Grant** | ¿Qué uso se permite HOY? | **Máximo 1 instancia en producción**; prohibido ofrecer *Database Service* |
| **Change Date** | ¿Cuándo cambia? | **2031-11-20** |
| **Change License** | ¿A qué cambia? | **AGPL-3.0 con excepción de linking** |

**Paso 3 — La prueba que importa: proyectar el grant contra el ÉXITO del proyecto.**
No preguntar *"¿puedo usarlo?"* sino **"¿sigo adentro del grant cuando esto escale?"**.
SpacetimeDB se usa legítimamente con una instancia; **el día que el juego anda bien y se
levanta la segunda, el proyecto sale del grant**. Un riesgo que se dispara con el éxito
es peor que uno que se dispara con el fracaso: llega cuando ya no hay tiempo de migrar.

**Paso 4 — Chequear partición por subdirectorio.** El `LICENSE.txt` de SpacetimeDB
declara licencias por subdirectorio (§2) **y** que ante conflicto gobierna la base —
**las dos cosas a la vez**, lo cual es contradictorio. Ante una cláusula así, documentar
la lectura **conservadora** (la base gobierna todo) como la única defendible.

**Paso 5 — Sustituir, no negociar.** Alternativas ya verificadas por esta KB:

| En lugar de | Usar | Licencia | Nota |
|---|---|---|---|
| SpacetimeDB | [heroiclabs/nakama](https://github.com/heroiclabs/nakama) | **Apache-2.0** | Default de esta KB |
| idem | [TaloDev/backend](https://github.com/TaloDev/backend) | **MIT** (`LICENSE`, 1.074 B) | Si el cliente no acepta Apache-2.0 |
| idem | [NamazuStudios/elements](https://github.com/NamazuStudios/elements) | **MPL-2.0** (`LICENSE.txt`, 16.726 B) | Copyleft **por archivo**: hostearlo no obliga a abrir el juego |

**Qué entregar:** por cada dependencia marcada, una ficha de cinco líneas — *grant de
hoy, Change Date, Change License, en qué escenario de crecimiento se rompe, con qué se
reemplaza*. La cuarta línea es la que convence a un CTO.

---

## 🆕 Receta 24: QA determinista con `godot-mcp` + inyección de input — la pieza que le faltaba a la Receta 9

**Problema que resuelve:** la Receta 9 mide agentes que juegan con LMGame Bench, pero
su bucle de QA sobre Godot dependía de MCP de edición de escena, **sin una forma
reproducible de inyectar input ni de leer estado de juego en vivo**. Sin determinismo no
hay regresión: una corrida que no se puede repetir no sirve como test.

**La pieza nueva, verificada hoy:**
[satelliteoflove/godot-mcp](https://github.com/satelliteoflove/godot-mcp) — **MIT**
(`LICENSE`, 1.056 B), 188 ★, 23 forks, push 2026-11-16. Aporta tres cosas que el
`godot-mcp` de Coding-Solo que esta KB ya lista no tiene: **inyección de input**,
**playtesting determinista** y **estado de juego en vivo** para el agente.

### Implementación

1. **Engine:** [godotengine/godot](https://github.com/godotengine/godot) (MIT).
2. **Capa MCP de QA:** `satelliteoflove/godot-mcp` (MIT) — inyección de input + estado.
3. **Semilla fija por corrida.** Fijar la semilla del RNG del juego **y** registrarla en
   el artefacto de la corrida. Sin esto, lo demás no es determinista aunque el input lo sea.
4. **Guion de input como dato, no como código:** una lista de `(frame, acción)`. El agente
   produce el guion; el runner lo reproduce. Un fallo se reporta **como guion**, y el
   equipo lo vuelve a correr idéntico.
5. **Aserciones sobre estado de juego en vivo**, no sobre píxeles: posición, vida,
   inventario, estado de máquina. Las aserciones de píxel rompen con cada cambio de arte.
6. **Comportamiento del agente:** behaviour tree (como [bitbrain/beehave](https://github.com/bitbrain/beehave), MIT)
   para la exploración, y el LLM sólo para **proponer guiones nuevos** — nunca dentro del
   bucle determinista, o el determinismo se pierde.
7. **Medición:** LMGame Bench (Receta 9) para puntuar al agente; esta receta para que la
   corrida **se pueda repetir**.

**Entregable:** una suite de guiones de input versionada junto al juego, donde cada bug
reportado trae su guion y su semilla. **El valor que se vende no es "un agente juega":
es "cualquier fallo que el agente encuentre se reproduce en un comando".**

---

## 🆕 Receta 25: **Sustitución de la línea completa de world models de Tencent** — extiende la Receta 15 con el dato que faltaba

**Qué cambió respecto de la Receta 15.** Aquella receta nació con **un** repo
confirmado con exclusión territorial (`HY-World-2.0`) y construyó el gate de CI
alrededor. Este pass verificó byte a byte los **tres** repos de la línea y
encontró, además, que **la exclusión no llega hasta el fondo del árbol**: hay un
subdirectorio Apache-2.0 dentro de uno de ellos. El gate de la Receta 15 falla
el build correctamente, pero **falla de más**: bloquea un subárbol que sí se
puede usar.

**Paso 1 — la matriz, ya medida (no hay que re-verificarla):**

| Componente | Licencia leída | Bytes | ¿EU / UK / KR? | ¿NA / LATAM / APAC-ex-KR? |
|---|---|---|---|---|
| `Tencent-Hunyuan/HY-World-2.0` → `License.txt` | Tencent HY-World 2.0 Community | 16.858 | ⛔ sin grant | ✅ con condiciones |
| `Tencent-Hunyuan/Hunyuan-GameCraft-1.0` → `LICENSE` | Tencent Hunyuan Community | 16.386 | ⛔ sin grant | ✅ con condiciones |
| `Tencent-Hunyuan/HY-WorldPlay` → `License.txt` | Tencent HY-WorldPlay Community | 16.045 | ⛔ sin grant | ✅ con condiciones |
| 🟢 `HY-WorldPlay` → **`worldcompass/LICENSE`** | **Apache-2.0** | 10.757 | ✅ **sin restricción** | ✅ |
| ⛔ `HY-WorldPlay` → `worldcompass/reward_function/HunyuanWorldMirror/License.txt` | Tencent HunyuanWorld-Mirror Community | 17.267 | ⛔ sin grant | ✅ con condiciones |

⚠️ **Leer la tabla de abajo hacia arriba:** dentro del subárbol Apache-2.0
reaparece una licencia Tencent un nivel más abajo. **Un gate que marque
`worldcompass/` como "permisivo" y pare ahí deja entrar la reward function.**

**Paso 2 — el manifiesto de la Receta 15, corregido para admitir rutas:**

```yaml
- slug: Tencent-Hunyuan/HY-WorldPlay
  paths:
    - path: "/"
      license: LicenseRef-Tencent-HY-WorldPlay-Community
      territory_excluded: [EU, UK, KR]
    - path: "worldcompass/"
      license: Apache-2.0
      territory_excluded: []
    - path: "worldcompass/reward_function/HunyuanWorldMirror/"   # re-entra la exclusión
      license: LicenseRef-Tencent-HunyuanWorld-Mirror-Community
      territory_excluded: [EU, UK, KR]
  resolution: longest-prefix-wins
```

`longest-prefix-wins` es la regla que hace falta y la que nadie escribe: **el
grant aplicable a un archivo es el del prefijo más largo que lo cubra**, no el
de la raíz.

**Paso 3 — el sustituto, con grant verificado este pass:**

| Sustituto | Licencia (byte-read) | ★ | Territorio | Nota |
|---|---|---|---|---|
| [SkyworkAI/Matrix-Game](https://github.com/SkyworkAI/Matrix-Game) | **MIT** (raíz, 1.118 B) + **Apache-2.0** (`Matrix-Game-4/`, 11.357 B) | 2.951 | ✅ sin cláusula | Mismo caso de uso: mundo jugable generado. **7 archivos de licencia** — aplicar `longest-prefix-wins` |
| [deeplethe/utopia](https://github.com/deeplethe/utopia) | **Apache-2.0** (11.353 B) | 8.745 | ✅ sin cláusula | El más grande; push 2026-11-22 |
| [nv-tlabs/lyra](https://github.com/nv-tlabs/lyra) | **Apache-2.0** (11.356 B) | 2.888 | ✅ sin cláusula | Salida **3D**, no video — equivalente funcional de HY-World 2.0 |
| [microsoft/mineworld](https://github.com/microsoft/mineworld) | MIT | 497 | ✅ sin cláusula | ⚠️ sin push desde 2026-05-08 |

**Paso 4 — decidir con una medición, no con un README.** Antes de comprometer
un sustituto, correr los tres candidatos contra
[galilai-group/stable-worldmodel](https://github.com/galilai-group/stable-worldmodel)
(**MIT**, 2.315 ★), que existe exactamente para eso: evaluación reproducible de
world models. Es la pieza que convierte "elegimos Matrix-Game" en "elegimos
Matrix-Game **porque midió así**".

⚠️ **El límite de esta receta, igual que en la Receta 15: cubre el código, no
los pesos.** `huggingface.co` no resuelve en este entorno (5º pass consecutivo
que se declara). **La licencia de los pesos de los cuatro sustitutos sigue sin
verificar** y va al manifiesto como riesgo abierto, no como casilla tildada.

---

## 🆕 Receta 26: **Auditoría de ruta de datos COPPA** — la receta que decide la topología del NPC antes de que alguien escriba un prompt

**Para qué.** Desde el **22-abr-2026** (COPPA enmendada, 90 FR 16970), un juego
estadounidense dirigido a menores necesita **consentimiento parental verificable
separado y desagrupado** antes de divulgar datos personales de un menor a un
tercero **para entrenar o desarrollar IA**. La FTC fundamenta que esa divulgación
**"no se considera, en general, integral al servicio"** — por eso no puede ir
empaquetada con el consentimiento del juego.

**Por qué rompe el patrón por defecto.** El NPC conversacional estándar manda la
frase del jugador a una API de LLM de un tercero. Si el jugador es un menor y el
tercero puede entrenar con eso, **el patrón entero queda del lado equivocado de
una regla que ya está en vigor.** No es una mejora pendiente: es una exposición
con fecha pasada.

**Paso 1 — clasificar el título, antes que nada.** ¿Es *child-directed*? Si la
respuesta es "no, pero tenemos jugadores menores", esta receta aplica igual a la
porción de la base de usuarios con edad conocida. Encadena con la **Receta 18**
(clasificación de audiencia por edad), que ya existe en este archivo para el
Art. 50 europeo: **la misma clasificación sirve para las dos jurisdicciones y se
hace una sola vez.**

**Paso 2 — inventario de rutas de datos.** Una fila por salida del dispositivo:

| Ruta | Qué sale | Destino | ¿Entrena con ello? | Veredicto |
|---|---|---|---|---|
| NPC conversacional | **texto libre del jugador** | API de LLM de terceros | según contrato | 🔴 **VPC separado** o rediseño |
| Voz → texto | audio del jugador | STT de terceros | según contrato | 🔴 idem |
| Telemetría de gameplay | eventos, sin texto libre | propio | no | 🟢 fuera de alcance |
| Moderación | texto del jugador | clasificador de terceros | según contrato | 🟡 revisar contrato |

**Paso 3 — elegir una de las tres salidas técnicas. Presentarlas juntas, porque
el cliente elige por coste y las tres cuestan distinto:**

1. **Inferencia on-device.** No hay divulgación a terceros ⇒ el requisito de VPC
   por entrenamiento **desaparece** (no se "cumple": deja de aplicar). Es la
   única salida que elimina el problema en vez de gestionarlo. Coste: modelo
   chico, calidad menor, presupuesto de memoria en consola/móvil.
2. **Contrato con opt-out de entrenamiento verificable.** Sigue habiendo
   divulgación, así que sigue habiendo obligaciones COPPA — pero **la rama
   específica de entrenamiento de IA se cierra por contrato**. Coste: negociación
   y una cláusula que alguien tiene que auditar cada renovación.
3. **Rediseño a intents cerrados.** El NPC nunca transmite entrada libre: el
   jugador elige entre opciones y lo que viaja es un identificador, no texto.
   Coste: se pierde la conversación abierta, que suele ser la razón por la que
   el cliente quería el NPC.

**Paso 4 — si queda alguna ruta 🔴, construir el segundo flujo de consentimiento.**
Es trabajo de producto, no de legal: una pantalla **separada**, con la opción de
**aceptar el juego y rechazar el entrenamiento**, telemetría que registre cuál
eligió cada padre, y un *kill switch* que apague la ruta para los que
rechazaron. **Si el juego deja de funcionar cuando el padre dice que no, el
consentimiento estaba empaquetado** — y ése es exactamente el supuesto que la
regla prohíbe.

**Composición concreta con lo que ya lista esta KB.** Para la salida nº1
(on-device), el camino más corto es
[undreamai/LLMUnity](https://github.com/undreamai/LLMUnity) (inferencia local
dentro de Unity, ya en `agents/top.md`) para el modelo, más
[CoplayDev/unity-mcp](https://github.com/CoplayDev/unity-mcp) (**MIT**, 16.104 ★)
o [hatayama/unity-cli-loop](https://github.com/hatayama/unity-cli-loop)
(**MIT**, 602 ★) para que el agente de desarrollo pueda **correr el juego y
verificar en Play Mode que ninguna ruta roja quedó viva** — que es la parte que
normalmente se verifica leyendo código y por eso se escapa.

⚠️ **Procedencia y límite.** Toda la base normativa de esta receta viene de
**fuentes secundarias** (blogs legales y de compliance): `WebFetch` no resuelve
hosts generales en esta sesión y **90 FR 16970 no pudo leerse**. Las fuentes
además discrepan sobre si el 22-abr-2026 es fecha de cumplimiento de una regla
efectiva desde jun-2025 o fecha de entrada en vigor. **En sustancia no cambia
nada —hoy aplica— pero esta receta no sustituye a un abogado estadounidense, y
decirlo forma parte del entregable.**

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
| 7 · **Marcado Art. 50(2) EU AI Act** | c2pa-python (**MIT *o* Apache-2.0** — elegir Apache-2.0 por la patente) + inventario de procedencia | 🔴 **reencuadrada: inventario en días, marcado parcial, plan fechado** | 🔴 **Deadline externo a 10 DÍAS: 2-dic-2026.** 🆕 **Y el inventario se recorta dos veces antes de empezar: la R27 saca el contenido efímero de runtime y las etapas intermedias de pipeline cerrado** |
| 🆕 8 · World model sobre engine | Godot/Unity + Matrix-Game 4.0 (Apache-2.0 en v4) | 4-6 semanas | Prototipo visual sin pipeline de arte |
| 9 · **QA con agentes, medido** | GamingAgent/LMGame Bench (MIT, ⚠️ congelado hace 1 año) + MCP | 1-4 semanas | QA con número defendible, no anécdota |
| 🆕 43 · **Fábrica de build permisiva** | git-lfs (MIT) + game-ci (MIT) + sccache (Apache-2.0) + autopilot (Apache-2.0) | 1-2 sem (Unity) · 3-5 sem (Unreal) | Cadencia nocturna build→test→playtest; **y el costo de pipeline de Unreal en la estimación, no en el mes 3** |
| 🆕 44 · **Gate de canal 5 (historia de la licencia)** | `git log --follow -- LICENSE` + tags por fecha | **2 min por repo** | Detecta relicenciamientos que las Recetas 10/14/17/20/21 **no pueden ver**. Copiar a toda la familia de KBs |
| 🆕 45 · **Plataforma con grant partido** | AYON: servidor FSL-1.1 + clientes Apache-2.0 | 1 día de diseño | El entregable queda del lado permisivo y se puede prometer por escrito |
| 🆕 10 · **Auditoría de licencias** | 2 canales + checksum de bytes (sin dependencias) | **2-4 días** | 🟢 **4/4 de acierto sobre las tablas de esta KB.** Entregable propio y auditable |
| 🆕 11 · **Pipeline de estudio con agentes** | Claude-Code-Game-Studios (MIT, 40.377★) + MCP de engine + blender-skills | 1-2 días setup, **3-6 sem de calibración** | Cubrir roles, no acelerar tareas |
| 🆕 12 · **Capacidad elástica de game servers** | Agones (Apache-2.0, 7.107★) + Nakama/Talo + Open Match | 2-3 semanas (K8s listo) | Escalar sin sobreaprovisionar |
| 🆕 13 · **Disclosure por reloj de sesión (NPC con LLM)** | Godot + LimboAI/Beehave (MIT) + `SessionClock` + log append-only + NPCTalk (MIT) | **1-2 semanas** (3-5 días si ya hay BT) | 🔴 **Vigencia externa 1-ene-2027 (WA) · cadencia 1 h/3 h (CT)**. Preparación, no cumplimiento: la definición de "companion chatbot" para NPCs sigue abierta |
| 🆕 14 · **Auditoría de ALCANCE de licencias (6 ejes)** | 2 canales + 11 nombres de archivo + 3 discriminadores de bytes | **medio día** sobre stack inventariado | 🟢 Sobre esta KB: 4/4 reclasificaciones, 1 rechazo revertido, 3 repos **más** permisivos, 2 licencias propias destapadas |
| 🆕 15 · **Gate de licencia por región en CI** | Manifiesto YAML de 6 campos + job de release + sustitutos predeclarados | **2-3 días** | 🟢 Evita descubrir en el lanzamiento que un componente **no se licencia** en el mercado de destino |
| 🆕 16 · **Gate de licencia para packs de skills y prompts** | `gate-skills.sh` + 3 discriminadores de bytes + awesome-gamedev-agent-skills (Apache-2.0 + `NOTICE`, 9.929★) | **1-2 días** + 0,5 día/repo | 🔴 **3 de 6 packs de gaming nuevos >400★ son copyleft** y entran con un `cp -r`: ningún gate de dependencias los ve |
| 🆕 17 · **Auditoría de grant por ENUMERACIÓN** | `gh api search/code filename:LICENSE` + `THIRD_PARTY_NOTICES` + discriminador de byte-identidad + regla del modelo base | **1 día/stack** | 🟢 **4 de 12** repos recomendados tenían grant no representado (LimboAI = MIT + **CC BY 4.0**); Matrix-Game: **7** archivos, no 2; **1er grant de pesos verificado** de la KB |
| 🆕 18 · **Clasificación de audiencia por edad antes del Art. 50** | Columna PEGI/ESRB en el inventario de la R7 + edad como parámetro de runtime (no constante de build) | **2-3 días** | 🟢 Libera semanas de ingeniería: la obligación europea **depende de la audiencia**, no de tener un NPC |
| 🆕 19 · **QA autónomo con agente de playtesting OSS** | [ramenvr/autopilot](https://github.com/ramenvr/autopilot) (Apache-2.0, 881★, UE 5.4–5.8 / Unity 6 / Godot 4 / Roblox) + MCP + LMGame Bench | **3-5 días** (loop MCP) · 2-3 semanas (gate CI) | 🟢 Reemplaza la dependencia de `GamingAgent`, **congelado desde 2025-11-16**. Cierra el loop de las R6/R11: el agente que escribe el código **prueba su propio cambio**. ⚠️ Compilar de fuente si hay revisión de seguridad; las cifras del vendor no están verificadas |
| 🆕 20 · **Auditoría de licencias CORREGIDA** | Canal 3 → canal 2 **por rama** + dedupe por `sha256` + 3 discriminadores (íncipit / apéndice / texto post-`END OF TERMS`) | **medio día** por stack | 🔴 **Corrige 3 defectos medidos del checksum**: Apache tiene **dos** valores válidos (falso negativo en `bevy` y `libgdx`), un conteo **por encima** del canónico son cláusulas añadidas (NVIDIA 15057 B), y **9977 B ≠ Apache** (`Defold License`, field-of-use). 8º eje: **partición por rama** (`ml-agents` 572 B vs 11.324 B) |
| 🆕 25 · **Sustitución de world models bloqueados por TERRITORIO** | Matriz de 5 grants medidos + manifiesto YAML con rutas y `longest-prefix-wins` + sustitutos Apache/MIT + [stable-worldmodel](https://github.com/galilai-group/stable-worldmodel) (MIT) para decidir con medición | **1-2 días** por stack | 🔴 **Las TRES releases abiertas de world models de Tencent (≈5.300★) no otorgan licencia en EU, UK ni Corea.** Extiende la R15 con el caso que ésta no cubría: dentro de `HY-WorldPlay` hay un subárbol **Apache-2.0** (`worldcompass/`) y, un nivel más abajo, **reaparece** la licencia Tencent |
| 🆕 26 · **Auditoría de ruta de datos COPPA (NPC infantil)** | Clasificación de audiencia (reusa la R18) + inventario de rutas de salida + 1 de 3 salidas: on-device ([LLMUnity](https://github.com/undreamai/LLMUnity)) / opt-out contractual / intents cerrados + verificación en Play Mode con [unity-cli-loop](https://github.com/hatayama/unity-cli-loop) (MIT) | **1-2 semanas** | 🔴 **Obligación con fecha YA VENCIDA: 22-abr-2026** (COPPA enmendada, 90 FR 16970). Exige **VPC separado y desagrupado** antes de divulgar datos de un menor a terceros **para entrenar IA** ⇒ el patrón "NPC contra LLM hosteado" **no es implementable tal cual** en un juego infantil de EE.UU. Comprador: legal+privacy, con presupuesto propio |
| 🆕 27 · **Memo de factibilidad Art. 50(2) para runtime** | Partición pipeline/runtime-efímero + memo de infactibilidad (6 puntos) + disclosure de sesión en producto | **2-3 días** por título ya inventariado | 🟢 **El recorte de alcance más grande del pass.** 🔵 Las guías adoptadas el **5-nov-2026** (`C(2026) 7702 final`) eximen del marcado al contenido **efímero generado en tiempo real** (videojuegos y VR citados) **si** se demuestra infactibilidad técnica **y** se da disclosure de sesión. Convierte un sprint de 3-5 semanas en una **decisión documentada**. ⚠️ Base **secundaria**: leer el original antes de firmar |
| 🆕 28 · **Elegir servidor de juego sin heredar un cadáver** | Decisión framework vs backend terminado + filtro `archived`/`pushed_at` **antes** que el de licencia: [skynet](https://github.com/cloudwu/skynet) · [pitaya](https://github.com/topfreegames/pitaya) · [zfoo](https://github.com/zfoo-project/zfoo) · [pinus](https://github.com/node-pinus/pinus) vs [nakama](https://github.com/heroiclabs/nakama) · [Talo](https://github.com/TaloDev/backend) | **medio día** la selección documentada | 🟢 Capa nueva en la KB (5 frameworks, 24.000 ★, todos permisivos). ⛔ Evita el error caro: los dos primeros resultados por estrellas de la categoría (`NetEase/pomelo` 11.855 ★, `SerpentAI` 6.987 ★) **están archivados**. 🔴 Declarar el linaje **pomelo→nano→pitaya** antes de comparar |

### Cuál proponer según la región del engagement

| Región | Receta de entrada | Por qué |
|--------|-------------------|---------|
| **EMEA** | 🔴 **primero el chequeo Art. 5, luego 🆕 FASE 0 de la R7 (partir el catálogo por fecha de puesta en el mercado), luego 18 (clasificar audiencia), luego 7 reencuadrada, luego 20, 17 y 15** · 🆕 **quedan 10 días** | Fecha legal dura **a 10 días** (2-dic-2026) y un segundo reloj en 2-dic-2027. 🆕 **Y el alcance es más chico de lo que vendíamos: el 2-dic cubre SÓLO el Art. 50(2) y SÓLO el catálogo puesto en el mercado antes del 2-ago-2026** (base legal citada: **Art. 111(4)**). Un título lanzado después **nunca tuvo gracia** y el Art. 50(1) **vence para todos desde agosto** ⇒ la Fase 0 decide si es un sprint o una remediación. 🆕 **C2PA no es obligatorio: el Acta no nombra tecnología de marcado.** 🆕 Sanción **15 M€ / 3%**. **Y antes del marcado: si hay NPC conversacional conectado a la tienda, puede ser práctica PROHIBIDA (Art. 5) — eso no se marca, se rediseña.** 🆕 **Y el sprint se recorta ANTES de empezar, con la R27: las guías adoptadas el 5-nov-2026 (`C(2026) 7702 final`) eximen del marcado al contenido efímero de runtime —videojuegos citados explícitamente— y sacan las etapas intermedias de un pipeline cerrado. El trabajo pasa de marcar a DEMOSTRAR, y la condición es disclosure de sesión.** 🆕 **Dato de venta: Europa integra MÁS PROFUNDO que Norteamérica (4,2 vs 3,5 /10, Vivrato VAIA 20-oct-2026) ⇒ en EMEA no se vende adoptar, se vende profundizar.** |
| **North America** | 🆕 **13** primero si hay NPC conversacional, luego **6** y **9**, luego 2, **17** y **10** · 🆕 **16 obligatoria** si el entregable incluye packs de skills | Steam exime las herramientas de desarrollo: la AI interna no dispara disclosure ni backlash, y 🆕 **55%** de los devs ve la gen AI como daño (GDC nov-2026; era 52%) y ~85% de los jugadores la rechaza. 🆕 **Adopción estadounidense estancada: 36%→38%, dentro del margen de error** ⇒ acá se vende *primer paso*, no escala. 🆕 **Y ya hay obligación de runtime: Receta 13.** **Dato nuevo: sólo 3% de las declaraciones de AI en Steam son generación en gameplay** — el pipeline interno es el 97% del mercado real. La Receta 10 vende bien donde los reguladores declaran "visibilidad limitada". 🆕 **Añadir 19** (QA con agente OSS): cae del lado **exento** de Steam. 🆕 **Y el dato de rechazo del jugador está en conflicto y hay que llevar las dos cifras:** ~85% de rechazo en abstracto (Quantic Foundry) **pero 43% de ~3.800 usuarios de Steam no tiene problema en comprar un juego con disclosure** (GameDiscoverCo) ⇒ **la fricción de compra medida es mucho menor que el rechazo declarado**, y ésa es la objeción nº1 que frena la R7. 🆕 Censo de **53.597 releases**: 10,9% (2024) → 19,9% (2025) → 30,8% (2026 a julio); **47% de los títulos con <10 reviews declaran AI vs 14% de los de ≥1.000** ⇒ para un AAA, declarar lo pone en el 14%: conversación de **riesgo de marca**. 🆕 **NA mide MENOS profundidad de integración que Europa: 3,5 vs 4,2 /10** (Vivrato VAIA, 20-oct-2026, n=101 líderes) ⇒ el pitch no es adopción —ya ocurrió, ancha y fina— es **integración de pipeline**. 🆕 **Y hay un competidor-socio nuevo con nombre: Ubisoft Motherbrain declara que va a servir a creadores *fuera* de su ecosistema** (NPC con memoria, AI game masters). Sin producto ni fecha anunciados. ⚠️ Despidos: esta KB publica 166, otras fuentes de la misma semana dicen 116 ⇒ **llevar las dos o ninguna**. |
| **APAC** | **8** y **11**, luego 7 adaptada, **17**, **10** y 🆕 **15** | Los world models abiertos punteros son de APAC (Skywork, Tencent) y Corea/China ya obligan etiquetado (Corea **extraterritorial** desde 22-ene-2026). **Y es la única región con NPCs con AI a escala de producción probada: Tencent reporta 110M de usuarios acumulados y 17,7M de DAU pico en *Game for Peace*** — acá la conversación no es "si funciona", es integración. La Receta 10 es obligatoria si se construye sobre I+D china (Hunyuan no es OSI; 🔴 **HY-World 2.0 sí publica licencia — `License.txt` — y excluye EU, UK y Corea**). 🆕 **Adopción japonesa 85,8% con 63% de uso diario (CESA 2026, 1.349 respuestas)** ⇒ acá no se vende adopción: se vende **gobernanza, escala y evaluación de calidad de salidas** (la preocupación nº1 declarada es precisión/calidad, 51,9%). 🆕 **Corea entra con cifra propia: 77,5% de 120 empresas** (KOCCA Q2-2026) y *"AI skills"* como **prioridad nº1 de contratación**; **JOGA: 100%** de las empresas japonesas de juego online. 🆕 **Y la práctica declarada del cliente YA coincide con nuestro entregable:** los estudios japoneses **restringen qué herramientas usa el personal** y **prohíben que salidas crudas lleguen al producto** ⇒ **15 y 16 no hay que justificarlas, hay que implementarlas**, y **19** es AI que no toca contenido de jugador. 🆕 **El embudo chino se está ABRIENDO, no cerrando: ~1.571 aprobaciones NPPA hasta sept-2026, +23% interanual** (🔵 fuentes secundarias que **no coinciden** en el agregado) ⇒ el cuello de botella dejó de ser la licencia. 🆕 **Segunda prueba de generación embarcada de cara al jugador: NetEase anuncia un "sistema de narrativa infinita" en *Justice Online Mobile*** (se suma a Tencent). 🆕 **Y la infraestructura de servidor de la R28 es mayormente de APAC y ENTERAMENTE permisiva** (skynet, zfoo, pinus) — a diferencia de los world models de la región, que traen exclusión territorial: **son dos capas distintas, no se juzgan juntas**. |
| **LATAM** | **1**, **3**, **5** y **12** (y **13** sólo si se publica en EE.UU.; **16** si el entregable lleva packs de skills) | No hay AI Act que vender; sí hay **48,8% de jugadores que compraría juegos hechos con AI** —contra ~85% de rechazo global— lo que hace de LATAM **el mercado de primer lanzamiento racional para AI de cara al jugador**, no el último. Más 86,9% de mobile en México y operadores iGaming con supervisión creciente. ⚠️ Cuidar la narrativa laboral: 40,6% de preocupación por precariedad (PGB Latam 2026), y Ubisoft acaba de ponerle titular. 🆕 **El único compliance con fecha en LATAM es publicitario, no de producto: guía CONAR de Brasil en vigor desde el 1-jun-2026** (el PL 2338/2023 sigue trabado en la Cámara ⇒ **no hay ley general de AI en vigor**). 🆕 **El constraint está nombrado por la industria y no es tecnológico: es FINANCIAMIENTO** ⇒ **el entregable tiene que pagarse solo dentro del trimestre**; un programa a 12 meses no tiene de dónde financiarse, una R1/R3 acotada sí. 🆕 **Estructura: Brasil ~13.225 devs / ~1.042 estudios activos** (el talento), **México 72,6 M de jugadores = 61% de la población 6+** (el jugador) — **son dos conversaciones distintas**. ⚫ **Cero medido con TRES instrumentos: no existe encuesta de adopción de gen AI por estudios LATAM** (🆕 tercera confirmación en el cycle 11, buscando en inglés y acotando a Brasil y México) ⇒ la cifra vendible de la región sigue siendo de **jugadores**, no de estudios. 🆕 **Lo cualitativo que sí hay, y sirve de contexto: >1.000 estudios en Brasil explorando AI para prototipado y localización (ABRAGAMES), y el informe GamesBeat LATAM 2026 — ninguno con porcentaje.** ⚠️ **Y un candidato a verificar, no a publicar: [pitaya](https://github.com/topfreegames/pitaya) (MIT, 2.847 ★) suele asociarse a un estudio brasileño y esta sesión NO pudo confirmarlo** (`gh api users/<org>` bloqueado) ⇒ la casilla *repos de infra de origen LATAM* sigue en **cero medido**. |

---
*Pass **2026-11-21 (cycle 6)**: recetas nuevas **16** (gate de licencia para
packs de skills y prompts: 3 de 6 packs nuevos de gaming con >400★ son
LGPL-3.0/AGPL-3.0 y la ruta de contaminación es un `cp -r` invisible para
cualquier gate de dependencias; incluye el script, los 3 discriminadores de
bytes y la honestidad sobre que "un prompt es software" no está resuelto —el
riesgo real es de **aceptación del entregable**), **17** (auditoría de grant
**por enumeración** con `gh api search/code`, el inventario de componentes
vendorizados de `THIRD_PARTY_NOTICES.md` que abre el **7º eje** de partición, el
**discriminador de byte-identidad** que separó 2 falsas alarmas de 2 hallazgos
reales, y la **regla del modelo base** que cerró el primer grant de pesos de la
historia de esta KB sin acceder a Hugging Face) y **18** (clasificar audiencia
**por edad** antes del sprint del Art. 50, porque según WH Partners un
single-player para adultos puede no necesitar etiqueta: etiquetar todo no es ir
a lo seguro, es gastar ingeniería en filas que no la necesitan con 11 días de
plazo; incluye la tabla de exposición por jurisdicción con **6 órdenes de
magnitud** entre el techo europeo y el coreano). Receta **10** gana el trap de
los **14 bytes**; **14** pasa de 6 a **7 ejes**; **15** queda explícitamente
incompleta sin la **16**. Secuencia recomendada: **18 → 7 → 10 → 14 → 17 → 15 →
16**.*

---
## 🆕 Receta 21: Verificación de **integridad** del archivo de licencia — el control que las Recetas 10, 14, 17 y 20 no tienen

**Problema que resuelve.** Las cuatro recetas de auditoría de licencias de esta
página responden *"¿qué dice el grant?"* (R10 checksum, R14 alcance, R17
enumeración, R20 íncipit). **Ninguna responde antes: "¿es este archivo un
documento completo y bien formado?"** — y si no lo es, las cuatro producen un
veredicto con cara de válido. Caso real que lo motiva:
`Farama-Foundation/Minigrid`, 2.543 ★, benchmark RL estándar (cycle 8).

**Síntomas que el control existente NO caza:**

- Un `LICENSE` de **2.507 B** cuyo íncipit es *"Apache License, Version 2.0"* ⇒
  el **íncipit dicta Apache-2.0** (R20, correcto) y el **checksum lo ubica en
  *"Apache sin apéndice"*** (R10, categoría real: `libgdx` tiene 10.174 B).
  **Ambos controles pasan.** Pero el archivo contiene **sólo las §§ 1 y 9**:
  faltan **§ 2 (Grant of Copyright License), § 3 (Grant of Patent License) y § 4
  (Redistribution)** — es decir, **falta el grant**.
- En la línea 18 hay **`@@ -154,49 +194,23 @@`**: un **hunk de diff** commiteado
  dentro del archivo. El `LICENSE` es un **parche a medio aplicar**.

### Fase 0 — Integridad (nueva; corre ANTES de cualquier interpretación)

```bash
S=owner/repo
curl -sS -f "https://raw.githubusercontent.com/$S/HEAD/LICENSE" -o L.txt || echo "NO EXISTE"

# 1. Artefactos de control de versiones dentro del archivo  →  debe dar 0
grep -cE '^\s*(@@|<<<<<<<|=======|>>>>>>>|\+\+\+ |--- )' L.txt

# 2. Cláusulas operativas presentes  →  Apache-2.0 debe dar 9
grep -cE '^ *[0-9]+\. ' L.txt

# 3. Para Apache-2.0, las tres que de verdad importan  →  deben estar las 3
grep -cE '^ *(2|3|4)\. (Grant of Copyright|Grant of Patent|Redistribution)' L.txt

# 4. Para MIT: los tres párrafos operativos
grep -ciE 'Permission is hereby granted|above copyright notice.*shall be included|AS IS.*WITHOUT WARRANTY' L.txt
```

**Criterio de parada.** Si (1) ≠ 0 **o** (2) < 9 en un archivo que se
auto-declara Apache-2.0 ⇒ **el archivo NO es el grant**. **No se interpreta: se
enumera** (R17) y se busca el documento real:

```bash
gh api -X GET search/code -f q="repo:$S filename:LICENSE" --jq '.items[].path'
# Minigrid → LICENSE  +  docs/LICENSE   ← el segundo es el verdadero (12.201 B)
```

### Fase 1 — Desempate manifiesto vs `LICENSE`

Si el manifiesto (`pyproject.toml`, `package.json`, `Cargo.toml`, `*.csproj`,
`pom.xml`) y el `LICENSE` **discrepan en la familia de licencia**, la T63
(*"leé el manifiesto, no el anuncio"*) **no desempata**: ninguno de los dos es un
anuncio, los dos están versionados.

```bash
curl -sS "https://raw.githubusercontent.com/$S/HEAD/pyproject.toml" | grep -iE '^license|License ::'
# Minigrid → license = { text = "MIT License" }   vs   LICENSE → "Apache License 2.0"
```

⇒ **Regla: discrepancia de familia = ambigüedad abierta, NO veredicto.** Se
resuelve con el tercer documento. En `Minigrid`, `docs/LICENSE` (12.201 B) dice
que el repo es **composición MIT + Apache-2.0** y **explica** la discrepancia:
hay código tomado de proyectos bajo ambas licencias, con los dos textos
incluidos. 🟢 **Veredicto final: permisivo y usable, citando `docs/LICENSE`.**

### Fase 2 — Control de vecindario (evita propagar la sospecha)

Un repo dañado **no implica un org dañado**. Correr Fase 0 sobre 2-3 hermanos:

```bash
for S in Farama-Foundation/Gymnasium Farama-Foundation/PettingZoo; do
  echo -n "$S: "; curl -sS "https://raw.githubusercontent.com/$S/HEAD/LICENSE" | wc -c
done
# Gymnasium 1104 B MIT, PettingZoo 1073 B MIT, 0 artefactos  →  org sano
```

### Qué entregar al cliente

Una línea por dependencia: **`slug | archivo citado | bytes | familia | integridad
(OK / DAÑADO→ruta real) | fuente del desempate`**. La columna de integridad es la
que no existía, y es la que evita las dos fallas opuestas de este caso:
**rechazar `Minigrid` por "le falta el grant"** (falso negativo: el grant existe
en otra ruta) y **declararlo "Apache-2.0"** (falso positivo: es MIT + Apache-2.0
compuesto, y el archivo citado estaría roto).

**Costo real:** 4 comandos por dependencia, ~1 s cada uno. **Se corre sobre todo
el `requirements.txt`, no sobre una muestra.**

*Pass **2026-11-21 (cycle 5)**: recetas nuevas **13** (capa de disclosure por reloj de
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

---
*Pass **2026-11-21 (cycle 7)**: recetas nuevas **19** (QA autónomo con
[ramenvr/autopilot](https://github.com/ramenvr/autopilot), Apache-2.0 verificado
sobre fuente real — 651 archivos, 50 cpp/81 cs/47 gd/156 py —, multi-engine
UE 5.4–5.8 / Unity 6 / Godot 4 / Roblox Studio y con MCP, que **reemplaza la
dependencia de `GamingAgent` congelado desde 2025-11-16**) y **20** (auditoría de
licencias **corregida**: el checksum de bytes tiene **tres defectos medidos** —
Apache-2.0 con **dos** valores legítimos 11358/10173 que producen **falso
negativo en `bevy` y `libgdx`**; un conteo **por encima** del canónico = cláusulas
añadidas, con el inventario de dependencias vendorizadas **dentro** del
`LICENSE` de NVIDIA, 15057 B, que la enumeración de la R17 no caza; y la
`Defold License 1.0` a **196 bytes** del Apache sin apéndice, que el conteo no
puede distinguir — más el **8º eje de partición del grant: por RAMA**,
`ml-agents` con el mismo `LICENSE.md` en **572 B en `develop`** y **11.324 B en
`main`**, y el defecto de instrumento **`raw` aliasea `master`→`main`** que
obliga a deduplicar por hash). **Receta 7 reencuadrada por TERCERA vez**, ahora
con base legal (**Art. 111(4)**) y una **Fase 0** nueva y previa al inventario:
partir el catálogo por **fecha de puesta en el mercado**, porque el 2-dic-2026
cubre **sólo el Art. 50(2)** y **sólo lo anterior al 2-ago-2026** — un título
lanzado después **nunca tuvo período de gracia**, y el Art. 50(1) vence para
todos desde agosto. **C2PA queda explícitamente como elección de ingeniería, no
como requisito legal** (el Acta no nombra tecnología de marcado). 2026-11-21.*
