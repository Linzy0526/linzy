# 项目端口被代理无法获取真实IP

> 如果你的项目通过 Nginx 进行代理，并且无法获取真实的客户端 IP，可能是因为 Nginx 默认会将代理请求的 IP 地址传递给后端服务器，而不是客户端的真实 IP。

### Nginx 配置调整 

##### Nginx 配置中的 location 块中添加 proxy_set_header 指令，将客户端的真实 IP 传递给后端服务器。

``` 
location / {
    proxy_pass http://backend_server;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    # 其他配置项...
}
```

以上配置中的 `X-Real-IP` 和 `X-Forwarded-For` 是用来传递客户端的真实 IP 的头部字段。      `$remote_addr` 代表客户端的 IP 地址，`$proxy_add_x_forwarded_for` 用于追加代理服务器的 IP 到头部字段中，以防有多级代理

##### 重启Nginx服务器

```
nginx -s reload 
```


### 在Koa项目中怎么获取到真实地址

``` js 
app.use(async (ctx, next) => {
  const realIP = ctx.request.headers['x-real-ip'] || ctx.request.headers['x-forwarded-for'] || ctx.request.ip;
  // 使用 realIP 进行处理...
  await next();
});
```

首先尝试从 X-Real-IP 头部字段中获取真实的 IP 地址，如果不存在则尝试从 X-Forwarded-For 头部字段中获取，最后备选方案是使用 ctx.request.ip 获取 Koa 默认的 IP 地址。