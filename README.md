# econ3916-lab11-sql
# SQL for Economists — Joins, Windows and the Cardinality Trap

## Objective
I translated my pandas skills into SQL with DuckDB, and tested the places where SQL quietly gives different answers than you'd expect.

## Methodology
- I translated pandas operations (select, filter, sort, groupby, merge) into SQL with DuckDB, and checked that both give the same rows.
- I joined customers to their orders and compared a Welch t-test before and after the join.
- I used window functions (AVG OVER PARTITION BY, RANK) to compute group averages and ranks without collapsing rows.
- I tested how SQL handles missing values in a WHERE filter, compared with pandas.
- I queried a public Parquet file of about 3 million NYC taxi trips over HTTP without downloading it, reading only the columns the query needed.
- I asked an AI to translate three of my pandas steps into SQL, revised my prompt once, and compared the row counts of its queries with my pandas results.

## Key Findings
- A one-to-many LEFT JOIN turned 600 customer rows into 3,612 rows. The same Welch t-test on the same revenue figures went from t = 0.07 (p = 0.9413) to t = 0.71 (p = 0.4749), because the repeated rows shrank the standard error and moved the difference.
- WHERE income != 50000 kept 3 of 5 survey rows, because SQL treats a comparison with a missing value as unknown and drops the row, while the same filter in pandas kept 5.
- Window functions let me compare each customer with their region's average while keeping all 600 rows.
- The AI's SQL matched all three of my pandas row counts (2, 3,612 and 5), but only because it chose on its own to keep missing incomes. My revised prompt stated that choice explicitly instead of leaving it to the AI.
