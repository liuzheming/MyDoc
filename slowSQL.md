1. 慢SQL处理
    1. 使用'EXPLAIN'分析SQL的执行过程(select,delete,update,insert,replace) [官网文档](https://dev.mysql.com/doc/refman/8.0/en/using-explain.html)
        1. id: 语句中每个select到的每张表都会对应一个id
        2. select_type:  select查询类型
        3. table: 查询的是哪张表
        4. type: join类型，判断是全表扫描还是索引扫描 **【重要】** 
            1. ALL: 遍历全表
            2. index: 遍历索引树
            3. range: 只检索给定范围的行，使用一个索引来选择行
            4. ref: 表示此表是上述表的连接匹配条件，即哪些列或常量被用于查找上述表中索引列的值
            5. eq_ref: 类似ref，区别在于所使用的的索引是唯一索引，只会找到一条匹配值
            6. const、system: 使用常量作为查询条件，system是const的特例
            7. NULL: MySQL在优化过程中分解语句，执行时甚至不用访问表或索引，例如从一个索引列里选取最小值可以通过单独索引查找完成。
            >  ALL->index->range->ref->eq_ref->const->system->NULL 性能依次提高
        5. possible_key: 本次查询可能选用的索引
        6. key: 本次查询实际使用到的索引
        7. key_len: 查询优化器用到的索引字节数，判断联合索引的使用情况
        8. ref: 哪个字段或常数与key一起被使用
        9. rows: 估算此查询一共读取多少行数据，**【重要】**
        10. filtered: 表示此查询所过滤的数据的百分比
        11. Extra: 额外的信息，如：
            1. filesort，表示MySQL需要额外排序而不能根据索引顺序达到排序的效果，通常建议优化
