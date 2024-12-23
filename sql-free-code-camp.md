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
