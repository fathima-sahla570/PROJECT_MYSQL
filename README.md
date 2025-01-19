Introduction
The Library Management System is an essential tool for efficiently managing library operations.
This project aims to create a robust database that keeps track of all critical information related to books,
employees, customers, and transactions in a library. 
It facilitates tasks such as book management, customer registration, book issuing, and return tracking. 
By implementing this system, libraries can improve their operational efficiency, reduce manual errors, and ensure seamless management of resources.

The project is structured to create a database named library and design six interrelated tables:
Branch, Employee, Books, Customer, IssueStatus, and ReturnStatus. 
Each table is carefully designed with primary and foreign key constraints to maintain data integrity and establish relationships between entities.
The project also includes SQL queries to extract valuable insights and perform essential operations, such as retrieving book details, tracking employee information, and analyzing customer activity.

This project combines concepts of database management, relational database design, and SQL querying to provide a comprehensive solution for managing a library system.
Structure
-- Step 1: Create the database
CREATE DATABASE library;
USE library;

-- Step 2: Create Branch table
CREATE TABLE Branch (
    Branch_no INT PRIMARY KEY,
    Manager_Id INT,
    Branch_address VARCHAR(255),
    Contact_no VARCHAR(15)
);
INSERT INTO Branch VALUES
(1, 101, '123 Library Street', '9876543210'),
(2, 102, '456 Knowledge Avenue', '9876543211');


-- Step 3: Create Employee table
CREATE TABLE Employee (
    Emp_Id INT PRIMARY KEY,
    Emp_name VARCHAR(100),
    Position VARCHAR(100),
    Salary DECIMAL(10, 2),
    Branch_no INT,
    FOREIGN KEY (Branch_no) REFERENCES Branch(Branch_no)
);
INSERT INTO Employee VALUES
(1, 'John Doe', 'Manager', 60000, 1),
(2, 'Jane Smith', 'Librarian', 50000, 1),
(3, 'Mark Wilson', 'Assistant', 45000, 2);

-- Step 4: Create Books table
CREATE TABLE Books (
    ISBN VARCHAR(13) PRIMARY KEY,
    Book_title VARCHAR(255),
    Category VARCHAR(100),
    Rental_Price DECIMAL(10, 2),
    Status VARCHAR(3) CHECK (Status IN ('yes', 'no')),
    Author VARCHAR(100),
    Publisher VARCHAR(100)
);
INSERT INTO Books VALUES
(1001, 'The Alchemist', 'Fiction', 20.00, 'Yes', 'Paulo Coelho', 'HarperOne'),
(1002, 'Atomic Habits', 'Self-help', 25.00, 'No', 'James Clear', 'Penguin'),
(1003, 'History of Time', 'History', 30.00, 'Yes', 'Stephen Hawking', 'Bantam');


-- Step 5: Create Customer table
CREATE TABLE Customer (
    Customer_Id INT PRIMARY KEY,
    Customer_name VARCHAR(100),
    Customer_address VARCHAR(255),
    Reg_date DATE
);
INSERT INTO Customer VALUES
(1, 'Alice', '789 Reader Lane', '2021-06-15'),
(2, 'Bob', '123 Thinker Avenue', '2022-03-10');

-- Step 6: Create IssueStatus table
CREATE TABLE IssueStatus (
    Issue_Id INT PRIMARY KEY,
    Issued_cust INT,
    Issued_book_name VARCHAR(255),
    Issue_date DATE,
    Isbn_book VARCHAR(13),
    FOREIGN KEY (Issued_cust) REFERENCES Customer(Customer_Id),
    FOREIGN KEY (Isbn_book) REFERENCES Books(ISBN)
);
INSERT INTO IssueStatus VALUES
(1, 1, 'Atomic Habits', '2023-06-10', 1002),
(2, 2, 'The Alchemist', '2023-06-12', 1001);

