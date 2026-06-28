# Appendix / Debug Artifacts

These are debug and appendix-only visualizations, not main paper figures.

## Seed Debug PNGs

`seed_debug_{A,C}/{EP}_seed_debug.png`

Each image shows: [anchor RGB with CoTracker seed point | R-channel heatmap with seed point].
The green dot marks where CoTracker is seeded for point tracking. This verifies the seed
is on the gripper/contact point, not on a background feature.

All 4 episodes x 2 methods = 8 debug images.

## IRIS Hard Episode

`iris_hard_episode/{A,C}/IRIS_{action}.mp4`

IRIS is consistently the hardest episode: DirAcc=0.250, D_sep < 0.2 px across all methods.
These videos show the CoTracker-tracked rollouts for IRIS under all action commands.

**Possible explanations:**
- The IRIS scene has high visual clutter near the EE, making CoTracker tracking unreliable
- The EE may be in a workspace region where the camera projection compresses base-frame
  motion to < 1 pixel of image-space displacement
- The model may genuinely fail on this scene configuration

This is framed as a "hard episode / tracking limitation" in the paper. IRIS is kept in
the quantitative mean — it is not excluded.

## Why these are appendix-only

- Seed debug images are implementation verification, not a paper claim
- IRIS failure analysis is transparency, not a positive result
- These support reproducibility and honest reporting
