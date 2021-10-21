# SQL_Portfolio

elect *,
round(percent_rank() over (order by price asc) *100, 2) as Percentage_Rank
from product where  brand='Samsung'

-- Query 1:

Write a SQL query to fetch all the duplicate records from a table.

--Tables Structure:


create table users
(
user_id int primary key,
user_name varchar(30) not null,
email varchar(50));

insert into users values
(1, 'Sumit', 'sumit@gmail.com'),
(2, 'Reshma', 'reshma@gmail.com'),
(3, 'Farhana', 'farhana@gmail.com'),
(4, 'Robin', 'robin@gmail.com'),
(5, 'Robin', 'robin@gmail.com');

create table doctors
(
id int primary key,
name varchar(50) not null,
speciality varchar(100),
hospital varchar(50),
city varchar(50),
consultation_fee int
);

insert into doctors values
(1, 'Dr. Shashank', 'Ayurveda', 'Apollo Hospital', 'Bangalore', 2500),
(2, 'Dr. Abdul', 'Homeopathy', 'Fortis Hospital', 'Bangalore', 2000),
(3, 'Dr. Shwetha', 'Homeopathy', 'KMC Hospital', 'Manipal', 1000),
(4, 'Dr. Murphy', 'Dermatology', 'KMC Hospital', 'Manipal', 1500),
(5, 'Dr. Farhana', 'Physician', 'Gleneagles Hospital', 'Bangalore', 1700),
(6, 'Dr. Maryam', 'Physician', 'Gleneagles Hospital', 'Bangalore', 1500);


Select *from users
;
select *
from
(
Select *,
row_number() over (partition by user_name order by user_id desc) as rn
from users
) x
where  x.rn>1
;

select *from users
;

select d1.* 
from doctors d1
 join doctors d2 on d1.id <>d2.id and d1.hospital=d2.hospital 

;
#Table Structure:

drop table login_details;
create table login_details(
login_id int primary key,
user_name varchar(50) not null,
login_date date);


insert into login_details values
(101, 'Michael', current_date),
(102, 'James', current_date),
(103, 'Stewart', DATE_ADD(CURDATE(), INTERVAL 1 DAY)),
(104, 'Stewart', DATE_ADD(CURDATE(), INTERVAL 1 DAY)),
(105, 'Stewart', DATE_ADD(CURDATE(), INTERVAL 1 DAY)),
(106, 'Michael', DATE_ADD(CURDATE(), INTERVAL 2 DAY)),
(107, 'Michael', DATE_ADD(CURDATE(), INTERVAL 2 DAY)),
(108, 'Stewart', DATE_ADD(CURDATE(), INTERVAL 3 DAY)),
(109, 'Stewart', DATE_ADD(CURDATE(), INTERVAL 3 DAY)),
(110, 'James', DATE_ADD(CURDATE(), INTERVAL 4 DAY)),
(111, 'James', DATE_ADD(CURDATE(), INTERVAL 4 DAY)),
(112, 'James', DATE_ADD(CURDATE(), INTERVAL 5 DAY)),
(113, 'James', DATE_ADD(CURDATE(), INTERVAL 6 DAY));

  select   distinct repeated_user 
from
(
select *,
case when user_name=lead(user_name) over(order by login_id)
      and user_name=lead(user_name,2) over( order by login_id) then user_name
      else null
      end as repeated_user
from login_details
) x
where x.repeated_user is not null
;

create table weather
(
id int,
city varchar(50),
temperature int,
day date
);
delete from weather;
insert into weather values
(1, 'London', -1, date('2021-01-01')),
(2, 'London', -2, date('2021-01-02')),
(3, 'London', 4, date('2021-01-03')),
(4, 'London', 1, date('2021-01-04')),
(5, 'London', -2, date('2021-01-05')),
(6, 'London', -5, date('2021-01-06')),
(7, 'London', -7, date('2021-01-07')),
(8, 'London', 5, date('2021-01-08'));


# fetch out the data for which three days the temperature get negative
select *,
 case when temperature <0
     and lead(temperature) over(order by id) < 0
	 and  lead(temperature,2) over (order by id)< 0
     then 'yes' 
    
    when temperature <0
 and lag(temperature) over(order by id) < 0
	and  lead(temperature) over (order by id)< 0
    then 'Yes'
    
    when temperature <0
 and lag(temperature) over(order by id) < 0
	and  lag(temperature,2) over (order by id)< 0
    then 'yes'
    else null
    end Flag
 from weather
;


drop table patient_logs;
create table patient_logs
(
  account_id int,
  date date,
  patient_id int
);

insert into patient_logs values 
(1, date('2020-01-02'), 100),
(1, date('2020-01-27'), 200),
(2, date('2020-01-01'), 300),
(2,date('2020-01-21'), 400),
(2, date('2020-01-21'), 300),
(2, date('2020-01-01'), 500),
(3, date('2020-01-20'), 400),
(1, date('2020-03-04'), 500),
(3, date('2020-01-20'), 450);

select months, account_id, no_ofPatient
from
(
Select *,  
rank() over(partition by months order by no_ofPatient desc,account_id) as Rnk
from
(
Select Months, account_id,count(1) as no_ofPatient
from
(
select  distinct monthname(date) as Months, account_id, patient_id
      from patient_logs
) x
 Group by Months, account_id
)pl
) temp
where temp.Rnk in (1,2)
;