-- Step 7: Create ReturnStatus table
CREATE TABLE ReturnStatus (
    Return_Id INT PRIMARY KEY,
    Return_cust INT,
    Return_book_name VARCHAR(255),
    Return_date DATE,
    Isbn_book2 VARCHAR(13),
    FOREIGN KEY (Return_cust) REFERENCES Customer(Customer_Id),
    FOREIGN KEY (Isbn_book2) REFERENCES Books(ISBN)
);
INSERT INTO ReturnStatus VALUES
(1, 1, 'Atomic Habits', '2023-07-01', 1002);
select * from books; 
select * from branch;
select * from customer;
select * from employee;
select * from issuestatus;
select * from returnstatus;
-- Queries
-- 1. Retrieve available books
select Book_title , Category , Rental_Price from books WHERE Status = 'Yes';
-- 2. List employees with salaries in descending order
SELECT Emp_name, Salary 
FROM Employee 
ORDER BY Salary DESC;
-- 3. Books issued and their customers
SELECT 
    (SELECT Book_title FROM Books WHERE Books.ISBN = IssueStatus.Isbn_book) AS Book_title,
    (SELECT Customer_name FROM Customer WHERE Customer.Customer_Id = IssueStatus.Issued_cust) AS Customer_name
FROM IssueStatus;
-- 4. Total count of books in each category
SELECT Category, COUNT(*) AS Total_Books 
FROM Books 
GROUP BY Category;
-- 5. Employees with salaries above Rs. 50,000
SELECT Emp_name, Position 
FROM Employee 
WHERE Salary > 50000;
-- 6. Customers registered before 2022 without issued books
SELECT Customer_name 
FROM Customer 
WHERE Reg_date < '2022-01-01' 
  AND Customer_Id NOT IN (SELECT Issued_cust FROM IssueStatus);
-- 7. Branch numbers and total employees in each branch
SELECT Branch_no, COUNT(*) AS Total_Employees 
FROM Employee 
GROUP BY Branch_no;
-- 8. Customers who issued books in June 2023
SELECT Customer_name 
FROM Customer
JOIN IssueStatus ON Customer.Customer_Id = IssueStatus.Issued_cust
WHERE MONTH(Issue_date) = 6 AND YEAR(Issue_date) = 2023;
-- 9. Books containing "history" in title
SELECT Book_title 
FROM Books 
WHERE Book_title LIKE '%history%';
-- 10. Branches with more than 5 employees
SELECT Branch_no, COUNT(*) AS Employee_Count 
FROM Employee 
GROUP BY Branch_no 
HAVING COUNT(*) > 5;
-- 11. Managers and branch addresses
SELECT Emp_name, Branch_address 
FROM Employee 
JOIN Branch ON Employee.Branch_no = Branch.Branch_no
WHERE Position = 'Manager';
-- 12. Customers who issued books with rental price > Rs. 25
SELECT DISTINCT Customer.Customer_name 
FROM Customer
JOIN IssueStatus ON Customer.Customer_Id = IssueStatus.Issued_cust
JOIN Books ON IssueStatus.Isbn_book = Books.ISBN
WHERE Books.Rental_Price > 25;





Conclusion
The Library Management System project demonstrates how databases can be effectively used to organize and manage library resources. 
By implementing the required tables and writing optimized SQL queries, the system ensures smooth operation and accurate record-keeping. 
The project addresses key functionalities such as tracking book availability, managing employee records, handling customer data, and monitoring the issuance and return of books.

The carefully designed database schema ensures data consistency and efficient query execution, providing a scalable solution that can be easily adapted for real-world applications. 
By completing this project, students gain hands-on experience in designing relational databases, applying primary and foreign key constraints, 
and writing complex SQL queries for data manipulation and retrieval.

This project not only serves as an academic exercise but also offers practical insights into real-life database management systems. 
Sharing the completed project on GitHub further demonstrates technical proficiency and fosters collaboration.







