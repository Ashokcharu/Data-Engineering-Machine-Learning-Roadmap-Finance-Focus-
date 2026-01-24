# 🚀 PySpark From Zero — Complete Explained Tutorial

> **Goal:** Learn PySpark from absolute basics to production-ready understanding — with *why*, *how*, and *hands-on practice*.

This tutorial is written to be:

* 📖 Readable like a book
* 🧠 Concept-first (not copy–paste)
* 🛠 Practice-oriented
* 💼 Data Engineering & Databricks ready

---

## 📌 Table of Contents

1. What Problem Spark Solves
2. Spark Architecture (Driver, Executor, Cluster)
3. Why SparkSession Exists
4. Spark DataFrames (Core Chapter)
5. Lazy Execution & DAG
6. Reading & Writing Data
7. Column Operations & Feature Engineering
8. Filtering, Sorting, Deduplication
9. GroupBy, Aggregations & Shuffles
10. Window Functions (Time-Series)
11. Joins & Real ETL Patterns
12. Performance Basics (Partitions, Cache)
13. Delta Lake Fundamentals
14. Final Project: End-to-End ETL Pipeline

---

## 1️⃣ What Problem Spark Solves

### ❌ Limitations of Pandas / Traditional Python

* Runs on **one machine**
* Limited by **RAM**
* Slow for GB–TB scale data
* No fault tolerance

### ✅ Spark Solution

* Distributes data across machines
* Processes data **in parallel**
* Recovers from failures
* Optimizes execution automatically

> 📌 Spark is not "fast Python" — it is a **distributed execution engine**.

---

## 2️⃣ Spark Architecture (Very Important)

### Core Components

| Component | Explanation              |
| --------- | ------------------------ |
| Driver    | Your main Python program |
| Executor  | Worker that runs tasks   |
| Task      | Small unit of work       |
| Partition | Chunk of data            |
| DAG       | Optimized execution plan |

### Mental Model

> You write code in **Driver**, data is processed in **Executors**.

---

## 3️⃣ Why SparkSession Exists

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("PySparkTutorial") \
    .getOrCreate()
```

### Why Spark Needs It

SparkSession:

* Connects to cluster
* Manages memory & executors
* Enables DataFrame & SQL APIs
* Holds configuration

❌ Without SparkSession → Spark cannot run

`getOrCreate()` ensures:

* One shared session
* Safe resource usage

---

## 4️⃣ Spark DataFrames (CORE CHAPTER)

### Why DataFrame Exists

Python objects **cannot**:

* Be optimized
* Be distributed safely
* Track lineage

Spark DataFrame provides:

* Schema
* Distribution
* Immutability
* Optimization

> DataFrame = Data + Schema + Execution Plan

---

### Creating a DataFrame

```python
data = [
    (1, "BTC", 45000),
    (2, "ETH", 3200),
    (3, "BNB", 410)
]

columns = ["id", "symbol", "price"]

df = spark.createDataFrame(data, columns)
```

```python
df.show()
df.printSchema()
```

### What Happens Internally

* Schema inferred
* Data split into partitions
* Logical plan created
* ❌ No execution yet

---

## 5️⃣ Lazy Execution & DAG

### Transformation (Lazy)

```python
filtered_df = df.filter(df.price > 1000)
```

### Action (Triggers execution)

```python
filtered_df.show()
filtered_df.count()
```

### Why Lazy Execution?

* Combines operations
* Reduces data movement
* Optimizes execution

---

## 6️⃣ Reading & Writing Data

### Reading CSV

```python
df = spark.read \
    .option("header", True) \
    .option("inferSchema", True) \
    .csv("data/market.csv")
```

### Writing Parquet

```python
df.write.mode("overwrite").parquet("output/market")
```

📌 Parquet = columnar + compressed

---

## 7️⃣ Column Operations & Feature Engineering

```python
from pyspark.sql.functions import col, when

df = df.withColumn("price_inr", col("price") * 83)

df = df.withColumn(
    "signal",
    when(col("price") > 1000, "HIGH").otherwise("LOW")
)
```

### Why `withColumn`?

* DataFrames are immutable
* Enables fault tolerance

---

## 8️⃣ Filtering, Sorting, Deduplication

```python
df.filter(df.price > 1000)
df.orderBy(df.price.desc())
df.dropDuplicates(["symbol"])
```

---

## 9️⃣ GroupBy, Aggregations & Shuffles

```python
from pyspark.sql.functions import avg, sum

df.groupBy("symbol").agg(
    avg("price").alias("avg_price"),
    sum("price").alias("total_price")
)
```

⚠️ groupBy causes **shuffle** (network movement)

---

## 🔟 Window Functions (Time-Series)

```python
from pyspark.sql.window import Window
from pyspark.sql.functions import row_number

windowSpec = Window.partitionBy("symbol").orderBy(col("price").desc())

df.withColumn("rank", row_number().over(windowSpec))
```

📌 Window ≠ groupBy

---

## 1️⃣1️⃣ Joins & ETL Patterns

```python
df_orders.join(df_users, on="user_id", how="left")
```

Join Types:

* inner
* left
* right
* full

---

## 1️⃣2️⃣ Performance Basics

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

---

## 1️⃣3️⃣ Delta Lake Fundamentals

```python
df.write.format("delta").mode("overwrite").save("/delta/market")
```

Benefits:

* ACID transactions
* Time travel
* Schema enforcement

---

## 1️⃣4️⃣ Final Project — End-to-End ETL

### Build This Pipeline

1. Read raw CSV (Bronze)
2. Clean & validate (Silver)
3. Aggregate features (Gold)
4. Save as Delta

This mirrors **real Databricks jobs**.

---

## 🎯 What You Achieve After This

✔ Strong Spark fundamentals
✔ Clear mental model
✔ Interview-ready explanations
✔ Production ETL confidence

---

## ⭐ Next Extensions

* Spark SQL Deep Dive
* PySpark ML
* MLflow + Databricks Jobs
* Streaming with Spark

---

📌 **Tip:** Read → type → break → fix → understand.

Happy Learning 🚀
