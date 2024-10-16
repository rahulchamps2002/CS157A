# Intro To Data Base Management Systems Midterm Review

## Topics:
### Database Management System (DBMS)
* benefits of using DMBS:
  * scalability
  * efficiency
  * concurrency
  * crash recovery
  * transactions
* Three-level architecture
  * views
  * logical
  * physical
* conceptual 
  * T/F
  * Multiple choice
  * short answer
### Entity-Relationship Model
* key constraint
* participation constraint
  * total vs partial
* design choices
  * entity vs attribute
  * entity vs relationship
* Extended ER
* Produce ER/EER based on a given scenario

### Relational Model
* definitions
  * relation
  * instance/table
  * schema
  * row/tuple
  * key
  * primary key
  * candidate key
  * superkey
* Integrity Constraints
  * primary key constraints
  * referential integrity (foreign key constraint)
  * domain constraints

### SQL Queries
* conceptual evaluation strategy
  * SELECT
  * FROM
  * WHERE
  * COUNT
  * SUM
  * AVG
  * MAX
  * MIN
  * GROUP BY
* Integrity Constraints
  * table constraints
  * domains constraints
  * foreign key constraints
  * etc
* Table Constraints vs Assertions
  * Views
* Handling NULLs

### Relational Model
* Map the produced ER/EER to set of relations
  * SQL may be required
  * CREATE TABLE statements
  * Table Constraints
  * FOREIGN constraints + Actions
    * NO ACTION
    * CASCADE
    * SET Default
    * Set NULL
    * If SQL Not required
      * Define the schema using the notation discussed and user for HW2
      * Identify PK and FK
### SQL
  * Be able to explain what the query does in simple English
  * Be able to evaluate Syntax
    * SELECT [DISTINCT] target-list
    * FROM relation-list
    * WHERE qualification
    * GROUP BY groupling-list
    * WHERE qualification
    * GROUP BY groupling 
    * HAVING group-qualification
  * Set Manipulation Constructs
    * Union
    * Intersect
    * Except
    * Remember that duplicated are removed
    * Union-Compatible rule
  * Nested Queries
    * IN
    * NOT IN
  * Correlated Nested Quesries
    * EXIST
    * NOT EXIST
  * ANY
  * ALL
  * Aggregated Operators
    * COUNT
    * SUM
    * AVG
    * MIN
    * MAX
  * Views
### Queries to solve/write
  * I may define how to write the query
    * I may say write the query using IN or EXIST or using Set Manipulation constructs, etc. 
### Table Constraints (Single Table Only)
  * Primary Key
  * Unique
  * CHECK
  * etc. 
### Assertion (Multiple Tables)
  * CREATE ASSERTION
  * CHECK
### Relational Algebra
* Selection
* Projections
* Rename
* Joins
  * Inner Join
  * Natural Join
  * Outer Joins
  * etc.
* Set operations
  * Union
  * Intersect
  * Difference
* You may have to write queries using RA format
* Be able to explain what the query does in simple English
* Be able to evaluate

### Evaluation Steps
* cross-Product if more than one table
* Apply Qualification and eliminate rows not matching qualification
* Eliminate unnecessary attributes not in target-list
* Partition the remaining tuples in groups defined by grouping-list
* Apply HAVING condition to eliminate groups

### ORDBMS
* Object Type
  * Defines user-defined types such as object types or collection types
    * VARRAYs
    * Nested Tables

```angular2html
CREATE [OR REPLACE] TYPE type_name AS OBJECT
(attribute 1 datatype, attribute 2 datatype, ... ,
MEMBER FUNCTION function_name
RETURN datatype);
```

* Object Table
  * A special single column table that stores objects of a user-defined object type

```angular2html
CREATE TABLE Employee_Table OF Employee;
```

* Inheritance
  * Use of "UNDER" to define inheritance relationship and use NOT FINAL to permit subtypes
* Nested Table 
  * Stores an unordered set of elements as a single column in a relational table

