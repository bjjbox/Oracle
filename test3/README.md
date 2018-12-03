## 用户
用户名：bjjbox<br>
密。码：123<br>
### 创建表 order
CREATE TABLE orders 
 (
  order_id NUMBER(10, 0) NOT NULL  primary key
  , customer_name VARCHAR2(40 BYTE) NOT NULL 
  , customer_tel VARCHAR2(40 BYTE) NOT NULL 
  , order_date DATE NOT NULL 
  , employee_id NUMBER(6, 0) NOT NULL 
  , discount NUMBER(8, 2) DEFAULT 0 
  , trade_receivable NUMBER(8, 2) DEFAULT 0 
 ) 
 TABLESPACE USERS
 PCTFREE 10 INITRANS 1
 STORAGE (   BUFFER_POOL DEFAULT )
 NOCOMPRESS NOPARALLEL
 PARTITION BY RANGE (order_date)
 (
  PARTITION PARTITION_BEFORE_2016 VALUES LESS THAN (
  TO_DATE(' 2016-01-01 00:00:00', 'YYYY-MM-DD HH24:MI:SS',
  'NLS_CALENDAR=GREGORIAN'))
  NOLOGGING
  TABLESPACE USERS
  PCTFREE 10
  INITRANS 1
  STORAGE
 (
  INITIAL 8388608
  NEXT 1048576
  MINEXTENTS 1
  MAXEXTENTS UNLIMITED
  BUFFER_POOL DEFAULT
 )
 NOCOMPRESS NO INMEMORY
 , PARTITION PARTITION_BEFORE_2017 VALUES LESS THAN (
 TO_DATE(' 2017-01-01 00:00:00', 'YYYY-MM-DD HH24:MI:SS',
 'NLS_CALENDAR=GREGORIAN'))
 NOLOGGING
 TABLESPACE USERS02
  PCTFREE 10
  INITRANS 1
  STORAGE
 (
  INITIAL 8388608
  NEXT 1048576
  MINEXTENTS 1
  MAXEXTENTS UNLIMITED
  BUFFER_POOL DEFAULT
 )
 NOCOMPRESS NO INMEMORY
 , PARTITION PARTITION_BEFORE_2018 VALUES LESS THAN (
 TO_DATE(' 2018-01-01 00:00:00', 'YYYY-MM-DD HH24:MI:SS',
 'NLS_CALENDAR=GREGORIAN'))
 NOLOGGING
 TABLESPACE USERS03
  PCTFREE 10
  INITRANS 1
  STORAGE
 (
  INITIAL 8388608
  NEXT 1048576
  MINEXTENTS 1
  MAXEXTENTS UNLIMITED
  BUFFER_POOL DEFAULT
 )
 NOCOMPRESS NO INMEMORY
 );
 ### order_details
 CREATE TABLE order_details
(
id NUMBER(10, 0) NOT NULL
, order_id NUMBER(10, 0) NOT NULL
, product_id VARCHAR2(40 BYTE) NOT NULL
, product_num NUMBER(8, 2) NOT NULL
, product_price NUMBER(8, 2) NOT NULL
, CONSTRAINT order_details_fk1 FOREIGN KEY  (order_id)
REFERENCES orders  (order_id)
ENABLE
)
TABLESPACE USERS
PCTFREE 10 INITRANS 1
STORAGE (   BUFFER_POOL DEFAULT )
NOCOMPRESS NOPARALLEL
PARTITION BY REFERENCE (order_details_fk1)
(
PARTITION PARTITION_BEFORE_2016
NOLOGGING
TABLESPACE USERS
PCTFREE 10
 INITRANS 1
 STORAGE
(
 INITIAL 8388608
 NEXT 1048576
 MINEXTENTS 1
 MAXEXTENTS UNLIMITED
 BUFFER_POOL DEFAULT
)
NOCOMPRESS NO INMEMORY,
PARTITION PARTITION_BEFORE_2017
NOLOGGING
TABLESPACE USERS02
PCTFREE 10
 INITRANS 1
 STORAGE
(
 INITIAL 8388608
 NEXT 1048576
 MINEXTENTS 1
 MAXEXTENTS UNLIMITED
 BUFFER_POOL DEFAULT
)
NOCOMPRESS NO INMEMORY,
PARTITION PARTITION_BEFORE_2018
NOLOGGING
TABLESPACE USERS03
PCTFREE 10
 INITRANS 1
 STORAGE
(
 INITIAL 8388608
 NEXT 1048576
 MINEXTENTS 1
 MAXEXTENTS UNLIMITED
 BUFFER_POOL DEFAULT
)
NOCOMPRESS NO INMEMORY
);
### 表创建情况<br>
![](https://github.com/bjjbox/Oracle/blob/master/test3/image/表分区.png)<br>
### 分配权限<br>
分配查询权限<br>
![](https://github.com/bjjbox/Oracle/blob/master/test3/image/权限.png)<br>
分配表空间权限<br>
grant UNLIMITED TABLESPACE to bjjbox;<br>
### 插入语句
declare
  dt date;
  V_EMPLOYEE_ID NUMBER(6);
  v_order_id number(10);
  v_name varchar2(100);
  v_tel varchar2(100);
  v_product_id varchar2(100);
begin
  for i in 1..10000
  loop
    if i mod 2 =0 then
      dt:=to_date('2016-1-1','yyyy-mm-dd')+(i mod 60);
      v_product_id:= '2';
    else if i mod 3=0 then
      dt:=to_date('2017-6-1','yyyy-mm-dd')+(i mod 60);
      v_product_id:='3';
      else
       dt:=to_date('2018-10-1','yyyy-mm-dd')+(i mod 60);
       v_product_id:='1';
       end if;
    end if;
    V_EMPLOYEE_ID:=CASE I MOD 6 WHEN 0 THEN 11 WHEN 1 THEN 111 WHEN 2 THEN 112
                                WHEN 3 THEN 12 WHEN 4 THEN 121 ELSE 122 END;
    v_order_id:=i;
    v_name := 'bjjbox' || i;
    v_tel := '13088120536';
    insert  into ORDERS (ORDER_ID,CUSTOMER_NAME,CUSTOMER_TEL,ORDER_DATE,EMPLOYEE_ID,DISCOUNT)
      values (v_order_id,v_name,v_tel,dt,V_EMPLOYEE_ID,dbms_random.value(100,0));  
      insert  into ORDER_DETAILS (ID,ORDER_ID,PRODUCT_ID,PRODUCT_NUM,PRODUCT_PRICE)
      values (v_order_id,v_order_id,v_product_id,dbms_random.value(1000,0),dbms_random.value(100,0));
  end loop;
end;<br>
### 循环插入，使用for in 循环<br>
<br>
### 查询数据匹配条数<br>
select count(*) from orders a,order_details b where a.order_id=b.order_id;<br>
查询两张表中相匹配数据的条数，因为插入数据时两张表是同时插入，order_details表中的order_id即为当时的orders表的id,
则查询出数据为1万条
### 查询匹配数据<br>
select * from orders a INNER JOIN order_details b ON (a.order_id=b.order_id);
### 结果<br>
![](https://github.com/bjjbox/Oracle/blob/master/test3/image/2.png)<br>
### 总结<br>
对于整个查询语句来说，使用的访问方式内嵌套循环，故效率较高。
同时，使用的内存较小，花费资源较少。
