# Day 2 — Pandas Data Cleaning & Analysis

## Overview

This project focuses on practical data cleaning and analysis using **Pandas**. The work is divided into two parts:

1. **Part 1:** A deliberately messy sales dataset created to practice common Pandas cleaning techniques
2. **Part 2:** The real-world **Online Retail Dataset** used to apply the same concepts to a larger transactional dataset

The overall workflow followed was:

**Inspect → Diagnose → Decide → Clean → Verify → Analyze**

---

## Project Objectives

- ✓ Create and inspect Pandas DataFrames
- ✓ Identify and handle missing values with context-based decisions
- ✓ Detect and remove duplicate records
- ✓ Handle invalid values (negative amounts, zero prices, etc.)
- ✓ Convert columns to appropriate data types
- ✓ Understand and apply `.loc` (label-based) and `.iloc` (position-based) indexing
- ✓ Apply Boolean filtering with multiple conditions
- ✓ Standardize categorical values (text cleaning)
- ✓ Perform `groupby()` analysis
- ✓ Understand vectorized operations vs `.apply()`
- ✓ Compare performance using `%timeit`
- ✓ Clean and analyze a real-world retail dataset

---

# Part 1 — Pandas Data Cleaning Practice

## Dataset Overview

A small sales dataset was created manually with intentional data-quality issues to practice cleaning techniques.

**Dataset size:** 8 rows × 4 columns

**Columns:**
- `date` – Transaction date
- `category` – Product category
- `amount` – Transaction amount
- `customer_age` – Customer age

---

## Dataset Diagnosis

### Initial Inspection Methods

The dataset was inspected using:

```python
df.head()          # View first 5 rows
df.info()          # Column names, data types, missing values
df.shape           # Dataset dimensions (8, 4)
df.describe()      # Statistical summary of numeric columns
df.isna().sum()    # Count missing values per column
```

### Missing Value Analysis

Initial missing values identified:

| Column | Missing Values | Strategy |
|--------|---|---|
| `date` | 1 | Row removed |
| `category` | 1 | Filled with "Unknown" |
| `amount` | 2 | Filled with median |
| `customer_age` | 2 | Filled with median |

```python
# Verification after cleaning
df.isna().sum()  # Returns 0 for all columns
```

---

## Cleaning Decisions & Implementation

### 1. Missing Data Handling

Different strategies were applied based on data meaning:

**Date Column:** Rows with missing dates were removed because a transaction without a date has no reliable context.
```python
df = df.dropna(subset=['date']).copy()
```

**Category Column:** Missing categories were filled with "Unknown" to retain transaction information.
```python
df['category'] = df['category'].fillna('Unknown')
```

**Amount & Customer Age:** Missing values were filled with the median (not mean) to minimize impact of outliers.
```python
df['amount'] = df['amount'].fillna(df['amount'].median())
df['customer_age'] = df['customer_age'].fillna(df['customer_age'].median())
```

### 2. Label-Based vs Position-Based Indexing

The difference between `.loc[]` and `.iloc[]` was demonstrated:

**`.loc[]` – Label-based indexing:**
```python
df.loc[2]  # Returns row with index label 2
```

**`.iloc[]` – Position-based indexing:**
```python
df.iloc[2]  # Returns row at position 2
```

**Important:** After sorting, row positions change but index labels are preserved. This distinction became crucial when analyzing data.

### 3. Boolean Filtering

Multiple conditions were applied using Pandas Boolean filtering:

```python
electronics_high_value = df[
    (df["amount"] > 200) &
    (df["category"] == "Electronics")
]
```

**Key operators:**
- `&` for AND (element-wise)
- `|` for OR (element-wise)
- Parentheses required around each condition
- Python's `and` operator causes ValueError in Pandas; element-wise operators are required

### 4. Category Standardization

Inconsistent capitalization identified:
```python
df["category"].value_counts()
```

Results showed:
- "Electronics", "electronics", "ELECTRONICS" (mixed cases)

Standardized using vectorized string operations:
```python
df["category"] = df["category"].str.strip().str.lower()
```

All values converted to lowercase and whitespace trimmed.

