# 📊 Day 5 — Exploratory Data Analysis with Pandas

## 📌 Project Overview

This project focuses on **Exploratory Data Analysis (EDA)** using Python and Pandas on a sales dataset.

The purpose of this project is to inspect, clean, analyze, and visualize sales data in order to identify meaningful patterns in customer purchasing behavior, product performance, transaction amounts, and sales trends.

The analysis covers both **statistical exploration** and **data visualization** using Pandas and Matplotlib.

---

## 🎯 Objectives

The main objectives of this project are:

- Understand the structure of the sales dataset
- Inspect data types and dataset dimensions
- Identify and handle missing values
- Check for duplicate records
- Perform descriptive statistical analysis
- Analyze categorical and numerical variables
- Explore customer purchasing behavior
- Analyze sales by product category
- Compare sales across genders
- Analyze sales across different age groups
- Study monthly sales trends
- Analyze transaction quantities
- Examine product price distribution
- Investigate relationships between numerical variables
- Create meaningful visualizations
- Extract business insights from the data

---

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| Python | Programming language |
| Pandas | Data manipulation and analysis |
| NumPy | Numerical operations |
| Matplotlib | Data visualization |
| Jupyter Notebook | Interactive analysis environment |

---

## 📂 Project Structure

```text
Day5/
│
├── data/
│   └── sales_dataset.csv
│
├── figures/
│   ├── 01_total_amount_distribution.png
│   ├── 02_total_amount_by_category.png
│   ├── 03_avg_amount_by_gender.png
│   ├── 04_age_vs_total_amount.png
│   ├── 05_monthly_sales_trend.png
│   ├── 06_total_sales_by_category.png
│   ├── 07_total_sales_by_gender.png
│   ├── 08_total_sales_by_age_group.png
│   ├── 09_transactions_by_category.png
│   ├── 10_avg_transaction_by_category.png
│   ├── 11_monthly_transaction_count.png
│   ├── 12_correlation_heatmap.png
│   ├── 13_amount_distribution_gender.png
│   ├── 14_quantity_distribution.png
│   └── 15_price_per_unit_distribution.png
│
├── Day5.ipynb
├── requirements.txt
└── README.md
```

> **Note:** Replace `sales_dataset.csv` with the exact filename of your sales CSV file if it is different.

---

## 📊 Dataset

The project uses a **Sales Dataset in CSV format**.

The dataset contains information related to customer transactions, product categories, quantities, prices, customer demographics, and total transaction amounts.

### Main Features

| Column | Description |
|---|---|
| `Date` | Date of the transaction |
| `Gender` | Gender of the customer |
| `Age` | Age of the customer |
| `Product Category` | Category of the purchased product |
| `Quantity` | Number of units purchased |
| `Price per Unit` | Price of one unit of the product |
| `Total Amount` | Total value of the transaction |

---

# 🔍 Exploratory Data Analysis

## 1. Data Inspection

The dataset was initially inspected to understand:

- Number of rows and columns
- Column names
- Data types
- Non-null values
- Basic dataset structure

Functions such as the following were used:

```python
df.head()
df.shape
df.info()
df.describe()
```

---

## 2. Data Cleaning

The dataset was checked and prepared before performing exploratory analysis.

The following steps were considered during data preparation:

- Checking missing values
- Checking duplicate records
- Verifying data types
- Converting date columns into datetime format
- Handling missing values where required
- Checking numerical columns
- Creating age groups for further analysis

---

# 📈 Data Visualizations

## 1. Distribution of Total Amount

A histogram was created to understand the distribution of transaction amounts across the dataset.

**Purpose:**

- Identify common transaction ranges
- Understand the spread of transaction amounts
- Observe the overall distribution

---

## 2. Total Amount by Product Category

A bar chart was used to compare total transaction amounts across product categories.

**Purpose:**

- Identify high-performing categories
- Compare category-level sales contribution

---

## 3. Average Total Amount by Gender

The average transaction amount was compared between Female and Male customers.

**Finding:**

Female and Male customers show very similar average transaction amounts.

---

## 4. Age vs Total Amount

A scatter plot was used to investigate the relationship between customer age and transaction amount.

**Finding:**

No clear linear relationship was observed between Age and Total Amount.

Customers across different age groups make both lower-value and higher-value purchases.

---

## 5. Monthly Sales Trend

Monthly sales were analyzed using a line chart.

**Purpose:**

- Identify high and low sales periods
- Observe fluctuations in monthly revenue
- Understand sales trends over time

The analysis shows noticeable fluctuations in monthly sales.

---

## 6. Total Sales by Product Category

A bar chart was used to compare total sales among product categories.

### Key Finding

**Electronics** is the highest-performing category, followed closely by **Clothing**, while **Beauty** has comparatively lower total sales.

---

## 7. Total Sales by Gender

Total sales were compared between Female and Male customers.

### Key Finding

Female customers contribute slightly more total sales than Male customers, but the difference is relatively small.

This indicates that the business has a fairly balanced sales contribution across genders.

---

## 8. Total Sales by Age Group

Customers were divided into different age groups:

- 18–25
- 26–35
- 36–45
- 46–55
- 56–65

### Key Finding

The **46–55** and **26–35** age groups contribute strongly to total sales.

The younger and older age groups show comparatively lower sales.

---

## 9. Transaction Count by Product Category

The number of transactions was compared across product categories.

This helps determine whether a category generates sales because of:

- More transactions
- Higher transaction values
- Or both

