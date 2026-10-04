# Memoria del proyecto

Bitácora de decisiones, cambios y pendientes. Lo más reciente arriba.

## 2026-10-04 — "Load failed": proxies caídos

Los 4 proxies CORS (corsproxy.io, allorigins, codetabs, thingproxy) fallaban y no cargaba
nada. Google Sheets publicado sí permite CORS, así que ahora se pide directo y los proxies
quedan de respaldo. Fotos (hoy en i.ibb.co) se cargan con `<img>` directo, sin proxy.

## 2026-10-04 — Niños y adultos en un solo link

**Decisión:** antes había dos páginas (Fichas-de-peleador para niños y Fichas-de-adultos). Se
unificaron en esta: un solo link que cambia de clase automáticamente.

- Horario real: niños 17:00–19:10, adultos 19:10–20:30.
- Regla AUTO: adultos entre 19:10 y 20:30; el resto del día, niños (así antes de clase ya está lista).
- Botón para forzar: AUTO → NIÑOS → ADULTOS. Al cambiar sale pantalla "CLASE DE …".
- Ambas clases usan el mismo Sheet, distinta pestaña (niños gid 0, adultos gid 661235561).
- Adultos muestra apodo (en rojo) y récord; niños muestra racha, premio del mes y cursos.
- El repo Fichas-de-adultos queda obsoleto.

## 2026-10-04 — Enfoque celular → TV

**Decisión:** la galería se proyecta en una TV pequeña desde un celular Android. El celular
pasa a ser el dispositivo principal (antes se pensaba en computadora/tablet).

**Bugs corregidos:**
- Cada refresco del Sheet regresaba el carrusel al alumno 1 (con 30 s de refresco y 8 s por
  alumno nunca pasaba del 4.º). Ahora solo reconstruye si el CSV cambió y conserva el alumno actual.
- Cambiar el "Refresco" en Ajustes no aplicaba hasta recargar. Ahora se reprograma al guardar.
- Un fallo de red en un refresco tapaba las fichas con la pantalla de error. Ahora solo se
  muestra error si todavía no hay fichas cargadas.
- Datos del Sheet se insertaban sin escapar en el HTML. Ahora pasan por `esc()`.
- Se evitan descargas simultáneas del CSV si un refresco tarda.

**Adaptación a celular:**
- Botón "⛶ PANTALLA COMPLETA": oculta la barra del navegador y bloquea en horizontal.
- Wake Lock: la pantalla del celular no se apaga mientras se proyecta.
- Deslizar con el dedo para cambiar de alumno.
- Botones (ajustes, flechas) se ocultan solos a los 3 s sin tocar.
- Layout por orientación: horizontal = datos a la izquierda y foto a la derecha; vertical = foto arriba.
- Letras escalan con el alto de pantalla para que todo quepa en celulares en horizontal.
- Soporte de notch (`safe-area`) y sin zoom por doble toque.

## Antes (historial de commits)

- Video del lobo: se probó Google Drive y GitHub Releases; quedó servido desde el repo con
  ruta relativa, `object-fit: contain`, más arriba y con más brillo para ver la cara completa.
- Fotos de alumnos centradas; `index.html` movido a la raíz.

## Pendientes / ideas

- Depende de proxies CORS gratuitos; si fallan todos, no cargan datos. Alternativa: Apps Script
  propio que devuelva el CSV con CORS.
- Posible PWA (manifest con `display: fullscreen` y `orientation: landscape`) para abrir desde
  el inicio del celular directo en pantalla completa.
