# Custom Instructions Template (Power BI)

> Copy this file to `.github/copilot-instructions.md` and customise it for your Power BI project.
> Delete sections that don't apply. Add your own rules.

(If no .github folder exists in the repo, then you can manually create it)

---

## Project Context

This is a Power BI semantic model and reporting project for sales analysis. It provides interactive dashboards and KPIs for business users to track revenue, customer trends, and product performance.

---

## Technologies

- **Power BI Desktop** (latest version)
- **DAX** for measures and calculated columns
- **Power Query (M)** for data transformation
- **Tabular Model** (Import mode)

---

## DAX Conventions

### Error Handling
- Always use `DIVIDE()` instead of `/` operator to avoid divide-by-zero errors
- Use `COALESCE()` or `ISBLANK()` for null handling
- Provide meaningful default values (0 for numbers, BLANK() for comparisons)

### Functions
- Use `SUMX/AVERAGEX/COUNTX` for row-by-row calculations
- Use `SUM/AVERAGE/COUNT` only for pre-aggregated columns
- Prefer `SELECTEDVALUE()` over `VALUES()` when expecting a single value
- Use `CALCULATE()` to modify filter context explicitly

### Code Style
- Use `VAR` to store intermediate calculations and improve readability
- Write multi-line DAX with proper indentation (4 spaces)
- Add blank lines between logical sections
- Include comments for complex logic
- Always include a /// description comment above each measure explaining the business purpose

### Formatting
- Currency: `"$#,##0.00"`
- Percentage: `"0.0%"` or `"0.00%"`
- Whole numbers: `"#,##0"`
- Dates: Use `FORMAT()` with appropriate format strings

---

## Measure Naming Conventions

- **Aggregations**: Prefix with "Total" (e.g., `Total Revenue`, `Total Quantity`)
- **Time intelligence**: Prefix with period (e.g., `YTD Revenue`, `MTD Sales`)
- **Percentages**: Prefix with "%" (e.g., `% of Total`, `% Growth`)
- **Ratios**: Use "per" (e.g., `Revenue per Customer`)
- **Counts**: Prefix with "Count" (e.g., `Count Orders`, `Distinct Customers`)
- Use full words, avoid abbreviations (except industry-standard ones like YTD, MTD, QTD)

---

## Measure Organization

- **Base measures**: Keep at table root (Total Revenue, Total Quantity, Count Orders)
- **Display folders**: Group related measures
  - `Time Intelligence` — YTD, MTD, SAMEPERIODLASTYEAR, etc.
  - `KPIs` — Business metrics and targets
  - `% Metrics` — Percentage calculations
  - `Ratios` — Per-customer, per-order, etc.
- Add descriptions to all measures explaining their business purpose
- Add a formatting rule: "Percentage measures must use formatString: 0.0%"
- Always include a /// description comment above each measure explaining the business purpose
- Assign percentage/KPI measures to displayFolder: KPIs

---

## Power Query (M) Conventions

- Use meaningful query names (not "Table1", "Query2")
- Remove unused columns early in transformation steps
- Use `Table.TransformColumnTypes` to set correct data types
- Prefer native M functions over custom code when possible
- Document complex transformations with comments

---

## What NOT to Do

- Do not use calculated columns for aggregations — use measures instead
- Do not use bidirectional relationships unless absolutely necessary
- Do not create many-to-many relationships without a bridge table
- Do not use `RELATED()` when `CALCULATE()` with filter is clearer
- Avoid using `ALL()` without understanding filter context implications

---

## Data Model Best Practices

- Follow star schema design (fact tables + dimension tables)
- Use integer keys for relationships when possible
- Mark date tables with "Mark as Date Table"
- Hide technical columns (keys, row numbers) from report view
- Use consistent data types across related columns

---

## Testing & Validation

- Always test measures with different filter combinations
- Verify time intelligence measures work correctly across year boundaries
- Check division measures with zero denominators
- Test with blank/null values in filter contexts
- Compare results with source data for accuracy

---

## Domain Terminology

- **Revenue**: `quantity × unit_price`
- **Order Value**: Total revenue for a single `order_id`
- **Customer Lifetime Value**: Total revenue per `customer_id`
- **Average Order Value**: `Total Revenue ÷ Distinct Count of Orders`

---

## Comments in DAX

- Add a comment line above each measure explaining its business purpose
- Use inline comments (`--`) for complex logic within measures
- Document any non-obvious filter context modifications

Example:
```dax
-- Calculate total revenue for active customers only
[Active Customer Revenue] = 
CALCULATE(
    [Total Revenue],
    sales_sample[customer_status] = "Active"  -- Filter to active customers
)
```
