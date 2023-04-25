# eslint

## 基本配置

```json
{
  // 环境配置
  "env": {
    "browser": true, // 浏览器环境
    "node": true, // Node.js 环境
    "es6": true // ES6 环境
  },

  // 扩展配置
  "extends": [
    "eslint:recommended", // 推荐配置
    "plugin:react/recommended" // React 插件推荐配置
  ],

  // 解析器配置
  "parser": "babel-eslint", // 使用 Babel 解析器
  "parserOptions": {
    "ecmaVersion": 2021, // 使用 ES2021 语法
    "sourceType": "module", // 使用 ES6 模块
    "ecmaFeatures": {
      "jsx": true // 启用 JSX 语法支持
    }
  },

  // 插件配置
  "plugins": [
    "react" // React 插件
  ],

  // 规则配置
  "rules": {
    "no-console": "error", // 禁止使用 console
    "no-debugger": "error", // 禁止使用 debugger
    "no-unused-vars": "warn", // 警告未使用的变量
    "react/jsx-uses-react": "error", // 检查是否导入了 React
    "react/jsx-uses-vars": "error" // 检查是否使用了 JSX 中定义的变量
  }
}
```

## 常用规则

```json
{
  "rules": {
    "no-console": "error", // 禁止使用 console
    "no-debugger": "error",// 禁止使用 debugger
    "no-unused-vars": "warn", // 禁止未使用的变量
    "quotes": ["error", "single"], // 强制使用单引号或双引号
    "semi": ["error", "always"], // 强制使用分号
    "indent": ["error", 2], // 强制缩进为 2 个空格
    "prefer-arrow-callback": "error", // 使用 ES6 的箭头函数
    "no-eval": "error", // 禁止使用 eval() 函数
    "no-var": "error", // 禁止使用 var 声明变量：
    "brace-style": ["error", "1tbs", { "allowSingleLine": true }], // 强制在代码块中使用一致的大括号风格
    "no-await-in-loop": "error" // 禁止在循环中出现异步操作
  }
}
```
