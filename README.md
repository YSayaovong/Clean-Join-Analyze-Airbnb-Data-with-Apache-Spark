# Clean · Join · Analyze Airbnb Data with Apache Spark

[![Apache Spark 3.x](https://img.shields.io/badge/Spark-3.x-blue.svg)](https://spark.apache.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 1 · Executive Summary
This repository demonstrates how to turn the raw *Inside Airbnb* listings and reviews into actionable intelligence using Apache Spark.  
You will:

1. **Clean** noisy CSV and JSON files at scale.  
2. **Join** heterogeneous datasets into a unified analytics model.  
3. **Analyze** supply, demand, and pricing trends that matter to investors, hosts, and city planners.  
4. **Submit & tune** Spark applications like a professional, with repeatable job scripts and automated tests.

---

## 2 · Dataset
*Inside Airbnb* publishes snapshot files for 100+ cities worldwide.  
Download the latest data set that interests you from `http://insideairbnb.com/get-the-data.html` and place the files in `/data/raw`.

| File | Description |
|------|-------------|
| `listings.csv.gz` | Listing-level facts (price, host_id, neighbourhood, room_type, etc.). |
| `reviews.csv.gz`  | Guest reviews with dates and comments. |
| `calendar.csv.gz` | Daily availability & pricing (optional for advanced capacity analysis). |

---

## 3 · Architecture

* **Language**: PySpark 3.x (Scala job templates included for flexibility).  
* **Deployment**: Stand-alone, YARN, or Kubernetes via Docker Compose.  
* **Testing**: `pytest` with Spark Local [2] for CI safety.  
* **Packaging**: Fat JAR or ZIP via `setup.py` for frictionless cluster submission.

---

## 4 · Quick Start

### 4.1 Prerequisites
* Docker 20+ **or** Python 3.10 + Java 11 + Apache Spark 3.5.0
* GNU Make (optional, for one-touch commands)

### 4.2 Clone & Set Up

git clone https://github.com/YSayaovong/Clean-Join-Analyze-Airbnb-Data-with-Apache-Spark.git
cd Clean-Join-Analyze-Airbnb-Data-with-Apache-Spark
make dev   # spins up Spark-Master, Spark-Worker, and Jupyter Lab

### 4.3 Run the Pipeline

###### 1. Clean listings and reviews
spark-submit jobs/clean_listings.py  --input data/raw/listings.csv.gz \
                                     --output data/bronze/listings

spark-submit jobs/clean_reviews.py   --input data/raw/reviews.csv.gz  \
                                     --output data/bronze/reviews

###### 2. Join into a gold-level fact table
spark-submit jobs/join_datasets.py   --listings data/bronze/listings \
                                     --reviews  data/bronze/reviews  \
                                     --output   data/gold/airbnb

###### 3. Execute analytic queries
spark-submit jobs/analyze_airbnb.py  --airbnb  data/gold/airbnb      \
                                     --report  output/analysis.parquet

### 4.4 Verify Results
Open notebooks/exploratory-analysis.ipynb in Jupyter Lab to visualise key metrics such as ADR, occupancy, and review sentiment.

### 5 · Key Analyses Delivered
Question	Metric Produced	Business Value
Which neighbourhoods command the highest ADR?	avg_price by neighbourhood	Guides capital allocation and dynamic pricing.
How does seasonality affect occupancy?	occupancy_rate by month	Informs revenue-management strategies.
Do longer reviews correlate with higher ratings?	Correlation len(review) vs rating	Improves host engagement tactics.
What is the growth trajectory of entire-home listings?	YoY % change	Signals market saturation or opportunity.

### 6 · Performance Tuning Tips
Partitioning – Repartition by city & year to minimise shuffles.

Broadcast Joins – Enable for lookup dimensions under 10 MB.

Caching – Cache the gold table during iterative notebook analysis.

Executor Memory – Start at 4 G & scale after inspecting spark-ui.

### 7 · Roadmap
Delta Lake support for ACID & time-travel.

MLlib pipeline for revenue forecasting.

Real-time stream with Structured Streaming + Airbnb calendar feed.

### 8 · Contributing
Pull requests are welcomed if they maintain code quality and business-first focus.
Please open an issue before making substantial changes.

### 9 · License
This project is licensed under the MIT License. See LICENSE for details.

### 10 · Contact
GitHub Issues preferred.
For professional inquiries: Yengkong Sayaovong – ysayaovong@gmail.com
