# java关键字汇总

把java里的字都罗列一下, 省得阅读代码时忽然碰到一个不常用的一脸懵逼

## 字分类

- 关键字
  - 访问控制		定义可访问的范围
  - 修饰符		**类|变量|代码块|接口|方法修饰(多,杂,难理解的都在这了)**
  - 程序控制		逻辑控制关键字
  - 异常处理      	不解释
  - 数据类型     	不解释
  - 对象引用      	描述这里要用什么
  - 包相关		不解释
- 保留字		不解释
- 直接量		这个比较特殊

## 字汇总(50个关键字、2个保留字、三个直接量)

| 访问控制      | 修饰符       | 程序控制   | 异常处理 | 数据类型 | 对象引用 | 包相关  | 保留字 | 直接量 |
| ------------- | ------------ | ---------- | -------- | -------- | -------- | ------- | ------ | ------ |
| private       | class        | for        | try      | boolean  | new      | import  | goto   | true   |
| protected     | abstract     | break      | catch    | byte     | super    | package | const  | false  |
| default(缺省) | extends      | continue   | finally  | char     | this     |         |        | null   |
| public        | implements   | do         | throw    | int      | void     |         |        |        |
|               | interface    | while      | throws   | long     |          |         |        |        |
|               | final        | if         |          | float    |          |         |        |        |
|               | strictfp     | else       |          | double   |          |         |        |        |
|               | static       | switch     |          | short    |          |         |        |        |
|               | synchronized | case       |          |          |          |         |        |        |
|               | transient    | default    |          |          |          |         |        |        |
|               | volatile     | instanceof |          |          |          |         |        |        |
|               | native       | return     |          |          |          |         |        |        |
|               | enum         | assert     |          |          |          |         |        |        |

ps：细心的可能数出来是51个关键字。。。是因为default用了两次, 一次是访问控制的缺省, 一次是程序控制那switch用的default

## 字解析

### 访问控制

这里不多谈, 先上表格

| 修饰符    | 当前类 | 当前包 | 子类 | 其他包(非子类) |
| --------- | ------ | ------ | ---- | -------------- |
| private   | √      |        |      |                |
| protected | √      | √      |      |                |
| default   | √      | √      | √    |                |
| public    | √      | √      | √    | √              |

可谓是整整齐齐, 这也能看出java对类之间关系优先级的逻辑

- private-私有的

  - 被修饰的客体为当前类所私有的, 别的类都无法访问
- protected-被保护的

  - 这里的意思其实就是说被包所保护
  - 写jar依赖的时候经常会用到, 毕竟只是jar包内需要的
  - 写多模块也经常用, 模块间调用不能乱
  - 但新手最初接触可能不知道这关键字存在的意义
- default-缺省的

  - 不写访问控制修饰符的时候, 就默认是这个
  - 缺省是从OO(面对对象 Object Oriented)角度出发的访问控制
  - java默认不是一个包、没有继承关系, 就没啥关系
- public-公共的

  - 都能用

### 修饰符

这里有些涉及到并发编程那块的, 之后补补链接

- class-类声明
  - class 关键字用来声明新的类。类是面向对象的程序设计方法的基本构造单位。要使用类, 通常使用 new 操作符将类的对象实例化, 然后调用类的方法来访问类的功能。
- abstract-抽象声明
  - abstract关键字可以修饰类或方法。abstract类可以扩展（增加子类）, 但不能直接实例化。abstract方法不在声明它的类中实现, 但必须在某个子类中重写。采用 abstract方法的类本来就是抽象类, 并且必须声明为abstract。
- extends-继承声明
  - extends 关键字用在 class 或 interface 声明中, 像A extends B, 代表是A继承于B, 是B的子类。子类继承父类的所有除 private 的变量和方法（但是不包括构造函数）。 子类可以重写父类的任何非 final 方法。一个类只能继承一个其他类, 但一个接口可以继承多个接口。
