> 创建循环

在Less中一个mixin可以调用自己。 这种递归mixins结合[Guard Expressions](#mixin-guards-feature)和[Pattern Matching](#mixins-parametric-feature-pattern-matching)，可以用来创建各种迭代/循环结构。

示例：

```less
.loop(@counter) when (@counter > 0) {
  .loop((@counter - 1));    // 下一次迭代
  width: (10px * @counter); // 每次迭代的code
}

div {
  .loop(5); // 启动循环
}
```

编译为

```css
div {
  width: 10px;
  width: 20px;
  width: 30px;
  width: 40px;
  width: 50px;
}
```

使用递归循环生成CSS网格类的一般示例：

```less
.generate-columns(4);

.generate-columns(@n, @i: 1) when (@i =< @n) {
  .column-@{i} {
    width: (@i * 100% / @n);
  }
  .generate-columns(@n, (@i + 1));
}
```

编译为

```css
.column-1 {
  width: 25%;
}
.column-2 {
  width: 50%;
}
.column-3 {
  width: 75%;
}
.column-4 {
  width: 100%;
}
```
