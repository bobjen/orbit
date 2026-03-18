# orbit

JavaScript gravitational orbit simulator, rendered on an HTML5 canvas.

Original implementation by Bob Jenkins (2019, public domain).

Documentation and live demos: https://burtleburtle.net/bob/js/orbit.html

## How it works

Newton's law gives accelerations from positions and masses.  Positions are
advanced using an explicit symmetric multistep method (1–15 prior
accelerations, default 8), which is time-reversible and more accurate than
simple Euler or Runge-Kutta for long simulations.  Several integration steps
are taken per displayed frame.

Massless bodies (mass 0) are skipped when computing forces on other bodies, so
a system with one massive sun and 1000 massless satellites runs in O(n) time
rather than O(n²).

## Controls

- Click without dragging — pause / resume
- Drag near the center — rotate the view (left/up)
- Drag near the edge — zoom and roll

## Usage

```html
<script src="orbit.js"></script>
<canvas id="sim"></canvas>
<script>
orbit({
    name: "sim",
    increment: 0.0015,
    work: 10,
    points: 8,
    background: "#000000",
    trail: true,
    eye: new Eye(5.0, new Point(0.0, 0.0, 0.0), 500.0),
    moons: [
        new Moon("sun",   new Point(0,0,0), new Point(0,0,0), 1000.0, "#ffff00", 0.05),
        new Moon("earth", new Point(5,0,0), new Point(0,0,-14), 1.0,  "#00aaff", 0.02),
    ]
});
</script>
```

## Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `name` | (required) | Canvas element id |
| `moons` | (required) | Array of `Moon` objects |
| `increment` | 1.0 | Simulated time per displayed frame |
| `work` | 20 | Integration steps per frame |
| `points` | 8 | Multistep order (1–15) |
| `framerate` | 20 | Milliseconds between frames |
| `background` | `"black"` | Canvas background color |
| `trail` | false | Draw orbital trails |
| `fade` | 0 | Trail fade rate (smaller = faster) |
| `eye` | `Eye(100, Point(0,0,0), 100)` | Camera: distance, rotation, zoom |
| `autocenter` | true | Keep center of mass at origin |
| `follow` | — | Name of moon to keep centered |
| `stop` | false | Start paused |
| `lifetime` | 0 (forever) | Simulated time before auto-restart |
| `scalemass` | 1.0 | Scale all masses by this factor |
| `dejitter` | false | Smooth out integration jitter (non-reversible) |
| `noPerspective` | false | Orthographic projection |
| `energy` | false | Report total energy |

## Moon constructor

```javascript
new Moon(name, position, velocity, mass, color, radius)
// All positions and velocities are Point(x, y, z)
```

## Building

No build step — `orbit.js` is a single self-contained script.  Open
`orbit.html` in a browser to see the included demo.
