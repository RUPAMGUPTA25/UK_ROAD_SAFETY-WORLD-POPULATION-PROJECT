WORLD-POPULATION-PROJECT - SQL ANALYSIS PROJECT

use "RGANALYTICS"

CREATE or replace TABLE pop_data (
	country VARCHAR(45) NOT NULL, 
	area DECIMAL(38, 0) NOT NULL, 
	birth_rate DECIMAL(38, 2) NOT NULL, 
	death_rate DECIMAL(38, 2) NOT NULL, 
	infant_mortality_rate DECIMAL(38, 2) NOT NULL, 
	internet_users DECIMAL(38, 0) NOT NULL, 
	life_exp_at_birth DECIMAL(38, 2) NOT NULL, 
	mater178l_mortality_rate DECIMAL(38, 0) NOT NULL, 
	net_migration_rate DECIMAL(38, 3) NOT NULL, 
	population DECIMAL(38, 0) NOT NULL, 
	population_growth_rate DECIMAL(38, 2) NOT NULL
);

select * from pop_data;

------#1. Which country has the highest population?---------------------------------------
select country from pop_data where population= (select max(population) from pop_data);

select country, population from pop_data where population= (select max(population) from pop_data);
---China = 1355692576

------#2. Which country has the least number of people?-----------------------------------
select country from pop_data where population= (select min(population) from pop_data);

select country, population from pop_data where population= (select min(population) from pop_data);
----Pitcairn Islands = 48


------#3. Which country is witnessing the highest population growth?-----------------------------------------


--------------------------------------------|--#UK ROAD SAFETY QnA--|-------------------------------------------------------------------------------------

use "INERUON_TASKS";

---------------------------------------------------------TASK 1---------------------------------------------------------------------------------------
create or replace table shopping_history(
product varchar(30) not null,
quantity integer not null,
unit_price integer not null);

insert into shopping_history values 
('Cookies', 10, 5),
('Cookies', 20, 7),
('Milk', 3, 10),
('Milk', 10, 15),
('Eggs', 30, 6);

select * from shopping_history;

select product, sum(quantity * unit_price) as Total_Price from shopping_history group by 1 order by product desc;

---------------TASK 2---------------------------------
-----TABLE-1-----
create or replace table phones(
name varchar(20) not null unique,
phone_number int not null unique);

insert into phones values('jack',1234),('lenna',3333),('mark',9999),('anna',7582);

SELECT * FROM phones;

-----TABLE-2-----
create or replace table calls(
id int not null unique,
caller int not null,
callee int not null,
duration int not null);

insert into calls values(25,1234,7582,8),(7,9999,7582,1),(18,9999,3333,4),(2,7582,3333,3),(3,3333,1234,1),(21,3333,1234,1);

select * from calls;


----------------TASK 2.A--------------------------------;

select b.name,TOT_DUR from
(select caller,sum(duration) as TOT_DUR from
(select caller,duration from calls
union all
select callee,duration from calls)
group by 1) a
left outer join phones b
on a.caller = b.phone_number
where tot_dur>=10 order by name;

-------------------TASK 2.B--------------------------------;

------TABLE 1---->

create or replace table phones1(
name varchar(20) not null unique,
phone_number int not null unique);

insert into phones1 values('john',6356),('Addisson',4315),('kate',8803),('ginny',9831);

SELECT * FROM phones1;

------TABLE 2----->

create or replace table calls1(
id int not null unique,
caller int not null,
callee int not null,
duration int not null)

insert into calls1 values(65,8803,9831,7),(100,9831,8803,3),(105,4315,9831,18);

select * from calls1; 

------problem_solution------;

select p.name, c.T_duration from 
(select caller,sum(duration) as T_duration from
(select caller, duration from calls1
union all
select callee, duration from calls1)
group by 1) c
left outer join phones1 p on 
p.phone_number= c.caller
where T_duration>=10 order by name;

-------TASK 3--------------------------------------------------
----------------------TASK 3.A---------------------------
  
create table transactions(
Amount int not null,
Date Date not null);

insert into transactions values(-10,'2020-01-14'),(-75,'2020-01-20'),(-5,'2020-01-25'),(-4,'2020-01-29'),
(2000,'2020-03-10'),(-75,'2020-03-10'),(-20,'2020-03-15'),(40,'2020-03-15'),(-50,'2020-03-17'),
(200,'2020-10-10'),(-200,'2020-10-10');

insert into transactions values(1000,'2020-01-06');

select * from transactions;

#-----------soultion------------>

WITH trans_detail AS

(SELECT SUM(amount) AS total_amount, extract(month FROM Date) AS months,
CASE WHEN amount<0 THEN 'withdrawal' 
     ELSE 'credited' end as statements,
COUNT(statements) AS Count_use FROM transactions GROUP BY 2,3)

SELECT SUM(total_amount)-(12-(SELECT COUNT(*) FROM trans_detail WHERE total_amount<= -100 AND Count_use>=3))*5 AS balance
FROM trans_detail;



----------------------------TASK 3.B----------------

create table transactions2(
Amount int not null,
Date Date not null);

insert into transactions2 values(1,'2020-06-29'),(35,'2020-02-20'),(-50,'2020-02-03'),(-1,'2020-02-26'),(-200,'2020-08-01'),(-44,'2020-02-07'),
(-5,'2020-02-25'),(1,'2020-06-29'),(1,'2020-06-29'),(-100,'2020-12-29'),(-100,'2020-12-30'),(-100,'2020-12-31');

SELECT * FROM transactions2;

WITH trans_detail AS

(SELECT SUM(amount) AS total_amount, extract(month FROM Date) AS months,
CASE WHEN amount<0 THEN 'withdrawal' 
     ELSE 'credited' end as statements,
COUNT(statements) AS Count_use FROM transactions2 GROUP BY 2,3)

SELECT SUM(total_amount)-(12-(SELECT COUNT(*) FROM trans_detail WHERE total_amount<= -100 AND Count_use>=3))*5 AS balance
FROM trans_detail;


-------------------------TASK 3.C------------------------------------

create table transactions3(
Amount int not null,
Date Date not null);

insert into transactions3 values(6000,'2020-04-03'),(5000,'2020-04-02'),(4000,'2020-04-01'),(3000,'2020-03-01'),(2000,'2020-02-01'),(1000,'2020-01-01');

SELECT * FROM transactions3; 

#----------*solution*---------}>>

WITH trans_detail AS

(SELECT SUM(amount) AS total_amount, extract(month FROM Date) AS months,
CASE WHEN amount<0 THEN 'withdrawal' 
     ELSE 'credited' end as statements,
COUNT(statements) AS Count_use FROM transactions3 GROUP BY 2,3)

SELECT SUM(total_amount)-(12-(SELECT COUNT(*) FROM trans_detail WHERE total_amount<= -100 AND Count_use>=3))*5 AS balance
FROM trans_detail; 


select country from pop_data where population_growth_rate= (select max(population_growth_rate) from pop_data);

select country, population_growth_rate from pop_data where population_growth_rate= (select max(population_growth_rate) from pop_data);
--Lebanon = 9.37%
 
------#4. Which country has an extraordinary number for the population?

select country, population from pop_data where population= (select max(population) from pop_data);
---China = 1355692576

------#5. Which is the most densely populated country in the world?

select country, round(population/area,2) densely_pop from pop_data where area > 0 order by densely_pop desc limit 1;
---Kingman Reef = 32294361.00
