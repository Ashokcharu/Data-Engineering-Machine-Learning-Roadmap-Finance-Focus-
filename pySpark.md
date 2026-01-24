# ⚡ PYSPARK COMPLETE TUTORIAL (ZERO → JOB-READY)

> **Goal:**
> By the end of this tutorial, you will confidently:

* Process **large datasets**
* Build **ETL pipelines**
* Understand **how Spark actually works**
* Be ready for **Databricks & Data Engineering interviews**

---

## 📘 CHAPTER 0: What Problem PySpark Solves (READ THIS)

### Why not Pandas?

* Pandas → single machine, limited memory
* PySpark → distributed, scalable, fault-tolerant

### When to use PySpark?

| Data Size | Tool         |
| --------- | ------------ |
| < 1 GB    | Pandas       |
| 1–50 GB   | PySpark      |
| 50 GB+    | PySpark only |

---

## 📘 CHAPTER 1: Spark Architecture (VERY IMPORTANT)

### Spark Components (Simple Explanation)

| Component       | What it does        |
| --------------- | ------------------- |
| Driver          | Your main program   |
| Cluster Manager | Allocates resources |
| Executor        | Runs tasks          |
| Partition       | Small piece of data |
| DAG             | Execution plan      |

📌 **Key Rule:**
Spark works in **parallel**, not line by line like Python.

---

## 📘 CHAPTER 2: Setting Up Spark

### Create Spark Session

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("ClearPySparkTutorial") \
    .getOrCreate()
```

Check Spark:

```python
spark
```

---

## 📘 CHAPTER 3: Your First DataFrame

### Create DataFrame

```python
data = [
    (1, "BTC", 45000),
    (2, "ETH", 3200),
    (3, "BNB", 410)
]

columns = ["id", "symbol", "price"]

df = spark.createDataFrame(data, columns)
```

### View Data

```python
df.show()
df.printSchema()
```

📌 **Understand:**
Spark DataFrame ≠ Pandas DataFrame
Spark is **lazy**.

### 📝 Practice

* Change price
* Add one more row

---

## 📘 CHAPTER 4: Lazy Execution (CORE CONCEPT)

### Transformation (No execution)

```python
df2 = df.filter(df.price > 1000)
```

### Action (Triggers execution)

```python
df2.show()
df2.count()
```

📌 **Remember forever:**

> **No ACTION → No Execution**

---

## 📘 CHAPTER 5: Reading Real Data

### Read CSV

```python
df = spark.read \
    .option("header", True) \
    .option("inferSchema", True) \
    .csv("market.csv")
```

### Always check schema

```python
df.printSchema()
```

📌 **Why schema matters:**
Wrong types = slow jobs + wrong results

---

## 📘 CHAPTER 6: Selecting & Filtering (Daily Use)

```python
df.select("symbol", "price").show()
df.filter(df.price > 1000).show()
df.filter((df.price > 1000) & (df.symbol == "BTC")).show()
```

📝 Practice:

* Filter ETH
* Select symbol + price

---

## 📘 CHAPTER 7: Adding Columns (Feature Engineering)

```python
from pyspark.sql.functions import col, when

df = df.withColumn("price_usd", col("price") * 1.1)

df = df.withColumn(
    "signal",
    when(col("price") > 1000, "HIGH").otherwise("LOW")
)
```

📌 Spark is **immutable** → always reassign.

---

## 📘 CHAPTER 8: Handling Missing Data

```python
df.dropna()
df.fillna({"price": 0})
```

📌 Missing data kills pipelines if ignored.

---

## 📘 CHAPTER 9: GroupBy & Aggregation (BIG DATA SKILL)

```python
from pyspark.sql.functions import avg, max

df.groupBy("symbol").agg(
    avg("price").alias("avg_price"),
    max("price").alias("max_price")
).show()
```

📝 Practice:

* Sum volume per symbol

---

## 📘 CHAPTER 10: Window Functions (VERY IMPORTANT)

```python
from pyspark.sql.window import Window
from pyspark.sql.functions import row_number

windowSpec = Window.partitionBy("symbol").orderBy(col("price").desc())

df.withColumn("rank", row_number().over(windowSpec)).show()
```

📌 Used in ranking, deduplication, time-series.

---

## 📘 CHAPTER 11: Joins (REAL-WORLD ETL)

```python
df_orders.join(df_users, on="user_id", how="left")
```

Join types:

* inner
* left
* right
* full

---

## 📘 CHAPTER 12: Time-Series with Spark

```python
from pyspark.sql.functions import window

df.groupBy(
    window("time", "5 minutes"),
    "symbol"
).avg("price").show()
```

📌 This replaces Pandas resample at scale.

---

## 📘 CHAPTER 13: Performance Basics

### Partitions

```python
df.rdd.getNumPartitions()
df = df.repartition(4)
```

### Cache

```python
df.cache()
df.count()
```

📌 Cache only reused data.

---

## 📘 CHAPTER 14: Delta Lake (MODERN DATA ENGINEERING)

```python
df.write.format("delta").mode("overwrite").save("/delta/market")
spark.read.format("delta").load("/delta/market")
```

Benefits:

* ACID
* Time travel
* Faster reads

---

## 📘 FINAL PROJECT (DON’T SKIP)

### 🏗️ Build: Market ETL Pipeline

1. Read raw CSV (Bronze)
2. Clean & validate (Silver)
3. Aggregate features (Gold)
4. Save as Delta

📌 This is **real Data Engineering work**.

---

## ✅ AFTER THIS TUTORIAL, YOU WILL:

✔ Truly understand PySpark
✔ Build scalable ETL jobs
✔ Pass PySpark interviews
✔ Be Databricks-ready

---
