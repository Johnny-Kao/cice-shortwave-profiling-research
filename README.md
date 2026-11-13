# CICE Shortwave profiling — Research

Independent whole-CICE timing study, not a patch to any upstream repository.

Goal: measure elapsed `Shortwave / Timeloop` in a genuine compiled CICE standalone run. The CICE `Shortwave` timer includes shortwave radiation *and* albedo. It is not an isolated timer for `solution_dEdd()`. See CICE official timer documentation.

Initial experiment: small built-in box case with no external forcing-data requirements, configured to use `dEdd`, GNU compiler, single MPI rank. It is a preliminary sanity measurement, **not** a representative global CICE performance profile.

A real research conclusion requires checking generated namelist, run PASS, and timer output. Outputs and upstream SHA are retained by CI. No modifications to Toolkit/Staging/upstream.


## Decision — PARK (2026-11-14 JST)

User threshold for pursuing **performance-only** Smart PR opportunities: target **>=5x acceleration**, not small constant-factor wins. Distinguish a 5x improvement in a local kernel from a 5x improvement in full-model wall time; neither has been demonstrated here.

Verified whole-CICE baseline with official `dEdd` shortwave:
- [gbox80, ktherm=1, boxnodyn](https://github.com/Johnny-Kao/cice-shortwave-profiling-research/actions/runs/43497549237): Shortwave 0.47 s / TimeLoop 17.21 s = 2.73%; winter/zero solar forcing.
- [gbox128, ktherm=2, boxnodyn](https://github.com/Johnny-Kao/cice-shortwave-profiling-research/actions/runs/43498296647): Shortwave 0.70 s / TimeLoop 30.09 s = 2.33%; winter/zero solar forcing.
- [gbox128, ktherm=2, official default summer forcing](https://github.com/Johnny-Kao/cice-shortwave-profiling-research/actions/runs/43498849465): Shortwave 4.04 s / TimeLoop 54.11 s = **7.47%**, absorbed shortwave ~53 W/m², whole driver PASS.

Amdahl bound: even eliminating the entire Shortwave timer in the summer box example improves overall model time at most `54.11/(54.11-4.04) = 1.081x` (~8.1% faster). Therefore, reaching **5x whole-model acceleration via Shortwave alone is impossible in this measured scenario**. At a hypothetical **5x Shortwave-kernel** improvement, the whole model improves only about `1/(1-0.0747+0.0747/5)=1.063x`. These are single-case theoretical bounds, not predictions for other production workloads.

**Disposition: PARK.** Do not spend more compute on this performance candidate or stage/open an upstream PR without new evidence of substantially different high-impact workload or a correctness defect. Research workflows remain manual-only and upstream/Toolkit/Staging unchanged.
