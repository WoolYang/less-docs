---
title: Variables
---

> 将常用的属性值提取出来。

### 1.概述

在样式表中看到相同的属性值重复许多次_甚至上百次_，这并不是罕见的情况：

```css
a,
.link {
  color: #428bca;
}
.widget {
  color: #fff;
  background: #428bca;
}
```

使用变量将这些属性值提取到一个地方会使代码变得更易于维护：

```less
// 定义变量
@link-color:        #428bca; // 海蓝色
@link-color-hover:  darken(@link-color, 10%);

// 使用方法
a,
.link {
  color: @link-color;
}
a:hover {
  color: @link-color-hover;
}
.widget {
  color: #fff;
  background: @link-color;
}
```

### 2.变量插补

上面例子中，着重于使用变量提取_CSS规则中的属性值_，其实变量也可以在其他地方使用，例如选择器名称，URL，以及“@ import”语句。

#### 2-1.选择器

Version: 1.4.0

```less
// 定义变量
@my-selector: banner;

// 使用方法
.@{my-selector} {
  font-weight: bold;
  line-height: 40px;
  margin: 0 auto;
}
```
编译为：

```css
.banner {
  font-weight: bold;
  line-height: 40px;
  margin: 0 auto;
}
```

### 3.URLs

```less
// 定义变量
@images: "../img";

// 使用方法
body {
  color: #444;
  background: url("@{images}/white-sand.png");
}
```

#### 3-1.声明import

Version: 1.4.0

语法： `@import "@{themes}/tidal-wave.less";`

请注意，在v2.0.0之前，只允许在根目录或当前作用域内声明的变量，并且只能在当前文件和调用文件中查找变量。

例如:

```less
// 定义变量
@themes: "../../src/themes";

// 使用方法
@import "@{themes}/tidal-wave.less";
```

#### 3-2.属性

Version: 1.6.0

```less
@property: color;

.widget {
  @{property}: #0ee;
  background-@{property}: #999;
}
```

编译为：

```css
.widget {
  color: #0ee;
  background-color: #999;
}
```

### 4.变量名定义

也可以使用一个变量来定义变量名称：

```less
@fnord:  "I am fnord.";
@var:    "fnord";
content: @@var;
```

编译为：

```
content: "I am fnord.";
```

<span class="anchor-target" id="variables-feature-lazy-loading"></span>
<!-- ^ please keep old anchor to not break zillion outer links -->
### 5.惰性加载

> 变量可以惰性加载，不必在使用之前进行声明。

合法的Less语法：

```less
.lazy-eval {
  width: @var;
}

@var: @a;
@a: 9%;
```
同样合法:

```less
.lazy-eval-scope {
  width: @var;
  @a: 9%;
}

@var: @a;
@a: 100%;
```
都会被编译为：

```css
.lazy-eval-scope {
  width: 9%;
}
```

当同一个变量被定义两次，第二次定义的变量将会生效，并从当前作用域开始向上查找，这与CSS中定义的最后一个属性值会被使用的规则类似。
例如：

```less
@var: 0;
.class {
  @var: 1;
  .brass {
    @var: 2;
    three: @var;
    @var: 3;
  }
  one: @var;
}
```
编译为：

```css
.class {
  one: 1;
}
.class .brass {
  three: 3;
}
```

### 6.变量默认值

我们有时候会需要设置变量默认值 - 只有变量在尚未使用的情况下才能设置。 此功能不是必需的，因为你可以地通过之后的定义来覆盖默认变量。

For instance:

```less
// library
@base-color: green;
@dark-color: darken(@base-color, 10%);

// 使用library
@import "library.less";
@base-color: red;
```

其原作机制是因为 [惰性加载](#variables-feature-lazy-loading) - base-color 会被覆盖 and dark-color 将会是深红色。
