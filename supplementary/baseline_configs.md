# Baseline Implementation Details

## Checkpointing and Model Selection

To ensure fair comparison, we followed official implementations or standard protocols for all baselines. We standardized checkpointing, model selection, and training horizons across methods.

We saved checkpoints every 10k steps and ran coarse evaluation every 100k steps (5 seeds). We then re-scanned checkpoints within ±100k of the best coarse point at 10k intervals (5 seeds), and reported the best averaged score.

## Convergence and Implementation Notes

Most baselines converge within 400k steps; however, DT-style sequence models (SemBid, DT, GAVE) can stabilize more slowly in a few runs. To avoid under-training and keep comparisons uniform, we trained all methods for 800k steps. GAS follows the official two-stage pipeline (policy then critic); GAVE uses a larger backbone for value guidance; PID adjusts gains dynamically based on the budget ratio.

## Training Configurations

| Method | Type | Max Steps | Batch | Key Configuration |
|---|---|---|---|---|
| **SemBid** (Ours) | DT+LLM | 800,000 | 64 | Emb Dim=2048, Random Proj |
| **GAS** | DT+Search | 800,000 | 64 | Two-stage (Policy+Critic), w=0.2 |
| **GAVE** | DT+Value | 800,000 | 64 | Large Arch (L=8, H=512), τ=0.99 |
| **DT** | Transformer | 800,000 | 64 | Vanilla, Score-based RTG |
| **TD3+BC** | Offline RL | 800,000 | 100 | Standard Implementation |
| **CQL** | Offline RL | 800,000 | 100 | Conservative α=5.0 |
| **BCQ** | Offline RL | 800,000 | 100 | Early Convergence |
| **IQL** | Offline RL | 800,000 | 100 | Implicit Q-Learning |
| **BC** | Imitation | 800,000 | 100 | Behavior Cloning |
