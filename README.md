# Analyze Payment Transaction Performance to Optimize Operations – E-commerce Business | Python

<img width="1910" height="1000" alt="image" src="https://github.com/user-attachments/assets/b015262e-202b-4f0e-aae6-609f6d0e9ba1" />


Author: Đỗ Hoàng Minh

Date: 2025-04-25

Tools Used: Python (google colab)

##📑 Table of Contents:

1.[📌Background & Overview](#-background--overview)

2.[📂 Dataset Description & Data Structure](#-dataset-description--data-structure)

3.[🔎 Final Conclusion & Recommendations](#-final-conclusion--recommendations)

## 📌 Background & Overview

###Objective:

###📖 What is this project about? 

-Analyzes e-commerce payment data (Jan-Apr 2023) using Python on Google Colab, processing payment_report.csv, product.csv, and transactions.csv for insights on performance and transactions.

-Identifies top products for revenue, detects ownership issues, finds underperforming teams and weak categories, optimizes refunds, and improves customer experience via transaction classification.

### 👤 Who is this project for?

-Product managers, marketing teams

-Financial analysts

-Business Analysts/Management

## 📂 Dataset Description & Data Structure

-Source: Source.ip.zip

-Size: 3 Tables ~ 500K rows

-Format: payment_report.csv, product.csv, transactions.csv

### 📊 Data Structure & Relationships

#### 1️⃣ Tables Used

-Main table: Df_transactions, Df_payment_enriched

#### 2️⃣ Table Schema & Data Snapshot

Table 1: Df_payment_enriched

| Column Name                      | Data Type 
|----------------------------------|-----------
| report_month                     | OBJECT    
| payment_group                    | OBJECT    
| product_id                       | INTEGER  
| source_id                        | INTEGER   
| volume                           | INTEGER  
| category                         | OBJECT   
| team_own                         | OBJECT   

Tabble 2: Df_transactions

| Column Name                      | Data Type 
|----------------------------------|-----------
| transaction_id                   | INTEGER    
| merchant_id                      | INTEGER    
| volume                           | INTEGER  
| transType                        | INTEGER   
| transStatus                      | INTEGER  
| sender_id                        | INTEGER   
| receiver_id                      | INTEGER   
| timeStamp                        | INTEGER   

## ⚒️ Main Process :

1️⃣ EDA & Data Cleaning

-Merge payment_report.csv with product.csv to create Df payment_enriched

<img width="357" height="360" alt="image" src="https://github.com/user-attachments/assets/4c8f66eb-65eb-4a51-abb0-22dbcc2cb3a2" />

-Remove duplicate value, handle missing value, change the type of 2 columns Df_transactions['sender_id'],Df_transactions['receiver_id'] to correct type

<img width="629" height="427" alt="image" src="https://github.com/user-attachments/assets/a79cc4c3-6e8a-4257-922b-b95e4cce0c1a" />

2️⃣ Data Wrangling & Python Analyst

## Task 1: Top 3 product_ids with the highest volume.

-Group data by Product_id and sum the weight using groupby() and sum() methods.

-Sort results in descending order using sort_values(ascending=False

-Get the first 3 rows using head(3).

<img width="204" height="166" alt="image" src="https://github.com/user-attachments/assets/c06288d9-e78f-49ed-8f30-9482fd15d870" />

## Task 2: Given that 1 product_id is only owed by 1 team, are there any abnormal products against this rule?

-Initialize Tracking: abnormal_products list stores violating product_ids.

-Identify Unique Products: df['product_id'].unique() extracts distinct product_id values to check.

-Check Team Ownership: For each product_id, collect its unique team_own values using unique(). If a product_id has more than 1 team (len(teams) != 1), it’s added to abnormal_products.

-Return Clear Results: Returns a list of violating IDs or confirms compliance.

<img width="581" height="32" alt="image" src="https://github.com/user-attachments/assets/86029000-bb14-4ded-aabf-e07dd6b77567" />

## Task 3: Find the team has had the lowest performance (lowest volume) since Q2.2023. Find the category that contributes the least to that team.

-Filter Data for Q2 2023: df['report_month'] >= '2023-04' isolates records from April 2023 onward (Q2 starts in April).

-Identify Weakest Team: groupby('team_own')['volume'].sum() calculates total volume per team. idxmin() returns the team with the lowest summed volume.

-Find Weakest Category in Weakest Team: Filters data for the weakest team (team_data). Groups by category and sums volume to find the least productive category (idxmin()).

-Return Results: Prints the team and category names for clarity. Returns both values for further analysis.

<img width="433" height="47" alt="image" src="https://github.com/user-attachments/assets/0432eb6f-0f37-4845-9ea5-98d00644f6c4" />

## Task 4: Find the contribution of source_ids of refund transactions (payment_group = ‘refund’), what is the source_id with the highest contribution?

-Filter Refund Transactions: df[df['payment_group'] == 'refund'] isolates refund records.

-Aggregate Refund Volume by Source: groupby('source_id')['volume'].sum() calculates total refund volume per source_id.

-Identify Top Contributor: idxmax() returns the source_id with the highest refund volume. max() returns the exact contribution value.

-Return Results: Prints the top source_id and its contribution for quick insights. Returns both values for further analysis (e.g., root-cause investigation).

<img width="414" height="59" alt="image" src="https://github.com/user-attachments/assets/10f15a4c-3db7-4002-b580-59c1e976b4ca" />

## Task 5: Define type of transactions (‘transaction_type’) for each row, given:

+ transType = 2 & merchant_id = 1205: Bank Transfer Transaction

+ transType = 2 & merchant_id = 2260: Withdraw Money Transaction

+ transType = 2 & merchant_id = 2270: Top Up Money Transaction

+ transType = 2 & others merchant_id: Payment Transaction

+ transType = 8, merchant_id = 2250: Transfer Money Transaction

+ transType = 8 & others merchant_id: Split Bill Transaction

+ Remained cases are invalid transactions

-Rule-Based Classification:

transType = 2:

merchant_id = 1205 → Bank Transfer

merchant_id = 2260 → Withdraw Money (Note: Your code uses 2270 but the docstring says 2260; adjust as needed)

merchant_id = 2270 → Top Up Money

All others → Payment Transaction

transType = 8:

merchant_id = 2250 → Transfer Money

All others → Split Bill

All other transType values: Marked as "Invalid Transaction"

-Application to DataFrame: Uses apply(define_transaction_type, axis=1) to process each row. Creates a new column transaction_type with the classified labels

-Validation :Df_transactions.head() previews the results for verification

<img width="1054" height="203" alt="image" src="https://github.com/user-attachments/assets/aa453d9b-4914-4b4b-8d52-7bae70a97aa7" />

## Task 6: Of each transaction type (excluding invalid transactions): find the number of transactions, volume, senders and receivers.

-Filter Valid Transactions: Excludes rows where transaction_type = 'Invalid Transaction'.

-Aggregate Metrics:

num_transactions: Counts occurrences of each transaction type.

volume: Sums the volume column per type.

senders & receivers: Counts unique sender_id and receiver_id values using nunique

-Return Structured Summary: Returns a DataFrame with transaction types as indices and aggregated metrics as columns.

<img width="619" height="229" alt="image" src="https://github.com/user-attachments/assets/3fa0ca23-ad0f-4b79-aa36-842c916de514" />

## 🔎 Final Conclusion & Recommendations  

📌 Key Takeaways:

✔️ 1. Recommendation: Optimize High-Refund Sources

Investigate root causes (e.g., product defects, payment errors).

Implement stricter validation for transactions from this source.

✔️2. Recommendation: Address Weakest Team Performance
Provide targeted training/resources to APS.

Reassess inventory or marketing strategies for the PXXXXXE category.

✔️3.Recommendation: Streamline Transaction Types

Subclassify generic payments (e.g., "Subscription Payment," "One-Time Purchase").

Automate invalid transaction alerts for real-time resolution
