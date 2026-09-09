# redflag-fraud-transaction
# 🚨 RedFlag — The Fraud Files

> **A SQL-based fraud detection engine built to uncover suspicious transaction patterns — no Machine Learning, no Python, just SQL.**

## 📌 Project Overview

**RedFlag — The Fraud Files** is a fraud detection and data analytics project built using **pure SQL**.

The project simulates the work of a fraud analyst at **PayFast**, a fictional Indian payment aggregator processing transactions across multiple cities and payment methods.

The objective was to investigate **200,000 transaction records collected over six months** and identify **12 different fraud patterns** hidden within the data.

Instead of using Machine Learning, the project focuses on using SQL-based analytical techniques to identify suspicious behaviour and generate actionable fraud signals.

---

## 🎯 Project Objective

The goal of this project was to answer one key question:

> **Can meaningful fraud patterns be detected using SQL alone?**

To answer this, I analysed transaction behaviour across:

* Users
* Merchants
* Transaction amounts
* Transaction timestamps
* Cities
* Payment modes
* Transaction status
* Transaction types

The final deliverable consists of **12 targeted SQL queries**, with each query designed to detect a specific fraud pattern.

---

## 📊 Dataset

| Feature           | Details                       |
| ----------------- | ----------------------------- |
| Transactions      | 200,000                       |
| Time Period       | January – June 2024           |
| Users             | ~14,500 legitimate users      |
| Suspect Users     | 255+                          |
| Merchants         | 800                           |
| Payment Modes     | UPI, CARD, NETBANKING, WALLET |
| Transaction Types | DEBIT, CREDIT, REFUND         |
| Cities            | 20+ Indian cities             |
| Database          | MySQL                         |

The dataset is stored in a single `transactions` table.

### Main Columns

```text
txn_id
user_id
merchant_id
amount
txn_time
status
payment_mode
city
txn_type
```

---

# 🔎 12 Fraud Patterns Detected

## 1. ⚡ Velocity Fraud

Detects users making an unusually large number of transactions within a single day.

**Detection signature:**

* 30+ transactions
* Same user
* Same calendar day

---

## 2. 💰 Round-Amount Clustering

Identifies users repeatedly making transactions using suspiciously round amounts.

Examples include:

```text
₹100
₹200
₹500
₹1,000
₹2,000
₹5,000
₹10,000
```

---

## 3. 💳 Card Testing

Detects potential card-testing behaviour through repeated low-value transactions.

**Detection signature:**

> 30+ transactions under ₹10 by the same user on the same day.

---

## 4. ❌ Failed-Then-Succeeded Transactions

Identifies users with unusually high numbers of failed transactions, potentially indicating automated attempts to find valid transaction/card combinations.

The advanced approach looks for:

> A failed transaction followed by a successful transaction of the same amount within two minutes.

---

## 5. 🌙 Odd-Hour Concentration

Detects users whose transaction activity is heavily concentrated between:

```text
02:00 AM – 05:00 AM
```

Users with 30+ transactions and at least 80% of their activity during these hours are flagged.

---

## 6. 🏦 Mule Accounts

Identifies accounts potentially being used to receive and quickly move suspicious funds.

The advanced pattern looks for:

> A CREDIT transaction followed by a DEBIT transaction within 30 minutes, where the debit is at least 70% of the credited amount.

---

## 7. 🔄 Refund Abuse

Detects users with unusually high refund activity.

**Detection signature:**

* 20+ total transactions
* Refund ratio > 40%

---

## 8. 🏪 Merchant Collusion

Identifies merchants where a small number of users generate a disproportionately large share of transaction volume.

**Detection signature:**

> Top 5 users account for more than 60% of a merchant's total transaction value.

This pattern required multi-step SQL analysis using CTEs and ranking.

---

## 9. 🎯 Just-Under-Threshold Structuring

Detects repeated transactions of exactly:

```text
₹9,999
```

The pattern identifies users with:

> 10+ transactions at exactly ₹9,999.

This can indicate an attempt to structure transactions below a monitoring threshold.

---

## 10. 💤 Dormant-Then-Active

Identifies accounts that remain inactive for an extended period and then suddenly become highly active.

**Detection signature:**

* 90+ day gap between consecutive transactions
* Followed by 15+ transactions

This pattern uses transaction history to identify potential account takeover behaviour.

