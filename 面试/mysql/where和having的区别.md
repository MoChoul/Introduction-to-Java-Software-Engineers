这两个都是添加查询条件用的。where的话就是拼接普通字段的查询条件，having后边跟上聚合之后数据的查询条件。

比如计算平均薪资在10k以上的部门信息，这会儿的话就要用select xx from table group by deptId having avg(salary)>10000

常用的聚合函数有：count、sum、avg、min、max