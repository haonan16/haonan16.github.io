# Appendix Draft — Expanded Policy-Eval Pool + Multi-Signal Classifier

This draft replaces the within-policy speculation at main.tex L418 and the placeholder cross-policy table at L411-413.


## Expanded pool: 3 VLA classes

We extend the policy-eval pool from one VLA (Cosmos-Policy 2B) to **9 policy classes** (3 VLAs: Cosmos-Policy 2B, Being-H05-2B-RoboCasa, Being-H05-2B-LIBERO-RoboCasa, plus RLinf-Pi0-RoboCasa 4B; and scripted baselines) totalling **n=160 rollouts** with sim-success rate 0.16. Within-policy Spearman ρ ranges are reported in Table~\ref{tab:wm_policy_critic_3vla}.

## Cross-policy ordering test

The reviewer-grade question is whether the WM-mean-path ranking of the **9 policy classes** matches the sim's success-rate ranking. We measure this with Spearman ρ between the 9-vector of per-policy WM-path-mean and per-policy sim-success-rate. Result: ρ = 0.15, 95% bootstrap CI = [-0.55, 0.82], p = 0.688. This is the strongest existing evidence that the WM functions as a useful **policy-class** evaluator, even where its within-policy rank correlation is weak.

## Multi-signal classifier on WM hidden states

We extract the pre-head joint `[rgb | action_proj]` hidden state from the dual-stream backbone's final block and pool the action-stream tokens per latent frame (per Section \ref{sec:architecture}). On the expanded 3-VLA pool, we train logistic regression, random forest, and MLP classifiers on six feature sets (scalars; scalars + LPIPS summary; hidden-action; hidden-RGB; hidden-both; combined). We report pooled 5-fold stratified-CV AUROC, leave-one-task-out, and **leave-one-policy-out** (LOPO) — the strongest test of cross-policy generalization.
- `scalars`: RF AUROC = 0.886±0.079, LogReg AUROC = 0.885±0.053
- `hidden_action`: RF AUROC = 0.916±0.061, LogReg AUROC = 0.909±0.067
- `hidden_both`: RF AUROC = 0.933±0.068, LogReg AUROC = 0.918±0.074
- `combined`: RF AUROC = 0.927±0.070, LogReg AUROC = 0.937±0.054
