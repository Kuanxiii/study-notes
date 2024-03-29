# SQL-where子句谓词函数&三值逻辑

## 问题

这里先模拟一下问题-LeetCode-584

- 这里有一张表 `customer`

  - 里面保存了所有客户信息和他们的推荐人id,建表语句如下:
  - ```sql
    Create table If Not Exists Customer (id int, name varchar(25), referee_id int)
    ```
  - 这里插入几条记录,有些的 `referee_id` 这里表如下:
  - ```
    | id | name | referee_id |
    | -- | ---- | ---------- |
    | 1  | Will |            |
    | 2  | Jane |            |
    | 3  | Alex | 2          |
    | 4  | Bill |            |
    | 5  | Zack | 1          |
    | 6  | Mark | 2          |
    ```
  - 在这个情况下找列表中客户的推荐人的编号 不是 2 的记录
  - 一开始想得很简单,就是`select * from Customer where referee_id != 2;`这么一条
  - 但是出现了很诡异的问题,就是结果集为:
  - ```
    | name |
    | ---- |
    | Zack |
    ```
  - 所有的空值都没有检索到
  - 后续换了`<>` `not in`等,都不行

### 问题解析

- 这里有几个点
  - SQL中的where子句是怎么根据逻辑运算确定范围的
  - `!=` `<>` `not in` 是什么
  - 这些运算的结果是什么,有哪些可能性
  - SQL中逻辑

#### where子句如何判定

首先我们知道几点:
- `where`子句的目的:筛选满足条件的集合
- `where`子句的做法:通过一个个逻辑表达式,再通过交|并的方式筛选集合

我们也知道,where子句中都是一个个的`命题`(中学都学过啊[旺柴],这里最好联想离散数学中的`命题逻辑`),那么命题的概念就是:使用语言、符号或式子表达的,可以判断真假的陈述句.

像xx列 = xx值,这些都是判断条件,中间用and或者or连接起来,依旧是一个命题

命题有真有假(下面会讲到真假以外的情况,也就是三值逻辑),SQL这里就是把命题判断为真的拿出来了

#### 谓词

这里还是得回到离散数学,这里会用到离散数学那时候学的`命题逻辑`和`谓词逻辑`,这里抄一波书:

> 在命题逻辑中,命题是具有真假意义的陈述句。从语法上分析,一个陈述句由主语和谓语组成。在谓词逻辑中,为揭示命题内部接口及不同命题的内部结构关系,就按照这两部分对命题进行分析,并且把主语称为个体或客体,把谓语称为谓词。

> 用以描述个体的性质或个体间关系的部分,称为谓词

- 这时候我们提出的`!=` `<>` `not in` 是什么这个问题就很明白了,是谓词
- 在SQL中,很多都是谓词,像`>`,`<>`,`=` 等比较谓词,以及`BETWEEN`, `LIKE`, `IN`, `IS NULL`等都是。
- 总结,在SQL中,谓词是一种函数,返回值是真值(随便理解一下,就是可能的返回值...虽然这么说相当于没说)。

#### 真值

真值是一个变量本身所具有的真实值

在SQL函数这里,可以用真值表来理解,概念在离散数学中(可恶啊,怎么又是tm离散数学)

谓词函数是一个命题公式对吧

> 真值表是在逻辑中使用的一类数学表，用来确定一个表达式是否为真或有效。 (表达式可以是论证；就是说，表达式的合取，它的每个结合项(conjunct)都是最后要做的结论的一个前提。)

一般来说,谓词表达式的真值有四种
- `true`:很好理解,真的
- `false`:很难理解,假的
- `unknown`好不好理解,不知道
  - "我明天将会吃一碗面条" 这句话是`true`还是`false`
  - 显然是不知道呀,神tm知道我明天到底吃不吃面条,吃一碗还是两碗还是一盆
  - 这就是`unknown`
- `inapplicable`你说什么?我不理解
  - 这里换个问题
  - "你现在看的这篇博客是男的"  这句话是`true`还是`false`
  - 这都不是知不知道的问题了,神都不知道了
  - "博客"根本就没有"性别"这个属性,显然这个结果就是"不适用的"

#### SQL中的三值逻辑

SQL中就是将上面最后两个`unknown`和`inapplicable`合并成了一个--NULL

SQL-where子句中的谓词返回
- true
  - 符合了条件,where子句把记录取出
- false
  - 不符合条件,where子句不把记录取出
- null
  - 不知道符不符合条件,取出来干嘛(这里猜想如果取出来.那么update将会多么恐怖..一行下去改了一大堆)

既然false和null都是不返回数据,怎么判定呢?

可以用not()函数试试理解

- 首先`SELECT * FROM customer WHERE NOT(1 = 1)`,`1=1`必定为`true`,not()后是`false`这显然啥都不会返回吧
- 然后`SELECT * FROM customer WHERE 1 = 1`,`1=1`必定为`true`,这显然都返回了吧
- 接下来是重点,试试这个`SELECT * FROM customer WHERE 1 = null`,啥都没有返回
- 加上not,`SELECT * FROM customer WHERE NOT(1 = null)`,还是啥都没有返回

这里可以理解了吧,`1 = null`这里的谓词函数返回是`null`,`null`又没有真假,not一下还是不知道,那当然还是`null`咯

因为查询结果只会包含WHERE子句里的判断结果为true的行！原因上面有嗷

> 重点理解： **NULL不是值，所以不能对其使用谓词函数，如果使用谓词函数，结果是null**。 

ps:SQL里`is null` 这个整体是一个谓词函数,单独的`is`并不是谓词函数

`referee_id = null` 和 `referee_id is null`的区别就是:
- 一个是函数`相等不(referee_id, null)`,有null参与,所以不知道;
- 另一个是函数`是NULL不(referee_id)`,实际上并没有null参与,所以它知道;

### 问题解决

问题解决贼简单,上面不是知道`is null`是没有null参与的谓词函数了嘛,这里直接并上就完事;

`select * from Customer where referee_id is null or referee_id != 2;`

ps:业务里where条件最好还是有个默认值吧..没有的话就注意一下null的情况
ps1:还有啊,尤其是int类型,别整个`null = null + 1`出来,必tm报错