# Supplementary Results

This document contains additional experimental results and implementation details referenced in the appendix of the main paper.


## Additional Appendix Tables

The camera-ready appendix keeps only compact summaries in the main PDF. The full numerical tables below provide the detailed evidence referenced by the appendix text.

### Embedding Source Ablation

This ablation separates aligned language semantics from additional model capacity, categorical prompt identity, and incidental embedding geometry. All results are measured at 100% budget.

| Method | AuctionNet-High | AuctionNet-Medium | AuctionNet-Low |
|---|---:|---:|---:|
| DT (Baseline) | 356.14 | 263.50 | 30.00 |
| MLP Numeric | 340.25 | 256.05 | 26.42 |
| One-hot | 352.30 | 265.16 | 25.78 |
| Shuffled LLM | 358.95 | 270.16 | 27.87 |
| Random Init | 365.37 | 275.00 | 28.37 |
| **SemBid** | **412.20** | **310.19** | **32.00** |

SemBid consistently outperforms numerical, one-hot, shuffled, and randomly initialized alternatives, supporting the claim that aligned language semantics drive the gains.

### CQL with Concatenated Semantic Features

To test whether semantics help a non-transformer offline RL baseline, we concatenate the same LLM-encoded semantic embeddings (Task, History, Strategy) to CQL's state vector while keeping other CQL hyperparameters unchanged.

| Budget | CQL | CQL+Semantics | SemBid |
|---|---:|---:|---:|
| 50% | 221.89 | 228.56 | **238.20** |
| 75% | 314.97 | 316.84 | **320.60** |
| 100% | 377.79 | 386.42 | **412.20** |
| 125% | 425.77 | 437.15 | **474.87** |
| 150% | 492.79 | 505.31 | **541.76** |

Concatenated semantic features improve CQL over most budget settings but remain below SemBid, indicating that simple feature-level fusion is insufficient compared with token-level semantic integration.

### Deployment Latency

Semantic embeddings are precomputed and retrieved by lookup during deployment. Latency is measured on a single NVIDIA A100 GPU.

| Model | Mean | P95 | P99 |
|---|---:|---:|---:|
| DT | 7.48 ms | 15.81 ms | 18.41 ms |
| **SemBid** | **8.99 ms** | **19.30 ms** | **25.00 ms** |
| GAS | 9.36 ms | 19.68 ms | 25.35 ms |

### Dataset Statistics

| Params | Low conversion | Medium conversion | High conversion |
|---|---:|---:|---:|
| Trajectories | 17,773 | 17,257 | 17,257 |
| Delivery periods | 8,371 | 8,253 | 8,253 |
| Time steps in a trajectory | 48 | 48 | 48 |
| State dimension | 16 | 16 | 16 |
| Action dimension | 1 | 1 | 1 |
| Return-to-go dimension | 1 | 1 | 1 |
| Action range | [0.0, 2960.53] | [0.0, 145.47] | [0.0, 145.47] |
| Impression value range | [0, 1] | [0, 1] | [0, 1] |
| CPA range | [60.0, 130.0] | [15.0, 50.0] | [6.0, 12.0] |
| Total conversion range | [0.0, 96.0] | [0, 907.5368] | [0.0, 1475.0] |

## Prompt Complementarity

CCA shows low overall redundancy across prompt categories (average CCA = 0.123), indicating that Task, History, and Strategy capture distinct decision factors. In linear probing, combining all three prompt categories reaches R² = 0.204, a +52.3% gain over the best single category. These results support compositional semantic injection rather than collapsing all semantic information into a single prompt. Detailed preliminary-study tables are available in `supplementary/preliminary_study.md`.

## Template Generalization

To assess whether SemBid's performance depends on dataset-specific template engineering, we compare using separate templates per conversion regime against a single unified template where constraint thresholds are normalized to a common range.

| Configuration | Score |
|---|---|
| Separate templates (full SemBid) | **412.20** |
| Unified template | 396.45 |
| Vanilla DT | 356.14 |

**Setup:** AuctionNet-High, 100% budget.

The unified template still outperforms DT by +11.4%, confirming that semantic information---not template engineering---is the primary driver. The gap between separate and unified templates (412.20 vs. 396.45, -3.8%) reflects the benefit of regime-specific prompt calibration, but the core gain over DT is preserved even without it.

## Task Token Injection Strategy

