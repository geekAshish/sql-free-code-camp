# JOIN

# Nested Queries

1. Find names of all employees who have sold over 30,000 to a single client
   select employee.first_name, employee.last_name

`from employee
where employee.emp_id in (
  select works_with.emp_id
  from works_with
  where works_with.total_sales > 30000
);`

2. Find all clients who are handled by the branch that Michael Scott manages, Assume you know Michael's ID

`select client.client_name
from client
where client.branch_id in (
	select branch.branch_id
    from branch
    where mgr_id=102
);`

- let say if we're using = instead of "in", so in that case we have to add limit to we only get one value not multiple because = only works with one value, if we want to go with multiple value we should go with "in"

`select client.client_name
from client
where client.branch_id = (
	select branch.branch_id
    from branch
    where mgr_id=102
    limit=1
);`

# On Delete

1. while create entry : ON DELETE SET NULL

and if you delete it
DELETE FROM employee WHERE emp_id = 102;
mgr_id will be NULL in branch;

2. while create entry : ON DELETE CASCADE

DELETE FROM branch WHERE branch_id = 2;
SELECT \* FROM branch_supplier;

NOTE (good to know) : In a table, if the foreign key is the primary key use ON DELETE CASCADE, and if the foreign key is just a foreign key then ok to go with ON DELETE SET NULL

# Triggers

go to command line

you can create trigger for, INSERT, UPDATE, DELETE
BEFORE, AFTER

`DELIMITER $$
CREATE
  TRIGGER my_trigger BEFORE INSERT
  ON employee
  FOR EACH ROW BEGIN
    INSERT INTO trigger_test VALUES('added new employee');
  END$$
DELIMITER ;`

`DELIMITER $$
CREATE
  TRIGGER my_trigger BEFORE INSERT
  ON employee
  FOR EACH ROW BEGIN
    INSERT INTO trigger_test VALUES(NEW.first_name); // get access to column value with NEW keyword
  END$$
DELIMITER ;`

- DROP TRIGGER trigger_name;

# ER Diagrams Intro

1. Entity Relationship diagram
2. Entity - An object we want to model & store information about
3. Attributes - Specific pieces of information about an entity
4. Primary Key - An attribute(s) that uniquely identify an entry in the database table
5. Composite Attribute - An attribute that can be broken up into sub-attributes
6. Multi-valued Attribute - An attribute that can have more than one value : two circles
7. Derived Attribute - An attribute that can be derived from the other attributes : dotted line circle
8. Multiple Entities - You can define more than one entity in the diagram
9. Relationships - defines a relationship between two entities
10. Total Participation - All members must participate in the relationship : double line
11. Partial participation - single line
12. Relationship Attribute - An attribute about the relationship
13. Relationship Cardinality - the number of instances of an entity from a relation that can be associated with the relation
14. Weak Entity - An entity that cannot be uniquely identified by its attributes alone
15. Identifying relationship - A relationship that serves to uniquely identify the weak entity

# Converting ER Diagrams to Schemas

4:08:34
