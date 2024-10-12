# Library Branches Data Analysis

This project involves analyzing data from a CSV file containing information about library branches. The analysis includes checking for duplicates, identifying outliers, mapping locations, and comparing various metrics like inventory size and population.

## Files

- **`Library.csv`**: The dataset used for this analysis.
- **`Library_Branches_Analysis.ipynb`**: Jupyter notebook containing the full analysis.

---

## Table of Contents

1. [Checking Data Types](#1-checking-data-types)
2. [Finding Duplicate Rows](#2-finding-duplicate-rows)
3. [Understanding Data Structure](#3-understanding-data-structure)
4. [Viewing First Few Rows](#4-viewing-first-few-rows)
5. [Displaying Summary Statistics](#5-displaying-summary-statistics)
6. [Viewing Column Names](#6-viewing-column-names)
7. [Displaying Libraries with Inventory Larger than 1 Lakh](#7-displaying-libraries-with-inventory-larger-than-1-lakh)
8. [Identifying Missing Values](#8-identifying-missing-values)
9. [Finding and Removing Outliers](#9-finding-and-removing-outliers)
10. [Distribution of Square Feet Across Branches](#10-distribution-of-square-feet-across-branches)
11. [Mapping Library Branches](#11-mapping-library-branches)
12. [Library Inventory Size per City](#12-library-inventory-size-per-city)
13. [Comparison of Population and Inventory Sizes per City](#13-comparison-of-population-and-inventory-sizes-per-city)
14. [Libraries with Access to Metro and Bus Services](#14-libraries-with-access-to-metro-and-bus-services)

---

### 1. Checking Data Types

We check the data types of the columns to ensure they are suitable for the analysis.

```python
df.dtypes
```

### 2. Finding Duplicate Rows

To ensure data quality, we identify any duplicate rows in the dataset.

```python
duplicates = df[df.duplicated()]
duplicates
```

### 3. Understanding Data Structure

We inspect the structure of the dataframe, including shape, summary statistics, and column names.

```python
df.info()
df.describe()
df.shape
df.columns
```

### 4. Viewing First Few Rows

We view the first five rows of the dataset for a quick look at the data.

```python
df.head(5)
```

### 5. Displaying Summary Statistics

Summary statistics for numerical columns are displayed to better understand the distribution of the data.

```python
df.describe()
```

### 6. Viewing Column Names

We list all column names in the dataframe.

```python
df.columns
```

### 7. Displaying Libraries with Inventory Larger than 1 Lakh

This query filters the data to show libraries with inventory sizes greater than 100,000 items.

```python
df['Inventory'] = pd.to_numeric(df['Inventory'], errors='coerce')
df.loc[df['Inventory'] > 100000].head(5)
```

### 8. Identifying Missing Values

We identify missing values across the dataset to assess data completeness.

```python
df.isnull()
```

### 9. Finding and Removing Outliers

Using z-scores, we identify and remove outliers from the dataset.

```python
from scipy.stats import zscore
zscore = zscore(df.select_dtypes(include=['float64', 'int64']))
df_outliers = df[(zscore > 3).any(axis=1) | (zscore < -3).any(axis=1)]
print(f'No of outliers: {df_outliers.shape[0]}')
```

### 10. Distribution of Square Feet Across Branches

We visualize the distribution of square footage across library branches.

```python
import matplotlib.pyplot as plt
plt.figure(figsize=(10, 6))
plt.title("Distribution of Sq Ft Across Branches")
plt.xlabel("Area in sq ft")
plt.ylabel("Frequency")
plt.hist(df['Square Feet'], bins=df.shape[0], edgecolor='k')
plt.show()
```

### 11. Mapping Library Branches

We plot library branch locations using latitude and longitude coordinates.

```python
import plotly.express as px
fig = px.scatter_geo(df, lat='Latitude', lon='Longitude', hover_name='Branch',
                     title='Library Branch Locations', projection="albers usa")
fig.update_geos(landcolor='lightgreen')
fig.show()
```

### 12. Library Inventory Size per City

This query generates a bar chart to visualize the inventory size across different cities.

```python
fig = px.bar(df, x='City', y='Inventory',
             title='Library Inventory per City', labels={'City': 'City', 'Inventory': 'Inventory'},
             hover_data=['Inventory'])
fig.update_traces(marker_color='lightgreen')
fig.show()
```

### 13. Comparison of Population and Inventory Sizes per City

We compare the population sizes and inventory sizes across cities using a grouped bar chart.

```python
df_melted = df.melt(id_vars='City', value_vars=['Population Size', 'Inventory'], var_name='Category', value_name='Size')

fig = px.bar(df_melted, x='City', y='Size', color='Category', barmode='group',
             title='Comparison of Population and Inventory Sizes per City')
fig.show()
```

### 14. Libraries with Access to Metro and Bus Services

This analysis counts the number of libraries with access to metro stations and bus routes and displays it as a bar chart.

```python
transport = ['Metro Station', 'Bus 4', 'Bus 5', 'Bus 6', 'Bus 7', 'Bus 9', 'Bus 12']
metro_bus_counts = df[transport].sum()

fig = px.bar(x=metro_bus_counts.index, y=metro_bus_counts.values, 
             title="Availability of Metro and Bus Services to Libraries",
             labels={'x': 'Transport Options', 'y': 'Count of Branches'})
fig.show()
```

---

## Installation and Requirements

- Python 3.x
- Required packages: `pandas`, `matplotlib`, `plotly`, `scipy`

You can install the necessary libraries using the following command:

```bash
pip install pandas matplotlib plotly scipy
```

---

## Usage

1. Clone the repository.
2. Load the `Library.csv` file into the Jupyter notebook.
3. Run the code cells to analyze the data.
4. Visualize the outputs, which include charts and maps.

---
