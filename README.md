# Subway Jack — Hito 1 (Renderizado estático)

Videojuego retro 2D estilo *Subway Surfers* con **vista cenital** y **3 carriles verticales**, implementado en el lenguaje **Jack** del ecosistema **Nand2Tetris** (plataforma Hack).

> **Este hito (1) es SOLO renderizado estático**: dibuja el tablero y los objetos gráficos iniciales una única vez. No hay game loop, movimiento ni colisiones. La arquitectura ya deja preparados los campos y getters necesarios para añadir movimiento y colisiones AABB en el Hito 2.

---

## Descripción del juego

- Pantalla monocromática de **512×256 px** (x: 0..511, y: 0..255).
- **Zona de juego:** x 128..383 (256 px de ancho), y 0..255 (alto completo).
- **3 carriles verticales** dentro de la zona de juego:
  | Carril | Posición | Rango x | Centro x |
  |-------|----------|---------|----------|
  | 0 | Izquierdo | 131..208 | 170 |
  | 1 | Central   | 213..298 | 256 |
  | 2 | Derecho   | 303..380 | 341 |
  - Separadores punteados en x = 210 y x = 300.
- **Paneles de HUD** laterales con texto estático (`Output`):
  - Izquierdo (x 0..127): título **SUBWAY JACK**, `PUNTOS: 0`, `VIDAS: 3`, `NIVEL: 1`.
  - Derecho (x 384..511): `CONTROLES`, `A/D: mover`, `Q: salir` y la etiqueta del hito.
- El **jugador** aparece en el carril central, abajo. Los **obstáculos** se muestran en posiciones fijas de ejemplo (en el Hito 2 descenderán desde arriba).

---

## Estructura de clases (un archivo por clase)

| Archivo | Responsabilidad |
|---------|-----------------|
| `Main.jack` | Punto de entrada. Crea `Game`, llama a `run()` y `dispose()`. |
| `Game.jack` | Orquestador. Dueño de `Board`, `Player` y un arreglo de `Obstacle` (4 de muestra). Dibuja todo una vez y espera una tecla. |
| `Board.jack` | Dibuja el escenario: marco, bordes gruesos, separadores punteados, postes decorativos y los paneles de HUD con texto. |
| `Player.jack` | Sprite compuesto (cabeza, gorra, cuerpo, mochila, brazos y piernas) con `draw()`/`erase()` desacoplados. |
| `Obstacle.jack` | Obstáculo con 3 tipos visuales: barrera (0), tren/bloque (1), cono (2). |

### Preparación para el Hito 2
`Player` y `Obstacle` exponen `getX()`, `getY()`, `getWidth()`, `getHeight()` y `getLane()`, y guardan su **caja envolvente** (x, y, width, height). Esto permite mover el jugador entre carriles, desplazar obstáculos hacia abajo y detectar colisiones por **cajas envolventes (AABB)** sin rediseñar las clases. El método `erase()` ya está separado de `draw()` para soportar animación sin artefactos.

---

## Compilación y ejecución

### 1. Compilar con JackCompiler
Coloca los 5 archivos `.jack` en una carpeta (p. ej. `SubwayJack/`) y ejecuta el compilador sobre la **carpeta** (no sobre archivos sueltos):

\`\`\`bash
# Desde la raíz de las herramientas de Nand2Tetris:
./tools/JackCompiler.sh ruta/a/SubwayJack
# En Windows:
tools\JackCompiler.bat ruta\a\SubwayJack
\`\`\`

Esto genera un archivo `.vm` por cada clase dentro de la misma carpeta.

### 2. Ejecutar en el VM Emulator
1. Abre el **VM Emulator** (`tools/VMEmulator.sh` / `VMEmulator.bat`).
2. Menú **File → Load Program** y selecciona la **carpeta** `SubwayJack` (carga todos los `.vm`, incluida la biblioteca estándar del OS).
3. Activa **No animation** (menú *Animate → No animation*) para un dibujado rápido y sin parpadeo.
4. Pulsa **Run** (▶). Verás el tablero, el jugador y los obstáculos de muestra.
5. Pulsa cualquier tecla en el emulador para finalizar (la escena espera con `Keyboard.readChar()`).

---

## Asignación de teclas prevista (Hito 2)

| Tecla | Acción |
|-------|--------|
| `A` | Mover al jugador al carril de la izquierda |
| `D` | Mover al jugador al carril de la derecha |
| `Q` | Salir del juego |

> En el Hito 1 solo se usa `Keyboard.readChar()` para esperar una tecla y terminar; la lógica de control se añadirá en el Hito 2.

---

## Créditos de autoría

- **Autores:** _(Mateo Montoya Ospina, Sebastian Andres Ibarra Prada, Miguel Angel Colorado Castaño)_ 
- **Materia / Curso:** _(Organizacion de computadores)_
- **Fecha:** _(11/5/2026)_
- **Hito:** 1 — Renderizado estático (Tercer Parcial)
