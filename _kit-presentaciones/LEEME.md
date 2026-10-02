# Kit de presentaciones interactivas

Motor, estilos y componentes para las presentaciones HTML de Programación Web. Todo vive en un solo
archivo, `base.html`, que funciona sin conexión y sin librerías. Este documento es la referencia
completa: con él y con `base.html` puedes construir de 40 a 50 diapositivas sin adivinar nada.

## 1. Cómo empezar

1. Copia `base.html` con el nombre de tu presentación.
2. Cambia `<title>` (2 a 4 palabras) y `<meta name="description">`.
3. Borra todo lo que hay entre `<!-- INICIO DIAPOSITIVAS -->` y `<!-- FIN DIAPOSITIVAS -->` y escribe tus `<section class="diapo">`.
4. Reemplaza el último `<script>` (bloque CONTENIDO) por tu glosario y tus componentes. No toques el bloque MOTOR DEL KIT.
5. Sirve la carpeta (`python -m http.server <puerto>`), abre `base.html#1` y revisa capturas a 1280x720 y a 375x667 con emulación de dispositivo.

Orden de arranque: el motor prepara todo el marcado declarativo (código, callouts, pestañas, quiz, ejecutores) y luego ejecuta las funciones registradas con `Kit.listo(fn)`. Crea siempre los componentes por JS dentro de `Kit.listo`.

```html
<script>
Kit.glosario({ dom: { termino: 'DOM', corta: 'El árbol de nodos que construye el navegador.' } });
Kit.listo(() => {
  Kit.flujo('#mi-flujo', { /* configuración */ });
});
</script>
```

## 2. Diapositivas, secciones y fragmentos

```html
<section class="diapo" data-seccion="El DOM" data-acento="cielo" data-num="01" data-titulo="Título corto">
  <div class="cabeza">
    <p class="antetitulo">Estructura</p>
    <h2 class="titulo">Cada etiqueta es un <em>nodo</em></h2>
  </div>
  ...
</section>
```

| Atributo | Dónde | Efecto |
|---|---|---|
| `data-seccion` | primera diapositiva de cada sección | Nombre de la sección. Las siguientes la heredan hasta que otra diapositiva declare una nueva. |
| `data-acento` | primera diapositiva de la sección (o cualquiera, para sobrescribir) | `hoja`, `maiz`, `tomate`, `cielo` o `uva`. Tiñe manchas del fondo, chip, puntos, `em` del título y antetítulo. |
| `data-num` | primera diapositiva de la sección | Número visible (`01`, `02`). Solo las secciones con número aparecen en el mapa de la sesión. |
| `data-titulo` | cualquier diapositiva | Título para índice, puntos y lector de pantalla. Si falta se usa el primer `h1` o `h2`. |

Clases de diapositiva: `portada`, `seccion-portada`, `centro` (centra vertical). Hash `#n` abre la diapositiva n (empieza en 1).

**Entrada escalonada.** Los hijos directos de `.diapo` entran uno tras otro al llegar. Usa `class="no-entra"` para excluir uno y `data-entra` (con `style="--i:3"`) para animar un elemento interno.

**Fragmentos.** Cualquier elemento con `data-paso` se revela con Espacio, flecha derecha o el botón siguiente. `data-paso=""` numera en orden; `data-paso="2"` agrupa elementos que aparecen juntos. Sirve también para nodos y conectores de diagramas (opción `paso`). La píldora inferior muestra `2 / 4`. En teléfono todos los fragmentos se ven desde el inicio. Clase opcional `atenuar-previos` en la diapositiva: los pasos ya vistos quedan al 50 %.

**Mapa de la sesión.** `<div class="mapa-sesion"></div>` se llena solo con las secciones numeradas, marca la actual y su avance, y navega al hacer clic.

**Diapositiva de sección.**

```html
<section class="diapo seccion-portada" data-seccion="La red" data-acento="uva" data-num="02">
  <span class="seccion-num">02</span>
  <h2 class="seccion-titulo">fetch: hablar con el servidor</h2>
  <p class="seccion-pregunta">¿Qué pasa entre el clic y los datos en pantalla?</p>
  <div class="mapa-sesion" style="max-width: 740px"></div>
  <svg class="seccion-ilu no-entra" viewBox="0 0 330 330" role="img"><title>...</title>...</svg>
</section>
```

**Portada.** `section.diapo.portada` con `p.antetitulo`, `h1.portada-titulo` (con `<em>`), `p.portada-enfoque` y una escena `svg.portada-escena` (560x520). Sin ficha de docente, asignatura, grupo ni semana.

