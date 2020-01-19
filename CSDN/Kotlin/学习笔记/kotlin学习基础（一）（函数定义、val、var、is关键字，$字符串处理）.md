# kotlin学习基础（一）（函数定义、val、var、is关键字，$字符串处理,返回类型指定可为空） #

</br>
</br>
## 函数的定义 ##
</br>
函数的主要结构为：

    fun 方法名（参数1：参数类型，参数2：参数类型）：返回数据类型（可不写或者写Unit,这两个基本上和void一个意思，可以没有返回）{
        方法体
    }
    
当然了也有特殊的方法定义方式，就是仅有一条表达式作为方法体，同时其结果做为返回值的函数，其结果为：

    fun 方法名（参数1：参数类型，参数2：参数类型）：参数1 (+-*/) 参数2


函数示例：

    示例1：
    fun sum(a: Int, b: Int): Int {
       return a + b
    }
    输出：
    sum of 3 and 5 is 8
 
    示例2：
    fun printSum(a: Int, b: Int): Unit {
       println("sum of $a and $b is ${a + b}")
    }
    输出：
    sum of -1 and 8 is 7 

    示例3：
    fun printSum(a: Int, b: Int) {
       println("sum of $a and $b is ${a + b}")
    }
    输出：
    sum of -1 and 8 is 7

    示例4：
    fun sum(a: Int, b: Int) = a + b
    输出：
    sum of 19 and 23 is 42

    示例5：
    fun maxOf(a: Int, b: Int) = if (a > b) a else b
    输出：
    max of 0 and 42 is 42
    

</br>
</br>
## val关键字以及var关键字 ##
  
val关键字：仅允许设置一次变量的值，一旦设置无法修改，同java当中的final关键字类似

var关键字：其标记的变量的值可以进行多次改变

    示例1：
    val a: Int = 1  // immediate assignment
    val b = 2   // `Int` type is inferred
    val c: Int  // Type required when no initializer is provided
    c = 3   // deferred assignment
    输出：
    a = 1, b = 2, c = 3

    示例2:
    var x = 5 // `Int` type is inferred
    x += 1
    输出：
    x = 6




</br>
</br>
## 字符串处理 ##

这里的字符串处理比较特殊，如果想要根据一个变量输出不同内容的话按照正常的话是先将字符串传递给一个变量，然后将这个变量使用replace掉字符串的某一个部分然后使用要替换后的变量作为替换结果，这样才能修改，但是kotlin的字符串处理有一点好处就是可以在字符串中直接使用变量，这样会将要动态显示的变量直接显示到字符串当中，但是这个变量要加**$**，否则的话是不会被识别成变量进行替换的。

    示例1：
    fun printProduct(arg1: String, arg2: String) {
       val x = parseInt(arg1)
       val y = parseInt(arg2)

       // Using `x * y` yields error because they may hold nulls.
       if (x != null && y != null) {
           // x and y are automatically cast to non-nullable after null check
           println(x * y)
       }
       else {
           println("either '$arg1' or '$arg2' is not a number")
       }    
    }
    输出：
    42(第一次输入后输出)
    either 'a' or '7' is not a number(第二次输入后输出)
    either 'a' or 'b' is not a number(第三次输入后输出)


</br>
</br>
## is关键字 ##

定义：判断变量类型时候为指定类型
结构：变量名 is 变量类型


    示例1：
    "asdf" is String
    输出：
    true 

    示例2：
    "asdf" !is String
    输出：
    false

</br>
</br>
## 返回类型指定可为空 ##

当运行函数时，其返回值可能是指的返回值，但是也会出现意外情况返回null，但是函数默认是不允许返回null的，所以这个时候就要在返回值类型上加个’？‘号做标记，来标记这个函数的返回值允许为null

    示例1：
    fun parseInt(str: String): Int? {
       return str.toIntOrNull()
    }
    
    fun printProduct(arg1: String, arg2: String) {
       val x = parseInt(arg1)
       val y = parseInt(arg2)
       
       // Using `x * y` yields error because they may hold nulls.
       if (x != null && y != null) {
          // x and y are automatically cast to non-nullable after null check
          println(x * y)
       }
       else {
          println("either '$arg1' or '$arg2' is not a number")
       }
    }
    
    
    fun main() {
       printProduct("6", "7")
       printProduct("a", "7")
       printProduct("a", "b")
    }


    输出：
    42
    either 'a' or '7' is not a number
    either 'a' or 'b' is not a number


