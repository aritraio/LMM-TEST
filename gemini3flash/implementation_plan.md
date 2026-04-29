# Implementation Plan - Interactive Water Ripple Simulation

Create a high-performance, interactive 2D water ripple simulation using the HTML5 Canvas API.

## User Review Required

> [!IMPORTANT]
> The simulation will target a ~500x500 grid. To ensure 60 FPS on various devices while using Canvas 2D, we will use `ImageData` for direct pixel manipulation and `Float32Array` for the physics buffers.

## Proposed Changes

### [NEW] [water_ripple.html](file:///Users/aritra/Dev/Projects/LMM-TEST/3.1pro/water_ripple.html)

A single self-contained file containing the structure, styling, and logic.

#### Physics Model
- **Algorithm**: Implement the Hugo Elias ripple algorithm (finite difference approximation of the wave equation).
- **Buffers**: Use two `Float32Array` buffers to represent the height map at $t$ and $t-1$.
- **Damping**: Apply a damping factor (e.g., 0.98) each frame to dissipate energy.

#### Rendering Logic
- **Direct Pixel Access**: Map height values to pixel intensities (shading) using `ctx.putImageData`.
- **Scaling**: If the window is larger than the grid, the canvas will be scaled to maintain performance while looking "smooth" (using `image-rendering: pixelated` or standard linear interpolation depending on aesthetic choice).
- **Aesthetics**: Implement a sleek "dark mode" aesthetic with vibrant cyan/blue ripple highlights.

#### Interaction Handling
- **Events**: Listen for `mousedown`, `mousemove` (when clicking), `touchstart`, and `touchmove`.
- **Projection**: Map screen coordinates to grid coordinates.
- **Debouncing**: Ensure rapid inputs are handled without overwhelming the simulation.

#### Architecture
- **`RippleSimulation` Class**: Encapsulates state (buffers) and physics logic.
- **`Renderer` Class**: Handles drawing the state to the canvas.
- **`App` Controller**: Manages the animation loop and input routing.

## Verification Plan

### Automated Tests
- Performance check: Monitor frame times to ensure < 16.6ms per frame.

### Manual Verification
- Test mouse clicks and drags for ripple generation.
- Test touch interactions on mobile-emulated browser.
- Verify window resizing behavior.
- Confirm interference patterns look natural when multiple ripples collide.
