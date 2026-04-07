##Data Engineering Project: Automated Gmail-to-Postgres Pipeline
#Project Overview
This project addresses the manual bottleneck of processing data from email attachments. I have developed an automated ETL (Extract, Transform, Load) pipeline that monitors a Gmail inbox for CSV files and syncs the data directly into a Supabase PostgreSQL database. This automation ensures 100% data accuracy and eliminates the need for manual intervention.

System Architecture
The pipeline is orchestrated using n8n and follows a structured data flow:

Extraction Phase: The Gmail Trigger node identifies new emails with specific attachments.

Transformation Phase: The binary CSV data is parsed into a structured JSON format.

Loading Phase: The processed data is mapped and inserted into the target PostgreSQL table.

Technical Stack
Orchestration Tool: n8n (Self-hosted)

Database Management: Supabase (PostgreSQL)

API Integration: Gmail API via OAuth2 authentication

Data Handling: CSV and JSON Parsing

Engineering Challenges and Solutions
1. Database Connection and Networking
I encountered connection timeouts while linking the local automation engine to the cloud database. To resolve this, I transitioned from a direct connection to a Transaction Pooler using Port 6543, which improved stability across different network protocols (IPv4/IPv6).

2. Data Cleaning and Formatting
The source CSV files often contained currency formatting with commas (e.g., 1,363). Since the database requires strict numeric types for calculations, I implemented JavaScript expressions to sanitize the strings and convert them into valid integers before the Load phase.

3. Error Handling and Schema Mapping
To ensure the pipeline doesn't break due to missing fields, I implemented strict schema mapping. This ensures that every attribute from the CSV aligns perfectly with the PostgreSQL table columns.

Repository Contents
workflow.json: The complete exported n8n workflow logic.

sample_sales_data.csv: A sample dataset used for testing the pipeline architecture.

documentation.md: Technical setup guide for API credentials.

Implementation Guide
Import the workflow.json file into your n8n environment.

Enable the Gmail API in Google Cloud Console and generate OAuth2 credentials.

Create a table in your Supabase instance that matches the CSV header structure.

Update the Postgres node with your specific Host, Port, and Password.

Toggle the workflow to Active to begin real-time data synchronization.
