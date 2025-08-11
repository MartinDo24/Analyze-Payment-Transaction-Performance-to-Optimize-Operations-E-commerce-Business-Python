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

###📖 What is this project about? What Business Question will it solve?

- This project focuses on analyzing payment transaction data to identify inefficiencies, detect anomalies, and optimize the transaction process for a financial service provider

- Using Python for data cleaning, exploratory data analysis, and visualization, the goal is to uncover patterns in transaction success rates, failure causes, and processing delays

- Which factors most significantly contribute to failed or delayed transactions?

- Are there specific time periods, locations, or transaction types where performance drops?

- How can we reduce transaction failure rates and improve overall processing efficiency?

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

1️⃣ Data Cleaning & EDA

-Remove duplicate value, handle missing value, change the type of 2 columns Df_transactions['sender_id'],Df_transactions['receiver_id'] to correct type

-Merge 2 payment_report and product table

<img width="1261" height="159" alt="image" src="https://github.com/user-attachments/assets/93652f88-5650-4848-814d-208de52d11e7" />

<img width="1214" height="70" alt="image" src="https://github.com/user-attachments/assets/6d66b3af-68eb-494b-a392-85865583d8d0" />

<img width="1200" height="235" alt="image" src="https://github.com/user-attachments/assets/81fdb50d-c322-47a3-9629-124612aa22a2" />

2️⃣ Data Wrangling & Python Analyst

## Task 1: Top 3 product_ids with the highest volume.

- Goal: Determine which products generate the highest transaction volume within the e-wallet ecosystem. This helps the company prioritize partnerships, allocate promotional budgets, and design targeted marketing campaigns to maximize transaction value.

-Approach

+ Group data by Product_id and sum the weight using groupby() and sum() methods.

