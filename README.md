# Database Design Practice

Welcome to **Database Design Practice**!  
This repository is built to help you learn database design by practice.

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

## What’s Inside?
- Real-world inspired scenarios  
- Practice problems with clear solutions  
- A gradual learning curve to make ERD design easy and intuitive  

---

### Problems

---
# Problem 01: Company Employees and Departments

## Description
A company wants to store information about its employees and departments.  
You are asked to design an Entity Relationship Diagram (ERD) for the following scenario:

- Each employee has: SSN, First Name, Last Name, Birth Date, and Gender.  
- Each department has: Department Number (unique), Department Name, and Location(s).  
- Each employee works in exactly one department.  
- A department can have multiple employees.  
- Each department has one manager.  

## Requirements
1. Identify the entities.  
2. Identify the attributes for each entity.  
3. Define the relationships between entities.  
4. Represent the ERD diagram.  

## Diagram Placeholder
![Problem 01 ERD](../images/problem01.png)

# Solution 01: Company Employees and Departments

## Entities and Attributes
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

## Relationships
- **Works_In**: Each employee works in one department (1:N relationship).  
- **Manages**: Each department has one manager (1:1 relationship between Department and Employee).  

## Final ERD
![Solution 01 ERD](../images/solution01.png)