### 5. GroupBy Analysis

Category-level analysis performed:

**Mean Amount per Category:**
```python
df.groupby("category")["amount"].mean()
```

**Transaction Count per Category:**
```python
df.groupby("category").size()
```

**Total Amount per Category (Sorted):**
```python
df.groupby("category")["amount"].sum().sort_values(ascending=False)
```

This demonstrated the split → apply → combine concept of Pandas `groupby()`.

### 6. Vectorized Operations vs .apply()

**Task:** Calculate 10% increase on amounts

**Vectorized approach (FASTER):**
```python
large_df["amount"] * 1.10
```

**Using .apply() (SLOWER):**
```python
large_df["amount"].apply(lambda x: x * 1.10)
```

**Performance comparison:**
```python
%timeit large_df["amount"] * 1.10
%timeit large_df["amount"].apply(lambda x: x * 1.10)
```

**Result:** Vectorized operations were significantly faster. This demonstrated why vectorized Pandas/NumPy operations should be preferred when possible.

---

# Part 2 — Online Retail Dataset

## Dataset Overview

Real-world transactional dataset from an online retail business.

**Dataset size:** 541,909 rows → 524,878 rows (after cleaning)

**Columns:**
- `InvoiceNo` – Transaction invoice number
- `StockCode` – Product stock code
- `Description` – Product description
- `Quantity` – Units purchased
- `InvoiceDate` – Transaction date and time
- `UnitPrice` – Price per unit
- `CustomerID` – Customer identifier
- `Country` – Customer country

---

## Dataset Diagnosis

### Initial Inspection

```python
retail_df.head()      # First 5 rows
retail_df.shape       # (541909, 8)
retail_df.columns     # Column names
retail_df.info()      # Data types and missing values
retail_df.describe()  # Statistical summary
```

### Missing Value Analysis

Initial missing values identified:

```python
retail_df.isna().sum()

# Results:
# Description: 1,454
# CustomerID: 135,080
```

**Missing value percentages:**
```python
missing_percentage = (
    retail_df.isna().sum() / len(retail_df) * 100
)
missing_percentage.sort_values(ascending=False)
```

---

## Cleaning Decisions & Implementation

### 1. CustomerID Handling

**Finding:** 135,080 missing CustomerID values (24.9% of dataset)

**Decision:** Rows were NOT filled with artificial customer IDs because the actual customer cannot be reliably reconstructed.

**Rationale:** Transactions without customer IDs retain valuable information:
- Product details
- Quantity
- Date
- Price
- Country

These can still be used for product-level analysis.

**For customer-level analysis:**
```python
customer_df = retail_df.dropna(subset=["CustomerID"]).copy()
```

### 2. Duplicate Detection & Removal

```python
retail_df.duplicated().sum()  # Found: 5,268 duplicate records
```

Duplicates were inspected before removal to understand if they represented legitimate repeated orders or data entry errors.

**Removal:**
```python
retail_df = retail_df.drop_duplicates().copy()
retail_df.duplicated().sum()  # Verified: 0 duplicates
```

### 3. Quantity Cleaning

**Finding:** 10,587 negative quantity records

**Investigation:** Large portion associated with cancellation invoices (InvoiceNo starting with 'C').

**Decision:** For sales analysis, negative quantities excluded:
```python
retail_df = retail_df[retail_df["Quantity"] > 0].copy()
```

This ensures the cleaned dataset contains only positive sales quantities.

### 4. UnitPrice Cleaning

**Finding:** 2,512 records with zero or negative unit prices

**Verification:**
```python
(retail_df["UnitPrice"] <= 0).sum()  # Identified: 2,512
```

**Removal:**
```python
retail_df = retail_df[retail_df["UnitPrice"] > 0].copy()

# Verification after cleaning:
(retail_df["UnitPrice"] <= 0).sum()  # Result: 0
```

### 5. InvoiceDate Data Type Conversion

Original format: Object/String (stored as text)

**Conversion to datetime:**
```python
retail_df["InvoiceDate"] = pd.to_datetime(retail_df["InvoiceDate"])
```

