# Wenbo

Wenbo is an event-driven scheduler for multiple DAG-shaped LLM workflows with bounded concurrency. It provides dependency tracking, logical resource slots, pluggable scheduling policies, execution backends, and runtime records.

## Features

Wenbo addresses resource fragmentation in large-scale agent serving by dynamically scheduling individual computation nodes across shared GPUs and CPUs, preserving task dependencies while improving resource utilization and throughput.

- **DAG-centric agent execution:** Represent reasoning and computation steps as nodes with explicit dependencies, exposing parallelism within and across agent tasks.
- **Dynamic reasoning graphs:** Incrementally materialize executable DAG flows and refine the reasoning graph as intermediate results become available, as described in the paper design.
- **Global dependency-aware scheduling:** Coordinate ready nodes across active DAGs and dispatch them according to their resource requirements and worker availability.
- **Shared GPU/CPU resources:** Allocate resources to individual computation nodes, allowing operations from different tasks to share heterogeneous resources without reserving a fixed resource pair for each task's lifetime.
- **Decoupled system architecture:** Separate reasoning orchestration (Director), resource scheduling (Scheduler), and node execution (Workers).

## Architecture

![Wenbo architecture: Director, Scheduler, and Workers](assets/wenbo_architecture.png)

Wenbo separates reasoning orchestration, global node scheduling, and heterogeneous execution through the Director, Scheduler, and Workers.

## System and benchmark

This repository contains the Wenbo workflow runtime. The independent ARGB benchmark maintains the paper datasets, discrete-event simulator, baselines, and experiments. The Quick Start below launches an ARGB experiment in an isolated subprocess.

Both distributions use the import name wenbo. Keep benchmark development in a separate environment; the Quick Start runs its source checkout directly and does not require installing the benchmark distribution.

## Quick start

Requires **Python 3.10 or newer**. From the system source directory, create a virtual environment:

```bash
python -m venv .venv
```

Activate it on macOS / Linux:

```bash
source .venv/bin/activate
```

Or on Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Install Wenbo, obtain the benchmark checkout, and run the experiment:

```bash
python -m pip install -e .
python -u examples/quickstart.py --benchmark-root ../benchmark
```

If you already have either checkout, use its existing directory. The benchmark must include the matching datasets and support --profile quickstart.

The default run compares Wenbo with the Flash-Searcher baseline on StrategyQA, MuSiQue, ToolBench, and ALFWorld. It uses discrete-event virtual time and requires no physical GPU, model download, or API key.

The terminal reports throughput, resource utilization, and speedup. Reports are saved to .local/quickstart-gpu2-cpu16/; reusing an output directory overwrites its reports. For a smaller first run:

```bash
python -u examples/quickstart.py --benchmark-root ../benchmark --limit 3 --seeds 11 --output-dir .local/paper-smoke
```

## License

Released under the [MIT License](LICENSE).
