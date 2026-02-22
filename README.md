# Guide

## Read This First: Provided Results for Direct Comparison

If your goal is to compare your method against our reported results, check these folders first.

Common structure in each module (`Algorithm_DP_F`, `Algorithm_DP_G`, `Algorithm_DP_H`):

- Instances: `<module>/results/instances_txt/`
- Per-instance detailed results: `<module>/results/raw/`
- Aggregated table-ready results: `<module>/results/summary/`

Key files typically used for comparison:

- DP-F baseline: `Algorithm_DP_F/results/raw/dp_f_fill_detailed.csv`
- DP-F speedup: `Algorithm_DP_F/results/raw/dp_f_speedup_fill_detailed.csv`
- DP-G: `Algorithm_DP_G/results/raw/dp_g_fill_detailed.csv`
- DP-H: `Algorithm_DP_H/results/raw/dp_h_fill_detailed.csv`
- Gurobi (F/G/H): `Algorithm_DP_F/results/raw/gurobi_fill_detailed.csv`, `Algorithm_DP_G/results/raw/gurobi_fill_detailed.csv`, `Algorithm_DP_H/results/raw/gurobi_fill_detailed.csv`

How to read objective values and status:

1. Use `results/raw/*.csv` for batch comparison (`*_obj`, runtime, status, timeout).
2. Use instance `.txt` files for per-case verification (`obj_*`, `time_*`, `status_*`; written when using `--backfill-txt`).

This repository contains three independent experiment modules:

- `Algorithm_DP_F`
- `Algorithm_DP_G`
- `Algorithm_DP_H`

Each module includes:

- C++ solver source code
- Python batch runner scripts
- Gurobi-based comparison scripts
- Instance and output directories

## Repository Layout

```text
response_0220/
├── Algorithm_DP_F/
│   ├── scripts/                  # build/run/generate scripts
│   ├── src/
│   │   ├── cpp/                  # C++ implementations (DP-F family)
│   │   └── gurobi/               # Gurobi model wrapper/code
│   ├── results/
│   │   ├── instances_txt/        # generated instances (*.txt)
│   │   ├── raw/                  # detailed run outputs (*.csv)
│   │   └── summary/              # table-ready summaries (*.csv)
│   └── README.md
├── Algorithm_DP_G/
│   ├── scripts/                  # build/run/generate scripts
│   ├── src/
│   │   ├── cpp/                  # C++ implementations (DP-G)
│   │   └── gurobi/               # Gurobi model wrapper/code
│   ├── results/
│   │   ├── instances_txt/        # generated instances (*.txt)
│   │   ├── raw/                  # detailed run outputs (*.csv)
│   │   └── summary/              # table-ready summaries (*.csv)
│   └── README.md
├── Algorithm_DP_H/
│   ├── scripts/                  # build/run/generate scripts
│   ├── src/
│   │   ├── cpp/                  # C++ implementations (DP-H)
│   │   └── gurobi/               # Gurobi model wrapper/code
│   ├── results/
│   │   ├── instances_txt/        # generated instances (*.txt)
│   │   ├── raw/                  # detailed run outputs (*.csv)
│   │   └── summary/              # table-ready summaries (*.csv)
│   └── README.md
├── .venv_all_algorithms/         # local environment (optional to share)
└── README.md
```

## Requirements

- Python 3.11+
- `g++` with C++17 support
- Gurobi with a valid license

Activate environment (recommended):

```bash
cd <repo_root>
source .venv_all_algorithms/bin/activate
python -c "import gurobipy as gp; print(gp.gurobi.version())"
```

If you do not include `.venv_all_algorithms` in your upload, create your own virtual environment and install at least `gurobipy`.

## Build C++ Solvers

```bash
bash Algorithm_DP_F/scripts/build_cpp.sh
bash Algorithm_DP_G/scripts/build_cpp.sh
bash Algorithm_DP_H/scripts/build_cpp.sh
```

## Quick Reproduction Example

