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
![](https://github.com/bjjbox/Oracle/blob/master/test1/images/优化3.png)
### 总结
1:从内存的使用：第一台条语句的查询消耗的内存少于第二条语句的内存，和第三条内存消耗差不多。<br>
2:从语句上来说，where的效率是要比having效率高的，我们应该尽量使用where进行筛选结果。<br>
3:从扫描方式：第一条在where处建立了索引，是索引查询，而第二条是全表查询，然后再筛选，故而第一条<br>
的效率高些，索引查询比全表的效率更高。