## 3. Presupuesto de espacio (lo que más falla)

- Lienzo fijo de 1280x720 que escala a la ventana. Relleno: 70 arriba, 76 a los lados, 86 abajo. Área útil: **1128 x 564**.
- La cabecera (`antetitulo` + `titulo` de una línea) ocupa unos 100 px. Quedan **unos 440 px** para el cuerpo.
- Medidas reales: flujo a todo el ancho 440 px; línea de tiempo con 4 carriles 400 px; bucle de eventos 440 px; código de 10 líneas a 15 px unos 300 px; quiz de 4 opciones en `.opciones.dos` con retroalimentación 420 px.
- Si algo no cabe, la diapositiva hace scroll, pero queda bajo la píldora inferior: divide la idea en dos diapositivas antes que apretar. Revisa siempre el estado **después** de interactuar (retroalimentación abierta, desplegable abierto, modo experto activo).
- Tamaños: título 50 px, portada 74 px, cuerpo 22 px (`.cuerpo`), `lead` 25 px, notas 17 px, etiquetas de diagrama 16 px mínimo, código 14 a 17 px (`data-tam`).

## 4. Tokens y colores

Definidos en `:root`. Cada color tiene base, `-claro` (fondos) y `-osc` (texto con contraste AA).

| Token | Valor | Significado fijo |
|---|---|---|
| `--papel`, `--papel-2`, `--papel-3` | #FBF8F1, #F3EEE2, #EAE2CF | fondos |
| `--tinta`, `--tinta-suave`, `--tinta-tenue` | #1F2A24, #56615A, #7A837D | texto |
| `--linea`, `--linea-2` | #E4DCC8, #D3C8AE | bordes |
| `--cielo` | #3B82C4 | estructura y DOM |
| `--hoja` | #2F8F5B | datos y estado |
| `--maiz` | #F2B33D | eventos y acciones |
| `--tomate` | #E4572E | errores |
| `--uva` | #7A5BC7 | red y asincronía |

`data-acento="x"` define `--acento`, `--acento-claro`, `--acento-osc`. `data-tono="x"` define `--tono`, `--tono-claro`, `--tono-osc` (acepta también `tinta`) y lo usan etiquetas, botones `tono`/`suave`, comparaciones, deslizadores y fichas. Otros tokens: `--radio` 20px, `--radio-m` 14px, `--sombra`, `--sombra-alta`, `--fuente`, `--mono`, `--curva`.

## 5. Utilidades de diseño

| Clase | Uso |
|---|---|
| `.cabeza`, `.cabeza-fila` | bloque de antetítulo y título; la segunda pone algo a la derecha (por ejemplo un interruptor) |
| `.antetitulo`, `.titulo` (con `<em>` resaltado), `.lead`, `.cuerpo`, `.nota`, `.marca-texto` | jerarquía tipográfica |
| `.dividido` + `data-proporcion` | rejilla de 2 columnas que ocupa el alto restante. Proporciones: `40/60`, `60/40`, `35/65`, `65/35`, `45/55`, `55/45` (sin atributo: 50/50). Los hijos se alinean arriba; flujo, árbol, ejecutor, línea de tiempo, bucle y `.estirar` se estiran al alto completo |
| `.columna`, `.fila`, `.crece`, `.rejilla-2`, `.rejilla-3` | apilado vertical, fila horizontal, ocupar el espacio libre, rejillas |
| `.carta` (`.plana`, `.tenue`), `.carta-titulo` | tarjeta con borde fino y sombra suave; subtítulo en versalitas |
| `.etiqueta` + `data-tono` | píldora con punto de color |
| `.boton` (`.primario`, `.tono`, `.suave`, `.mini`) | botones; `data-tono` para `.tono` y `.suave`; `data-ico="nombre"` antepone un icono |
| `.ventana` > `.ventana-barra` (`.luces`, `.url`) + `.ventana-cuerpo` | navegador simulado para mostrar una página |
| `.tienda-lista` con `li`, `.precio`, `li.oferta`, `li.agotado` | lista de productos de ejemplo |
| `.sr` | texto solo para lector de pantalla |

## 6. Ilustraciones e iconos

Ilustraciones (viewBox 64, trazo tinta y relleno pastel): `tomate`, `aguacate`, `papa`, `panela`, `canasta`, `finca`, `tarjeta`, `navegador`, `servidor`, `basedatos`, `paquete`, `documento`, `nube`, `candado`, `engranaje`, `lupa`, `reloj`, `clic`.

