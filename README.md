# INFOSYSIT-Employee-Project-Management-System


# 📚 InfosysIT Database Management System  

This project is a **comprehensive SQL-based database system** designed for managing organizational data within **InfosysIT**. It structures and handles essential business operations like **employee management, departments, projects, clients, payroll, attendance, training, asset allocation, and performance reviews**.  

## 🚀 Key Features  

- **Departments Management**: Create and manage company departments and assign managers.  
- **Employee Records**: Maintain detailed records including personal information, salary, and department associations.  
- **Client and Project Tracking**: Manage client information and track projects with timelines and associated departments.  
- **Payroll System**: Automate salary calculations with considerations for bonuses and deductions.  
- **Attendance Tracking**: Log daily check-in and check-out times for employees.  
- **Leave Management**: Track employee leaves, types, and approval statuses.  
- **Training Programs**: Manage employee training details, trainers, and durations.  
- **Asset Management**: Assign and track assets like laptops, mobiles, and furniture.  
- **Performance Reviews**: Record employee performance ratings and comments for appraisal processes.  
- **Relational Integrity**: Enforces data consistency using foreign key constraints and checks.  

## 🛠️ Technologies Used  
- **SQL** for database design and data manipulation.  
- **MySQL** or any other RDBMS compatible with SQL standards.  

## 🔄 Sample Queries  
- Retrieve all employee details along with their respective department names.  
```sql
SELECT e.employeeID, e.firstNAME, e.lastNAME, e.email, e.phone, e.salary, d.departmentNAME 
FROM employees e
JOIN departments d ON e.departmentID = d.departmentID;
```  

## ⚡ How to Use  
1. **Create the Database**:  
```sql
CREATE DATABASE infosysIT;  
USE infosysIT;  
```  

2. **Run the SQL Script**:  
Execute the provided SQL script to set up tables and insert sample data.  

3. **Execute Queries**:  
Run predefined or custom queries to retrieve or manipulate data.


CREATE DATABASE infosysIT;
USE infosysIT;

CREATE TABLE departments (
     departmentID INT PRIMARY KEY AUTO_INCREMENT,
     departmentNAME VARCHAR(100) UNIQUE NOT NULL,
     managerID INT NULL 
);

CREATE TABLE employees (
    employeeID INT PRIMARY KEY AUTO_INCREMENT,
    firstNAME VARCHAR(50) NOT NULL,
    lastNAME VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(15),
    hiredate DATE NOT NULL,
    departmentID INT,
    salary DECIMAL(10,2) CHECK (salary > 0),
    CONSTRAINT fk_department FOREIGN KEY (departmentID) REFERENCES departments(departmentID)
);

ALTER TABLE departments
ADD CONSTRAINT fk_manager FOREIGN KEY (managerID) REFERENCES employees(employeeID);

CREATE TABLE clients (
    clientID INT PRIMARY KEY AUTO_INCREMENT,
    clientNAME VARCHAR(100) NOT NULL,
    contactemail VARCHAR(100) UNIQUE NOT NULL
);

CREATE TABLE projects (
    projectID INT PRIMARY KEY AUTO_INCREMENT,
    projectNAME VARCHAR(100) NOT NULL,
    startdate DATE NOT NULL,
    enddate DATE,
    clientID INT,
    departmentID INT,
    CONSTRAINT fk_project_client FOREIGN KEY (clientID) REFERENCES clients(clientID),
    CONSTRAINT fk_project_department FOREIGN KEY (departmentID) REFERENCES departments(departmentID)
);

CREATE TABLE payroll (
    payrollID INT PRIMARY KEY AUTO_INCREMENT,
    employeeID INT,
    basicsalary DECIMAL(10,2) CHECK (basicsalary > 0),
    bonus DECIMAL(10,2) DEFAULT 0,
    deductions DECIMAL(10,2) DEFAULT 0, 
    netsalary DECIMAL(10,2) GENERATED ALWAYS AS (basicsalary + bonus - deductions) STORED,
    CONSTRAINT fk_payroll_employee FOREIGN KEY (employeeID) REFERENCES employees(employeeID)
);
CREATE table attendance (
    attendanceID int primary key auto_increment,
    employeeID int,
    attendancedate DATE NOT NULL,
    checkintime time not null,
    checkouttime time,
    constraint fk_attendance_employee foreign key (employeeid) references employees(employeeID)
);
create table leaves (
    leaveID int primary key auto_increment,
    employeeID int,
    leavetype enum('sick leave','casual leave','earned leave','maternity leave') not null,
    startdate date not null,
    enddate date not null,
    status enum('pending','approved','rejected') default 'pending',
    constraint fk_leaves_employee foreign key (employeeID) references
    employees(employeeID)
    );
    CREATE TABLE training (
    trainingID INT PRIMARY KEY AUTO_INCREMENT,
    trainingName VARCHAR(100) NOT NULL,
    trainingDate DATE NOT NULL,
    trainerName VARCHAR(100),
    duration INT CHECK (duration > 0), -- in hours
    employeeID INT,
    CONSTRAINT fk_training_employee FOREIGN KEY (employeeID) REFERENCES employees(employeeID)
);
CREATE TABLE assets (
    assetID INT PRIMARY KEY AUTO_INCREMENT,
    assetName VARCHAR(100) NOT NULL,
    assetType ENUM('Laptop', 'Mobile', 'Furniture', 'Other') NOT NULL,
    assignedTo INT,
    assignedDate DATE,
    returnDate DATE,
    status ENUM('Assigned', 'Returned') DEFAULT 'Assigned',
    CONSTRAINT fk_assets_employee FOREIGN KEY (assignedTo) REFERENCES employees(employeeID)
);
CREATE TABLE performance_reviews (
    reviewID INT PRIMARY KEY AUTO_INCREMENT,
    employeeID INT,
    reviewDate DATE NOT NULL,
    rating DECIMAL(2,1) CHECK (rating BETWEEN 1.0 AND 5.0),
    comments TEXT,
    CONSTRAINT fk_reviews_employee FOREIGN KEY (employeeID) REFERENCES employees(employeeID)
);

