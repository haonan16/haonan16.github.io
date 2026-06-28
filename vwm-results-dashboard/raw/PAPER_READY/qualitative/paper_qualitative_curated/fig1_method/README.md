# Fig 1: Method Rendering — Action as Rendering

Shows the 3-channel action-projection image for both DROID (robot) and SSv2 (human hand).

## Channel explanation

| Channel | Color | What it encodes | Varies per frame? |
|---------|-------|----------------|-------------------|
| **R** | Red | Gaussian heatmap at the current EE/fingertip pixel position | Yes (tracks EE across frames) |
| **G** | Green | Arrow from EE[t] to EE[t+1] showing instantaneous direction (v2) | Yes (direction changes per frame) |
| **B** | Blue | Normalized per-pixel depth from camera (monocular for SSv2, ZED stereo for DROID) | Partly (depth at heatmap location varies) |

## Files

**DROID (robot arm, ZED stereo camera):**
- `fig1_droid_rgb.png` — Original RGB frame
- `fig1_droid_R_heatmap.png` — R channel: EE position heatmap
- `fig1_droid_G_direction.png` — G channel: EE direction arrow
- `fig1_droid_B_depth.png` — B channel: depth map
- `fig1_droid_composite.png` — All three channels overlaid

**SSv2 (human hand, monocular camera):**
- `fig1_ssv2_rgb.png` — Original RGB frame
- `fig1_ssv2_R_heatmap.png` — R channel: fingertip position heatmap (MediaPipe)
- `fig1_ssv2_G_direction.png` — G channel: fingertip direction arrow
- `fig1_ssv2_B_depth.png` — B channel: depth map (Depth Anything v2)
- `fig1_ssv2_composite.png` — All three channels overlaid

## Paper claim supported

"Action as rendering" — the same 3-channel image representation encodes both robot EE
trajectories (via extrinsics + intrinsics) and human fingertip trajectories (via MediaPipe +
monocular depth). This visual representation is what enables training on heterogeneous data.

## Rendering details

- Render version: v2 (per-frame arrow, not static polyline)
- R sigma: 16 px (at target resolution)
- G arrow thickness: 24 px, min length: 24 px
- Rendered at source resolution (720x1280 for DROID), then cropped+resized to target (480x832)
