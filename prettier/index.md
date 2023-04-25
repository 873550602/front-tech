# Prettier

## 安装

```cmd
npm install --save-dev prettier
# OR
yarn add prettier
```

## 配置

```plain text
printWidth (80)：指定代码行的最大长度，超过该长度就会被换行。
tabWidth (2)：指定缩进的空格数。
useTabs (false)：指定是否使用 Tab 字符进行缩进。
semi (true)：指定是否在语句末尾添加分号。
singleQuote (false)：指定是否使用单引号代替双引号。
quoteProps (as-needed)：指定对象属性是否使用引号。可选值有 as-needed（只在必需时添加），consistent（保持一致性）和 preserve（保留原样）。
jsxSingleQuote (false)：指定 JSX 中是否使用单引号代替双引号。
trailingComma (none)：指定是否在对象或数组最后一个元素后添加逗号。可选值有 none（不添加），es5（添加 ES5 中允许的逗号），all（添加所有可能的逗号）。
bracketSpacing (true)：指定是否在对象字面量的括号内添加空格。
jsxBracketSameLine (false)：指定 JSX 元素的尖括号是否与该元素的开头放在同一行。
arrowParens (avoid)：指定箭头函数的参数是否使用括号。可选值有 avoid（单个参数时省略括号），always（总是使用括号）和 avoid（没有参数时使用括号）。
rangeStart (0)：指定要格式化的范围的起始位置。
rangeEnd (Infinity)：指定要格式化的范围的结束位置。
parser (babylon)：指定要使用的解析器。可选值有 flow、babylon、typescript、css、less、scss、json、graphql、markdown、vue、html 和 angular。
filepath (none)：指定当前文件的路径，用于在 .prettierignore 文件中排除特定文件。
requirePragma (false)：指定是否需要使用特殊的注释才会格式化文件。
insertPragma (false)：指定是否在文件顶部添加一个特殊的注释，指示文件已经被格式化。
proseWrap (preserve)：指定如何处理 Markdown 文件中的换行符。可选值有 always（在 Markdown 段落之间添加换行符），never（删除 Markdown 段落之间的换行符）和 preserve（保留原样）。
htmlWhitespaceSensitivity (css)：指定 HTML 文件中空格敏感度。可选值有 css（只保留需要的空格），strict（空格非常敏感）和 ignore（忽略空格）。
```

## 兼容eslint

### 1.使用插件配置

```javascript
// 1. 安装插件 eslint-config-prettier
// 1. 在.eslintrc下添加如下配置
{
  "extends": [
    "eslint:recommended",
    "plugin:prettier/recommended"
  ]
}

```

### 2.通过在.eslintrc下配置prettier配置

```javascript
{
    "prettier/prettier": [
            "error",
            {
              "printWidth": 80,
              "tabWidth": 4,
              "useTabs": false,
              "semi": true,
              "singleQuote": false,
              "quoteProps": "as-needed",
              "jsxSingleQuote": false,
              "trailingComma": "none",
              "bracketSpacing": true,
              "jsxBracketSameLine": false,
              "arrowParens": "avoid",
              "requirePragma": false,
              "insertPragma": false,
              "proseWrap": "preserve",
              "htmlWhitespaceSensitivity": "css",
              "vueIndentScriptAndStyle": false,
              "endOfLine": "auto",
              "embeddedLanguageFormatting": "auto"
            }
          ]
}
```

## 在VS Code中使用

打开 **vscode**的**settion.json**文件添加如下配置

```json
{
    "editor.defaultFormatter": "dbaeumer.vscode-eslint",
    "editor.formatOnSave": true
}
```

## eslint下的tabWidth如何适配多个size

```json
{ "tabWidth": [2,4]}
```

## 通配符匹配文件

```bash
# 匹配 .js 和 .jsx 文件
[*.{js,jsx}]
indent_style = space
indent_size = 4

# 匹配 .html 和 .htm 文件
[*.{html,htm}]
indent_style = space
indent_size = 2
```
