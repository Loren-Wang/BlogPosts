# could not extract ResultSet

## 状况1、执行的是增删改sql语句

该情况下说明缺少了@modify（org.springframework.data.jpa.repository.Modifying）注解，该注解包含了增删改的权限，如果不加该注解，那么增删改操作的sql语句是无法执行的，同样还要加@Transactional（javax.transaction.Transactional）注解，否则会报另外一个错误，因为在jpa要求，’没有事务支持，不能执行增删改操作’。

参考：[https://blog.csdn.net/moshowgame/article/details/80090453](https://blog.csdn.net/moshowgame/article/details/80090453)

## 状况2、sql语句出错

这样就仔细检查sql语句，在控制台运行一遍再加到@query当中
