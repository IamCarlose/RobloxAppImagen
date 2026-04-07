# 🎮 RobloxAppImagen

> **Convierte imágenes en estructuras 3D de partes dentro de Roblox Studio.**
> 
> Proporciona una imagen → el sistema genera un script Luau con una cuadrícula de `Part`s coloreadas que reproducen la imagen píxel a píxel en tu juego.

---

## 📁 Estructura del Proyecto

```
RobloxAppImagen/
│
├── README.md                        ← Esta guía
├── Prompt.md                        ← Prompt original del proyecto
│
├── image_processor/                 ← 🐍 Herramienta Python (procesamiento externo)
│   ├── requirements.txt             ← Dependencias: Pillow, numpy, colormath
│   ├── image_to_roblox.py          ← CLI principal ⭐
│   ├── color_mapper.py             ← Mapeo RGB → Color3 / BrickColor
│   └── image_utils.py              ← Carga, resize y extracción de píxeles
│
├── roblox_scripts/                  ← 📜 Scripts Luau para Roblox Studio
│   ├── ImagePixelPlacer.lua        ← Script principal de colocación ⭐
│   ├── PixelDataLoader.lua         ← Módulo utilitario reutilizable
│   └── DecalApplier.lua            ← Alternativa: aplicar imagen como Decal
│
├── roblox_plugin/                   ← 🔌 Plugin GUI para Roblox Studio
│   ├── init.lua                    ← Entry point del plugin ⭐
│   └── toolbar.lua                 ← Toolbar y botones del plugin
│
└── output/                          ← 📦 Archivos de ejemplo y salida generada
    ├── example_output.json          ← Ejemplo de JSON generado por Python
    └── example_pixel_placer.lua    ← Script Luau de ejemplo listo para usar ⭐
```

---

## 🚀 Métodos Disponibles

| Método | Descripción | Dificultad | Calidad |
|--------|-------------|------------|---------|
| **A — Python + Command Bar** | Procesa en Python, ejecuta en Studio | ⭐⭐ Fácil | ⭐⭐⭐⭐⭐ Máxima |
| **B — Plugin de Studio** | GUI integrada en Roblox Studio | ⭐⭐⭐ Medio | ⭐⭐⭐⭐ Alta |
| **C — Decal** | Imagen como textura en una Part | ⭐ Muy fácil | ⭐⭐⭐ Media |

---

## ⚡ Inicio Rápido — Prueba en 30 segundos

Si solo quieres probar el resultado **sin instalar nada**:

1. Abre **Roblox Studio** → crear nuevo proyecto vacío
2. Ve a **View → Command Bar**
3. Abre el archivo `output/example_pixel_placer.lua`
4. **Copia todo el contenido** y pégalo en el Command Bar
5. Presiona **Enter** → aparecerá un Model `example_Image` en el Workspace 🎉

---

## 📋 Método A — Python + Command Bar ⭐ (Recomendado)

### Paso 1: Instalar Dependencias

```powershell
cd C:\Users\PC\RobloxAppImagen\image_processor
pip install -r requirements.txt
```

