##### practical 01 

To write DDL Queries to demonstrate the use of objects such as Table, View, Index,

Sequence, Synonym, different constraints(Pk, FK, Not NULL Constraints) etc. Write at least

10 queries on the suitable database application using DML statements. Ref: Employees

(columns: EmployeeID, FirstName, LastName, DepartmentID, Salary, HireDate) Departments

(columns: DepartmentID, DepartmentName)



-- Step 1: Create and use database

CREATE DATABASE IF NOT EXISTS employee;

USE employee;



-- Step 2: Create Tables

CREATE TABLE IF NOT EXISTS departments (

&nbsp;   dept\_id INT NOT NULL PRIMARY KEY,

&nbsp;   department\_name VARCHAR(255)

);



CREATE TABLE IF NOT EXISTS emp (

&nbsp;   id INT NOT NULL PRIMARY KEY,

&nbsp;   firstname VARCHAR(255),

&nbsp;   lastname VARCHAR(255),

&nbsp;   dept\_id INT,

&nbsp;   salary INT,

&nbsp;   hiredate DATE,

&nbsp;   FOREIGN KEY (dept\_id) REFERENCES departments(dept\_id)

);



-- Step 3: Insert Data

INSERT INTO departments VALUES

(10, 'IT'),

(11, 'HR'),

(12, 'Designer');



INSERT INTO emp VALUES

(101, 'Raj', 'Sharma', 10, 25000, '2024-08-07'),

(102, 'Rahul', 'Bhosale', 10, 20000, '2022-08-07'),

(111, 'Om', 'Birla', 11, 30000, '2022-06-05'),

(112, 'Harsh', 'Bhandare', 11, 35000, '2022-06-12'),

(121, 'Sanjay', 'Sharma', 12, 32000, '2020-12-12'),

(122, 'Raj', 'Bhingare', 12, 32500, '2023-08-07');



-- Step 4: Display Tables

SELECT \* FROM emp;

SELECT \* FROM departments;



-- Step 5: Alter Table - Rename Column

ALTER TABLE emp RENAME COLUMN firstname TO first\_name;

SELECT \* FROM emp;



-- Step 6: WHERE Condition

SELECT \* FROM emp WHERE dept\_id = 10;



-- Step 7: ORDER BY Ascending and Descending

SELECT \* FROM emp ORDER BY salary;

SELECT \* FROM emp ORDER BY salary DESC;



-- Step 8: Update Record

UPDATE emp SET first\_name = 'Alice' WHERE id = 111;

SELECT \* FROM emp;



-- Step 9: Modify Data Type

ALTER TABLE emp MODIFY COLUMN salary VARCHAR(255);

DESC emp;



-- Step 10: LIMIT Query

SELECT \* FROM emp LIMIT 2;







##### practical 02 :



To write DML Queries : Write at least 10 queries for suitable database application using DML

statements –CRUD,sort,Aggregation and indexing queries. Ref: Products (columns:

ProductID, ProductName, Category, Price, StockQuantity) Orders (columns: OrderID,

OrderDate) OrderItems (columns: OrderItemID, OrderID, ProductID, Quantity)



-- CREATE DATABASE AND USE IT

CREATE DATABASE dbms;

USE dbms;



-- CREATE TABLES

CREATE TABLE Products (

&nbsp;   ProductID INT PRIMARY KEY,

&nbsp;   ProductName VARCHAR(255) NOT NULL,

&nbsp;   Category VARCHAR(255),

&nbsp;   Price DECIMAL(10, 2),

&nbsp;   StockQuantity INT

);



CREATE TABLE Orders (

&nbsp;   OrderID INT PRIMARY KEY,

&nbsp;   OrderDate DATE

);



CREATE TABLE OrderItems (

&nbsp;   OrderItemID INT PRIMARY KEY,

&nbsp;   OrderID INT,

&nbsp;   ProductID INT,

&nbsp;   Quantity INT,

&nbsp;   FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),

&nbsp;   FOREIGN KEY (ProductID) REFERENCES Products(ProductID)

);



-- INSERT VALUES

INSERT INTO Products (ProductID, ProductName, Category, Price, StockQuantity) VALUES

