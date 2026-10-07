# Changes 1008

- `monolithic_two_solids_residual_jacobian_scaled.m`: active-force activation fix (provided 10/7). For f0 < 0 (force
  pointing down, -z) the activation now uses the lowest leukocyte point (min z, the leading edge) and
  pore_depth = z_pore_bottom - z_min; for f0 >= 0 the highest point (max z) and pore_depth = z_max - z_pore_bottom.
  Before, the highest point was always used, so for f0 < 0 the activation factor was 0 and the force never acted.
- Previous version: `pre_change_backup_1008/`.
