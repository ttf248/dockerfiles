### 参考

[dockerfile来自这里](https://github.com/woqutech/docker-images/tree/master/Oracle/11.2.0.4), 下面是它的说明:

> - 指定是否开启归档
> - 指定SGA及PGA大小(官方image指定的是固定的内存大小，如需修改，需要在数据库创建之后手动调整，所以，在此我们做了相应的自动化)
> - 指定数据库角色，包括primary及standby(官方镜像只能创建primary数据库，我们同时实现了创建standby数据库的逻辑，但该部分逻辑依赖沃趣科技QCFS云存储提供的快照功能，目前只能在QFusion 3.0 RDS数据库云平台中实现)
> - 包含对主库实例状态、备库实例状态和MRP恢复状态的健康检查
> - ONLINE REDO LOG自动调整为1G大小
> - 设置用户名密码永不过期(虽不安全，但在绝大部分企业级用户均采用此实践)
> - 关闭Concurrent Statistics Gathering功能
> - TEMP表空间设置为30G大小
> - SYSTEM表空间设置为1G大小
> - SYSAUX表空间设置为1G大小
> - UNDO表空间设置为10G大小

### Image构建

```
1. 下载本目录的所有文件
2. 下载11.2.0.4 Patchset：p13390677_112040_Linux-x86-64_1of7.zip p13390677_112040_Linux-x86-64_2of7.zip
3. 执行构建命令：
	docker build -t  xiangtl/oracle:server .
```

### Image使用举例

```
docker run -d --name oracledb \
--volumes-from oradata \
-p 1521:1521 \
-e ORACLE_SID=HS2015 \
-e ORACLE_PWD=handsome \
-e ORACLE_CHARACTERSET=ZHS16GBK \
-e SGA_SIZE=1G \
-e PGA_SIZE=1G \
-e DB_ROLE=primary \
-e ENABLE_ARCH=false \
xiangtl/oracle:server

docker run -it -v /opt/oracle/oradata -d  --name oradata.spx centos:7
docker run -it -v /opt/oracle/oradata -d  --name oradata.sp2 centos:7

docker run --volumes-from oradata.spx -v$(pwd):/backup xiangtl/centos:dev tar -jcvf /backup/oradata.spx.tar.bz2 /opt/oracle/oradata
docker run --volumes-from oradata.spx -v$(pwd):/backup xiangtl/centos:dev tar -jxvf /backup/oradata.spx.tar.bz2

docker run -d -P \
-e ORACLE_SID=HS2015 \
-e ORACLE_PWD=handsome \
-e ORACLE_CHARACTERSET=ZHS16GBK \
-e SGA_SIZE=1G \
-e PGA_SIZE=1G \
-e DB_ROLE=primary \
-e ENABLE_ARCH=false \
xiangtl/oracle:server

docker run -d -P \
-e ORACLE_SID=HS2015 \
-e ORACLE_PWD=handsome \
-e ORACLE_CHARACTERSET=ZHS16GBK \
-e SGA_SIZE=1G \
-e PGA_SIZE=1G \
-e DB_ROLE=primary \
-e ENABLE_ARCH=false \
-v /disk02/dbdata:/opt/oracle/oradata \
xiangtl/oracle:server.dev

docker run -d \
-p 8923:1521 \
-e ORACLE_SID=GFDB \
-e ORACLE_PWD=handsome \
-e ORACLE_CHARACTERSET=ZHS16GBK \
-e SGA_SIZE=8G \
-e PGA_SIZE=8G \
-e DB_ROLE=primary \
-e ENABLE_ARCH=false \
-v /disk01/dbdata:/opt/oracle/oradata \
xiangtl/oracle:server

如何恢复广发数据库，同名实例先创建数据库，然后移除数据配置文件和数据文件，使用原有的数据文件直接替换，已经参考之前的数据库做过启动参数配置文件的修改
重建数据文件路径，initGFDB.ora /disk01/oradata/GFDB /disk01/oradata/HOTFA 新建三个软连接

已经执行过数据导出和导入，无须再次建立软连接，直接使用当前镜像启动即可

```
