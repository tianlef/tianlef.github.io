# SQL(1)

## 联合索引

- 创建联合索引 `ALTER TABLE table_name add Index name_phone(name,phone)`
- 联合索引怎么查找数据
  - 索引的第一项数据整体有序
  - 在第一项数据有序的前提下，第二项数据有序
- 联合索引和单值索引的关系
  - 当属性列*A*作为联合索引的第一项时，没必要对*A*创建单值索引
  - index(a,b,c) = index(a), index(a,b), index(a,b,c)
- 最左前缀规则
  - 严格遵守属性列的排序
  
## 覆盖索引

- 非主键索引，有回表过程，
- 可以减少IO次数，减少数据的访问量，提升查询效率

## 索引条件下推

- 联合索引中，排序为一的属性可以索引，排序为二的属性使用了模糊查询等
- 如果开启索引条件下推，则排序为二属性也使用索引查找
- 不开启，则查完第一项属性之后，回主键表查询数据，再进行筛选
- 开启命令，`set optimizer_switch='index_condition_pushdown'=on`
- 5.6之后完善的功能，只适用于二级索引

## 前缀索引

- 在很长的字符列(BLOB, TEXT, VARCHAR)上创建索引，会造成索引特别大并且特别慢
  - 解决方案：
  - 使用模拟哈希索引
  - 使用前缀索引
- 使用索引列最左的n个字符来建立索引
  - 缺点：无法使用前缀索引做`order by`, `group by`和覆盖扫描
- 创建前缀索引关键在于选择足够长的前缀保证索引选择性
  - 可以让MySQL在查找时过滤掉更多的数据行
- 命令
  - `CREATE INDEX idx_author_email ON author(email(3))`

## MySQL 游标

- Intro
  - 查询时结果集中会返回多条记录，需要返回第一行，下一行，前十行数据时可以用到游标
  - 也称为光标，标识数据取到了什么地方
  - MySQL只能用于存储过程和函数
- 声明
  - `DECLARE cursor_name CURSOR FOR select_statement`
  - `select_statement`表示select语句
- 打开游标
  - `OPEN cursor_name`
  - 指向第一条记录的前边
- 使用游标
  - `FETCH cursor_name INTO var_name[,var_name]...`
  - `var_name`必须在游标使用之前定义
- 关闭游标
  - `CLOSR cursor_name`