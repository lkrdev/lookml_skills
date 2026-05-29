# Common High-Quality Table Calculations

Professional LookML dashboards use table calculations to create context and solve common visualization hurdles. Use these patterns to enhance your tiles.

## 1. Partial Period Suppression
Prevents trend lines from "crashing" to zero at the end of the chart when the current day/month is incomplete.
```yaml
- table_calculation: trend_line
  label: Trend Line
  expression: "if(${created_date} <= now(), ${count}, null)"
```

## 2. Period-over-Period Variance
Calculates the difference from the previous data point (e.g., Prior Week).
```yaml
- table_calculation: pop_variance
  label: "vs Prior Period"
  expression: "${count} / offset(${count}, 1) - 1"
  value_format_name: percent_1
```

## 3. Return / Retention Rates
Calculates the percentage of users returning relative to a starting cohort (index 1).
```yaml
- table_calculation: return_rate
  label: Return Rate
  expression: "${user_count} / index(${user_count}, 1)"
  value_format_name: percent_1
```

## 4. Cumulative Growth vs Linear Goal
Visualizes progress toward a linear target across a period.
```yaml
# Cumulative Growth
- table_calculation: running_total
  expression: "running_total(${amount})"

# Linear Goal Line
- table_calculation: goal_line
  expression: "(row() / max(row())) * ${goal_amount}"
```

## 5. Relative Growth (Share of Wallet)
Compares a specific segment's performance against the total.
```yaml
- table_calculation: segment_share
  expression: "${segment_amount} / sum(${segment_amount})"
```
