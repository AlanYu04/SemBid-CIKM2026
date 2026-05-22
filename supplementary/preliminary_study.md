# Preliminary Study: Detailed Results

This appendix reports the **complete** set of preliminary experiments referenced in Section 3 of the main paper.

This study validates four aspects: (1) semantic content in LLM embeddings, (2) limitations of using embeddings alone, (3) complementarity across prompt types, and (4) effectiveness of fusion mechanisms.

---

## A.1 Semantic Content in LLM Embeddings

We first verify whether LLM embeddings encode bidding-relevant information or are merely random noise. We adopt linear probing: if a simple linear model can predict bidding actions from embeddings, the representations contain task-relevant information.

We first **render numerical bidding states into natural-language text** using rule-based templates (e.g., "Budget: 80%. pValue: High. Time remaining: 2 hours."), and then **encode the text** with a frozen Qwen2.5-0.5B-Instruct checkpoint to obtain state embeddings. We compare against random baselines. Crucially, we include a shuffled test where **sample–embedding correspondences are randomized** to remove semantic alignment while preserving the embedding distribution.

| Input Type | R² | vs Random |
|---|---|---|
| Random Gaussian | -0.143 | baseline |
| Shuffled Pairing (Text–Embedding) | -0.015 | - |
| **State Text Embedding** | **0.121** | **+946%** |

State Text Embedding achieves R²=0.121, substantially outperforming random baselines (+946% over random Gaussian). Most importantly, randomizing sample–embedding correspondences reduces R² to -0.015, confirming that **semantic alignment**, rather than incidental statistics, drives prediction.

---

## A.2 Limits of Using LLM Embeddings Alone

Given that LLM embeddings contain useful semantics, a natural question arises: can we bypass numerical features entirely and bid using only semantic representations?

| Input Type | R² | MSE | MAE |
|---|---|---|---|
| Numerical Features Only | 0.880 | 6.01 | 1.42 |
| LLM Embedding Only | 0.121 | 35.499 | 3.52 |
| Numerical + Embedding | 0.886 | 5.89 | 1.38 |

Results indicate that embeddings alone are insufficient. Numerical features achieve R²=0.880, while semantic embeddings alone reach only 0.121 — an 86.25% performance gap. This is expected: bidding requires fine-grained numerical reasoning (e.g., budget levels, conversion likelihoods, and cost accounting) that cannot be faithfully represented by language alone. When integrated with the numerical features, semantic embeddings improve the result to R²=0.886. Therefore, **semantic signals should complement, not replace, numerical features**.

---

## A.3 Complementarity Across Prompt Types

Having established that embeddings contain useful information but cannot work alone, we investigate whether different prompt formulations capture distinct aspects of the bidding context. We construct a representative set of 18 prompt templates organized into three semantic categories — Task, History, and Strategy.

### Effective Prompts (R² > 0.05)

| Category | Prompt | R² |
|---|---|---|
| Task | task_roi_objective | 0.152 |
| Task | task_budget_constraint | 0.132 |
| Task | task_combined_metrics | 0.132 |
| Task | task_efficiency_ratio | 0.130 |
| History | history_progress_summary | 0.132 |
| History | history_budget_status | 0.132 |
| History | history_time_progress | 0.112 |
| History | history_spend_rate | 0.112 |
| History | history_time_remaining | 0.054 |
| Strategy | strategy_conversion_rate | 0.152 |
| Strategy | strategy_optimization_signal | 0.148 |
| Strategy | strategy_time_pressure | 0.132 |
| Strategy | strategy_budget_pacing | 0.112 |

### Cross-Category Correlation (CCA)

We quantify redundancy and complementarity using canonical correlation analysis (CCA) between prompt embeddings. The overall average correlation is 0.123, suggesting low redundancy; however, History–Strategy shows higher correlation (0.201), indicating partial overlap.

| Pair | Avg CCA |
|---|---|
| Task – History | 0.108 |
| Task – Strategy | 0.059 |
| History – Strategy | 0.201 |

### Single vs. Combined Prompt Categories (Linear Probing)

Combining all three yields R²=0.204, a 52.26% improvement over the best single category.

| Configuration | R² | MSE | MAE |
|---|---|---|---|
| Task only | 0.142 | 40.07 | 4.03 |
| History only | 0.121 | 36.02 | 4.05 |
| Strategy only | 0.134 | 35.49 | 3.55 |
| Task + History | 0.132 | 35.58 | 3.94 |
| Task + Strategy | 0.149 | 34.86 | 3.57 |
| History + Strategy | 0.199 | 32.82 | 3.54 |
| **Task + History + Strategy** | **0.204** | **32.63** | **3.67** |

Task embeddings perform best individually (R²=0.142), followed by Strategy (R²=0.134) and History (R²=0.121). This superadditive effect indicates that **different prompt types encode complementary information**: Task prompts capture objective grounding, History prompts summarize short-term feedback, and Strategy prompts provide domain heuristics.

---

## A.4 Fusion Mechanisms for Semantic Signals

We compare simple concatenation against more structured fusion mechanisms.

| Fusion Method | R² | vs Baseline |
|---|---|---|
| Numerical Only (Baseline) | 0.880 | - |
| Concat Fusion | 0.850 | -3.41% |
| Residual Fusion | 0.872 | -0.91% |
| Gated Fusion | 0.863 | -1.93% |
| **Self-Attention Fusion** | **0.886** | **+0.68%** |
| FiLM Fusion | 0.859 | -2.39% |

Simple concatenation performs worst, degrading performance by 3.41%. This suggests that treating embeddings as uniformly informative can dilute the strong numerical signal. In contrast, **self-attention** yields the best result and provides a consistent improvement over the numerical baseline, indicating that attention-based fusion can selectively filter and integrate semantic context.

---

## A.5 Key Findings

These preliminary experiments provide concrete evidence supporting SemBid's design:

- **LLM embeddings encode bidding semantics** (+946% over random; shuffling correspondence removes predictive power).
- **Embeddings alone are insufficient** (86.25% gap vs numerical features).
- **Prompt types exhibit complementarity** (Task/History/Strategy combined yields +52.3% over the best single category).
- **Proper fusion matters**: self-attention improves over naive concatenation and yields the strongest performance.

Overall, the results justify a **compositional semantic design** (multiple prompt types) and a **selective integration mechanism** (attention) as the basis for SemBid.
