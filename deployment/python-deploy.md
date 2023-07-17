# Python项目部署命令示例

#### 部署方式

Supervisor+Gunicorn+Nginx+PostgreSQL

#### 项目目录及虚拟环境

```shell
mkdir yitian-backend
cd yitian-backend/
mkdir sys_logs
virtualenv venv -p python3.5
```

#### 创建数据库

```shell
进入控制台：sudo -u postgres psql
创建数据库：CREATE DATABASE yitian OWNER postgres
权限赋予：GRANT ALL PRIVILEGES ON DATABASE yitian to postgres
```

#### Superviser配置

```ini
cd /etc/supervisor/conf.d
touch yitian.conf 

[program:yitian-backend]
command=/home/deployer/yitian-backend/venv/bin/gunicorn -w 2 -b 127.0.0.1:8182 --timeout 128 -e PROD=1 -e STAGING=1 -e NODE_PATH=/home/deployer/yitian-backend yitian.wsgi:application
directory=/home/deployer/yitian-backend/current
user=deployer
autostart=true
autorestart=true
stdout_logfile=/home/deployer/supervisor_logs/yitian-backend.log
stderr_logfile=/home/deployer/supervisor_logs/yitian-backend.err
environment=DJANGO_SETTINGS_MODULE="yitian.my_settings.staging_settings",PYTHONIOENCODING="UTF-8"
```

#### Nginx配置

目录：/etc/nginx/sites-enabled

```ini
upstream yitian_backend_server {
    server 127.0.0.1:8182;
}
server {
    server_name demo.tian.chilunyc.com;
    client_max_body_size 4G;

    keepalive_timeout 75s;
    proxy_connect_timeout 75s;
    proxy_read_timeout 128s;

    set $deploy_path /home/deployer/yitian-frontend/current/build;
    if ($host !~ ^(demo.tian.chilunyc.com|tian.chilunyc.com|127.0.0.1|localhost)$ ) {
        return 444;
    }
    location /frontend_static {
        alias /home/deployer/yitian-frontend/current/build/frontend_static/;
    }
    location   ~* ^/api(.*)$ {
        try_files $uri @proxy_to_app;
    }
    location / {
        root  $deploy_path;
        try_files $uri /index.html;
    }
    location @proxy_to_app {
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Host $http_host;
      proxy_redirect off;
      proxy_pass http://yitian_backend_server;
    }
    listen 80;

    ssl_stapling on;
    ssl_stapling_verify on;
}

sudo ln -s /etc/nginx/sites-available/yitian.conf yitian.conf
```