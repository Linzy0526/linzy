# 2023 年 04 月 14 日

> weather: ☀️
>
> emoji: 😀

### 问题记录

#### 1.项目环境 Koa+Prisma+ts 中编写全局声明文件/typings/x.d.ts 时，vscode 成功加载，编译阶段报错提示未加载到声明

> 初步思考：是否为 tsconfig.json 配置出现错误？然而配置中 typeRoots 和 include 已正确配置

```json
{
  "compilerOptions": {
    "typeRoots": ["node_modules/@types", "typings"]
  },
  "include": ["app/**/*", "configs/**/*", "**/*.ts", "**/*.tsx"]
}
```

> 解决：最后发现是 ts-node 不会主动编译 x.d.ts 文件，需要在执行的时候添加命令`--files`

```json
{
  "scripts": {
    "dev": "cross-env NODE_ENV=development; ts-node-dev --files -r tsconfig-paths/register app.ts"
  }
}
```
