```
# Airflow ETL Pipeline – Employee Data Processing

A small end-to-end **ETL pipeline built with Apache Airflow and PostgreSQL**, running locally using **Docker Compose**.

This project demonstrates how to orchestrate a data pipeline that:
- Ingests external CSV data
- Loads raw data into a staging table
- Deduplicates and merges records into a final table
- Runs reliably using Airflow DAGs

---

## 🧱 Tech Stack

- Apache Airflow (TaskFlow API)
- PostgreSQL
- Docker & Docker Compose
- Python
- SQL

---

## 📂 Project Structure

```

airflow-tutorial/
├── dags/
│   ├── process_employees.py      # Airflow DAG definition
│   ├── files/
│   │   └── employees.csv         # Downloaded CSV file
├── logs/                          # Airflow logs
├── plugins/
├── docker-compose.yaml
├── .env
└── README.md

````

---

## 🔄 Pipeline Overview

The `process_employees` DAG performs the following steps:

1. **Create final table (`employees`)**
   - Stores clean and deduplicated employee records
   - Enforces a primary key on `Serial Number`

2. **Create staging table (`employees_temp`)**
   - Stores raw data
   - Allows duplicate records
   - No constraints applied

3. **Download and load CSV data**
   - Fetches employee data from a public URL
   - Loads it into the staging table using PostgreSQL `COPY`

4. **Merge and deduplicate data**
   - Inserts distinct records into the final table
   - Updates existing records using `ON CONFLICT`

---

## ▶️ How to Run the Project

### 1️⃣ Prerequisites

Ensure the following are installed:

- Docker
- Docker Compose

Verify installation:

```bash
docker --version
docker compose version
````

---

### 2️⃣ Clone the Repository

```bash
git clone https://github.com/<your-username>/airflow-etl-employees.git
cd airflow-etl-employees
```

---

### 3️⃣ Set Up the Airflow Environment

```bash
# Create required folders
mkdir -p dags logs plugins

# Set Airflow user ID
echo -e "AIRFLOW_UID=$(id -u)" > .env

# Initialize Airflow
docker compose up airflow-init
```

---

### 4️⃣ Start Airflow

```bash
docker compose up
```

Open the Airflow UI:

```
http://localhost:8080
```

Login credentials:

* **Username:** airflow
* **Password:** airflow

---

## 🔌 Postgres Connection Setup

In the Airflow UI:

Navigate to **Admin → Connections → Add**

Fill in the following:

| Field           | Value              |
| --------------- | ------------------ |
| Connection ID   | `tutorial_pg_conn` |
| Connection Type | Postgres           |
| Host            | postgres           |
| Database        | airflow            |
| Login           | airflow            |
| Password        | airflow            |
| Port            | 5432               |

Save the connection.

---

## 🚀 Running the DAG

1. Open the **process_employees** DAG in the Airflow UI
2. Toggle the DAG **ON**
3. Click **Trigger DAG**

Monitor execution via the **Grid** and **Logs** views.

---

## 🧪 Data Model

### Staging Table: `employees_temp`

* Stores raw CSV data
* Allows duplicate records
* No primary key or constraints

### Final Table: `employees`

* Stores clean, deduplicated data
* Enforces a primary key on `Serial Number`
* Supports upserts via `ON CONFLICT`

---

## 🧠 Key Learnings

* Writing clean and maintainable Airflow DAGs
* Using the TaskFlow API for Python-based tasks
* Designing staging vs final tables in ETL pipelines
* Loading data efficiently with PostgreSQL `COPY`
* Handling duplicate data safely using SQL

---

## 📌 Notes

* The project runs entirely locally using Docker
* Intended as a learning and portfolio project
* Easily extendable with data validation, retries, or alerting

---
