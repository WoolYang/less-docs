> Extend是Less的一个伪类，它会把所选择的选择器与它所引用的选择器进行合并。

发布[v1.4.0]({{ less.master.url }}CHANGELOG.md)

```less
nav ul {
  &:extend(.inline);
  background: blue;
}
```

在上面的规则集中，`:extend`选择器会将“扩展选择器”（`nav ul`）应用到 `.inline` class _出现的 `.inline` class中_ 。 声明块将保持原样，并不会直接添加extend中的属性（因为extend不是css）

如下：

```less
nav ul {
  &:extend(.inline);
  background: blue;
}
.inline {
  color: red;
}
```

编译为

```css
nav ul {
  background: blue;
}
.inline,
nav ul {
  color: red;
}
```

注意`nav ul:extend(.inline)`选择器如何在保持还是`nav ul`时输出结果 - 在输出之前将extend移除，而选择器块保持原样。 如果没有任何属性放在该块中，则会从输出中移除（但extend仍然可能会影响其他选择器）。

### 1.Extend 语法
extend要么附加到选择器，要么放在规则集中。 它看起来像一个带有选择器参数的伪类，可搭配关键字`all`：

例子:

```less
.a:extend(.b) {}

// 上面和下面的会得到同样的结果。
.a {
  &:extend(.b);
}
```

```less
.c:extend(.d all) {
  // extend “.d”的所有实例，例如 “.x.d”或“.d.x”。
}
.c:extend(.d) {
  // ".d"仅继承实例将被输出为“.d”的选择器。
}
```

可以继承包含一个或多个类，以逗号分隔。

例如：

```less
.e:extend(.f) {}
.e:extend(.g) {}

// 上面和下面的将会得到同样的结果。
.e:extend(.f, .g) {}
```

### 2.Extend 附加到选择器
Extend附加到选择器看起来像一个普通的伪类带选择器作为参数。 一个选择器可以包含多个extend子句，但所有的extend必须在选择器的末尾。

* 在选择器之后Extend： `pre:hover:extend(div pre)`.
* 选择器和Extend之间允许有空格： `pre:hover :extend(div pre)`.
* 运行多个Extend: `pre:hover:extend(div pre):extend(.bucket tr)` - 请注意，这是相同的 `pre:hover:extend(div pre, .bucket tr)`
* 这是不允许的： `pre:hover:extend(div pre).nth-child(odd)`. Extend必须在末尾。

如果一个规则集包含多个Extend，它们中的任何一个都可以有个Extend关键字。 多个带有Extend的选择器在一个规则集中：

```less
.big-division,
.big-bag:extend(.bag),
.big-bucket:extend(.bucket) {
  // 主体
}
```

### 3.Extend 在规则集中
Extend 可以被放在一个规则集的主体中通过使用 `&:extend(selector)` 语法。将Extend放到主体中就是将其放到该规则集的每个选择器中的快捷方式。


Extend 在一个主体中：

```less
pre:hover,
.some-class {
  &:extend(div pre);
}
```

与每个选择器之后添加Extend完全相同：

```less
pre:hover:extend(div pre),
.some-class:extend(div pre) {}
```

### 4.Extend 嵌套选择器
Extend能够匹配嵌套的选择器。 如下：

例子：

```less
.bucket {
  tr { // 与目标选择器嵌套的规则集
    color: blue;
  }
}
.some-class:extend(.bucket tr) {} // 嵌套的规则集被识别
```
编译为

```css
.bucket tr,
.some-class {
  color: blue;
}
```

Extend本质上接受的是编译后的CSS，而不是原始的Less。

例如:

```less
.bucket {
  tr & { // 与目标选择器嵌套的规则集
    color: blue;
  }
}
.some-class:extend(tr .bucket) {} // 嵌套的规则集被识别
```

编译为

```css
tr .bucket,
.some-class {
  color: blue;
}
```


### 5.Extend 精确匹配
在默认情况下Extend会查找精确匹配到的选择器。 无论选择者器是否使用通配符`*`，或者两个nth表达式是否具有相同的含义，它们都需要具有相同的形式才能被匹配。 唯一的例外是属性选择器中的单引号和双引号，less知道它们具有相同的含义并会匹配它们。

