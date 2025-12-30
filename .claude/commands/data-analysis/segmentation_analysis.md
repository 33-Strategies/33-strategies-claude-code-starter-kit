---
description: Segment the dataset into meaningful categories to surface patterns
allowed-tools: Read, Bash(python3:*), Write
category: workflow
---

Analyze this dataset by segmenting across relevant dimensions (e.g., grade, year, title, publisher, price range, seller).

## Analysis Requirements

- Highlight key differences in volume, pricing, and velocity across segments
- Identify outliers or surprising contrasts
- Suggest 2–3 hypotheses about why certain segments behave differently

## Output Format

Present with charts/tables that clearly show the differences between segments.

## Steps

1. Identify the most meaningful segmentation dimensions in the dataset
2. Calculate metrics for each segment (volume, average price, velocity, etc.)
3. Compare segments to identify patterns and anomalies
4. Generate hypotheses explaining the observed differences
5. Create visualizations that clearly illustrate segment contrasts
6. **MANDATORY DATA VALIDATION CHECK**:
   - Audit every segment count, average, median, and percentage
   - Trace each metric back to the underlying SQL query or aggregation code
   - Verify segment definitions are clear and non-overlapping
   - Check that segment totals sum to overall dataset totals (no double-counting)
   - Disclose any segments with small sample sizes (n < 30) as potentially unreliable
   - Flag any projections or estimates with explicit labels and methodology
   - Add "Data Sources & Methodology" section documenting all calculations
   - Confirm another analyst could reproduce all segment metrics
