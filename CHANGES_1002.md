# Leukocyte-1002-Serial (non-parallel)

Base: Leukocyte_Main_Files-1001 (active force in the Jacobian solve), all files unchanged except the 4 below.
Originals of the 4 changed files: `pre_fix_backup_1002/`.

| File | Change |
|---|---|
| `softlube_run_case_global_coupled.m` | Restart fix, all changes marked `[1002 FIX]`: checkpoint/output paths re-based to the run folder; prestress paths fall back to the code folder; `t0State`/`t0Fluid`/`baseE`/`baseL` saved, so a `.mat` file can be restarted from any saved step, repeatedly; stored `parOverrides` only fill missing fields (runtime cfg wins, active force kept); optional `SOFTLUBE_STOP_AFTER_STEP` |
| `apply_interface_traction.m`, `apply_interface_traction_sensitivity.m` | Faster traction: vectorized force-only path, tangent only when requested, sparse matrix assembled once. Bit-identical to the original (unit test) |
| `run_0930.m` | Optional run options: `RESTART_FILE`, `RESTART_STEP`, `FRESH_START=1` (t = 0, active force on) |

Run: `run('run_0930.m')` (restart from `case_7_t_step14.mat`, step 13). Input `.mat` files are not in the repo.

## Update 10/2: restart state fix (`softlube_run_case_global_coupled.m`, marked `[1002 FIX]`)

A restart now continues exactly like the uninterrupted run, also after a step that needed a dt retry:
- next dt = min(par.dt, dtGrowFactor * dt of the restart step) after a retried/reduced step (was always par.dt);
- dtPrev of the fsolve warm start = dt of the restart step (was par.dt);
- a file written by the fixed code keeps the predictor setting (`useMonoPredictor`) of the run that wrote it; older files (e.g. the step-13 input) keep the original behaviour (predictor on).

Tests (check_fix_1002, OVERALL PASS): restart 13 -> 14 -> 15 bit-identical to 13 -> 15 (parallel N = 8 and serial); with the divergence retry (test code only, not in this repository) 0 -> 1 -> 2 bit-identical to 0 -> 2.
