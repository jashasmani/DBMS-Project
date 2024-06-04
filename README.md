# DBMS-Project

# Payroll Management System

## Developed By:
IT008 - Jash Asmani

## Guided By:
Prof. Pooja Makwana  
Department of Information Technology  
Faculty of Technology  
Dharmsinh Desai University

---

## Table of Contents

1. [System Overview](#system-overview)
   - [Current System](#current-system)
   - [Advantages of the Proposed System](#advantages-of-the-proposed-system)
2. [E-R Diagram](#e-r-diagram)
3. [Relational Schema](#relational-schema)
4. [Data Dictionary](#data-dictionary)
   - [Company](#company)
   - [Department](#department)
   - [Employee](#employee)
   - [Project](#project)
   - [Payroll](#payroll)
   - [Password](#password)
   - [Admin](#admin)
   - [Account](#account)
   - [Paygrade](#paygrade)
   - [Leaves](#leaves)
   - [Casual Leave](#casual-leave)
   - [Earned Leave](#earned-leave)
   - [Medical Leave](#medical-leave)
   - [Maternity Leave](#maternity-leave)
5. [Database Implementation](#database-implementation)
   - [Create Schema](#create-schema)
   - [Insert Data Values](#insert-data-values)
6. [Queries](#queries)
   - [Basic Queries](#basic-queries)
   - [Joins & SubQueries](#joins-subqueries)
7. [Functions and Triggers](#functions-and-triggers)
   - [Functions](#functions)
   - [Triggers](#triggers)
8. [Cursor](#cursor)

---

## System Overview

### Current System
The current payroll management system involves manual processes which are time-consuming and prone to errors. This project aims to automate the payroll system to improve efficiency and accuracy.

### Advantages of the Proposed System
- Automated payroll calculations
- Reduced manual errors
- Efficient management of employee information
- Enhanced data security
- Simplified record-keeping and reporting

## E-R Diagram
(Insert E-R Diagram here)

## Relational Schema
(Insert Relational Schema here)

## Data Dictionary

### Company
- CID: Company ID (Primary Key)
- CNAME: Company Name
- ADDRESS: Address
- MOBNO: Mobile Number
- EMAIL: Email

### Department
- DID: Department ID (Primary Key)
- DNAME: Department Name
- MANAGER: Manager Name
- LOCATION: Location

### Employee
- EID: Employee ID (Primary Key)
- ENAME: Employee Name
- STATE: State
- MOB_NO: Mobile Number
- EMAIL: Email
- DID: Department ID (Foreign Key)

### Project
- PRO_NO: Project Number (Primary Key)
- PRO_NAME: Project Name
- PRO_BUDGET: Project Budget
- PRO_START: Project Start Date
- PRO_END: Project End Date
- CONTROLLING_DEPT: Controlling Department
- EID: Employee ID (Foreign Key)
- T_ID: Transaction ID (Foreign Key)

### Payroll
- I_ID: Payroll ID (Primary Key)
- M_SALARY: Monthly Salary
- Y_SALARY: Yearly Salary
- ENAME: Employee Name

### Password
- U_ID: User ID (Primary Key)
- PASSWORD: Password
- COMMPASS: Common Password

### Admin
- A_ID: Admin ID
- ANAME: Admin Name
- A_EMAIL: Admin Email
- U_ID: User ID (Foreign Key)

### Account
- ACC_NO: Account Number (Primary Key)
- BANK_NAME: Bank Name
- BRANCH: Branch
- AMT: Amount
- CID: Company ID (Foreign Key)

### Paygrade
- G_ID: Grade ID (Primary Key)
- BASIC_GRADE: Basic Grade
- DA: Dearness Allowance
- HRA: House Rent Allowance
- TA: Travel Allowance
- MA: Medical Allowance

### Leaves
- LEAVE_ID: Leave ID (Primary Key)
- LEAVE_DATE: Leave Date
- TOTAL: Total Leaves
- EID: Employee ID (Foreign Key)

### Casual Leave
- CASUAL_ID: Casual Leave ID (Primary Key)
- EVENT: Event
- LEAVE_ID: Leave ID (Foreign Key)

### Earned Leave
- EARN_ID: Earned Leave ID (Primary Key)
- NUMBER: Number of Leaves
- LEAVE_ID: Leave ID (Foreign Key)

### Medical Leave
- MED_ID: Medical Leave ID (Primary Key)
- DISEASE: Disease
- LEAVE_ID: Leave ID (Foreign Key)

### Maternity Leave
- M_ID: Maternity Leave ID (Primary Key)
- DURATION: Duration
- LEAVE_ID: Leave ID (Foreign Key)

## Database Implementation

### Create Schema
```sql
CREATE TABLE COMPANY009 (
  CID VARCHAR(10) PRIMARY KEY,
  CNAME VARCHAR(20) NOT NULL,
  ADDRESS VARCHAR(25),
  MOBNO NUMERIC(10),
  EMAIL VARCHAR(15)
);

CREATE TABLE DEPARTMENT (
  DID VARCHAR(20) PRIMARY KEY,
  DNAME VARCHAR(20),
  MANAGER VARCHAR(20),
  LOCATION VARCHAR(20)
);

CREATE TABLE EMPLOYEE (
  EID VARCHAR(10) PRIMARY KEY,
  ENAME VARCHAR(20),
  STATE VARCHAR(20),
  MOB_NO NUMERIC(10),
  EMAIL VARCHAR(20),
  DID VARCHAR(20) REFERENCES DEPARTMENT(DID)
);

CREATE TABLE PROJECT (
  PRO_NO VARCHAR(20) PRIMARY KEY,
  PRO_NAME VARCHAR(20),
  PRO_BUDGET NUMERIC(7,2),
  PRO_START DATE NOT NULL,
  PRO_END DATE NOT NULL,
  CONTROLLING_DEPT VARCHAR(20),
  EID VARCHAR(20) REFERENCES EMPLOYEE(EID),
  I_ID VARCHAR(20) REFERENCES PAYROLL(I_ID)
);

CREATE TABLE PAYROLL (
  I_ID VARCHAR(10) PRIMARY KEY,
  M_SALARY NUMERIC(7,2),
  Y_SALARY NUMERIC(7,2),
  ENAME VARCHAR(20) NOT NULL
);

CREATE TABLE PASSWORD (
  U_ID VARCHAR(12) PRIMARY KEY,
  PASSWORD VARCHAR(15),
  COMMPASS VARCHAR(20)
);

CREATE TABLE ADMIN (
  A_ID VARCHAR(10),
  ANAME VARCHAR(10),
  A_EMAIL VARCHAR(15),
  U_ID VARCHAR(15) REFERENCES PASSWORD(U_ID)
);

CREATE TABLE ACCOUNT (
  ACC_NO NUMERIC(12) PRIMARY KEY,
  BANK_NAME VARCHAR(15),
  BRANCH VARCHAR(10),
  AMT NUMERIC(15),
  CID VARCHAR(15) REFERENCES COMPANY009(CID)
);

CREATE TABLE PAYGRADE (
  G_ID VARCHAR(20) PRIMARY KEY,
  BASIC_GRADE NUMERIC(8,2),
  DA NUMERIC(8,2),
  HRA NUMERIC(8,2),
  TA NUMERIC(8,2),
  MA NUMERIC(8,2)
);

CREATE TABLE LEAVES (
  LEAVE_ID VARCHAR(10) PRIMARY KEY,
  LEAVE_DATE DATE NOT NULL,
  TOTAL NUMERIC(4),
  EID VARCHAR(20) REFERENCES EMPLOYEE(EID)
);

CREATE TABLE CASUAL_LEAVE (
  CASUAL_ID VARCHAR(20) PRIMARY KEY,
  EVENT VARCHAR(20),
  LEAVE_ID VARCHAR(12) REFERENCES LEAVES(LEAVE_ID)
);

CREATE TABLE EARNED_LEAVE (
  EARN_ID VARCHAR(12) PRIMARY KEY,
  NUMBER NUMERIC(5) NOT NULL,
  LEAVE_ID VARCHAR(12) REFERENCES LEAVES(LEAVE_ID)
);

CREATE TABLE MEDICAL_LEAVE (
  MED_ID VARCHAR(12) PRIMARY KEY,
  DISEASE VARCHAR(14),
  LEAVE_ID VARCHAR(12) REFERENCES LEAVES(LEAVE_ID)
);

CREATE TABLE MATERNITY_LEAVE (
  M_ID VARCHAR(12) PRIMARY KEY,
  DURATION VARCHAR(20) NOT NULL,
  LEAVE_ID VARCHAR(12) REFERENCES LEAVES(LEAVE_ID)
);
```

### Insert Data Values
```sql
-- Company
INSERT INTO COMPANY009 (CID, CNAME, ADDRESS, MOBNO, EMAIL) VALUES
('C101', 'CRESTDATA', 'AHMEDABAD', 9090616123, 'CRamd@GMAIL.COM'),
('C102', 'INFOCHIPS', 'PUNE', 8989787812, 'INPUN@GMAIL.COM'),
('C103', 'WIPRO', 'MUMBAI', 1234567890, 'QR@GMAIL.COM'),
('C104', 'TCS', 'BANGALORE', 7041497127, 'TBAN@GMAIL.COM'),
('C105', 'WIPRO', 'HYDERABAD', 9685741236, 'WHYD@GMAIL.COM');

-- Department
INSERT INTO DEPARTMENT (DID, DNAME, MANAGER, LOCATION) VALUES
('D101', 'Marketing', 'MAYUR', 'TOWER1'),
('D120', 'Operations', 'SURESH', 'TOWER2'),
('D103', 'Finance', 'MAHESH', 'TOWER3'),
('D104', 'Sales', 'KIRIRT', 'TOWER4'),
('D105', 'IT', 'RAMESH', 'TOWER5');

-- Payroll
INSERT INTO PAYROLL (I_ID, M_SALARY, Y_SALARY, ENAME) VALUES
('I101', 5000.00, 6000.00, '

MAHESH'),
('I102', 7000.00, 9000.00, 'NISARG'),
('I103', 10000.00, 12000.00, 'SHYAM'),
('I104', 15000.00, 19000.00, 'RAM');

-- Employee
INSERT INTO EMPLOYEE (EID, ENAME, STATE, MOB_NO, EMAIL, DID) VALUES
('E101', 'JASH', 'GUJARAT', 9824358212, 'IT.JASH@GMAIL.COM', 'D101'),
('E102', 'NISARG', 'MP', 9724358212, 'IT.NISARG@GMAIL.COM', 'D102'),
('E103', 'TIRTH', 'MH', 9824356212, 'IT.TIRTH@GMAIL.COM', 'D103'),
('E104', 'AKSHAR', 'DELHI', 9829358212, 'IT.AKSHAR@GMAIL.COM', 'D104');

-- Project
INSERT INTO PROJECT (PRO_NO, PRO_NAME, PRO_BUDGET, PRO_START, PRO_END, CONTROLLING_DEPT, EID, I_ID) VALUES
('P101', 'OASIS', 800000.00, '2015-03-10', '2015-04-10', 'D120', 'E101', 'I101'),
('P102', 'OPERATIONS', 500000.00, '2015-03-11', '2015-04-15', 'D103', 'E102', 'I102'),
('P103', 'PMA', 900000.00, '2015-05-01', '2015-06-20', 'D104', 'E103', 'I103'),
('P104', 'RTM', 1000000.00, '2015-07-01', '2015-08-30', 'D105', 'E104', 'I104');
```

## Queries

### Basic Queries
```sql
-- Query to select all employees
SELECT * FROM EMPLOYEE;

-- Query to find the total payroll expenses
SELECT SUM(M_SALARY) AS Total_Payroll_Expenses FROM PAYROLL;

-- Query to find employee details with specific department ID
SELECT * FROM EMPLOYEE WHERE DID = 'D101';
```

### Joins & SubQueries
```sql
-- Join query to find employee details along with their department name
SELECT EMPLOYEE.ENAME, DEPARTMENT.DNAME 
FROM EMPLOYEE 
JOIN DEPARTMENT ON EMPLOYEE.DID = DEPARTMENT.DID;

-- SubQuery to find the employee with the highest salary
SELECT ENAME 
FROM PAYROLL 
WHERE M_SALARY = (SELECT MAX(M_SALARY) FROM PAYROLL);
```

## Functions and Triggers

### Functions
```sql
-- Example function to calculate annual salary
CREATE FUNCTION calculate_annual_salary(monthly_salary NUMERIC)
RETURNS NUMERIC AS $$
BEGIN
  RETURN monthly_salary * 12;
END;
$$ LANGUAGE plpgsql;
```

### Triggers
```sql
-- Example trigger to update yearly salary when monthly salary is updated
CREATE TRIGGER update_yearly_salary
AFTER UPDATE OF M_SALARY ON PAYROLL
FOR EACH ROW
BEGIN
  NEW.Y_SALARY := NEW.M_SALARY * 12;
END;
```

## Cursor
```sql
-- Example cursor to iterate over employees
DECLARE employee_cursor CURSOR FOR
SELECT EID, ENAME FROM EMPLOYEE;

BEGIN
  OPEN employee_cursor;
  FETCH NEXT FROM employee_cursor INTO @eid, @ename;
  WHILE @@FETCH_STATUS = 0
  BEGIN
    -- Process each employee
    FETCH NEXT FROM employee_cursor INTO @eid, @ename;
  END;
  CLOSE employee_cursor;
  DEALLOCATE employee_cursor;
END;
```

---

This README provides a detailed overview of the Payroll Management System, including its structure, schema, and example SQL scripts for creating and managing the database.
