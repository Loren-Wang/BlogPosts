**React** 是一个用于构建用户界面的 JavaScript 库。

&nbsp;&nbsp;其中最重要的就是组件的**render()**方法，这个方法的主要作用就是渲染DOM树的，具体的显示内容、显示结构都有它来决定，其中内部和vue一样，都可以直接使用变量的定义某一个熟悉或者显示数据。

&nbsp;&nbsp;在React当中基本上是没有html文件的，所以的页面以及逻辑都是js以及css文件。

一、介绍

二、元素渲染

三、组件

四、元素渲染流程

五、props

六、state

七、事件处理

八、条件渲染

九、列表

十、key

十一、表单

十二、状态提升

十三、组合vs继承

# 一、介绍

<div>
<a href="https://zh-hans.reactjs.org/">参考：https://zh-hans.reactjs.org/</a>
</div>

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

## 4、受控组件

&nbsp;&nbsp;被React控制取值的表单输入元素，也就是使React的state成为输入元素的“唯一数据源”，这种组件就叫做受控组件。

# 二、元素渲染

<div>
<a href="https://zh-hans.reactjs.org/docs/rendering-elements.html">参考：https://zh-hans.reactjs.org/docs/rendering-elements.html</a>
</div>

1、元素是构成 React 应用的最小砖块。

2、元素描述了你在屏幕上想看到的内容。

3、组件是由一个甚至多个元素组成的。

4、元素的更新只能通过render进行全局更新，需要创建一个全新的元素才可以。

*ps：React的元素是不可变对象，一旦被创新，则无法更改他的子元素或者属性。*

5、React的更新是会只更新他需要更新的部分，即在更新之前React DOM会将元素状态进行相互比较，只会更新需要更新的部分。

# 三、组件

<div>
<a href="https://zh-hans.reactjs.org/docs/components-and-props.html">参考：https://zh-hans.reactjs.org/docs/components-and-props.html</a>
</div>

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

<div>
<a href="https://zh-hans.reactjs.org/docs/components-and-props.html">参考：https://zh-hans.reactjs.org/docs/components-and-props.html</a>(页面底部)
</div>

1. 我们调用  `ReactDOM.render()`  函数，并传入  `<Welcome name="Sara" />`  作为参数。
2. React 调用  `Welcome`  组件，并将  `{name: 'Sara'}`  作为 props 传入。
3. `Welcome`  组件将  `<h1>Hello, Sara</h1>`  元素作为返回值。
4. React DOM 将 DOM 高效地更新为  `<h1>Hello, Sara</h1>`。

# 五、props

<div>
<a href="https://zh-hans.reactjs.org/docs/components-and-props.html">参考：https://zh-hans.reactjs.org/docs/components-and-props.html</>
</div>

&nbsp;&nbsp;该对象是组件内部接收调用该组件方所传递的参数使用的，其最**重要的特性就是对象不可变**，也就是说在该组件当中的**props对象不可被替换不可被修改，但是其内部变量是可以修改变动的。**

# 六、state

<div>
<a href="https://zh-hans.reactjs.org/docs/state-and-lifecycle.html">参考：https://zh-hans.reactjs.org/docs/state-and-lifecycle.html</a>
</div>

&nbsp;&nbsp;该对象是组件内部自己使用的，也就是说相当于局部变量，而如果想让该组件刷新的话那么就要调用`this.setState({})`方法更新一些变量才能使得该组件刷新，其他的任何方式均无法导致该组件刷新。

&nbsp;&nbsp;下面说下state的特性：

1. 除了在构造函数当中，其他任何地方都不可以对state进行赋值，要想修改state数据只有通过`this.setState({})`方法修改；

2. state的更新可能是异步的，因为React可能会把多个`setState()`调用合并成一个调用，也就是说在该调用中单纯修改变量可能不会生效，但是如果在该调用中调用另外一个函数返回state，同时将state作为参数那么就可以进行修改了，同时修改后的变量也会生效；