Iconos de interfaz (viewBox 24, trazo 2, `currentColor`): `anterior`, `siguiente`, `indice`, `glosario`, `apoyo`, `pantalla`, `pantalla-salir`, `ayuda`, `play`, `pausa`, `paso-ant`, `paso-sig`, `reiniciar`, `copiar`, `check`, `cerrar`, `buscar`, `mas`, `chevron`, `idea`, `observa`, `cuidado`, `analogia`, `consola`, `reloj`, `pila`, `cola`, `rayo`, `ojo-codigo`, `libro`, `red`, `objetivo`, `candado`, `experto`, `teclado`, `flecha-der`.

```html
<!-- Con texto alternativo -->
<svg class="ilu g" viewBox="0 0 64 64" role="img"><title>Canasta de mercado</title><use href="#ilu-canasta"/></svg>
<!-- Decorativa -->
<span class="ilu xg" data-ilu="tomate"></span>
<!-- Dentro de una escena SVG propia -->
<use href="#ilu-servidor" x="410" y="44" width="96" height="96"/>
<!-- Icono -->
<svg class="ico" aria-hidden="true"><use href="#i-reloj"/></svg>
```

Tamaños: `.ilu` 64 px, `.ilu.g` 120 px, `.ilu.xg` 180 px. Por JS: `Kit.ilu('papa', 'Papa criolla')` y `Kit.ico('reloj')` devuelven nodos SVG. Para escenas usa `<svg viewBox>` con `<title>` y varias `<use>`; las animaciones SMIL se pausan solas con movimiento reducido.

## 7. Componentes de texto y apoyo

### Callout

```html
<div class="callout" data-tipo="idea">Texto con <code>código</code>.</div>
```

`data-tipo`: `idea` (Idea clave, hoja), `observa` (cielo), `cuidado` (tomate), `analogia` (uva). `data-etiqueta` cambia el rótulo.

### Definición emergente y glosario

```html
<dfn data-def="evento">evento</dfn>
<span data-def="Tiempo que tarda un mensaje en ir y volver." data-termino="Latencia">latencia</span>
```

Si `data-def` es una clave de `GLOSARIO`, el popover muestra `corta` y el botón "Ver más en el glosario". Si no, el texto del atributo es la definición. Se abre al pasar el puntero (escritorio), con clic o con Enter. Esc lo cierra.

```js
Kit.glosario({
  fetch: {
    termino: 'fetch',                  // obligatorio
    corta: 'Función que hace una petición HTTP y devuelve una promesa.',  // obligatorio, admite HTML
    larga: 'Definición detallada.',    // opcional, HTML
    analogia: 'Como pedir a domicilio.',
    ejemplo: "const res = await fetch('/api/productos');",  // texto de código, se resalta
    lang: 'js',                        // lenguaje del ejemplo: js, html, css, json
    errores: 'No revisar <code>res.ok</code>.',
    mdn: 'MDN: Usando Fetch',
  },
});
Kit.abrirGlosario('fetch');   // abre y resalta la entrada
```

El buscador ignora tildes y mayúsculas. Tecla G.

### Profundiza (desplegable)

```html
<details class="profundiza" data-grupo="eventos" data-ilu="clic">
  <summary><span class="pf-titulo"><code>addEventListener</code></span><span class="pf-sub">Una línea de resumen</span></summary>
  <div class="pf-cuerpo">
    <div class="pf-bloque" data-tipo="definicion">...</div>
    <div class="pf-bloque" data-tipo="analogia">...</div>
    <div class="pf-bloque" data-tipo="ejemplo"><code>...</code></div>
    <div class="pf-bloque" data-tipo="errores">...</div>
  </div>
</details>
```

- `data-grupo`: los del mismo grupo funcionan como acordeón.
- Icono: `data-ilu="nombre"` (ilustración) o `data-icono="nombre"` (icono). Color: `data-acento`.
- `data-tipo` del bloque: `definicion`, `analogia`, `ejemplo`, `errores`, `nota`. `data-etiqueta` cambia el rótulo (por ejemplo "Corrección").
- Variante `.profundiza.compacta` para listas (errores frecuentes, pistas).
- Abierto ocupa espacio: deja hueco en la columna o usa la variante compacta.

### Panel de apoyo por diapositiva (tecla A)

