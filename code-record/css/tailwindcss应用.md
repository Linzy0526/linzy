# tailwindcss 应用

> [tailwindcss 中文文档](https://www.tailwindcss.cn/docs/flex)

### 解决了什么痛点？

- 繁琐的 CSS 编写：传统 CSS 编写需要手写大量的 CSS 代码来定义各种样式，这非常繁琐和耗时。Tailwind CSS 通过提供预定义的类来简化 CSS 编写，使得开发人员可以更快速地构建界面

- 样式一致性问题：在传统的 CSS 编写中，样式的一致性非常难以维护，因为不同的开发人员可能会使用不同的命名规则、类名和样式。Tailwind CSS 通过提供一组明确的类和样式来确保样式的一致性，从而使得代码更易于维护和协作。

- 响应式设计困难：在传统的 CSS 编写中，实现响应式设计需要编写大量的媒体查询和 CSS 代码。Tailwind CSS 提供了一组响应式的类来简化响应式设计，使得开发人员可以更轻松地创建适应不同屏幕尺寸的界面。

### 在 Vue3 + Vite 中配置安装

#### 1. 安装依赖

```
npm install tailwindcss postcss autoprefixer -D
```

#### 2.初始化配置文件

```
npx tailwindcss init
```

执行完这条命令后会在当前目录下生成 tailwind.config.js 文件用来来自定义配置

#### 3.配置

```js
module.exports = {
  // Just-In-Time 编译模式，提升编译速度
  mode: "jit",
  // 指定需要 Tree Shaking 的文件
  purge: ["./index.html", "./src/**/*.{vue,js}"],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {
      // 扩展主题
    },
  },
  variants: {
    extend: {},
  },
  plugins: [],
};
```

#### 4.Vite 中配置

```js
export default defineConfig({
  css: {
    postcss: {
      plugins: [require("tailwindcss"), require("autoprefixer")],
    },
  },
});
```

#### 5.项目中引入 css

在 mian.js 中直接应用 css

```js
import "tailwindcss/tailwind.css";
```

### 常用样式类
