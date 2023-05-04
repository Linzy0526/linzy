# 2023 年 05 月 04 日

> weather: ☀️
>
> emoji: 😘

### 问题记录

#### linux 服务器 node 无法正常读取配置文件.env 文件

需要考虑文件可读权限

```
// 粗暴方式
chmod 777 .env
```

### 学习记录

#### Nginx SSL 证书部署及配置

1. 获取 SSL 证书，可以使用 Certbot 自动获得免费的 Let's Encrypt SSL 证书

2. 将 SSL 证书上传到服务器，将 SSL 证书上传到服务器上，并将其放置在安全的位置。一般情况下，证书将被放置在/etc/ssl/certs 和/etc/ssl/private 目录中。确保证书文件和密钥文件的所有权和权限设置正确。

3. 配置 Nginx 使用 SSL 证书，打开 Nginx 配置文件，并添加以下代码块，将 SSL 证书文件路径和密钥文件路径替换为正确的路径：

```
server {
    listen 443 ssl;
    server_name example.com;

    ssl_certificate /etc/ssl/certs/example.com.crt;
    ssl_certificate_key /etc/ssl/private/example.com.key;

    # Other Nginx configuration directives for the server...
}

```

注：如果配置了 80 端口的 server_name 需要与 443 端口一致用域名

4. 重启 Nginx

```
sudo systemctl restart nginx
```
