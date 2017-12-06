> 如何将参数传递给mixin

Mixins也可以使用参数，这些参数是在混入时传递给选择器块的变量。

例子：

```less
.border-radius(@radius) {
  -webkit-border-radius: @radius;
     -moz-border-radius: @radius;
          border-radius: @radius;
}
```

下是我们如何将其混入到各种规则集中：

```less
#header {
  .border-radius(4px);
}
.button {
  .border-radius(6px);
}
```

mixin参数也可以设定默认值：

```less
.border-radius(@radius: 5px) {
  -webkit-border-radius: @radius;
     -moz-border-radius: @radius;
          border-radius: @radius;
}
```

我们现在可以像这样调用它：

```less
#header {
  .border-radius;
}
```

border-radius将会为5px。

你也可以使用不带参数的参数mixins。 如果您希望从CSS输出中隐藏规则集，但希望将其属性包含在其他规则集中，则这非常有用：

```less
.wrap() {
  text-wrap: wrap;
  white-space: -moz-pre-wrap;
  white-space: pre-wrap;
  word-wrap: break-word;
}

pre { .wrap }
```

编译为：

```css
pre {
  text-wrap: wrap;
  white-space: -moz-pre-wrap;
  white-space: pre-wrap;
  word-wrap: break-word;
}
```

### 1.多参数Mixins
参数是*分号*或*逗号*分隔。 建议使用*分号*。 逗号有双重含义：它可以被解释为混合参数分隔符或CSS列表分隔符。

使用逗号作为mixin分隔符使得不能再用逗号作为参数创建分隔列表。 另一方面，如果编译器在mixin调用或声明中看到至少一个分号，则假定参数以分号分隔，并且所有逗号都属于css列表：

* 两个参数，每个都包含逗号分隔列表： `.name(1, 2, 3; something, else)`,
* 三个参数，每个包含一个数字： `.name(1, 2, 3)`,
* 使用分号模拟创建一个包含逗号分隔的CSS列表参数的mixin调用: `.name(1, 2, 3;)`,
* 逗号分隔默认值: `.name(@param1: red, blue;)`.

定义多个具有相同名称和参数的mixin是合法的。 Less将使用所有可以用属性。 如果你用了一个参数的mixin `.mixin（green）;`，那么将使用具有一个必需参数属性的mixin：

```less
.mixin(@color) {
  color-1: @color;
}
.mixin(@color; @padding: 2) {
  color-2: @color;
  padding-2: @padding;
}
.mixin(@color; @padding; @margin: 2) {
  color-3: @color;
  padding-3: @padding;
  margin: @margin @margin @margin @margin;
}
.some .selector div {
  .mixin(#008000);
}
```
编译为

```css
.some .selector div {
  color-1: #008000;
  color-2: #008000;
  padding-2: 2;
}
```

### 2.命名参数

一个mixin引用可以通过它们的名字来提供参数值，而不仅仅是位置。 任何参数都可以通过它的名字来引用，而且它们不需要特殊的顺序：

```less
.mixin(@color: black; @margin: 10px; @padding: 20px) {
  color: @color;
  margin: @margin;
  padding: @padding;
}
.class1 {
  .mixin(@margin: 20px; @color: #33acfe);
}
.class2 {
  .mixin(#efca44; @padding: 40px);
}
```
编译为

```css
.class1 {
  color: #33acfe;
  margin: 20px;
  padding: 20px;
}
.class2 {
  color: #efca44;
  margin: 10px;
  padding: 40px;
}
```

### 3. `@arguments` 变量

`@arguments` 在mixin中有一个特殊的含义，它包含所有传入的参数，当mixin被调用时。 如果您不想处理个别参数，这很有用：

```less
.box-shadow(@x: 0; @y: 0; @blur: 1px; @color: #000) {
  -webkit-box-shadow: @arguments;
     -moz-box-shadow: @arguments;
          box-shadow: @arguments;
}
.big-block {
  .box-shadow(2px; 5px);
}
```

编译为：

```css
.big-block {
  -webkit-box-shadow: 2px 5px 1px #000;
     -moz-box-shadow: 2px 5px 1px #000;
          box-shadow: 2px 5px 1px #000;
}
```

### 4.高级参数和`@rest`变量

如果你想让你的mixin获取可变数量的参数，你可以使用`...`。 在变量名后使用这个参数将把这些参数赋值给变量。

```less
.mixin(...) {        // 匹配0-N个参数
.mixin() {           // 精确匹配0个参数
.mixin(@a: 1) {      // 匹配0-1个参数
.mixin(@a: 1; ...) { // 匹配0-N个参数
.mixin(@a; ...) {    // 匹配1-N个参数
```

此外:

```less
.mixin(@a; @rest...) {
   // @在@a之后，@rest被绑定到参数上
   // @arguments 会接受所有参数
}
```

## 5.模式匹配

有时，可能想要根据传递给它的参数来改变mixin的行为。 让我们从基本的东西开始：

```less
.mixin(@s; @color) { ... }

.class {
  .mixin(@switch; #888);
}
```

现在，我们希望`.mixin`的行为不同，基于`@ switch`的值，我们可以这样定义`.mixin`：

```less
.mixin(dark; @color) {
  color: darken(@color, 10%);
}
.mixin(light; @color) {
  color: lighten(@color, 10%);
}
.mixin(@_; @color) {
  display: block;
}
```

现在，如果运行：

```less
@switch: light;

.class {
  .mixin(@switch; #888);
}
```

编译为：

```css
.class {
  color: #a2a2a2;
  display: block;
}
```

传到`.mixin`的颜色变浅了。 如果`@ switch`的值是`dark`，结果将是一个更深的颜色。

以下是发生的事情：

* 第一个mixin的定义不匹配，因为它预计`dark`是第一个参数。
* 第二个mixin的定义匹配，因为它期望`light`。
* 第三个mixin的定义匹配，因为它预期任意值。

只有mixin匹配的结果被使用。 变量会匹配并绑定到任意值。<!-- 此句翻译有歧义 -->
除变量以外的任何内容只与等于它自己的值相匹配。

同样可以根据参数数量匹配，例如：

```less
.mixin(@a) {
  color: @a;
}
.mixin(@a; @b) {
  color: fade(@a; @b);
}
```

现在，如果我们用一个参数调用`.mixin`，我们将得到第一个定义的输出，
但是如果我们用*2个*参数来调用它，我们会得到第二个定义，即`@ a`渐变为`@ b`。
