# Policy-Eval 20h Run — Final Report

## Outputs

- Cross-policy table CSV: `results/policy_eval_20h_run/table5/table5_3vla.csv`
- Cross-policy table MD : `results/policy_eval_20h_run/table5/table5_3vla.md`
- Cross-policy summary  : `results/policy_eval_20h_run/table5/summary.json`
- Classifier summary    : `results/policy_eval_20h_run/classifier/summary.json`
- Paper-table LaTeX     : `/net/holy-isilon/ifs/rc_labs/ydu_lab/Lab/haonan/video_world_model/results/policy_eval_20h_run/paper_assets/table5_3vla.tex`
- Paper-classifier LaTeX: `/net/holy-isilon/ifs/rc_labs/ydu_lab/Lab/haonan/video_world_model/results/policy_eval_20h_run/paper_assets/table_classifier.tex`
- Appendix draft (MD)   : `/net/holy-isilon/ifs/rc_labs/ydu_lab/Lab/haonan/video_world_model/results/policy_eval_20h_run/paper_assets/appendix_classifier.md`

## Headline numbers
- Total rollouts: 160 (success rate 0.16)
- `scalars` pooled-CV RF AUROC = 0.886 ± 0.079
- `hidden_action` pooled-CV RF AUROC = 0.916 ± 0.061
- `hidden_both` pooled-CV RF AUROC = 0.933 ± 0.068
- `combined` pooled-CV RF AUROC = 0.927 ± 0.070