(1, 'Laptop', 'Electronics', 1500, 100),

(2, 'Smartphone', 'Electronics', 800, 200),

(3, 'Headphones', 'Accessories', 150, 300),

(4, 'Monitor', 'Electronics', 300, 50),

(5, 'Keyboard', 'Accessories', 50, 150);



INSERT INTO Orders (OrderID, OrderDate) VALUES

(1, '2024-08-25'),

(2, '2024-08-26'),

(3, '2024-08-27');



INSERT INTO OrderItems (OrderItemID, OrderID, ProductID, Quantity) VALUES

(1, 1, 1, 2),

(2, 1, 3, 5),

(3, 2, 2, 1),

(4, 2, 4, 2),

(5, 3, 5, 10);



-- DISPLAY ALL TABLES

SELECT \* FROM Products;

SELECT \* FROM Orders;

SELECT \* FROM OrderItems;



-- UPDATE EXAMPLE

UPDATE Products

SET Price = 1600

WHERE ProductID = 1;



-- INSERT EXAMPLE

INSERT INTO Products (ProductID, ProductName, Category, Price, StockQuantity)

VALUES (6, 'Tablet', 'Electronics', 400, 100);



-- DELETE EXAMPLE

DELETE FROM Products WHERE ProductID = 6;



-- ORDER BY STOCK QUANTITY DESCENDING

SELECT \* FROM Products ORDER BY StockQuantity DESC;



-- AGGREGATE QUERY: TOTAL QUANTITY SOLD PER PRODUCT

SELECT p.ProductName, SUM(oi.Quantity) AS TotalSold

FROM Products p

JOIN OrderItems oi ON p.ProductID = oi.ProductID

GROUP BY p.ProductName;



-- CREATE INDEX ON PRODUCT NAME (for faster searches)

CREATE INDEX idx\_productname ON Products (ProductName);



-- ALTERNATIVE 1: UPDATE STOCK QUANTITY BASED ON ORDERS (using subquery)

UPDATE Products p

SET p.StockQuantity = p.StockQuantity - (

&nbsp;   SELECT SUM(oi.Quantity)

&nbsp;   FROM OrderItems oi

&nbsp;   WHERE oi.ProductID = p.ProductID

)

WHERE p.ProductID IN (

&nbsp;   SELECT DISTINCT ProductID FROM OrderItems

);



-- ALTERNATIVE 2: UPDATE STOCK QUANTITY (using JOIN)

UPDATE Products p

JOIN OrderItems oi ON p.ProductID = oi.ProductID

SET p.StockQuantity = p.StockQuantity - oi.Quantity;



-- FINAL DISPLAY WITH ALL DETAILS (JOIN ALL TABLES)

SELECT p.\*, oi.\*, o.\*

FROM Products p

JOIN OrderItems oi ON p.ProductID = oi.ProductID

JOIN Orders o ON oi.OrderID = o.OrderID;





###### practical 03 : 



To write Joins queries : all types of Join, Sub-Query and View Employees (columns:

EmployeeID, FirstName, LastName, DepartmentID, Salary) Departments (columns:

DepartmentID, DepartmentName) Projects (columns: ProjectID, ProjectName)

EmployeeProjects (columns: EmployeeID, ProjectID)





-- use database

USE dbms;



-- CREATE TABLES

CREATE TABLE Departments (

&nbsp;   DepartmentID INT PRIMARY KEY,

&nbsp;   DepartmentName VARCHAR(100)

);



CREATE TABLE Projects (

&nbsp;   ProjectID INT PRIMARY KEY,

&nbsp;   ProjectName VARCHAR(100)

);



CREATE TABLE Employees (

&nbsp;   EmployeeID INT PRIMARY KEY,

&nbsp;   Name VARCHAR(100),

&nbsp;   DepartmentID INT,

&nbsp;   Salary DECIMAL(10, 2),

&nbsp;   FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)

);



CREATE TABLE EmployeeProjects (

&nbsp;   EmployeeID INT,

&nbsp;   ProjectID INT,

&nbsp;   PRIMARY KEY (EmployeeID, ProjectID),

&nbsp;   FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID),