```html
<template class="apoyo">
  <section data-tipo="definiciones"><p><strong>Evento:</strong> ...</p></section>
  <section data-tipo="analogia"><p>...</p></section>
  <section data-tipo="errores"><ul><li>...</li></ul></section>
  <section data-tipo="mas"><p>En MDN busca <em>EventTarget.addEventListener()</em>.</p></section>
</template>
```

Va dentro de la diapositiva. El botón de apoyo muestra un punto cuando la diapositiva lo tiene. Tipos: `definiciones`, `analogia`, `errores`, `mas`, `nota`. Admite `dfn` y `code`.

## 8. Código

### Bloque de código

```html
<pre class="codigo" id="cod-fetch" data-lang="js" data-titulo="productos.js" data-tam="15" data-resaltar="3">
const res = await fetch('/api/productos');
if (res.status &lt; 400) console.log('ok');</pre>

<script type="text/plain" class="codigo" data-lang="html" data-titulo="index.html">
<ul id="canasta"><li>Panela</li></ul>
</script>
```

- En `pre` escapa `<` como `&lt;` y `&` como `&amp;`. En `script type="text/plain"` no hace falta escapar, pero no puede contener `</script>`.
- `data-lang`: `js`, `html`, `css`, `json`, `texto`. `data-titulo`: nombre de archivo. `data-tam`: tamaño en px (17 por defecto). `data-resaltar`: líneas iniciales (`"2,4-5"`). `data-sin-barra`: oculta barra y botón Copiar.
- La sangría común se elimina y el texto se resalta solo (tokenizador propio). Sin ligaduras: `=>` se ve tal cual.

```js
const c = Kit.codigo('cod-fetch');
c.resaltar([3, 4], 'Esta línea <code>espera</code> la respuesta');  // líneas + nota flotante (HTML)
c.resaltar('2-4');      // sin nota
c.limpiar();
c.fijarTexto('nuevo código');
Kit.crearCodigo(elementoPre, { texto, lang: 'js', titulo: 'app.js', tam: 15 });
```

### Pestañas

Cualquier contenedor cuyos hijos tengan `data-pestana` se convierte en pestañas (flechas para moverse). Ideal para variantes de código:

```html
<div>
  <div data-pestana="Con async/await"><pre class="codigo" id="cod-await" data-lang="js">...</pre></div>
  <div data-pestana="Con then"><pre class="codigo" id="cod-then" data-lang="js">...</pre></div>
</div>
```

Evento `kit:pestana` (detail = índice). Para elegir por JS: `contenedor._elegir(1)`.

## 9. Controles de entrada

```html
<div class="chips" aria-label="Modo">
  <button class="chip" data-valor="fila" aria-checked="true">En fila</button>
  <button class="chip" data-valor="paralelo">En paralelo</button>
</div>
<button class="interruptor" aria-checked="false">El servidor falla</button>
<button class="interruptor" data-experto aria-checked="false">Modo experto</button>
<label class="deslizador" data-unidad="ms" data-tono="uva"><span>Latencia</span><input type="range" min="0" max="2000" value="400"><output></output></label>
```

- Chips: selección única con flechas; evento `kit:cambio` con el `data-valor`. Con `data-multiple` en el grupo son conmutadores y el evento trae un arreglo.
- Interruptor: evento `kit:cambio` con `true` o `false`. Con `data-experto` activa el modo experto global: muestra todo lo que tenga `class="solo-experto"` (también filas de tabla). Por JS: `Kit.experto(true)`.
- Deslizador: actualiza `output` y el relleno de la pista. Con `ms` y valores de 1000 o más muestra segundos con coma decimal.

## 10. Comparación y tabla

```html
<div class="comparar">
  <div class="comparar-lado" data-tono="hoja"><h3><code>textContent</code></h3><p class="nota">...</p></div>
  <div class="comparar-vs" aria-hidden="true">vs</div>
  <div class="comparar-lado" data-tono="tomate"><h3><code>innerHTML</code></h3><p class="nota">...</p></div>
</div>

<table class="tabla-kit">
  <thead><tr><th scope="col">Criterio</th><th scope="col">A</th><th scope="col">B</th></tr></thead>
  <tbody>
    <tr data-paso><td>Interpreta etiquetas</td><td><span class="no" data-ico="cerrar">No</span></td><td><span class="si" data-ico="check">Sí</span></td></tr>
  </tbody>
</table>
```

