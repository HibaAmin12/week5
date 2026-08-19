# Week 5 — Day 3: Exploratory Data Analysis & Data Visualization

## Overview

This task focuses on applying Pandas and Matplotlib concepts to a real-world retail transaction dataset. The main objective was to perform Exploratory Data Analysis (EDA), identify data-quality issues, clean the dataset using evidence-based decisions, and visualize important patterns in sales, customers, products, and countries.

The analysis follows a structured workflow:

**Load → Inspect → Diagnose → Clean → Verify → Analyze → Visualize → Interpret**

The primary dataset used for this task is the **Online Retail Dataset**, which contains transactional records from a UK-based online retail business.

---

## Objectives

The main objectives of this task were to:

- Load and inspect a real-world dataset using Pandas.
- Understand the structure, dimensions, columns, and data types.
- Identify missing values and calculate their percentages.
- Detect and remove duplicate records.
- Convert date columns to appropriate datetime types.
- Investigate unusual and invalid values.
- Handle negative quantities appropriately.
- Remove transactions with non-positive unit prices.
- Create a new `TotalSales` feature.
- Perform descriptive statistical analysis.
- Analyze sales distributions.
- Compare sales across countries.
- Analyze the relationship between quantity and unit price.
- Examine daily sales trends over time.
- Identify top customers based on total sales.
- Identify top products based on total sales.
- Compare honest and potentially misleading visualizations.
- Save generated visualizations for further use.

---

# Dataset

## Online Retail Dataset

The Online Retail dataset contains transactional records from an online retail business.

Each row represents a transaction line and contains information about the invoice, product, quantity, price, customer, and country.

### Dataset Columns

| Column | Description |
|---|---|
| `InvoiceNo` | Unique invoice number associated with a transaction |
| `StockCode` | Unique product or stock code |
| `Description` | Description of the product |
| `Quantity` | Number of units purchased |
| `InvoiceDate` | Date and time of the transaction |
| `UnitPrice` | Price of one unit of the product |
| `CustomerID` | Unique identifier of the customer |
| `Country` | Country associated with the transaction |

---

# Project Structure

```text
Day3_task/
│
├── data/
│   ├── Online Retail.xlsx
│   ├── Online_Retail.csv
│   └── Online_Retail_Cleaned.csv
│
├── figures/
│   ├── amount_boxplot.png
│   ├── amount_histogram.png
│   ├── amount_vs_age_scatter.png
│   ├── bar_category_totals.png
│   ├── correlation_heatmap.png
│   ├── daily_total_sales.png
│   ├── eda_subplots.png
│   ├── eda_subplots_retail.png
│   ├── honest_vs_misleading.png
│   ├── honest_vs_misleading_retail.png
│   ├── line_amount_over_time.png
│   ├── quantity_vs_unitprice_scatter.png
│   ├── retail_total_sales_histogram.png
│   ├── retail_total_sales_histogram_zoomed.png
│   ├── top_10_countries_sales.png
│   ├── top_10_customers_sales.png
│   └── top_10_products_sales.png
│
├── requirements.txt
├── README.md
└── week05_day3_eda_visualization.ipynb
```

---

# 1. Loading the Dataset

The Online Retail dataset was loaded using Pandas.

The dataset was initially loaded without performing any cleaning so that its original condition could be inspected first.

```python
retail_df = pd.read_csv("data/Online_Retail.csv")
```

The initial dataset contained:

- 541,909 rows
- 8 columns

The original dataset contained missing values, duplicate records, unusual quantities, and non-positive unit prices.

---

# 2. Initial Dataset Inspection

Several Pandas methods were used to understand the structure and quality of the dataset.

**Dataset dimensions**

```python
retail_df.shape
```

The original dataset contained:

`541909 rows × 8 columns`

**Column names**

```python
retail_df.columns
```

The dataset contained the following columns:

- InvoiceNo
- StockCode
- Description
- Quantity
- InvoiceDate
- UnitPrice
- CustomerID
- Country

**Dataset information**

```python
retail_df.info()
```

The `info()` method was used to inspect:

- Number of non-null values
- Data types
- Memory usage

**Missing values**

**Statistical summary**

```python
retail_df.describe()
```

The `describe()` method was used to examine numerical columns such as:

- Quantity
- UnitPrice
- CustomerID