**Benefits:** Enables date-based analysis:
- Monthly sales trends
- Daily sales patterns
- Yearly comparisons
- Time-based grouping

### 6. Sales Calculation

**New column created:** TotalSales

**Formula:** `TotalSales = Quantity × UnitPrice`

```python
retail_df["TotalSales"] = (
    retail_df["Quantity"] * retail_df["UnitPrice"]
)
```

**Overall metrics calculated:**
```python
total_sales = retail_df["TotalSales"].sum()
average_transaction = retail_df["TotalSales"].mean()
```

---

## Final Data Validation

### Validation Checks

After cleaning, comprehensive validation performed:

```python
retail_df.info()                        # Data types and null values
retail_df.isna().sum()                  # Missing value count
(retail_df["Quantity"] < 0).sum()       # Negative quantities check
(retail_df["UnitPrice"] <= 0).sum()     # Invalid prices check
retail_df.duplicated().sum()            # Duplicate records check
retail_df.describe()                    # Statistical summary
```

### Final Dataset Summary

| Check | Before Cleaning | After Cleaning |
|-------|---|---|
| **Total Rows** | 541,909 | 524,878 |
| **Columns** | 8 | 8 |
| **Duplicate Records** | 5,268 | 0 |
| **Negative Quantities** | 10,587 | 0 |
| **Zero/Negative Prices** | 2,512 | 0 |
| **Missing Description** | 1,454 | 0 |
| **Missing CustomerID** | 135,080 | 132,186* |
| **InvoiceDate Type** | object | datetime64[ns] |

*Intentionally retained because cannot be reliably reconstructed.

---

## Technologies Used

- **Python 3.x**
- **Pandas** – Data manipulation and cleaning
- **NumPy** – Numerical operations
- **Jupyter Notebook** – Interactive development environment

---

## Key Pandas Concepts Practiced

### DataFrame Methods
- `head()` – View first rows
- `info()` – Dataset information
- `shape` – Dimensions
- `describe()` – Statistical summary
- `isna()` – Identify missing values

### Data Cleaning
- `dropna()` – Remove rows with missing values
- `fillna()` – Fill missing values
- `duplicated()` – Identify duplicates
- `drop_duplicates()` – Remove duplicates

### Indexing
- `.loc[]` – Label-based indexing
- `.iloc[]` – Position-based indexing

### Filtering & Analysis
- Boolean filtering with `&`, `|`
- `value_counts()` – Frequency analysis
- `groupby()` – Group-level analysis
- Column creation and calculation

### Type Conversion
- `pd.to_datetime()` – Convert to datetime

### Performance
- Vectorized operations
- `.apply()` with lambda functions
- `%timeit` – Performance measurement

---

## Project Structure

```
Day2_task/
│
├── data/
│   └── Online Retail.xlsx
      └── Online Retail.csv 
│
├── Pandas_Data_Cleaning_Practice.ipynb
│
└── README.md
```

**Note:** If dataset was converted to CSV, update path accordingly: `data/Online_Retail.csv`

---

## Key Learning Outcomes

### 1. Context-Based Decision Making
Real-world data cleaning requires understanding the data's meaning. Not all missing or unusual values should be removed blindly.

### 2. Workflow Mastery
The systematic approach was demonstrated:
```
Inspect → Diagnose → Decide → Clean → Verify → Analyze
```

### 3. Practical Pandas Skills
- Multiple methods to handle same problem (different strategies for different data types)
- Importance of verification after each cleaning step
- Balance between data loss and data quality

### 4. Real-World Applicability
Working with an actual retail dataset showed:
- Complexity of real-world data
- Need for domain knowledge in decision-making
- Importance of documentation
- Data validation as continuous process

### 5. Performance Awareness
Understanding vectorized vs iterative operations is crucial for working with large datasets.

---

## Conclusion

This project demonstrated a complete Pandas data-cleaning workflow from raw, deliberately messy data to a validated dataset ready for analysis. The progression from a small practice dataset to a real-world dataset with 500,000+ rows showed how the same principles scale to production-level work.

**Final Insight:** Data cleaning is not a single step—it's an iterative process of inspection, decision-making, implementation, and verification that requires both technical skills and domain understanding.