Filas sin `data-paso` entran escalonadas. `tr.resaltada` pinta la fila en maíz. Celdas: `.si`, `.no`, `.medio`. Para mostrar HTML de alguien sin riesgo usa `<iframe class="resultado" sandbox="" srcdoc="...">` (sin scripts).

## 11. Predice, quiz y reto

### Predice

```html
<div class="predice" data-correcta="3">
  <p class="pq-pregunta">¿Qué imprime la consola?</p>
  <div class="opciones cuatro">
    <button class="opcion" data-opcion="3">3</button>
    <button class="opcion" data-opcion="4">4</button>
    <button class="opcion" data-opcion="0">0</button>
    <button class="opcion" data-opcion="error">Error</button>
  </div>
  <div class="predice-revelacion">Explicación en HTML; admite <dfn data-def="nodelist">NodeList</dfn>.</div>
</div>
```

Flujo: elegir, comprometerse (bloquea), revelar (marca correcta e incorrecta, muestra la explicación dentro de la retroalimentación, chispas si acierta), intentar de nuevo. `.opciones` (una columna), `.opciones.dos`, `.opciones.cuatro` (opciones cortas). Atributos opcionales: `data-ok` y `data-mal` (títulos de la retroalimentación), `data-etiqueta`. Variante de apuesta escrita: en lugar de opciones pon `<input class="predice-apuesta">`, `data-correcta` y `data-aceptar="3|tres"`. Eventos: `kit:compromiso`, `kit:revelar` (detail `{eleccion, acierto}`).

### Quiz

```html
<div class="quiz" data-correcta="b">
  <p class="pq-pregunta">Antes del primer clic, ¿qué ocurre?</p>
  <div class="opciones dos">
    <button class="opcion" data-opcion="a" data-explica="Por qué no.">...</button>
    <button class="opcion" data-opcion="b" data-explica="Por qué sí.">...</button>
  </div>
</div>
```

Retroalimentación inmediata con la explicación de cada opción (`data-explica`, HTML). Las incorrectas se pueden reintentar.

### Reto con verificación

```html
<div class="reto" data-ejecutor="#ej-total" data-espera="Total: 19300" data-ok="Mensaje de éxito">
  <p class="reto-instruccion">Completa la línea 8 ...</p>
</div>
<div class="ejecutor" id="ej-total">...</div>
```

El botón Verificar ejecuta el código y compara la consola con `data-espera` (varias líneas separadas por `|`, cada una debe aparecer exacta). Estados visibles: Pendiente, Verificando, Logrado, Revisa. El ejecutor puede ir dentro del reto o fuera con `data-ejecutor`. Verificación propia: agrega `data-manual` al reto y en `Kit.listo` llama

```js
Kit.reto('#mi-reto', { verificar: (salida, ejecutor) => ({ ok: salida.logs.includes('6400'), mensaje: 'HTML del mensaje' }) });
```

`salida` = `{ ok, logs: [texto], errores: [texto], listo }`.

## 12. Ejecutor de código

```html
<div class="ejecutor" id="ej-dom" data-vista="si" data-api="mercado">
  <script type="text/plain" data-archivo="index.html">
<ul id="canasta"><li>Panela</li></ul>
  </script>
  <script type="text/plain" data-archivo="app.js">
const lista = document.querySelector('#canasta');
console.log(lista.children.length);
  </script>
  <script type="text/plain" data-archivo="estilos.css">li { color: green; }</script>
</div>
```

- Cada `script type="text/plain"` con `data-archivo` es una pestaña. La extensión define el lenguaje (`.js`, `.html`, `.css`). El HTML es el contenido del `body`; sus etiquetas `script` se ignoran.
- Ejecuta solo en un `iframe sandbox="allow-scripts"` con `srcdoc`. Captura `console.log/info/warn/error/table/clear`, errores con número de línea del archivo JS, promesas rechazadas y `alert` (se muestra como aviso).
- `data-vista="si"`: muestra el resultado del HTML al lado. Sin él, el iframe es invisible.
- Protección: cada `while` y `for` lleva una guarda que corta el ciclo a los 1,5 s con un `RangeError` explicado; si aun así no responde en 2 s, el iframe se destruye y se avisa. La página nunca se congela.
- Teclado: Ctrl + Enter ejecuta, Tab inserta dos espacios, Esc y luego Tab sale del editor.
- API simulada para `fetch` (offline): define la ruta y referénciala con `data-api`.

