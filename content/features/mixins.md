> 从现有样式的“混入”属性

您可以混入类选择器和ID选择器，例如

```less
.a, #b {
  color: red;
}
.mixin-class {
  .a();
}
.mixin-id {
  #b();
}
```
编译为
```css
.a, #b {
  color: red;
}
.mixin-class {
  color: red;
}
.mixin-id {
  color: red;
}
```

请注意，当您调用mixin时，括号是可选的。

```less
// 这两个写法效果相同：
.a(); 
.a;
```

## 1.不输出Mixin

如果你想创建一个mixin，但你不想要输出mixin本身，你可以在后面加上括号。

```less
.my-mixin {
  color: black;
}
.my-other-mixin() {
  background: white;
}
.class {
  .my-mixin;
  .my-other-mixin;
}
```
编译为

```css
.my-mixin {
  color: black;
}
.class {
  color: black;
  background: white;
}
```

## 2.Mixins中的选择器

Mixins不仅可以包含属性，还可以包含选择器。

例子：

```less
.my-hover-mixin() {
  &:hover {
    border: 1px solid red;
  }
}
button {
  .my-hover-mixin();
}
```

编译为

```css
button:hover {
  border: 1px solid red;
}
```

## 3.命名空间

如果你想在更复杂的选择器中混入属性，你可以堆叠多个id或类。

```less
#outer {
  .inner {
    color: red;
  }
}

.c {
  #outer > .inner;
}
```

 `>` 和空格是可选的

```less
// 全部都是同样的效果
#outer > .inner;
#outer > .inner();
#outer .inner;
#outer .inner();
#outer.inner;
#outer.inner();
```

这种使用称为命名空间。 你可以把你的mixin放在一个id选择器下，这样可以确保它不会和另一个库冲突。

例子：

```less
#my-library {
  .my-mixin() {
    color: black;
  }
}
// 可以这样使用
.class {
  #my-library > .my-mixin();
}
```

## 4.命名空间警戒

如果名称空间有一个警戒，那么只有在警戒条件返回true的情况下才能使用定义的mixins。 命名空间警戒的评估方式与mixin上的警戒完全相同，所以接下来的两个mixin的工作方式相同：

```less
#namespace when (@mode=huge) {
  .mixin() { /* */ }
}

#namespace {
  .mixin() when (@mode=huge) { /* */ }
}
```

对于所有嵌套的命名空间和mixin，`default`函数被假定为相同的值。 mixin从来没有被评估过，其中一个警戒保证是假的：

```less
#sp_1 when (default()) {
  #sp_2 when (default()) {
    .mixin() when not(default()) { /* */ }
  }
}
```

## 5.`!important` 关键词

mixin调用后使用`！important`关键字来标记所有由它继承的属性为：`!important`：

例子：

```less
.foo (@bg: #f5f5f5, @color: #900) {
  background: @bg;
  color: @color;
}
.unimportant {
  .foo();
}
.important {
  .foo() !important;
}
```

编译为

```css
.unimportant {
  background: #f5f5f5;
  color: #900;
}
.important {
  background: #f5f5f5 !important;
  color: #900 !important;
}
```