---

## 10. Average Transaction Amount by Product Category

Average transaction amounts were calculated for each product category.

This analysis helps compare the typical transaction value across categories.

---

## 11. Monthly Transaction Count

The number of transactions was analyzed month by month.

This provides another perspective on monthly business activity in addition to total sales.

---

## 12. Correlation Heatmap

A correlation heatmap was created to examine relationships between numerical variables.

### Key Findings

- `Price per Unit` has the strongest positive relationship with `Total Amount`.
- `Quantity` has a weaker positive relationship with `Total Amount`.
- `Age` shows little or no linear relationship with the other numerical variables.
- `Quantity` and `Price per Unit` show little relationship with each other.

---

## 13. Transaction Amount Distribution by Gender

A box plot was used to compare transaction amount distributions between Female and Male customers.

### Key Finding

The distributions for both genders are very similar, suggesting that gender does not create a major difference in transaction amount distribution in this dataset.

---

## 14. Distribution of Quantity

A histogram was used to analyze the number of units purchased per transaction.

### Key Finding

Transaction quantities are mainly concentrated between **1 and 4 units**.

The different quantity values occur with relatively similar frequencies.

---

## 15. Distribution of Price per Unit

A histogram was created to analyze product price levels.

### Key Finding

The dataset contains several distinct price levels, with lower-priced products appearing more frequently than some higher-priced products.

---

# 📌 Key Findings

The overall EDA produced the following findings:

1. **Electronics** is the highest-performing product category by total sales.
2. **Clothing** follows Electronics closely.
3. **Beauty** has comparatively lower total sales.
4. Female and Male customers have **similar average transaction amounts**.
5. Total sales are relatively balanced between genders.
6. Age does not show a clear linear relationship with transaction amount.
7. The **26–35** and **46–55** age groups contribute strongly to total sales.
8. Monthly sales show noticeable fluctuations over time.
9. `Price per Unit` has the strongest positive correlation with `Total Amount`.
10. `Quantity` has a weaker positive relationship with `Total Amount`.
11. Transaction quantities are mainly between **1 and 4 units**.
12. Product prices appear at several distinct pricing levels.

---

# 💡 Business Insights

Based on the exploratory analysis:

### Product Strategy

Electronics is the strongest category in terms of total sales. Product-level analysis can be performed further to identify the products responsible for this performance.

### Customer Segmentation

Gender does not show a major difference in transaction behavior. Therefore, customer segmentation should not rely solely on gender.

### Age-Based Analysis

Although some age groups contribute more sales than others, age does not show a strong direct relationship with transaction amount.

Additional customer characteristics should be considered for more effective segmentation.

### Pricing

The strong relationship between `Price per Unit` and `Total Amount` indicates that product price is an important factor associated with transaction value.

### Sales Monitoring

Monthly sales fluctuations indicate that tracking sales trends over time can help identify periods of higher and lower business activity.

---

# 📊 Figures

All visualizations generated during the analysis are saved inside the:

```text
figures/
```

directory.

The saved figures include:

- Total Amount Distribution
- Total Amount by Product Category
- Average Amount by Gender
- Age vs Total Amount
- Monthly Sales Trend
- Total Sales by Product Category
- Total Sales by Gender
- Total Sales by Age Group
- Transaction Count by Product Category
- Average Transaction by Product Category
- Monthly Transaction Count
- Correlation Heatmap
- Transaction Amount Distribution by Gender
- Quantity Distribution
- Price per Unit Distribution

All figures are saved at high resolution using Matplotlib.

---

# ⚙️ Installation

## 1. Clone the Repository

```bash
git clone <your-github-repository-url>
```

## 2. Navigate to the Project

```bash
cd Day5
```

## 3. Create a Virtual Environment

```bash
python3 -m venv .venv
```

## 4. Activate the Virtual Environment

### Linux / Ubuntu

```bash
source .venv/bin/activate
```

### Windows

```bash
.venv\Scripts\activate
```

## 5. Install Dependencies

```bash
pip install -r requirements.txt
```

---

# ▶️ Running the Project

Start Jupyter Notebook:

```bash
jupyter notebook
```

or JupyterLab:

```bash
jupyter lab
```

Then open:

```text
Day5.ipynb
```

Run the notebook cells sequentially to reproduce the analysis and visualizations.

---

# 📦 Requirements

The main dependencies used in this project include:

```text
pandas==2.3.3
numpy
matplotlib
jupyter
```

The exact versions can be found in:

```text
requirements.txt
```

---

# 🎓 Learning Outcomes

Through this project, the following concepts were practiced:

- Pandas DataFrame operations
- Data inspection
- Data cleaning
- Missing value analysis
- Duplicate checking
- Data type handling
- Datetime manipulation
- GroupBy operations
- Aggregation
- Descriptive statistics
- Correlation analysis
- Feature creation
- Age-group analysis
- Matplotlib visualization
- Histogram
- Bar chart
- Line chart
- Scatter plot
- Box plot
- Correlation heatmap
- Business insight generation
- Exploratory Data Analysis workflow

---

# 🚀 Future Improvements

The project can be extended with:

- Customer-level segmentation
- Repeat customer analysis
- Product-level performance analysis
- Customer Lifetime Value analysis
- Advanced seasonal analysis
- Statistical hypothesis testing
- Customer clustering using Machine Learning
- Sales forecasting
- Interactive dashboards using Streamlit
- Predictive modeling

---

