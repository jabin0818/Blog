###  void 0

在JavaScript中，void是一个用来返回undefined的操作符。它可以将任何表达式返回为undefined。使用void 0代替undefined可以保证undefined的值不会被替换	

### 空值合并运算符 (??)

空值合并运算符在变量为 null 或 undefined 时提供默认值。

### 尾调用

尾调用（Tail Call）是指一个函数的最后一条语句是一个函数调用。这个调用的返回值直接作为函数的返回值，而不需要进行额外的操作或计算。具体来说，尾调用发生在函数的最后一个操作，且是函数返回之前的最后一个调用。

尾调用可以提供一些优化的机会。在某些编程语言和编译器中，通过对尾调用进行优化，可以将其转化为一个非递归形式，从而避免额外的函数调用带来的内存消耗和性能开销。这种优化称为尾调用优化（Tail Call Optimization）。

```js
function foo(x) {
  if (x <= 0) {
    return 0;
  }

  // 尾调用
  return bar(x - 1);
}
function bar(x) {
  return x * x;
}
```

在上面的示例中，函数foo中的最后一条语句是一个函数调用bar(x- 1)。这是一个尾调用，因为其返回值直接作为foo函数的返回值，没有进行其他的操作。在递归调用中，尾调用优化可以将递归转化为迭代，从而避免了递归的堆栈消耗。

值得注意的是，并非所有的编程语言都对尾调用进行优化，这取决于具体语言和编译器的实现。尾调用优化通常在函数调用嵌套较深或递归层数较大时能提供更明显的优势。

#### 关闭浏览器时发送请求

[Navigator.sendBeacon() - Web API 接口参考| MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Navigator/sendBeacon)

### if (document.all) === false

在JavaScript中，document.all是一个特殊的属性，它只在旧版本的InternetExplorer（IE）浏览器中存在，并且用于选择页面中的所有元素。这个属性在现代浏览器中已经被废弃，不被推荐使用。

在标准的JavaScript中，document.all属性是undefined。也就是说，在现代浏览器中使用if (document.all)条件判断时，它会被视为假值（falsy value）。

所以，如果你在使用现代浏览器时遇到了if (document.all)的情况，它将返回false，条件块中的代码将不会被执行。

需要注意的是，如果你运行在较旧版本的IE浏览器中，document.all是真值（truthy value），并且可以用于选择页面中的所有元素。但是，出于兼容性和可维护性的考虑，推荐使用现代的方法来选择和操作DOM元素，例如使用document.querySelector或document.getElementById等方法。

### object.propertyIsEnumerable('key')

判断对象属性是否可枚举

### Array.from()方法

Array.from()方法可以将可迭代对象转换为新的数组。

 

函数可接受3个参数（后两个参数可以没有）：

第一个表示将被转换的可迭代对象(如果只有一个参数就是把形参转变成数组)

第二个是回调函数，将对每个数组元素应用该回调函数，然后返回新的值到新数组，

第三个是回调函数内this的指向。

```js
let arr = [1, 2, 3];
let obj = {
    double(n) {
        return n * 2;
    }
}
console.log(Array.from(arr, function (n){
    return this.double(n);
}, obj)); // [2, 4, 6]

```

### >>.0

新运算符，将小数点后面置为0，用于number转int，也可直接使用 | 0



