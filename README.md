# Data Engineering Project: Automated Gmail-to-Postgres Pipeline

## Project Overview
This project addresses the manual bottleneck of processing data from email attachments. I have developed an automated ETL (Extract, Transform, Load) pipeline that monitors a Gmail inbox for CSV files and syncs the data directly into a Supabase PostgreSQL database.

## 📁 Project Dataset
To test this pipeline, you can use the sample dataset provided in this repository:
* **[Download Sample CSV](sample_sales_data.csv)** (Use this file to trigger the automation via Gmail).

## System Architecture
The pipeline is orchestrated using n8n and follows a structured data flow:

1. **Extraction Phase:** The Gmail Trigger node identifies new emails with specific attachments.
2. **Transformation Phase:** The binary CSV data is parsed into a structured JSON format.
3. **Loading Phase:** The processed data is mapped and inserted into the target PostgreSQL table.

![n8n Workflow](work-flow.png)

## Technical Stack
* **Orchestration Tool:** n8n (Self-hosted)
* **Database Management:** Supabase (PostgreSQL)
* **API Integration:** Gmail API via OAuth2 authentication
* **Data Handling:** CSV and JSON Parsing

## Engineering Challenges and Solutions

### 1. Database Connection and Networking
I transitioned to a Transaction Pooler using Port 6543 to resolve connection timeouts between the local automation engine and the cloud database.

### 2. Data Cleaning (The Comma Problem)
The source CSV files contained currency formatting with commas (e.g., 1,363). I implemented JavaScript expressions within n8n to strip these commas and convert strings into valid integers before database insertion.

![Database Result](dataset.png)

## Implementation Guide
1. Import the `workflow.json` file into your n8n environment.
2. Upload the `sample_sales_data.csv` as an email attachment to your registered Gmail.
3. Check your Supabase table for the synced records.

![Gmail Trigger](gmail.png)