例如:

```less
.a.class,
.class.a,
.class > .a {
  color: blue;
}
.test:extend(.class) {} // 以上选择器都不会被匹配
```

带通配符不会被匹配。 虽然 `*.class` 和 `.class` 是一样的效果, 但是 extend 不会匹配它们:

```less
*.class {
  color: blue;
}
.noStar:extend(.class) {} //  *.class 将不会被匹配
```

编译为

```css
*.class {
  color: blue;
}
```

伪类的顺序很重要。选择器 `link:hover:visited` 和 `link:visited:hover` 会匹配相同的一组元素，但是对于extend，它们是不同的：

```less
link:hover:visited {
  color: blue;
}
.selector:extend(link:visited:hover) {}
```
编译为

```css
link:hover:visited {
  color: blue;
}
```

### 6.nth 表达式

Nth 表达形式很重要。 Nth表达式 `1n+3` 和 `n+3`是一样的效果，但是 extend 不会匹配它们：

```less
:nth-child(1n+3) {
  color: blue;
}
.child:extend(:nth-child(n+3)) {}
```
编译为

```css
:nth-child(1n+3) {
  color: blue;
}
```

在属性选择器中的引号为''或""或为空都没有关系。 以上它们都是一样的效果：

```less
[title=identifier] {
  color: blue;
}
[title='identifier'] {
  color: blue;
}
[title="identifier"] {
  color: blue;
}

.noQuote:extend([title=identifier]) {}
.singleQuote:extend([title='identifier']) {}
.doubleQuote:extend([title="identifier"]) {}
```
编译为

```css
[title=identifier],
.noQuote,
.singleQuote,
.doubleQuote {
  color: blue;
}

[title='identifier'],
.noQuote,
.singleQuote,
.doubleQuote {
  color: blue;
}

[title="identifier"],
.noQuote,
.singleQuote,
.doubleQuote {
  color: blue;
}
```

### 7.Extend "all"

当使用关键字all在extend参数的最后时，Less会将其作为另一个选择器的一部分匹配该选择器。选择器将被复制，只有选择器匹配部分将被替换为extend，同时创建一个新的选择器。

例如：

```less
.a.b.test,
.test.c {
  color: orange;
}
.test {
  &:hover {
    color: green;
  }
}

.replacement:extend(.test all) {}
```

编译为

```css
.a.b.test,
.test.c,
.a.b.replacement,
.replacement.c {
  color: orange;
}
.test:hover,
.replacement:hover {
  color: green;
}
```

_你可以把这种操作模式看作是一个非破坏性的搜索和替换。_


### 8.选择器中插入Extend
> Extend 不能匹配带变量的选择器. 如果选择器中带有变量, 将会被extend忽略。

有一个悬而未决的提案，同时也不会轻易改变。 但是，extend可以附加到被插值的选择器。

带变量的选择器将不会被匹配：

```less
@variable: .bucket;
@{variable} { // 被插值的选择器
  color: blue;
}
.some-class:extend(.bucket) {} // 不会被匹配
```

同时在目标选择器中使用变量也不会被extend匹配到：

```less
.bucket {
  color: blue;
}
.some-class:extend(@{variable}) {} // 被插值的选择器不会被匹配
@variable: .bucket;
```

上述两个例子都编译成：

```less
.bucket {
  color: blue;
}
```

然而, `:extend` 附加到一个被插值得选择器上是可以有效的：

```less
.bucket {
  color: blue;
}
@{variable}:extend(.bucket) {}
@variable: .selector;
```

编译为

```less
.bucket, .selector {
  color: blue;
}
```

### 9.作用于 / 在@media中使用Extend

在媒体查询中声明Extend只匹配同一媒体查询中的选择器：

```less
@media print {
  .screenClass:extend(.selector) {} // 在@media中使用Extend
  .selector { // 会被匹配 - 在同一个媒体查询中
    color: black;
  }
}
.selector { // 规则集在样式表的顶部 - extend会忽略它
  color: red;
}
@media screen {
  .selector {  // 规则集在另一个媒体查询中 - extend会忽略它
    color: blue;
  }
}
```

