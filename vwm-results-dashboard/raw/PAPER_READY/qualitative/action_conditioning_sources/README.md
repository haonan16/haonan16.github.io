# Action Conditioning Sources

Curated paper-ready examples showing the same 3-channel action-image
conditioning format across data sources (human / robot / simulation).

Each subfolder contains:

- `raw.mp4` — resized raw video
- `action_conditioning.mp4` — 3-channel projection (R = current EE/fingertip
  heatmap, G = per-frame direction arrow, B = normalized depth)
- `overlay.mp4` — alpha blend of raw + action conditioning
- `raw_action_overlay.mp4` — 3-panel side-by-side: `raw | action | overlay`
- `manifest.json` — provenance + render config

All clips are rendered at **480×832 / 6 fps** using the v2 renderer (per-frame
arrow, σ=16px, arrow_thickness=24px, arrow_min_length=24px).

## Clips

### Originals (curated 2026-05-10)

| Slug | Source | Description |
|---|---|---|
| `human_ssv2/` | SSv2 #77488 | pulling wooden scale from left to right |
| `robot_droid/` | DROID RPL `Mon_Jun__5_18_30_00_2023`, ext1 | move object |
| `simulation_robocasa/` | RoboCasa `CloseCabinet/demo_0000` | close cabinet door |

### Expanded set (added 2026-05-10 via `scripts/make_action_conditioning_sources.py`)

Human (Something-Something V2), 6 clips:

| Slug | Clip | Caption |
|---|---|---|
| `human_ssv2_pulling_pen_l2r/` | 183553 | pulling pen from left to right |
| `human_ssv2_moving_tumbler_up/` | 154412 | moving tumbler up |
| `human_ssv2_pushing_sharpner_l2r/` | 53864 | pushing sharpner from left to right |
| `human_ssv2_moving_zip_down/` | 178129 | moving zip down |
| `human_ssv2_pulling_plate_r2l/` | 63378 | pulling plate from right to left |
| `human_ssv2_putting_binder_on_surface/` | 8245 | putting binder on a surface |

Robot (DROID), 5 clips:

| Slug | Episode | Camera | Lab task |
|---|---|---|---|
| `robot_droid_rad_fri_sep_29/` | RAD `Fri_Sep_29_17_37_03_2023` | ext1 | move object: grasp / relocate / flip |
| `robot_droid_rail_wed_apr_05/` | RAIL `Wed_Apr__5_15_08_50_2023` | ext1 | move object into/out of container |
| `robot_droid_iris_thu_mar_02/` | IRIS `Thu_Mar__2_14_57_27_2023` | ext1 | move object into/out of container |
| `robot_droid_rail_mon_jun_05_multi/` | RAIL `Mon_Jun__5_17_34_23_2023` | ext1 | do three tasks consecutively (multi-action) |
| `robot_droid_rad_fri_sep_29_ext2/` | RAD `Fri_Sep_29_17_37_03_2023` | ext2 | same episode as RAD, alternate camera angle |

Simulation (RoboCasa), 6 clips:

| Slug | Demo | Task |
|---|---|---|
| `simulation_robocasa_open_cabinet/` | `OpenCabinet/demo_0000` | open cabinet door |
| `simulation_robocasa_pickplace_to_sink/` | `PickPlaceCounterToSink/demo_0000` | pick item from counter, place in sink |
| `simulation_robocasa_turn_on_microwave/` | `TurnOnMicrowave/demo_0000` | turn on microwave |
| `simulation_robocasa_open_drawer/` | `OpenDrawer/demo_0000` | open drawer |
| `simulation_robocasa_pickplace_to_cabinet/` | `PickPlaceCounterToCabinet/demo_0000` | pick item from counter, place in cabinet |
| `simulation_robocasa_turn_on_sink_faucet/` | `TurnOnSinkFaucet/demo_0000` | turn on sink faucet |

## Regenerating / extending

```bash
# Default plan (8 new clips, ~1 min/clip):
python scripts/make_action_conditioning_sources.py

# Subset:
python scripts/make_action_conditioning_sources.py --sources ssv2 robocasa

# Single slug:
python scripts/make_action_conditioning_sources.py --slug human_ssv2_moving_tumbler_up

# Add a new clip: edit DEFAULT_PLANS in the script.
```

DROID episodes need `metadata_*.json`, `trajectory.h5`, and
`recordings/MP4/<serial>.mp4`, plus the ZED `SN<serial>.conf` calibration in
`datasets/calibrations/zed/`. RoboCasa demos need `rgb.mp4` + `cache.npz` + 
`meta.json`. SSv2 clips need entries in
`/net/holy-isilon/ifs/rc_labs/ydu_lab/Lab/haonan/ssv2_action_proj_cache/full_subset_v1/{fingertip,depth}/<bucket>/<clip_id>.{npz,npy}`.