This helped identify unusual values and potential outliers.

---

# 3. Missing Value Analysis

Missing values were diagnosed using:

```python
retail_df.isna().sum()
```

The original dataset contained missing values mainly in:

- Description
- CustomerID

The `CustomerID` column had a substantial number of missing values.

Instead of replacing missing customer IDs with artificial values, they were preserved because the actual customer identity could not be reliably reconstructed.

This allows the transactions to remain useful for transaction-level and sales-level analysis.

For customer-level analysis, records without a valid CustomerID can be excluded when necessary.

---

# 4. Duplicate Record Detection

Duplicate rows were identified using:

```python
retail_df.duplicated().sum()
```

The original dataset contained:

**5,268 duplicate records**

Duplicate records can cause transactions to be counted multiple times and may distort statistical analysis.

Therefore, duplicate records were removed using:

```python
retail_df = retail_df.drop_duplicates().copy()
```

The dataset was then checked again to verify that duplicate records had been removed.

---

# 5. Date Conversion

The `InvoiceDate` column was initially stored as an object/string data type.

Since it represents transaction date and time, it was converted to Pandas datetime format:

```python
retail_df["InvoiceDate"] = pd.to_datetime(retail_df["InvoiceDate"])
```

This conversion allows the dataset to be used for:

- Date-based filtering
- Sorting
- Daily sales analysis
- Time-series visualization
- Extracting year, month, day, and other time components

---

# 6. Investigating Negative Quantities

The `Quantity` column was inspected for negative values:

```python
(retail_df["Quantity"] < 0).sum()
```

Negative quantities were investigated rather than immediately treated as errors.

In the Online Retail dataset, many negative quantities are associated with cancelled transactions. Invoice numbers beginning with `C` commonly indicate cancellations.

For the purpose of analyzing completed positive sales transactions, negative-quantity records were excluded:

```python
retail_df = retail_df[retail_df["Quantity"] >= 0].copy()
```

This prevents cancelled or returned quantities from distorting positive sales analysis.

---

# 7. Investigating Unit Price

The `UnitPrice` column was checked for zero and negative values:

```python
(retail_df["UnitPrice"] <= 0).sum()
```

Transactions with zero or negative unit prices do not represent normal positive-price sales.

Therefore, they were removed:

```python
retail_df = retail_df[retail_df["UnitPrice"] > 0].copy()
```

This produces a cleaner dataset for revenue and sales-value analysis.

---

# 8. Final Data Cleaning Verification

After cleaning, the dataset was checked again using:

```python
retail_df.info()
retail_df.isna().sum()
retail_df.duplicated().sum()
(retail_df["Quantity"] < 0).sum()
(retail_df["UnitPrice"] <= 0).sum()
```

These checks confirmed that the major cleaning operations had been applied successfully.

The cleaned dataset contained approximately:

**524,878 rows**

The `CustomerID` column still contains missing values because those customer identifiers cannot be reliably reconstructed.

---

# 9. Creating Total Sales

A new feature called `TotalSales` was created to represent the monetary value of each transaction line.

The calculation is:

`TotalSales = Quantity × UnitPrice`

Implemented using:

```python
retail_df["TotalSales"] = (
    retail_df["Quantity"] * retail_df["UnitPrice"]
)
```

The resulting column was used throughout the EDA to analyze sales performance.

---

# 10. Descriptive Statistics of Total Sales

The distribution of `TotalSales` was examined using:

```python
retail_df["TotalSales"].describe()
```

The statistics showed:

- Mean ≈ 20.28
- Median ≈ 9.92
- Minimum ≈ 0.001
- Maximum ≈ 168,469.60

The large difference between the mean and median indicates a highly skewed distribution.

---

# 11. Distribution of Transaction Sales

A histogram was created to examine the distribution of transaction-level sales.

The distribution is strongly right-skewed.

Most transactions have relatively small sales values, while a small number of transactions have very large values.

### Findings

- Most transactions are concentrated in the lower sales range.
- The median transaction sales value is approximately 9.92.
- The mean is considerably higher than the median because of extreme high-value transactions.
- A long right tail is present.
- Some unusually large transactions may represent legitimate bulk purchases or special orders.
- These extreme values should be investigated separately when performing statistical analysis.

**Business implication**

