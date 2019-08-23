&nbsp;&nbsp;JSX的语法既不是字符串也不是html，他是一个JavaScript的语法扩展，React官方建议在开发中使用，因为他可以很好的描述UI应该呈现的本质形式。

&nbsp;&nbsp;其实在我看来jsx就是一个最简化的组件，不过他没有对于自身状态的维护，但是他可以接受外部的参数数据，因为组件最终的是render()内部的DOM树渲染，而jsx起始也是一个DOM树，不过其没有内部逻辑，仅有DOM树的结果以及数据的加载。

&nbsp;&nbsp;其实下面这个表达式我觉的可以很好的描述我的观点：

```
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```





### JSX防止注入工具

&nbsp;&nbsp;React DOM在渲染所有的输入内容之前都会对内容做转义，转换成为字符串，这样可以确保应用中不会注入非自己编写的内容，这样可以有效地防止  [XSS（cross-site-scripting, 跨站脚本）](https://en.wikipedia.org/wiki/Cross-site_scripting)攻击。



### JSX表示对象

&nbsp;&nbsp;Babel 会把 JSX 转译成一个名为 `React.createElement()` 函数调用。但是我个人来说不建议做转换，直接使用标签会更利于观测布局的层级，但是这还是要看个人的喜好了。
