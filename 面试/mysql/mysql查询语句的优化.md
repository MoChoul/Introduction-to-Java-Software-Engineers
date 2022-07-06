对查询进行优化，应尽量避免全表扫描，首先应考虑在 where 及 order by 涉及 的列上建立索引。

应尽量避免在 where 子句中使用!=或<>操作符，否则引擎将放弃使用索引而进行 全表扫描。

应尽量避免在 where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用 索引而进行全表扫描，如： select id from t where num is null 可以在 num 上设置默认值 0，确保表中 num 列没有 null 值，然后这样查询:select id from t where num=0

应尽量避免在 where 子句中使用 or 来连接条件，否则将导致引擎放弃使用索引 而进行全表扫描，如： select id from t where num=10 or num=20 ，可以使用可以这样查询： select id from t where num=10 union all select id from t where num=20

以%开头的模糊查询也会导致全表扫描： select id from t where name like '%abc%' ，如果要提高效率的话，可以考虑全文检索来解决。

in 和 not in 也要慎用，否则会导致全表扫描，如： select id from t where num in(1,2,3) 对于连续的数值，能用 between 就不要用 in 了： select id from t where num between 1 and 3

应尽量避免在 where 子句中对字段进行表达式操作，这将导致放弃使用索引 而进行全表扫描。如：select id from t where num/2=100 应改为: select id from t where num=100*2

应尽量避免在 where 子句中对字段进行函数操作，这将导致引擎放弃使用索引而 进行全表扫描。

比如说查询name以abc开头的数据： select id from t where substring(name,1,3)='abc' ，可以改为select id from t where name like 'abc%'

不要在 where 子句中的“=”左边进行函数、算术运算或其他表达式运算，否则系 统将可能无法正确使用索引。

在使用索引字段作为条件时，如果该索引是复合索引，那么必须使用到该索引中 的第一个字段作为条件时才能保证系统使用该索引，否则该索引将不会被使用，并且应尽可 能的让字段顺序与索引顺序相一致。

很多时候用 exists 代替 in 是一个好的选择： select num from a where num in(select num from b) 用下面的语句替换： select num from a where exists(select 1 from b where num=a.num)

并不是所有索引对查询都有效，SQL 是根据表中数据来进行查询优化的，当索引 列有大量数据重复时，SQL 查询可能不会去利用索引，如一表中有字段 sex，男、女的值 几乎各一半，那么即使在 sex 上建了索引也对查询效率起不了作用。

索引并不是越多越好，索引固然可以提高相应的 select 的效率，但同时也降低 了 insert 及 update 的效率，因为 insert 或 update 时有可能会重建索引，所以怎样建索 引需要慎重考虑，视具体情况而定。一个表的索引数最好不要超过 6 个