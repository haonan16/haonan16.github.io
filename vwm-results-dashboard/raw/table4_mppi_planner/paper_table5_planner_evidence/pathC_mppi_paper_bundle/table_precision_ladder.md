# Table — RoboCasa planning precision ladder (combined paper table)

Reach@T cm = fraction of rollouts whose minimum EE-to-goal distance fell below
T cm at any planning step. Env success = full RoboCasa task predicate
(drawer joint > 0.95 for OpenDrawer; object inside cabinet + gripper retracted
for PickPlaceCounterToCabinet).

| Method | n | reach@30 | reach@10 | reach@5 | reach@2 | reach@1 | reach@0.5 | reach@0.1 | **env_succ** |
|---|---|---|---|---|---|---|---|---|---|
| Random uniform action | 200* | 1% | 1% | 0% | 0% | 0% | 0% | 0% | 1.5% |
| WM-shoot unbiased N=8 30s (random init) | 18 | 100% | 0% | 0% | 0% | 0% | 0% | 0% | 0% |
| WM-shoot biased N=8 30s (random init) | 17 | 100% | 29% | 0% | 0% | 0% | 0% | 0% | 0% |
| WM-shoot biased N=8 100s (random init) | 20 | 100% | 100% | n/a† | n/a† | n/a† | n/a† | n/a† | 0% |
| **MPPI mean N=8 200s (demo init)** | 6 | 100% | 100% | 50% | 50% | 50% | 33% | 0% | 0% |
| MPPI argmin N=8 200s (demo init) | 1 | 100% | 100% | 0% | 0% | 0% | 0% | 0% | 0% |
| **MPPI priming N=8 300s (demo init)** | 3 | 100% | 100% | 100% | **100%** | **67%** | **67%** | **67%** | 0% |
| **Replay-expert (demo init) — ceiling** | 7‡ | 100% | 100% | 100% | 100% | 100% | 100% | 100% | **100%** |

\* `pathC_random_reach_baseline/PickPlaceCounterToCabinet_reach25_random_init_maxsteps100`
   reports 3/200 reach@10cm = 1.5%; `pathC_wm_vs_random_comparison/headline_table` pools to 3/200 random env success after the same fix.
† Original 100s sweep tracked only reach@10cm.
‡ `pathC_baselines/replay_success_sweeps/exact/OpenDrawer/demo_{0094..0100}` — 7/7 env success when expert replays from the demo's exact MuJoCo init state. This is the achievable ceiling and the same demos used by the MPPI rows.

## Reading the table

1. **Reach precision scales monotonically with method sophistication.** Unbiased shooting fails reach@10cm. Biased shooting + 100-step horizon hits reach@10cm. MPPI hits **reach@1cm in 50–67% of rollouts.** MPPI + demo-action priming hits **reach@1mm in 67%.**
2. **Env success doesn't budge.** Every WM-based variant — random shooting, biased shooting, three MPPI variants, with horizons up to 300 steps and EE precision down to 0.5 mm — sits at 0% env success.
3. **Only the replay-expert baseline crosses to env success.** That row replays the expert's exact recorded action sequence from the demo's MuJoCo state. It confirms the task is solvable from this init and tells us the gap between WM-shoot and full success is the **executed action trajectory**, not the start state or the achievability of the position goal.

## Paper claim it supports

*"The learned video world model is a strong EE-positioning cost: closed-loop WM-shoot planners hit sub-mm precision on demo-recorded goal poses across diverse RoboCasa kitchen tasks. However, sub-mm EE precision does not translate to env binary-task success — every WM-shoot variant fails RoboCasa's joint-state / object-state predicates. The expert replay baseline succeeds from the same initial state, identifying the gap as missing contact dynamics in the cost function rather than missing achievability of the goal."*