+ Sort results in descending order using sort_values(ascending=False

+ Get the first 3 rows using head(3).

<img width="1218" height="66" alt="image" src="https://github.com/user-attachments/assets/b7e652ef-f656-4ba2-8549-20cadcfde60a" />

<img width="204" height="166" alt="image" src="https://github.com/user-attachments/assets/c06288d9-e78f-49ed-8f30-9482fd15d870" />

## Task 2: Given that 1 product_id is only owed by 1 team, are there any abnormal products against this rule?

- Goal: Identify if there are any products that violate the rule where each product_id is only owned by one team

-Approach

+ Initialize Tracking: abnormal_products list stores violating product_ids.

+ Identify Unique Products: df['product_id'].unique() extracts distinct product_id values to check.

+ Check Team Ownership: For each product_id, collect its unique team_own values using unique(). If a product_id has more than 1 team (len(teams) != 1), it’s added to abnormal_products.

+ Return Clear Results: Returns a list of violating IDs or confirms compliance.

<img width="1213" height="357" alt="image" src="https://github.com/user-attachments/assets/96ad66d3-4acd-4fc4-8fe1-72d497769750" />

<img width="581" height="32" alt="image" src="https://github.com/user-attachments/assets/86029000-bb14-4ded-aabf-e07dd6b77567" />

## Task 3: Find the team has had the lowest performance (lowest volume) since Q2.2023. Find the category that contributes the least to that team.

- Goal: Identify the lowest performing business groups from Q2 2023 and find the product portfolio that contributed least to that performance, to support strategic management of improvements or renewable energy sources.

- Approach:

+ Filter Data for Q2 2023: df['report_month'] >= '2023-04' isolates records from April 2023 onward (Q2 starts in April).

+ Identify Weakest Team: groupby('team_own')['volume'].sum() calculates total volume per team. idxmin() returns the team with the lowest summed volume.

+ Find Weakest Category in Weakest Team: Filters data for the weakest team (team_data). Groups by category and sums volume to find the least productive category (idxmin()).

+ Return Results: Prints the team and category names for clarity. Returns both values for further analysis.

<img width="1214" height="379" alt="image" src="https://github.com/user-attachments/assets/17f1da9c-5028-442f-9d05-d83078633314" />

<img width="433" height="47" alt="image" src="https://github.com/user-attachments/assets/0432eb6f-0f37-4845-9ea5-98d00644f6c4" />

## Task 4: Find the contribution of source_ids of refund transactions (payment_group = ‘refund’), what is the source_id with the highest contribution?

- Goal :Determine which refund source (identified by source_id) contributes the most to the total refund amount, so that the business can prioritize investigation into its causes and implement targeted strategies to reduce refund losses.

- Approach:

+ Filter Refund Transactions: df[df['payment_group'] == 'refund'] isolates refund records.

+ Aggregate Refund Volume by Source: groupby('source_id')['volume'].sum() calculates total refund volume per source_id.

+ Identify Top Contributor: idxmax() returns the source_id with the highest refund volume. max() returns the exact contribution value.

+ Return Results: Prints the top source_id and its contribution for quick insights. Returns both values for further analysis (e.g., root-cause investigation).

<img width="1212" height="196" alt="image" src="https://github.com/user-attachments/assets/907dfa66-7546-4e67-92c2-b723d124daa8" />

<img width="414" height="59" alt="image" src="https://github.com/user-attachments/assets/10f15a4c-3db7-4002-b580-59c1e976b4ca" />

## Task 5: Define type of transactions (‘transaction_type’) for each row, given:

+ transType = 2 & merchant_id = 1205: Bank Transfer Transaction

+ transType = 2 & merchant_id = 2260: Withdraw Money Transaction

+ transType = 2 & merchant_id = 2270: Top Up Money Transaction

+ transType = 2 & others merchant_id: Payment Transaction

+ transType = 8, merchant_id = 2250: Transfer Money Transaction

+ transType = 8 & others merchant_id: Split Bill Transaction

+ Remained cases are invalid transactions

- Goal:

  Classify all corporate e-wallet transactions by type (Bank Transfer, Withdrawal, Deposit, Payment, Transfer, Bill Split, Invalid) to:
+ Support the operations department to accurately track each type of transaction.

+ Detect invalid (invalid) transactions for timely processing or blocking

+ Provide input data for performance analysis by transaction type, thereby optimizing resources and business strategies

- Approach:

+ Rule-Based Classification:

transType = 2:

merchant_id = 1205 → Bank Transfer

merchant_id = 2260 → Withdraw Money (Note: Your code uses 2270 but the docstring says 2260; adjust as needed)

merchant_id = 2270 → Top Up Money

All others → Payment Transaction

transType = 8:

merchant_id = 2250 → Transfer Money

All others → Split Bill

All other transType values: Marked as "Invalid Transaction"

Application to DataFrame: Uses apply(define_transaction_type, axis=1) to process each row. Creates a new column transaction_type with the classified labels

Validation :Df_transactions.head() previews the results for verification

<img width="1209" height="349" alt="image" src="https://github.com/user-attachments/assets/2d7da148-13e4-422b-924c-008e7189d24d" />

<img width="1054" height="203" alt="image" src="https://github.com/user-attachments/assets/aa453d9b-4914-4b4b-8d52-7bae70a97aa7" />

## Task 6: Of each transaction type (excluding invalid transactions): find the number of transactions, volume, senders and receivers.

- Goal:
Determine the performance and popularity of each valid transaction type (excluding invalid transactions) to:

+ Evaluate which transaction types account for a large proportion in terms of quantity and value

+ Identify customer behavior based on the number of senders and recipients.

+ Support the optimization of transaction processing infrastructure and design appropriate service packages for key customer groups

-Approach:

+ Filter Valid Transactions: Excludes rows where transaction_type = 'Invalid Transaction'.

+ Aggregate Metrics:

num_transactions: Counts occurrences of each transaction type.

volume: Sums the volume column per type.

senders & receivers: Counts unique sender_id and receiver_id values using nunique

+ Return Structured Summary: Returns a DataFrame with transaction types as indices and aggregated metrics as columns.

<img width="1215" height="216" alt="image" src="https://github.com/user-attachments/assets/ee63c233-594a-4b6c-84dd-3117fbc26c92" />

<img width="619" height="229" alt="image" src="https://github.com/user-attachments/assets/3fa0ca23-ad0f-4b79-aa36-842c916de514" />

## 🔎 Final Conclusion & Recommendations  

- Final Conclusion

After analyzing transaction data across payment groups, channels, and customer segments, several key findings emerged:

✔️Refunds Concentrated in Specific Source IDs

Source ID 38 accounts for the highest refund amount, totaling $36,527,454,759. This concentration suggests potential operational, product, or partner-related issues tied to this specific source.

✔️High Transaction Volume from Certain Payment Channels

A few channels dominate transaction counts and values, indicating strong customer adoption but also potential risk if over-reliance is not managed.

✔️Seasonal and Monthly Transaction Patterns

Fluctuations in transaction activity point to seasonal campaigns, promotional periods, or market behavior trends.

✔️Customer Behavior Segmentation Insights

Distinct patterns emerge among frequent users, high-value users, and refund-prone customers, allowing for targeted retention and risk management strategies.

✔️Revenue Impact from Refunds

Refund transactions are not only operationally costly but also have a measurable impact on monthly revenue, requiring tighter control and monitoring.
Automate invalid transaction alerts for real-time resolution

📌 Recommendations:

✔️Investigate High-Refund Source IDs

Conduct an in-depth audit of Source ID 38 to identify root causes (e.g., merchant disputes, technical errors, fraud).

Implement stricter quality checks, partner performance reviews, and contractual refund penalty clauses if applicable.

✔️Diversify Payment Channel Dependence

Encourage customer adoption of underutilized but secure and cost-effective payment channels.

Offer channel-specific incentives to balance transaction flow.

✔️Seasonal Strategy Optimization

Align marketing campaigns with observed transaction peaks to maximize revenue.

Introduce targeted promotions during off-peak months to smooth revenue seasonality.

✔️Customer Retention & Risk Segmentation

Reward loyal and high-value users through exclusive perks, cashback offers, or tier-based rewards.

Closely monitor high refund-rate users and apply stricter refund validation.

✔️Refund Reduction Initiatives

Enhance transaction verification processes before settlement.

Introduce a tiered refund approval process for high-value transactions.

Launch customer education campaigns on transaction confirmation steps to reduce accidental refunds.