&nbsp;   FOREIGN KEY (ProjectID) REFERENCES Projects(ProjectID)

);



-- INSERT VALUES

INSERT INTO Departments VALUES

(1, 'HR'),

(2, 'IT'),

(3, 'Finance');



INSERT INTO Employees VALUES

(1, 'John Doe', 2, 60000.00),

(2, 'Jane Smith', 1, 65000.00),

(3, 'Robert Brown', 3, 70000.00),

(4, 'Emily Davis', 2, 75000.00);



INSERT INTO Projects VALUES

(1, 'Alpha'),

(2, 'Beta'),

(3, 'Gamma');



INSERT INTO EmployeeProjects VALUES

(1, 1),

(1, 2),

(2, 3),

(3, 1),

(4, 2);



-- DISPLAY ALL TABLES

SELECT \* FROM Employees;

SELECT \* FROM Departments;

SELECT \* FROM Projects;

SELECT \* FROM EmployeeProjects;



-- INNER JOIN (Employees + Departments)

SELECT e.EmployeeID, e.Name, d.DepartmentName

FROM Employees e

INNER JOIN Departments d ON e.DepartmentID = d.DepartmentID;



-- LEFT JOIN (Employees + Projects)

SELECT e.EmployeeID, e.Name, p.ProjectName

FROM Employees e

LEFT JOIN EmployeeProjects ep ON e.EmployeeID = ep.EmployeeID

LEFT JOIN Projects p ON ep.ProjectID = p.ProjectID;



-- RIGHT JOIN (Projects + Employees)

SELECT p.ProjectName, e.Name

FROM Projects p

RIGHT JOIN EmployeeProjects ep ON p.ProjectID = ep.ProjectID

RIGHT JOIN Employees e ON ep.EmployeeID = e.EmployeeID;



-- FULL JOIN (using UNION)

SELECT e.EmployeeID, e.Name, p.ProjectName

FROM Employees e

LEFT JOIN EmployeeProjects ep ON e.EmployeeID = ep.EmployeeID

LEFT JOIN Projects p ON ep.ProjectID = p.ProjectID

UNION

SELECT e.EmployeeID, e.Name, p.ProjectName

FROM Projects p

LEFT JOIN EmployeeProjects ep ON p.ProjectID = ep.ProjectID

LEFT JOIN Employees e ON ep.EmployeeID = e.EmployeeID;



-- SUBQUERY 1: Employees earning above average salary

SELECT \*

FROM Employees

WHERE Salary > (

&nbsp;   SELECT AVG(Salary)

&nbsp;   FROM Employees

);



-- SUBQUERY 2: Departments having more than one employee

SELECT DepartmentID, DepartmentName

FROM Departments

WHERE DepartmentID IN (

&nbsp;   SELECT DepartmentID

&nbsp;   FROM Employees

&nbsp;   GROUP BY DepartmentID

&nbsp;   HAVING COUNT(\*) > 1

);



-- VIEW CREATION

CREATE VIEW EmployeeProjectView AS

SELECT e.EmployeeID, e.Name, p.ProjectName

FROM Employees e

JOIN EmployeeProjects ep ON e.EmployeeID = ep.EmployeeID

JOIN Projects p ON ep.ProjectID = p.ProjectID;



-- DISPLAY VIEW

SELECT \* FROM EmployeeProjectView;





##### practical 04 :



Stored Procedures: Named PL/Block: PL/Stored Procedure. Products (columns: ProductID,

ProductName, Category, Price, StockQuantity) Orders (columns: OrderID, OrderDate)

OrderItems (columns: OrderItemID, OrderID, ProductID, Quantity) Create a PL/stored



/\* ============================

&nbsp;  DBMS PRACTICAL – DDL + DML + PROCEDURE

&nbsp;  ============================ \*/



USE dbms;



/\* ---------------------------------------

&nbsp;  1️⃣ CREATE TABLES (DDL)

--------------------------------------- \*/



-- Product table

CREATE TABLE Product (

&nbsp; ProductID INT AUTO\_INCREMENT PRIMARY KEY,

&nbsp; ProductName VARCHAR(255) NOT NULL,

&nbsp; Category VARCHAR(255),

&nbsp; Price DECIMAL(10,2),

&nbsp; StockQuantity INT

);



