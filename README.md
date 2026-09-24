# AlgorithmArt

Procedural shader art in WebGL2 — step-by-step educational recreations inspired by the compact GLSL work of **Xor (@XorDev)** and other graphics programmers. Their tiny, dense procedural shaders are the blueprint for every technique rebuilt here.

Each demo is a **single, self-contained HTML file**: no build step, no dependencies, no textures, no models. Everything on screen is computed per-pixel on the GPU from pure math — ray emission, wave dynamics, volumetric ray marching, scattering, palettes, and tone mapping.

中文说明见 [README.zh-CN.md](README.zh-CN.md)。

## Quick Start

Open any `.html` file directly in a WebGL2-capable browser (a recent Chrome / Edge / Firefox). That is all there is to it.

- Move the mouse to orbit the camera (on larger screens).
- Use the buttons at the bottom to switch between Auto / High / Medium / Low render resolution.
- The HUD shows live FPS and the current render scale.

## The Two Series

### 🌙 MoonSea — Moon over the Sea (海上生明月)

A night ocean scene built from scratch in five progressive steps:

| Step | File | Focus |
|------|------|-------|
| 1 | `MoonSea/moonsea_step1_camerafrustum.html` | Ray emission: Rodrigues axis-angle camera rotation with a breathing focal length; the view frustum is visualized as a perspective grid |
| 2 | `MoonSea/moonsea_step2_starsmoon.html` | Procedural starfield: 250-cell hash grid with jittered placement, per-star hue/saturation, and atmospheric extinction; smoothstep moon disc with corona and halo |
| 3 | `MoonSea/moonsea_step3_gerstnerwave.html` | Wave dynamics: exponential Gerstner-like waves `exp(sin(x)-1)` with 36-octave iteration and positional drag, height field shown as contour lines |
| 4 | `MoonSea/moonsea_step4_raymarchnormal.html` | Water surface intersection: ray marching bounded between two planes (y = 0 / y = −depth), 36-tap finite-difference normals, distance-based normal flattening |
| 5 | `MoonSea/moonsea_step5_fresnelscatter.html` | Final shading: Schlick Fresnel, moonlit specular path, Beer–Lambert-style depth scattering, ACES tone mapping (`moonsea_step5_fresnelscatter_backup_v1.html` is an earlier backup variant) |

### 🌅 Sunset — Sunset Cloud Sea (落日云海)

A volumetric sunset cloudscape, also in five progressive steps:

| Step | File | Focus |
|------|------|-------|
| 1 | `Sunset/sunset_step1_slab.html` | Ray emission and a symmetric atmospheric slab (y = ±0.3) with adaptive ray-march step sizes |
| 2 | `Sunset/sunset_step2_turbulence.html` | 8-octave orthogonal permutation turbulence — `p += amp · sin(p·f − vel·t).yzx / f`, frequency doubled every octave |
| 3 | `Sunset/sunset_step3_inscattering.html` | Exponential photon in-scattering (Beer–Lambert approximation) — density = exp(s·10)/d gives the clouds volume |
| 4 | `Sunset/sunset_step4_palette.html` | Cosine sunset palette from spatial phase — the RGB channels are phase-shifted by [0, 1, 2] radians |
| 5 | `Sunset/sunset_step5_tonemap.html` | Final effect: 100-step volumetric ray marching × 8-octave turbulence × cosine palette × tanh tone mapping |

## Shared Engineering

- **WebGL2 + GLSL ES 3.00**: fullscreen triangle pair, raw WebGL API, zero third-party libraries.
- **Adaptive resolution scaling**: auto mode continuously measures frame time and scales the internal render target to hold ~60 FPS; manual High/Medium/Low overrides are provided.
- **Camera**: mouse-driven orbit on desktop; small screens fall back to a fixed, gently swaying auto camera.
- **Incremental steps**: each step adds exactly one technique on top of the previous one, so each series reads as a guided tour of the shader code — from the first ray to the final image.

## Repository Layout

```
AlgorithmArt/
├── MoonSea/    # Moon-over-the-sea series (5 steps + 1 backup)
└── Sunset/     # Sunset-cloud-sea series (5 steps)
```

## Credits

An homage to **Xor (@XorDev)** and the demoscene / Shadertoy community — the masters of saying the most with the fewest lines of GLSL.
