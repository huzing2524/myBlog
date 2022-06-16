官方文档及下载地址：
https://www.oracle.com/cn/database/technologies/instant-client/linux-x86-64-downloads.html
https://www.oracle.com/cn/database/technology/linuxx86-64soft.html

- 背景：
  - Python使用cx_Oracle直接连接Oracle数据库时会报错如下：
  - cx_Oracle.DatabaseError: DPI-1047: Cannot locate a 64-bit Oracle Client library: "libclntsh.so: cannot open shared object file: No such file or directory".
  - cx_Oracle.DatabaseError: DPI-1047: Cannot locate a 64-bit Oracle Client library: "libnnz19.so: cannot open shared object file: No such file or directory".

- 安装环境：
  - 操作系统：Red Hat Enterprise Linux Server release 7.6 (Maipo)
  - 数据库：Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production Version 19.6.0.0.0
  - Python 3.7.7
  - Instant Client客户端与服务端版本兼容关系

- 安装过程：
  - `sudo mkdir /opt/oracle`
  - `cd /opt/oracle`
  - `unzip instantclient-basic-linux.x64-19.15.0.0.0dbru-2.zip`
  - `unzip instantclient-sqlplus-linux.x64-19.15.0.0.0dbru-2.zip`
  - 安装 libaio 软件包：
    - 查看是否已经安装：`yum list installed | grep libaio`
    - 如果未安装，执行命令：`rpm -ivh libaio-0.3.109-13.el7.x86_64.rpm`，或者 `sudo yum install libaio`
  - vim ~/.bash_profile，编辑输入以下内容：
    - export LD_LIBRARY_PATH=/opt/oracle/instantclient_19_15:$LD_LIBRARY_PATH
    - export PATH=/opt/oracle/instantclient_19_15:$PATH
    - 保存退出文件，执行命令：`source ~/.bash_profile`

- 测试连接Oracle数据库：
  ```python
  # -*- coding: utf-8 -*-

  import cx_Oracle

  # 不知道为什么不起作用，不配置.bash_profile文件，只指定instantclient_19_15的文件夹路径
  # cx_Oracle.init_oracle_client(lib_dir='/home/instantclient_19_15')

  # 方法一
  connection = cx_Oracle.connect(user='数据库用户名', password='数据库密码', dsn='ip地址:端口号/数据库名称', encoding='UTF-8')
  # 方法二
  connection = cx_Oracle.connect('数据库用户名/数据库密码@ip地址:端口号/数据库名称')
  # cursor = connection.cursor()
  # cursor.execute('')

  print(connection.version)
  ```
  
  输出以下结果说明数据库连接成功：
  ```bash
  19.6.0.0.0
  ```
