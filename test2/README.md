### 1:自己的用户名
我的角色是baijunjie，我的用户名是bjjbox。<br>
### 2:角色创建正确
角色是baijunjie<br>
CREATE ROLE baijunjie;
### 3:用户创建及角色分配正确
创建用户<br>
CREATE USER bjjbox IDENTIFIED BY 123 DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;
角色分配<br>
GRANT baijunjie TO bjjbox
### 4:表空间分配正确
这是在给表分配50m的空间<br>
ALTER USER bjjbox QUOTA 50M ON users
### 5:对象创建及共享的设置正确
共享的设置<br>
GRANT SELECT ON myview TO public
