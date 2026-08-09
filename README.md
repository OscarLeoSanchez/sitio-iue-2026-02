# Sitio de clases IUE 2026-02

Material de las asignaturas **Programación Móvil (IF2004)** y **Programación Web (IF2003)** de la
Institución Universitaria de Envigado, semestre 2026-02.

Sitio publicado: <https://oscarleosanchez.github.io/sitio-iue-2026-02/>

## Cómo se construye

El sitio se genera con [Quarto](https://quarto.org). Los fuentes son los archivos `.qmd`; la
carpeta `docs/` contiene el HTML ya renderizado, que es lo que sirve GitHub Pages.

```powershell
quarto render          # regenera docs/
quarto preview         # servidor local con recarga automática
```

## Estructura

```
_quarto.yml              Configuración del sitio: navegación, tema, formato
estilos/estilo.scss      Tema visual compartido por todas las guías
index.qmd                Portada
movil/index.qmd          Presentación y calendario de Programación Móvil
movil/semana-NN/         Una carpeta por semana: la guía y sus imágenes
web/index.qmd            Presentación y calendario de Programación Web
web/semana-NN/
docs/                    Salida renderizada. GitHub Pages publica desde aquí
```

Cada semana vive en su propia carpeta con las imágenes al lado de la guía que las usa.

## Publicar una semana nueva

```powershell
quarto render
git add -A
git commit -m "Semana NN de Movil"
git push
```

GitHub Pages actualiza el sitio en menos de un minuto.

## Configuración de GitHub Pages

Settings → Pages → Source: **Deploy from a branch** → rama `main`, carpeta `/docs`.

El archivo `docs/.nojekyll` es necesario: sin él GitHub procesa el sitio con Jekyll y descarta
las carpetas que empiezan con guion bajo, que es donde Quarto pone los estilos y scripts.
