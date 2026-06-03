Total VAT = SUM(Sales[VAT])

Total Gross Revenue = SUM(Sales[GrossRevenue])

Total Orders = COUNTROWS(Sales)

Total Customers = DISTINCTCOUNT(Sales[CustomerID])

Total Products Sold = DISTINCTCOUNT(Sales[ProductID])

Avg Order Value = DIVIDE([Total Net Revenue], [Total Orders], 0)

Total Cost = SUMX(Sales, Sales[Quantity] * RELATED(Products[CostPrice]))

Total Profit = [Total Net Revenue] - [Total Cost]

Profit Margin % = DIVIDE([Total Profit], [Total Net Revenue], 0)

Electronics Revenue =
CALCULATE(
    [Total Net Revenue],
    Products[Category] = "Electronics"
)

Enterprise Revenue =
CALCULATE(
    [Total Net Revenue],
    Customers[Segment] = "Enterprise"
)

Glasgow Revenue =
CALCULATE(
    [Total Net Revenue],
    Customers[City] = "Glasgow"
)

Revenue % of Total =
DIVIDE(
    [Total Net Revenue],
    CALCULATE([Total Net Revenue], ALL(Sales)),
    0
)

Revenue Last Year =
CALCULATE(
    [Total Net Revenue],
    SAMEPERIODLASTYEAR('Date'[Date])
)

Revenue YoY % =
DIVIDE(
    [Revenue YoY Change],
    [Revenue Last Year],
    0
)
