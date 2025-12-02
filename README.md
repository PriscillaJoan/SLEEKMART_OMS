# Project: End-to-End Analytics Pipeline Using dbt + Snowflake

This project demonstrates the design and implementation of a complete analytics workflow using dbt (data build tool) and Snowflake. 
It showcases modern ELT development practices, including modeling, testing, documentation, and warehouse orchestration.

## 🔹 Overview

The project builds a clean and well structured transformation pipeline that ingests raw data from Snowflake and turns it into trusted, analysis ready models.
It includes staged models, business logic transformations, automated tests and data quality validations.

## 🔹 Key Features
### 1. Source Configuration

*Raw Snowflake tables are registered using sources.yml, allowing dbt to*:

- consistently reference upstream landing tables

- track lineage from raw → staging → marts

- run freshness and quality checks on ingestion layers

### 2. Staging Layer

*Each raw table is standardized and cleaned in the staging layer:*

- Computed fields: Amount Model (amount_stg): Computes order amounts from quantity and unit price.
- Consistent naming conventions: Combines customer first and last names into a full name for standardization like in the *customer_stg model*.

### 3. Intermediate & Business Models

*Models combine staging data to create business level entities, such as:*

- Customer Revenue Model: Combines the Customers, Completed Orders, and Amount models to calculate each customer’s total revenue.
- Store Performance Model: Uses the Target Sales seed and Actual Sales model to calculate and evaluate each store’s performance.

These models represent the logic analysts or dashboards rely on.

### 4. Seeds

*Project seeds (CSV files)*
- Sales targets. Loaded as warehouse tables using dbt’s seed command and referenced in the store performance model.

### 5. Testing & Data Quality

*The project includes:*

- Generic tests (unique and not_null): Verify that customer ID, email, phone number, and address are present and unique.

- Custom singular test: Checks for negative order amounts in the amount data.

<pre> ```sql SELECT ORDERID FROM {{ ref('amount_stg') }} WHERE AMOUNT < 0; ``` </pre>

### 6. Macros
   
*generate_schema_name*

- Uses a custom schema if provided.

- Defaults to the target schema from the dbt profile if no custom schema is specified.

- Helps ensure consistent schema assignment across models.

### 7. Documentation

Each model and column includes rich descriptions via YAML.

## 🔹 Tools & Technologies

- dbt Core

- Snowflake Data Warehouse

- Jinja templating

- SQL

- Git for version control

- VS Code development environment

## 🔹 How to Use the Project

- Clone the repository

- Configure your Snowflake profile

Run:

 - dbt seed
 -  dbt run
 - dbt test
