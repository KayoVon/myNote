`reduce` 方法接收一个函数作为累加器, 数组中的每个值(从左到右)开始缩减, 最终计算为一个值.

对于空数组, `reduce` 不会执行回调函数.

语法:

```JS
array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
```

其中回调函数有四个参数:

-   `total`: 必填. 初始值或者计算结束后的返回值.
-   `currentValue`: 必填. 当前元素.
-   `currentIndex`: 可选. 当前元素的索引.
-   `arr`: 可选. 当前元素所属的数组对象(哪个数组调用的).

`initialValue` 表示初始值或者计算结束后的返回值.

1. 自我介绍
2. 项目经历 遇到的问题
3. 跨平台开发的看法
4. 跨域问题的原因和解决方案 浏览器导致还是客户端导致
5. webpack 中预加载器的执行顺序 .less 文件是怎么被解析的
6. .vue 文件如何被解析，vue-loader 的原理（半引导式询问） <template></template>中的文件怎么处理的
7. html 打包完之后在哪里？
8. CommonJS AMD CMD 的模块机制
9. react有了解吗？react 和 vue与原生有什么区别，react和vue有什么区别
10. 有什么想问的
