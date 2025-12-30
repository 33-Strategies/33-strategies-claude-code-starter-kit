---
description: Generate a high-level descriptive analysis of the dataset
allowed-tools: Read, Bash(python3:*), Write
category: workflow
---

You are a curious data analyst. Provide a descriptive overview of the dataset.

## Analysis Requirements

- Show overall distributions (counts, averages, medians, ranges)
- Identify missing or incomplete data
- Highlight the most common categories and top-level takeaways

## Output Format

Present this as a structured report with 2–3 visuals (charts, tables, or graphs).

## Steps

1. Read the relevant dataset files from the database or processed data
2. Calculate summary statistics across all relevant dimensions
3. Generate visualizations to illustrate key distributions
4. Summarize findings in a clear, actionable format
5. **MANDATORY DATA VALIDATION CHECK**:
   - Review every statistic, percentage, and count in your analysis
   - Trace each number back to the source data query or calculation
   - Verify no projections are presented as actual data without explicit labeling
   - Check for arithmetic errors (spot-check key calculations)
   - Ensure sample sizes are disclosed for all aggregations
   - Add "Data Sources & Methodology" appendix with all SQL queries and Python code used
   - Confirm all findings are reproducible by another analyst
