# webpack 打包 Koa

> 项目打包工具有很多，比如 webpack、rollup、esbuild 等。本文主要选用 webpack 对项目进行打包

### 项目环境

Koa2、Typescript、Prisma、routing-controller、typedi

### 打包相关依赖

```
npm install webpack webpack-cli ts-loader webpack-node-externals -D
```

- webpack、webpack-cli 用于启动 webpack 打包工具
- ts-loader 用于 webpack 解析 Typescript 语法
- webpack-node-externals 用于 webpack 别名路径与 tsconfig.json 保持一致

### webpack.config.js 配置

```javascript
const path = require("path");
const nodeExternals = require("a"); // 允许从打包中排除node_modules依赖项
const TsconfigPathsPlugin = require("tsconfig-paths-webpack-plugin");

module.exports = {
  entry: "./app.ts",
  target: "node",
  devtool: "source-map",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "bundle.js",
  },
  resolve: {
    extensions: [".ts", ".tsx", ".js", ".json"],
    plugins: [
      // 用于 webpack 别名路径与 tsconfig.json 保持一致
      new TsconfigPathsPlugin({
        configFile: path.resolve(__dirname, "tsconfig.json"),
      }),
    ],
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        loader: "ts-loader",
      },
    ],
  },
  externals: [nodeExternals()],
};
```

使用 nodeExternals()创建外部函数并将其传递给 externals 属性。将 target 属性设置为'node'，以指示正在为 Node.js 环境构建

### 打包项目

```
npx webpack --mode=production
```

mode 可是设置当前环境