> **Requisito:** Python 3.10 o superior. Descarga en [python.org](https://www.python.org/downloads/)

### Paso 2: Procesar tu Imagen

```powershell
# Formato básico (genera imagen de 32x32 píxeles):
python image_to_roblox.py --image "C:\ruta\a\tu\imagen.png" --size 32

# Con más opciones:
python image_to_roblox.py --image foto.png --size 64 --mode color3 --output-lua output\mi_script.lua

# Ver todas las opciones:
python image_to_roblox.py --help
```

#### Opciones del CLI

| Opción | Descripción | Default |
|--------|-------------|---------|
| `--image` / `-i` | Ruta a la imagen (PNG, JPG, BMP...) | *requerido* |
| `--size` / `-s` | Tamaño máximo en píxeles (lado más largo) | `32` |
| `--mode` / `-m` | `color3` (colores exactos) o `brickcolor` (paleta Roblox) | `color3` |
| `--output-json` / `-j` | Ruta del JSON generado | `<nombre>_output.json` |
| `--output-lua` / `-l` | Ruta del script Luau generado | `<nombre>_placer.lua` |
| `--no-lua` | No generar el script Lua | — |
| `--no-json` | No generar el archivo JSON | — |

#### Tamaños Recomendados

| `--size` | Partes generadas (aprox.) | Tiempo en Studio |
|----------|--------------------------|-----------------|
| 16 | ~256 | < 1 segundo |
| 32 | ~1,024 | ~2 segundos |
| 64 | ~4,096 | ~10 segundos |
| 128 | ~16,384 | ~40 segundos |

> ⚠️ **Nota:** Roblox Studio puede ralentizarse con más de 10,000 partes. Usa `--size 64` como máximo recomendado.

### Paso 3: Ejecutar en Roblox Studio

1. Abre **Roblox Studio** → nuevo proyecto vacío
2. Ve a **View → Command Bar** (o `Alt + F9`)
3. Abre el archivo `<nombre>_placer.lua` generado
4. **Copia todo el contenido** y pégalo en el Command Bar
5. Presiona **Enter**
6. El Model con las partes aparece en el **Workspace** ✅

---

## 🔌 Método B — Plugin de Roblox Studio

El plugin ofrece una **interfaz gráfica** dentro de Studio con botones en la toolbar.

### Instalación del Plugin

1. Abre Roblox Studio
2. Ve a **Plugins → Plugins Folder** (abre la carpeta en el explorador)
3. Crea una subcarpeta llamada `RobloxAppImagen`
4. Copia los archivos de `roblox_plugin/` dentro de esa carpeta:
   - `init.lua`
   - `toolbar.lua`
5. **Reinicia** Roblox Studio
6. Aparecerá la barra **"🎮 RobloxAppImagen"** en la toolbar superior

### Uso del Plugin

| Botón | Función |
|-------|---------|
| **Ejemplo 4x4** | Genera una imagen de prueba de 4x4 partes coloridas |
| **Limpiar** | Elimina todos los modelos generados del Workspace |
| **Test Decal** | Instrucciones para aplicar un Decal (→ Método C) |

---

## 🖼️ Método C — Decal (Más Simple)

Aplica la imagen como textura sobre una Part. No crea partes individuales.

### Pasos

1. Sube tu imagen en [roblox.com](https://www.roblox.com) → **Create → Asset Manager → Images**
2. Copia el **Asset ID** numérico de la imagen (ej. `12345678`)
3. Abre `roblox_scripts/DecalApplier.lua` y reemplaza:
   ```lua
   local ASSET_ID = 12345678  -- ← tu Asset ID aquí
   ```
4. Copia el script y ejecútalo en el **Command Bar** de Studio

> ⚠️ Las imágenes subidas a Roblox pasan por revisión y pueden tardar unos minutos en estar disponibles.

---

## 🎨 Modos de Color

### `color3` (Default, Recomendado)
- Usa `Color3.fromRGB(r, g, b)` → colores exactos, sin limitaciones
- Las Parts adoptan el color RGB exacto de cada píxel
- **Máxima fidelidad visual**

### `brickcolor`
- Mapea cada píxel al `BrickColor` más cercano de la paleta oficial de Roblox
- Paleta de ~40 colores disponibles (limitada)
- Útil si quieres el estilo visual clásico de LEGO/Roblox
- Activa con: `--mode brickcolor`

---

## 🛠️ Descripción de los Archivos

### `image_processor/image_to_roblox.py` — CLI Principal
Coordina todo el proceso: carga imagen → resize → extrae píxeles → mapea colores → genera JSON + Lua.

### `image_processor/image_utils.py` — Utilidades de Imagen
- `load_image(path)`: Carga la imagen como RGBA
- `resize_image(img, max_size)`: Redimensiona manteniendo proporción
- `extract_pixels(img)`: Extrae matriz 2D de tuplas (R,G,B); los píxeles transparentes se convierten a blanco

### `image_processor/color_mapper.py` — Mapeo de Colores
- `rgb_to_color3(r,g,b)`: Convierte a valores Roblox (0.0–1.0)
- `rgb_to_brick_color(r,g,b)`: Encuentra el BrickColor más cercano (distancia euclidiana)
- `build_color_data(pixels, mode)`: Construye la estructura completa de datos de color

### `roblox_scripts/ImagePixelPlacer.lua` — Script Principal
Crea el Model con todas las Parts coloreadas. Edita la sección `CONFIG` para personalizar: nombre del modelo, tamaño de partes, centrado automático.

### `roblox_scripts/PixelDataLoader.lua` — Módulo Reutilizable
Módulo Lua que puede cargarse con `require()`. Funciones: `parseInlineData()`, `buildModel()`, `centerModel()`. Incluye yield automático para no bloquear el editor en imágenes grandes.

### `roblox_scripts/DecalApplier.lua` — Aplicador de Decal
Crea una Part canvas con un Decal y un marco decorativo. Solo requiere un Asset ID de Roblox.

### `roblox_plugin/init.lua` — Plugin Principal
Entry point del plugin de Studio. Crea la toolbar, conecta los eventos de los botones, y contiene la lógica de generación de modelos integrada.

### `output/example_pixel_placer.lua` — Ejemplo Listo
Script Luau autogenerado listo para pegar en el Command Bar. Perfecto para probar sin necesidad de correr Python.

---

## ❓ Preguntas Frecuentes

**¿Cuántas partes puedo generar sin que Studio se congele?**  
Recomendamos máximo `--size 64` (~4,000 partes). Para solo ver la imagen, usa el Método C (Decal).

**¿El script generado es compatible con Roblox LocalScript?**  
No directamente. Los scripts generados están diseñados para el **Command Bar** de Studio (ejecución en el editor, no en tiempo de ejecución del juego). Para juegos en vivo, adapta `ImagePixelPlacer.lua` a un `Script` de servidor.

**¿Puedo usar imágenes con fondo transparente?**  
Sí. Los píxeles con alpha < 128 se convierten automáticamente a blanco (255,255,255).

**¿Qué formatos de imagen acepta Python?**  
Todos los soportados por Pillow: PNG, JPG, BMP, GIF (primer frame), TIFF, WebP, etc.

**¿El plugin funciona con Roblox Studio más antiguo?**  
El plugin usa APIs estándar de Studio que son compatibles con versiones recientes (2023+).

---

## 📦 Dependencias

### Python (image_processor)
| Librería | Versión | Uso |
|----------|---------|-----|
| [Pillow](https://python-pillow.org/) | ≥10.0 | Carga y procesamiento de imágenes |
| [numpy](https://numpy.org/) | ≥1.24 | Manipulación eficiente de arrays de píxeles |
| [colormath](https://python-colormath.readthedocs.io/) | ≥3.0 | Conversión de espacios de color |

### Roblox Studio
- Roblox Studio cualquier versión reciente (2023+)
- API de Luau estándar (sin dependencias externas)

---

## 📝 Licencia

Este proyecto es de uso libre para aprendizaje y proyectos personales de Roblox.  
Creado con 🎮 por **RobloxAppImagen** — 2026

---

*Generado con RobloxAppImagen v1.0*
