# Prompt Templates for Semantic Signals

SemBid uses three groups of template-based prompts: **Task** (4 variants), **History** (~7 categories × 5 variants), and **Strategy** (~7–9 categories × 5 variants). At each timestep, one template per group is randomly sampled to prevent memorization.

The medium-conversion setting reuses the high-conversion pool.

## Standard Templates (used in all experiments)

- [High-Conversion Templates](standard_high.md) — 7 History + 7 Strategy categories
- [Low-Conversion Templates](standard_low.md) — 7 History + 9 Strategy categories (event-based conversion indicators)

## Prompt Variants (used only in the high-conversion prompt-variant study, RQ3)

- [Directive Style](directive.md) — Imperative commands with minimal explanation
- [Concise Style](concise.md) — Minimal tokens with essential information only
- [Verbose Style](verbose.md) — Detailed explanations with full context
- [Structured Style](structured.md) — Label-value pairs with explicit field names

## Template Placeholders

| Placeholder | Description |
|---|---|
| `{cpa:.1f}` | Target CPA value (e.g., 8.3) |
| `{pvalue}` | Impression pValue / conversion probability |
| `{budget}` | Budget remaining ratio |
| `{spent}` | Budget utilization ratio |
