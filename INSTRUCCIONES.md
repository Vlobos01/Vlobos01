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
│   ├── banner.svg               ← banner con el logo embebido en base64
│   ├── logo-360.png             ← logo optimizado que quedó embebido
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

⚠️ **Revisá dos cosas:**

1. **LinkedIn**: quedó como `https://www.linkedin.com/in/agustin-verga-lobos` (lo deduje, no lo confirmé). Si tu URL es otra, cambiala en el badge de LinkedIn del README.
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

## 5. Cambiar el logo del banner

El logo no es un link: está **embebido en base64** dentro de `banner.svg` (porque GitHub bloquea recursos externos dentro de un SVG). Para reemplazarlo:

**Opción A (fácil):** pasame el nuevo `logo.png` y lo vuelvo a embeber optimizado.

**Opción B:** reemplazá el archivo `logo.png` y corré esto en PowerShell desde la carpeta del proyecto:

```powershell
Add-Type -AssemblyName System.Drawing
$src = [System.Drawing.Bitmap]::FromFile(".\logo.png")
$dst = New-Object System.Drawing.Bitmap 360,360
$g = [System.Drawing.Graphics]::FromImage($dst)
$g.Clear([System.Drawing.Color]::Black)
$g.InterpolationMode = 'HighQualityBicubic'
$g.DrawImage($src, 0, 0, 360, 360)
$g.Dispose(); $src.Dispose()
$dst.Save(".\assets\logo-360.png", [System.Drawing.Imaging.ImageFormat]::Png); $dst.Dispose()

$b64 = [Convert]::ToBase64String([IO.File]::ReadAllBytes(".\assets\logo-360.png"))
$svg = [IO.File]::ReadAllText(".\assets\banner.svg")
$svg = [regex]::Replace($svg, 'base64,[A-Za-z0-9+/=]+', "base64,$b64")
[IO.File]::WriteAllText(".\assets\banner.svg", $svg, (New-Object System.Text.UTF8Encoding($false)))
```

> Tip: el logo actual se recortó automáticamente al contenido (el PNG original tiene mucho margen negro) para que el lobo se vea grande dentro de la tarjeta.

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
| El logo no se ve | Se rompió el base64: volvé a embeberlo (sección 5). |
| Stats en 404 o en blanco | `github-readme-stats` es gratuito y a veces se cae o tarda. Recargá más tarde. |
| La serpiente da 404 | El workflow no corrió o no tiene permisos de escritura (sección 7). |
| Se ve cortado en el celular | Las tablas anchas scrollean; el banner y el divider usan `width="100%"`. |

## 9. Ideas para después

- **Efecto máquina de escribir** con `readme-typing-svg`.
- **Trofeos**: `github-profile-trophy`.
- **Ahora escuchando en Spotify**: `spotify-github-profile`.
- Un segundo SVG con un **proyecto destacado** hecho a medida (mockup de la app).
- Cuando tengas más repos de React, sumarlos a "Proyectos destacados".