The majority of transactions are relatively small, while a small number of high-value transactions contribute disproportionately to the overall spread of transaction sales.

---

# 12. Top 10 Countries by Total Sales

Country-level sales were calculated using `groupby()`:

```python
top_countries = (
    retail_df.groupby("Country")["TotalSales"]
    .sum()
    .sort_values(ascending=False)
    .head(10)
)
```

### Findings

- The United Kingdom overwhelmingly dominates total sales.
- UK sales are close to 9 million in the analyzed dataset.
- The remaining countries contribute substantially smaller amounts.
- The Netherlands and EIRE are among the stronger markets after the UK.
- The sales distribution is therefore highly concentrated geographically.

**Business implication**

The UK appears to be the primary market for the business. This strong concentration may indicate an opportunity to explore growth and diversification in international markets.

---

# 13. Quantity vs Unit Price

A scatter plot was used to investigate the relationship between Quantity and UnitPrice.

```python
plt.scatter(
    retail_df["Quantity"],
    retail_df["UnitPrice"],
    alpha=0.3
)
```

### Findings

- Most transactions are concentrated near the lower values of both quantity and unit price.
- There is no obvious strong linear relationship between quantity and unit price.
- Several extreme values are visible.
- Some transactions contain unusually high unit prices.
- Some transactions involve unusually large quantities at low unit prices.

**Business implication**

The extreme values may represent:

- Bulk purchases
- Premium products
- Special orders
- Non-standard transactions
- Potential data-quality issues

These records should be investigated separately rather than assuming that every extreme value is an error.

---

# 14. Daily Total Sales Over Time

Daily sales were calculated by grouping transactions by date.

The resulting data was visualized using a line chart.

### Findings

- Daily sales show considerable variation throughout the year.
- Sales frequently fluctuate between lower and higher levels.
- Several prominent sales spikes appear throughout the year.
- Sales activity becomes stronger toward the later part of the year.
- A particularly large spike appears near the end of the timeline.

**Business implication**

The increase in sales activity toward the end of the year may indicate seasonal retail demand.

Further analysis could compare these patterns with:

- Holidays
- Promotions
- Special events
- Product launches
- Marketing campaigns

---

# 15. Top 10 Customers by Total Sales

Customer-level sales were analyzed using `CustomerID`.

```python
top_customers = (
    retail_df.dropna(subset=["CustomerID"])
    .groupby("CustomerID")["TotalSales"]
    .sum()
    .sort_values(ascending=False)
    .head(10)
)
```

### Findings

- A relatively small number of customers generate very high total sales.
- Customer 14646 is the highest-value customer in the analyzed data.
- Customers 18102, 17450, and 16446 are also among the highest contributors.
- The top customers contribute substantially more than typical customers.

**Business implication**

These customers can be considered high-value customers and may be important for:

- Customer retention
- Loyalty programs
- Personalized offers
- Relationship management

However, a formal Pareto/80-20 conclusion should be tested quantitatively rather than assumed.

---

# 16. Top 10 Products by Total Sales

Products were ranked according to their total sales value.

```python
top_products = (
    retail_df.groupby("Description")["TotalSales"]
    .sum()
    .sort_values(ascending=False)
    .head(10)
)
```

### Findings

- DOTCOM POSTAGE appears as the highest-sales entry.
- REGENCY CAKESTAND 3 TIER and PAPER CRAFT, LITTLE BIRDIE are also among the strongest entries.
- Several other products contribute significant sales.
- Some entries such as POSTAGE and Manual are not conventional merchandise products.

**Business implication**

Non-merchandise entries such as postage and manual adjustments should be considered separately when analyzing actual product performance.

For a more accurate merchandise ranking, these administrative or service-related entries can be filtered out in a dedicated product analysis.

---

# 17. Honest vs Misleading Visualization

A comparison was created between an honest and a potentially misleading bar chart.

The purpose was to demonstrate how visualization choices can influence interpretation.

### Findings

- Both bar charts were built from the same underlying country sales data, with the United Kingdom dominating at roughly 9 million against much smaller totals for every other country.
- Because the UK's sales value is so much larger than the rest, the two charts can appear visually very similar even when a manipulation (such as a narrower axis range, altered spacing, or bar proportions) has been applied — the extreme scale of the data masks the distortion.
- This shows that misleading visualizations are not always visually obvious; the effect of an axis or proportion trick can be diluted when the underlying values already differ by orders of magnitude.
- Relying on visual comparison alone is not a reliable way to detect a misleading chart — the axis limits and scaling logic in the underlying code should always be checked explicitly.