-- Order Info table

CREATE TABLE Order\_Info (

&nbsp; OrderID INT AUTO\_INCREMENT PRIMARY KEY,

&nbsp; OrderDate DATE NOT NULL

);



-- Order Item table

CREATE TABLE OrderItem (

&nbsp; OrderItemID INT AUTO\_INCREMENT PRIMARY KEY,

&nbsp; OrderID INT,

&nbsp; ProductID INT,

&nbsp; Quantity INT,

&nbsp; FOREIGN KEY (OrderID) REFERENCES Order\_Info(OrderID),

&nbsp; FOREIGN KEY (ProductID) REFERENCES Product(ProductID)

);



/\* ---------------------------------------

&nbsp;  2️⃣ INSERT SAMPLE DATA (DML)

--------------------------------------- \*/



INSERT INTO Product (ProductName, Category, Price, StockQuantity)

VALUES 

('Laptop', 'Electronics', 1000.00, 50),

('Smartphone', 'Electronics', 500.00, 100),

('Tablet', 'Electronics', 300.00, 30);



/\* ---------------------------------------

&nbsp;  3️⃣ CREATE STORED PROCEDURE

--------------------------------------- \*/



DELIMITER //



CREATE PROCEDURE process\_multiple\_orders(

&nbsp; IN p\_ProductID INT,

&nbsp; IN p\_Quantity INT

)

BEGIN

&nbsp; DECLARE v\_StockQuantity INT;

&nbsp; DECLARE v\_OrderID INT;



&nbsp; /\* 1. Get current stock of given product \*/

&nbsp; SELECT StockQuantity 

&nbsp; INTO v\_StockQuantity 

&nbsp; FROM Product 

&nbsp; WHERE ProductID = p\_ProductID;



&nbsp; /\* 2. Check if enough stock is available \*/

&nbsp; IF v\_StockQuantity >= p\_Quantity THEN

&nbsp; 

&nbsp;   /\* 3. Update stock in Product table \*/

&nbsp;   UPDATE Product

&nbsp;   SET StockQuantity = StockQuantity - p\_Quantity

&nbsp;   WHERE ProductID = p\_ProductID;



&nbsp;   /\* 4. Create new record in Order\_Info table \*/

&nbsp;   INSERT INTO Order\_Info(OrderDate)

&nbsp;   VALUES(CURDATE());



&nbsp;   /\* 5. Fetch last inserted OrderID \*/

&nbsp;   SET v\_OrderID = LAST\_INSERT\_ID();



&nbsp;   /\* 6. Insert corresponding record into OrderItem table \*/

&nbsp;   INSERT INTO OrderItem(OrderID, ProductID, Quantity)

&nbsp;   VALUES(v\_OrderID, p\_ProductID, p\_Quantity);



&nbsp; ELSE

&nbsp;   /\* 7. Throw error if stock is not enough \*/

&nbsp;   SIGNAL SQLSTATE '45000' 

&nbsp;   SET MESSAGE\_TEXT = 'Insufficient stock available';

&nbsp; END IF;



END //



DELIMITER ;



/\* ---------------------------------------

&nbsp;  4️⃣ EXECUTE PROCEDURE (TEST CASES)

--------------------------------------- \*/



CALL process\_multiple\_orders(1, 10);  -- Order 10 Laptops

CALL process\_multiple\_orders(2, 25);  -- Order 25 Smartphones

CALL process\_multiple\_orders(3, 40);  -- Try to order 40 Tablets (will fail)



/\* ---------------------------------------

&nbsp;  5️⃣ CHECK FINAL TABLES

--------------------------------------- \*/



SELECT \* FROM Product;

SELECT \* FROM Order\_Info;

SELECT \* FROM OrderItem;



##### practical 05 :

To create a PL/function that takes a student\&#39;s ID as input and does the following:Calculates

the Grade Point Average (GPA) for the student based on their grades and credit

hours.Returns the GPA as the function\&#39;s result. Student GPA Calculation Consider university

database with the following tables: Students (columns: StudentID, FirstName, LastName)

