###【软件部署】阿里云小程序后台部署 Python Django + uWGSI + Nginx

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
   