State类

一、生命周期

二、State的作用

# State类

```dart
class RandomWordsState extends State {
  @override
  Widget build(BuildContext context) {
    final wordPair = new WordPair.random();
    return new Text(wordPair.asPascalCase);
  }
}

类说明：
class 类名称 extends State{
  @override
  Widget build(BuildContext context) {
    .
    .
    //根据逻辑和状态返回widget布局，同时也有部分的逻辑操作，例如点击事件等
    .
    .
    final wordPair = new WordPair.random();
    return new Text(wordPair.asPascalCase);
 }
}
```

# 一、生命周期

![](https://img4.mukewang.com/5b0b75310001253f10000524.jpg)

State的生命周期有四种状态：

      **created：**当State对象被创建时候，State.initState方法会被调用；
    
      **initialized：**当State对象被创建，但还没有准备构建时，State.didChangeDependencies在这个时候会被调用；
    
      **ready：**State对象已经准备好了构建，State.dispose没有被调用的时候；          
    
      **defunct：**State.dispose被调用后，State对象不能够被构建。

完整生命周期如下：

      **1.**创建一个State对象时，会调用StatefulWidget.createState；

和一个BuildContext相关联，可以认为被加载了（mounted）；

      **2.**调用initState；
    
      **3.**调用didChangeDependencies；
    
      **4.**经过上述步骤，State对象被完全的初始化了，调用build；
    
     **5.**如果有需要，会调用didUpdateWidget；
    
     **6.**如果处在开发模式，热加载会调用reassemble；
    
     **7.**如果它的子树（subtree）包含需要被移除的State对象，会调用deactivate；
    
     **8.**调用dispose,State对象以后都不会被构建；
    
     **9.**当调用了dispose,State对象处于未加载（unmounted），已经被dispose的State对象没有办法被重新加载（remount）。

# 二、State的作用有两点

1. 在widget构建的时候可以被同步读取；

2. 在widget的生命周期中可能会被改变。
