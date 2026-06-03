# Advanced DAX

This section demonstrates a set of reusable DAX patterns that move beyond basic aggregations into more realistic, production-ready analytics.

The focus is not just syntax, but:

- How measures behave under different filter contexts
- How to reuse logic effectively
- How to build flexible, user-driven reporting

---

## Time Intelligence: YTD and Comparison

### Revenue YTD

Calculates year-to-date revenue using the Date table.

Requires:

- A properly configured and marked Date table
- Current filter context to be respected, such as Year and slicers

```DAX
Revenue YTD =
TOTALYTD(
    [Total Net Revenue],
    'Date'[Date]
)
```

---

### Revenue YTD Last Year

Reuses the YTD logic and evaluates it in the previous year context.

Key principle: **always reuse measures instead of duplicating logic.**

```DAX
Revenue YTD Last Year =
CALCULATE(
    [Revenue YTD],
    SAMEPERIODLASTYEAR('Date'[Date])
)
```

---

### Revenue YTD Variance

Simple comparison between current and prior year.

```DAX
Revenue YTD Variance =
[Revenue YTD] - [Revenue YTD Last Year]
```

---

### Revenue YTD Variance %

Safe percentage calculation using `DIVIDE`.

```DAX
Revenue YTD Variance % =
DIVIDE(
    [Revenue YTD Variance],
    [Revenue YTD Last Year],
    0
)
```

---

## Ranking Pattern

### Product Revenue Rank

Ranks products dynamically by revenue.

- `ALL` removes product-level filtering to calculate a global rank
- Ranking still respects slicers, such as Year or Segment

```DAX
Product Revenue Rank =
RANKX(
    ALL(Products[ProductName]),
    [Total Net Revenue],
    ,
    DESC
)
```

Common use case:

- Top N analysis

---

## Disconnected Table Pattern: User-Driven Metrics

### Metric Selector Table

Creates a disconnected table used purely for user interaction.

```DAX
Metric Selector =
DATATABLE(
    "Metric", STRING,
    {
        {"Net Revenue"},
        {"VAT"},
        {"Profit Margin %"}
    }
)
```

---

### Selected Metric Value

Dynamically switches logic based on slicer selection.

```DAX
Selected Metric Value =
SWITCH(
    SELECTEDVALUE('Metric Selector'[Metric]),
    "Net Revenue", [Total Net Revenue],
    "VAT", [Total VAT],
    "Profit Margin %", [Profit Margin %],
    BLANK()
)
```

Why this matters:

- Reduces visual clutter
- Enables flexible dashboards
- Supports a single visual showing multiple metrics

---

## Percentage of Total Pattern

### Category Revenue %

Calculates each category's share of total revenue.

```DAX
Category Revenue % =
DIVIDE(
    [Total Net Revenue],
    CALCULATE(
        [Total Net Revenue],
        ALL(Products[Category])
    ),
    0
)
```

Key concept:

- Removes only the Category filter
- Retains all other filters, such as Year and Region

This is **selective filter removal**, not a full context reset.

---

## Target vs Actual Pattern

### Category Targets

Defines benchmark or target values.

```DAX
Category Targets =
DATATABLE(
    "Category", STRING,
    "TargetRevenue", CURRENCY,
    {
        {"Electronics", 250000},
        {"Furniture", 150000},
        {"Accessories", 80000},
        {"Office Supplies", 100000}
    }
)
```

---

### Target Revenue

Aggregates target values.

```DAX
Target Revenue =
SUM('Category Targets'[TargetRevenue])
```

---

### Revenue Variance to Target

Compares actuals against targets.

```DAX
Revenue Variance to Target =
[Total Net Revenue] - [Target Revenue]
```

---

### Revenue Variance to Target %

Calculates percentage variance.

```DAX
Revenue Variance to Target % =
DIVIDE(
    [Revenue Variance to Target],
    [Target Revenue],
    0
)
```

Common use cases:

- Budget vs Actual
- Forecast tracking
- Performance management

---

## Defensive DAX

### Revenue per Customer

Uses `DIVIDE` for safe arithmetic.

```DAX
Revenue per Customer =
DIVIDE(
    [Total Net Revenue],
    [Total Customers],
    0
)
```

---

### Revenue per Customer Display

Ensures no blank outputs in visuals.

```DAX
Revenue per Customer Display =
COALESCE(
    [Revenue per Customer],
    0
)
```

These patterns:

- Prevent errors
- Improve usability
- Ensure cleaner report output

---

## Key Takeaways

- Build base measures first, then layer logic
- Use `CALCULATE` to control filter context explicitly
- Avoid duplication — always reuse measures
- Use `DIVIDE` and `COALESCE` for safer DAX
- Apply patterns, not one-off formulas

Advanced DAX is not about writing complex code — it is about controlling context, reusing logic, and building models that scale.
