## 起因

Kitematic 在我电脑里躺了快一个多月了，再次打开它却出现问题了。报了四种错误。
折腾了一晚上，卸载/重装，重启，开关Hyper-V. 还是没能驯服这只怪。。
突发奇想，是否可以用mongo连接阿里云上的docker中的mongodb数据库？如果这样可以的话，就不用本地安装docker了。

就这个问题我问了下徐老师，徐老师说这个难度不大，只要：
1. 在阿里云的<安全设置>中开启端口 
2. 在服务器上启动mongodb服务

之前我已经在阿里云服务器上布置了一个mongodb，可以直接先用这个测试看看。熟悉了流程在用docker创建mongodb。

## 服务器

很久没有在云服务器上运行Mongodb了，手有些生了，边操作边看指令。
```
//这个命令能开启mongodb的server端。不填写dbpath参数的话，数据库默认路径是/data/db
$ mongod

//简单的mongo，就可以连上本地的默认数据库了。
$ mongo
```
用`sudo lsof -i :27017`查看端口信息。
```
$ sudo lsof -i :27017
COMMAND   PID USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
mongod  16056 root    8u  IPv4 5170918      0t0  TCP *:27017 (LISTEN)
```
嗯，端口已经有服务在跑了。

## 客户端

>这里稍微岔开一下，介绍一下**chocolatey**这个windows端的包管理器。从官网安装了chocolatey之后，可以在命令行上敲入`choco install mongo`就能安装好Mongodb了，在配置下环境变量 ( *将mongodb的安装目录添加到 PATH中* ) 就可以在命令行中使用`mongo`指令。
>
>参考链接：[https://chocolatey.org/packages?q=mongo](https://chocolatey.org/packages?q=mongo)

##### 在cmder终端下敲入 `mongo  47.95.199.219:27017`。问题就出现了。
![](https://images-cdn.shimo.im/UxPQv1Dmq4UpJpvZ/image.png!thumbnail)
与服务器的连接超时。

在网上搜索了半个多小时，却没什么收获。
有一个最接近我的情况的博文，里面提到了修改mongodb的启动参数bind_ip (127.0.0.1 => 0.0.0.0) 本来只能本地访问，修改之后就可以从外网访问了。
学着网上的VI编辑器的基本命令，总算把mongo.conf 的 bind_ip 部分修改好了。
敲入命令`/etc/init.d/mongodb restart` 重启了mongodb。
服务器本机运行也OK。

但是——问题还是没有解决，连接还是超时了。



