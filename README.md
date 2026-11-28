# EMS

Employee Management System (EMS) - Backend

A backend REST API for managing company employees, built with ASP.NET Core and SQL Server as a practice project for layered (3-tier) design.

Status: In progress. See the Roadmap for what is done and what is planned.

Features
Add employee with full information: first name, last name, phone, email, address, department, salary, gender, date of birth, and start date
Update employee information (the last update time is recorded)
Soft delete: employees are never removed permanently. They are deactivated with a required reason and an automatic date
Departments: employees belong to a department
User accounts and login: role-based access (Admin, HR, Viewer) using JWT authentication
Tech Stack
Area	Technology
Language	C#
Framework	ASP.NET Core Web API
ADO.NET  
Database	SQL Server
Architecture

The solution follows a 3-tier architecture:

EMS.API   ->  EMS.BLL  ->  EMS.DAL
Project	Responsibility
EMS.API	Controllers, request handling, Swagger, startup configuration
EMS.BLL	Business rules, validation, DTOs, services
EMS.DAL	Entities, DbContext, repositories, database access

Dependencies point one way only: the API knows the BLL, and the BLL knows the DAL. The DAL knows nothing about the layers above it.

Database Design

Three main tables in the EMS database:

Departments
Column	Type	Notes
DepartmentId	INT	PK, Identity
DepartmentName	NVARCHAR	Required
Employees
Column	Type	Notes
EmployeeId	INT	PK, Identity
FirstName	NVARCHAR(50)	Required
LastName	NVARCHAR(50)	Required
Phone	VARCHAR(20)	Optional
Email	VARCHAR(100)	Required, unique
Address	NVARCHAR(200)	Optional
Salary	DECIMAL(10,2)	Required, greater than 0
Gender	TINYINT	Required
DateOfBirth	DATE	Required
StartDate	DATE	Required
IsActive	BIT	Default 1
DeactivationReason	NVARCHAR(200)	Optional, required when deactivating
DeactivationDate	DATETIME2(0)	Set automatically on deactivation
UpdatedAt	DATETIME2(0)	Set on update
DepartmentId	INT	FK to Departments
Users
Column	Type	Notes
UserId	INT	PK, Identity
Username	VARCHAR(50)	Required, unique
PasswordHash	VARCHAR(255)	Hashed, never plain text
Role	TINYINT	Admin, HR, Viewer
IsActive	BIT	Default 1
CreatedAt	DATETIME2(0)	Default current UTC time
EmployeeId	INT	FK to Employees, unique
Relationships
One Department has many Employees
One User belongs to one Employee (one-to-one)
Functional Requirements
ID	Requirement
FR-1	Add a new employee with validation (valid unique email, existing department, positive salary, valid dates)
FR-2	Update employee information
FR-3	Deactivate an employee (soft delete) with a reason and date
FR-4	Store users and control login permissions by role
Getting Started


Author

Ahmed Ghoraib
