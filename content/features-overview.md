---
title: Features Overview
---

> Less作为CSS的扩展，不仅向下兼容CSS，而且Less是在现有CSS语法基础上增添的额外特性，这使Less更容易上手，如果有疑问，可以回退的普通的CSS。

### 1.变量

变量看上去就一目了然：

```less
@nice-blue: #5B83AD;
@light-blue: @nice-blue + #111;

#header {
  color: @light-blue;
}
```

输出为:

```css
#header {
  color: #6c94be;
}
```

请注意，变量实际上是“常量”，因为它们只能被定义一次。


### 2.混合

混合是一种将一个规则集中包含（“混入”）一组属性插入到另一个规则集的方法。 比如说有以下class：

```css
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
```

我们希望在其他规则集中使用这些属性。 那么，我们只需要将我们想要放入其中的规则集的class名称放入，就像这样：

```less
#menu a {
  color: #111;
  .bordered;
}

.post a {
  color: red;
  .bordered;
}
```

“.bordered” 这个class的属性现在将出现在“#menu a”和“.post a”中。 （请注意，您也可以使用`＃ids`作为混合。）

**了解更多**

* [更多混合相关](#mixins-feature)
* [参数混合](#mixins-parametric-feature)


### 3.嵌套规则

Less使您能够使用嵌套代替CSS中逐级书写。 假设我们有以下CSS：

```css
#header {
  color: black;
}
#header .navigation {
  font-size: 12px;
}
#header .logo {
  width: 300px;
}
```

我们可以使用Less这样书写：

```less
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}
```

模仿HTML的结构，使代码更加简洁。

你也可以使用这种方法将伪选择器与混合捆绑在一起。这是经典的clearfix hack，用混合重写（`＆`代表当前的父选择器）：

```less
.clearfix {
  display: block;
  zoom: 1;

  &:after {
    content: " ";
    display: block;
    font-size: 0;
    height: 0;
    clear: both;
    visibility: hidden;
  }
}
```

**参见**

* [父选择器](#parent-selectors-feature)

### 4.嵌套指令和冒泡

像“media”或“keyframe”这样的指令可以像选择器一样嵌套。 指令放置在顶部，相对于同一规则集内其他元素的相对顺序保持不变。 这叫做冒泡。

条件指令，例如 `@ Media`，`@ supports`和`@ document`选择器会被复制到它们的主体中：
```less
.screen-color {
  @media screen {
    color: green;
    @media (min-width: 768px) {
      color: red;
    }
  }
  @media tv {
    color: black;
  }
}

```
编译为

```css
@media screen {
  .screen-color {
    color: green;
  }
}
@media screen and (min-width: 768px) {
  .screen-color {
    color: red;
  }
}
@media tv {
  .screen-color {
    color: black;
  }
}
```

其他的非条件指令，例如`font-face`或`keyframes`，也会冒泡。 它们的主体不会改变：
```less
#a {
  color: blue;
  @font-face {
    src: made-up-url;
  }
  padding: 2 2 2 2;
}
```

编译为

```less
#a {
  color: blue;
}
@font-face {
  src: made-up-url;
}
#a {
  padding: 2 2 2 2;
}
```

### 5.运算

算术运算`+`，```，*`，`/`可以在任何数字，颜色或变量上运行。 数学运算会尽可能将单位考虑在内并在加，减或比较之前转换为数字。 结果最左边明确指出的单位类型。 如果是无意义或无效的转换，则单位被忽略。 无法转换的示例：px到cm或rad到％。

```less
// 数字运算被转换成相同的单位
@conversion-1: 5cm + 10mm; // 结果为 6cm
@conversion-2: 2 - 3cm - 5mm; // 结果为 -1.5cm

// 无效的转换
@incompatible-units: 2 + 5px - 3cm; // 结果为 4px

// 带变量的例子
@base: 5%;
@filler: @base * 2; // 结果为 10%
@other: @base + @filler; // 结果为 15%
```

乘法和除法不转换数字。 在大多数情况下，这是没有意义的 - 长度乘以长度会给出一个区域，而css不支持指定区域。 Less将按原样操作数字，并将明确指定的单位类型分配给结果。

```less
@base: 2cm * 3mm; // 结果是 6cm
```

颜色分为红色，绿色，蓝色和alpha值。 该操作分别应用于每个颜色维度。 例如，如果用户添加了两种颜色，则结果的绿色维度等于输入颜色的绿色维度的总和。 如果用户乘以一个数字的颜色，则每个颜色尺寸将相乘。

注意：没有定义对alpha的算术运算，因为对颜色的数学运算没有标准的约定意义。 不要依赖当前的实现，因为它可能会在更高版本中[更改](https://github.com/less/less.js/issues/2694)。

对颜色的操作总是产生有效的颜色。 如果结果的某个颜色尺寸大于`ff`或小于`00`，那么这个尺寸被舍入为`ff`或`00`。 如果alpha结果大于`1.0`或小于`0.0`，则alpha被舍入为`1.0`或`0.0`。

```less
@color: #224488 / 2; //结果为 #112244
background-color: #112244 + #111; // 结果为 #223355
```

### 6.转义

转义允许您使用任何任意字符串作为属性或变量值。 任何`~`或`~`任何内容都可以按照原样使用，除了[插值](#variables-feature-variable-interpolation)之外没有任何变化。

```less
.weird-element {
  content: ~"^//* some horrible but needed css hack";
}
```

results in:
```less
.weird-element {
  content: ^//* some horrible but needed css hack;
}
```

### 7.函数

Less提供变换颜色，操作字符串和数学运算的各种函数。 具体在函数参考表中有完整记录。

使用函数非常简单。 以下示例使用百分比来转换0.5到50％，将基本颜色的饱和度增加5％，然后将背景颜色设置为减少25％并旋转8度的颜色：

```less
@base: #f04615;
@width: 0.5;

.class {
  width: percentage(@width); // 返回 `50%`
  color: saturate(@base, 5%);
  background-color: spin(lighten(@base, 25%), 8);
}
```


### 8.命名空间和访问器

(不要混淆 [CSS `@namespace`](http://www.w3.org/TR/css3-namespace/) 和 [命名空间选择器](http://www.w3.org/TR/css3-selectors/#typenmsp)).

有时，为了组织结构，你可能想要分组你的mixins，或者只是提供一些封装。 你可以在Less中很直观地做到这一点，比如你想在`＃bundle`下捆绑一些mixin和变量，以便以后重用或分发：

```less
#bundle {
  .button {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover {
      background-color: white
    }
  }
  .tab { ... }
  .citation { ... }
}
```

现在，如果我们要在`#header a`中混入`.button`类，我们可以这样做：

```less
#header a {
  color: orange;
  #bundle > .button;
}
```

Note that variables declared within a namespace will be scoped to that namespace only and will not be available outside of the scope via the same syntax that you would use to reference a mixin (`#Namespace > .mixin-name`). So, for example, you can't do the following: (`#Namespace > @this-will-not-work`).

请注意，在名称空间中声明的变量将仅限于该名称空间，当你想引用一个mixin (`#Namespace> .mixin-name`)，相同的语法在作用域之外将会失效。例如，你不能这样使用：(`#Namespace > @this-will-not-work`)。

### 9.作用域

Less中的作用域与编程语言非常相似。 首先在本地查找变量和mixin，如果找不到，编译器将查找父范围，依此类推。

```less
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // 白色
  }
}
```

变量和mixin在使用之前不必声明，所以下面的Less代码和前面的例子是一样的：

```less
@var: red;

#page {
  #header {
    color: @var; // 白色
  }
  @var: white;
}
```

**参见**

* [懒加载](#variables-feature-lazy-loading)


### 10.注释

可以使用块式和行内式注释：

```less
/* 块式风格
  注释 */
@var: red;

// 行内式注释！
@var: white;
```

### 11.导入

导入非常便捷。 你可以导入一个`.less`文件，可以使用其中的所有变量。 默认扩展名为`.less`文件。

```css
@import "library"; // library.less
@import "typo.css";
```