- implements-实现声明
  - implements 关键字在 class 声明中使用, 以指示所声明的类提供了在 implements 关键字后面的名称所指定的接口中所声明的所有方法的实现。类必须提供在接口中所声明的所有方法的实现。一个类可以实现多个接口。
- interface-接口声明
  - 声明一个接口, 接口是方法的集合
- final-最终的、不可改变的
  - 这个可以修饰类\方法\变量, 甚至可以修饰方法参数...
  - 值得单独水一篇
  - 之后会放链接在这
- strictfp-精准的
  - 精确浮点运算
  - 在Java虚拟机进行浮点运算时, 如果没有指定strictfp关键字时, Java的编译器以及运行环境在对浮点运算的表达式是采取一种近似于我行我素的行为来完成这些操作, 以致于得到的结果往往无法令人满意。而一旦使用了strictfp来声明一个类、接口或者方法时, 那么所声明的范围内Java的编译器以及运行环境会完全依照浮点规范IEEE-754来执行。因此如果想让浮点运算更加精确, 而且不会因为不同的硬件平台所执行的结果不一致的话, 那就请用关键字strictfp。
  - 可以将一个类、接口以及方法声明为strictfp, 但是不允许对接口中的方法以及构造函数声明strictfp关键字。
- static-静态声明
  - 这个可以修饰属性\方法\代码块\类...
  - 又是一个可以单独水一篇的
  - 等链接
- synchronized-串行化的
  - 在多线程时应用, 表明所修饰的部分是同步的
  - 可以修饰方法或代码块
  - 可以防止代码块被多个线程同时执行
  - 这个细讲的话应该在java并发基础那块
  - 等链接
- transient-暂时的
  - 这个用于java序列化
  - 修饰类的成员变量
  - 被修饰的变量不会被序列化
- volatile-易失的
  - volatile 关键字用于表示可以被多个线程异步修改的成员变量
  - 保证可见性, 不保证原子性
  - 当写一个volatile变量时, JMM会把该线程本地内存中的变量强制刷新到主内存中去
  - 这个细讲也会在并发编程基础那里
  - 等链接
- native-本地的
  - java毕竟不是直接能和操作系统交互的语言
  - native关键字修饰的方法意思就是, 这方法的实现不归java管, 是调用的本地方法(别的语言写的)
- enum-枚举
  - 枚举类型（Enumerated Type） 很早就出现在编程语言中, 它被用来将一组类似的值包含到一种类型当中。而这种枚举类型的名称则会被定义成独一无二的类型描述符, 在这一点上和常量的定义相似。不过相比较常量类型, 枚举类型可以为申明的变量提供更大的取值范围。

### 程序控制

流程控制这些直接用实例来说明, 这里要是不懂。。建议菜鸟教程

- if-else
  - 不解释
  - ```java
    if(boolean){
        ...
    }else{
        ...
    }
    ```
- for & continue & break
  - 不解释
  - ```java
    for(int i = 0; i < 10;i++){
        if(i == 5) continue;
        if(i == 9) break;
    }
    ```
  - 遍历集合
  - ```java
    for(类型声明 命名 : 集合){
        ...
    }
    ```
- while
  - 条件里的boolean为true则执行循环体
  - ```java
    while (boolean) {
        ...
    }
    ```
  - 还有do-while, 不同是while先判断再执行, do-while是先执行do再while判断, 也就是说不论如何do内代码块都会执行至少一次
  - ```java
    do {
        ...
    }while(boolean)
    ```
- switch
  - 其实加break的话就是if-else if-else if-...-else
  - 不加的话就是嵌套的if-if-if-...
  - 像下面的语句, 输入1, 如果没有break, 会打出12default, 有的话就打1
  - 顺序是有影响的(没有break的情况, 或者default在前面default没有break)。这里就不多说了, 有兴趣可以自行验证。主要记得加break就行
  - ```java
    switch(变量){
        case 值1 :
          System.out.println("1");
          break; //可选, 但不写的话会一个一个case执行下去。。一直到遇到break或者return或者switch结束
        case 值2 :
          System.out.println("2");
          break; //可选
        ...
        default : //可选
          System.out.println("default");
    }
    ```
