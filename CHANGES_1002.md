# Leukocyte-1002-Serial (non-parallel)

Base: Leukocyte_Main_Files-1001 (active force in the Jacobian solve), all files unchanged except the 4 below.
Originals of the 4 changed files: `pre_fix_backup_1002/`.

| File | Change |
|---|---|
| `softlube_run_case_global_coupled.m` | Restart fix, all changes marked `[1002 FIX]`: checkpoint/output paths re-based to the run folder; prestress paths fall back to the code folder; `t0State`/`t0Fluid`/`baseE`/`baseL` saved, so a `.mat` file can be restarted from any saved step, repeatedly; stored `parOverrides` only fill missing fields (runtime cfg wins, active force kept); optional `SOFTLUBE_STOP_AFTER_STEP` |
| `apply_interface_traction.m`, `apply_interface_traction_sensitivity.m` | Faster traction: vectorized force-only path, tangent only when requested, sparse matrix assembled once. Bit-identical to the original (unit test) |
| `run_0930.m` | Optional run options: `RESTART_FILE`, `RESTART_STEP`, `FRESH_START=1` (t = 0, active force on) |

Run: `run('run_0930.m')` (restart from `case_7_t_step14.mat`, step 13). Input `.mat` files are not in the repo.
