# Standard Prompt Templates — Low-Conversion

The Task templates are shared across regimes; differences mainly appear in History feedback and Strategy guidance due to sparsity and scale shifts. Due to extreme reward sparsity in the low-conversion subset, we render feedback using event-based conversion indicators instead of short-term CVR trend descriptors.

Placeholders follow Python-style formatting (e.g., `{cpa:.1f}`) used in our preprocessing scripts.

---

## Task Description (4 variants)

1. Balance conversion volume and cost efficiency with target CPA {cpa:.1f}.
2. Maximize conversions while maintaining CPA below {cpa:.1f}.
3. Optimize bidding to achieve target CPA of {cpa:.1f}.
4. Control cost per acquisition to stay within {cpa:.1f}.

---

## History (7 categories × 5 variants)

### ROI Low
- The ROI was low after the last bid; Previous bid resulted in low return on investment; Last action led to poor ROI performance; The bid was not profitable; ROI dropped below expectations.

### ROI Moderate
- The ROI was moderate after the last bid; Previous bid achieved acceptable ROI; Last action resulted in moderate returns; The bid showed moderate profitability; ROI was within acceptable range.

### ROI Good
- The ROI was good after the last bid; Previous bid achieved high return on investment; Last action resulted in excellent ROI; The bid was highly profitable; ROI exceeded expectations.

### Conversion Happened
- A conversion happened after the bid; Conversion was observed following the action; The bid led to a conversion; Conversion occurred after this bid; A conversion was achieved.

### No Conversion
- No conversion was observed after the bid; No conversion happened following the action; The bid did not lead to conversion; No conversion was recorded; Conversion did not occur this time.

### CPA Increase
- Cost per acquisition increased; CPA rose after the bid; Acquisition cost went up; CPA showed upward trend; Cost efficiency decreased.

### CPA Decrease
- Cost per acquisition decreased; CPA dropped after the bid; Acquisition cost went down; CPA showed downward trend; Cost efficiency improved.

---

## Strategy (9 categories × 5 variants)

### Conservative
- Consider bidding conservatively to maintain budget; Use a conservative bid to preserve resources; A moderate bid would be safer now; Keep the bid conservative to avoid overspending; Maintain conservative bidding strategy.

### Moderate
- A moderate bid would be appropriate; Consider a balanced bidding approach; Use a moderate bid to balance risk and reward; Moderate bid would help maintain stability; Try a moderate bidding strategy.

### Aggressive
- Consider increasing the bid to capture more opportunities; An aggressive bid might capture better conversions; Raise the bid to compete more effectively; Higher bid could improve win rate; Consider bidding more aggressively.

### High pValue
- High pValue suggests good conversion potential. Consider higher bid; The pValue is favorable, you can bid higher; Strong pValue indicates good opportunity; High pValue means better conversion probability; Favorable pValue supports higher bidding.

### Mid pValue
- Mid pValue suggests stable conversion potential; The pValue is moderate, keep bids balanced; Average pValue supports a steady bid; pValue is moderate, avoid extremes; Moderate pValue indicates cautious optimism.

### Low pValue
- Low pValue suggests lower conversion potential. Consider conservative bid; The pValue is low, bid conservatively; Weak pValue indicates limited opportunity; Low pValue means lower conversion probability; Unfavorable pValue suggests lower bidding.

### Budget Low
- Remaining budget is low. Bid conservatively; Limited budget remaining, use cautious bidding; Low budget requires careful bid management; Conserve budget with moderate bids; Budget constraint suggests conservative approach.

### Budget Mid
- Budget is moderate; keep bids balanced; Adequate budget supports steady bidding; Budget level allows for moderate bids; Maintain budget discipline with balanced bids; Moderate budget supports stable bidding.

### Budget High
- Remaining budget is sufficient. You can bid more aggressively; Adequate budget allows for higher bids; Good budget availability supports competitive bidding; Sufficient budget enables flexible bidding strategy; Budget is healthy, consider more aggressive bids.
