# Changes 1013

- Stabilization changes provided on 10/13 (made to the 10/8 parallel code), merged with the 10/9 changes:
  - `softlube_run_case_global_coupled.m`: the time step of step 14 is limited to 1.25e-6 s (`tn == 13`);
    `v_wall_E` / `v_wall_L` are set to zero if missing; the monolithic solve is wrapped in try/catch, so that an
    fsolve failure halves the time step and retries the step (`[AUTOMATIC RETRY]`).
  - `solve_monolithic_two_solids_fsolve_timestep.m`: fsolve OutputFcn that prints `[fsolve Guard]` lines for
    iterations 1-3 (print only); a result with exitflag <= 0 always goes through the line search and raises
    `Monolithic:fsolveFailed` (time-step reduction) if the line search fails or the residual stays above 0.1.
- Kept from 10/9: ramp start = start time of the run, `[1009 RAMP]` log lines; `SOFTLUBE_STOP_AFTER_STEP` removed.
- `run_1013.m`: copy of `run_0930.m` with only the restart changed (`case_7_t_step28.mat`, `targetStep = 27`, step 27 -> 28).
- Previous versions: `pre_change_backup_1013/`.
