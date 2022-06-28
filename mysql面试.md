## 1、Explain执行计划详解
一、id

id： ：表示查询中执行select子句或者操作表的顺序，id的值越大，代表优先级越高，越先执行。 
有几个 select 就有几个id，id列越大执行优先级越高，id相同则从上往下执行，id为NULL最后执行

二、type
type：查询使用了何种类型，它在SQL优化中是一个非常重要的指标。
type 常用的取值有:
  system: 表中只有一条数据. 这个类型是特殊的 const 类型.
  const: 针对主键或唯一索引的等值查询扫描, 最多只返回一行数据. const 查询速度非常快, 因为它仅仅读取一次即可.

以下性能从好到坏依次是：system>const>eq_ref>ref>ref_or_null>index_merge>unique_subquery>index_subquery>range>index>ALL

六、possible_keys
possible_keys：表示在MySQL中通过哪些索引，能让我们在表中找到想要的记录，一旦查询涉及到的某个字段上存在索引，则索引将被列出，但这个索引并不定一会是最终查询数据时所被用到的索引

七、key
key：区别于possible_keys，key是查询中实际使用到的索引，若没有使用索引，显示为NULL。具体请参考上边的例子。

当 type 为 index_merge 时，可能会显示多个索引。

八、key_len
  key_len：表示查询用到的索引长度（字节数），原则上长度越短越好 。

单列索引，那么需要将整个索引长度算进去；
多列索引，不是所有列都能用到，需要计算查询中实际用到的列。
注意：key_len只计算where条件中用到的索引长度，而排序和分组即便是用到了索引，也不会计算到key_len中。

九、ref
  ref：常见的有：const，func，null，字段名。

当使用常量等值查询，显示const，
当关联查询时，会显示相应关联表的关联字段
如果查询条件使用了表达式、函数，或者条件列发生内部隐式转换，可能显示为func

十、rows
rows：以表的统计信息和索引使用情况，估算要找到我们所需的记录，需要读取的行数。
这是评估SQL 性能的一个比较重要的数据，mysql需要扫描的行数，很直观的显示 SQL 性能的好坏，一般情况下 rows 值越小越好

十一、Extra
Extra ：不适合在其他列中显示的信息，Explain 中的很多额外的信息会在 Extra 字段显示。

1、Using filesort
表示无法利用索引完成的排序操作，也就是ORDER BY的字段没有索引，通常这样的SQL都是需要优化的。表示按文件排序，一般是在指定的排序和索引排序不一致的情况才会出现。

2、Using index
Using index：我们在相应的 select 操作中使用了覆盖索引，通俗一点讲就是查询的列被索引覆盖，使用到覆盖索引查询速度会非常快，SQl优化中理想的状态。

什么又是覆盖索引?

一条 SQL只需要通过索引就可以返回，我们所需要查询的数据（一个或几个字段），而不必通过二级索引（需要回表查询），查到主键之后再通过主键查询整行数据（select * ）。

3、Using temporary
查询有使用临时表, 一般出现于排序, 分组和多表 join 的情况, 查询效率不高, 建议优化.


## 2、mybatis占位符${}、#{}  https://blog.csdn.net/ypxcan/article/details/121160797
### #传入的参数在SQL中显示为字符串，$传入的参数在SqL中直接显示为传入的值.
### #方式能够很大程度防止sql注入，$方式无法防止Sql注入；

{}占位符的特点：
使用PrepareStatement对象执行sql语句;
使用PrepareStatement对象，能避免sql注入，sql语句执行更安全;
#{}常常作为列值使用的，位于等号的右侧，#{}位置的值和数据类型有关的;

${}的特点

使用Statement对象，执行sql语句，效率低;
${}占位符的值，使用的字符串连接方式，有sql注入的风险。有代码安全的问题;
${}数据是原样使用的,不会区分数据类型;
常用作表名或者列明，再能保证数据安全的情况下使用{};

在mybatis中#和$的主要区别
（1）#传入的参数在sql中显示为字符串，$传入的参数在sql中直接显示为传入的值，

（2）#方式能够很大程度防止sql注入，$方式无法防止Sql注入

（3）能用 #{} 的地方就用 #{}，不用或少用 ${}

（4）表名作参数时，必须用 ${}。如：select * from ${tableName}

（5）order by 时，必须用 ${}。如：select * from t_user order by ${columnName}

 (6) ${}可以从properties文件中获取值，也可以作为表名，列名等。
