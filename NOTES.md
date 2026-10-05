# BrewMetrics - Copilot DAX Development Notes

This file documents the development of the DAX measures used in the BrewMetrics Business Intelligence project. GitHub Copilot was used as an assistant during the DAX development process. The suggestions were reviewed and tested against the actual Power BI data model before being used in the report.

The project follows a star-schema structure containing the following main tables:

- Fact_Sales
- Dim_Date
- Dim_City
- Dim_Product

The DAX measures were developed to analyse overall sales, monthly changes, cumulative performance, and city-level performance.

---

## 1. Total Sales

### Objective

The purpose of this measure is to calculate the total sales generated from all transactions.

### Copilot Suggestion

Copilot suggested using the SUM function on the sales amount column from the Fact_Sales table.

### Final DAX

```DAX
Total Sales =
SUM(Fact_Sales[sales_amount])
````

### Review and Validation

The suggested formula was checked against the Fact_Sales table and the sales_amount column. Since sales_amount represents the value of each transaction, SUM was appropriate for calculating the overall sales amount.

No major modification was required.

### Usage

This measure was used in KPI cards and other report visuals where total sales were required.

---

## 2. Month-over-Month Sales Growth

### Objective

This measure calculates the percentage change in sales between the current month and the previous month.

### Copilot Suggestion

Copilot suggested using CALCULATE and DATEADD to retrieve the sales value for the previous month and then compare it with the current month's sales.

### Final DAX

```DAX
MoM Sales Growth =
VAR CurrentSales =
    [Total Sales]
VAR PreviousSales =
    CALCULATE(
        [Total Sales],
        DATEADD(Dim_Date[date], -1, MONTH)
    )
RETURN
    DIVIDE(
        CurrentSales - PreviousSales,
        PreviousSales
    )
```

### Review and Validation

The initial approach was checked against the project's date dimension. The calculation was connected to Dim_Date[date] so that the measure could work correctly with the date relationships in the model.

The result was tested using the monthly sales visual to make sure the percentage changed according to the selected month.

### Usage

This measure was used to analyse whether sales increased or decreased compared with the previous month.

---

## 3. Running Total Sales

### Objective

The purpose of this measure is to calculate cumulative sales over time.

### Copilot Suggestion

Copilot suggested using CALCULATE, FILTER and ALLSELECTED to calculate sales from the beginning of the selected period up to the current date.

### Final DAX

```DAX
Running Total Sales =
CALCULATE(
    [Total Sales],
    FILTER(
        ALLSELECTED(Dim_Date[date]),
        Dim_Date[date] <= MAX(Dim_Date[date])
    )
)
```

### Review and Validation

The measure was tested using the date field from Dim_Date. The result was checked in a visual containing monthly or date-based values.

The important validation was to confirm that the value accumulated progressively rather than displaying the same total for every date.

### Usage

This measure was used to show cumulative sales performance across the selected period.

---

## 4. City Sales Rank

### Objective

This measure ranks cities according to their sales performance.

### Copilot Suggestion

Copilot suggested using the RANKX function to compare the sales values of the different cities.

### Final DAX

```DAX
City Sales Rank =
RANKX(
    ALLSELECTED(Dim_City[city]),
    [Total Sales],
    ,
    DESC,
    DENSE
)
```

### Review and Validation

The calculation was checked against the Dim_City table and the Total Sales measure.

The ranking was set to descending order so that the city with the highest sales receives Rank 1.

The DENSE option was used so that equal sales values receive the same rank without creating unnecessary gaps in the ranking.

### Usage

This measure was used to compare the performance of different cities and identify the higher and lower performing locations.

---

## 5. Average Transaction Value

### Objective

This measure calculates the average sales value generated per transaction.

### Copilot Suggestion

Copilot suggested calculating the average transaction value by dividing total sales by the number of unique transactions.

### Final DAX

```DAX
Average Transaction Value =
DIVIDE(
    [Total Sales],
    DISTINCTCOUNT(Fact_Sales[sale_id])
)
```

### Review and Validation

The calculation was checked against the transaction identifier in Fact_Sales.

DISTINCTCOUNT was used to count each transaction once. DIVIDE was used instead of direct division so that the measure could safely handle a zero denominator.

### Usage

This measure was used as a KPI to understand the average value of individual transactions.

---

# DAX Validation Process

The DAX formulas were not used without verification. The following process was followed during development:

1. Identify the purpose of the required measure.
2. Use Copilot to obtain an initial DAX approach.
3. Review the suggested formula.
4. Match the table and column names with the actual semantic model.
5. Check the filter and date context.
6. Create the measure in Power BI.
7. Test the result using report visuals.
8. Modify the formula when required.
9. Verify the final result using different filters and selections.

This process was particularly important for the time-based calculations because their results depend on the date dimension and filter context.


# Overall Observation

GitHub Copilot was useful for generating initial DAX approaches and explaining the purpose of different DAX functions. However, the suggested formulas still needed to be reviewed against the actual Power BI model.

Testing the measures through report visuals and applying filters helped confirm that the calculations were working as intended. The process also showed that AI-generated DAX should be treated as a starting point rather than being accepted without validation.

The combination of Power BI, GitHub, and Copilot provided a structured approach to developing and documenting the BrewMetrics BI solution.

```

This follows the assignment's requirement that `NOTES.md` document the Copilot-assisted development of the DAX measures, including what was suggested and how the formulas were reviewed/corrected. :contentReference[oaicite:0]{index=0}
```
