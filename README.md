# Retail Sales Analytics Dashboard - NOT FINISHED

An interactive Power BI dashboard analyzing ~540,000 transaction records from the UCI Online Retail dataset, built on a custom star schema with DAX-driven KPIs for executive-level sales reporting.

🔗 **[View Live Dashboard](https://app.powerbi.com/view?r=eyJrIjoiYjcwNGYzZGYtNzY0Yi00MzBlLThiYzYtNmI3MzM4NmU1ZjJjIiwidCI6IjUxY2NhMGUxLTJkNWEtNGQxYi1hYTlhLWRkYWFhNzhhZWVjMiJ9)**

![Dashboard Screenshot](PutScreenShot.png)

## Overview

This dashboard turns raw transactional retail data into an executive-facing report covering revenue trends, customer behavior, product performance, and geographic breakdown. The focus was on building a clean, scalable data model first, then layering analysis and design on top of it.

## Data Model

Built as a star schema rather than a flat table, to keep the model performant and the DAX maintainable:

- **Fact table:** Sales (transaction-level detail)
- **Dimension tables:** Customers, Products, Calendar

## Key Metrics (DAX)

A few of the core measures powering the report:

```dax
Total Revenue = SUMX(Sales, Sales[Quantity] * Sales[UnitPrice])

Repeat Customer Rate =
VAR CustomersWithMultipleOrders =
    CALCULATE(
        DISTINCTCOUNT(Sales[CustomerID]),
        FILTER(
            VALUES(Sales[CustomerID]),
            CALCULATE(DISTINCTCOUNT(Sales[InvoiceNo])) > 1
        )
    )
VAR TotalCustomers = DISTINCTCOUNT(Sales[CustomerID])
RETURN DIVIDE(CustomersWithMultipleOrders, TotalCustomers)
```

Other measures include Total Orders, Average Order Value, Spend Bucket segmentation, and a Product Margin Flag.

## Report Pages

| Page | Focus |
|---|---|
| Executive Overview | Top-line KPIs, revenue trend, revenue by country, top products |
| Customer Analysis | Customer segmentation and repeat purchase behavior |
| Product Analysis | Product-level revenue and margin performance |
| Geographic Analysis | Country and regional sales breakdown |

## Tools Used

Power BI Desktop, DAX, Power Query (data cleaning and transformation)

## Data Source

[UCI Machine Learning Repository — Online Retail Dataset](https://archive.ics.uci.edu/dataset/352/online+retail)