Courses (columns: CourseID, CourseName) Grades (columns: GradeID, StudentID, CourseID,

Grade, CreditHours)







/\* ==========================================

&nbsp;  DBMS PRACTICAL – FUNCTION (CALCULATE GPA)

&nbsp;  ========================================== \*/



USE dbms;



/\* -----------------------------------------

&nbsp;  1️⃣ CREATE TABLES (DDL)

----------------------------------------- \*/



-- Create Students table

CREATE TABLE Students (

&nbsp; StudentID INT PRIMARY KEY,

&nbsp; Name VARCHAR(100) NOT NULL

);



-- Create Courses table

CREATE TABLE Courses (

&nbsp; CourseID INT PRIMARY KEY,

&nbsp; CourseName VARCHAR(100) NOT NULL

);



-- Create Grades table

CREATE TABLE Grades (

&nbsp; GradeID INT AUTO\_INCREMENT PRIMARY KEY,

&nbsp; StudentID INT,

&nbsp; CourseID INT,

&nbsp; Grade FLOAT NOT NULL,

&nbsp; CreditHours INT NOT NULL,

&nbsp; FOREIGN KEY (StudentID) REFERENCES Students(StudentID),

&nbsp; FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)

);



/\* -----------------------------------------

&nbsp;  2️⃣ INSERT SAMPLE DATA (DML)

----------------------------------------- \*/



INSERT INTO Students VALUES 

(1, 'John Doe'),

(2, 'Jane Smith'),

(3, 'Alice Johnson');



INSERT INTO Courses VALUES 

(101, 'Mathematics'),

(102, 'Physics'),

(103, 'Computer Science');



INSERT INTO Grades (StudentID, CourseID, Grade, CreditHours)

VALUES 

(1, 101, 3.5, 4),

(1, 102, 3.0, 3),

(1, 103, 3.8, 4),

(2, 101, 3.2, 4),

(2, 103, 3.6, 4),

(3, 102, 2.9, 3),

(3, 103, 3.5, 4);



/\* -----------------------------------------

&nbsp;  3️⃣ DEFINE FUNCTION (UDF)

----------------------------------------- \*/



DELIMITER //



CREATE FUNCTION CalculateGPA(p\_StudentID INT)

RETURNS FLOAT

DETERMINISTIC

BEGIN

&nbsp; DECLARE v\_GPA FLOAT;

&nbsp; DECLARE v\_TotalPoints FLOAT;

&nbsp; DECLARE v\_TotalCreditHours INT;



&nbsp; /\* Initialize totals \*/

&nbsp; SET v\_TotalPoints = 0;

&nbsp; SET v\_TotalCreditHours = 0;



&nbsp; /\* Calculate total grade points (Grade × CreditHours) \*/

&nbsp; SELECT SUM(Grade \* CreditHours)

&nbsp; INTO v\_TotalPoints

&nbsp; FROM Grades

&nbsp; WHERE StudentID = p\_StudentID;



&nbsp; /\* Calculate total credit hours \*/

&nbsp; SELECT SUM(CreditHours)

&nbsp; INTO v\_TotalCreditHours

&nbsp; FROM Grades

&nbsp; WHERE StudentID = p\_StudentID;



&nbsp; /\* Compute GPA = TotalPoints / TotalCreditHours \*/

&nbsp; IF v\_TotalCreditHours > 0 THEN

&nbsp;   SET v\_GPA = ROUND(v\_TotalPoints / v\_TotalCreditHours, 2);

&nbsp; ELSE

&nbsp;   SET v\_GPA = NULL; -- No grades available

&nbsp; END IF;



&nbsp; RETURN v\_GPA;

END //



DELIMITER ;



/\* -----------------------------------------

&nbsp;  4️⃣ DISPLAY STUDENT DETAILS WITH GPA

----------------------------------------- \*/



SELECT

&nbsp; s.StudentID,

&nbsp; s.Name AS StudentName,

&nbsp; CalculateGPA(s.StudentID) AS GPA

FROM

&nbsp; Students s;





##### practical 06:

To create a PL/trigger that automatically executes when a new order is added to the Orders

table. The trigger should do the following:Calculate the loyalty points earned for the order

