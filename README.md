# Database Design Practice

Welcome to **Database Design Practice**!  
This repository is built to help you learn database design by practice.

---

## Table of Contents
- [Purpose](#purpose)  
- [Who is this for?](#who-is-this-for)  
- [How to Use](#how-to-use)  
- [Problem 01: Company Employees and Departments](#problem-01-company-employees-and-departments)  
- [Solution 01: Company Employees and Departments](#solution-01-company-employees-and-departments)  

---

## Purpose
- Practice creating **Entity Relationship Diagrams (ERDs)**  
- Work on **simple, real-world scenarios**  
- Build skills step by step with **guided problems and solutions**  

---

## Who is this for?
- Absolute beginners in database design  
- Students or self-learners looking for hands-on ERD practice  
- Anyone who wants to strengthen their fundamentals in database design  

---

## How to Use
All problems and solutions are included directly in this README.  

1. Read a **Problem** section.  
2. Try to solve it on your own by identifying entities, attributes, and relationships.  
3. Draw your own ERD diagram.  
4. Scroll down to the matching **Solution** section to compare with the provided answer.  

---

## Problem 01: Company Employees and Departments

### Description
A company wants to store information about its employees and departments.  
You are asked to design an ERD for the following scenario:

- Each employee has: SSN, First Name, Last Name, Birth Date, and Gender.  
- Each department has: Department Number (unique), Department Name, and Location(s).  
- Each employee works in exactly one department.  
- A department can have multiple employees.  
- Each department has one manager.  

### Requirements
1. Identify the entities.  
2. Identify the attributes for each entity.  
3. Define the relationships between entities.  
4. Draw the ERD diagram.  
5. Map the ERD to relational schema.  
6. Write SQL code in PostgreSQL to create the schema.  

---

## Solution 01: Company Employees and Departments

### Entities and Attributes
- **Employee**
  - SSN (Primary Key)  
  - First Name  
  - Last Name  
  - Birth Date  
  - Gender  

- **Department**
  - Department Number (Primary Key)  
  - Department Name  
  - Location  

### Relationships
- **Works_In**: Each employee works in one department (1:N).  
- **Manages**: Each department has one manager (1:1 between Department and Employee).  

### ERD Diagram
![Problem 1 Chen Solution](assets/Problem1ChenSolution.png)

### ER-to-Relational Mapping
- **Employee(SSN, FirstName, LastName, BirthDate, Gender, DeptNo)**  
- **Department(DeptNo, DeptName, Location, ManagerSSN)**  

### PostgreSQL Implementation
```sql
-- Create Department table
CREATE TABLE Department (
    DeptNo SERIAL PRIMARY KEY,
    DeptName VARCHAR(100) NOT NULL,
    Location VARCHAR(100),
    ManagerSSN CHAR(9) UNIQUE
);

-- Create Employee table
CREATE TABLE Employee (
    SSN CHAR(9) PRIMARY KEY,
    FirstName VARCHAR(50) NOT NULL,
    LastName VARCHAR(50) NOT NULL,
    BirthDate DATE NOT NULL,
    Gender CHAR(1) CHECK (Gender IN ('M', 'F')),
    DeptNo INT NOT NULL,
    FOREIGN KEY (DeptNo) REFERENCES Department(DeptNo),
    CONSTRAINT fk_manager FOREIGN KEY (SSN) REFERENCES Department(ManagerSSN)
);
```

---

## Problem 02: Library Books and Members

### Description
A small library wants to store information about its books and members.  
You are asked to design an ERD for the following scenario:

- Each book has: ISBN, Title, Author, and Publication Year.  
- Each member has: Member ID, Name, and Email.  
- A member can borrow many books.  
- A book can only be borrowed by one member at a time.  

### Requirements
1. Identify the entities.  
2. Identify the attributes for each entity.  
3. Define the relationships between entities.  
4. Draw the ERD diagram.  
5. Map the ERD to relational schema.  
6. Write SQL code in PostgreSQL to create the schema.  

---

## Solution 02: Library Books and Members

### Entities and Attributes
- **Book**
  - ISBN (Primary Key)  
  - Title  
  - Author  
  - PubYear  

- **Member**
  - MemberID (Primary Key)  
  - Name  
  - Email  

### Relationships
- **Borrows**: A member can borrow many books (1:N).  
- Each book is borrowed by at most one member.  

### ERD Diagram
![Problem 2 Chen Solution](assets/Problem2ChenSolution.png)

### ER-to-Relational Mapping
- **Book(ISBN, Title, Author, PubYear, MemberID)**  
- **Member(MemberID, Name, Email)**  

### PostgreSQL Implementation
```sql
-- Create Member table
CREATE TABLE Member (
    MemberID SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE NOT NULL
);

-- Create Book table
CREATE TABLE Book (
    ISBN CHAR(13) PRIMARY KEY,
    Title VARCHAR(200) NOT NULL,
    Author VARCHAR(100) NOT NULL,
    PubYear INT,
    MemberID INT,
    FOREIGN KEY (MemberID) REFERENCES Member(MemberID)
);
```

---

## Problem 03: Students, Courses, and Enrollments

### Description
A university wants to store information about its students, courses, and enrollments.  
You are asked to design an ERD for the following scenario:

- Each student has: Student ID, Name, Major, and Email.  
- Each course has: Course ID, Course Name, and Credits.  
- Students can enroll in many courses.  
- A course can have many students.  
- Each enrollment also stores the **Grade** the student received.  

### Requirements
1. Identify the entities.  
2. Identify the attributes for each entity.  
3. Define the relationships between entities.  
4. Draw the ERD diagram.  
5. Map the ERD to relational schema.  
6. Write SQL code in PostgreSQL to create the schema.  

---

## Solution 03: Students, Courses, and Enrollments

### Entities and Attributes
- **Student**
  - StudentID (Primary Key)  
  - Name  
  - Major  
  - Email  

- **Course**
  - CourseID (Primary Key)  
  - CourseName  
  - Credits  

- **Enrollment**
  - StudentID (Foreign Key)  
  - CourseID (Foreign Key)  
  - Grade  

### Relationships
- **Enrolls_In**:  
  - A student can enroll in many courses.  
  - A course can have many students.  
  - This is a **many-to-many (M:N)** relationship, resolved with the **Enrollment** table.  

### ERD Diagram
![Problem 3 Chen Solution](assets/Problem3ChenSolution.png)

### ER-to-Relational Mapping
- **Student(StudentID, Name, Major, Email)**  
- **Course(CourseID, CourseName, Credits)**  
- **Enrollment(StudentID, CourseID, Grade)**  

### PostgreSQL Implementation
```sql
-- Create Student table
CREATE TABLE Student (
    StudentID SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Major VARCHAR(100),
    Email VARCHAR(100) UNIQUE NOT NULL
);

-- Create Course table
CREATE TABLE Course (
    CourseID SERIAL PRIMARY KEY,
    CourseName VARCHAR(100) NOT NULL,
    Credits INT CHECK (Credits > 0)
);

-- Create Enrollment table (M:N relationship)
CREATE TABLE Enrollment (
    StudentID INT,
    CourseID INT,
    Grade CHAR(2),
    PRIMARY KEY (StudentID, CourseID),
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Course(CourseID)
);
```
---

## Problem 04: Hospital Management System


### Description
A hospital wants to manage data about doctors, patients, and appointments.  
You are asked to design an ERD for the following scenario:

- Each doctor has: DoctorID, Name, Specialty, and Phone.  
- Each patient has: PatientID, Name, Address, and Phone.  
- A doctor can have many appointments, but an appointment must belong to exactly one doctor.  
- A patient can have many appointments, but an appointment must belong to exactly one patient.  
- An appointment cannot exist without being linked to both a doctor and a patient.  
- Each appointment has: AppointmentID, Date, and Time.  

### Requirements
1. Identify the entities.  
2. Identify the attributes for each entity.  
3. Define the relationships between entities (detect the cardinality and participation).  
4. Draw the ERD diagram.  
5. Map the ERD to relational schema.  
6. Write SQL code in PostgreSQL to create the schema.  

---


## Solution 04: Hospital Management System

### Entities and Attributes
- **Doctor**
  - DoctorID (Primary Key)  
  - Name  
  - Specialty  
  - Phone  

- **Patient**
  - PatientID (Primary Key)  
  - Name  
  - Address  
  - Phone  

- **Appointment**
  - AppointmentID (Primary Key)  
  - Date  
  - Time  
  - DoctorID (Foreign Key)  
  - PatientID (Foreign Key)  

### Relationships
- **Doctor–Appointment**: 1:N  
- **Patient–Appointment**: 1:N  

### ERD Diagram
![Problem 4 Chen Solution](assets/Problem4ChenSolution.png)

### ER-to-Relational Mapping
- **Doctor(DoctorID, Name, Specialty, Phone)**  
- **Patient(PatientID, Name, Address, Phone)**  
- **Appointment(AppointmentID, Date, Time, DoctorID, PatientID)**  

### PostgreSQL Implementation
```sql
-- Create Doctor table
CREATE TABLE Doctor (
    DoctorID SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Specialty VARCHAR(100),
    Phone VARCHAR(20) UNIQUE
);

-- Create Patient table
CREATE TABLE Patient (
    PatientID SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Address VARCHAR(200),
    Phone VARCHAR(20) UNIQUE
);

-- Create Appointment table
CREATE TABLE Appointment (
    AppointmentID SERIAL PRIMARY KEY,
    Date DATE NOT NULL,
    Time TIME NOT NULL,
    DoctorID INT NOT NULL,
    PatientID INT NOT NULL,
    FOREIGN KEY (DoctorID) REFERENCES Doctor(DoctorID),
    FOREIGN KEY (PatientID) REFERENCES Patient(PatientID)
);
```
---

## Problem 05: Online Shopping System  

### Description  
An online shopping platform wants to store information about customers, products, orders, and payments.  
You are asked to design an ERD for the following scenario:  

- Each customer has: CustomerID, Name, Email, and Phone Numbers (a customer can have multiple phone numbers).  
- Each product has: ProductID, ProductName, Price, and StockQuantity.  
- A customer can place many orders, but each order must belong to exactly one customer.  
- An order can include many products, and a product can appear in many orders. For each product in an order, the system stores the Quantity purchased.  
- Each order has: OrderID and OrderDate.  
- Each order must have one payment, and a payment must be linked to exactly one order.  
- A payment has: PaymentID, PaymentDate, and Amount.  

### Requirements  
1. Identify the entities.  
2. Identify the attributes for each entity.  
3. Identify any multi-valued attributes.  
4. Define the relationships between entities (detect the cardinality and participation).  
5. Draw the ERD diagram.  
6. Map the ERD to relational schema.  
7. Write SQL code in PostgreSQL to create the schema.  

---

## Solution 05: Online Shopping System  

### Entities and Attributes  
- **Customer**  
  - CustomerID (Primary Key)  
  - Name  
  - Email  
  - PhoneNumbers (Multi-valued)  

- **Product**  
  - ProductID (Primary Key)  
  - ProductName  
  - Price  
  - StockQuantity  

- **Order**  
  - OrderID (Primary Key)  
  - OrderDate  
  - CustomerID (Foreign Key)  

- **Payment**  
  - PaymentID (Primary Key)  
  - PaymentDate  
  - Amount  
  - OrderID (Foreign Key, Unique)  

- **OrderProduct (Associative Entity for M:N)**  
  - OrderID (Foreign Key)  
  - ProductID (Foreign Key)  
  - Quantity  

### Relationships  
- **Customer–Order**: 1:N  
- **Order–Product**: M:N (with Quantity attribute)  
- **Order–Payment**: 1:1  

### ERD Diagram  
![Problem 5 Chen Solution](assets/Problem5ChenSolution.png)  

### ER-to-Relational Mapping  
- **Customer(CustomerID, Name, Email)**  
- **CustomerPhone(CustomerID, PhoneNumber)**  
- **Product(ProductID, ProductName, Price, StockQuantity)**  
- **Order(OrderID, OrderDate, CustomerID)**  
- **Payment(PaymentID, PaymentDate, Amount, OrderID)**  
- **OrderProduct(OrderID, ProductID, Quantity)**  

### PostgreSQL Implementation  
```sql
-- Create Customer table
CREATE TABLE Customer (
    CustomerID SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE NOT NULL
);

-- Create CustomerPhone table (multi-valued attribute)
CREATE TABLE CustomerPhone (
    CustomerID INT NOT NULL,
    PhoneNumber VARCHAR(20) NOT NULL,
    PRIMARY KEY (CustomerID, PhoneNumber),
    FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)
);

-- Create Product table
CREATE TABLE Product (
    ProductID SERIAL PRIMARY KEY,
    ProductName VARCHAR(100) NOT NULL,
    Price DECIMAL(10,2) NOT NULL,
    StockQuantity INT NOT NULL
);

-- Create Order table
CREATE TABLE Orders (
    OrderID SERIAL PRIMARY KEY,
    OrderDate DATE NOT NULL,
    CustomerID INT NOT NULL,
    FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)
);

-- Create Payment table (1:1 with Orders)
CREATE TABLE Payment (
    PaymentID SERIAL PRIMARY KEY,
    PaymentDate DATE NOT NULL,
    Amount DECIMAL(10,2) NOT NULL,
    OrderID INT UNIQUE NOT NULL,
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID)
);

-- Create OrderProduct table (M:N relationship)
CREATE TABLE OrderProduct (
    OrderID INT NOT NULL,
    ProductID INT NOT NULL,
    Quantity INT NOT NULL,
    PRIMARY KEY (OrderID, ProductID),
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
    FOREIGN KEY (ProductID) REFERENCES Product(ProductID)
);
```

---

## Problem 06: University Course Enrollment System  

### Description  
A university needs to store information about students, courses, professors, enrollments, and grades.  
You are asked to design an ERD for the following scenario:  

- Each student has: StudentID, Name, Major, and Email.  
- Each professor has: ProfessorID, Name, Department, and Office.  
- Each course has: CourseID, CourseName, and Credits.  
- A professor can teach many courses, but each course must be taught by exactly one professor.  
- A student can enroll in many courses, and a course can have many students.  
- For each enrollment, the system records the Semester and Year.  
- Each enrollment must also store a Grade (e.g., A, B, C, etc.).  
- A student may have multiple grades for the same course across different semesters.  
- Each enrollment record is uniquely identified by the combination of StudentID, CourseID, and Semester-Year.  

### Requirements  
1. Identify the entities.  
2. Identify the attributes for each entity.  
3. Identify if there are any weak entities.  
4. Define the relationships between entities (detect the cardinality and participation).  
5. Draw the ERD diagram.  
6. Map the ERD to relational schema.  
7. Write SQL code in PostgreSQL to create the schema.  

---

## Solution 06: University Course Enrollment System  

### Entities and Attributes  
- **Student**  
  - StudentID (Primary Key)  
  - Name  
  - Major  
  - Email  

- **Professor**  
  - ProfessorID (Primary Key)  
  - Name  
  - Department  
  - Office  

- **Course**  
  - CourseID (Primary Key)  
  - CourseName  
  - Credits  
  - ProfessorID (Foreign Key)  

- **Enrollment (Weak Entity)**  
  - StudentID (Foreign Key)  
  - CourseID (Foreign Key)  
  - Semester  
  - Year  
  - Grade  
  - (Primary Key = StudentID + CourseID + Semester + Year)  

### Relationships  
- **Professor–Course**: 1:N  
- **Student–Course**: M:N (via Enrollment, with attributes Semester, Year, Grade)  

### ERD Diagram  
![Problem 6 Chen Solution](assets/Problem6ChenSolution.png)  

### ER-to-Relational Mapping  
- **Student(StudentID, Name, Major, Email)**  
- **Professor(ProfessorID, Name, Department, Office)**  
- **Course(CourseID, CourseName, Credits, ProfessorID)**  
- **Enrollment(StudentID, CourseID, Semester, Year, Grade)**  

### PostgreSQL Implementation  
```sql
-- Create Student table
CREATE TABLE Student (
    StudentID SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Major VARCHAR(100),
    Email VARCHAR(100) UNIQUE
);

-- Create Professor table
CREATE TABLE Professor (
    ProfessorID SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Department VARCHAR(100),
    Office VARCHAR(50)
);

-- Create Course table
CREATE TABLE Course (
    CourseID SERIAL PRIMARY KEY,
    CourseName VARCHAR(100) NOT NULL,
    Credits INT NOT NULL,
    ProfessorID INT NOT NULL,
    FOREIGN KEY (ProfessorID) REFERENCES Professor(ProfessorID)
);

-- Create Enrollment table (Weak Entity for M:N relationship with attributes)
CREATE TABLE Enrollment (
    StudentID INT NOT NULL,
    CourseID INT NOT NULL,
    Semester VARCHAR(20) NOT NULL,
    Year INT NOT NULL,
    Grade CHAR(2),
    PRIMARY KEY (StudentID, CourseID, Semester, Year),
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Course(CourseID)
);
```
---

## Problem 07: Library Management System  

### Description  
A library needs to manage data about members, books, authors, staff, borrowings, and fines.  
You are asked to design an ERD for the following scenario:  

- Each member has: MemberID, Name, Email, and Address.  
- Each author has: AuthorID, Name, and Nationality.  
- Each book has: BookID, Title, ISBN, and PublicationYear.  
- A book can be written by one or more authors, and an author can write many books.  
- Each member can borrow many books, and each book can be borrowed by many members over time. For each borrowing, the system stores BorrowDate and ReturnDate.  
- If a member returns a book late, a fine is issued. Each fine has: FineID, Amount, and PaymentStatus. A fine must be linked to exactly one borrowing.  
- The library also employs staff. Each staff member has: StaffID, Name, and HireDate.  
- Staff are divided into two categories (specialization):  
  - **Librarian**: manages borrowing and fines. (Attributes: LibrarianID, Shift)  
  - **Technician**: maintains books. (Attributes: TechnicianID, Expertise)  

### Requirements  
1. Identify the entities.  
2. Identify the attributes for each entity.  
3. Identify any weak entities.  
4. Define the relationships between entities (detect the cardinality and participation).  
5. Represent the specialization/generalization.  
6. Draw the ERD diagram.  
7. Map the ERD to relational schema.  
8. Write SQL code in PostgreSQL to create the schema.  

---

## Solution 07: Library Management System  

### Entities and Attributes  
- **Member**  
  - MemberID (Primary Key)  
  - Name  
  - Email  
  - Address  

- **Author**  
  - AuthorID (Primary Key)  
  - Name  
  - Nationality  

- **Book**  
  - BookID (Primary Key)  
  - Title  
  - ISBN  
  - PublicationYear  

- **Borrowing (Weak Entity)**  
  - BorrowID (Primary Key)  
  - BorrowDate  
  - ReturnDate  
  - MemberID (Foreign Key)  
  - BookID (Foreign Key)  

- **Fine**  
  - FineID (Primary Key)  
  - Amount  
  - PaymentStatus  
  - BorrowID (Foreign Key, Unique)  

- **Staff (Superclass)**  
  - StaffID (Primary Key)  
  - Name  
  - HireDate  

- **Librarian (Subclass of Staff)**  
  - StaffID (Primary Key, Foreign Key)  
  - Shift  

- **Technician (Subclass of Staff)**  
  - StaffID (Primary Key, Foreign Key)  
  - Expertise  

### Relationships  
- **Book–Author**: M:N  
- **Member–Borrowing–Book**: M:N (Borrowing is weak entity with attributes BorrowDate, ReturnDate)  
- **Borrowing–Fine**: 1:1 (each borrowing can have at most one fine)  
- **Staff Specialization**: Staff ISA Librarian, Staff ISA Technician  

### ERD Diagram  
![Problem 7 Chen Solution](assets/Problem7ChenSolution.png)  

### ER-to-Relational Mapping  
- **Member(MemberID, Name, Email, Address)**  
- **Author(AuthorID, Name, Nationality)**  
- **Book(BookID, Title, ISBN, PublicationYear)**  
- **BookAuthor(BookID, AuthorID)**  
- **Borrowing(BorrowID, BorrowDate, ReturnDate, MemberID, BookID)**  
- **Fine(FineID, Amount, PaymentStatus, BorrowID)**  
- **Staff(StaffID, Name, HireDate)**  
- **Librarian(StaffID, Shift)**  
- **Technician(StaffID, Expertise)**  

### PostgreSQL Implementation  
```sql
-- Create Member table
CREATE TABLE Member (
    MemberID SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE NOT NULL,
    Address VARCHAR(200)
);

-- Create Author table
CREATE TABLE Author (
    AuthorID SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Nationality VARCHAR(50)
);

-- Create Book table
CREATE TABLE Book (
    BookID SERIAL PRIMARY KEY,
    Title VARCHAR(200) NOT NULL,
    ISBN VARCHAR(20) UNIQUE,
    PublicationYear INT
);

-- Create BookAuthor (M:N)
CREATE TABLE BookAuthor (
    BookID INT NOT NULL,
    AuthorID INT NOT NULL,
    PRIMARY KEY (BookID, AuthorID),
    FOREIGN KEY (BookID) REFERENCES Book(BookID),
    FOREIGN KEY (AuthorID) REFERENCES Author(AuthorID)
);

-- Create Borrowing table (Weak Entity)
CREATE TABLE Borrowing (
    BorrowID SERIAL PRIMARY KEY,
    BorrowDate DATE NOT NULL,
    ReturnDate DATE,
    MemberID INT NOT NULL,
    BookID INT NOT NULL,
    FOREIGN KEY (MemberID) REFERENCES Member(MemberID),
    FOREIGN KEY (BookID) REFERENCES Book(BookID)
);

-- Create Fine table (1:1 with Borrowing)
CREATE TABLE Fine (
    FineID SERIAL PRIMARY KEY,
    Amount DECIMAL(10,2) NOT NULL,
    PaymentStatus VARCHAR(20) NOT NULL,
    BorrowID INT UNIQUE NOT NULL,
    FOREIGN KEY (BorrowID) REFERENCES Borrowing(BorrowID)
);

-- Create Staff table (Superclass)
CREATE TABLE Staff (
    StaffID SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    HireDate DATE NOT NULL
);

-- Create Librarian table (Subclass of Staff)
CREATE TABLE Librarian (
    StaffID INT PRIMARY KEY,
    Shift VARCHAR(50),
    FOREIGN KEY (StaffID) REFERENCES Staff(StaffID)
);

-- Create Technician table (Subclass of Staff)
CREATE TABLE Technician (
    StaffID INT PRIMARY KEY,
    Expertise VARCHAR(100),
    FOREIGN KEY (StaffID) REFERENCES Staff(StaffID)
);
```
