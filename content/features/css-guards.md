> "if"在选择器周围

发布 [v1.5.0]({{ less.master.url }}CHANGELOG.md)

Guard 也可以应用于CSS选择器， 这是用于声明mixin的语法糖，然后立即调用它。

例如，在1.5.0之前，你将不得不这样做：

```less
.my-optional-style() when (@my-option = true) {
  button {
    color: white;
  }
}
.my-optional-style();
```

现在，可以直接将guard应用于某种样式。

```less
button when (@my-option = true) {
  color: white;
}
```

也可以通过与`&`功能结合起来来实现一个`if`类型的语句，允许您将多个guard分组。

```less
& when (@my-option = true) {
  button {
    color: white;
  }
  a {
    color: blue;
  }
}
```
