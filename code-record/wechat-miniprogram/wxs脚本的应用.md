# wxs 脚本的应用

> wxs（WeiXin Script）是微信小程序提供的一种类 JavaScript 脚本语言，它可以在 wxml 文件中使用，用于处理复杂的逻辑和计算

### 应用环境

由于 路由页面 wxml 文件无法直接调用关联的.js 文件内的 method，所以可以通过引入 wxs 去解决这个问题。它的主要功能如下：

- 处理复杂的逻辑：可以通过编写 wxs 脚本来处理一些复杂的逻辑，例如排序、过滤、搜索等等。
- 计算属性：可以使用 wxs 脚本来定义计算属性，以便在 wxml 文件中使用。
- 共享代码：可以在不同的 wxml 文件中引用同一个 wxs 脚本文件，以达到代码共享的目的。

### 注意事项

- WXS 不依赖于运行时的基础库版本，可以在所有版本的小程序中运行。
- WXS 与 JavaScript 是不同的语言，有自己的语法，并不和 JavaScript 一致。
- WXS 的运行环境和其他 JavaScript 代码是隔离的，WXS 中不能调用其他 JavaScript 文件中定义的函数，也不能调用小程序提供的 API。
- 由于运行环境的差异，在 iOS 设备上小程序内的 WXS 会比 JavaScript 代码快 2 ~ 20 倍。在 android 设备上二者运行效率无差异。
- 需要通过 Common.js 的方式导出变量、函数等

### 如何应用 wxs

#### 内联模式

直接在 wxml 直接添加模块并引用

```html
<wxs modle="wxsName"> module.exports = { func: function(val) {} } </wxs>
<view>{{wxsName.func(pageData)}}</view>
```

#### 外联模式

新建一个扩展名为.wxs 文件，在文件编写方法、编写等并导出

```javascript
// ./util.wxs
module.exports = { func: function (val) {} };
```

```html
// ./index.wxml
<wxs modle="wxsName" src="./util.wxs"></wxs>
<view>{{wxsName.func(pageData)}}</view>
```

注：建议通过外联模式去使用，便与方法的统一管理~

### wxs 脚本语法

[微信官方 wxs 语法参考](https://developers.weixin.qq.com/miniprogram/dev/reference/wxs/)
