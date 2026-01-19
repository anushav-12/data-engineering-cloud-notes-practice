# Data Engineering – Day 2 Notes

## 1. Types of Data

### Structured Data
- Fixed schema (rows and columns)
- Stored in relational databases
- Example: PostgreSQL, Oracle

### Semi-Structured Data
- Flexible schema
- JSON, XML, CSV
- Stored in JSONB columns, NoSQL databases, or data lakes

### Unstructured Data
- No predefined schema
- Images, PDFs, logs, text files
- Commonly used in ML and AI systems

---

## 2. OLTP vs OLAP

### OLTP (Online Transaction Processing)
- Handles day-to-day transactions
- High volume of INSERT/UPDATE
- Normalized schema
- Example: Registration systems, payment portals

### OLAP (Online Analytical Processing)
- Used for analytics and reporting
- Read-heavy operations
- Aggregations and historical data
- Example: Dashboards, monthly/yearly reports

---

## 3. ETL vs ELT

### ETL
Extract → Transform → Load  
- Transformation happens before loading
- Used in legacy systems

### ELT
Extract → Load → Transform  
- Raw data is loaded first
- SQL-based transformations
- Used in modern data platforms

---

## 4. Data Pipeline Flow

User / Application  
↓  
OLTP Database  
↓  
ETL / ELT Job  
↓  
Reporting Tables  
↓  
Dashboards / Analytics

---

## 5. Role of SQL in Data Engineering
- Data aggregation
- Data transformation
- Data validation
- Performance optimization
  
----------------------------------------------

-- Day 2 SQL Practice: Data Engineering Basics

-- 1. Total number of registrations
SELECT COUNT(*) AS total_registrations
FROM pt_profile_master;

-- 2. Total tax collected
SELECT SUM(amount) AS total_collection
FROM pt_payments;

-- 3. Count of registrations per office
SELECT off_code, COUNT(*) AS total_rc
FROM pt_profile_master
GROUP BY off_code;

-- 4. Monthly tax collection
SELECT DATE_TRUNC('month', payment_date) AS month,
       SUM(amount) AS total_amount
FROM pt_payments
GROUP BY DATE_TRUNC('month', payment_date)
ORDER BY month;

-- 5. Join profile and payment tables
SELECT p.rcn,
       p.name,
       pay.amount,
       pay.payment_date
FROM pt_profile_master p
JOIN pt_payments pay
  ON p.rcn = pay.rcn;

-- 6. Filter recent payments (last 30 days)
SELECT *
FROM pt_payments
WHERE payment_date >= CURRENT_DATE - INTERVAL '30 days';

-- 7. Age calculation using DOB
SELECT rcn,
       EXTRACT(YEAR FROM AGE(CURRENT_DATE, dob)) AS age
FROM pt_profile_master
WHERE dob IS NOT NULL;
