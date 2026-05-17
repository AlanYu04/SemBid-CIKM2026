# Penalty Sensitivity Analysis

The penalty function follows the established standard in prior work: `penalty = (CPA_constraint / CPA)^β`, where β = 2, as used in GAS and GAVE. To verify that SemBid's advantage does not depend on this specific choice, we conducted a sensitivity analysis sweeping β across {0.5, 1.0, 1.5, 2.0, 2.5, 3.0, 4.0}.

## Full Results (AuctionNet-High, 100% budget)

| Method | β=0.5 | β=1.0 | β=1.5 | **β=2.0** | β=2.5 | β=3.0 | β=4.0 |
|---|---|---|---|---|---|---|---|
| **SemBid** | 416.92 | 415.28 | 413.71 | **412.20** | 410.75 | 409.37 | 406.76 |
| GAS | 408.33 | 401.75 | 395.69 | 390.10 | 384.95 | 380.19 | 371.77 |
| CQL | 401.77 | 392.97 | 385.01 | 377.79 | 371.26 | 365.34 | 355.13 |
| DT | 384.36 | 373.88 | 364.52 | 356.14 | 348.65 | 341.95 | 330.59 |

## Key Observations

1. **SemBid is robust to β**: Score varies only ±1.3% across the entire range (417→407), because SemBid controls CPA well and most advertisers rarely trigger the penalty.

2. **SemBid outperforms all baselines at every β value**: The ranking SemBid > GAS > CQL > DT remains consistent regardless of β, confirming that our performance gain stems from better bidding strategy rather than a specific penalty configuration.

3. **β mainly penalizes models with poor CPA control**: DT shows ±7.9% variation while SemBid shows only ±1.3%, indicating that models with better constraint satisfaction are inherently less sensitive to the penalty formulation.
