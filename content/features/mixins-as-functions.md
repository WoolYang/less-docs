> 从mixin返回变量或mixin

在mixin中定义的变量和mixin是可见的，可用于调用者的作用域。 只有一个例外，如果调用者包含相同名称的变量（包括在另一个mixin调用中定义的变量）时，变量不会被复制。 只有调用者本地作用域中的变量才受保护。 从父范围继承的变量会被覆盖。

例子：

```less
.mixin() {
  @width:  100%;
  @height: 200px;
}

.caller {
  .mixin();
  width:  @width;
  height: @height;
}

```
编译为

```css
.caller {
  width:  100%;
  height: 200px;
}
```

Thus variables defined in a mixin can act as its return values. This allows us to create a mixin that can be used almost like a function.

因此，在mixin中定义的变量可以作为其返回值。 这使我们可以像创建函数一样创建一个mixin。

例子：

```less
.average(@x, @y) {
  @average: ((@x + @y) / 2);
}

div {
  .average(16px, 50px); // "调用" mixin
  padding: @average;    // 使用其"返回"值
}
```

编译为

```css
div {
  padding: 33px;
}
```

在当前作用域中定义的变量不能被覆盖。 但是，在其父作用域中定义的变量不受保护并会被覆盖：

````less
.mixin() {
  @size: in-mixin;
  @definedOnlyInMixin: in-mixin;
}

.class {
  margin: @size @definedOnlyInMixin;
  .mixin();
}

@size: globaly-defined-value; // 调用者父作用域 - 不受保护
````

编译为
````css
.class {
  margin: in-mixin in-mixin;
}
````

最后，mixin中定义的mixin也作为返回值：

````less
.unlock(@value) { // 外层 mixin
  .doSomething() { // 嵌套 mixin
    declaration: @value;
  }
}

#namespace {
  .unlock(5); // unlock doSomething mixin
  .doSomething(); //嵌套的mixin被复制到这里，可用
}
````

编译为：
````css
#namespace {
  declaration: 5;
}
````
