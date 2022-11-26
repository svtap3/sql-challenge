# sql-challenge

Tables
drop table "dept_emp"
CREATE TABLE "dept_emp" (
    "emp_no" integer   NOT NULL,
    "dept_no" varchar   NOT NULL
);

drop table "dept_manager"
CREATE TABLE "dept_manager" (
    "dept_no" varchar   NOT NULL,
    "emp_no" integer   NOT NULL,
    CONSTRAINT "pk_dept_manager" PRIMARY KEY (
        "emp_no"
     )
);

CREATE TABLE "departments" (
    "dept_no" varchar   NOT NULL,
    "dept_name" varchar(50)   NOT NULL,
    CONSTRAINT "pk_departments" PRIMARY KEY (
        "dept_no"
     )
);

CREATE TABLE "employees" (
    "emp_no" int   NOT NULL,
    "emp_title_id" varchar   NOT NULL,
    "birth_date" date   NOT NULL,
    "first_name" varchar(50)   NOT NULL,
    "last_name" varchar(50)   NOT NULL,
    "sex" varchar(50)   NOT NULL,
    "hire_date" date   NOT NULL,
    CONSTRAINT "pk_employees" PRIMARY KEY (
        "emp_no"
     )
);

CREATE TABLE "salaries" (
    "emp_no" int   NOT NULL,
    "salary" integer   NOT NULL,
    CONSTRAINT "pk_salaries" PRIMARY KEY (
        "emp_no"
     )
);

CREATE TABLE "titles" (
    "title_id" varchar   NOT NULL,
    "title" varchar(50)   NOT NULL,
    CONSTRAINT "pk_titles" PRIMARY KEY (
        "title_id"
     )
);

select * from dept_emp;

select emp_no, emp_title_id
from employees
where emp_no = 10010;


Data Analysis
--Query listing the employee number, last name, first name, sex, and salary of each employee.
select employees.emp_no, employees.last_name, employees.first_name, employees.sex, salaries.salary
from employees join salaries on salaries.emp_no = employees.emp_no
order by emp_no;

--Query listing the first name, last name, and hire date for the employees who were hired in 1986.
select first_name, last_name, hire_date
from employees 
where hire_date >= '1986-01-01' and hire_date <= '1986-12-31'
order by hire_date;

--Query listing the manager of each department along with their department number, department name, employee number, 
--last name, and first name.
select employees.emp_no, employees.last_name, employees.first_name, dept_manager.dept_no, departments.dept_name
from employees 
join dept_manager on dept_manager.emp_no = employees.emp_no
join departments on departments.dept_no = dept_manager.dept_no
order by emp_no;

-- Query listing the department number for each employee along with that employee’s employee number, 
-- last name, first name, and department name.
select employees.emp_no, employees.last_name, employees.first_name, dept_emp.dept_no, departments.dept_name
from employees 
join dept_emp on dept_emp.emp_no = employees.emp_no
join departments on departments.dept_no = dept_emp.dept_no
order by emp_no;

-- Query listing first name, last name, and sex of each employee whose first name is Hercules and whose
-- last name begins with the letter B.
select first_name, last_name, sex
from employees
where first_name = 'Hercules' AND last_name similar to '(B%)'
order by last_name;

-- Query listing each employee in the Sales department, including their employee number, last name, and first name.
select employees.emp_no, employees.last_name, employees.first_name, departments.dept_name
from employees
join dept_emp on dept_emp.emp_no = employees.emp_no
join departments on departments.dept_no = dept_emp.dept_no
where dept_name = 'Sales'
order by emp_no;

-- Query listing each employee in the Sales and Development departments, including their employee number, last name, first name, and department name.

select employees.emp_no, employees.last_name, employees.first_name, departments.dept_name
from employees
join dept_emp on dept_emp.emp_no = employees.emp_no
join departments on departments.dept_no = dept_emp.dept_no
where dept_name = 'Sales' or  dept_name = 'Development'
order by emp_no;

-- Query listing the frequency counts, in descending order, of all the employee last names (that is, how many employees
-- share each last name).
select last_name, count(*)
from employees
group by last_name
order by last_name;
