# Week 05 — Thursday Integration Assignment

## Full EDA Pipeline on an Orders Dataset

### Overview

This project is the Thursday integration assignment for Week 05. The goal is to perform a complete **Exploratory Data Analysis (EDA) pipeline** on a larger and intentionally messy orders dataset.

The workflow covers:

1. Dataset generation
2. Initial diagnosis
3. Data-quality issue identification
4. Column-by-column cleaning
5. Cleaning validation
6. Data visualization
7. Findings and interpretation
8. Technical summary
9. Git-based submission workflow

The analysis was performed using **Python, Pandas, NumPy, and Matplotlib**.

---

## Learning Objectives

This assignment demonstrates the ability to:

* Perform an end-to-end EDA workflow independently.
* Diagnose missing values and other data-quality problems.
* Identify problems that are not simply missing values.
* Make and justify cleaning decisions for individual columns and issues.
* Select appropriate visualizations for different analytical questions.
* Interpret charts using evidence from the dataset.
* Produce a concise technical summary for a non-technical reader.
* Use a feature branch and meaningful Git commits as part of the submission workflow.

---

## Dataset

The dataset represents order transactions containing the following columns:

| Column             | Description                      |
| ------------------ | -------------------------------- |
| `order_id`         | Unique identifier for each order |
| `order_date`       | Date and time of the order       |
| `customer_id`      | Customer identifier              |
| `product_category` | Product category                 |
| `quantity`         | Number of units ordered          |
| `unit_price`       | Price per unit                   |
| `region`           | Geographic region                |

The dataset was generated from the required assignment specification using a fixed random seed of `42`.

### Initial Dataset

The original dataset contained:

* **5,015 rows** after intentionally adding 15 duplicate records.
* **7 columns**
* Missing customer and region values
* Inconsistent category capitalization
* Negative quantities
* Extreme unit-price values
* Duplicate records

---

# 1. Diagnosis

The diagnosis stage was completed before cleaning the dataset.

The following Pandas diagnostic methods were used:

* `.head()`
* `.info()`
* `.describe()`
* `.isna().sum()`
* `.value_counts()`

The diagnosis identified the following data-quality problems:

### Missing Values

Missing values were found in:

* `customer_id`
* `region`

### Inconsistent Category Labels

The `product_category` column contained both:

```text
Electronics
electronics
```

These represent the same category but use inconsistent capitalization.

### Negative Quantities

Some `quantity` values were negative. These were treated as invalid records for this analysis because the assignment required the negative quantities to be removed.

### Extreme Unit Prices

Twenty records contained a unit price of:

```text
4999.99
```

These values were identified as intentionally introduced data-entry outliers.

### Duplicate Records

Fifteen duplicate rows were introduced into the dataset and identified during diagnosis.

---

# 2. Data Cleaning

Each identified problem was handled separately rather than applying one blanket cleaning operation.

## Missing `customer_id`

Missing customer IDs were removed because customer-level identification is important for transaction analysis, and inventing customer IDs would introduce false information.

## Missing `region`

Missing region values were replaced with:

```text
Unknown
```

This preserves the order records while explicitly identifying that the geographic information was unavailable.

## Inconsistent Product Categories

Category labels were standardized so that:

```text
electronics
```

became:

```text
Electronics
```

This ensures that the same category is not counted as two separate categories.

## Negative Quantities

Negative quantity records were removed because they represent invalid quantities for the sales analysis being performed.

## Extreme Unit Prices

The 19 remaining `4999.99` unit-price values were identified as implausible data-entry outliers.

Instead of allowing these extreme values to distort the analysis, they were replaced with the median of the non-outlier unit prices.

The calculated replacement median was:

```text
44.69
```

The median was selected because it is less sensitive to extreme values than the mean.

## Duplicate Rows

The 15 duplicate records were removed so that repeated transactions would not artificially increase order counts or distort the analysis.

---

# 3. Final Cleaning Validation

After cleaning, the dataset contained:

```text
Shape: (4788, 7)
```

Final validation confirmed:

| Check                      | Result |
| -------------------------- | -----: |
| Missing values             |      0 |
| Negative quantities        |      0 |
| Negative unit prices       |      0 |
| `4999.99` prices remaining |      0 |
| Duplicate rows             |      0 |

### Final Category Counts

| Product Category | Orders |
| ---------------- | -----: |
| Electronics      |  1,922 |
| Home Goods       |  1,002 |
| Apparel          |    955 |
| Books            |    909 |

The cleaned dataset was saved as:

```text
cleaned_orders.csv
```

---

# 4. Visualization

The visualization stage was designed to answer different analytical questions.

All charts were created using:

```python
fig, ax = plt.subplots()
```

and were given appropriate titles and axis labels.

## 4.1 Unit Price Distribution

**Chart:** Histogram

**Question:** What does the distribution of unit prices look like?

The histogram shows that most unit prices are concentrated in the lower-to-middle price range, while relatively fewer observations occur at higher prices.

---

## 4.2 Product Category Distribution

**Chart:** Bar chart

**Question:** Which product categories have the most orders?

Electronics has the highest number of orders with **1,922 records**.

The remaining categories have lower but relatively similar order counts:

* Home Goods: 1,002
* Apparel: 955
* Books: 909

This indicates that Electronics is the most represented category in the cleaned dataset.

---

## 4.3 Order Quantity Distribution

**Chart:** Histogram

**Question:** How are order quantities distributed?

The quantity distribution covers the observed range of order sizes without a strong concentration around one particular quantity.

