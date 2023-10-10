# ESlint 日常问题记录

##### 1.ESlint 和 Prettier 格式化冲突
ESLint 和 Prettier 在代码风格规范方面有不同的目标和方法，因此在某些情况下它们可能会发生冲突。主要的问题是它们可能会产生不一致的代码格式，导致 ESLint 报错，或者 Prettier 格式化后的代码与 ESLint 规则不匹配。

解决：

1.安装依赖
```
npm install --save-dev eslint-plugin-prettier eslint-config-prettier
```

2.ESLint配置文件调整
一般为 `.eslintrc.js` 或 `.eslintrc.cjs` 文件
``` js
{
  "extends": ["eslint:recommended", "plugin:prettier/recommended"],
  "plugins": ["prettier"]
}
```

##### 2.Vue3 的script中全局申明ESLint无法校验
场景：在根目录的全局申明文件 `global.d.ts` 中什么 `IMessage` , 我们在 `src/index.ts` 文件引用ESlint无报错， 在Vue3 的script中引用提示`not define` 错误
``` ts
// global.d.ts
interface IMessage {
    id: number
}

// src/index.ts
const a: IMessage = { id: 0 }; // 无报错

// src/index.vue
<script lang="ts" setup>
const a: IMessage = { id: 0 }; // 报错 not define
</script>
```

解决：
通过注释 `/*global AAA BBB*/` 告诉代码检查工具或编辑器某个变量或标识符是全局定义的，以免出现未定义的变量或警告。
``` ts
// src/index.vue
<script lang="ts" setup>
/*global IMessage*/
const a: IMessage = { id: 0 };
</script>
```