- instanceof
  - 对象类型判断, 前面一个对象实例, 后面一个类
  - 输出一个boolean, 说这个实例是不是后面那个类的实例
  - 继承也算, 经常用于判断是不是xx接口的实例或者是xx抽象类的实例
  - 跑题：这是判断实例和类的关系, 如果你想判断类和类的关系可以用class下面的isAssignableFrom()方法
  - ```java
    boolean 是他儿子 = 需要判断的对象 instanceof 已知对象;
    ```
- return
  - 不谈
- assert
  - 断言
  - 在单元测试里用的多, 业务上可能用它来控制抛个异常？。。。
  - 笔者没在业务代码里怎么用过

### 异常处理

直接上代码：
```java
public static void main(String[] args) throws Exception { //这里的throws意思是定义这个方法会抛出啥
    try {// try后面也可以加括号(),里面写try里面要用的资源,执行完自动释放
        //...可能发生异常的代码块
    } catch (Exception e) {// 捕捉什么样的异常
        throw new Exception();// 抛出一个异常,异常可以自己定义
    } finally {// 不论是否发生异常, finally都会执行
        System.out.println("完事");
    }
}
```

### 数据类型

瞅:

| 类型名称     | 关键字  | 占用内存 | 取值范围                                   |
| ------------ | ------- | -------- | ------------------------------------------ |
| 字节型       | byte    | 1 字节   | -128~127                                   |
| 短整型       | short   | 2 字节   | -32768~32767                               |
| 整型         | int     | 4 字节   | -2147483648~2147483647                     |
| 长整型       | long    | 8 字节   | -9223372036854775808L~9223372036854775807L |
| 单精度浮点型 | float   | 4 字节   | +/-3.4E+38F（6~7 个有效位）                |
| 双精度浮点型 | double  | 8 字节   | +/-1.8E+308 (15 个有效位）                 |
| 字符型       | char    | 2 字节   | ISO 单一字符集  0-65535                    |
| 布尔型       | boolean | 1 字节   | true 或 false                              |

### 对象引用

- new 实例化一个对象, 后面跟构造方法
  - `Xxxx xx = new Xxxx();`
- super 引用父类或超类
- this 引用当前对象
- void 声明无返回值,用在方法返回类型定义上

### 包相关

- import 引入
  - import 关键字使一个包中的一个或所有类在当前 Java 源文件中可见。可以不使用完全限定的类名来引用导入的类。
  - 当多个包包含同名的类时, 许多 Java 程序员只使用特定的 import 语句（没有*）来避免不确定性。

- package 包
  - package 关键字指定在 Java 源文件中声明的类所驻留的 Java 包。
  - package 语句（如果出现）必须是 Java 源文件中的第一个非注释性文本。 例:java.lang.Object。 如果 Java 源文件不包含package 语句, 在该文件中定义的类将位于“默认包”中。请注意, 不能从非默认包中的类引用默认包中的类。

### 保留字

> 正确识别java语言的关键字（keyword）和保留字（reserved word）是十分重要的。Java的关键字对java的编译器有特殊的意义, 他们用来表示一种数据类型, 或者表示程序的结构等。保留字是为java预留的关键字, 他们虽然现在没有作为关键字, 但在以后的升级版本中有可能作为关键字。 识别java语言的关键字, 不要和其他语言如c/c++的关键字混淆。 const和goto是java的保留字。 所有的关键字都是小写。

- goto
  - 保留字,没用,不需要,不允许
- const
  - 别的语言用来定义静态常量的,意思是不能更新,和final差不多

### 直接量

没啥好说的..null的话还有点.但都得结合实例

- null 空的,嘛也没有
- true boolean值
- false boolean值