The orders generally contain relatively small numbers of units, with no extreme quantity values remaining after cleaning.

---

## 4.4 Unit Price vs Order Quantity

**Chart:** Scatter plot

**Question:** Is there an obvious relationship between unit price and quantity?

The scatter plot does not show a strong visible linear relationship between unit price and order quantity.

Different quantities occur across a range of prices, suggesting that higher-priced products are not consistently ordered in either larger or smaller quantities.

---

## 4.5 Orders Over Time

**Chart:** Line chart

**Question:** How does order activity change over time?

The daily order counts appear relatively consistent across the observed period, without major spikes or sudden drops.

Because each cleaned record has a unique order date in this dataset, the time-series pattern should be interpreted carefully rather than treated as evidence of a real-world seasonal trend.

---

## 4.6 Orders by Region

**Chart:** Bar chart

**Question:** How are orders distributed geographically?

The four named regions have relatively similar order volumes, while a smaller number of records are classified as `Unknown`.

This indicates that the dataset does not show strong regional concentration.

---

## 4.7 Unit Price by Product Category

**Chart:** Box plot

**Question:** How does unit-price distribution vary across product categories?

The box plot allows the price distributions of the four categories to be compared through their medians, quartiles, whiskers, and remaining variation.

The category distributions are broadly similar, although the category-level differences should be interpreted from the actual cleaned data rather than assuming that identical distributions imply identical pricing strategies.

---

## 4.8 Average Unit Price by Region

**Chart:** Bar chart

**Question:** Does average unit price differ substantially between regions?

The average unit prices across regions are relatively close to one another.

This suggests that there is no strong regional price difference visible in this dataset.

---

## 4.9 Order Quantity by Region

**Chart:** Box plot

**Question:** Does order quantity vary substantially across regions?

The box plots show broadly similar quantity distributions across regions.

No major regional difference in typical order quantity is evident from this visualization.

---

## 4.10 Correlation Heatmap

**Chart:** Heatmap

**Question:** Are there strong linear relationships between numerical variables?

The correlation heatmap provides an overview of relationships between numerical variables such as:

* `quantity`
* `unit_price`
* and other numerical fields included in the analysis

The analysis does not indicate a strong relationship between unit price and quantity.

---

# 5. Key Findings

### Finding 1 — Electronics Has the Highest Order Volume

Electronics is the most frequently represented product category with **1,922 orders**, considerably more than Books, which has **909 orders**.

This indicates that Electronics contributes the largest share of orders in the cleaned dataset.

### Finding 2 — Unit Price and Quantity Do Not Show a Strong Relationship

The unit-price-versus-quantity scatter plot shows no obvious strong linear relationship.

This suggests that customers do not consistently purchase either larger or smaller quantities based solely on unit price.

### Finding 3 — Regional Order and Pricing Patterns Are Relatively Similar

The regional visualizations show relatively similar order volumes, average prices, and quantity distributions across the named regions.

Therefore, the cleaned dataset does not reveal a strong geographic difference in purchasing behavior.

---

# 6. Technical Summary

This project analyzed an intentionally messy orders dataset containing order IDs, dates, customer IDs, product categories, quantities, unit prices, and regions.

The initial diagnosis identified multiple data-quality problems, including missing customer and region values, inconsistent product-category capitalization, negative quantities, extreme unit-price values, and duplicate records.

Each issue was handled separately with a justified cleaning decision. Missing customer IDs were removed, missing regions were labeled as `Unknown`, category labels were standardized, negative quantities were removed, extreme `4999.99` prices were replaced using the non-outlier median of **44.69**, and duplicate rows were removed.

After cleaning, the final dataset contained **4,788 records and 7 columns** with no remaining missing values, negative quantities, negative prices, targeted extreme prices, or duplicate records.

The visual analysis found that Electronics had the highest order volume, unit price did not show a strong visible relationship with quantity, and regional purchasing and pricing patterns were broadly similar.

### Limitation

The dataset is artificially generated according to a fixed assignment specification rather than collected from a real business. Therefore, the observed patterns should not be interpreted as real-world customer or market behavior.

Additionally, the dataset contains a highly controlled structure, so some apparent uniformity in the visualizations may be a characteristic of the generated data rather than evidence of genuine business equilibrium.

---

# 7. Project Structure

```text
Day4_task/
│
├── figures/
│   ├── avg_unit_price_by_region.png
│   ├── correlation_heatmap.png
│   ├── order_quantity_distribution.png
│   ├── orders_by_region.png
│   ├── orders_over_time.png
│   ├── product_category_distribution.png
│   ├── quantity_by_region.png
│   ├── unit_price_by_category.png
│   ├── unit_price_distribution.png
│   └── unit_price_vs_quantity.png
│
├── cleaned_orders.csv
├── README.md
├── requirements.txt
└── week05_day4_eda.ipynb
```

---

# 8. Tools and Libraries

* Python
* NumPy
* Pandas
* Matplotlib
* Jupyter Notebook
* Git
* GitHub

---

# 9. Git Workflow

The assignment was completed on a dedicated feature branch:

```text
feature/week5-eda
```

Meaningful commits were used during the development process, including commits for:

* EDA cleaning
* Cleaning validation
* Saving the cleaned dataset
* Visualization and analysis
* Repository cleanup

The repository was reviewed locally before pushing the feature branch.

---

# 10. Final Status

The Thursday integration assignment covers the complete EDA workflow:

**Generate → Diagnose → Clean → Validate → Visualize → Interpret → Summarize → Version Control**

The cleaned dataset and visualization outputs are prepared for submission and review.
