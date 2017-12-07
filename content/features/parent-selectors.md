> 用`&`引用父选择器

`&`运算符表示[嵌套规则](#features-overview-feature-nested-rules)中的父选择器，常用于在已存在选择器上修改类或伪类。

```less
a {
  color: blue;
  &:hover {
    color: green;
  }
}
```

编译为：

```css
a {
  color: blue;
}

a:hover {
  color: green;
}
```

注意，如果没有 `&`，上面的例子会导致`a：hover`规则（匹配`<a>`标签内的悬停元素的后代选择器），嵌套`：hover`不是我们通常想要的。

“父选择器”操作符有多种用途。 基本上任何时候，嵌套规则的选择器需要以其他非默认的方式进行组合。 例如 `&`的另一个典型用法是产生重复的类名：

```less
.button {
  &-ok {
    background-image: url("ok.png");
  }
  &-cancel {
    background-image: url("cancel.png");
  }

  &-custom {
    background-image: url("custom.png");
  }
}
```

编译为

```css
.button-ok {
  background-image: url("ok.png");
}
.button-cancel {
  background-image: url("cancel.png");
}
.button-custom {
  background-image: url("custom.png");
}
```

### 1.复合 `&`

`&`可能在选择器中多次出现。 可以实现多次引用父选择器名称而不用重复书写其名称

```less
.link {
  & + & {
    color: red;
  }

  & & {
    color: green;
  }

  && {
    color: blue;
  }

  &, &ish {
    color: cyan;
  }
}
```

编译为

```css
.link + .link {
  color: red;
}
.link .link {
  color: green;
}
.link.link {
  color: blue;
}
.link, .linkish {
  color: cyan;
}
```


请注意`&` 代表所有的父选择器（不只是最近的祖先），所以下面的例子：

```less
.grand {
  .parent {
    & > & {
      color: red;
    }

    & & {
      color: green;
    }

    && {
      color: blue;
    }

    &, &ish {
      color: cyan;
    }
  }
}
```

编译为：

```css
.grand .parent > .grand .parent {
  color: red;
}
.grand .parent .grand .parent {
  color: green;
}
.grand .parent.grand .parent {
  color: blue;
}
.grand .parent,
.grand .parentish {
  color: cyan;
}
```


### 2.改变选择器顺序

将选择器预先添加到继承的（父）选择器可能很有用。 可以通过在当前选择器之后放置 `&` 来完成。
例如，使用Modernizr库时，可能需要根据支持的功能指定不同的规则：

```less
.header {
  .menu {
    border-radius: 5px;
    .no-borderradius & {
      background-image: url('images/button-background.png');
    }
  }
}
```

`.no-borderradius &`选择器会将`.no-borderradius`添加到其父级`.header .menu`中，以`.no-borderradius .header .menu`形式输出：

```css
.header .menu {
  border-radius: 5px;
}
.no-borderradius .header .menu {
  background-image: url('images/button-background.png');
}
```


### 3.超级组合

`&` 也可以用来在逗号分隔列表中生成每个选择器可能的排列组合：

```less
p, a, ul, li {
  border-top: 2px dotted #366;
  & + & {
    border-top: 0;
  }
}
```

这种会扩展指定元素所有可能的（16）的组合：

```css
p,
a,
ul,
li {
  border-top: 2px dotted #366;
}
p + p,
p + a,
p + ul,
p + li,
a + p,
a + a,
a + ul,
a + li,
ul + p,
ul + a,
ul + ul,
ul + li,
li + p,
li + a,
li + ul,
li + li {
  border-top: 0;
}
```
