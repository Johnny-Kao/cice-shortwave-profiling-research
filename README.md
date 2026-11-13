# CICE Shortwave profiling — Research

Independent whole-CICE timing study, not a patch to any upstream repository.

Goal: measure elapsed `Shortwave / Timeloop` in a genuine compiled CICE standalone run. The CICE `Shortwave` timer includes shortwave radiation *and* albedo. It is not an isolated timer for `solution_dEdd()`. See CICE official timer documentation.

Initial experiment: small built-in box case with no external forcing-data requirements, configured to use `dEdd`, GNU compiler, single MPI rank. It is a preliminary sanity measurement, **not** a representative global CICE performance profile.

A real research conclusion requires checking generated namelist, run PASS, and timer output. Outputs and upstream SHA are retained by CI. No modifications to Toolkit/Staging/upstream.
