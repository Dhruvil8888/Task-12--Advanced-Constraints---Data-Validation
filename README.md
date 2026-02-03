/*
Task 12: Advanced Constraints & Data Validation
Objective: Enforce strong data quality using
CHECK, DEFAULT, UNIQUE, and NOT NULL constraints.
*/

USE intern_training_db;

DROP TABLE IF EXISTS user_accounts;

CREATE TABLE user_accounts (
    user_id INT AUTO_INCREMENT PRIMARY KEY,

    username VARCHAR(50) 
        NOT NULL 
        UNIQUE,

    email VARCHAR(100) 
        NOT NULL 
        UNIQUE,

    age INT 
        CHECK (age BETWEEN 18 AND 60),

    salary DECIMAL(10,2) 
        CHECK (salary >= 10000),

    status VARCHAR(20) 
        DEFAULT 'ACTIVE'
        CHECK (status IN ('ACTIVE','INACTIVE','SUSPENDED')),

    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

/* -----------------------------------
   Valid Insert
----------------------------------- */
INSERT INTO user_accounts (username, email, age, salary)
VALUES ('john_doe', 'john@gmail.com', 30, 35000);

/* -----------------------------------
   Constraint Violations (Will FAIL)
----------------------------------- */

-- Age below range
INSERT INTO user_accounts (username, email, age, salary)
VALUES ('bad_age', 'bad@gmail.com', 15, 20000);

-- Duplicate email
INSERT INTO user_accounts (username, email, age, salary)
VALUES ('john2', 'john@gmail.com', 28, 30000);

-- Invalid status
INSERT INTO user_accounts (username, email, age, salary, status)
VALUES ('bad_status', 'status@gmail.com', 25, 25000, 'PENDING');

/* -----------------------------------
   View valid records
----------------------------------- */
SELECT * FROM user_accounts;

/* -----------------------------------
   End of Task 12
----------------------------------- */
📘 Professional README.md (GitHub Ready)
# Task 12: Advanced Constraints & Data Validation

## 📌 Project Overview
This task demonstrates how to enforce **strong data validation at the database level** using advanced SQL constraints such as `CHECK`, `UNIQUE`, and `DEFAULT`.  
It ensures that only valid, consistent data can be stored.

---

## 🛠 Tools & Technologies
- **Primary:** MySQL Workbench  
- **Alternatives:** PostgreSQL, BigQuery Sandbox, SQLite  

---

## 📂 Repository Structure
- `task12_advanced_constraints.sql` → SQL schema with constraints  
- `README.md` → Documentation (this file)

---

## 🎯 Learning Objectives
- Use CHECK, UNIQUE, NOT NULL, DEFAULT constraints
- Validate numeric ranges
- Enforce allowed values
- Test and analyze constraint violations
- Understand constraint execution order
- Compare app-level vs DB-level validation

---

## 🗄 Table Used
### `user_accounts`

| Column | Constraint | Purpose |
|--------|-----------|---------|
| user_id | PRIMARY KEY | Unique user |
| username | NOT NULL, UNIQUE | Prevent duplicates |
| email | NOT NULL, UNIQUE | Unique login |
| age | CHECK (18–60) | Valid age |
| salary | CHECK ≥ 10000 | Minimum salary |
| status | DEFAULT + CHECK | Controlled states |
| created_at | DEFAULT | Auto timestamp |

---

## 🧪 SQL Code Examples

### Create Table
```sql
CREATE TABLE user_accounts (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    age INT CHECK (age BETWEEN 18 AND 60),
    salary DECIMAL(10,2) CHECK (salary >= 10000),
    status VARCHAR(20) DEFAULT 'ACTIVE'
        CHECK (status IN ('ACTIVE','INACTIVE','SUSPENDED')),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
Valid Insert
INSERT INTO user_accounts (username, email, age, salary)
VALUES ('john_doe', 'john@gmail.com', 30, 35000);
Constraint Violations
-- Invalid age
INSERT INTO user_accounts (username, email, age, salary)
VALUES ('bad_age', 'bad@gmail.com', 15, 20000);

-- Duplicate email
INSERT INTO user_accounts (username, email, age, salary)
VALUES ('john2', 'john@gmail.com', 28, 30000);

-- Invalid status
INSERT INTO user_accounts (username, email, age, salary, status)
VALUES ('bad_status', 'status@gmail.com', 25, 25000, 'PENDING');
🔍 Key Concepts
Constraint	Purpose
CHECK	Validates data rules
UNIQUE	Prevents duplicates
DEFAULT	Auto assigns values
NOT NULL	Mandatory fields
⚖️ DB vs App Validation
Database Level	Application Level
Strong & enforced	Can be bypassed
Centralized	Spread across apps
Safer	Less secure