Use `Algorithm_DP_G` as a complete runnable example:

```bash
cd <repo_root>
source .venv_all_algorithms/bin/activate

# 1) Generate instances
python Algorithm_DP_G/scripts/generate_classIIB_instances_random.py --clear-existing

# 2) Run DP-G
python Algorithm_DP_G/scripts/DPG_solve_instances.py --backfill-txt

# 3) Run Gurobi (single worker by default)
python Algorithm_DP_G/scripts/Gurobi_solve_instances.py --backfill-txt
```

## How To Run

### 1) Algorithm_DP_F

Generate instances:

```bash
python Algorithm_DP_F/scripts/generate_classI_instances_random.py --clear-existing
```

Run DP-F baseline:

```bash
python Algorithm_DP_F/scripts/DPF_solve_instances.py --backfill-txt
```

Run DP-F speedup (current active variant):

```bash
python Algorithm_DP_F/scripts/DPF_speedup_solve_instances.py --backfill-txt
```

Run Gurobi:

```bash
python Algorithm_DP_F/scripts/Gurobi_solve_instances.py --workers 10 --backfill-txt
```

### 2) Algorithm_DP_G

Generate instances:

```bash
python Algorithm_DP_G/scripts/generate_classIIB_instances_random.py --clear-existing
```

Run DP-G:

```bash
python Algorithm_DP_G/scripts/DPG_solve_instances.py --backfill-txt
```

Run Gurobi:

```bash
python Algorithm_DP_G/scripts/Gurobi_solve_instances.py --backfill-txt
```

### 3) Algorithm_DP_H

Generate instances:

```bash
python Algorithm_DP_H/scripts/generate_classIIA_instances_random.py --clear-existing
```

Run DP-H:

```bash
python Algorithm_DP_H/scripts/DPH_solve_instances.py --backfill-txt
```

Run Gurobi:

```bash
python Algorithm_DP_H/scripts/Gurobi_solve_instances.py --backfill-txt
```

## Default Output Files

### Algorithm_DP_F

- DP-F raw: `Algorithm_DP_F/results/raw/dp_f_fill_detailed.csv`
- DP-F summary: `Algorithm_DP_F/results/summary/table_dp_f_only_table_ready.csv`
- DP-F speedup raw: `Algorithm_DP_F/results/raw/dp_f_speedup_fill_detailed.csv`
- DP-F speedup summary: `Algorithm_DP_F/results/summary/table_dp_f_speedup_only_table_ready.csv`
- Gurobi raw: `Algorithm_DP_F/results/raw/gurobi_fill_detailed.csv`
- Gurobi summary: `Algorithm_DP_F/results/summary/table_gurobi_only_table_ready.csv`

### Algorithm_DP_G

- DP-G raw: `Algorithm_DP_G/results/raw/dp_g_fill_detailed.csv`
- DP-G summary: `Algorithm_DP_G/results/summary/table_dp_g_only_table_ready.csv`
- Gurobi raw: `Algorithm_DP_G/results/raw/gurobi_fill_detailed.csv`
- Gurobi summary: `Algorithm_DP_G/results/summary/table_gurobi_only_table_ready.csv`

### Algorithm_DP_H

- DP-H raw: `Algorithm_DP_H/results/raw/dp_h_fill_detailed.csv`
- DP-H summary: `Algorithm_DP_H/results/summary/table_dp_h_only_table_ready.csv`
- Gurobi raw: `Algorithm_DP_H/results/raw/gurobi_fill_detailed.csv`
- Gurobi summary: `Algorithm_DP_H/results/summary/table_gurobi_only_table_ready.csv`

## Notes

- Default per-instance time limit is `1800` seconds (override with `--time-limit`).
- `--backfill-txt` writes results back into files under `results/instances_txt`.
- `Algorithm_DP_G/scripts/Gurobi_solve_instances.py` uses single worker by default and writes one ordered raw file following `instances_txt` order.
- For GitHub upload, exclude local cache files such as `__pycache__` and `.DS_Store`.
