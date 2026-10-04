BrewMetrics BI – Copilot DAX Development Notes

Overview

This file documents the development of the DAX measures used in the BrewMetrics BI project. Copilot/AI assistance was used to generate and refine DAX expressions, while the final measures were reviewed against the project data model and business requirements.

The main model contains:

Fact_Sales

Dim_Date

Dim_City

Dim_Product

1. Total Sales – Supporting Measure

Purpose

To calculate the total sales amount from the transaction-level Fact_Sales table.

DAX

Total Sales =
SUM(Fact_Sales[sales_amount])

Copilot suggestion

Copilot suggested using the SUM() function on the sales_amount column to create a reusable total-sales measure.

Changes made

The measure was reviewed and kept as a simple SUM() calculation because sales_amount is the transaction sales value.

Reason

This measure is used as a base measure for the other DAX calculations and dashboard visuals.

2. Month-over-Month Sales Growth %

Purpose

To calculate the percentage change in sales compared with the previous month.

DAX

MoM Sales Growth % =
VAR CurrentSales = [Total Sales]
VAR PreviousSales =
    CALCULATE(
        [Total Sales],
        DATEADD(Dim_Date[date], -1, MONTH)
    )
RETURN
    DIVIDE(CurrentSales - PreviousSales, PreviousSales)

Copilot suggestion

Copilot suggested using CALCULATE() with DATEADD() to retrieve the previous month's sales and then calculating the percentage change.

Changes made

The calculation was structured using variables for current sales and previous sales. DIVIDE() was used instead of direct division to handle cases where the previous-month value is zero or blank.

Reason

The assignment requires a month-over-month or year-over-year growth measure, and this measure provides a clear month-to-month sales comparison.

3. Running Total Sales

Purpose

To calculate cumulative sales over the selected date period.

DAX

Running Total Sales =
CALCULATE(
    [Total Sales],
    FILTER(
        ALLSELECTED(Dim_Date[date]),
        Dim_Date[date] <= MAX(Dim_Date[date])
    )
)

Copilot suggestion

Copilot suggested using CALCULATE() together with a filtered date context to accumulate sales up to the current date.

Changes made

ALLSELECTED() was used so that the running total respects the current report selections while accumulating sales through the selected dates.

Reason

The measure is useful for showing cumulative sales progression in the dashboard.

4. City Sales Rank – RANKX

Purpose

To rank cities according to their total sales.

DAX

City Sales Rank =
RANKX(
    ALL(Dim_City[city]),
    [Total Sales],
    ,
    DESC,
    DENSE
)

Copilot suggestion

Copilot suggested using RANKX() with the city field and the total-sales measure.

Changes made

The ranking was configured in descending order so that the city with the highest sales receives rank 1. DENSE ranking was selected so that tied values do not create gaps in the ranking sequence.

Reason

The assignment requires a RANKX measure and the city ranking directly supports the required city-level performance analysis.

5. Average Transaction Value

Purpose

To calculate the average sales amount per transaction.

DAX

Average Transaction Value =
DIVIDE(
    [Total Sales],
    DISTINCTCOUNT(Fact_Sales[sale_id])
)

Copilot suggestion

Copilot suggested calculating average transaction value by dividing total sales by the number of distinct transactions.

Changes made

DISTINCTCOUNT() was used on sale_id so that each transaction is counted once. DIVIDE() was used to avoid errors when the transaction count is zero.

Reason

This measure provides an additional management-level metric for understanding the average value generated per transaction.

