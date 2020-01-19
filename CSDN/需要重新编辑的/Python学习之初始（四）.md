**字符串**



- Python中的字符串是使用单引号或者双引号来标记的；
- 首字母大写方法："abc def".title();
- 字符串全部大写："abc def".upper();
- 字符串全部小写："abc def".lower();
- 字符串拼接　　：使用**‘+’**号进行拼接
- 添加制表符**"\t"**：print("\tPython")，用来在输出钱有两个汉字字符的空格
- 添加换行符**"\n"**：print("Python\n\tPython")
- 字符串的内容单双引号使用要注意不要和Python本身对于字符串的定义产生冲突



**移除字符串中开头或者结尾的字符或者空白**

　　首先这里说的移除仅仅只是开头或者结尾部分的，不包括字符串除了开头和结尾其他部分的字符或者空白。

　　这里用到了字符串的三个方法：**rstrip();lstrip();strip();**，这三个方法从头至尾依次是**-移除尾部的空白-移除首部的空白-移除首尾的空白**，当然了这是不带参数下的情况，也就是默认是移除空白的，我们也可以在方法中添加参数来进行移除首尾相应的字符，参数可以是一个字符串，这三个方法会把参字符串首尾中和参数中的每个字符相同的字符依次从字符串的首尾向前或者向后依次移除，直到出现不同字符为止！


    # 移除首部空白
    print("  Hello World ".lstrip())
    # 移除尾部空白
    print("  Hello World ".rstrip())
    # 移除两侧空白
    print("  Hello World ".strip())
    # 移除首部H
    print("HHHHello World ".lstrip("H"))
    # 移除尾部d
    print("Hello Worlddd".rstrip("d"))
    # 移除两侧Hd
    print("HHHHello Worldddd".strip("Hd"))
    # 移除首部h
    print("HHHHello World ".lstrip("h"))
    # 移除尾部D
    print("Hello Worlddd".rstrip("D"))
    # 移除两侧hD
    print("HHHHello Worldddd".strip("hD"))


**数字**
　　

　　Python中对于数字的运算和其他语言没什么两样，只不过有几点需要注意，一个就是数字的幂次运算使用的符号是‘**’符号进行运算的，另一个就是浮点数运算的结果的小数位数是不可控的，还有一个是整数不可以直接和字符串进行拼接，需要使用str()；方法将整数转换成字符串再进行拼接！
