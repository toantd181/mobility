# Live Traffic Analytics: Real-Time Data Engineering Pipeline

This repository contains a real-time data engineering pipeline for processing road traffic data. It is a hands-on learning project developed to explore stream processing, data quality enforcement, and containerized big data infrastructure.

## 🎓 Acknowledgments & Learning Source
This project was built as a learning exercise inspired by the excellent tutorial video **[Real-Time Traffic Data Engineering Project | Kafka + Spark + Delta Lake](https://www.youtube.com/watch?v=yBc_UVnVhfY)** by **Data with Jay**. The core architecture and data flow concepts are heavily based on his instructional content.

## 🏗️ Architecture & Data Flow
The pipeline simulates real-time traffic events, injects realistic corrupted data, and processes the stream through a Medallion Architecture (Bronze, Silver, Gold layers).

1. **Data Ingestion:** A Python producer uses the `Faker` library to generate continuous traffic records (including vehicle IDs, city zones, speeds, and weather conditions) and publishes them to Apache Kafka.
2. **Stream Processing (Apache Spark):** 
   - **Bronze Layer (`traffic_bronze.py`):** Reads raw JSON byte streams from Kafka, applies an initial structural schema, flattens the records, and writes them to Delta Lake.
   - **Silver Layer (`traffic_silver.py`):** Cleanses the data by handling late events (via watermarking), dropping duplicate vehicle IDs, and filtering out injected anomalies like negative speeds, future events, or corrupted JSON strings.
   - **Gold Layer (`traffic_gold.py`):** Transforms the cleaned data into a business-ready Star Schema, generating distinct Dimension tables (e.g., Road, Zone) and a Fact table for traffic metrics.
3. **Storage & Metadata:** All data is persisted locally in Delta Lake format. A Hive Metastore backed by PostgreSQL is used to manage table schemas and metadata.

## 📁 Repository Structure
- `docker-compose.yml`: Container orchestration for Kafka, Spark Master, Spark Worker, PostgreSQL, and Hive Metastore.
- `producer/traffic_dirty_producer.py`: Python script simulating the real-time Kafka data stream with a 30% chance of injecting dirty/corrupt events.
- `apps/`: Contains the PySpark ETL scripts for the Bronze, Silver, and Gold processing layers.
- `hive-conf/hive-site.xml`: Configuration XML for initializing the Hive Metastore connection to PostgreSQL.

## 🚀 Environment & Setup
The entire infrastructure is containerized and designed to run locally on Ubuntu Linux. Docker Compose manages the multi-container environment, circumventing the need for managed cloud services like Databricks. 

## 👨‍💻 About the Developer
Developed by Trần Đức Toàn, an undergraduate student at the School of Information and Communication Technology (SoICT), Hanoi University of Science and Technology (HUST). This repository serves as a practical implementation to deepen my expertise in PySpark, Kafka, big data containerization, and enterprise data architectures.