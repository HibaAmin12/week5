# Week 05 Thursday — Integration EDA

## Overview

This project performs a complete Exploratory Data Analysis (EDA) pipeline on an intentionally messy orders dataset.

The workflow follows:

**Diagnose → Clean → Visualize → Summarize**

## Dataset

The dataset contains 5,000 generated orders with order ID, order date, customer ID, product category, quantity, unit price, and region.

Several data-quality issues were intentionally introduced, including missing values, inconsistent category labels, negative quantities, extreme unit prices, negative prices, and duplicate records.

## Data Cleaning

Each identified data-quality issue is handled separately with a justified cleaning decision.

The cleaning process includes:

- Handling missing customer IDs
- Handling missing regions
- Standardizing product category labels
- Handling negative quantities
- Handling extreme unit-price outliers
- Handling negative unit prices
- Removing duplicate records

## Notebook

The complete analysis is available in:

`week05_day4_eda.ipynb`

The notebook contains the diagnosis, cleaning process, visualizations, findings, and technical summary.

## Requirements

Install the required packages using:

```bash
pip install -r requirements.txt