INSERT INTO departments (departmentNAME) VALUES 
('IT Department'),
('HR Department'),
('Finance Department'),
('Marketing Department');

INSERT INTO employees (firstNAME, lastNAME, email, phone, hiredate, departmentID, salary) VALUES 
('Amaan', 'Hussain', 'amaan.hussain@email.com', '9876543210', '2023-06-01', 1, 75000.00),
('Anuj', 'Maurya', 'anuj.maurya@email.com', '9876543211', '2022-08-15', 2, 60000.00),
('Lavkush', 'Gautam', 'lavkush.gautam@email.com', '9876543212', '2021-10-10', 3, 55000.00),
('Saif', 'Ali', 'saif.ali@email.com', '9876543213', '2020-07-20', 4, 50000.00),
('Hassan', 'Ashraf', 'hassan.ashraf@email.com', '9876543214', '2022-12-05', 1, 65000.00);

INSERT INTO clients (clientNAME, contactemail) VALUES 
('ABC Technologies', 'contact@abctech.com'),
('XYZ Solutions', 'info@xyzsolutions.com'),
('Global IT Services', 'support@globalit.com');

INSERT INTO projects (projectNAME, startdate, enddate, clientID, departmentID) VALUES 
('AI Chatbot Development', '2024-01-01', '2024-06-30', 1, 1),
('Employee HR System', '2023-11-10', '2024-05-15', 2, 2),
('Financial Dashboard', '2024-02-20', '2024-09-01', 3, 3);

INSERT INTO payroll (employeeID, basicsalary, bonus, deductions) VALUES 
(1, 75000.00, 5000.00, 2000.00),
(2, 60000.00, 3000.00, 1500.00),
(3, 55000.00, 2500.00, 1000.00),
(4, 50000.00, 2000.00, 800.00),
(5, 65000.00, 4000.00, 1800.00);

INSERT INTO attendance (employeeID, attendanceDate, checkInTime, checkOutTime) VALUES 
(1, '2024-02-19', '09:00:00', '18:00:00'),
(2, '2024-02-19', '09:15:00', '17:45:00'),
(3, '2024-02-19', '09:30:00', '18:15:00'),
(4, '2024-02-19', '08:45:00', '17:30:00'),
(5, '2024-02-19', '09:10:00', '18:10:00');

INSERT INTO training (trainingName, trainingDate, trainerName, duration, employeeID) VALUES 
('Python Advanced', '2024-03-01', 'Dr. Sharma', 10, 1),
('Leadership Skills', '2024-04-10', 'Ms. Verma', 8, 2),
('Finance Management', '2024-05-20', 'Mr. Gupta', 6, 3),
('Digital Marketing', '2024-06-05', 'Dr. Khan', 5, 4),
('AI & ML Workshop', '2024-07-15', 'Dr. Patel', 12, 5);

INSERT INTO assets (assetName, assetType, assignedTo, assignedDate, returnDate, status) VALUES 
('Dell Laptop', 'Laptop', 1, '2023-06-01', NULL, 'Assigned'),
('iPhone 13', 'Mobile', 2, '2023-08-15', NULL, 'Assigned'),
('Office Chair', 'Furniture', 3, '2023-10-10', NULL, 'Assigned'),
('MacBook Pro', 'Laptop', 4, '2022-07-20', '2024-07-20', 'Returned'),
('Samsung Galaxy S21', 'Mobile', 5, '2022-12-05', NULL, 'Assigned');

INSERT INTO performance_reviews (employeeID, reviewDate, rating, comments) VALUES 
(1, '2024-02-15', 4.5, 'Excellent performance in AI development.'),
(2, '2024-02-10', 4.2, 'Good leadership and HR handling.'),
(3, '2024-01-25', 3.8, 'Average finance management skills.'),
(4, '2024-01-30', 4.0, 'Creative marketing strategies.'),
(5, '2024-02-05', 4.7, 'Exceptional coding and problem-solving skills.');

SELECT e.employeeID, e.firstNAME, e.lastNAME, e.email, e.phone, e.salary, d.departmentNAME 
FROM employees e
JOIN departments d ON e.departmentID = d.departmentID;
