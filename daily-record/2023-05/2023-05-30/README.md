# 2023 年 05 月 31 日

> weather: ☀️
>
> emoji: 😀

### 问题记录

1. Prisma 中事务回滚理解有误，关联[2023-04-26 1问题](https://github.com/Linzy0526/linzy/tree/master/daily-record/2023-04/2023-04-26/README.md)
   
在04-26记录的错误，误认为$transcation回调函数内无法实现事务回滚，其实用法存在问题！！

Prisma 事务用 prisma.$transcation 去实现

```typescript
// 2023-04-26 记录问题
const result = await prisma.$transcation(async () => {
  const user = await prisma.user.update();

  if (user.xx) {
    throw new Error("xx");
  }

  await prisma.task.update(); // 抛出异常
});

// 正确用法
const result = await prisma.$transcation(async (p) => {
  const user = await p.user.update();

  if (user.xx) {
    throw new Error("xx");
  }

  await p.task.update(); // 抛出异常
});
```
当prisma.$transcation用回调函数来处理事务队列时，回调函数内部的prisma实例需要引用回调函数的第一个参数的prisma实例对象

[prisma事务](https://github.com/Linzy0526/linzy/tree/master/code-record/prisma/事务.md)


### 学习记录

这里写今天学习记录，可以包括阅读的技术文档、代码、生活。