based on the order amount (e.g., 1 point for every $10 spent).Update





USE dbms;



-- CREATE TABLES

CREATE TABLE Customers (

&nbsp; CustomerID INT PRIMARY KEY,

&nbsp; FirstName VARCHAR(50) NOT NULL,

&nbsp; LastName VARCHAR(50) NOT NULL,

&nbsp; TotalPurchases DECIMAL(10, 2) DEFAULT 0.00,

&nbsp; LoyaltyPoints INT DEFAULT 0

);



CREATE TABLE OrderDetails (

&nbsp; OrderID INT AUTO\_INCREMENT PRIMARY KEY,

&nbsp; CustomerID INT,

&nbsp; OrderDate DATE NOT NULL,

&nbsp; OrderAmount DECIMAL(10, 2) NOT NULL,

&nbsp; FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)

);



-- INSERT DATA

INSERT INTO Customers VALUES

(1, 'John', 'Doe', 150.00, 15),

(2, 'Jane', 'Smith', 250.00, 25),

(3, 'Alice', 'Johnson', 100.00, 10);



INSERT INTO OrderDetails (OrderID, CustomerID, OrderDate, OrderAmount) VALUES

(1, 1, '2024-08-26', 50.00),

(2, 2, '2024-08-25', 100.00),

(3, 3, '2024-08-24', 20.00);



-- CREATE TRIGGER

DELIMITER //

CREATE TRIGGER UpdateLoyaltyPoints

AFTER INSERT ON OrderDetails

FOR EACH ROW

BEGIN

&nbsp; DECLARE v\_Points INT;

&nbsp; SET v\_Points = FLOOR(NEW.OrderAmount / 10);

&nbsp; UPDATE Customers

&nbsp; SET LoyaltyPoints = LoyaltyPoints + v\_Points

&nbsp; WHERE CustomerID = NEW.CustomerID;

END //

DELIMITER ;



-- INSERT NEW ORDERS TO TEST TRIGGER

INSERT INTO OrderDetails (CustomerID, OrderDate, OrderAmount)

VALUES (1, '2024-08-26', 100.00);



INSERT INTO OrderDetails (CustomerID, OrderDate, OrderAmount)

VALUES (2, '2024-08-25', 75.00);



INSERT INTO OrderDetails (CustomerID, OrderDate, OrderAmount)

VALUES (3, '2024-08-27', 50.00);



-- DISPLAY UPDATED DATA

SELECT \* FROM Customers;

SELECT \* FROM OrderDetails;



##### practical 07:

To create a PL/block that includes the following components: Cursor Declaration: Declare an

explicit cursor named employee\_cursor to retrieve e employee records from the Employees

table. Cursor FOR LOOP: Use a FOR LOOP to iterate through the employees in the cursor.

-- Use the desired database

USE dbms;



-- 1. CREATE TABLES



CREATE TABLE Employee\_Info (

&nbsp;   EmployeeID INT PRIMARY KEY,

&nbsp;   FirstName VARCHAR(50) NOT NULL,

&nbsp;   LastName VARCHAR(50) NOT NULL,

&nbsp;   Salary DECIMAL(10, 2) NOT NULL

);



CREATE TABLE SalaryUpdates (

&nbsp;   UpdateID INT AUTO\_INCREMENT PRIMARY KEY,

&nbsp;   EmployeeID INT,

&nbsp;   NewSalary DECIMAL(10, 2) NOT NULL,

&nbsp;   UpdateDate DATE NOT NULL,

&nbsp;   FOREIGN KEY (EmployeeID) REFERENCES Employee\_Info(EmployeeID)

);



-- 2. INSERT INITIAL DATA



INSERT INTO Employee\_Info (EmployeeID, FirstName, LastName, Salary)

VALUES 

&nbsp;   (1, 'John', 'Doe', 50000.00),

&nbsp;   (2, 'Jane', 'Smith', 60000.00),

&nbsp;   (3, 'Alice', 'Johnson', 55000.00);



INSERT INTO SalaryUpdates (EmployeeID, NewSalary, UpdateDate)

VALUES 

&nbsp;   (1, 52000.00, '2024-08-26'),

