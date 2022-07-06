存储过程
存储程序是被存储在服务器中的组合SQL语句，经过创建编译并保存在数据库中，用户可通过存储过程的名字调用执行。存储过程核心思想就是数据库SQL语言层面的封装与复用。使用存储过程可以较少应对系统的业务复杂性，但是会增加数据库服务器系统的负荷，所以在使用的时候需要综合业务考虑。

对应存储过程的名字使用call调用 ，把对应的参数传递进去，输出参数使用@声明

基本语法，了解熟悉一下

-- 创建存储过程

DROP PROCEDURE IF EXISTS p01_discount;  //如果存在先删掉再创建 

CREATE PROCEDURE p01_discount(IN consume NUMERIC(5,2),OUT payfee NUMERIC(5,2)) //声明存储过程，in输入参数  out输出参数

BEGIN

​       --判断收费方式

​       IF(consume>100.00AND consume<=300.00) THEN

​              SET payfee=consume*0.8;

​       ELSEIF(consume>300.00) THEN 

​              SET payfee=consume*0.6;

​       ELSE

​              SET payfee = consume;

​       END IF;

​       SELECT payfee AS result;

END ;

-- 调用存储过程

CALL p01_discount(100.0,@discount); 
视图
视图本身是一张虚拟表，不存放任何数据。在使用SQL语句访问视图的时候，获取的数据是MySQL从其它表中生成的，视图和表在同一个命名空间（因为表和视图共享数据库中相同的名称空间，因此，数据库不能包含具有相同名称的表和视图）。视图查询数据相对安全，视图可以隐藏一些数据和结构，只让用户看见权限内的数据，使复杂的查询易于理解和使用。

原来我们公司做过一个项目的时候，用的是5张表的联查，然后用sql语句来写的话，比较慢，比较麻烦，然后我们把这5张表的联查创建了一个视图，然后就直接查找的是视图，查询速度快，这个视图就是只能做查询，而不能做增删改操作

基本语法，了解熟悉一下

-- 创建视图

CREATE OR REPLACE VIEW user_order_view AS 

SELECT

​       t1.id,t1.user_name,t2.order_no,t2.good_id,

​       t2.good_name,t2.num,t2.total_price

FROM v01_user t1

LEFT JOIN v02_order t2 ON t2.user_id =t1.id;

-- 视图调用

SELECT * FROM user_order_view WHERE user_name='Cicada';