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

### 1.通过输出打包树状图分析打包bundle结构

```javascript
// vue打包输出树状图命令
vue-cli-service build --report
```

### 2.通过splitChunks拆包

通过webpack的optimization配置拆包将大体积的bundle拆分为多个小体积的bundle，提升http请求速度和加载速度

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

### 3.外部化处理

通过外部化（externals）配置，将vue，vuex,vue-router等从打包体积中分离出来，通过外部的cdn或者本地静态资源加载的方式使用，从而减少打包体积并升打包速度

```javascript
// webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
    }),
  ],
  externals: {
    jquery: 'jQuery',
  },
};
```

### 4. copy-webpack-plugin

可以将文件或文件夹复制到打包输出文件夹,配合外部化使用时即可以享受npm包管理的功能又可以像本地资源一样加载加载

```javascript
const CopyWebpackPlugin = require("copy-webpack-plugin")
{
    configureWebpack:config=>{
        config.plugins=[
            new CopyWebpackPlugin({
                patterns: [
                  { from: "node_modules/xlsx/dist/xlsx.full.min.js", to: "js/xlsx.js" },
                  { from: "node_modules/vuex/dist/vuex.min.js", to: "js/vuex.js" },
                  { from: "node_modules/vue-router/dist/vue-router.min.js", to: "js/vue-router.js" }
                  { from: "node_modules/vue/dist/vue.min.js", to: "js/vue.js" }
                ],
                options: {
                  concurrency: 100 // 控制并发数，默认为 100
                }
            })
        ]
    }
}
```

### 通过runjs循环打包实现多入口打包库或插件

将库打包成更细小的功能单位

## 三、其他优化

### 1. webpack开启gzip压缩

```javascript
const CopyWebpackPlugin = require("copy-webpack-plugin");
config.plugins.push(
      new CompressionPlugin({
        test: /\.js$|\.html$|.\css|\.less|\.scss/, // 指定要压缩的文件类型
        threshold: 1024 * 30, // 只有大小大于该值的资源会被处理 30KB
        deleteOriginalAssets: false, // 是否删除原文件
      })
    );
```

### 2. 图片压缩

### 3. 对较大对静态资源文件根据情况采取强制缓存或者协商缓存策略

#### 强缓存原理

强缓存是指浏览器直接从缓存中获取资源，并且不会向服务器发送请求。当浏览器第一次请求资源时，服务器会在响应头中设置 Cache-Control 或 Expires 字段，告诉浏览器该资源在一段时间内不会发生变化，可以直接从缓存中获取。当浏览器再次请求该资源时，会先检查缓存是否过期，如果未过期，则直接从缓存中获取，否则会向服务器发送请求。

其中，Cache-Control 字段是 HTTP/1.1 中新增的缓存控制字段，常用的取值有：

max-age=<seconds>：表示资源在经过 <seconds> 秒后过期，需要重新向服务器请求。
no-cache：表示必须先向服务器发送请求，确认资源是否过期。
no-store：表示不缓存任何响应内容。
而 Expires 字段则是 HTTP/1.0 中定义的缓存控制字段，它的值为资源过期的具体时间，如 Expires: Wed, 21 Oct 2015 07:28:00 GMT。

#### 协商缓存原理

协商缓存是指浏览器向服务器发送请求，并在请求头中带上与该资源相关的信息（如上次请求的时间、ETag 等），由服务器根据这些信息判断资源是否发生变化。如果资源未发生变化，服务器会返回 304 状态码和空的响应体，告诉浏览器可以直接从缓存中获取资源。如果资源发生了变化，服务器会返回新的资源，并在响应头中设置新的缓存控制字段，告诉浏览器如何缓存新的资源。

协商缓存通常使用的是 Last-Modified 和 If-Modified-Since 或 ETag 和 If-None-Match 搭配使用，它们的作用分别如下：

Last-Modified：表示资源的最后修改时间，服务器在返回资源时会在响应头中设置该字段。
If-Modified-Since：表示上次请求资源时的最后修改时间，浏览器在请求头中设置该字段。服务器在接收到请求后会将该字段的值与资源的最后修改时间进行比较，如果资源的最后修改时间早于该字段的值，则返回 304 状态码；否则返回新的资源。
ETag：表示资源的标识符，服务器在返回资源时会在响应头中设置该字段。
If-None-Match：表示上次请求资源时的标识符，浏览器在请求头中设置该字段。服务器在接收到请求后会将该字段的值与资源的标识符进行比较，如果相等，则返回 304 状态码；否则返回新的资源。

### 4.通过link或者script加载的资源文件通过添加async，defer或preload优化加载过程

#### async

async 属性表示该文件可以异步加载，不会阻塞页面的渲染和加载，但不保证执行顺序。当浏览器遇到 <script async> 标签时，会立即开始下载文件，并在下载完成后立即执行。如果页面中存在多个异步加载的文件，它们的执行顺序是不确定的。

#### defer

defer 属性表示该文件可以延迟加载，直到页面加载完毕再执行。当浏览器遇到 <script defer> 标签时，会立即开始下载文件，但不会立即执行。而是在页面加载完成后，按照顺序依次执行延迟加载的文件。如果页面中存在多个延迟加载的文件，它们会按照在页面中出现的顺序依次执行。

#### preload

preload 属性表示该文件需要优先加载，可以在页面加载时预加载文件，以缩短文件下载时间。当浏览器遇到 <link rel="preload"> 标签时，会立即开始下载文件，并在下载完成后缓存到浏览器中。然后在后续需要使用该文件时，可以直接从缓存中获取，避免了网络传输的时间。但需要注意的是，预加载文件会占用额外的网络带宽和资源，因此应该根据文件的大小和重要性选择合适的文件进行预加载。

### 5.第三方ui组件按需加载
