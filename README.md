# Oracle
### 第一条查询语句:

SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;
### 结果
![第一条结果](https://github.com/bjjbox/Oracle/tree/master/test1/result1.png)
