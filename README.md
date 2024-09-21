演示视频：https://github.com/swvincent8498/ka-whatsapp/raw/b9c8813eafe06e632a322fae67f1667e7b99d3b2/9-15.mp4

# WhatsApp养号辅助工具安装部署教程

## 1.环境变量约定
![image](https://github.com/user-attachments/assets/2d54614d-a113-4cf9-81ae-667ce17976b4)

为了方便以下安装部署说明，暂定一些固定设置信息，实际操作根据实际情况进行替换即可。
- 1.1.网段
192.168.71.0/24
- 1.2.Docker服务器信息
Linux 系统 Debian或Ubuntu
192.168.71.3
提供给docker实例访问的目录统一放在/docker/****
网卡名称eth0
- 1.3.养号辅助工具控制端IP和端口
Windows系统，有4核8线程，16G内存即可，更高配置更佳
192.168.71.2
会开启6101端口供给chrome浏览器实例连接
- 1.4.MariaDB数据库信息
```
MYSQL_DB_NAME=chat
MYSQL_IP=192.168.71.4
MYSQL_PORT=3301
MYSQL_USER=root
MYSQL_PWD=123456
```
## 2.docker服务器
### 2.1.桥接网络安装
输入以下命令行：
```
命令1：docker network create -d macvlan --subnet 192.168.71.0/24 --gateway 192.168.71.1 -o parent=eth0 mymacvlan
命令2：ip link set eth0 promisc on
#eth0根据实际情况更改
```
### 2.2.Chrome浏览器docker镜像加载
- 1、将“whatsapp_chrome_v0.3.rar”文件解压出来“whatsapp_chrome_v0.3.tar”，ftp到/opt/whatsapp_chrome_v0.3.tar
- 2、命令：docker load -i /opt/whatsapp_chrome_v0.3.tar
- 3、![image](https://github.com/user-attachments/assets/2a11c086-1149-4a11-90ef-87ea16d95736)

### 2.3.Chrome浏览器实例创建
- 1、创建目录，命令：mkdir -p /docker/chrome 
- 2、解压“client.rar”，解压出“client”目录，将它ftp到/docker/chrome上面，ftp后完整路径：/docker/chrome/client
- 3、![image](https://github.com/user-attachments/assets/9633fc47-1538-4708-a807-f7eacaf2f7c3)

- 4、对“config.properties”进行编辑，![image](https://github.com/user-attachments/assets/144163f9-3843-4477-b016-1609f8073192)

- 5、命令：chmod 777 -R /docker 
- 6、命令：docker run -d --privileged --name wsClient001 --restart=always --net mymacvlan --ip 192.168.71.20 --shm-size='1g' -e VNC_NO_PASSWORD=1 -v /docker/chrome:/data -e SE_OPTS='--session-timeout 86400' -e TZ='Europe/London' whatsapp/chrome:v0.3 
- 7、wsClient001需要修改而且唯一，192.168.71.20根据实际修改，shm-size='1g'实例内存，最好设置1.2g，根据硬件实际情况
### 2.4.MariaDB数据库部署
- 1、创建目录，命令：mkdir -p /docker/mariadb11.5/data
- 2、命令：chmod 777 -R /docker 
- 3、创建数据库实例，命令：docker run --name mariadb11.5 -p 3301:3306 -e MYSQL_ROOT_PASSWORD=123456 -e TZ="Asia/Shanghai" -v /docker/mariadb11.5/data:/var/lib/mysql -d mariadb:noble 
- 4、3301端口，123456密码，自行修改
- 5、导入数据库脚本，用数据库操作工具“Navicat for MySQL”其它公司工具也可以，只要能导入sql脚本就可以；
- 6、数据库操作工具连接上数据库，使用chat20240921.sql脚本进行导入，脚本会创建一个chat的库，然后再创建所需的数据库对象和数据
## 3.养号辅助工具控制端
### 3.1.配置
- 1、将“keepAccountServer.rar”解压出来![image](https://github.com/user-attachments/assets/9eaaacf7-5486-4f60-a784-f15c6317957a)

- 2、解压后打开“conf”目录，![image](https://github.com/user-attachments/assets/a992e13d-24cf-4949-bf68-c020bd9a1e75)

- 3、打开“config.properties”配置文件，![image](https://github.com/user-attachments/assets/801079a7-992c-48db-b875-9a37ec904305)


### 3.2.使用
3.2.1.WhatsApp账号导入
![image](https://github.com/user-attachments/assets/33469179-abfc-49b2-88f0-668f375a83b6)

1、编辑“账号模板.xlsx”账号信息进行导入，导入时会覆盖原有账号信息

```





```















