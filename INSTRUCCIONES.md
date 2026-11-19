# Guía del perfil de GitHub — Vlobos01

## 1. Lo primero: qué se puede y qué no

GitHub **no** permite CSS ni JavaScript en los README. Filtra el HTML y deja pasar solo etiquetas seguras. Por eso el aspecto "neón" se consigue así:

| Recurso | ¿Se puede? | Cómo |
|---|---|---|
| CSS / `<style>` externo | ❌ | GitHub lo elimina |
| `<script>` | ❌ | Eliminado |
| HTML de layout (`div`, `p`, `img`, `a`, `table`, `details`) | ✅ | Con atributos `align`, `width`, `height` |
| **SVG propio** (gradientes, animaciones, logo embebido) | ✅ | Se sube al repo y se embebe como imagen |
| Badges (shields.io, skillicons) | ✅ | Imágenes desde servicios externos |
| Tarjetas de stats | ✅ | github-readme-stats / streak-stats |

> Regla de oro: **si querés diseño propio, hacelo en un `.svg`**. Los SVG sí aceptan `<style>` interno, gradientes y animaciones CSS/SMIL.

## 2. Archivos del proyecto

```
.
├── README.md                    ← tu perfil (esto es lo que se ve)
├── logo.png                     ← logo original (azul #006FFD)
├── assets/
│   ├── banner.svg               ← banner con nombre + terminal .NET
│   └── divider.svg              ← línea degradada decorativa
├── .github/workflows/snake.yml  ← genera la serpiente de contribuciones
├── preview.html                 ← solo local, para ver el banner en el navegador
└── INSTRUCCIONES.md
```

## 3. Datos ya cargados en el README

| Dato | Valor usado |
|---|---|
| Usuario GitHub | `Vlobos01` |
| Nombre | Agustín Verga Lobos |
| Rol | Analista en Sistemas |
| Ubicación | Buenos Aires, Argentina |
| Email | agusvergalobos2016@gmail.com |
| Proyectos | `Cobranza`, `CuentasContables`, `Parcial2Tp4` |

⚠️ **Revisá esto:**

1. **LinkedIn**: ya apunta a `https://www.linkedin.com/in/agustin-verga-lobos-951a26267/`. Si algún día lo cambiás, actualizá la `href` del badge.
2. **Email público**: el link `mailto:` y el texto del README quedan visibles para cualquiera. Si no querés eso, borrá el badge de Email y el bloque de Contacto.

## 4. Paleta (sacada de tu logo)

| Rol | Hex | Dónde se usa |
|---|---|---|
| Azul logo | `#006FFD` | anillo, glow, badges, iconos |
| Azul claro | `#4DA3FF` | nombre en degradé, títulos de tarjetas |
| Celeste | `#00C2FF` | acentos, racha (`fire`) |
| Blanco | `#EAF2FF` | "AGUSTÍN" |
| Fondo | `#03060D` / `#0A1220` / `#05070D` | banner y tarjetas |

Para reemplazar colores usá buscar/reemplazar en `assets/banner.svg`, `assets/divider.svg` y `README.md`. En las URLs de stats los colores van **sin `#`**.

Si cambiás el azul base, tocá estas claves del README:

- `bg_color=05070D` → fondo de la tarjeta
- `title_color=4DA3FF` → títulos
- `icon_color=006FFD` → iconos
- `text_color=C9D6E3` → texto
- streak: `ring=006FFD&fire=00C2FF`
- visitas: `color=006FFD&labelColor=05070D`

## 5. Sobre el logo

El logo **ya no está en el banner**: GitHub muestra tu avatar arriba a la izquierda del perfil, así que repetirlo era redundante. Los colores del banner sí salen de él (`#006FFD`).

En su lugar el banner tiene una terminal con `whoami` y `flutter run`.

Si algún día querés volver a poner el logo adentro del SVG, hay que embeberlo en base64 (GitHub bloquea recursos externos dentro de un SVG). Pasame `logo.png` y lo vuelvo a embeber recortado y optimizado.

## 6. Subirlo a GitHub

El repo `Vlobos01/Vlobos01` ya existe (vacío) y debe ser **público**. Desde esta carpeta:

```powershell
git init
git add .
git commit -m "feat: perfil con banner, logo, stats y proyectos"
git branch -M main
git remote add origin https://github.com/Vlobos01/Vlobos01.git
git push -u origin main
```

Si ya tenías un remoto configurado, usá `git remote set-url origin https://github.com/Vlobos01/Vlobos01.git`.

## 7. Activar la serpiente 🐍

1. Repo → **Settings** → **Actions** → **General**.
2. En *Workflow permissions* elegí **Read and write permissions** → **Save**.
3. Pestaña **Actions** → **Generate Snake** → **Run workflow**.
4. Esperá 1–2 minutos: se crea la rama `output` con `snake-dark.svg`.
5. Recargá tu perfil (puede tardar por caché).

## 8. Problemas comunes

| Síntoma | Causa / solución |
|---|---|
| El banner no aparece | Revisá rama (`main` vs `master`), repo público y ruta exacta. Usá URLs absolutas `raw.githubusercontent.com`, nunca rutas relativas. |
| Cambié el SVG y sigo viendo el viejo | Caché de imágenes de GitHub (camo). Agregá `?v=2` al final de la URL del banner o esperá unos minutos. |
| Stats en 404 o en blanco | `github-readme-stats` es gratuito y a veces se cae o tarda. Recargá más tarde. |
| La serpiente da 404 | El workflow no corrió o no tiene permisos de escritura (sección 7). |
| Se ve cortado en el celular | Las tablas anchas scrollean; el banner y el divider usan `width="100%"`. |

## 9. Ideas para después

- **Efecto máquina de escribir** con `readme-typing-svg`.
- **Trofeos**: `github-profile-trophy`.
- **Ahora escuchando en Spotify**: `spotify-github-profile`.
- Un segundo SVG con un **proyecto destacado** hecho a medida (mockup de la app).
- Cuando tengas más repos de React, sumarlos a "Proyectos destacados".