&nbsp;   (3, 58000.00, '2024-08-26');



-- 3. DISPLAY INITIAL DATA



SELECT \* FROM Employee\_Info;

SELECT \* FROM SalaryUpdates;



-- 4. CREATE PROCEDURE TO UPDATE SALARIES



DELIMITER //



CREATE PROCEDURE update\_employee\_salaries()

BEGIN

&nbsp;   DECLARE emp\_id INT;

&nbsp;   DECLARE new\_salary DECIMAL(10,2);

&nbsp;   DECLARE done INT DEFAULT 0;



&nbsp;   -- Declare cursor to iterate through employees

&nbsp;   DECLARE employee\_cursor CURSOR FOR 

&nbsp;       SELECT EmployeeID FROM Employee\_Info;



&nbsp;   -- Handler for end of cursor

&nbsp;   DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;



&nbsp;   -- Open cursor

&nbsp;   OPEN employee\_cursor;



&nbsp;   employee\_loop: LOOP

&nbsp;       -- Fetch employee ID

&nbsp;       FETCH employee\_cursor INTO emp\_id;



&nbsp;       -- Exit loop when no more rows

&nbsp;       IF done = 1 THEN

&nbsp;           LEAVE employee\_loop;

&nbsp;       END IF;



&nbsp;       -- Check for a corresponding SalaryUpdate record

&nbsp;       IF EXISTS (SELECT 1 FROM SalaryUpdates WHERE EmployeeID = emp\_id) THEN

&nbsp;           -- Get the new salary

&nbsp;           SELECT NewSalary INTO new\_salary

&nbsp;           FROM SalaryUpdates

&nbsp;           WHERE EmployeeID = emp\_id;



&nbsp;           -- Update salary if a new one exists

&nbsp;           IF new\_salary IS NOT NULL THEN

&nbsp;               UPDATE Employee\_Info

&nbsp;               SET Salary = new\_salary

&nbsp;               WHERE EmployeeID = emp\_id;

&nbsp;           END IF;

&nbsp;       END IF;

&nbsp;   END LOOP employee\_loop;



&nbsp;   -- Close cursor

&nbsp;   CLOSE employee\_cursor;

END //



DELIMITER ;



-- 5. EXECUTE PROCEDURE \& DISPLAY UPDATED RESULTS



CALL update\_employee\_salaries();



SELECT \* FROM Employee\_Info;







group b 01 : 

To design and Develop MongoDB Queries using CRUD operations. (Use CRUD operations,

SAVE method, logical operators etc.).



// Create Operation (Insert a Document)

db.students.insertMany(\[

&nbsp; { "student\_id": 1, "first\_name": "A", "last\_name": "AA", "age": 20, "major": "Computer Engineering" },

&nbsp; { "student\_id": 2, "first\_name": "B", "last\_name": "BB", "age": 22, "major": "Information Technology" },

&nbsp; { "student\_id": 3, "first\_name": "C", "last\_name": "CC", "age": 21, "major": "Mechanical Engineering" }

])



// Read Operation (Find Documents)

// Find all students:

db.students.find()

// Find a student by student\_id:

db.students.find({ "student\_id": 1 })

// Find students majoring in "Computer Engineering":

db.students.find({ "major": "Computer Engineering" })



// Update Operation (Modify a Document)

// Update a student's major based on their student\_id:

db.students.updateOne(

&nbsp; { "student\_id": 1 },

&nbsp; { $set: { "major": "Software Engineering" } }

)



// Delete Operation (Remove Documents)

// Delete a student by student\_id:

db.students.deleteOne({ "student\_id": 2 })

// Delete all students majoring in "Mechanical Engineering":

db.students.deleteMany({ "major": "Mechanical Engineering" })





group b : 03 

To implement Map reduces operation with suitable example using MongoDB.

db.Products.aggregate(\[

{

$project: {

Category: 1,

TotalSales: { $multiply: \[\&quot;$Price\&quot;, \&quot;$QuantitySold\&quot;] }

}

},

{

$group: {

\_id: \&quot;$Category\&quot;,

TotalSales: { $sum: \&quot;$TotalSales\&quot; }

}

}

])

Output



