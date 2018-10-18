# Oracle
### 第一条查询语句:

SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;
### 结果
![](https://github.com/bjjbox/Oracle/blob/master/result.png)

### 第二条查询语句:
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');

### 优化对比
第一条：
![](https://github.com/bjjbox/Oracle/blob/master/test1/images/优化1.png)
第二条：
![](https://github.com/bjjbox/Oracle/blob/master/test1/images/优化2.png)
### 自定义语句
SELECT d.department_name ,count(e.job_id)as "部门总人数" ,
avg(e.salary)as "平均工资"
FROM hr.departments d, hr.employees e
WHERE d.department_id = e.department_id
and d.department_name = 'IT' or d.department_name = 'Sales'
GROUP BY department_name;

### 结果
![](https://github.com/bjjbox/Oracle/blob/master/result3.png)

### 优化指导

### 总结


