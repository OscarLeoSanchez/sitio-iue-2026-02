# Guía de diseño de las presentaciones interactivas

Fuente de verdad versionada en el repositorio `Sitio/`. La skill `presentaciones-interactivas` (en `Clases/.claude/skills/`) solo apunta a este archivo. El kit vive en esta misma carpeta: `base.html` y `LEEME.md`.

El docente aprobó este diseño y esta dinámica ("me gustaron mucho"). Cuando pida una presentación,
úsalo por defecto. No propongas revealjs ni otro tema salvo que lo pida.

Referencias terminadas, para ver el resultado antes de construir:

- `Sitio/web/semana-07/presentacion-interactiva.html` (55 diapositivas, DOM y JavaScript).
- `Sitio/web/semana-08/presentacion-interactiva.html` (53 diapositivas, fetch y APIs).

## Qué contiene esta skill

- `Sitio/_kit-presentaciones/base.html`: un solo archivo con motor, CSS, componentes y 14 diapositivas de galería.
- `Sitio/_kit-presentaciones/LEEME.md`: marcado, atributos `data-*`, API de JavaScript y reglas de cada componente.
  Léelo COMPLETO antes de escribir una diapositiva.

## Procedimiento

1. Lee `Sitio/_kit-presentaciones/LEEME.md` y `base.html`. Lee también la guía de la semana si existe (las
   Partes de la guía y los nombres de conceptos deben coincidir con la presentación).
2. Copia el motor y el CSS del kit tal cual. No rediseñes la identidad. Reemplaza solo la galería
   por el contenido nuevo. Si el contenido pide un componente que el kit no tiene, añádelo en tu
   archivo con la misma calidad visual.
3. Escribe la salida en `Sitio/<curso>/semana-NN/presentacion-interactiva.html`, un solo archivo.
4. Regístrala en `resources` de `Sitio/_quarto.yml` (ningún `.qmd` la incluye, así que Quarto no
   la copia a `docs/` si falta) y enlázala con un botón al inicio de la guía de la semana.
5. Ejecuta el bucle de revisión de abajo. Después `quarto render` desde `Sitio/`.

## Diseño aprobado (no negociable)

- Fondo claro y cálido (papel marfil con rejilla de puntos muy suave y manchas orgánicas de color
  en las esquinas). Nada de barras azul oscuro, hoja blanca angosta ni cajas con sombra pesada.
- Paleta con significado fijo: estructura y DOM en cielo (azul), datos y estado en hoja (verde),
  eventos y acciones en maíz (amarillo), errores en tomate (rojo), red y asincronía en uva
  (violeta). Cada sección toma un color de acento.
- Titulares grandes y contundentes, mucho aire, una idea por diapositiva. Cartas redondeadas con
  borde fino y sombra difusa.
- Ilustraciones SVG propias (tomate, aguacate, papa, panela, canasta, navegador, servidor,
  paquete, nube, reloj), de trazo y relleno pastel. Cero emojis.
- Controles flotantes discretos: píldora inferior con puntos de progreso, chip de sección arriba
  a la izquierda, iconos arriba a la derecha (ayuda "?", índice, glosario, apoyo, pantalla
  completa).

## Dinámica pedagógica aprobada

Cada concepto sigue el mismo ciclo: problema real del proyecto, PREDICE (el estudiante apuesta
antes de ver la respuesta), modelo mental con diagrama animado, simulación que el estudiante
manipula, código con líneas vinculadas al efecto visible, y reto o falla provocada.

- Toda diapositiva de concepto lleva al menos un diagrama o animación. Apunta a 45 a 55
  diapositivas y más de 35 animaciones o diagramas distintos.
- Componente estrella: `Flujo`, con paquetes que viajan, narración por paso, pausa, paso a paso,
  velocidad, barra arrastrable y parámetros que recalculan el recorrido.
- En cada concepto: desplegable "Profundiza" (definición, analogía, ejemplo, error común),
  términos con definición emergente enlazados a un glosario de 60 o más entradas, y panel de apoyo
  con analogía, errores frecuentes y nombres de documentación (MDN).
- Código del estudiante solo en `Ejecutor` (iframe `sandbox="allow-scripts"`, con límite de 2 s).
- Cierre: mapa conceptual animado, errores frecuentes y autoevaluación formativa sin nota.

## Reglas de contenido del docente

- Sin nombre del docente, asignatura, grupo, unidad ni semana como ficha o bloque en la portada.
  Portada limpia: título, frase de enfoque, ilustración.
- Sin bloque grande de "cómo se usa esta presentación": lo resuelve el botón "?".
- Sin actividad, entregable ni evaluación cuando la semana no los tiene. Nunca pongas rúbricas ni
  material evaluativo: el sitio es público.
- Sin relleno ni texto sin sentido. Sin enlaces a Móvil ni a la portada del sitio.
- Texto de §12 de `AGENTS.md`: español de Colombia, sin raya larga, punto medio, comillas
  tipográficas, flechas unicode ni emojis; tildes correctas.

## Bucle de revisión obligatorio (el docente lo pidió)

Mínimo 4 rondas con navegador real (chrome-devtools). En cada ronda captura y MIRA todas las
diapositivas a 1280x720 con los fragmentos revelados y revisa: consistencia visual, desbordes,
errores técnicos (contra MDN o Chrome DevTools Docs, ejecutando los ejemplos), coherencia con la
guía, texto sin sentido, calidad profesional del diseño, que cada animación y simulación
funcione con clics reales y consola limpia. Corrige y repite; la última ronda cubre todas las
diapositivas y no encuentra nada. Prueba también a 375x667 con emulación de dispositivo real,
`node --check` sobre el JS y la búsqueda de caracteres prohibidos de §12.

## Defectos conocidos del kit

Se corrigieron dentro de las presentaciones, no en `base.html` del kit. Porta el arreglo cuando
reutilices el componente:

- `data-manual` vacío creaba igual un reto con verificación automática.
- El paquete del último paso de un `Flujo` quedaba encima del nodo destino.
- El `fetch` simulado del `Ejecutor` no respetaba la cancelación, fallaba con respuestas 204 y no
  dejaba ver qué recibía el servidor. Versión corregida: `Sitio/web/semana-08/presentacion-interactiva.html`.
- Los chips conmutadores marcaban dos opciones si la segunda empezaba seleccionada, y los retos con
  verificación propia se inicializaban dos veces. Versión corregida: `Sitio/web/semana-07/presentacion-interactiva.html`.

Ya corregido en el kit: el visor de árbol DOM ejecutaba `onerror` al dibujar HTML (ahora usa un
documento inerte).

## Trampas de proceso

- Sirve con `python -m http.server <puerto propio>` y cierra solo tu servidor. No termines
  `python.exe` en bloque: tumba los servidores de otros agentes.
- Un archivo de 600 KB es normal aquí. Si delegas, usa un agente por presentación, dale el kit y
  esta skill, y exige el bucle de revisión. Los agentes largos pueden cortarse por el límite de
  uso de la API: guardan avances en el archivo final y se reanudan con SendMessage.
- No quedó probado: lector de pantalla, Firefox, Safari y `prefers-reduced-motion` real.
