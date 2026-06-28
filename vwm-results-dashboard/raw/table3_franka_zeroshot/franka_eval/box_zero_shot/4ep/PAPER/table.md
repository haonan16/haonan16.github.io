# Zero-shot generalization to franka_box (paper appendix)


**Setup:** 4 held-out Franka box-manipulation episodes, ±x ±y action interventions
at 0.02 m magnitude, sample_steps=40, n_frames=17, use_real_history=True.
Eval protocol matches the canonical paper protocol from
`scripts/eval_action_metrics.py` — only the dataset is swapped from DROID
to franka_box (a different Franka setup with different cameras and lab
geometry, never seen during training).

| method | DirAcc | DirAcc 95% CI | D_sep (px) | n_eps | n_trials |
| --- | --- | --- | --- | --- | --- |
| A nf=17 (DROID-only) | 0.500 (8/16) | [0.250, 0.750] | 0.30 [0.1, 0.6] | 4 | 16 |
| 3way nf=17 (DROID+SSv2+RoboCasa) | 0.625 (10/16) | [0.375, 0.875] | 0.27 [0.1, 0.6] | 4 | 16 |
