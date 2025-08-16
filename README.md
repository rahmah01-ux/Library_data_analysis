## Library Management System Using SQL Project

# Project Overview

Project Title: Library Mangement System 
Level" Intermediate
Database: library_db


This project demonstrates the implementation of a Library Management System using SQL. It includes creating and managing tables and executing advanced SQL queries.The goal is to demonstrate skills in database designs, manipulation and querying. 

# Objectives
-Set up the Library Management System Database: Create and populate the database with tables for branches, employees, members, books, issued status, and return status.
-CRUD Operations: Perform Create, Read, Update, and Delete operations on the data.
-CTAS (Create Table As Select): Utilise CTAS to create new tables based on query results.
-Advanced SQL Queries: Develop complex queries to analyse and retrieve specific data.


Project Structure
1. Database Setup

![ER Diagram](images/er_diagram.png)




--Q1 Create a New Book Record -- "978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.')"

INSERT INTO books(isbn, book_title, category, rental_price, status, author, publisher)
VALUES
('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');
SELECT * 
FROM books;

--Q2 Update an Existing Member's Address

UPDATE members
SET member_address = '125 Main St'
WHERE member_id = 'C101';
SELECT * 
FROM members;

--Q3 Delete a Record from the Issued Status Table -- Objective: Delete the record with issued_id = 'IS121' from the issued_status table.

DELETE FROM issued_status
WHERE issued_id = 'IS121'
SELECT * 
FROM issued_status;

-- Q4 Retrieve All Books Issued by a Specific Employee -- Objective: Select all books issued by the employee with emp_id = 'E101'.
SELECT *
FROM issued_status
WHERE issued_emp_id = 'E101';

--Q4 List Members Who Have Issued More Than One Book -- Objective: Use GROUP BY to find members who have issued more than one book.

SELECT issued_emp_id, COUNT(issued_id) AS total_book_issued
FROM issued_status
GROUP BY issued_emp_id
HAVING COUNT(*)> 1;


--Q5 Create Summary Tables: Used CTAS to generate new tables based on query results - each book and total book_issued_cnt**

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


--Q7 Retrieve All Books in a Specific Category

SELECT * 
FROM books
WHERE category = 'Classic'


--Q8 Find Total Rental Income by Category

SELECT 
	b.category,
	SUM(b.rental_price),
	COUNT(*)
FROM books AS b
JOIN
issued_status AS ist
ON ist.issued_book_isbn = b.isbn
GROUP BY 1; 

--Q9 List Members Who Registered in the Last 500 Days:

SELECT * 
FROM members
WHERE reg_date >= CURRENT_DATE - INTERVAL '500 days';

--Q10 List Employees with Their Branch Manager's Name and their branch details

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


--Q11 Create a Table of Books with Rental Price Above a Certain Threshold 7 for example
CREATE TABLE books_threshhold as
SELECT * 
FROM books
WHERE rental_price > 7


SELECT * 
FROM books_threshhold


--Q12 Retrieve the List of Books Not Yet Returned

SELECT * 
FROM issued_status as ist
LEFT JOIN return_status rs
ON ist.issued_id = rs.issued_id
WHERE rs.return_id IS  NULL
