# DRCFJSP: benchmark instances, solver outputs and detailed results

This repository contains the benchmark instances, all solver output files and the detailed results for the paper

> R. Ghorbani Saber, P. Leyman, E.-H. Aghezzaf. *A Benders Decomposition-Based Hybrid Approach to the Dual Resource Constrained Flexible Job Shop Scheduling Problem.*

The paper studies the dual resource constrained flexible job shop scheduling problem (DRCFJSP). Each operation must be assigned to an eligible machine and a qualified operator, and it must be sequenced on both resources so that the makespan is minimized.

The source code is not included in this repository.

## Contents

| Path | Content |
|---|---|
| `Inputs/` | the 262 benchmark instances: WFJSP `wfjsp1_1`–`wfjsp24_10` (240), MK `Mk01`–`Mk10`, DP `Dauzere_01a`–`Dauzere_12a` |
| `Outputs/CP/` | CP Optimizer runs (1 h): the reference solutions of all comparisons |
| `Outputs/Rerun_2026-10/` | the four LBBD variants (1 h) and the six tabu search configurations, seed 1 |
| `Outputs/FaFComparison/` | FaF, FaFM, FaFMx2, FaFMx3, FaFMJ, TS@FaF, TS@FaFM and TSN1@FaFM with seeds 1–5 (`<instance>_s<seed>.txt`), and the TSN1 configurations with seed 1; `results/` holds a one-line summary per run (method, instance, seed, makespan, time) |
| `results/` | detailed results (CSV and Excel), the statistical tests and the figures of the paper (see below) |

## Methods

| Name | Method |
|---|---|
| `CP` | CP model solved with CP Optimizer |
| `LBBD_CP`, `CP_LBBD_CP` | LBBD with CP subproblems, without and with an initial CP phase |
| `LBBD_CP_TS`, `CP_LBBD_CP_TS` | the proposed hybrid LBBD with tabu search, without and with an initial CP phase |
| `TS_a_b_c` | standalone tabu search with at most `a` iterations, a time limit of `b` minutes and `c` back-jump tracks |
| `FaF`, `FaFM` | re-implementation of the filter-and-fan heuristics of Müller & Kress (2022) with the recommended parameter values |
| `FaFMx2`, `FaFMx3` | FaFM with doubled (η1 = 30, η2 = 50, L = 600) and tripled (η1 = 45, η2 = 75, L = 900) parameter values |
| `TS@FaF`, `TS@FaFM` | tabu search with exactly the computation time used by FaF or FaFM on the same instance and seed |
| `FaFMJ` | FaFM with the proposed joint neighborhood |
| `TSN1_a_b_c`, `TSN1@FaFM` | tabu search with the neighborhood N1 of filter-and-fan |

## Output format

Each output file contains:
- the method and the seed;
- `CPU Time:` (wall-clock seconds);
- `obj:` (makespan);
- `Best found lower bound:` (exact methods);
- the schedule: for every operation its machine, operator, start and completion time.

All schedules in `Outputs/` were checked for feasibility: eligibility, durations, precedences, no overlaps on machines and operators, and `obj` equal to the makespan.

## Results

| File | Content |
|---|---|
| `results/instance_results.csv` | one row per instance with the best known lower bound and, for every method, the makespan (UB), the lower bound (exact methods) and the time; for the methods run with five seeds, the mean and the best makespan over the seeds |
| `results/instances.csv` | one row per (method, instance) for CP, the four LBBD variants and the six TS configurations: UB, LB, optimality gap (UB−LB)/UB, RPD with respect to the best UB of the exact methods, deviation from CP, time |
| `results/faf_runs.csv` | one row per run of the filter-and-fan comparison (method, instance, seed, makespan, time, CP makespan `ref`, deviation from CP in % `gap`) |
| `results/HPC_Results_Rerun_2026-10.xlsx` | the makespans, lower bounds, gaps and times of the exact methods, the TS configurations and the filter-and-fan comparison per instance, with summary sheets |
| `results/Statistical_tests.xlsx` | the paired t-tests of Section 5: the tests of Section 5.1 (gap and RPD of the exact methods) and the comparisons of Table 4 and Section 5.3 (deviation from CP). Each test is computed step by step with worksheet formulas on the raw data |
| `results/figures/` | Figures 5–9 of the paper: `optimality_gaps.pdf` (Fig. 5), `rpd.pdf` (Fig. 6), `ts_deviation.pdf` (Fig. 7), `ts_time.pdf` (Fig. 8), `quality_time.pdf` (Fig. 9) |

Definitions:
- optimality gap: (UB − LB) / UB;
- RPD: (UB − UB\*) / UB\*, where UB\* is the best UB over the exact methods;
- deviation from CP: (UB − UB_CP) / UB_CP, where UB_CP is the makespan of the one-hour CP run.

The deviations in `Statistical_tests.xlsx` are recomputed from the makespans, so that equal makespans are exact ties. For methods run with five seeds, the seeds are averaged per instance before the test. When a method with five seeds is compared with a method run with seed 1 only, seed 1 of both is used.

## Instance format

Instances are text files with literal section headers:
- `Jobs:`, `Machines:`, `Workers:`;
- `Operations per job:`;
- `Predecessors per operation:` and `Successors per operation:`;
- `Machines per operation:`;
- `Processing times (o m w p):`, one row per eligible (operation, machine, operator) combination with its processing time.

All IDs are 0-based.

## Benchmark sources

- **WFJSP**: Müller, D., Kress, D. (2022). Filter-and-fan approaches for scheduling flexible job shops under workforce constraints. *International Journal of Production Research*, 60(15), 4743–4765.
- **MK and DP**: Brandimarte (1993) and Dauzère-Pérès & Paulli (1997), extended to the DRCFJSP by Lei, D., Guo, X. (2014). Variable neighbourhood search for dual-resource constrained flexible job shop scheduling. *International Journal of Production Research*, 52(9), 2519–2529.
