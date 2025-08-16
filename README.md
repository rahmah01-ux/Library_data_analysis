# Library Management System Using SQL Project

# Project Overview

Project Title: Library Mangement System 
Level" Intermediate
Database: library_db


This project demonstrates the implementation of a Library Management System using SQL. It includes creating and managing tables and executing advanced SQL queries.The goal is to demonstrate skills in database designs, manipulation and querying. 

## Objectives
Set up the Library Management System Database: Create and populate the database with tables for branches, employees, members, books, issued status, and return status.
CRUD Operations: Perform Create, Read, Update, and Delete operations on the data.
CTAS (Create Table As Select): Utilise CTAS to create new tables based on query results.
Advanced SQL Queries: Develop complex queries to analyse and retrieve specific data.


## Project Structure
1. Database Setup

<img width="4640" height="2314" alt="database setup " src="https://github.com/user-attachments/assets/50db05ff-ea18-46a4-8ed7-537616205365" />
### Table Creation: Created tables for branches, employees, members, books, issued status, and return status. Each table includes relevant columns and relationships.


-- Library Management System Project 2

-- Creating branch table 
```sql
DROP TABLE IF EXISTS branch;
CREATE TABLE branch
	(
		branch_id VARCHAR(10) PRIMARY KEY,
		manager_id VARCHAR(10),
		branch_address VARCHAR(55),
		contact_no VARCHAR(10)
	);

ALTER TABLE branch 
ALTER COLUMN contact_no TYPE VARCHAR(20);

DROP TABLE IF EXISTS employees;
CREATE TABLE employees
	(   emp_id	VARCHAR(10)PRIMARY KEY,
		emp_name VARCHAR(25),
		position VARCHAR(15),
		salary	INT,
		branch_id VARCHAR(25) -- FK
	);

DROP TABLE IF EXISTS books;
CREATE TABLE books
	( 	isbn VARCHAR(20) PRIMARY KEY,
		book_title VARCHAR(75),
		category VARCHAR(25),
		rental_price FLOAT,
		status VARCHAR(15),
		author VARCHAR(15),
		publisher VARCHAR(55)

	);

ALTER TABLE books
ALTER COLUMN author TYPE VARCHAR(25);


DROP TABLE IF EXISTS members;
CREATE TABLE members
	(
		member_id VARCHAR(20) PRIMARY KEY,	
		member_name VARCHAR(25),
		member_address VARCHAR(75),	
		reg_date DATE
	);


DROP TABLE IF EXISTS issued_status;
CREATE TABLE issued_status 
	(
	issued_id VARCHAR(10) PRIMARY KEY,	
	issued_member_id VARCHAR(10), -- FK
	issued_book_name VARCHAR(75),
	issued_date DATE,
	issued_book_isbn VARCHAR(25), -- FK
	issued_emp_id VARCHAR(10) -- FK
	);

DROP TABLE IF EXISTS return_status;
CREATE TABLE return_status
	(
	return_id VARCHAR(10) PRIMARY KEY,	
	issued_id VARCHAR(10),
	return_book_name VARCHAR(75),
	return_date DATE,
	return_book_isbn VARCHAR(20)
	);
```

## 2. CRUD Operations
Create: Inserted sample records into the books table.
Read: Retrieved and displayed data from various tables.
Update: Updated records in the employees table.
Delete: Removed records from the members table as needed.





### Q1 Create a New Book Record -- "978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.')"

```sql
INSERT INTO books(isbn, book_title, category, rental_price, status, author, publisher)
VALUES
('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');
SELECT * 
FROM books;
```

### Q2 Update an Existing Member's Address
```sql
UPDATE members
SET member_address = '125 Main St'
WHERE member_id = 'C101';
SELECT * 
FROM members;
```

### Q3 Delete a Record from the Issued Status Table -- Objective: Delete the record with issued_id = 'IS121' from the issued_status table.
```sql
DELETE FROM issued_status
WHERE issued_id = 'IS121'
SELECT * 
FROM issued_status;
```
### Q4 Retrieve All Books Issued by a Specific Employee -- Objective: Select all books issued by the employee with emp_id = 'E101'.
```sql
SELECT *
FROM issued_status
WHERE issued_emp_id = 'E101';
```
### Q5 List Members Who Have Issued More Than One Book -- Objective: Use GROUP BY to find members who have issued more than one book.
```sql
SELECT issued_emp_id, COUNT(issued_id) AS total_book_issued
FROM issued_status
GROUP BY issued_emp_id
HAVING COUNT(*)> 1;
```

### Q6 Create Summary Tables: Used CTAS to generate new tables based on query results - each book and total book_issued_cnt**
```sql
CREATE TABLE book_counts 
AS 
SELECT
	b.isbn,
	b.book_title,
	COUNT(ist.issued_id) as no_issued
FROM books AS b
JOIN
issued_status AS ist
ON ist.issued_book_isbn = b.isbn
GROUP BY 1,2;

SELECT *
FROM book_counts;
FROM 
```
## Data Analysis & Findings
The following SQL queries were used to address specific questions:
### Q7 Retrieve All Books in a Specific Category
```sql
SELECT * 
FROM books
WHERE category = 'Classic'
```

### Q8 Find Total Rental Income by Category
```sql
SELECT 
	b.category,
	SUM(b.rental_price),
	COUNT(*)
FROM books AS b
JOIN
issued_status AS ist
ON ist.issued_book_isbn = b.isbn
GROUP BY 1; 
```
### Q9 List Members Who Registered in the Last 500 Days:
```sql
SELECT * 
FROM members
WHERE reg_date >= CURRENT_DATE - INTERVAL '500 days';
```
### Q10 List Employees with Their Branch Manager's Name and their branch details
```sql
SELECT 
	e.*,
	e.emp_id,
	e2.emp_name as manager,
	br.branch_id
FROM employees as e
JOIN branch as br
ON br.branch_id = e.branch_id
JOIN employees as e2
ON br.manager_id = e2.emp_id;
```

### Q11 Create a Table of Books with Rental Price Above a Certain Threshold 7 for example
```sql
CREATE TABLE books_threshhold as
SELECT * 
FROM books
WHERE rental_price > 7


SELECT * 
FROM books_threshhold
```

### Q12 Retrieve the List of Books Not Yet Returned
```sql
SELECT * 
FROM issued_status as ist
LEFT JOIN return_status rs
ON ist.issued_id = rs.issued_id
WHERE rs.return_id IS  NULL
```


### Reports
### Database Schema: Detailed table structures and relationships.
Data Analysis: Insights into book categories, employee salaries, member registration trends, and issued books.
Summary Reports: Aggregated data on high-demand books and employee performance.
### Conclusion
This project demonstrates the application of SQL skills in creating and managing a library management system. It includes database setup, data manipulation, and advanced querying, providing a solid foundation for data management and analysis.