We compare three Task injection strategies: injecting the full embedding at every step (current design), injecting only at the first step, and replacing Task with a zero vector.

| Task Injection Mode | Score |
|---|---|
| Full per-step (current) | **412.20** |
| First step only | 395.31 |
| No Task | 387.52 |

**Setup:** AuctionNet-High, 100% budget.

Per-step injection keeps the Task--State distance short and constant, enabling strong local attention. First-step-only injection causes the distance to grow with t, reducing Task's influence on later decisions.

## Penalty Sensitivity

Following the evaluation protocol, we sweep the CPA penalty exponent β ∈ {0.5, 1.0, 1.5, 2.0, 2.5, 3.0, 4.0}. SemBid remains the best-performing method under all settings with only ±1.3% score variation, confirming that its advantage is not tied to a specific penalty choice.

## Environment Characterization

**High Conversion** features the highest CVR (11.21%) with the strictest CPA constraints (Target CPA ≈ 8.27, compliance rate 29.17%), making constraint satisfaction the primary challenge.

**Medium Conversion** has moderate CVR (3.85%) with significant historical over-spending (Budget Usage ≈ 123.56%), testing budget pacing.

**Low Conversion** is a sparse-reward scenario (CVR ≈ 1.45%, 3 advertisers) with a large action-space shift (μ_a ≈ 81.20 vs. ≈ 8.0 in other scenarios), challenging exploration and generalization.

## State Feature Engineering

The numerical state s_t ∈ R^16 consists of four logical groups. All features are z-score normalized using statistics computed from the training set.

1. **Temporal (2-dim):** Time remaining ratio, Time elapsed.
2. **Budget (4-dim):** Budget remaining ratio, Consumption rate, Budget Pacing, Exhaustion risk.
3. **Performance (4-dim):** Current CPA, Target CPA, CPA violation degree, Historical CVR.
4. **History (4-dim):** Avg. bid (last 3 steps), Win rate (last 3 steps), Avg. cost (last 3 steps), Bid volatility.

## Prompt Template Details

Each semantic signal is generated via rule-based template verbalization with stochastic template sampling (one template per group is randomly selected at each step to prevent memorization).

### Task Signal

Encodes campaign objective and CPA constraint. Four template variants are used, e.g.:
- "Maximize conversions while maintaining CPA below {cpa}."
- "Goal: achieve maximum conversion volume under CPA ≤ {cpa} constraint."

### History Signal

Discretizes three metrics into qualitative descriptions:

- **ROI:** ROI ≈ (ω · ΔConv - ΔCost) / (ΔCost + ε), mapped to Low (<0.5), Moderate (0.5–1.5), Good (≥1.5).
- **CVR trend:** ΔCVR > 0.001 → "increased"; < -0.001 → "decreased"; otherwise "stable."
- **CPA:** |ΔCPA| > τ_cpa → "rose" or "dropped"; otherwise "stable."

### Strategy Signal

Composite heuristic from three factors:

- **Value:** p > 0.01 → high opportunity; p < 0.001 → low opportunity; else moderate.
- **Budget:** R_b < 0.2 → scarcity; R_b > 0.7 → abundance; else moderate.
- **Bid magnitude:** b_t > 50 → aggressive; b_t < 10 → conservative; else moderate.

Thresholds above are for the high-conversion setting; low-conversion uses scale-adjusted thresholds.

### Template Variants by Style

SemBid uses three groups of template-based prompts:
- Task: 4 variants
- History: ~7 categories × 5 variants
- Strategy: ~7–9 categories × 5 variants

Prompt styles evaluated: Standard, Directive, Concise, Verbose, Structured.

## Implementation Details

All methods are trained for 800k steps under the same evaluation protocol.

**SemBid architecture:**
- 6-layer causal Transformer (4 attention heads, hidden dimension 128)
- Frozen Qwen2.5-0.5B-Instruct semantic encoder projected to 2048-d via a random linear layer
- SemBid parameter count: 2.8M

**Checkpoint and evaluation:**
- Checkpoints saved every 10k steps
- Coarse evaluation runs every 100k steps (5 seeds)
- Fine scanning within ±100k of the best coarse point

**Hardware:** All experiments run on a single NVIDIA A100 (40GB) GPU.

**Baseline configurations** follow the original papers' recommended hyperparameters. Full configuration details are available in `code/Algorithms/` and `code/Training/`.