drop table employee;

drop table employee;
create table employee
( emp_ID int primary key
, emp_NAME varchar(50) not null
, DEPT_NAME varchar(50)
, SALARY int);

insert into employee values(101, 'Mohan', 'Admin', 4000);
insert into employee values(102, 'Rajkumar', 'HR', 3000);
insert into employee values(103, 'Akbar', 'IT', 4000);
insert into employee values(104, 'Dorvin', 'Finance', 6500);
insert into employee values(105, 'Rohit', 'HR', 3000);
insert into employee values(106, 'Rajesh',  'Finance', 5000);
insert into employee values(107, 'Preet', 'HR', 7000);
insert into employee values(108, 'Maryam', 'Admin', 4000);
insert into employee values(109, 'Sanjay', 'IT', 6500);
insert into employee values(110, 'Vasudha', 'IT', 7000);
insert into employee values(111, 'Melinda', 'IT', 8000);
insert into employee values(112, 'Komal', 'IT', 10000);
insert into employee values(113, 'Gautham', 'Admin', 2000);
insert into employee values(114, 'Manisha', 'HR', 3000);
insert into employee values(115, 'Chandni', 'IT', 4500);
insert into employee values(116, 'Satya', 'Finance', 6500);
insert into employee values(117, 'Adarsh', 'HR', 3500);
insert into employee values(118, 'Tejaswi', 'Finance', 5500);
insert into employee values(119, 'Cory', 'HR', 8000);
insert into employee values(120, 'Monica', 'Admin', 5000);
insert into employee values(121, 'Rosalin', 'IT', 6000);
insert into employee values(122, 'Ibrahim', 'IT', 8000);
insert into employee values(123, 'Vikram', 'IT', 8000);
insert into employee values(124, 'Dheeraj', 'IT', 11000);

Select x.*
from employee e 
join
(
Select *, 
max(SALARY) over(partition by DEPT_NAME ) as MaximumSalary,
 min(Salary) over(partition by DEPT_NAME) as MinimumSalary
from employee
)x
on
e.emp_ID=x.emp_ID and (e.SALARY=x.MaximumSalary or e.Salary=x.MinimumSalary)
order by x.DEPT_NAME,x.Salary

;
create table students
(
id int primary key,
student_name varchar(50) not null
);
insert into students values
(1, 'James'),
(2, 'Michael'),
(3, 'George'),
(4, 'Stewart'),
(5, 'Robin');

select id, student_name,
case when id%2<>0 then lead(student_name,1,student_name)  over(order by id)
	 when id%2=0 then lag(student_name) over (order by id) 
     end as new_student  
 from students;
 
 
 ;

create table event_category
(
  event_name varchar(50),
  category varchar(100)
);


create table physician_speciality
(
  physician_id int,
  speciality varchar(50)
);


create table patient_treatment
(
  patient_id int,
  event_name varchar(50),
  physician_id int
);


insert into event_category values ('Chemotherapy','Procedure');
insert into event_category values ('Radiation','Procedure');
insert into event_category values ('Immunosuppressants','Prescription');
insert into event_category values ('BTKI','Prescription');
insert into event_category values ('Biopsy','Test');


insert into physician_speciality values (1000,'Radiologist');
insert into physician_speciality values (2000,'Oncologist');
insert into physician_speciality values (3000,'Hermatologist');
insert into physician_speciality values (4000,'Oncologist');
insert into physician_speciality values (5000,'Pathologist');
insert into physician_speciality values (6000,'Oncologist');


insert into patient_treatment values (1,'Radiation', 1000);
insert into patient_treatment values (2,'Chemotherapy', 2000);
insert into patient_treatment values (1,'Biopsy', 1000);
insert into patient_treatment values (3,'Immunosuppressants', 2000);
insert into patient_treatment values (4,'BTKI', 3000);
insert into patient_treatment values (5,'Radiation', 4000);
insert into patient_treatment values (4,'Chemotherapy', 2000);
insert into patient_treatment values (1,'Biopsy', 5000);
insert into patient_treatment values (6,'Chemotherapy', 6000);


select * from patient_treatment;
select * from event_category;
select * from physician_speciality;

select ps.speciality, count(1) as speciality_count
from patient_treatment pt
join event_category ec on ec.event_name = pt.event_name
join physician_speciality ps on ps.physician_id = pt.physician_id
where ec.category = 'Procedure'
and pt.physician_id not in (select pt2.physician_id
							from patient_treatment pt2
							join event_category ec on ec.event_name = pt2.event_name
							where ec.category in ('Prescription'))
group by ps.speciality;



select * from patient_treatment;
select * from event_category;
select * from physician_speciality;



Select   ps.speciality, count(1) as  speciality_Count 
from patient_treatment pt 
join event_category et
on et.event_name=pt.event_name
  join physician_speciality ps
on ps.physician_id=pt.physician_id
where et.category='Procedure'
and pt.physician_id  in
   ( 
   select pt2.physician_id 
   from patient_treatment pt2
	join event_category ec2
    on ec2.event_name=pt2.event_name
    where ec2.event_name='Prescription'
    )
group by ps.speciality
