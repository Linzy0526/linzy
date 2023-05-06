# 2023 年 05 月 06 日

> weather: 🌧️
>
> emoji: 😀

### 问题记录

#### 1.koa routing-controller 无法拦截全局的异常

由于 routing-controller useKoaServer 方法内部对全局默认内部做了 try catch 操作，官方提示需要将添加配置参数 `defaultErrorHandler: false`，该属性主要功能为禁用库的默认错误处理程序

之后需要在该中间 useKoaServer 之前插入全局拦截异常中间件

```js
koa.use(async (ctx: Koa.Context, next: Koa.Next) => {
  try {
    await next();
  } catch (err) {
    if (err.constructor.name === "UnauthorizedError") {
      ctx.status = 401;
      ctx.body = { errcode: 10401, message: "用户权限失效~" };
      return;
    }
    logger.error(`${ctx.method} ${ctx.originalUrl}`, { err });
    ctx.status = err.httpCode || 500;
    ctx.body = { errcode: 10500, message: "系统繁忙" };
  }
});

const app: Koa = useKoaServer < Koa > (koa, routingConfigs);
```

遗留问题：catch 的 err 无法判断是否为 routing-controller 内暴露出的一些寄生 Error 类，如无法用 `err instarnceof UnauthorizedError`,暂时只能判断构造函数名是否相同

### 学习记录

#### 1.linux 命令—进程

[linux 命令-进程.md](https://github.com/Linzy0526/linzy/tree/master/code-record/linux/linux命令-进程.md)
