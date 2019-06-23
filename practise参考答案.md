# 一、employees数据库50题参考答案
## (一)、插入数据到员工表，你的雇佣时间是你18岁时。
```sql
insert into employees(emp_no,birth_date,first_name,last_name,gender,hire_date)
values(10000,'1990-09-09','jack','wang','M','2008-09-09');
```
## (二)、插入数据到部门表，你在入职前1年在开发部，现在在产品部
```sql
insert into dept_emp(emp_no,dept_no,from_date,to_date)
values(10000,'d005','2008-09-09','2009-09-09');
insert into dept_emp(emp_no,dept_no,from_date,to_date)
values(10000,'d004','2009-09-09','9999-01-01');
```
## (三)、插入数据部门经理表，你在进入产品部半年后被任命为产品部部门经理（注意之前的部门经理应该下岗）
```sql
update dept_manager
set to_date='2010-03-09'
where dept_no='d004'
and to_date='9999-01-01';
insert into dept_manager(emp_no,dept_no,from_date,to_date)
values(10000,'d004','2010-03-09','9999-01-01');
```
## (四)、插入数据到薪水表，在研发部时你的薪水是40000，作为部门经理你的薪水涨了30000。
```sql
insert into salaries(emp_no,salary,from_date,to_date)
values(10000,40000,'2008-09-09','2010-03-09');
insert into salaries(emp_no,salary,from_date,to_date)
values(10000,70000,'2010-03-09','9999-01-01');
```
## (五)、插入数据到职称表，你刚入职为工程师，你在入职半年后升为高级工程师，然后现在，你升为技术负责人
```sql
insert into titles(emp_no,title,from_date,to_date)
values(10000,'Engineer','2008-09-09','2009-03-09');
insert into titles(emp_no,title,from_date,to_date)
values(10000,'Senior Engineer','2009-03-09',CURRENT_DATE());
insert into titles(emp_no,title,from_date,to_date)
values(10000,'Technique Leader',CURRENT_DATE(),'9999-01-01');
```
## (六)、你的薪资数据记录错误，你的薪资应该比现在再多10000，请编写sql。
```sql
update salaries
set salary=salary+10000
where emp_no=10000
and to_date='9999-01-01'
```
## (七)、请查询出您的员工信息（编号，姓名，性别，生日，入职时间，工龄，年龄）
```sql
select
emp_no as '编号',
CONCAT(last_name,' ',first_name) as '姓名',
gender as '性别',
birth_date as '生日',
hire_date as '入职时间',
round(datediff(CURRENT_DATE(),hire_date)/365*100)/100 as '工龄(年)',
FLOOR(datediff(CURRENT_DATE(),birth_date)/365) as '年龄'
from employees
where emp_no=10000;
```
## (八)、请查询出年龄比你大的员工信息
```sql
select *
from employees
where birth_date<'1990-09-09';
```
## (九)、请查询出和你同姓的员工信息
```sql
select *
from employees
where last_name='wang';
```
## (一〇)、请查询出名字为三个字的员工信息
```sql
select *
from employees
where first_name like '___';
select *
from employees
where length(first_name)=3;
```
## (一一)、请查询出名字中包含字母A的所有员工信息
```sql
select *
from employees
where first_name like binary '%A%';
```
## (一二)、请查询出和你同姓同性的员工信息
```sql
select *
from employees
where last_name ='wang' and gender='M';
```
## (一三)、请查询出入职时间和你同月的员工信息
```sql
select *
from employees
where MONTH(hire_date)=9
```
## (一四)、请查询出入职时间和你同月，性别和你相同且年龄相差不到5岁的员工
```sql
select *
from employees
where MONTH(hire_date)=9
and gender='M'
and  abs(datediff(birth_date,'1990-09-09'))<5*365
```
## (一五)、请查询出公司年龄最大的5位男员工（升序）
```sql
select *
from employees
where gender='M'
order by birth_date asc
LIMIT 0,5
```
## (一六)、请查询出公司员工平均年龄。
```sql
select avg(datediff(CURRENT_DATE(),birth_date))/365
from employees;
```
## (一七)、请查询出你入职以来待过的所有部门的信息（编号，名称，进入时间）
```sql
SELECT d1.dept_no '编号',d2.dept_name '名称',d1.from_date '进入时间'
FROM dept_emp d1 INNER JOIN departments d2
ON d1.dept_no = d2.dept_no
WHERE d1.emp_no = 10000;
```
## (一八)、请查询出你入职以来所有职称的信息（名称，获得时间）
```sql
SELECT title '名称',from_date '获得时间'
FROM titles
WHERE emp_no=10000;
```
## (一九)、请查询出你入职以来所有薪水的信息（开始时间，薪水）
```sql
SELECT from_date '开始时间',salary '薪水'
FROM salaries
WHERE emp_no=10000;
```
## (二〇)、请查询出公司工龄最大和最小的员工
```sql
select *
from employees
where hire_date = (
select max(hire_date)
from employees
)
or hire_date=(
select min(hire_date)
from employees
)
```
## (二一)、请查询出公司年龄最大和最小的员工
```sql
select *
from employees
where hire_date=(
select max(hire_date) from employees
union
select *
from employees
where hire_date=(
select min(hire_date) from employees
```
## (二二)、请查询出公司员工男女性别比例。
```sql
select gender,count(*)
from employees
group by gender
```
```sql
select (
(select count(*)
from employees
where gender='F')
(select count(*)
from employees
where gender='M')
) 比例;
```
## (二三)、请查询出公司年龄最小的女员工。
```sql
select *
from employees
where gender='F'
and birth_date=(SELECT max(birth_date)
from employees
where gender='F'
```
## (二四)、请查询公司工龄最小的10位女员工（降序）
```sql
select *
from employees
where gender='F'
order by  hire_date desc
LIMIT 0,10
```
## (二五)、请查询出你当前所属部门的信息（编号，名称，进入时间）
```sql
SELECT d2.dept_no,d2.dept_name,d1.from_date
FROM dept_emp d1 INNER JOIN departments d2
ON d1.dept_no=d2.dept_no
WHERE d1.to_date>CURRENT_DATE() AND d1.emp_no=10000
```
## (二六)、请查询出你当前职务的信息（名称，获得时间）
```sql
SELECT title,from_date
FROM titles
WHERE emp_no=10000 AND to_date="9999-01-01"
```
## (二七)、请查询出你当前薪水的信息（薪水）
```sql
SELECT salary
FROM salaries
WHERE emp_no=777777 AND to_date="9999-01-01"
```
## (二八)、查询出所有职称为工程师的员工（编号，姓名，性别，工龄）-按工龄降序
```sql
SELECT
h1.emp_no,
concat(h1.first_name,'',h1.last_name) AS '姓名',
h1.gender,
FLOOR(DATEDIFF(CURDATE(),h1.hire_date)/365) as workage
FROM employees h1
INNER JOIN titles h2
on h1.emp_no=h2.emp_no
WHERE h2.title="Engineer"
ORDER BY workage desc
```
## (二九)、请查询出公司薪水最高的3位员工信息（编号，姓名，性别，当前薪水）-按薪水降序
```sql
SELECT
h1.emp_no,
concat(h1.first_name,'',h1.last_name) AS '姓名',
h1.gender,
h2.salary
FROM	employees h1
INNER JOIN salaries h2 ON h1.emp_no=h2.emp_no
WHERE to_date="9999-01-01"
ORDER BY h2.salary desc
LIMIT 0,3
```
## (三〇)、请查询出公司工龄最高的3位员工及其部门信息（编号，姓名，性别，工龄）-按工龄降序
```sql
SELECT t1.emp_no,CONCAT(t2.last_name,t2.first_name),t2.gender,
year(CURRENT_DATE())-year(t2.hire_date) as 'workage'
FROM dept_emp t1 INNER JOIN employees t2
ON t1.emp_no=t2.emp_no
ORDER BY workage DESC
LIMIT 0,3
```
## (三一)、请查询出公司工龄最高的3位员工及其职称信息（编号，姓名，性别，职称）-按工龄降序
```sql
SELECT t2.emp_no,
CONCAT(t2.last_name,t2.first_name) as 'name',
t2.gender,
t1.title
FROM titles t1 INNER JOIN employees t2
ON t1.emp_no=t2.emp_no
ORDER BY year(CURRENT_DATE())-year(t2.hire_date) DESC
LIMIT 0,3
```
## (三二)、统计公司各个职称员工总数（职称，员工数量）-按员工数量降序
```sql
select title,count(*)
from titles
where to_date='9999-01-01'
group by title
order by 2 desc;
```
## (三三)、统计各个部门员工总数（部门编号，部门名称，员工总数）-按员工总数降序
```sql
select
de.dept_no 部门编号,
(select dept_name from departments where dept_no=de.dept_no)部门名称,
count(*) 员工总数
from dept_emp de
where to_date='9999-01-01'
group by de.dept_no
ORDER BY 3 desc;
```
## (三四)、统计出所有职称拥有员工数量（职称，员工数量）-按第2列降序
```sql
select  title,count(*)
from titles
where to_date='9999-01-01'
GROUP BY title
ORDER BY 2;
```
## (三五)、统计出公司员工调动部门次数（员工编号，员工姓名，调动次数）-按调动次数降序
```sql
select
de.emp_no 员工编号,
(select CONCAT(first_name,' ',last_name) from employees where emp_no=de.emp_no) 员工姓名,
count(*) 调动次数
from dept_emp de
GROUP BY emp_no
order by 1 asc;
```
## (三六)、统计出各个部门拥有高级职员(Senior Staff)的情况（部门编号，部门名称，高级职员人数）-按部门编号降序
```sql
select d.dept_no 部门编号,
(select dept_name from departments where dept_no=d.dept_no) 部门名称,
count(*) 高级职员人数
from (
select dept_no,emp_no
from dept_emp
where to_date='9999-01-01'
) d
join (
select  emp_no
from titles
where to_date='9999-01-01'
and title='Senior Staff'
) t on t.emp_no=d.emp_no
group by d.dept_no
order by 1
```
## (三七)、统计出公司工龄第二大的所有员工- 按工龄降序
```sql
select *
from employees
where
hire_date=(
SELECT min(hire_date)
from employees
where
hire_date>(select min(hire_date) from employees)
order by hire_date desc;
```
## (三八)、统计出所有没有女员工的部门（部门编号，部门名称）-按部门编号降序
```sql
select d.dept_no,d.dept_name
from departments d
where
exists (
select *
from (
select dept_no,emp_no
from dept_emp
where to_date='9999-01-01'
) de
join
employees e on e.emp_no=de.emp_no and e.gender='M'
where de.dept_no=d.dept_no
```
## (三九)、查询出你部门中入职时间最早的员工信息（编号，名称，性别，工龄）-按入职时间升序
```sql
select *
from employees e
join dept_emp de on e.emp_no=de.emp_no
where e.hire_date =(
select min(hire_date)
from employees e
join dept_emp de on e.emp_no=de.emp_no
where de.dept_no='d004'
and de.dept_no='d004'
order by e.hire_date;
```
## (四〇)、查询出你部门中薪水比你高的员工信息（编号，姓名，性别，当前薪水）-按薪水降序
```sql
select
de.emp_no 编号,
(select CONCAT(first_name,' ',last_name) from employees where emp_no=de.emp_no) 姓名,
(select gender from employees where emp_no=de.emp_no) 性别,
s.salary
from (
select emp_no,dept_no
from dept_emp where dept_no='d004' and to_date='9999-01-01'
) de
join
select emp_no,salary from salaries where to_date='9999-01-01'
) s
on de.emp_no=s.emp_no
where s.salary>80000
order by 4;
```
## (四一)、统计所有部门中的最高薪水（部门编号，部门名称，最高薪水）,按薪水降序
```sql
select
de.dept_no 部门编号,
(select dept_name from departments where dept_no=de.dept_no) 部门名称,
max(salary) 最高薪水
from (
select emp_no,dept_no
from dept_emp where to_date='9999-01-01'
) de
join(
select emp_no,salary from salaries where to_date='9999-01-01'
) s
on de.emp_no=s.emp_no
group by de.dept_no
```
## (四二)、统计各个部门员工男女总数（部门编号，部门名称，男员工总数，女员工总数）,先按男员工总数降序，然后女员工降序
```sql
select
m.dept_no 部门编号,
(select dept_name from departments where dept_no=m.dept_no) 部门名称,
m.mcount 男员工总数,
f.fcount 女员工总数
from (
select
de.dept_no,
count(*) mcount
from (
select emp_no,dept_no
from dept_emp
where to_date='9999-01-01'
) de
join employees e on de.emp_no=e.emp_no and e.gender='M'
group by de.dept_no
) m
join (
select
de.dept_no,
count(*) fcount
from	(
select emp_no,dept_no
from dept_emp
where to_date='9999-01-01'
)  de join employees e on de.emp_no=e.emp_no and e.gender='F'
group by de.dept_no
) f
on m.dept_no=f.dept_no
order by 3,4
```
## (四三)、统计出女员工比男员工数量多的所有部门（部门编号，部门名称，男员工总数，女员工总数）-按部门编号降序
```sql
select
m.dept_no 部门编号,
(select dept_name from departments where dept_no=m.dept_no) 部门名称,
m.mcount 男员工总数,
f.fcount 女员工总数
from (
select
de.dept_no,
count(*) mcount
from (
select emp_no,dept_no
from dept_emp
where to_date='9999-01-01'
) de
join employees e on de.emp_no=e.emp_no and e.gender='M'
group by de.dept_no
) m
join (
select
de.dept_no,
count(*) fcount
from	(
select emp_no,dept_no
from dept_emp
where to_date='9999-01-01'
)  de join employees e on de.emp_no=e.emp_no and e.gender='F'
group by de.dept_no
) f
on m.dept_no=f.dept_no
where m.mcount>f.fcount
order by m.dept_no
```
## (四四)、统计出拥有员工数量第二多的职称（职称，员工数量）
```sql
select
a.title,
count(*)
from (
select  title,count(*) amount
from titles
where to_date='9999-01-01'
GROUP BY title
) a
join (
select  title,count(*) amount
from titles
where to_date='9999-01-01'
GROUP BY title
) b
where  a.amount<b.amount
group  by a.title
having count(*)=1
```
## (四五)、统计出拥有员工数量第二多的部门（部门编号，部门名称，员工数量）
```sql
select
a.dept_no 部门编号,
(select dept_name from departments where dept_no=a.dept_no) 部门名称,
count(*) 员工数量
from
select dept_no,count(*) amount
from dept_emp
where to_date='9999-01-01'
GROUP BY dept_no
) a,
select dept_no,count(*) amount
from dept_emp
where to_date='9999-01-01'
GROUP BY dept_no
) b
where a.amount<b.amount
group by a.dept_no
HAVING count(*)=1;
```
## (四六)、统计出部门平均薪水处于公司平均薪水之上的所有部门信息。（部门编号，部门名称，部门平均薪水）
```sql
select
de.dept_no 部门编号,
(select dept_name from departments where dept_no=de.dept_no) 部门名称,
avg(s.salary) 部门平均薪水
from (
SELECT dept_no,emp_no
from dept_emp
where to_date='9999-01-01'
) de
join (
select emp_no,salary
from salaries
where to_date='9999-01-01'
) s
on s.emp_no=de.emp_no
group by de.dept_no
HAVING avg(s.salary)>(
select avg(salary)
from salaries
where to_date='9999-01-01'
order by 3 desc;
```
## (四七)、统计出公司涨薪次数最多的员工（员工编号，名称，所属部门，涨薪次数）
```sql
select
x.emp_no 员工编号,
(select CONCAT(first_name,' ',last_name ) from employees where emp_no=x.emp_no) 名称,
(select emp_no from dept_emp where emp_no=x.emp_no and to_date='9999-01-01') 所属部门,
x.amount 涨薪次数
from (
select s.emp_no ,count(*) amount
from salaries s
GROUP BY s.emp_no
) x
where
x.amount=(
select max(t.amount)
from (
select s.emp_no ,count(*) amount
from salaries s
GROUP BY s.emp_no
) t
)
```
## (四八)、统计出公司涨薪幅度金额最高的一次对应的员工（员工编号，名称，所属部门，涨薪幅度）
```sql
select s.emp_no 员工编号,
(select CONCAT(first_name,' ',last_name ) from employees where emp_no=s.emp_no) 名称,
(select emp_no from dept_emp where emp_no=s.emp_no and to_date='9999-01-01') 所属部门,
s.fudu 涨薪幅度
from (
select
s1.emp_no,
s2.salary-s1.salary fudu
from salaries s1
join salaries s2
on s1.emp_no=s2.emp_no
and s1.to_date=s2.from_date
) s
where s.fudu=(
select
max(s2.salary-s1.salary)
from salaries s1
join salaries s2
on s1.emp_no=s2.emp_no
and s1.to_date=s2.from_date
```
## (四九)、统计出所有部门中的薪水最高的员工信息（员工编号，名称，所属部门，薪水）-按薪水降序
```sql
select
de.emp_no 员工编号,
(select CONCAT(first_name,' ',last_name ) from employees where emp_no=de.emp_no) 名称,
(select emp_no from dept_emp where emp_no=de.emp_no and to_date='9999-01-01') 所属部门,
s.salary 薪水
from (
select emp_no,dept_no
from dept_emp
where to_date='9999-01-01'
) de
join (
select emp_no,salary
from salaries
where to_date='9999-01-01'
) s
on s.emp_no=de.emp_no
where
(de.dept_no,s.salary) in (
select de.dept_no,max(s.salary)
from (
select emp_no,dept_no
from dept_emp
where to_date='9999-01-01'
) de
join (
select emp_no,salary
from salaries
where to_date='9999-01-01'
) s on s.emp_no=de.emp_no
GROUP BY de.dept_no
ORDER BY 4
```
## (五〇)、统计出所有部门中的薪水最高的女员工信息（员工编号，名称，所属部门，薪水）-按薪水降序
```sql
select
de.emp_no 员工编号,
CONCAT(e.first_name,' ',e.last_name) 名称,
(select dept_no from dept_emp where emp_no=de.emp_no and to_date='9999-01-01') 所属部门,
s.salary 薪水
from (
select emp_no,dept_no
from dept_emp
where to_date='9999-01-01'
) de
join (
select emp_no,salary
from salaries
where to_date='9999-01-01'
) s
on s.emp_no=de.emp_no
join (
select *
from employees
where gender='F'
) e
on s.emp_no=e.emp_no
where
(de.dept_no,s.salary) in (
select de.dept_no,max(s.salary)
from (
select emp_no,dept_no
from dept_emp
where to_date='9999-01-01'
) de
join (
select emp_no,salary
from salaries
where to_date='9999-01-01'
) s on s.emp_no=de.emp_no
join (
select *
from employees where gender='F'
) e on e.emp_no=s.emp_no
GROUP BY de.dept_no
ORDER BY 4
```
