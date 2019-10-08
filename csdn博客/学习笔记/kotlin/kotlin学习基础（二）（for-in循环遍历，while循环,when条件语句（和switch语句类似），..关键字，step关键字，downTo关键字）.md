# kotlin学习基础（二）（for-in循环遍历，while循环,when条件语句（和switch语句类似），..关键字，step关键字，downTo关键字）

</br>
</br>
## for-in循环遍历 ##

这个循环遍历的使用方式和java的增强for循环有些类似，不过增强for循环中间是冒号，这个是in罢了

结构定义：for （item元素 in list集合）

    示例1：
    fun main() {
        val items = listOf("apple", "banana", "kiwifruit")
        for (item in items) {
           println(item)
        }
    }
    
    输出1：
    apple
    banana
    kiwifruit
    
    
    示例2：
    fun main() {
       val items = listOf("apple", "banana", "kiwifruit")
       for (index in items.indices) {
            println("item at $index is ${items[index]}")
       }
    }
    
    输出：
    item at 0 is apple
    item at 1 is banana
    item at 2 is kiwifruit

</br>
</br>
## while循环 ##

 while循环基本都差不多，所以这里就不写示例了，直接就写结构

结构定义：while（条件语句）{}

</br>
</br>
### when条件语句（和switch语句类似） ###

这个语句的作用和java的switch语句类似，是可以根据不同条件执行不同方法的语句。

结构定义：

      when（要匹配的条件，可为空，为空时不带括号，条件匹配为条件表达式）{
           条件1  ->   要执行的方法
           条件2  ->   要执行的方法
           else  ->   要执行的方法
      }
    
    
    
    示例：
    fun describe(obj: Any): String =
        when (obj) {
            1          -> "One"
            "Hello"    -> "Greeting"
            is Long    -> "Long"
            !is String -> "Not a string"
            else       -> "Unknown"
    }
    
    fun main() {
        println(describe(1))
        println(describe("Hello"))
        println(describe(1000L))
        println(describe(2))
        println(describe("other"))
    }
    
    输出：
    One
    Greeting
    Long
    Not a string
    Unknown
    
    
    示例：
    fun main() {
        for (x in 1..5) {
            print(x)
        }
    }
    
    输出：
    12345

</br>
</br>
## step关键字 ##
间隔关键字，一般和**"..关键字"**使用，用来跳过指定的间隔，例如：1..10 step 2就代表着一到十的数字当中从一开始每隔2是一个元素。

    示例：
    fun main() {
        for (x in 1..10 step 2) {
            print(x)
        }
    }
    
    输出：
    13579

</br>
</br>
## downTo关键字 ##
和**“..关键字”**正相反，他是倒序生成数组集合，前面的数字要比后面大。

    示例：
    fun main() {
        for (x in 9 downTo 0 step 3) {
            print(x)
        }
    }
    
    输出：
    9630
