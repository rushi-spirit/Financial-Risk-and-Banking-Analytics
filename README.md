# Financial Risk and Banking Analytics

A relational database project focused on financial risk assessment using SQL and MySQL. Designed to simulate a real-world banking environment where loan distribution, account behavior, and payment patterns are analyzed to support risk segmentation and decision-making.

---

## 🗂️ Project Overview

This project involves designing and querying a normalized relational database that models core banking entities. The goal is to identify high-risk customer patterns through structured SQL analysis — without machine learning, purely through query-driven insight generation.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| MySQL | Database design & querying |
| SQL | Joins, aggregations, conditional logic |
| ERD | Schema design & relationship mapping |

---

## 🗃️ Database Schema

The database consists of 4 interconnected entities:

- **Customers** — demographic and profile data
- **Accounts** — account type, status, and balance information
- **Loans** — loan amount, type, tenure, and approval status
- **Payments** — payment history, due dates, and payment status

### Entity Relationships
- One customer → multiple accounts
- One account → multiple loans
- One loan → multiple payment records

---

## 🔍 Key Analyses

### 1. Loan Distribution Analysis
- Breakdown of loan types across customer segments
- Assessment of approved vs. rejected loan applications
- Average loan amount by account type

### 2. Payment Behavior Analysis
- Identification of missed and delayed payments
- Payment frequency patterns per customer
- Customers with consecutive missed payments flagged as high-risk

### 3. Risk Segmentation
- Customers segmented into risk tiers based on payment history
- High-risk profiles identified using conditional SQL logic
- Account status correlation with loan default likelihood

---

## 📋 Sample Queries

```sql
-- Identify high-risk customers with missed payments
SELECT 
    c.customer_id,
    c.customer_name,
    COUNT(p.payment_id) AS missed_payments
FROM customers c
JOIN accounts a ON c.customer_id = a.customer_id
JOIN loans l ON a.account_id = l.account_id
JOIN payments p ON l.loan_id = p.loan_id
WHERE p.payment_status IN ('Missed', 'Delayed')
GROUP BY c.customer_id, c.customer_name
HAVING COUNT(p.payment_id) > 2
ORDER BY missed_payments DESC;
```

```sql
-- Loan distribution by type
SELECT 
    loan_type,
    COUNT(*) AS total_loans,
    AVG(loan_amount) AS avg_loan_amount,
    SUM(loan_amount) AS total_exposure
FROM loans
GROUP BY loan_type
ORDER BY total_exposure DESC;
```

```sql
-- Account status vs loan default correlation
SELECT 
    a.account_status,
    COUNT(l.loan_id) AS total_loans,
    SUM(CASE WHEN p.payment_status = 'Missed' THEN 1 ELSE 0 END) AS missed_count
FROM accounts a
JOIN loans l ON a.account_id = l.account_id
JOIN payments p ON l.loan_id = p.loan_id
GROUP BY a.account_status;
```

---

## 📁 Repository Structure

```
Financial-Risk-and-Banking-Analytics/
│
├── schema/
│   └── create_tables.sql        # DDL — table creation scripts
│
├── data/
│   └── sample_data.sql          # Sample INSERT statements
│
├── queries/
│   ├── loan_distribution.sql    # Loan analysis queries
│   ├── payment_behavior.sql     # Payment pattern queries
│   └── risk_segmentation.sql    # Risk segmentation queries
│
├── erd/
│   └── banking_erd.png          # Entity Relationship Diagram
│
└── README.md
```

---

## 💡 Key Insights

- Customers with 3+ missed payments account for a disproportionate share of total loan exposure
- Delayed payments are most concentrated in personal loan categories
- Inactive accounts show a significantly higher rate of loan defaults compared to active accounts

---

## 🚀 How to Run

1. Clone the repository
   ```bash
   git clone https://github.com/rushi-spirit/Financial-Risk-and-Banking-Analytics.git
   ```

2. Import the schema
   ```bash
   mysql -u root -p < schema/create_tables.sql
   ```

3. Load sample data
   ```bash
   mysql -u root -p banking_db < data/sample_data.sql
   ```

4. Run queries from the `/queries` folder in MySQL Workbench or any SQL client

---

## 👤 Author

**Hrushikesh Patil**  
MCA Candidate | Data Analyst  
[LinkedIn](https://www.linkedin.com/in/hrushikesh-patil-analyst) · [GitHub](https://github.com/rushi-spirit)