```angular2html
CREATE TABLE Department (
dept_id NUMBER, dept_name VARCHAR(100),
employees NumList -- Nested table column
) NESTED TABLE employees STORE AS employee_nested_tab;
```
* Department table includes a nested table column employees of type NumList, which can store multiple employee IDs for each department.
* Object Comparison (MAP/ORDER)
  * Understand the difference between MAP/ORDER MEMBER FUNCTIONs and how they evaluate when comparisons involved
  * MAP function maps the object to a scalar value (like a number or string) that can be used for comparison.
  * ORDER function directly compares two objects of the same type and must return an integer for comparison

## Declaring Keys

* PRIMARY KEY or UNIQUE

```angular2html
CREATE TABLE emp
(ssn    int Primary Key,
 name   char(15),
 dno    int
);
```

```angular2html
CREATE TABLE emp
(ssn    int,
 name   char(15),
 dno    int,
 Primary Key (ssn)
);
```

```angular2html
CREATE TABLE emp
(ssn    int,
 name   char(15),
 dno    int,
 Primary Key (dno, name)
);
```


## Foreign-Key Examples

```angular2html
CREATE TABLE emp
(ssn    int,
 name   char(15),
 dno    int REFERENCES dept(dept#)
);
```
**Emp (ename, dno, sal)**

| eName | Dno | Sal |
|-------|-----|-----|
| Jack  | 111 | 50K |
| Alice | 111 | 90K |
| Lisa  | 222 | 80K |
| Tom   | 333 | 70K |
| Mary  | 333 | 60K |

```angular2html
CREATE TABLE emp
(ssn    int,
 name   char(15),
 dno    int,
 FOREIGN KEY dno REFERENCES dept(dept#)
);
```

**Dept (dno, dname, mgr)**

| dno | dname       | mgr   |
|-----|-------------|-------|
| 111 | Sells       | Alice |
| 222 | Toys        | Lisa  |
| 333 | Electronics | Mary  |

* Allow multiple attributes in one foreign-key constraint
* Allow multiple foreign key constraints in one table

## Enforcing Foreign-Key Constraints

**Emp (ename, dno, sal)**

| eName | Dno | Sal |
|-------|-----|-----|
| Jack  | 111 | 50K |
| Alice | 111 | 90K |
| Lisa  | 222 | 80K |
| Tom   | 333 | 70K |
| Mary  | 333 | 60K |

**Dept (dno, dname, mgr)**

| dno | dname       | mgr   |
|-----|-------------|-------|
| 111 | Sells       | Alice |
| 222 | Toys        | Lisa  |
| 333 | Electronics | Mary  |

***Emp.dno references Dptt.dept#***

* modification (delete, update) of Dept whose old "dno" is referenced by a record in Emp. There are 3 strategies:
  * Default: reject
  * Cascade: change the Emp tuple correspondingly
    * Delete in Dept: delete the referring record(s) in Emp
    * Update in Dept: update the dno of the referring record(s) in Emp to the new dno
  * Set Null: change dno value in referring record(s) in Emp to NULL

## Insertion of a Tuple

**INSERT INTO R(A1, ..., An) VALUES (v1, ..., vn)**

* Example:
```angular2html
INSERT INTO Emp (ename, dno, sal)
VALUES ('TOM', 123, 45000)
```
* Can drop attribute names if we provide all of them in order.
```angular2html
INSERT INTO Emp
VALUES ('TOM', 123, 45000)
```

* If we don't provide all attributes, they will be filled with NULL
```angular2html
INSERT INTO Emp (ename, sal)
VALUES ('Tom', 45000)
```

**INSERT INTO relation(subquery);**
```angular2html
CREATE TABLE LowIncomeEmp(ename char(12)
                          dno   int,
                          sal float);

INSERT INTO LowIncomeEmp
( SELECT * 
  FROM emp
  WHERE sal <= 30L AND dno = 123;
);

INSERT INTO LowIncomeEmp
( SELECT ename, dno, sal * 1.1
  FROM emp
  WHERE sal <= 30K and dno = 123;
);
```
* Note the order of querying and inserting: subquery first

