# SQL
SQL practice

drop database if exists `interview_test_db`;

create database if not exists `interview_test_db`;
use `interview_test_db`;

drop table if exists `employee_details`;
create table if not exists `employee_details`
(
	empId int not null primary key auto_increment,
    empFullName varchar(50) not null,
    empManagerId int,
    empJoiningDate date
);

insert into `employee_details`(`empFullName`,`empManagerId`,`empJoiningDate`)
values('Ashish Tiwari',1001,'1991-01-11'),
('Vivek Mahamiya',1001,'1992-12-23'),
('Vinod Badwal',1001,'1992-11-23'),
('Lalit Chowdhary',1002,'1992-09-27'),
('Urvish Bhatti',1001,'1992-10-07'),
('Satyapal Chouhan',1002,'1990-09-21');

drop table if exists `employee_project_details`;
create table if not exists `employee_project_details`
(
	empProjId int not null primary key auto_increment,
	empId int not null,
    empProjectId char(2) not null,
    empSalary float not null,
    constraint `FK_EMPDTLS` foreign key (empId)
    references interview_test_db.employee_details(`empId`)
);

insert into `employee_project_details`(`empId`,`empProjectId`,`empSalary`)
values(1,'P1',100000),
(2,'P1',58000),
(3,'P1',65000),
(4,'P2',100000),
(5,'P1',50000),
(6,'P2',60000);

select count(*) as `Employee In Project 1`
From `employee_project_details` as `epd`
where `epd`.`empProjectId` = 'P1';

/* By Using Joins */
SELECT empd.empFullName as `Employee Name`
FROM `employee_details` as `empd`
INNER JOIN `employee_project_details` as `epd`
ON empd.empId=epd.empId
WHERE epd.empSalary BETWEEN 60000 AND 100000
ORDER BY 1 ASC;

/* By Using Sub-Queries */
SELECT empd.empFullName as `Employee Name`
FROM `employee_details` as `empd`
WHERE empd.empId IN (
	SELECT epd.empId
    FROM `employee_project_details` as `epd`
    WHERE epd.empSalary between 60000 AND 100000
)
ORDER BY 1 ASC;

SELECT empProjectId as `Project ID`, count(*) as `Project Count`
FROM `employee_project_details`
GROUP BY empProjectId
ORDER BY `Project Count` DESC;

SELECT distinct(empSalary) as `DISTINCT Salary`
FROM employee_project_details
ORDER BY 1 DESC;

SELECT distinct(empSalary) as `second highest`
FROM employee_project_details
WHERE empSalary NOT IN (
	SELECT MAX(empSalary)
    FROM employee_project_details
)
ORDER BY 1 DESC
LIMIT 1;


SELECT distinct(empSalary) as `second last lowest`
FROM employee_project_details
WHERE empSalary NOT IN (
	SELECT MIN(empSalary)
    FROM employee_project_details
)
ORDER BY 1 ASC
LIMIT 1;

select distinct(empSalary) as `Salary`,count(empSalary) as `Duplicate Salary`
from employee_project_details
group by empSalary
having count > 1;
