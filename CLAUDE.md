# CanMMA — Fichas de Peleador

Galería tipo carrusel con las fichas de los alumnos de la academia CanMMA. Un solo link para
las dos clases (**niños** y **adultos**), que cambia sola según el horario. Se abre en el
navegador de un **celular Android** y se **proyecta a una TV pequeña** (duplicar pantalla /
Cast). El celular es el dispositivo principal: todo cambio debe verse bien ahí primero.

## Estructura

- `index.html` — toda la app (HTML + CSS + JS en un solo archivo, sin build ni dependencias).
- `mp7jpumh-Animacion-de-lobo--fondo-de-fichas-de-peleador.mp4` — video de fondo (lobo), servido con ruta relativa.
- `memoria.md` — bitácora de decisiones y cambios del proyecto. Actualizarla al terminar cambios importantes.

Publicado desde GitHub: `https://github.com/mmah0715-wq/Fichas-de-peleador` (rama `main`).

## Cómo funciona

- Datos: un Google Sheet publicado como CSV (`SHEET_CSV`), una pestaña por clase
  (`PROFILES`): niños `gid=0`, adultos `gid=661235561`. Se descarga a través de proxies CORS
  en cascada (`PROXIES`).
- Columnas (cabeceras sin importar mayúsculas/acentos):
  - Niños: nombre, fecha de nacimiento, edad, grado, premio del mes, cursos completados,
    premios canmma 2025, foto, racha semanal, mensualidades.
  - Adultos: nombre, apodo, fecha de nacimiento, edad, grado, récord, premios canmma 2025,
    foto, mensualidad. Se limpia el label repetido dentro del dato ("Grado: X" → "X").
- Cambio de clase: modo `auto` (por hora del celular), `ninos` o `adultos`. Botón arriba a la
  derecha alterna los modos. Horario por defecto: adultos 19:10–20:30, niños el resto del día
  (clase real de niños 17:00–19:10). Editable en Ajustes. El link acepta `?clase=ninos|adultos`.
- `buildSlide(s, profile)` decide qué datos y en qué orden mostrar según la clase.
- Fotos de Google Drive se bajan vía proxy y se convierten a blob URL (cola con concurrencia 4 y caché).
- Refresco periódico: solo reconstruye las fichas si el CSV cambió y mantiene el alumno actual.
- Ajustes (refresco, segundos por alumno, modo y horario de adultos) en `localStorage` con la clave `canmma_v6`.

## Reglas para trabajar aquí

- Idioma: español en textos de la interfaz, comentarios y commits.
- Prioridad celular en **horizontal** (ej. 915×412, 800×360). Probar también vertical (412×915).
- Tamaños de letra con `clamp()` usando `min(vw, vh)` para que quepa en pantallas bajas.
- Layout por orientación (`orientation: landscape/portrait`), no por ancho.
- Respetar `env(safe-area-inset-*)` (notch en horizontal).
- No romper: pantalla completa + bloqueo horizontal, Wake Lock (pantalla siempre encendida),
  swipe, controles que se ocultan solos a los 3 s.
- Todo dato del Sheet que entre en `innerHTML` pasa por `esc()`.
- Mantener un solo archivo `index.html` salvo que se pida lo contrario.

## Probar

Abrir `index.html` en el navegador. Para simular el celular: DevTools → modo dispositivo, o
Edge headless:

```sh
msedge --headless=new --window-size=915,412 --screenshot=land.png index.html
```

Wake Lock y pantalla completa requieren HTTPS (GitHub Pages) y un toque del usuario.
