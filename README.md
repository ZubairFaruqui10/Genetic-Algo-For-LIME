# GA-XAI: Genetic Algorithm with Explainable AI-Guided Bound Narrowing

An optimization framework that combines Genetic Algorithms (GA) with LIME-based Explainable AI (XAI) to iteratively narrow search bounds, improving convergence on benchmark optimization problems.

## How It Works

Each iteration:
1. **Optimize** — Run GA (or DE/PSO/ES) on the problem within current bounds
2. **Log** — Record solutions, fitness values, and constraint satisfaction per generation
3. **Explain** — Use LIME on a trained Random Forest to identify which features (dimensions) most influence fitness and constraint satisfaction
4. **Update bounds** — Narrow lower/upper bounds for the top-K influential features based on their correlation direction
5. **Repeat** — Next iteration runs within tighter bounds

This loop focuses the search space over time, guided by XAI-derived feature importance rather than random restarts.

## Project Structure

```
GA_XAI/
├── genticalgorithm.py   # Main entry point — runs the GA+XAI loop
├── XAI.py               # LIME-based explainability and feature ranking
├── problem_config.py    # Problem factory (Zakharov, Ackley, Sphere, etc.)
├── plots.py             # Fitness convergence plots
├── GA_XAI_plots/        # Output directory for generated plots
└── optimization_log_GA.csv  # Sample output log
```

## Supported Problems

- Zakharov, Ackley, Sphere, Rastrigin, Rosenbrock, Schwefel, Griewank, Himmelblau
- Constrained: G1, G2

## Supported Algorithms

Configured in `genticalgorithm.py` — swap by uncommenting:
- `GA` (default)
- `DE` (Differential Evolution)
- `PSO` (Particle Swarm Optimization)
- `ES` (Evolution Strategy)

## Dependencies

```
pymoo
scikit-learn
lime
numpy
pandas
scipy
matplotlib
```

Install with:
```bash
pip install pymoo scikit-learn lime numpy pandas scipy matplotlib
```

## Configuration

Key parameters in `genticalgorithm.py`:

| Parameter | Default | Description |
|---|---|---|
| `pop_size` | 50 | Population size |
| `n_gen` | 50 | Generations per iteration |
| `max_iterations` | 5 | Number of GA+XAI cycles |
| `problem_name` | `"Zakharov"` | Optimization problem |
| `K` | 3 | Top-K features to adjust bounds for |

## Usage

Update the CSV output paths in `genticalgorithm.py` and `XAI.py` to match your system, then:

```bash
python genticalgorithm.py
```

Plots are saved to `GA_XAI_plots/` and per-iteration CSVs log all solutions with fitness and constraint data.
