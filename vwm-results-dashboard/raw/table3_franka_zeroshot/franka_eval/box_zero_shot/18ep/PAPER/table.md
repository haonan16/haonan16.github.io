# Zero-shot generalization to franka_box (paper appendix)


**Setup:** 4 held-out Franka box-manipulation episodes, ±x ±y action interventions
at 0.02 m magnitude, sample_steps=40, n_frames=17, use_real_history=True.
Eval protocol matches the canonical paper protocol from
`scripts/eval_action_metrics.py` — only the dataset is swapped from DROID
to franka_box (a different Franka setup with different cameras and lab
geometry, never seen during training).

| method | DirAcc | DirAcc 95% CI | D_sep (px) | n_eps | n_trials |
| --- | --- | --- | --- | --- | --- |
| A nf=17 (DROID-only) | 0.542 (39/72) | [0.431, 0.653] | 0.24 [0.1, 0.3] | 18 | 72 |
| 3way nf=17 (DROID+SSv2+RoboCasa) | 0.681 (49/72) | [0.569, 0.778] | 0.34 [0.2, 0.5] | 18 | 72 |
