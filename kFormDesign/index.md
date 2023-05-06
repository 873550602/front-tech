# k-form-design表单设计器

## 什么是k-form-design

## 安装

```cmd
npm install k-form-design --save
# OR
yarn add k-form-design
```

## 安装 ant-design-vue UI ，推荐 vue2 版本最新的 1.7.8版本

```cmd
npm i ant-design-vue@1.7.8 --save
# OR
yarn add ant-design-vue@1.7.8
```

## 使用

```javascript
// 在main.js引入

// 注：useComponents 需放最上面，优先注册懒加载组件
import { useAntd } from 'k-form-design/packages/core/useComponents'
import KFormDesign from 'k-form-design/packages/use.js'
import 'k-form-design/lib/k-form-design.css'

useAntd(Vue)
Vue.use(KFormDesign)

// 使用预加载模式
- import { useAntd } from 'k-form-design/packages/core/useComponents'
+ import { useAntd } from 'k-form-design/packages/core/preUseComponents'
```

## 按需加载

```javascript
import { KFormDesign, KFormBuild } from "k-form-design/packages/use.js";
import "k-form-design/lib/k-form-design.css";
```

## 如何添加自定义组件

```javascript
// 一、使用nodeSchema
import { nodeSchema } from "k-form-design";
const Cmp = {
  label: "cmp",
  render: function (h) {
    return h("lc-button", "click me");
  }
};
// 添加组件
nodeSchema.addSchemas([
  {
    type: "Cmp", // 表单类型
    label: "Cmp",
    icon: "",
    component: Cmp,
    options: {
      defaultValue: "",
      multiple: false,
      disabled: false,
      width: "auto",
      clearable: true,
      placeholder: "请选择",
      showSearch: false,
      showLabel: false
    }
  }
]);

// 添加分组
nodeSchema.addSchemaGroup({
  title: "自定义组件",
  list: ["Cmp"]
});

// 二、使用setFormDesignConfig
import { setFormDesignConfig } from "k-form-design";
const Cmp = {
  label: "cmp",
  render: function (h) {
    return h("lc-button", "click me");
  }
};
// 使用函数配置
setFormDesignConfig({
  title: "自定义字段",
  list: [
    {
      type: "Cmp", // 组件类型
      label: "自定义组件", // 组件名称
      component: Cmp, // 组件
      options: {
        defaultValue: undefined, // 可选值
        multiple: false, // 可选值
        disabled: false, // 可选值
        width: "100%",
        clearable: true, // 可选值
        placeholder: "请选择", // 可选值
        showSearch: false // 可选值
      },
      model: "", // 可选值
      key: "",
      rules: [
        // 可选值
        {
          required: false,
          message: "必填项"
        }
      ]
    }
  ]
});
```

## nodeSchema的所有属性配置

```javascript
{
    
}
```
