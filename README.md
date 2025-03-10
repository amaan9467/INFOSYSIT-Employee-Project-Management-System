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
