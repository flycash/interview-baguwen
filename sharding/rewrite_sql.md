# 改写 SQL 
分析：改写 SQL 是分库分表的第一个核心步骤。改写 SQL 的目标是生成物理 SQL，或者说目标 SQL。改写 SQL 主要受到两方面的影响：
- 查询本身
- 分库分表规则

举个典型例子来说，SELECT * FROM user_tab WHERE id < 10000 这条语句：
- 如果是哈希分库分表，那么大概率是广播，即将查询发到全部表上
- 如果是范围分表，那么就可以根据范围来计算出准确的目标表

而如果是 SELECT avg(age) FROM user_tab，那么不管是范围还是哈希分库分表，都要改写为 SELECT count(id), sum(age) FROM user_tab，然后在处理结果的时候根据 count 和 sum 来求平均值。

