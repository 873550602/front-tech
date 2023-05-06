# vue2性能优化总结

## 一、vue组件的优化

### 1.异步组件

只有在需要时才会被加载的组件。这可以减少应用程序的初始加载时间，并提高应用程序的响应速度。你可以使用 Vue.component() 方法或 import() 语法来定义异步组件。

```javascript
const HelloWorld = () => import("./components/HelloWorld.vue");
```

### 2.路由懒加载

如果你的应用程序使用了 Vue Router，你可以使用路由懒加载来延迟加载页面和组件，以提高应用程序的响应速度和性能。你可以使用 import() 语法来定义懒加载路由。

```javascript
const HelloWorld = () => import("./components/HelloWorld.vue");
{
    path:"/hello-world",
    component:HelloWorld
}
```

### 合理使用v-if和v-show

使用 v-if 和 v-show：在 Vue.js 应用程序中，v-if 和 v-show 可以用来控制组件的显示和隐藏。v-if 会在组件不需要显示时销毁组件，而 v-show 只是隐藏组件，因此在组件需要频繁切换时，使用 v-show 可以提高应用程序的性能。

### 使用 keep-alive

在 Vue.js 应用程序中，keep-alive 可以将组件缓存起来，从而避免不必要的组件重复渲染和销毁。你可以使用 include 和 exclude 属性来控制哪些组件需要缓存。

### 使用 v-cloak

v-cloak 可以用来避免在应用程序初始化时出现闪烁的问题。你可以使用 v-cloak 来隐藏组件，然后使用 CSS 来控制组件的显示和隐藏。

## 二、vue打包优化

### 1.通过splitChunks拆包

```javascript
optimization: {
  splitChunks: {
    chunks: 'async', // 选择哪些块进行优化，可选值为 'all'，'async' 和 'initial'，默认为 'async'
    minSize: 30000, // 块的最小体积（字节），默认为 30000
    maxSize: 0, // 块的最大体积（字节），默认为 0，表示没有限制
    minChunks: 1, // 块的最小被引用次数，默认为 1
    maxAsyncRequests: 5, // 按需加载时的最大并行请求数，默认为 5
    maxInitialRequests: 3, // 入口点最大并行请求数，默认为 3
    automaticNameDelimiter: '~', // 默认的块名称分隔符
    name: true, // 根据块和缓存组名称自动生成名称，如果为 false，则使用 chunkId
    cacheGroups: { // 缓存组配置
      vendors: {
        test: /[\\/]node_modules[\\/]/, // 匹配规则，用于确定哪些模块应该被打包到这个缓存组中
        priority: -10, // 缓存组优先级，决定了该缓存组中的模块应该打包到哪个块中，默认为 -10
        filename: 'vendors.js' // 生成的文件名
      },
      default: {
        minChunks: 2, // 最小被引用次数，默认为 2
        priority: -20, // 缓存组优先级，默认为 -20
        reuseExistingChunk: true, // 是否重用已经存在的块，默认为 true
        filename: 'common.js' // 生成的文件名
      }
    }
  }
}
```
