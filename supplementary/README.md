# Supplementary Materials for SemBid

This directory contains the full appendix materials that accompany the CIKM 2026 paper:

> **On the Role of Language Representations in Auto-Bidding: Findings and Implications**

Due to page limits, the published appendix includes only essential tables and summaries. The detailed results below are provided as supplementary material.

---

## Contents

### [Preliminary Study: Detailed Results](preliminary_study.md)
Full tables and analysis for the four preliminary experiments:
- A.1 Semantic Content in LLM Embeddings
- A.2 Limits of Using LLM Embeddings Alone
- A.3 Complementarity Across Prompt Types (3 tables)
- A.4 Fusion Mechanisms for Semantic Signals
- A.5 Key Findings Summary

### [Hyperparameters](hyperparameters.md)
Complete model architecture, semantic encoder, and training configurations.

### [Baseline Configurations](baseline_configs.md)
Training configurations for all 9 baseline methods, including checkpoint selection protocol.

### [Penalty Sensitivity Analysis](penalty_sensitivity.md)
Full results sweeping β ∈ {0.5, 1.0, ..., 4.0} across SemBid, GAS, CQL, and DT.

### [Prompt Templates](prompt_templates/)
Complete prompt template pools for all styles and conversion regimes:
- [Standard (High-conversion)](prompt_templates/standard_high.md)
- [Standard (Low-conversion)](prompt_templates/standard_low.md)
- [Directive style](prompt_templates/directive.md)
- [Concise style](prompt_templates/concise.md)
- [Verbose style](prompt_templates/verbose.md)
- [Structured style](prompt_templates/structured.md)
