> 组合属性

`merge`功能允许将多个属性的值汇总到单个属性下的逗号或空格分隔列表中。 `merge`对于诸如背景和变换之类的属性非常有用的。


### 1.逗号

> 用逗号追加属性值

发布 [v1.5.0]({{ less.master.url }}CHANGELOG.md)

示例：

```less
.mixin() {
  box-shadow+: inset 0 0 10px #555;
}
.myclass {
  .mixin();
  box-shadow+: 0 0 20px black;
}
```
编译为

```css
.myclass {
  box-shadow: inset 0 0 10px #555, 0 0 20px black;
}
```

### 2.空格

> 用空格追加属性值

发布 [v1.7.0]({{ less.master.url }}CHANGELOG.md)

示例：

```less
.mixin() {
  transform+_: scale(2);
}
.myclass {
  .mixin();
  transform+_: rotate(15deg);
}
```
编译为

```css
.myclass {
  transform: scale(2) rotate(15deg);
}
```

为了避免任何不必要的连接，`merge`需要在每个连接等待声明中都显式地使用 `+`'或`+_`标志。