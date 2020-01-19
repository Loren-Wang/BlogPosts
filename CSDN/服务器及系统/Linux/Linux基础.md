1、查看端口使用情况

2、删除文件夹

3、查看服务运行状态

4、查看磁盘使用情况

5、Jenkins启动/重启/停止

6、安装命令

7、刷新系统配置文件

## 1、查看端口使用情况

`netstat -ntlp`

### 2、删除文件夹

`rm -rf 文件夹地址`

### 3、查看服务运行状态

`systemctl status 服务名称`

**常用服务名称如下：**

- firewalld  防火墙

- jenkins  Jenkins自动部署

### 4、查看磁盘使用情况

- 查看磁盘整体使用：`df -hT`；

- 查看当前文件夹使用总量：`du -sh`；

- 查看当前文件夹内各文件或文件夹使用：`du * -sh`或者`ll -lh`（查的更详细）或者`du -h --max-depth=1 /usr/local/`(指定层数返回显示)；

- 查看当前文件夹内各文件或文件夹使用并排序：`du * -sh | sort -hr`；

### 5、Jenkins启动/重启/停止

启动

```shell
service jenkins start
```

重启

```shell
service jenkins restart
```

停止

```shell
service jenkins stop
```

### 6、安装命令

`sudo apt-get install 软件名称`或者`yum install -y 软件名称`

### 7、刷新系统配置文件

`source /etc/profile`
