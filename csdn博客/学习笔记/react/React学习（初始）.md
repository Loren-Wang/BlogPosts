**React** 是一个用于构建用户界面的 JavaScript 库。

&nbsp;&nbsp;其中最重要的就是组件的**render()**方法，这个方法的主要作用就是渲染DOM树的，具体的显示内容、显示结构都有它来决定，其中内部和vue一样，都可以直接使用变量的定义某一个熟悉或者显示数据。

&nbsp;&nbsp;在React当中基本上是没有html文件的，所以的页面以及逻辑都是js以及css文件。

一、介绍

二、元素渲染

三、组件

四、元素渲染流程

五、props

六、state

# 一、介绍

## 1、简单组件

&nbsp;&nbsp;简单组件使用一个名为render（）的方法接收输入的数据并返回要展示的内容，接收的数据在render内的this.props当中。

## 2、有状态组件

&nbsp;&nbsp; 除了像简单组件直接使用外部数据之外，react的组件还可以维护自身的内部状态，内部状态的控制通过**this.state**访问，**当内部状态发生变更时会再次调用内部 render()方法重写渲染对应的标记。**

## 3、在组件中使用外部插件

&nbsp;&nbsp;下面的代码就是在react的组件中使用了一个Remarkable的外部库。

```
 getRawMarkup() {
    const md = new Remarkable();
    return { __html: md.render(this.state.value) };
  }
```

# 二、元素渲染

1、元素是构成 React 应用的最小砖块。

2、元素描述了你在屏幕上想看到的内容。

3、组件是由一个甚至多个元素组成的。

4、元素的更新只能通过render进行全局更新，需要创建一个全新的元素才可以。

*ps：React的元素是不可变对象，一旦被创新，则无法更改他的子元素或者属性。*

5、React的更新是会只更新他需要更新的部分，即在更新之前React DOM会将元素状态进行相互比较，只会更新需要更新的部分。

# 三、组件

&nbsp;&nbsp;组件，从概念上类似于 JavaScript 函数。它接受任意的入参（即 “props”），并返回用于描述页面展示内容的 React 元素。

&nbsp;&nbsp;定义组件最重要的两条代码为：

```
 import React from 'react';(必须引入，否则该组件无效)
 .
 .
 .
 export default Welcome（如果在index.js文件当中可以没有，但是作为一个单独文件组件使用的话没有则表示无法使用该组件）
```

&nbsp;&nbsp;定义组件最简单的方式就是编写JavaScript函数，其参数只能是props属性对象，代表着内部代码块要处理的数据，而返回数据时一个**DOM**树（react元素），该种方式被称为“函数组件”，示例代码：

```
import React from 'react';

function Welcome(props) {
 return <h1>Hello, {props.name}</h1>;
 }
export default Welcome
```

&nbsp;&nbsp;同时还可以用ES6的class来定义组件

```
import React from 'react';
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
export default Welcome
```

ps:组件名称必须以大写字母开头，如果以小写开头的话会被React识别为原生标签。

# 四、元素渲染流程

1. 我们调用  `ReactDOM.render()`  函数，并传入  `<Welcome name="Sara" />`  作为参数。
2. React 调用  `Welcome`  组件，并将  `{name: 'Sara'}`  作为 props 传入。
3. `Welcome`  组件将  `<h1>Hello, Sara</h1>`  元素作为返回值。
4. React DOM 将 DOM 高效地更新为  `<h1>Hello, Sara</h1>`。

# 五、props

&nbsp;&nbsp;该对象是组件内部接收调用该组件方所传递的参数使用的，其最**重要的特性就是对象不可变**，也就是说在该组件当中的**props对象不可被替换不可被修改，但是其内部变量是可以修改变动的。**

# 六、state

&nbsp;&nbsp;该对象是组件内部自己使用的，也就是说相当于局部变量，而如果想让该组件刷新的话那么就要调用`this.setState({})`方法更新一些变量才能使得该组件刷新，其他的任何方式均无法导致该组件刷新。

&nbsp;&nbsp;下面说下state的特性：

1. 除了在构造函数当中，其他任何地方都不可以对state进行赋值，要想修改state数据只有通过`this.setState({})`方法修改；

2. state的更新可能是异步的，因为React可能会把多个`setState()`调用合并成一个调用，也就是说在该调用中单纯修改变量可能不会生效，但是如果在该调用中调用另外一个函数返回state，同时将state作为参数那么就可以进行修改了，同时修改后的变量也会生效；
