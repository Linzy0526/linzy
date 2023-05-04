# YYYY 年 MM 月 DD 日

> weather: ☀️
>
> emoji: 😮

### 问题记录

#### 1.esbuild 打包 routing-controller、typedi、Typescript 后无法正常读取到@Inject 注入的对象

项目环境：Koa2、routing-controller、typedi、Typescript

❌ 打包工具：esbuild

```Javascript
// esbuild 打包配置
const { nodeExternalsPlugin } = require('esbuild-node-externals')
require('esbuild')
  .build({
    entryPoints: ['app.ts'],
    bundle: true,
    outfile: 'dist/index.js',
    platform: 'node',
    plugins: [
      nodeExternalsPlugin({
        dependencies: false,
      }),
    ],
    external: ['cors', 'kcors'],
  })
  .catch(err => {
    console.log(err)
    process.exit(1)
  })

```

chatGpt 建议`esbuild-node-externals`即可，但是仍然无法解决（待解决）

⭕️ 打包工具：webpack

本着一条路走不通，那就换一条路的原则。选用 webpack 打包 😅

基础安装依赖：

```
npm install webpack webpack-cli ts-loader webpack-node-externals -D
```

如需要 webpack 配置的别名路径与 tsconfig 一致需要安装 `tsconfig-paths-webpack-plugin`

```Javascript
// ./webpack.config.js
const path = require('path');
const nodeExternals = require('webpack-node-externals');
const TsconfigPathsPlugin = require('tsconfig-paths-webpack-plugin');

module.exports = {
  entry: './app.ts',
  target: 'node',
  devtool: 'source-map',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },
  resolve: {
    extensions: ['.ts', '.tsx', '.js', '.json'],
    plugins: [
      new TsconfigPathsPlugin({
        configFile: path.resolve(__dirname, 'tsconfig.json'),
      }),
    ],
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        loader: 'ts-loader',
      },
    ],
  },
  externals: [nodeExternals()],
};
```
[webpack打包Koa](https://github.com/Linzy0526/linzy/tree/master/code-record/koa/webpack打包Koa.md)

### 学习记录

这里写今天学习记录，可以包括阅读的技术文档、代码、生活。
