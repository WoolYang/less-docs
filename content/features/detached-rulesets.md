> 允许在mixin中定义一个包裹的css块

发布 [v1.7.0]({{ less.master.url }}CHANGELOG.md)

分离的规则集是一组css属性，嵌套的规则集，媒体查询或存储在变量中的其他内容。 你可以将它包含到一个规则集或其他结构中，它所有的属性都将被复制到过去。 你也可以用它作为一个mixin参数，并把它作为任何其他变量传递。

例子：
````less
// 声明分离的规则集
@detached-ruleset: { background: red; };

// 使用分离的规则集
.top {
    @detached-ruleset(); 
}
````

编译为：
````css
.top {
  background: red;
}
````

调用分离的规则集必需使用括号。 不能用`@ detached-ruleset;`调用。

当你想要定义一个mixin来抽象出在媒体查询中包含的一段代码或者不支持的浏览器类名时， 规则集可以传递给mixin，以便mixin可以包裹内容。

```less
.desktop-and-old-ie(@rules) {
  @media screen and (min-width: 1200px) { @rules(); }
  html.lt-ie9 &                         { @rules(); }
}

header {
  background-color: blue;

  .desktop-and-old-ie({
    background-color: red;
  });
}
```

这里`desktop-and-old-ie` mixin定义了媒体查询和根class，这样你就可以使用一个mixin来包裹一段代码。编译为

```css
header {
  background-color: blue;
}
@media screen and (min-width: 1200px) {
  header {
    background-color: red;
  }
}
html.lt-ie9 header {
  background-color: red;
}
```

一个规则集可以立刻分配给一个变量，或者被传递给一个混合，并且可以包含一整套Less特征，

```less
@my-ruleset: {
    .my-selector {
      background-color: black;
    }
  };
```

你甚至可以使用 [媒体查询冒泡](#features-overview-feature-media-query-bubbling-and-nested-media-queries)， 例如
```less
@my-ruleset: {
    .my-selector {
      @media tv {
        background-color: black;
      }
    }
  };
@media (orientation:portrait) {
    @my-ruleset();
}
```

编译为

```css
@media (orientation: portrait) and tv {
  .my-selector {
    background-color: black;
  }
}
```

分离的规则集调用解锁（返回）与mixin调用将其所有mixin解锁（调用）到调用者中是相同的方式。 但是，它不返回变量。

返回 mixin:
````less
// 带mixin的分离规则集
@detached-ruleset: { 
    .mixin() {
        color:blue;
    }
};
// 调用分离的规则集
.caller {
    @detached-ruleset(); 
    .mixin();
}
````

编译为：
````css
.caller {
  color: blue;
}
````

私有变量：
````less
@detached-ruleset: { 
    @color:blue; //这是个私有变量
};
.caller {
    color: @color; // 语法报错
}
````

## 1.作用域
一个分离的规则集可以使用所有的变量和mixin，它们在*被定义*和*被调用*处被访问。即定义和调用者作用域都可用。 如果两个作用域都包含相同的变量或mixin，则当前声明的作用域中的值优先。

*声明作用域*被定义为分离规则主题。将分离规则集从一个变量复制到另一个变量时，不能修改其范围。 规则集不能仅通过在那里引用就可访问新的作用域。

最后，分离的规则集可以通过解锁（导入）的方式来访问作用域。

#### 1-1.定义和调用作用域可见性
分离的规则集可以看到调用者的变量和mixin：

````less
@detached-ruleset: {
  caller-variable: @caller-variable; // 变量在这里未定义
  .caller-mixin(); // mixin在这里未定义
};

selector {
  // 使用分离规则集
  @detached-ruleset(); 

  // 在分离的规则集内定义需要的变量和mixin
  @caller-variable: value;
  .caller-mixin() {
    variable: declaration;
  }
}
````

编译为
````css
selector {
  caller-variable: value;
  variable: declaration;
}
````

变量和mixins可访问到的外部定义优先于调用者中可用的定义：

````less
@variable: global;
@detached-ruleset: {
  // 使用全局变量，因为其可以访问
  // 从分离规则集定义
  variable: @variable; 
};

selector {
  @detached-ruleset();
  @variable: value; // 在调用者中定义的变量将被忽略
}
````

编译为：
````css
selector {
  variable: global;
}
````

#### 1-2.引用*不会*修改分离规则集的作用域
规则集不能仅仅通过引用而访问新的作用域：
````less
@detached-1: { scope-detached: @one @two; };
.one {
  @one: visible;
  .two {
    @detached-2: @detached-1; // 复制/重命名规则集
    @two: visible; // 规则集不能访问这个变量
  }
}

.use-place {
  .one > .two(); 
  @detached-2();
}
````

报错：
````
ERROR 1:32 The variable "@one" was not declared.
````

#### 1-3.解锁*将*修改分离规则集的作用域
分离的规则集通过在范围内解锁（导入）来获得访问权限：
````less
#space {
  .importer-1() {
    @detached: { scope-detached: @variable; }; // 定义分离规则集
  }
}

.importer-2() {
  @variable: value; // 解锁分离的规则集CAN可以看到这个变量
  #space > .importer-1(); // 解锁/导入分离的规则集
}

.use-place {
  .importer-2(); // 再次解锁/导入分离的规则集
   @detached();
}
````

编译为：
````css
.use-place {
  scope-detached: value;
}
````
