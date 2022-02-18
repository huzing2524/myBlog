### 【软件安装】Ubuntu 18.04 安装 psycopg2 报错解决办法
- `pip3 install psycopg2`
- 报错信息：
    ```text
    Collecting psycopg2==2.8.3 (from -r requirements.txt (line 8))
    Using cached https://files.pythonhosted.org/packages/5c/1c/6997288da181277a0c29bc39a5f9143ff20b8c99f2a7d059cfb55163e165/psycopg2-2.8.3.tar.gz
        ERROR: Command errored out with exit status 1:
        command: /home/huzing2524/.virtualenvs/circuit_board/bin/python3 -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-ufsmqwic/psycopg2/setup.py'"'"'; __file__='"'"'/tmp/pip-install-ufsmqwic/psycopg2/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' egg_info --egg-base pip-egg-info
            cwd: /tmp/pip-install-ufsmqwic/psycopg2/
        Complete output (7 lines):
        running egg_info
        creating pip-egg-info/psycopg2.egg-info
        writing pip-egg-info/psycopg2.egg-info/PKG-INFO
        writing dependency_links to pip-egg-info/psycopg2.egg-info/dependency_links.txt
        writing top-level names to pip-egg-info/psycopg2.egg-info/top_level.txt
        writing manifest file 'pip-egg-info/psycopg2.egg-info/SOURCES.txt'
        Error: b'You need to install postgresql-server-dev-X.Y for building a server-side extension or libpq-dev for building a client-side application.\n'
        ----------------------------------------
    ERROR: Command errored out with exit status 1: python setup.py egg_info Check the logs for full command output. 
    ```
- 报错原因：`The libpq-dev library is built following Debian's libpq-dev package_ idea: it contains a minimal set of PostgreSQL_ binaries and headers requried for building 3rd-party applications for PostgreSQL_.`
- 解决办法，在全局安装依赖：`sudo apt install libpq-dev`
- 执行过程：
  ```text
  正在读取软件包列表... 完成
  正在分析软件包的依赖关系树       
  正在读取状态信息... 完成       
  下列软件包是自动安装的并且现在不需要了：
    linux-headers-5.0.0-23 linux-headers-5.0.0-23-generic linux-image-5.0.0-23-generic linux-modules-5.0.0-23-generic linux-modules-extra-5.0.0-23-generic
  使用'sudo apt autoremove'来卸载它(它们)。
  建议安装：
    postgresql-doc-10
  下列【新】软件包将被安装：
    libpq-dev
  升级了 0 个软件包，新安装了 1 个软件包，要卸载 0 个软件包，有 39 个软件包未被升级。
  需要下载 218 kB 的归档。
  解压缩后会消耗 1,092 kB 的额外空间。
  获取:1 http://us.archive.ubuntu.com/ubuntu bionic-updates/main amd64 libpq-dev amd64 10.10-0ubuntu0.18.04.1 [218 kB]
  已下载 218 kB，耗时 2秒 (95.0 kB/s)                   
  正在选中未选择的软件包 libpq-dev。
  (正在读取数据库 ... 系统当前共安装有 207162 个文件和目录。)
  正准备解包 .../libpq-dev_10.10-0ubuntu0.18.04.1_amd64.deb  ...
  正在解包 libpq-dev (10.10-0ubuntu0.18.04.1) ...
  正在设置 libpq-dev (10.10-0ubuntu0.18.04.1) ...
  正在处理用于 man-db (2.8.3-2ubuntu0.1) 的触发器 ...
  ```
- 成功安装：
  ```text
  Collecting psycopg2==2.8.3 (from -r requirements.txt (line 8))
    Using cached https://files.pythonhosted.org/packages/5c/1c/6997288da181277a0c29bc39a5f9143ff20b8c99f2a7d059cfb55163e165/psycopg2-2.8.3.tar.gz
  Building wheels for collected packages: psycopg2
    Building wheel for psycopg2 (setup.py) ... done
    Created wheel for psycopg2: filename=psycopg2-2.8.3-cp36-cp36m-linux_x86_64.whl size=418320 sha256=4b87f24e17156d65c4276f02a01506c3301b7832b28094367c5e85baddf08c2d
    Stored in directory: /home/huzing2524/.cache/pip/wheels/48/06/67/475967017d99b988421b87bf7ee5fad0dad789dc349561786b
  Successfully built psycopg2
  Installing collected packages: psycopg2
  Successfully installed psycopg2-2.8.3
  ```