---

## 11. 📈 Velocity Spike

Detects sudden changes in a user's normal transaction behaviour.

The analysis compares:

* Average monthly transaction count
* Peak monthly transaction count

A user is flagged when:

> Peak monthly activity is at least 5× their average monthly activity and the peak contains at least 20 transactions.

This acts as a SQL-based approach to behavioural anomaly detection.

---

## 12. 🌍 Geographic Impossibility

Detects potentially impossible travel between transaction locations.

**Detection signature:**

> The same user makes consecutive transactions in different cities within 60 minutes.

This pattern uses SQL window functions to compare each transaction with the user's previous transaction.

---

# 🛠️ SQL Techniques Used

This project helped me move beyond basic SQL queries and apply SQL to a real-world analytical problem.

### Core SQL

```text
SELECT
WHERE
GROUP BY
HAVING
ORDER BY
COUNT()
SUM()
CASE WHEN
IN
BETWEEN
```

### Advanced SQL

```text
JOINs
Subqueries
Correlated Subqueries
EXISTS
CTEs
Window Functions
LAG()
ROW_NUMBER()
OVER()
PARTITION BY
TIMESTAMPDIFF()
DATE()
HOUR()
DATE_FORMAT()
```

---

# 🧠 Key Learning

One of the biggest takeaways from this project was understanding that **fraud detection is not always dependent on Machine Learning**.

By analysing transaction frequency, timing, amount, location and behavioural changes, SQL can uncover patterns that deserve further investigation.

This project helped me strengthen my ability to:

* Think analytically about suspicious behaviour
* Translate business problems into SQL logic
* Work with large transaction datasets
* Use aggregation and conditional logic
* Apply joins and subqueries
* Work with CTEs
* Use window functions for behavioural analysis
* Build queries that produce actionable results

---

# 📁 Project Structure

```text
RedFlag-The-Fraud-Files/
│
├── README.md
│
├── RedFlag_YourName.sql
│
└── screenshots/
    ├── velocity_fraud.png
    ├── merchant_collusion.png
    ├── velocity_spike.png
    └── geographic_impossibility.png
```

> **Note:** The original transaction dataset is not included in the GitHub repository because of its size. The project brief specifies keeping the 18 MB `redflag_transactions.sql` dataset outside the repository.

---

# 🚀 How to Run

### 1. Install MySQL

Use **MySQL Workbench** or another MySQL-compatible environment.

### 2. Load the dataset

Import:

```text
redflag_transactions.sql
```

The dataset creates the `redflag` database and `transactions` table.

### 3. Select the database

```sql
USE redflag;
```

### 4. Run the fraud detection queries

Open:

```text
RedFlag_YourName.sql
```

and execute the queries individually.

---

# 📸 Project Screenshots
output of pattern 8

<img width="907" height="435" alt="Screenshot 2026-09-09 163616-P8op" src="https://github.com/user-attachments/assets/68478a8c-130a-4178-858a-a9fdc5858261" />



These demonstrate the range of SQL techniques used in the project.

---

# 💡 Why This Project Matters

Fraud detection is a practical application of data analytics.

Rather than simply analysing historical data, this project focuses on identifying **behavioural signals that could indicate suspicious activity**.

The project simulates a workflow where query results could be used by fraud operations teams to investigate suspicious accounts and transactions.

---

# 📌 Project Highlights

```text
200,000+ Transactions
6 Months of Data
12 Fraud Patterns
Pure SQL
MySQL
CTEs
Joins
Subqueries
Window Functions
Fraud Analytics
```

---

# 🎓 Project Context

**Project:** RedFlag — The Fraud Files
**Focus:** Fraud Detection & Data Analytics
**Database:** MySQL
**Approach:** SQL-based Fraud Detection
**Duration:** 7 Days

Built as part of the **The Unlox Academy Industry-Graded Minor Project**.

---

# 👨‍💻 Author

Goutami RS

Aspiring Data Analyst | SQL | Data Analytics | FinTech

📌 GitHub: https://github.com/goutami-rs04
📌 LinkedIn: https://www.linkedin.com/in/goutami-rs-81857832b/

---

## ⭐ If you find this project interesting

Feel free to explore the SQL queries, experiment with the fraud patterns, and suggest improvements.

**Real data. Real patterns. Real analytical thinking.**
