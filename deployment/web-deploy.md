# 前端web项目部署

第三方托管服务、OSS静态网站托管、服务器部署。

#### 第三方托管服务

https://vercel.com/

https://www.netlify.com/

#### OSS静态网站托管

阿里云oss

https://help.aliyun.com/document_detail/68218.html

#### 阿里云服务器部署

##### 一、通过ssh远程服务器

1、复制本地电脑的ssh公钥

```shell
pbcopy < ~/.ssh/id_rsa.pub
```

2、讲本地电脑的ssh公钥写入服务器

写入文件：/home/admin/.ssh/authorized_keys

3、测试ssh登录服务器

```
ssh admin@39.108.57.24
```

##### 二、复制前端打包后的文件到服务器

```
scp -r ./build  admin@39.108.57.24:/home/admin/
```

##### 三、配置nginx

1、在nginx服务的配置目录下/etc/nginx/sites-available添加my-site.conf文件

server_name为域名配置，需要到域名服务商进行域名DNS解析配置

```
server {
    listen 80;
    server_name insseek.com;

    root /home/admin/leetan-home/build;
    index index.html;

    location / {
        try_files $uri /index.html;
    }
}
```

2、重启nginx

```
systemctl reload nginx
systemctl restart nginx
```

3、尝试访问

https://insseek.com/