# 🐼 COMPLETE PANDAS TUTORIAL (START → FINISH)

Pandas is a powerful Python library used for working with structured data such as tables, spreadsheets, and time-series data. It provides easy-to-use data structures like DataFrame and Series that allow you to load, clean, transform, analyze, and summarize data efficiently. Pandas is widely used for data analysis, data cleaning, feature engineering for machine learning, and time-series processing. It helps handle missing values, filter and aggregate data, perform fast vectorized operations, and prepare datasets for visualization or ML models. In short, Pandas makes raw data easy to understand, modify, and convert into insights.

🧠 Pandas in One Line

Pandas turns raw data into clean, usable data using simple Python code.

---

## 📘 CHAPTER 0: Setup (Do This First)

```python
import pandas as pd
import numpy as np
```

Check version:

```python
pd.__version__
```

---

## 📘 CHAPTER 1: What is Pandas?

* Pandas is for **tabular data**
* Two main objects:

  * **Series** → 1D (column)
  * **DataFrame** → 2D (table)

---

## 📘 CHAPTER 2: Series (Basics)

### Create a Series

```python
s = pd.Series([100, 102, 101, 105])
print(s)
```

### Series operations

```python
s.mean()
s.max()
s.min()
```

### Indexing

```python
s[0]
s[1:3]
```

### 📝 Practice

* Create a Series of 10 numbers
* Find mean and max

---

## 📘 CHAPTER 3: DataFrame (Core Concept)

### Create DataFrame

```python
df = pd.DataFrame({
    "price": [100, 102, 101, 105],
    "volume": [1200, 1500, 1100, 1800]
})
```

### Inspect DataFrame

```python
df.head()
df.tail()
df.shape
df.columns
df.dtypes
df.info()
df.describe()
```

### 📝 Practice

* Add one more column: `symbol`
* Print column names

---

## 📘 CHAPTER 4: Reading & Writing Files

### Read CSV

```python
df = pd.read_csv("data.csv")
```

### Save CSV

```python
df.to_csv("output.csv", index=False)
```

### 📝 Practice

* Load any CSV
* Save first 10 rows to a new file

---

## 📘 CHAPTER 5: Selecting Data (VERY IMPORTANT)

### Select columns

```python
df["price"]
df[["price", "volume"]]
```

### Select rows

```python
df.iloc[0]
df.iloc[0:3]
df.loc[df["price"] > 100]
```

### Combined filter

```python
df[(df["price"] > 100) & (df["volume"] > 1200)]
```

### 📝 Practice

* Filter rows with price > 100 and volume > 1300

---

## 📘 CHAPTER 6: Data Cleaning

### Missing values

```python
df.isna().sum()
df.dropna()
df.fillna(method="ffill")
```

### Remove duplicates

```python
df.drop_duplicates()
```

### Change data type

```python
df["price"] = df["price"].astype(float)
```

### 📝 Practice

* Create missing values and clean them

---

## 📘 CHAPTER 7: Creating New Columns (FEATURE ENGINEERING)

### Percentage return

```python
df["return"] = df["price"].pct_change()
```

### Log transform

```python
df["log_price"] = np.log(df["price"])
```

### Conditional column

```python
df["direction"] = np.where(df["return"] > 0, 1, 0)
```

### 📝 Practice

* Create 3 new columns from price

---

## 📘 CHAPTER 8: Apply, Map, Lambda

```python
df["price_scaled"] = df["price"].apply(lambda x: x / df["price"].max())
```

⚠️ Use `apply` only when vectorization is not possible.

### 📝 Practice

* Create a column classifying price as HIGH / LOW

---

## 📘 CHAPTER 9: GroupBy & Aggregation

### GroupBy

```python
df.groupby("symbol")["price"].mean()
```

### Multiple aggregations

```python
df.groupby("symbol").agg({
    "price": ["mean", "max"],
    "volume": "sum"
})
```

### 📝 Practice

* Group by symbol and calculate average volume

---

## 📘 CHAPTER 10: Sorting & Ranking

```python
df.sort_values("price", ascending=False)
df["rank"] = df["price"].rank(ascending=False)
```

### 📝 Practice

* Rank rows by volume

---

## 📘 CHAPTER 11: Time-Series (CRITICAL FOR YOU)

### Convert to datetime

```python
df["time"] = pd.to_datetime(df["time"])
df.set_index("time", inplace=True)
```

### Resample

```python
df_5m = df.resample("5T").agg({
    "price": "last",
    "volume": "sum"
})
```

### Rolling window

```python
df["ma_5"] = df["price"].rolling(5).mean()
```

### 📝 Practice

* Convert 1-min data into 5-min candles

---

## 📘 CHAPTER 12: Performance Best Practices

### Vectorized (FAST)

```python
df["fast"] = df["price"] * 1.01
```

### Avoid loops ❌

```python
# slow
for i in range(len(df)):
    df.loc[i, "slow"] = df.loc[i, "price"] * 1.01
```

---

## 📘 FINAL PROJECT (MANDATORY)

### 🎯 Project: Financial Feature Engineering

**Build this notebook:**

1. Load candle data
2. Clean missing values
3. Create features:

   * returns
   * rolling mean
   * volatility
4. Save ML-ready dataset

📌 **This notebook becomes your GitHub proof.**

---

## ✅ AFTER THIS TUTORIAL, YOU CAN:

✔ Handle real datasets
✔ Build ML-ready features
✔ Move confidently to **PySpark**
✔ Understand financial time-series data

---