```js
Kit.apis.mercado = {
  '/api/productos': { estado: 200, demora: 600, cuerpo: [{ id: 1, nombre: 'Panela', precio: 4500 }] },
  '/api/pedidos': { estado: 500, cuerpo: { error: 'Falla interna' } },
  '/api/lento': { red: true },   // la promesa se rechaza: TypeError Failed to fetch
  '/api/productores': { metodos: { POST: { estado: 201, cuerpo: { id: 9 } } } },
};
```

Las rutas sin definir responden 404. Cada petición se anuncia en la consola. Por JS: `el._ejecutor.ejecutar({ espera: 700 })` devuelve una promesa con la salida; `Kit.ejecutor(el, { archivos: [{ nombre: 'app.js', codigo: '...' }], vista: true, api: 'mercado' })` crea uno sin marcado.

## 13. Flujo animado (componente estrella)

```html
<div id="flujo-peticion" class="crece"></div>
```

```js
Kit.flujo('#flujo-peticion', {
  titulo: 'GET /api/productos', icono: 'red',
  vista: [1100, 262],                       // viewBox; deja 70 px bajo los nodos para estados
  nodos: [
    { id: 'nav', x: 140, y: 112, icono: 'navegador', etiqueta: 'Navegador', sub: 'tu código', tono: 'cielo' },
    { id: 'api', x: 550, y: 112, icono: 'servidor', etiqueta: 'API', sub: 'ASP.NET Core', tono: 'uva' },
  ],
  conectores: [
    { desde: 'nav', hasta: 'api', curva: -34 },
    { desde: 'api', hasta: 'nav', curva: -34 },
  ],
  parametros: [
    { id: 'latencia', tipo: 'rango', etiqueta: 'Latencia', min: 200, max: 3000, paso: 100, valor: 900, unidad: 'ms', tono: 'uva' },
    { id: 'falla', tipo: 'interruptor', etiqueta: 'El servidor falla', valor: false, tono: 'tomate' },
    { id: 'modo', tipo: 'opciones', etiqueta: 'Modo', opciones: [{ valor: 'a', etiqueta: 'A' }, { valor: 'b', etiqueta: 'B' }], valor: 'a' },
  ],
  intro: 'Texto inicial antes de reproducir (HTML).',
  pasos: (v) => [
    { nodo: 'nav', duracion: 1500, texto: 'Se llama a <code>fetch</code>.', estado: { nav: { texto: 'esperando', tono: 'maiz' } },
      codigo: { id: 'cod-fetch', lineas: [3], nota: 'Nota flotante' } },
    { desde: 'nav', hasta: 'api', paquete: 'GET /api/productos', tono: 'uva', duracion: v.latencia, texto: 'La petición viaja.' },
    { desde: 'api', hasta: 'nav', paquete: v.falla ? '500' : '200 OK + JSON', tono: v.falla ? 'tomate' : 'hoja', duracion: v.latencia,
      texto: 'Llega la respuesta.', estado: { nav: null } },
  ],
  alPaso: (i, paso, flujo) => {},           // opcional
  alRecalcular: (valores, pasos, flujo) => {},
});
```

- **Nodo**: `id`, `x`, `y` (centro), `etiqueta`, `sub`, `icono` (nombre de ilustración), `tono`, `ancho` (190), `alto` (136 con icono; 80 con sub; 60 sin nada), `paso` (fragmento).
- **Conector**: `desde`, `hasta`, `curva` (desplazamiento del arco; con izquierda a derecha, negativo arquea hacia arriba; usar el mismo valor en ida y vuelta separa los dos arcos), `tono`, `etiqueta`, `dyEtiqueta`, `flecha: true` (línea continua con punta), `paso`.
- **Paso**: `desde`/`hasta` (mueve un paquete por el conector, en cualquier sentido; si no hay conector traza uno recto), `paquete` (etiqueta en monoespaciada; ancho aproximado 9,6 px por carácter), `tono`, `duracion` en ms (1500 por defecto), `texto` (narración HTML, se anuncia con `aria-live`), `nodo` o `pulso` (resalta con halo pulsante), `resaltar` (lista de ids), `estado` (`{ id: { texto, tono } }`, `{ id: 'texto' }` o `{ id: null }` para borrar; los estados se acumulan), `codigo` (objeto o arreglo `{ id, lineas, nota }` que resalta líneas de bloques de código; al cambiar de paso los demás se limpian).
- `pasos` puede ser un arreglo o una función de los valores de `parametros`. Al cambiar un parámetro el flujo se recalcula y vuelve al inicio.
- Controles: reiniciar, paso anterior, reproducir o pausar, paso siguiente (anima solo ese paso), barra arrastrable con marcas por paso, velocidad 0,5x, 1x, 2x, reloj en ms. Se pausa al salir de la diapositiva.
- API: `f.reproducir()`, `f.pausar()`, `f.reiniciar()`, `f.irPaso(i)`, `f.fijarParametro(id, v)`, `f.valores`, `f.pasos`, `el._flujo`. Evento `kit:paso`.

