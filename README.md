# 🏗️ Data Lakehouse Architecture on AWS

This project demonstrates building a scalable **data lakehouse** on AWS using:
- **AWS Lake Formation** for secure data governance  
- **Apache Iceberg** for high-performance, open-table storage  
- **Terraform** for infrastructure-as-code  
- **AWS Glue & Athena** for data ingestion, transformation, and querying

The architecture follows the **medallion model**:  
**Landing → Curated → Presentation**

---

## 📁 Project Structure

```
.
├── setup.sh            # Prepares environment variables for Terraform
├── terraform/          # Contains Terraform configs for AWS resources
│   ├── main.tf
│   ├── variables.tf
│   ├── backend.tf
│   └── modules/
├── scripts/            # Glue scripts or supporting ETL code
├── .gitignore
└── README.md
```

---

## 🚀 Quickstart: How to Run

### 1️⃣ Set DB Credentials

```bash
export DB_USER="your-db-username"
export DB_PASS="your-db-password"
```

### 2️⃣ Run Setup Script

```bash
bash setup.sh
```

This script sets up all required environment variables using Terraform-compatible keys.

### 3️⃣ Deploy Infrastructure

```bash
cd terraform
terraform init
terraform apply
```

---

## 🔍 Architecture Overview

### 🟠 Landing Zone
- Ingests raw data from:
  - **MySQL (Amazon RDS)** → AWS Glue job → CSV in S3
  - **Streaming (simulated JSON)** → saved to S3

### 🟡 Curated Zone
- Transforms CSV and JSON data using Glue
- Joins datasets and applies schema validation
- Stores final datasets as **Apache Iceberg** tables

### 🟢 Presentation Zone
- Uses **Athena** + `awswrangler` to create analytical tables via CTAS
- Final tables include:
  - `ratings_for_ml`
  - `sales_report`
  - `ratings_per_product`

---

## ❄️ Apache Iceberg Features

- ✅ **Schema Evolution**: Add new columns like `ratingtimestamp`
- ✅ **ACID Transactions**: Ensures reliable updates
- ✅ **Partitioning**: Improves Athena performance
- ✅ **Metadata Management**: Enables table versioning and rollback

---

## 🔐 Lake Formation Permissions

Lake Formation is used to:
- Register S3 locations
- Grant access to Glue jobs and IAM roles
- Secure `curated_zone` and `presentation_zone` tables

---

## 🧹 Clean-Up

To destroy all deployed AWS resources:

```bash
cd terraform
terraform destroy
```

---



---

## ✅ Features Summary

- 🧱 Infrastructure-as-code (Terraform)
- 🔄 Glue-based ETL pipeline
- ❄️ Iceberg-powered data lakehouse
- 🔐 Governed by Lake Formation
- 📊 Queried via Amazon Athena + `awswrangler`

---