编译为

```css
@media print {
  .selector,
  .screenClass { /*  在同一个媒体查询中的规则集将会被集成 */
    color: black;
  }
}
.selector { /* 规则集在样式表的顶部被忽略 */
  color: red;
}
@media screen {
  .selector { /* 规则集在另一个媒体查询中被忽略 */
    color: blue;
  }
}
```

Extend在媒体查询中不会匹配媒体查询中嵌套的选择器：

```less
@media screen {
  .screenClass:extend(.selector) {} // extend在媒体查询中
    .selector {  //规则集在媒体查询的嵌套中- extend会忽略它
      color: blue;
    }
  }
}
```

编译为

```css
@media screen and (min-width: 1023px) {
  .selector { /* 规则集在另一个媒体查询的嵌套中，会被忽略 */
    color: blue;
  }
}
```

顶级的extend会匹配包括嵌套在媒体查询内的所有选择器：

```less
@media screen {
  .selector {  /* 规则集嵌套在媒体查询中 - 顶级 extend 有效 */
    color: blue;
  }
  @media (min-width: 1023px) {
    .selector {  /* 规则集嵌套在媒体查询中 - 顶级 extend 有效 */
      color: blue;
    }
  }
}

.topLevel:extend(.selector) {} /* 顶级 extend都会匹配 */
```

编译为

```css
@media screen {
  .selector,
  .topLevel { /* 在媒体查询中的规则集会被继承 */
    color: blue;
  }
}
@media screen and (min-width: 1023px) {
  .selector,
  .topLevel { /* 在媒体查询中的规则集会被继承 */
    color: blue;
  }
}
```

### 10.重复检测

目前没有重复检测。

例如:

```less
.alert-info,
.widget {
  /* 声明 */
}

.alert:extend(.alert-info, .widget) {}
```
编译为

```css
.alert-info,
.widget,
.alert,
.alert {
  /* 声明 */
}
```

### 11.Extend用例

#### 11.1经典用例

其中一个经典用例是避免添加一个基本class。 例如：

```css
.animal {
  background-color: black;
  color: white;
}
```

你希望有一个animal子类型并覆盖其背景颜色，那么你有两个选择，首先，一种是改变你的HTML

```html
<a class="animal bear">Bear</a>
```
```css
.animal {
  background-color: black;
  color: white;
}
.bear {
  background-color: brown;
}
```

或着简化HTML和在Less中使用Extend。 例如：

```html
<a class="bear">Bear</a>
```

```less
.animal {
  background-color: black;
  color: white;
}
.bear {
  &:extend(.animal);
  background-color: brown;
}
```

#### 11.2减少CSS大小

Mixins会将所有的属性复制到一个选择器，这可能会导致不必要的重复。 因此，可以使用Extend替代Mixins，将选择器移动到希望使用的属性上，这样会生成较少的CSS。

例子 - 使用 Mixin：

```less
.my-inline-block() {
  display: inline-block;
  font-size: 0;
}
.thing1 {
  .my-inline-block;
}
.thing2 {
  .my-inline-block;
}
```

输出为

```css
.thing1 {
  display: inline-block;
  font-size: 0;
}
.thing2 {
  display: inline-block;
  font-size: 0;
}
```

例子 - 使用 Extend：

```less
.my-inline-block {
  display: inline-block;
  font-size: 0;
}
.thing1 {
  &:extend(.my-inline-block);
}
.thing2 {
  &:extend(.my-inline-block);
}
```

编译为

```css
.my-inline-block,
.thing1,
.thing2 {
  display: inline-block;
  font-size: 0;
}
```

#### 11.3样式组合 / 更先进的 Mixin

还有一个用例是替代mixin - 因为mixins只能与简单的选择器一起使用，如果你有两个不同的html块，但是需要使用相同的样式，你可以使用extends来关联两个区域。

例如:

```less
li.list > a {
  // list 样式
}
button.list-style {
  &:extend(li.list > a); // 使用同样的 list 样式
}
```
