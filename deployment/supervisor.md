# Supervisor: A Process Control System

文档：http://supervisord.org/

#### supervisord

supervisord 的配置文件默认位于 `/etc/supervisord.conf`

```ini
; supervisor config file

[unix_http_server]
; (the path to the socket file) UNIX socket 文件，supervisorctl 会使用
file=/var/run/supervisor.sock   
; sockef file mode (default 0700) socket 文件的 mode，默认是 0700
chmod=0700        

[supervisord]
logfile=/var/log/supervisor/supervisord.log ; (main log file;default $CWD/supervisord.log) 日志文件，默认是 $CWD/supervisord.log
pidfile=/var/run/supervisord.pid ; (supervisord pidfile;default supervisord.pid) pid 文件
childlogdir=/var/log/supervisor            ; ('AUTO' child log dir, default $TEMP)

; the below section must remain in the config file for RPC
; (supervisorctl/web interface) to work, additional interfaces may be
; added by defining them in separate rpcinterface: sections
[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

[supervisorctl]
serverurl=unix:///var/run/supervisor.sock ; use a unix:// URL  for a unix socket 通过 UNIX socket 连接 supervisord，路径与 unix_http_server 部分的 file 一致

; 在增添需要管理的进程的配置文件时，推荐写到 `/etc/supervisor/conf.d/` 目录下，所以 `include` 项，就需要像如下配置。
; 包含其他的配置文件
[include]
files = /etc/supervisor/conf.d/*.conf ; 引入 `/etc/supervisor/conf.d/` 下的 `.conf` 文件
```

#### program 配置

program 的配置文件就写在，supervisord 配置中 `include` 项的路径下：`/etc/supervisor/conf.d/`，然后 program 的配置文件命名规则推荐：app_name.conf

下面是supervisor管理django及celery的配置示例

```ini
[program:gearwechat]
command=/home/deployer/gearwechat/venv/bin/gunicorn -w 2 -b 127.0.0.1:8082 -e PROD=1 -e STAGING=1 gearwechat.wsgi:application
directory=/home/deployer/gearwechat/current
user=deployer
autostart=true
autorestart=true
stdout_logfile=/home/deployer/supervisor_logs/gearwechat.log
stderr_logfile=/home/deployer/supervisor_logs/gearwechat.err
environment=DJANGO_SETTINGS_MODULE="gearwechat.my_settings.staging_settings",PYTHONIOENCODING="UTF-8"
```

```ini
; ==================================
;  celery worker supervisor example
; ==================================

[program:farm_celery_beat]
; Set full path to celery program if using virtualenv
command=/home/deployer/farm/venv/bin/celery beat -A gearfarm --loglevel=INFO

; Alternatively,
;command=celery --app=your_app.celery:app worker --loglevel=INFO -n worker.%%h
; Or run a script
;command=celery.sh

directory=/home/deployer/farm/current
user=deployer
numprocs=1
stdout_logfile=/home/deployer/supervisor_logs/farm_celery_beat.log
stderr_logfile=/home/deployer/supervisor_logs/farm_celery_beat.err
autostart=true
autorestart=true
startsecs=10
environment=DJANGO_SETTINGS_MODULE="gearfarm.my_settings.staging_settings",PYTHONIOENCODING="UTF-8"
; Need to wait for currently executing tasks to finish at shutdown.
; Increase this if you have very long running tasks.
stopwaitsecs = 600

; Causes supervisor to send the termination signal (SIGTERM) to the whole process group.
stopasgroup=true

; Set Celery priority higher than default (999)
; so, if rabbitmq is supervised, it will start first.
priority=999
```

### supervisorctl 操作

supervisorctl 是 supervisord 的命令行客户端工具，使用的配置和 supervisord 一样，这里就不再说了。下面，主要介绍 supervisorctl 操作的常用命令：

输入命令 `supervisorctl` 进入 supervisorctl 的 shell 交互界面（还是纯命令行😓），就可以在下面输入命令了。：

- help # 查看帮助
- status # 查看程序状态
- stop program_name # 关闭 指定的程序
- start program_name # 启动 指定的程序
- restart program_name # 重启 指定的程序
- tail -f program_name # 查看 该程序的日志
- update # 重启配置文件修改过的程序（修改了配置，通过这个命令加载新的配置)

也可以直接通过 shell 命令操作：

- supervisorctl status
- supervisorctl update
- ...