**Diagrama estático** (mapa conceptual, arquitectura): `Kit.diagrama(el, { vista, nodos, conectores, tituloSvg })`, misma configuración sin pasos ni controles; nodos teñidos con su tono. Con `paso` en nodos y conectores se construye por fragmentos.

## 14. Línea de tiempo con carriles

```js
Kit.lineaTiempo('#lt', {
  titulo: 'opcional', icono: 'reloj',
  carriles: [{ id: 'productos', etiqueta: '/api/productos', tono: 'uva' }, { id: 'pintar', etiqueta: 'pintar DOM', tono: 'hoja' }],
  parametros: [ /* igual que en Flujo */ ],
  barras: (v) => [{ carril: 'productos', inicio: 0, fin: 700, etiqueta: '700 ms', tono: 'uva' }],
  escala: (v) => 2500,                       // fija la escala para comparar modos con la misma regla
  resumen: (v, barras, fin) => 'Total: ' + fin + ' ms',
  narrar: (t, v, barras, fin) => 'HTML según el instante t',
  alRecalcular: (v, barras, lt) => {},
});
```

Las barras crecen cuando el cabezal pasa; mismos controles que el flujo. Ancho mínimo recomendado: 700 px. Etiquetas cortas: una barra de 300 ms en una escala de 2,5 s mide unos 50 px.

## 15. Bucle de eventos (pila, Web APIs, colas)

```js
Kit.bucleEventos('#bucle', {
  archivo: 'orden.js', duracionPaso: 1200, tamCodigo: 15,
  programas: [{
    nombre: 'Promesa o timeout',
    codigo: "console.log('A');\nsetTimeout(() => {\n  console.log('C');\n}, 0);",
    instrucciones: [
      { tipo: 'log', texto: 'A', linea: 1 },
      { tipo: 'timeout', ms: 0, linea: 2, lineaCb: 3, cuerpo: [{ tipo: 'log', texto: 'C', linea: 3 }] },
    ],
  }],
});
```

El simulador ejecuta el algoritmo real: el script entra como tarea, los temporizadores cuentan en Web APIs y al vencer pasan a la cola de tareas, las promesas cumplidas encolan microtareas, y con la pila vacía se vacían **todas** las microtareas antes de tomar **una** tarea. Cada foto resalta la línea, anima las fichas entre zonas y narra el porqué.

Instrucciones: `log` (`texto`), `llamar` (`nombre`, `cuerpo`), `timeout` (`ms`, `cuerpo`, `lineaCb`, `nombreCb`), `then` (promesa ya cumplida; `etiqueta`, `cuerpo`, `lineaCb`), `fetch` (`recurso`, `ms`, `cuerpo`, `lineaCb`). Todas aceptan `linea` y `narr` (narración propia). Varios programas generan chips para elegir. `Kit.simularBucle(instrucciones)` devuelve las fotos sin dibujar.

## 16. Árbol DOM animado

```js
const arbol = Kit.arbolDom('#arbol', { raiz: document.getElementById('app'), titulo: 'Árbol DOM' });
arbol.nota("lista.append(li);");   // texto en la leyenda
```

- Con `raiz` observa un elemento vivo con `MutationObserver`: cualquier cambio (append, remove, textContent, classList) redibuja con animación; los nodos nuevos o cambiados destellan en maíz y los que salen se desvanecen.
- Con `html: '<ul>...</ul>'` dibuja un fragmento; `arbol.dibujar(html)` lo actualiza con animación.
- Opciones: `orientacion` (`horizontal` por defecto, mejor para listas; `vertical` para árboles anchos y bajos), `maxTexto` (16 caracteres), `anchoMinimo` (640) y `altoMinimo` (300) evitan que un árbol pequeño se vea gigante.
- Elementos en cielo con `tag#id` o `tag.clase`; textos en hoja con borde punteado. Ignora textos vacíos.
- Para editar HTML a mano, limpia `script`, `iframe` y atributos `on*` antes de insertarlo (ver el ejemplo de la galería).

