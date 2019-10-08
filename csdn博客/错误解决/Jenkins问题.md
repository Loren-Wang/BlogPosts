1、Jenkins在运行但是却无法访问

2、Jenkins在部署jar包时构建结束进程被杀



### 1、Jenkins在运行但是却无法访问

&nbsp;&nbsp;确认/var/cache/jenkins /var/log/jenkins这两个目录是否存在，不存在则创建。

### 2、Jenkins在部署jar包时构建结束进程被杀

参考：https://blog.51cto.com/jdcdyjy/1712274

```
#!/bin/bash -ilex
BUILD_ID=DONTKILLME

rm -rf "$WORKSPACE"/src/test/
gradle build
general="$(find $WORKSPACE/build/libs/ -name '*.jar')"
cp "$general" /opt/*********/MomentsOfLifeForService.jar

pid="$(netstat -lnp|grep 8888 | awk '{print $7}')"
if [ "$pid" !="" ];then
  kill -9 "${pid%%/*}"
fi    
nohup java -jar /opt/*********/MomentsOfLifeForService.jar &
```
