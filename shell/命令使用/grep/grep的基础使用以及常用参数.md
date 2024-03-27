# grep的基础使用以及常用参数

## 基础使用

`grep [option] pattern [files]`

### option(非必填)

- 表示使用grep的模式
- 包括显输出、显示、帮助等等

### pattern(必填，除-V、--help类option外)

- 可以是字符串、正则表达式
- 这个代表grep进行检索的依据
- 可以理解成SQL里查询的where？
- 默认是区分大小写的

### files(非必填)

- 可以是文件、文件夹
- 默认是当前目录
- 不填的话就会读取标准输出
- 可以理解成SQL里查询的from？

## 常用参数和使用场景