## 17. API de `Kit`

| Miembro | Descripción |
|---|---|
| `Kit.listo(fn)` | ejecuta `fn` cuando el marcado ya está preparado |
| `Kit.ir(n)`, `Kit.siguiente()`, `Kit.anterior()`, `Kit.actual()`, `Kit.total()`, `Kit.diapositiva(n)` | navegación (índice desde 0) |
| `Kit.alEntrar(objetivo, fn)`, `Kit.alSalir(objetivo, fn)` | objetivo: índice, selector o elemento dentro de una diapositiva |
| `Kit.glosario(obj)`, `Kit.abrirGlosario(clave)`, `Kit.abrirApoyo()`, `Kit.cerrarPaneles()` | paneles |
| `Kit.experto(v)` | activa, desactiva o alterna el modo experto |
| `Kit.anunciar(texto)` | mensaje para lector de pantalla |
| `Kit.codigo(id)`, `Kit.crearCodigo(el, op)` | bloques de código |
| `Kit.flujo`, `Kit.diagrama`, `Kit.lineaTiempo`, `Kit.bucleEventos`, `Kit.simularBucle`, `Kit.arbolDom`, `Kit.ejecutor`, `Kit.reto` | componentes |
| `Kit.preparar(raiz)` | prepara marcado agregado después del arranque |
| `Kit.tokenizar(texto, lang)`, `Kit.resaltar(texto, lang)` | resaltado de sintaxis |
| `Kit.h(tag, attrs, ...hijos)`, `Kit.s(...)` (SVG), `Kit.ico`, `Kit.ilu`, `Kit.esc` | ayudantes de DOM |
| `Kit.reducido()`, `Kit.esMovil()`, `Kit.escala`, `Kit.apis`, `Kit.GLOSARIO` | estado |

Eventos (burbujean): `kit:entrar` y `kit:salir` en cada diapositiva, `kit:cambio` (chips, interruptor), `kit:pestana`, `kit:paso` (flujo), `kit:compromiso` y `kit:revelar` (predice).

## 18. Navegación y accesibilidad

- Teclado: Espacio y flecha derecha avanzan paso o diapositiva; flecha izquierda retrocede; Inicio y Fin; I índice visual; G glosario; A apoyo; F pantalla completa; `?` ayuda; Esc cierra. Las flechas no navegan cuando el foco está en pestañas, chips o campos.
- Clic en una zona vacía de la diapositiva avanza. Deslizar horizontal en pantallas táctiles avanza o retrocede.
- Botón de ayuda (signo de interrogación) con atajos y qué hace cada control. No escribas bloques de "cómo usar la presentación".
- La última diapositiva vista se recuerda en `localStorage` (con try/catch). El hash `#n` tiene prioridad.
- Las diapositivas inactivas son `inert`. Botones reales, `aria-live` en narraciones, foco visible, contraste AA con los tonos `-osc`.
- Movimiento reducido (`prefers-reduced-motion` o `?sinmov` en la URL): sin entradas ni transiciones, paso a paso salta sin animar, escenas SMIL en pausa.
- Teléfono (menos de 700 px): diapositivas apiladas con scroll natural, barra superior con título de sección e iconos, columnas en una sola, todos los fragmentos visibles, diagramas con scroll horizontal propio. Verifica con emulación de dispositivo real.

## 19. Reglas de contenido

- Español de Colombia, voz activa, una idea por diapositiva. Contenido técnico correcto y ejecutable.
- Prohibido en el archivo: raya larga, raya corta, punto medio, puntos suspensivos unicode, comillas tipográficas, flechas unicode, emojis y caracteres de dibujo de cajas. Solo ASCII más tildes, eñe, ¿ ¡ °. Dibuja flechas en SVG. En JS escribe rangos unicode como `\u00C0`, nunca el carácter literal.
- Sin nombre del docente, asignatura, grupo, unidad ni semana como bloque. Sin entregables ni listas de lo que hay que entregar. Sin material evaluativo. Sin enlaces a otros cursos. El cierre es mapa conceptual, errores frecuentes y autoevaluación formativa (`.autoeval` con `.autoeval-item` y `data-repaso`; guarda solo en el navegador de quien la usa).
- Verificación mínima por presentación: `node --check` del JS extraído, búsqueda de caracteres prohibidos, consola limpia, capturas de todas las diapositivas a 1280x720 antes y después de interactuar, y prueba a 375x667.
