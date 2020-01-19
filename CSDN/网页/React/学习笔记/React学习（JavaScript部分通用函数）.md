1、setInterval函数（间隔时间启动函数）

2、map()函数（数组遍历函数）

# 1、setInterval函数

```
*var intervalID* = scope.setInterval（*func*，*delay* [，*arg1*，*arg2*，...]）;
```

参数说明：

**func**：每隔delay时间执行一次的方法；

 **delay**：间隔时间，从执行该方法开始后的delay时间之后执行func函数，同时再次间隔相同时间之后再次执行func函数，循环不间断执行；

 **arg1, ..., argN**`可选的，计时器到期后传递给*func*指定的*函数的*其他参数。 

**intervalID**返回的`intervalID`是一个数字非零值，用于标识调用创建的计时器`setInterval()`; 可以传递此值[`WindowOrWorkerGlobalScope.clearInterval()`]([https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/clearInterval](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/clearInterval) "WindowOrWorkerGlobalScope mixin的clearInterval（）方法取消了之前通过调用setInterval（）建立的定时重复操作。")以取消超时。

# 2、map()函数

```
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
console.log(doubled);
```

针对于数组的遍历，和iterator迭代器很相似，调用方为数组，map函数内部其实可以理解为是一个function函数，其中函数的参数值是数组的某一个位置的元素，而箭头后面的是元素的处理逻辑。同时map函数返回的数据时新的数组（可以是任意参数集的数组）；
