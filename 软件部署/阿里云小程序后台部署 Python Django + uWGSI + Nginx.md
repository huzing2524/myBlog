### 【软件部署】阿里云小程序后台部署 Python Django + uWGSI + Nginx


1. 使用 uWSGI 运行Python Django 后台, Nginx 转发到 uWSGI。
2. uWSGI 配置，我使用的 ini 文件。 运行命令: uwsgi --ini uwsgi.ini
    ```ini
    [uwsgi]
    # Nginx 转发到 uWSGI 使用 socket，直接使用 uWSGI 当做 web 后台使用 HTTP
    socket=127.0.0.1:8000
    # 项目目录，使用绝对路径
    chdir=/root/projects/your_project
    # 项目中wsgi.py文件的目录，相对于项目目录，使用相对路径
    wsgi-file=your_project/wsgi.py
    # 进程数
    processes=1
    # 线程数
    threads=2
    # uwsgi服务器的角色
    master=True
    pidfile=/root/projects/your_project/logs/uwsgi.pid
    # 指定依赖的虚拟环境
    virtualenv=/root/.virtualenvs/tenement
    # 后台守护模式运行
    daemonize=/root/projects/your_project/logs/uwsgi.log
    ```
3. 配置 Nginx。 我在阿里云上安装 Nginx 后，总配置文件 `nginx.conf` 是在 `/etc/nginx/` 的目录下。 Nginx 重新加载配置：`nginx -s reload`
    ```text
    # nginx.conf 里面这两句，说明 /conf.d/ 文件夹下所有的自定义配置文件都会被识别包含在总配置中
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*; 
    ```
   所以我在 `/etc/nginx/conf.d/` 目录中新建了一个文件 `ssl.conf`
   ```text
   server {
       # 微信小程序必须使用HTTPS，443端口。HTTP是80端口。
       listen 443;
       # 自己买的域名，需要通过备案。
       server_name yoursite.com;
       ssl on;
       root html;
       index index.html index.htm;
       # HTTPS必须使用SSL证书加密，把两个文件放在这里。
       ssl_certificate  /etc/nginx/ssl/your_ssl.pem;
       ssl_certificate_key  /etc/nginx/ssl/your_ssl.key;
       ssl_session_timeout 5m;
       ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
       ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
       ssl_prefer_server_ciphers on;
       location / {
           root   /usr/share/nginx/html;
           index  index.html index.htm;
           # 固定写法，Nginx 转发到 uWSGI 方式。
           include uwsgi_params;
           # uWSGI 后台运行的 域名:端口
           uwsgi_pass 127.0.0.1:8000;
           uwsgi_read_timeout 2;
       }
   }
   ```
4. 然后试一下接口，正常来说应该是这样：

    ![1](https://user-images.githubusercontent.com/39394831/154393210-1defea5c-c974-469c-9a22-17e968349398.png)
5. 记录一下另外一个问题：
    - `Fatal Python error: Py_Initialize: Unable to get the local encoding
ModuleNotFoundError: No module named 'encodings'`
    - 搬瓦工VPS服务器上uWSGI不能正常启动，阿里云ECS服务器上正常启动：这是因为搬瓦工服务器上没有默认安装 apt install python-dev 这个包的原因，不是uWSGI配置文件虚拟环境路径没有正常生效的原因！参考 [stack overflow: python-dev](https://link.zhihu.com/?target=https%3A//stackoverflow.com/questions/16272542/uwsgi-fails-with-no-module-named-encoding-error)。
    - 另外一种办法就是删除当前的虚拟环境，然后重新安装虚拟环境和依赖包。
    
    ![2](https://user-images.githubusercontent.com/39394831/154393941-0f210e57-2540-4819-8829-10c58b2ebc3e.jpg)
6. Nginx启动的问题：
    ```bash
    root@bwg:/etc/nginx# nginx -t -c ./nginx.conf 
    nginx: [emerg] open() "/usr/share/nginx/./nginx.conf" failed (2: No such file or directory)
    nginx: configuration file /usr/share/nginx/./nginx.conf test failed
    ```
    这是Nginx服务没有正常启动的原因，service nginx start 之后 nginx -t -c ./nginx.conf 一切正常。
    
    ![3](https://user-images.githubusercontent.com/39394831/154394177-72629e9a-56f6-4aba-9b75-da8322714943.jpg)

   
