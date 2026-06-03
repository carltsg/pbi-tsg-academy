##Exersise 3.2: Create Your First Measures##

Total VAT = SUM(Sales[VAT])

Total Gross Revenue = SUM(Sales[GrossRevenue])

##Exersise 3.3: Create Count Measures##

Total Orders = COUNTROWS(Sales)

Total Customers = DISTINCTCOUNT(Sales[CustomerID])

Total Products Sold = DISTINCTCOUNT(Sales[ProductID])

##Exersise 3.4: Create Average and Margin Measures##

Avg Order Value = DIVIDE([Total Net Revenue], [Total Orders], 0)

Total Cost = SUMX(Sales, Sales[Quantity] * RELATED(Products[CostPrice]))

Total Profit = [Total Net Revenue] - [Total Cost]

Profit Margin % = DIVIDE([Total Profit], [Total Net Revenue], 0)

##Exersise 3.5: Use CALCULATE for Filtered Measures##

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

##Exersise 3.6: Create a Percentage Measure##

Revenue % of Total =
DIVIDE(
    [Total Net Revenue],
    CALCULATE([Total Net Revenue], ALL(Sales)),
    0
)

##Exercise 3.7: Year-over-Year Comparison##

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
