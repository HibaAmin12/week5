# Week 5 — Day 1: NumPy Fundamentals

## 📋 Overview

This project builds a practical foundation in **NumPy** and **JupyterLab** for numerical computing and AI/ML workflows. The work is implemented in a structured Jupyter notebook with explanatory Markdown, reproducible code, and hands-on experiments.

The focus is not just on performing NumPy operations, but on **understanding why** each operation is used, how array shapes change, and how NumPy improves numerical computation compared to standard Python lists.

---

## 🎯 Learning Objectives

By completing this task, you will practice:

- ✅ Working with JupyterLab inside a dedicated Python virtual environment
- ✅ Structuring notebooks using Markdown and code cells
- ✅ Understanding Jupyter kernel state and reproducibility
- ✅ Creating and inspecting NumPy arrays
- ✅ Comparing Python list operations with NumPy vectorized operations
- ✅ Performing 2D indexing and slicing
- ✅ Applying Boolean masking for data filtering
- ✅ Understanding broadcasting mechanics
- ✅ Performing mean-centering as a preprocessing operation
- ✅ Applying aggregation functions along different axes
- ✅ Understanding reshape behavior and memory sharing
- ✅ Using Jupyter/IPython tools for interactive exploration
- ✅ Building clean, reproducible notebook workflows

---

## 🔧 Environment

This project requires the following setup:

| Component | Version |
|-----------|---------|
| Python | 3.10.12 |
| NumPy | 2.2.6 |
| JupyterLab | 4.6.3 |
| IPython | 8.39.0 |

### Environment Verification

The notebook includes cells to verify:
- Python executable path
- Python version
- NumPy version

This ensures experiments run in the intended environment.

---

## 📁 Project Structure

```
Day1_task/
│
├── week05_day1.ipynb        # Main Jupyter notebook
├── README.md                 # This file
├── requirements.txt          # Python dependencies
└── .gitignore               # Git ignore rules
```

---

## 📚 Key Concepts Covered

### 1. **NumPy Array Creation**
Creating arrays from Python lists and using NumPy functions (`zeros`, `ones`, `arange`, `linspace`).

### 2. **Array Inspection**
Exploring array properties:
- `shape` — dimensions of the array
- `dtype` — data type of elements
- `size` — total number of elements
- `ndim` — number of dimensions

### 3. **Vectorized Operations**
Comparing performance:
```python
# Python loop (slow)
result = [x * 2 for x in data]

# NumPy vectorized (fast)
result = data * 2
```

### 4. **Indexing and Slicing**
Precise control over subsets:
```python
array[0]          # single element
array[1:3]        # slice
array[0, 1]       # 2D indexing
```

### 5. **Boolean Masking**
Filtering based on conditions:
```python
mask = data > 0.5
filtered_values = data[mask]
```

### 6. **Broadcasting**
Operating on arrays of different shapes:
```python
column_means = data.mean(axis=0)
centered_data = data - column_means  # broadcasts automatically
```

### 7. **Axis-Based Aggregation**
Understanding dimensional reduction:
- `axis=0` → aggregate across rows → one result per column
- `axis=1` → aggregate across columns → one result per row

### 8. **Reshape and Memory Sharing**
Reorganizing array elements while understanding view vs. copy behavior.

### 9. **Interactive Documentation**
Exploring NumPy functions directly in JupyterLab using:
- `Shift + Tab` — quick documentation
- `function?` — full documentation

---

## 🚀 Quick Start

### Prerequisites

- Python 3.10 or higher
- Virtual environment (recommended)
- pip or conda

### Setup Instructions

1. **Clone or navigate to the project directory:**
   ```bash
   cd Day1_task
   ```

2. **Create a virtual environment:**
   ```bash
   python -m venv .venv
   ```

3. **Activate the virtual environment:**
   ```bash
   # On macOS/Linux:
   source .venv/bin/activate
   
   # On Windows:
   .venv\Scripts\activate
   ```

4. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

5. **Launch JupyterLab:**
   ```bash
   jupyter lab
   ```

6. **Open the notebook:**
   - Navigate to `week05_day1.ipynb` in the file browser

---

## ✅ Validation & Quality Checks

Before considering the notebook complete, validate it from a **fresh kernel**:

### Step-by-Step Validation

1. **Restart the Jupyter kernel:**
   - Click `Kernel` → `Restart Kernel`

2. **Run all notebook cells from top to bottom:**
   - Click `Run` → `Run All Cells`

3. **Confirm execution:**
   - ✓ All cells execute without errors
   - ✓ Variables are defined before use
   - ✓ No dependency on previously executed hidden state

### Running Validation from Command Line

```bash
# Restart kernel and run all cells
jupyter nbconvert --to notebook --execute --inplace week05_day1.ipynb
```

---

## 📊 Example Workflows

### Creating Arrays
```python
import numpy as np

# From Python list
arr = np.array([1, 2, 3, 4, 5])

# Using numpy functions
zeros = np.zeros((3, 3))
ones = np.ones((2, 4))
range_arr = np.arange(0, 10, 2)
```

### Mean-Centering (Preprocessing)
```python
data = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
column_means = data.mean(axis=0)
centered_data = data - column_means

# Verify: new means should be ≈ 0
print(centered_data.mean(axis=0))
```

### Boolean Masking
```python
data = np.array([1, 2, 3, 4, 5])
mask = data > 2
filtered = data[mask]  # Returns [3, 4, 5]
```

### Performance Comparison with `%timeit`
```python
# In Jupyter cell:
%timeit [x * 2 for x in range(1000)]
%timeit np.arange(1000) * 2
```

---

## 💡 Key Takeaways

Through this task, you develop practical understanding of:

- NumPy array creation and inspection
- Array shapes and dimensional analysis
- Vectorized numerical operations
- Performance optimization using `%timeit`
- 2D indexing and advanced slicing
- Boolean masking for data filtering
- Broadcasting mechanics
- Mean-centering and data preprocessing
- Axis-based aggregation (axis=0, axis=1)
- Reshaping and view vs. copy behavior
- Jupyter interactive documentation (`?`, `Shift+Tab`)
- Notebook reproducibility and kernel state management

These concepts form the **foundation for AI/ML work**, particularly for:
- Numerical preprocessing
- Feature manipulation
- Efficient data operations
- Dataset preparation

