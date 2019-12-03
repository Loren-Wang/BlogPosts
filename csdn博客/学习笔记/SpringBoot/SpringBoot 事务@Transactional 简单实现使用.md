&nbsp;&nbsp;搞这个东西搞了两天都没搞明白，感觉都快愁死了，网上的文章一水的说要弄什么xml文件，关键是还不说放在哪里？蓝瘦香菇！！！！！！！！！

&nbsp;&nbsp;这个东西的生效是有条件的，你在后台程序初始化的时候直接使用该注解是无效的，没有任何卵用，这个东西需要结合**@Component、@Repository、@Service、@Controller**这四个注解相关的类才会生效，否则任你怎么操作也是没用了。

***ps：***要注意一点的是不要想着用代码去实现事务的实现，没卵用，我实现了两天的时间，结果还是没有成功，所以大家就不要浪费功夫了。





&nbsp;&nbsp;下面我贴下我实现的代码，很简单，但是还是花了好长时间去实现：

```kotlin
import org.springframework.context.ConfigurableApplicationContext
import org.springframework.stereotype.Service
import org.springframework.transaction.annotation.Transactional
import org.springframework.transaction.interceptor.TransactionAspectSupport

/**
 * 功能作用：数据库表版本控制操作类
 * 创建时间：2019-10-10 下午 15:16:31
 * 创建人：王亮（Loren wang）
 * 思路：
 * 方法：
 * 注意：
 * 修改人：
 * 修改时间：
 * 备注：
 */
@Service
open class DataBaseTableVersionUpdateOptions {
    //数据库表版本表操作实例
    private lateinit var databaseTableVersionRepository: DatabaseTableVersionRepository;

    @Transactional
    fun initData(applicationContext: ConfigurableApplicationContext) {
        try {
            LogUtils.instance.logI(javaClass, "开始执行版本${Setting.DATABASE_TABLE_VERSIONNAME}的更新")
            //获取数据库表版本表操作实例
            databaseTableVersionRepository = applicationContext.getBean(DatabaseTableVersionRepository::class.java)
            val tableVersion = databaseTableVersionRepository.findDatabaseTableVersionTbByVersionCodeAndVersionName(Setting.DATABASE_TABLE_VERSIONCODE!!, Setting.DATABASE_TABLE_VERSIONNAME!!)
            if (tableVersion == null) {
                when (Setting.DATABASE_TABLE_VERSIONCODE) {
                    100L -> {

                    }
                    else -> {

                    }
                }

                //保存版本信息
                val databaseTableVersionTb = DatabaseTableVersionTb()
                databaseTableVersionTb.versionName = Setting.DATABASE_TABLE_VERSIONNAME
                databaseTableVersionTb.versionCode = Setting.DATABASE_TABLE_VERSIONCODE
                databaseTableVersionRepository.save(databaseTableVersionTb)
            }
        } catch (e: Exception) {
            LogUtils.instance.logE(javaClass, "更新发生异常，手动执行异常回滚")
            TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();
        }
    }
}
```




