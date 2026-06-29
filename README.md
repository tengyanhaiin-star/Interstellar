# 🌌 Interstellar Travel Simulation

A real-time 3D interstellar travel simulation rendered in a single HTML file using Three.js. Fly through a sea of 125,000 stars with physically-inspired Doppler color shifting and full camera rotation control.

## Demo

Open [Interstellar Travel](https://tengyanhaiin-star.github.io/Interstellar/) directly in any modern browser — no build step, no dependencies to install.

## Features

- **125,000 stars** distributed in a hollow cylindrical volume, rendered as glowing circular sprites
- **Doppler effect**: stars ahead of you shift red (receding), stars behind shift blue (approaching)
- **Uniform star density**: radial positions sampled via square-root distribution to ensure even coverage across the cylinder cross-section
- **Spherical visibility boundary**: only stars within a sphere of radius 1000 centered on the camera are visible, with a smooth fade at the edge
- **Inner exclusion zone**: a hollow core (inner radius 10) prevents stars from passing through the camera
- **Infinite loop**: stars travel along the z-axis and wrap seamlessly from the far end back to the start
- **Camera rotation**: drag to look in any direction — pitch clamped to ±90° to prevent flipping
- **iOS optimized**: Retina display support, safe area insets, orientation change handling, pinch-zoom prevention, and add-to-home-screen web app mode

## Controls

| Input | Action |
|---|---|
| Mouse drag | Rotate view |
| Touch drag (single finger) | Rotate view (mobile) |

## How It Works

### Star Distribution

Stars are placed inside a cylindrical volume:

- **Outer radius**: 1000 units
- **Inner radius**: 10 units (hollow core around the camera)
- **z range**: −1000 to +1000

Radial positions are drawn using `Math.sqrt(Math.random()) * outerRadius` so that the probability of a star landing in any annular ring is proportional to its area — producing uniform density from core to edge.

### Infinite Travel

The camera stays fixed at the origin `(0, 0, 0)`. Every frame, each star moves +0.1 units along the z-axis. When a star's z coordinate exceeds +1000, it wraps back to −1000, creating a seamless infinite loop.

### Doppler Effect

Each frame the radial velocity of a star relative to the camera is computed from its z position:

```
radialVelocity = starSpeed × (z / distance)
```

- **z > 0** (star ahead, receding) → red shift: red channel boosted, blue channel reduced
- **z < 0** (star behind, approaching) → blue shift: blue channel boosted, red channel reduced

The shift magnitude is clamped to ±0.3 and applied on top of each star's base color (white, pale yellow, or blue-white).

### Visibility Sphere

Stars outside a sphere of radius 1000 have their color set to black, effectively hiding them. Stars in the outer 10% of the sphere fade out smoothly to avoid a hard cutoff.

## Key Parameters

| Parameter | Value | Notes |
|---|---|---|
| `cylinderOuterRadius` | 1000 | Master scale reference |
| `cylinderInnerRadius` | 10 | Exclusion zone around camera |
| `particleCount` | 125,000 | `floor((outerRadius / 20)³)` |
| `starSpeed` | 0.1 | Units per frame along z-axis |
| `visibleRadius` | 1000 | Equals `cylinderOuterRadius` |
| FOV | 60° | Camera field of view |

## Dependencies

- [Three.js r128](https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js) — loaded from CDN, no local install needed

## Browser Compatibility

| Platform | Status |
|---|---|
| Chrome / Edge / Firefox (desktop) | ✅ |
| Safari (macOS) | ✅ |
| Safari (iOS) | ✅ — add to Home Screen for full-screen experience |
| Android Chrome | ✅ |

## License

MIT — see [LICENSE](LICENSE) for details.
