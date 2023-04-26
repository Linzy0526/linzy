# 2023 年 04 月 26 日

> weather: ☀️
>
> emoji: 😀

### 问题记录

1. Prisma 中事务回滚无法正常回滚

Prisma 事务用 prisma.$transcation 去实现

```typescript
const result = await prisma.$transcation(async () => {
  const user = await prisma.user.update();

  if (user.xx) {
    throw new Error("xx");
  }

  await prisma.task.update(); // 抛出异常
});
```

上诉代码理论上在抛出异常或者下面的 task 更新抛出异常事务都会回滚，不在更新 user 表数据。但是貌似这种方法不行！

解决：

- 升级 prisma 版本，4.6.1 据说不可靠（升级了也没效果- -）
- 直接用sql语句去创建事务
- 更换低版本的写法

```typescript
const result = await prisma.$transcation([
  prisma.user.update(),
  prisma.task.update(),
]);
```

这种写法可以实现回滚，但是不能对做额外的业务逻辑处理，主动抛出异常去中断事务

2. 奇葩的资源连接异常

在开发过程中经常应用到一些静态文件的访问，今日突然碰到一个连接无法访问的问题。

排查：

- 资源链接 404，复制链接到浏览器可以正常访问
- 链接链接 OSS 内部参数配置，做同等参数对比，无果
- 开发环境因素，控制变量环境因素，无果

解决：

由于在客服端复制链接时 http 前面存在一个特殊字符，在编辑器中显示的是空，无法容易发现。在排查一种未复制全链接，导致排查不到位。

- 资源链接复制时，最好将链接文本格式化一下

### 学习记录

这里写今天学习记录，可以包括阅读的技术文档、代码、生活。
