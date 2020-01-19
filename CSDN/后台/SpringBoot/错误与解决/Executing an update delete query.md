# Executing an update/delete query

## 状况1、执行的是sql新增插入语句

问题原因：没有加事务注解

问题解决：因为jpa要求，’没有事务支持，不能执行增删改操作’，所以在语句上加入@Transactional（javax.transaction.Transactional）注解

参考：[https://blog.csdn.net/moshowgame/article/details/80090453](https://blog.csdn.net/moshowgame/article/details/80090453)
