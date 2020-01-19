一、错误边界

二、Fragments

三、高阶组件

# 一、错误边界

[参考：https://zh-hans.reactjs.org/docs/error-boundaries.html](https://zh-hans.reactjs.org/docs/error-boundaries.html)

&nbsp;&nbsp;部分 UI 的 JavaScript 错误不应该导致整个应用崩溃，为了解决这个问题，React 16 引入了一个新的概念 —— 错误边界。

&nbsp;&nbsp;错误边界是一种 React 组件，这种组件**可以捕获并打印发生在其子组件树任何位置的 JavaScript 错误，并且，它会渲染出备用 UI**，而不是渲染那些崩溃了的子组件树。错误边界在渲染期间、生命周期方法和整个组件树的构造函数中捕获错误。

**ps：**错误边界**无法**捕获以下场景中产生的错误：

- 事件处理（[了解更多](https://zh-hans.reactjs.org/docs/error-boundaries.html#how-about-event-handlers)）
- 异步代码（例如 `setTimeout` 或 `requestAnimationFrame` 回调函数）
- 服务端渲染
- 它自身抛出来的错误（并非它的子组件）

### &nbsp;&nbsp;如果一个 class 组件中定义了 [`static getDerivedStateFromError()`](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromerror) 或 [`componentDidCatch()`](https://zh-hans.reactjs.org/docs/react-component.html#componentdidcatch) 这两个生命周期方法中的任意一个（或两个）时，那么它就变成一个错误边界。当抛出错误后，请使用 `static getDerivedStateFromError()` 渲染备用 UI ，使用 `componentDidCatch()` 打印错误信息。然后你可以将它作为一个常规组件去使用。

&nbsp;&nbsp;try/cache 异常捕获仅适用于命令式代码。

# 二、Fragments

<div>
<a href="https://zh-hans.reactjs.org/docs/fragments.html">参考：https://zh-hans.reactjs.org/docs/fragments.html
</a>
</div>

&nbsp;&nbsp;React 中的一个常见模式是一个组件返回多个元素。Fragments 允许你将子列表分组，而无需向 DOM 添加额外节点。

&nbsp;&nbsp;其实Fragments就是虚拟一个父节点，这个节点在代码中是存在的，但是在实际的编译好的文件当中是不存在的，他多用于**父级使用子控件，但是子组件应为一个层级，但是子组件有多个元素，直接返回的话属于多个对象无法返回，只能包一层DOM节点，但是这层节点无用，同时还有可能影响父级调用，这个时候用Fragments替换这个节点就好了，两种情况都会满足。**

&nbsp;&nbsp;使用方式：`<React.Fragment><td>Hello</td><td>World</td></React.Fragment>`

&nbsp;&nbsp;`key`  是唯一可以传递给  `Fragment`  的属性。未来我们可能会添加对其他属性的支持，例如事件。

# 三、高阶组件

<div>
<a href="https://zh-hans.reactjs.org/docs/higher-order-components.html">参考：https://zh-hans.reactjs.org/docs/higher-order-components.html
</a>
</div>

&nbsp;&nbsp;具体而言，**高阶组件是参数为组件，返回值为新组件的函数。**高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧。HOC 自身不是 React API 的一部分，它是一种基于 React 的组合特性而形成的设计模式。组件是将 props 转换为 UI，而高阶组件是将组件转换为另一个组件。

&nbsp;&nbsp;**其实高阶组件在我理解看来其实就是和设计模式里面的装饰器模式很像，就是我给你一个实例，你对这个实例处理，处理完之后再新增装饰器的代码，最后再讲处理后的实例或者新实例返回回来给我，那就好了。**

&nbsp;&nbsp;**之所以理解成为这样是因为他所举得例子就和装饰器模式很像，有一些通用方法抽象到一个组件或者函数当中，在生命周期当中实现订阅以及取消订阅，然后返回新建的实例。**
