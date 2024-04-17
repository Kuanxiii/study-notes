# Oracle-COUNT函数加条件 & CASE WHEN语句运用

## 背景

- 有一个场景需要count出FIELD_A字字段大于1的记录条数
- 直接按`MYSQL`的`IF()`形式去写会报错`ORA-00907: 缺失右括号`

## 具体操作

- MYSQL等效语句：
  - `SELECT COUNT(IF (FIELD_A > 1, 1, NULL)) FROM TABLE;`
- ORACLE需要使用`CASE`语句，下写法与`MYSQL-IF语句`等效
```SQL
SELECT COUNT(
        CASE
            WHEN FIELD_A > 1 THEN 1
            ELSE NULL
        END
    )
FROM TABLE;
```

### CASE WHEN语句运用

- `CASE WHEN`有两种表达
  1. 简单对字段进行判断
     - `CASE FIELD_A WHEN 1 THEN 1 ELSE 0 END`
     - 意为：当字段FIELD_A的值为1时，返回1，否则返回0
  2. 对字段值进行条件判断
     - `CASE WHEN FIELD_A = 1 THEN 1 ELSE 0 END`
     - 语义与上一种相同
     - 区别在于，这种写法可以and多个字段条件，并且表达式可以是任意的表达式，而不仅仅是字段值判断
- `CASE WHEN`基础语法
```sql
CASE 
  WHEN 条件1 THEN 结果1
  WHEN 条件2 THEN 结果2 
  ... 
  ELSE 结果N 
END
```
  - 就是一个程序式的表达，条件按序判断触发，优先返回
  - 在COUNT、WHERE等语句中，可以利用[SQL的三值逻辑](https://blog.csdn.net/Chase_k/article/details/136649964)，将CASE WHEN的值表达反馈在记录的命中上，从而达到不同的效果