**Key lesson**

A chart should always be evaluated based on:

- Axis scale
- Starting point of the axis
- Bar proportions
- Labels
- Category ordering
- Visual spacing
- Context

For bar charts, beginning the y-axis at zero is generally important because bar length represents magnitude.

**Main takeaway**

The underlying data does not change when the visualization changes, but the visual presentation can influence how the audience perceives differences between categories. When the data itself spans a very large range, even a genuine manipulation can be hard to spot by eye, which makes explicitly checking chart construction (not just appearance) essential.

---

# 18. Visualizations Created

The EDA includes several visualizations:

**Sales Distribution**
- `retail_total_sales_histogram.png`
- `retail_total_sales_histogram_zoomed.png`

Used to understand the distribution and skewness of transaction sales.

**Country Sales**
- `top_10_countries_sales.png`

Shows the top 10 countries based on total sales.

**Quantity vs Unit Price**
- `quantity_vs_unitprice_scatter.png`

Shows the relationship between quantity and unit price.

**Daily Sales**
- `daily_total_sales.png`

Shows changes in total sales over time.

**Customer Analysis**
- `top_10_customers_sales.png`

Shows the highest-value customers based on total sales.

**Product Analysis**
- `top_10_products_sales.png`

Shows the products or transaction descriptions with the highest total sales.

**Visualization Comparison**
- `honest_vs_misleading_retail.png`

Demonstrates how visualization choices can affect interpretation.

---

# 19. Technologies Used

The following tools and libraries were used:

- Python
- Pandas
- NumPy
- Matplotlib
- Jupyter Notebook

---

# 20. Main Pandas Concepts Practiced

This task provided practical experience with:

- `pd.read_csv()`
- `DataFrame.shape`
- `DataFrame.info()`
- `DataFrame.describe()`
- `DataFrame.columns`
- `DataFrame.isna()`
- `DataFrame.duplicated()`
- `DataFrame.drop_duplicates()`
- Boolean filtering
- `pd.to_datetime()`
- `groupby()`
- `sum()`
- `mean()`
- `sort_values()`
- `head()`
- `dropna()`
- Creating calculated columns
- Data aggregation
- Time-series analysis

---

# 21. Main Visualization Concepts Practiced

The task also covered:

- Histograms
- Bar charts
- Scatter plots
- Line charts
- Subplots
- Axis labels
- Chart titles
- Tick rotation
- Figure sizing
- Saving figures to files
- Comparing honest and misleading visualizations

---

# 22. Key Findings Summary

The major findings from the EDA are:

- The transaction sales distribution is strongly right-skewed.
- Most transactions have relatively small sales values.
- A small number of transactions contain extreme high sales values.
- The United Kingdom is by far the largest market.
- Quantity and unit price do not show a clear strong linear relationship.
- Several extreme values exist in both quantity and unit price.
- Daily sales show substantial volatility and noticeable spikes.
- Sales activity becomes stronger toward the end of the year.
- A small group of customers contributes a significant amount of total sales.
- Some high-sales entries are non-merchandise items such as postage and manual entries.
- Honest and misleading bar charts can look deceptively similar when the underlying data spans a very large range, so chart construction should always be checked explicitly rather than judged by eye alone.

---

# 23. Conclusion

This task demonstrated a complete Exploratory Data Analysis workflow using a real-world retail dataset.

The dataset was first inspected to understand its structure and identify data-quality issues. Missing values, duplicate records, negative quantities, and non-positive unit prices were then investigated and handled according to the meaning of the data.

After cleaning, additional features such as `TotalSales` were created to support business-oriented analysis.

The EDA revealed important patterns in transaction values, country-level sales, customer contribution, product performance, and sales over time. Visualizations were used to make these patterns easier to understand and communicate.

The overall workflow demonstrates that effective data analysis is not simply about creating charts. It requires:

**Understanding the data → Diagnosing problems → Making justified cleaning decisions → Verifying the result → Analyzing patterns → Communicating findings clearly.**