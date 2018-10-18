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
### 优化指导
where查询处效率低，两个筛选就是一条一条进行筛选，效率不高。
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
无

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
where处的查询效率过低，数据一条一条的查询。应该一个集合一个集合的进行筛选。
### 总结
第二条语句效率更好！！第一条语句在where处相当于一条一条的进行筛选，where处两个筛选语句<br>
进行了两次筛选，导致效率更差。而第二条是在所有查询完之后再进行筛选，相当于把一个集合进行筛选<br>
执行having语句，效率会更快！！！

