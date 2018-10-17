# Oracle
### 第一条查询语句:

SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;
### 结果
![](https://github.com/bjjbox/Oracle/blob/master/result1.png)


### 第二条查询语句:
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');
### 结果
![](https://github.com/bjjbox/Oracle/blob/master/result1.png)
### 优化指导
第二条语句效率更好！！第一条语句在where处积压了太多的执行结果，<br>
导致效率更差。而第二条是在所有查询完之后再进行筛选，执行having语句
