### cx_Oracle找不到客户端

- 报错信息：
    ```shell
    Traceback (most recent call last):
      File "/home/huzing2524/Desktop/boc/liudeng/linux_monitor/main.py", line 173, in <module>
        connection = cx_Oracle.connect(user='xxx', password='xxx', dsn='127.0.0.1:1521/xxx')
    cx_Oracle.DatabaseError: DPI-1047: Cannot locate a 64-bit Oracle Client library: "libclntsh.so: cannot open shared object file: No such file or directory". See https://oracle.github.io/odpi/doc/installation.html#linux for help
    ```
- 解决办法：
  - Windows：
  
  - Linux：
  