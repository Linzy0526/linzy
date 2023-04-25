# koa-static 静态文件访问

> koa-static 是一个基于 koa 框架的静态文件中间件，它可以帮助我们快速地向客户端提供静态资源，如 HTML、CSS、JavaScript、图片等。在实际开发中，我们通常会将静态文件放在 public 或 static 目录中，并使用 koa-static 中间件将这些文件映射到指定的 URL 上，以供客户端访问。

### 安装依赖

```shell
npm install koa-static
```

### 使用

使用 koa-static 中间件非常简单，我们只需要在 koa 应用中注册该中间件，并指定静态文件的根目录和 URL 前缀即可。

下面是一个例子，我们将 public 目录中的静态文件映射到 /static URL 上：

```javascript
const Koa = require("koa");
const static = require("koa-static");

const app = new Koa();

// 注册静态文件中间件
app.use(static("public", { prefix: "/static" }));

// 处理请求
app.use(async (ctx) => {
  ctx.body = "Hello, World!";
});

// 启动应用
app.listen(3000, () => {
  console.log("Server is running at http://localhost:3000");
});
```

在这个例子中，我们使用 koa-static 中间件将 public 目录映射到 /static URL 上。这意味着，当客户端请求 /static 开头的 URL 时，koa-static 中间件会自动在 public 目录中查找对应的文件，并将其发送给客户端。例如，如果 public 目录中有一个名为 index.html 的文件，那么客户端请求 /static/index.html 时，koa-static 中间件会自动返回该文件。

除了将整个目录映射到 URL 上，我们还可以将指定的文件映射到 URL 上。例如，如果我们只想将 public/index.html 文件映射到 / URL 上，可以使用以下代码：

```
app.use(static('public/index.html'));
```

### 配置

koa-static 中间件还支持一些配置选项，可以通过第二个参数传递给中间件。下面是一些常用的配置选项：

- maxAge：指定缓存的最大时间，单位为毫秒，默认为 0，表示不缓存。
- immutable：指定是否使用 immutable 缓存，该选项仅在 maxAge 大于 0 时生效。
- gzip：指定是否启用 Gzip 压缩，默认为 false。
- hidden：指定是否返回隐藏文件，默认为 true。
- index：指定默认的索引文件，可以是字符串或字符串数组，默认为 index.html。
- setHeaders: 指定自定义响应头。该选项是一个回调函数，用于在发送响应前设置响应头。该回调函数有三个参数：res、path 和 stats，分别表示响应对象、文件路径和文件状态。我们可以在该回调函数中设置自定义的响应头，例如 Content-Type、Cache-Control 等

示例：

```Javascript
const Koa = require('koa');
const static = require('koa-static');
const app = new Koa();

// 配置选项
const options = {
  maxAge: 24 * 60 * 60 * 1000, // 1 天
  immutable: true,
  index: 'index.htm',
  gzip: true,
  setHeaders: (res, path, stats) => {
    res.setHeader('Cache-Control', 'public, max-age=31536000, immutable');
  },
};

// 使用中间件
app.use(static('public', options));

app.listen(3000);
```

通过 maxAge 和 immutable 选项来配置缓存。由于 immutable 选项为 true，因此服务器会在响应头中添加 Cache-Control 字段，用于指定缓存时间。同时，我们在 setHeaders 回调函数中设置了自定义的 Cache-Control 响应头，以覆盖默认的缓存时间。此外，我们还启用了 Gzip 压缩，并将默认索引文件设置为 index.htm。