3. state的更新会被合并，当你调用 `setState()` 的时候，React 会把你提供的对象合并到当前的 state。也就是说每次执行`setState()`的时候只会对内部的响应变量做改动，其他的不变，例如一个对象里面有a、b、c、三个变量，而`setState()`内只有b变量的变动，那么只会修改b，a、c、变量不会有任何修改；

4. 数据流是自上而下的，也就是所有的数据只针对于当前的组件以及子组件，数据无法向上传递；

# 七、事件处理

<div>
<a href="https://zh-hans.reactjs.org/docs/handling-events.html">参考：https://zh-hans.reactjs.org/docs/handling-events.html</a>
</div>

1. React 事件的命名采用小驼峰式（camelCase），而不是纯小写；

2. 使用 JSX 语法时你需要传入一个函数作为事件处理函数，而不是一个字符串。例如：`<button  onClick={activateLasers}>Activate Lasers</button>`；

3. 在 React 中另一个不同点是你不能通过返回 `false` 的方式阻止默认行为。你必须显式的使用 `preventDefault`，例如：`function  handleClick(e)  {e.preventDefault();console.log('The link was clicked.');}`。ps:`e` 是一个合成事件。React 根据 [W3C 规范](https://www.w3.org/TR/DOM-Level-3-Events/)来定义这些合成事件；

4. 使用 React 时，你一般不需要使用 `addEventListener` 为已创建的 DOM 元素添加监听器。事实上，你只需要在该元素初始渲染的时候添加监听器即可；

5. 在组件的构造方法中需要将click事件进行绑定，否则的话再回调的click当中的this的值会为undefined，使用方式为：`// 为了在回调中使用 `this`，这个绑定是必不可少的    this.handleClick =  this.handleClick.bind(this);`

6. 同样点击事件也可以在render()渲染是通过箭头函数调用，这样也可以确保this是绑定状态，例如：`<button  onClick={(e) => this.handleClick(e)}>Click me</button>`，不过这样会在每次渲染时创建不同的函数，同样如果有参数传递给子组件的话子组件可能会重新渲染，所以最好还是把这个东西放在构造函数当中绑定最稳妥；

7. 向事件处理传参的话除了**e**是必传的之外，其他的参数是都可以自定义的；

# 八、条件渲染

<div>
<a href="https://zh-hans.reactjs.org/docs/conditional-rendering.html">参考：https://zh-hans.reactjs.org/docs/conditional-rendering.html</a>
</div>

1. 条件渲染既然有条件这两个字那就代表着在渲染的时候使用**if-else**属性，也就是说在组件中的click方法中可以使用条件判断，同样在rendre()返回要渲染的DOM树的时候也可以做**if-else**的逻辑判断，甚至内部也可以定义局部变量，只要最后返回的是一个DOM树或者空（不进行渲染）即可；

2. 同样React的内部是可以使用变量来存储元素的，也就是变量可以存储DOM树数据；

3. 条件渲染也可以根据**与运算符&&**来决定，这个和普通的java当中的与运算符的使用方式不一样，这里面的**与运算结果为真的话则执行与运算符右侧的代码，否则的话会直接返回false，React会忽略并跳过**，这是是可以使用false的，但是注意在click事件当中是不可以返回false的，依然要使用**preventDefault()**方法来阻止；

4. 三目运算符就不用说了，条件判断最常使用的方式；

5. 如果想要阻止组件渲染的话那么可以直接在**render()**当中直接返回null进行阻止，当然了，这个并不会影响组件的生命周期使用；

# 九、列表

<div>
<a href="https://zh-hans.reactjs.org/docs/lists-and-keys.html">参考：https://zh-hans.reactjs.org/docs/lists-and-keys.html</a>
</div>

&nbsp;&nbsp;列表其实没啥东西，不管是迭代器也好还是for循环还是map，他们其实都是对列表的数据处理，唯一不同的就是如果要列表输出显示，那么就要将**DOM树元素**做为返回值返回，将**DOM树的集合**存储为变量或者直接放到另外一个DOM树中进行数据展示处理，当然了，如果返回的是**DOM树元素**的话那么每一个元素都要设置一个key，否则的话虽然不影响数据显示，但是控制台会报错的（Warning: Each child in a list should have a unique "key" prop.）。

# 十、key

<div>
<a href="https://zh-hans.reactjs.org/docs/lists-and-keys.html">参考：https://zh-hans.reactjs.org/docs/lists-and-keys.html</a>
</div>

&nbsp;&nbsp;key 帮助 React 识别哪些元素改变了，比如被添加或删除。因此你应当给数组中的每一个元素赋予一个确定的标识。

1. 一般情况下所有的key最好都要不同，如果做不到的话那么最低要求为同级内不允许有相同的key，子组件和父组件不算，因为在每一个组件当中，内部的所有变量都是相对于当前组件独立的（ key 只是在兄弟节点之间必须唯一）；

2. 默认情况下React会将索引作为key值；

3. key可以用来提取组件，但是必须将key设置到你要提取的组件上，放到子组件或者父组件上都提取不到你要提取的组件；

4. key的信息之后再当前组件上，相当于一个虚拟的定位符，让React用来定位或者获取这一个组件的，也就是他只会将信息传递给React，而不会传递给其他组件，如果其他组件要用的话则通过其他属性名称进行传递；

# 十一、表单

<div>
<a href="https://zh-hans.reactjs.org/docs/forms.html">参考：https://zh-hans.reactjs.org/docs/forms.html</a>
</div>

&nbsp;&nbsp;表单的使用上其实和html没有什么区别，唯一的区别就是值的设定以及使用，和vue一样，表单或者说和输入相关的所有的值都使用value去设定，一旦设定了只有通过`setState()`方法去修改了，而输入的监听则通过`onChange={function(event)}`属性去监听改变，然后通过event.target.value去获取新值，然后通过`setState()`方法去设置新值。

&nbsp;&nbsp;Select标签通过在select标签上面设置value以及onchange来做数据的处理以及选中的改变，而不是在opotion子元素中处理，**如果要进行多选则使用代码：`<select multiple={true} value={['B', 'C']}>`**处理。

&nbsp;&nbsp;当有多个输入需要处理的时候，如果要处理数据那么只能通过name属性处理，其中如果要读取到name属性只有通过**target**去获取，上面也说到了监听中是有一个**event**的参数的，而**event.target**就可以获取到一些额外的参数，例如name，例如type等‘。

&nbsp;&nbsp;一般情况下设置了value值就会锁定掉输入，无法再进行输入，**加了onchange**的除外，如果仍然可以输入的话则可能是意外地将`value` 设置为 `undefined` 或 `null`。

# 十二、状态提升

<div>
<a href="https://zh-hans.reactjs.org/docs/lifting-state-up.html">参考：https://zh-hans.reactjs.org/docs/lifting-state-up.html</a>
</div>

&nbsp;&nbsp;说实话，我整个读下来感觉其实不应该叫做状态提升，而应该叫做子组件调用父组件方法，因为其中最重要的逻辑就是**父组件传递一个function函数到子组件**当中，函数名称可以自定义，然后子组件状态等发生变更时将调用父组件传递过来的function，从而出发父组件的数据变更，最后父组件调用`setState()`方法更新数据从而更新响应的多个子组件。

# 十三、组合vs继承

<div>
<a href="https://zh-hans.reactjs.org/docs/composition-vs-inheritance.html">参考：https://zh-hans.reactjs.org/docs/composition-vs-inheritance.html</a>
</div>

&nbsp;&nbsp;组合其实就是对于不同组件或者元素之间的相互拼接，例如一个弹窗，弹窗需要一些输入框和按钮，但是这个弹窗是个通用弹窗，什么都可以用，但是内部布局或者标题、内部显示的不同，那么就可以由父组件传递或者直接读取父组件该标签内部的所有的元素，例如：`props.children`会读取父组件当中能该标签内部的所有元素标签进行绘制显示，或者通过父组件的属性读取响应的元素标签或者内容等。

&nbsp;&nbsp;按照官方的说法就是到现在还没有人用到，所以他也就懒得介绍了。
