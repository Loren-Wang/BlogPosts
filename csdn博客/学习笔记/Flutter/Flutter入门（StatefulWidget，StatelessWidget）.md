1、StatelessWidget

2、StatefulWidget



```
      无状态静态的视图展示使用StatelessWidget，而有交互,需要动态变化的使用StatefulWidget.

  StatelessWidget初始化之后就无法改变，如果想改变，那便需要重新创建，new另一个StatelessWidget进行替换。但StatelessWidget因为是静态的，他没有办法重新创建自己。所以StatefulWidget便提供了这样的机制，通过调用`setState((){})`标记自身为dirty状态，以等待下一次系统的重绘检查。





  1.Stateless widgets 是不可变的，这意味着它们的属性不能改变——所有的值都是 final。

  2.Stateful widgets 持有的状态可能在 widget 生命周期中发生变化，实现一个 stateful widget 至少需要两个类：1）一个 StatefulWidget 类；2）一个 State 类，StatefulWidget 类本身是不变的，但是 State 类在 widget 生命周期中始终存在。
```



# StatelessWidget

        Flutter中的`StatelessWidget`是一个不需要状态更改的widget - 它没有要管理的内部状态，也不依赖于对象本身中的配置信息以及widget的BuildContext 。 

        也就是说如如果widget不会出现任何修改一直是静态显示的则用`StatelessWidget`这个东西。

        StatelessWidget初始化之后就无法改变，如果想改变，那便需要重新创建，new另一个StatelessWidget进行替换。但StatelessWidget因为是静态的，他没有办法重新创建自己。



 无状态widget的build方法通常只会在以下三种情况调用:

- 将widget插入树中时
- 当widget的父级更改其配置时
- 当它依赖的[InheritedWidget](https://docs.flutter.io/flutter/widgets/InheritedWidget-class.html)发生变化时



# StatefulWidget

       持有的状态可能在 widget 生命周期中发生变化，实现一个 stateful widget 至少需要两个类：1）一个 StatefulWidget 类；2）一个 State 类，StatefulWidget 类本身是不变的，但是 State 类在 widget 生命周期中始终存在。

       [StatefulWidget](https://docs.flutter.io/flutter/widgets/StatefulWidget-class.html) 是可变状态的widget。 使用`setState`方法管理StatefulWidget的状态的改变。调用`setState`告诉Flutter框架，某个状态发生了变化，Flutter会重新运行build方法，以便应用程序可以应用最新状态。  
