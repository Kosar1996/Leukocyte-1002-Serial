# Leukocyte-1005-Serial (non-parallel)

Base: Leukocyte_Main_Files-1001_v1 (stable active force), all files unchanged except the 6 below.
Originals of the 6 changed files: `pre_change_backup_1005/`. All changes give bit-identical results to the original code
except where the restart behaviour is fixed (restart from a saved step now continues exactly like the uninterrupted run).

| File | Change |
|---|---|
| `softlube_run_case_global_coupled.m` | Her v1 solver + the restart fix (marked `[1002 FIX]`): output/checkpoint paths in the run folder; mesh/prestress files found when the stored path is not readable; `t0State`/`t0Fluid`/`baseE`/`baseL` saved, so a `.mat` file can be restarted from any saved step, repeatedly; run settings (incl. active force) applied after a restart; restart continues with the next dt, `dtPrev` and predictor setting of the uninterrupted run; optional `SOFTLUBE_STOP_AFTER_STEP` |
| `solve_finite_def_solid.m` | `[1005 OPT]` the line-search trial residuals call `assemble_finite_def_axisym` and `apply_interface_traction` with one output (the discarded tangents are no longer computed) |
| `assemble_finite_def_axisym.m` | `[1005 OPT]` with one output the element tangent and `K` are not computed (`K = []`); `Fint` bit-identical |
| `apply_interface_traction.m`, `apply_interface_traction_sensitivity.m` | Faster traction (10/2): vectorized force-only path, tangent only when requested, sparse matrix assembled once; bit-identical |
| `run_0930.m` | Optional run options: `RESTART_FILE`, `RESTART_STEP`, `FRESH_START=1` (t = 0, active force on) |

Run: `run('run_0930.m')` (restart from `case_7_t_step14.mat`, step 13). Input `.mat` files are not in the repo.
