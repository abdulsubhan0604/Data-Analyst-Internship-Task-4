# Task 4 – Dashboard Design

## Objective
Create an interactive business dashboard in Power BI using a sales/financial dataset, with KPIs, slicers, time-series analysis, summary cards, a consistent theme, and navigation where practical.

## Dataset
**Supermart Grocery Sales – Retail Analytics Dataset**

The dataset is a Kaggle sales dataset containing 9,994 order records and 11 columns:
Order ID, Customer Name, Category, Sub Category, City, Order Date, Region, Sales, Discount, Profit, State.

Dataset source:
https://www.kaggle.com/datasets/mohamedharris/supermart-grocery-sales-retail-analytics-dataset

## Power BI Dashboard
Recommended page title: **Supermart Sales & Profit Dashboard**

### KPI cards
1. Total Sales
2. Total Profit
3. Profit Margin %
4. YoY Sales Growth %

### Slicers
- Year
- Region
- Category
- Sub Category

### Visuals
- Line chart: Sales by Month
- Clustered column chart: Sales and Profit by Category
- Bar chart: Profit by Region
- Bar chart: Top 10 Sub Categories by Sales
- Scatter chart: Sales vs Profit, with Discount as a tooltip
- Optional map: Sales by City

### Navigation
Create buttons/bookmarks for:
- Overview
- Product Analysis
- Regional Analysis

## DAX measures

```DAX
Total Sales = SUM('Sales'[Sales])

Total Profit = SUM('Sales'[Profit])

Profit Margin % = DIVIDE([Total Profit], [Total Sales], 0)

Sales LY =
CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR('Date'[Date])
)

YoY Sales Growth % =
DIVIDE(
    [Total Sales] - [Sales LY],
    [Sales LY],
    0
)
```

### Date table
```DAX
Date =
ADDCOLUMNS(
    CALENDAR(
        MIN('Sales'[Order Date]),
        MAX('Sales'[Order Date])
    ),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month", FORMAT([Date], "MMM"),
    "Year-Month", FORMAT([Date], "YYYY-MM")
)
```

Then:
- Mark `Date` as a Date Table.
- Create a relationship: `Date[Date]` → `Sales[Order Date]`.
- Sort `Date[Month]` by `Date[Month Number]`.

## Data preparation
In Power Query:
- Set Order Date to Date type.
- Set Sales, Discount and Profit to Decimal Number.
- Remove exact duplicate rows if present.
- Check null values.
- Rename the imported table to `Sales`.

## Design
Use a clean professional theme:
- Dark navy/blue header
- White/light-gray canvas
- One accent color for positive performance
- Consistent fonts and spacing
- Avoid excessive chart colors

## Business insights to discuss
The source analysis reports:
- Eggs, Meat & Fish, Snacks, and Fruits & Veggies as leading categories.
- Sales show growth across the years, with a significant increase in 2018.
- The reported relationship between profit and discount is minimal.

These are source-derived observations; the dashboard should allow the reviewer to validate them interactively.

## GitHub submission structure
```text
task-4-dashboard-design/
├── README.md
├── Task_4_Dashboard_Summary.pptx
├── DAX_Measures.txt
├── data/
│   └── Supermart Grocery Sales - Retail Analytics Dataset.csv
├── powerbi/
│   └── Task4_Sales_Dashboard.pbix
└── screenshots/
    ├── dashboard-overview.png
    ├── product-analysis.png
    └── regional-analysis.png
```

Do not upload the dataset if its license/redistribution terms do not permit it. In that case, keep the Kaggle source link in the README and add screenshots of the Power BI report.

## Interview questions
1. Key dashboard elements: KPIs, charts, filters/slicers, context, consistent design, and actionable insights.
2. KPI: Key Performance Indicator; a measurable value used to evaluate business performance against a target or objective.
3. Slicers: interactive visual filters that let users filter report data.
4. Power BI vs Tableau: both are BI/visual analytics platforms; Power BI integrates strongly with the Microsoft ecosystem and DAX, while Tableau is particularly known for flexible visual analytics and its visualization workflow.
5. Interactivity: slicers, cross-filtering, drill-through, tooltips, bookmarks and navigation buttons.
6. Large datasets: optimize the model, remove unnecessary columns, use star schema, aggregations/incremental refresh where appropriate, and avoid inefficient DAX.
7. Trend charts: line charts are the primary choice; clustered columns can complement them for period comparisons.
