> 附带条件的 mixins

Guards are useful when you want to match on _expressions_, as opposed to simple values or arity. If you are familiar with functional programming, you have probably encountered them already.

当你想在匹配_表达式_，而不是简单的值或数量时，Guard很有。 如果你熟悉函数式编程，你可能已经遇到过它们了。

In trying to stay as close as possible to the declarative nature of CSS, Less has opted to implement conditional execution via **guarded mixins** instead of `if`/`else` statements, in the vein of `@media` query feature specifications.

为了尽量保持CSS的声明性，Less选择通过**guarded mixins**而不是`if` /`else`语句来实现条件执行的格式。

从一个例子开始：

```less
.mixin (@a) when (lightness(@a) >= 50%) {
  background-color: black;
}
.mixin (@a) when (lightness(@a) < 50%) {
  background-color: white;
}
.mixin (@a) {
  color: @a;
}
```

关键是`when`关键字，它引入了一个guard序列（这里只有一个guard）。 如果我们运行下面的代码：

```less
.class1 { .mixin(#ddd) }
.class2 { .mixin(#555) }
```

得到的结果为：

```css
.class1 {
  background-color: black;
  color: #ddd;
}
.class2 {
  background-color: white;
  color: #555;
}
```

### 1.Guard 比较运算符

在guard中可用的所有比较运算符：`>`，`> =`，`=`，`= <`，`<`。 此外，关键字 `true`是唯一的真值，以下两个mixin等价：

```less
.truth (@a) when (@a) { ... }
.truth (@a) when (@a = true) { ... }
```

除关键字“true”以外的任何值都是假的：

```less
.class {
  .truth(40); // 不符合上述任何一个定义。
}
```

请注意，也可以参数相互比较或者与非参数比较：

```less
@media: mobile;

.mixin (@a) when (@media = mobile) { ... }
.mixin (@a) when (@media = desktop) { ... }

.max (@a; @b) when (@a > @b) { width: @a }
.max (@a; @b) when (@a < @b) { width: @b }
```

### 2.Guard 逻辑运算符

您可以使用guard逻辑运算符。 该语法基于CSS媒体查询。

使用`and和`关键字组合guard：

```less
.mixin (@a) when (isnumber(@a)) and (@a > 0) { ... }
```

可以通过用逗号`,`分隔guard来模拟*or*操作符。 如果任何一名guards为真，则认为是匹配：

```less
.mixin (@a) when (@a > 10), (@a < -10) { ... }
```

使用`not`关键字否定条件：

```less
.mixin (@b) when not (@b > 0) { ... }
```

### 3.类型检查功能

最后，如果你想基于值类型来匹配mixins，你可以使用`is`函数：

```less
.mixin (@a; @b: 0) when (isnumber(@b)) { ... }
.mixin (@a; @b: black) when (iscolor(@b)) { ... }
```

这里是基本的类型检查功能：

* `iscolor`
* `isnumber`
* `isstring`
* `iskeyword`
* `isurl`

如果你想检查一个值是否在一个特定的单位，除了是一个数字，你可以使用以下：

* `ispixel`
* `ispercentage`
* `isem`
* `isunit`

### 4.附带条件的 Mixins

_(**FIXME**)_此外，可以使用`default`函数使一个mixin匹配依赖于另一个mixin匹配，可以使用它来创建类似于`else` 或`default`的“条件mixins” 语句（分别是 `if`和`case`结构）：

```less
.mixin (@a) when (@a > 0) { ...  }
.mixin (@a) when (default()) { ... } // 只有当第一个mixin不匹配，即@a <= 0时才匹配
```
