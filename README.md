# Intervalos 10K

PWA de una sola página para correr las sesiones del plan 10K con temporizador, voz en español y ritmo GPS en vivo.

## Archivos

| Archivo | Para qué |
|---|---|
| `index.html` | Toda la app (HTML + CSS + JS) |
| `manifest.json` | Hace que se pueda instalar en la pantalla de inicio |
| `sw.js` | Service worker: funciona sin conexión |
| `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` | Iconos |

## Ponerla en el móvil (GitHub Pages)

El GPS solo funciona por HTTPS, así que hay que publicarla:

1. Crea un repositorio en GitHub (por ejemplo `intervalos-10k`) y sube estos archivos a la raíz.
2. En el repositorio: **Settings → Pages → Source: Deploy from a branch → `main` / `(root)`** → Save.
3. En un minuto estará en `https://TU-USUARIO.github.io/intervalos-10k/`.
4. Ábrela en el móvil:
   - **Android (Chrome):** menú ⋮ → *Instalar aplicación*.
   - **iPhone (Safari):** Compartir → *Añadir a pantalla de inicio*.
5. La primera vez acepta el permiso de ubicación.

Si cambias `index.html`, sube también `sw.js` con el número de versión aumentado (`intervalos10k-v2`…) para que el móvil coja la versión nueva (puede hacer falta abrir la app dos veces).

## Probar en el ordenador

```bash
python -m http.server 8765
```

Abre `http://localhost:8765/?sim=1&x=20`:
- `sim=1` simula el GPS (con botones para ir más rápido o más lento),
- `x=20` acelera el tiempo ×20.

## Cómo funciona

- **Ritmo del bloque** = distancia desde que empezó el bloque / tiempo del bloque (se reinicia en cada bloque).
- **Ritmo instantáneo** = distancia de los últimos ~20 s.
- **Color de pantalla:** verde = en zona, rojo = vas demasiado rápido, azul = vas demasiado lento, naranja = pausa/rectas, gris = sin objetivo (recuperación, test).
- **Aviso de voz** si estás fuera de zona más de 15 s seguidos (se repite cada 30 s). En los rodajes suaves solo avisa si vas más rápido del tope (6:50 por defecto) y canta cada km.
- **GPS:** descarta puntos con precisión peor de 25 m y saltos imposibles (> 9 m/s).
- **Saltar / Terminar** piden dos toques para no darle sin querer.
- Si la app se cierra a mitad de sesión, lo hecho hasta entonces aparece en el Historial como «incompleta».
- **Modo cinta:** sin GPS; los bloques por distancia pasan a tiempo usando el ritmo objetivo y el test se cierra a mano.

Todo (ajustes e historial) se guarda en `localStorage` del móvil.
