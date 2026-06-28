# Franka horizon-mode tracked-pixel displacement (paper §5)

Active = mean of all ± axis nudge conditions; null = 0-magnitude
rollout (control: how much does the model drift WITHOUT a command).
Values in pixels (image-space tracked end-effector displacement).

| Task | Ckpt | disp @h=4 | @h=8 | @h=12 | @h=15 | null @h=15 |
|---|---|---|---|---|---|---|
| Box / success | A nf=17 (DROID-only) | 1.2 | 2.1 | 2.1 | 2.3 | 3.1 |
| Box / success | 3way nf=17 (DROID+SSv2+RoboCasa) | 4.3 | 17.7 | 42.7 | 57.9 | 3.8 |
| Box / failure | A nf=17 (DROID-only) | 1.2 | 3.1 | 6.8 | 12.1 | 2.2 |
| Box / failure | 3way nf=17 (DROID+SSv2+RoboCasa) | 5.7 | 17.6 | 33.9 | 43.6 | 2.3 |
| Mustard / success | A nf=17 (DROID-only) | 7.1 | 6.6 | 6.9 | 6.8 | 3.6 |
| Mustard / success | 3way nf=17 (DROID+SSv2+RoboCasa) | 14.9 | 35.9 | 62.2 | 71.7 | 5.5 |
| Mustard / failure | A nf=17 (DROID-only) | 0.9 | 2.9 | 3.9 | 4.7 | 1.2 |
| Mustard / failure | 3way nf=17 (DROID+SSv2+RoboCasa) | 1.3 | 5.6 | 17.3 | 28.3 | 4.2 |
| Rope / success | A nf=17 (DROID-only) | 0.6 | 4.4 | 9.9 | 10.3 | 1.2 |
| Rope / success | 3way nf=17 (DROID+SSv2+RoboCasa) | 1.0 | 8.0 | 29.3 | 32.8 | 7.5 |
| Rope / failure | A nf=17 (DROID-only) | 0.6 | 1.2 | 2.5 | 3.8 | 2.2 |
| Rope / failure | 3way nf=17 (DROID+SSv2+RoboCasa) | 0.5 | 1.1 | 15.1 | 21.8 | 4.5 |
| Rice cooker / success | A nf=17 (DROID-only) | 10.4 | 25.4 | 38.4 | 49.6 | 2.7 |
| Rice cooker / success | 3way nf=17 (DROID+SSv2+RoboCasa) | 2.2 | 4.7 | 13.2 | 24.7 | 1.8 |
| Rice cooker / failure | A nf=17 (DROID-only) | 11.5 | 19.8 | 31.9 | 40.8 | 2.5 |
| Rice cooker / failure | 3way nf=17 (DROID+SSv2+RoboCasa) | 8.1 | 19.9 | 33.5 | 47.4 | 1.3 |
| Whiteboard / success | A nf=17 (DROID-only) | 0.7 | 1.4 | 2.7 | 3.6 | 1.7 |
| Whiteboard / success | 3way nf=17 (DROID+SSv2+RoboCasa) | 0.6 | 1.8 | 6.3 | 8.4 | 1.2 |
| Whiteboard / failure | A nf=17 (DROID-only) | 0.6 | 1.1 | 1.6 | 2.2 | 1.3 |
| Whiteboard / failure | 3way nf=17 (DROID+SSv2+RoboCasa) | 0.5 | 2.5 | 9.5 | 20.6 | 8.3 |
