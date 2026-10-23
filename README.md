# 3D Wireframe Cube Renderer in JavaScript

How does a 3D scene become a 2D image without a graphics library? This project builds a small rendering pipeline from scratch in JavaScript and uses it to display an interactive wireframe cube on an HTML canvas. It makes the geometry behind rotation, near-plane clipping, perspective, and screen coordinates visible in one runnable example.

## Project highlights

- Renders a cube defined by **8 vertices and 12 unique edges** on a **600 × 600** canvas.
- Implements rotations around all three axes for both the cube and the view, with keyboard controls for each.
- Clips edges at a near plane of **0.01** before perspective division, so edges crossing the plane can still be drawn.
- Targets **60 updates per second** using a timer; actual frame rate depends on the browser and device.

## Setup and installation

**Requirements:** A desktop browser with JavaScript and HTML Canvas support. There are no packages to install, build steps, API keys, or external data files.

1. Clone the repository:

   ```sh
   git clone https://github.com/NeefMan/3d-graphics-simulation.git
   cd 3d-graphics-simulation
   ```

2. Open `index.html` in your browser. You can also open the file directly from a downloaded copy of the repository.
3. Click the page if needed, then use the keys below to interact with the cube.

| Action | Keys |
| --- | --- |
| Move the cube farther / closer | `W` / `S` |
| Rotate the cube around the Y axis | `T` / `R` |
| Rotate the cube around the X axis | `G` / `F` |
| Rotate the cube around the Z axis | `B` / `V` |
| Rotate the view around the Y axis | `U` / `Y` |
| Rotate the view around the X axis | `J` / `H` |
| Rotate the view around the Z axis | `M` / `N` |

## How it works

Each update follows a 3D-to-2D rendering pipeline:

1. **Build the geometry.** `getCubePoints(2)` defines eight cube vertices; `faces` lists the vertex connections used to draw its outline.
2. **Transform the endpoints.** Each edge endpoint is rotated around the cube's Y, X, and Z axes, shifted along Z by `dz`, then rotated by the view angles. The rotation functions apply the corresponding trigonometric coordinate transforms directly.
3. **Clip against the near plane.** `clipLine` discards edges entirely behind `z = 0.01` and interpolates a new endpoint for an edge that crosses it. Clipping happens before division by Z, avoiding projection of points at or behind the viewer.
4. **Project and map to the canvas.** `point3DToGraph` divides X and Y by Z, making farther points appear smaller. `pointGraphToScreen` converts those projected coordinates to canvas pixels and flips the Y direction to match screen coordinates.
5. **Draw and repeat.** The renderer clears the canvas, draws the visible edges as green lines, and schedules the next update. Pressed keys change the position and angles on each update.

The code keeps these stages as separate functions in `index.js`, so each transformation can be read in the order it is applied. This is a wireframe demonstration: it does not fill surfaces, shade them, or use WebGL.

## Team member

- Ethan Lahr ([@NeefMan](https://github.com/NeefMan))

## Files

- `index.html` creates the canvas and loads the script.
- `index.js` contains the geometry, transformations, clipping, projection, controls, and drawing loop.

For a longer explanation of the graphics concepts, see [The Elegance of 3D Graphics](https://dev.to/neefman/the-elegance-of-3d-graphics-1kc4).
