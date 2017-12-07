> 从其他样式表导入样式

在标准CSS中，`@import` 规则必须在所有其他类型规则之前。 但 不关心你把`@ import`语句放在哪里。

例如：

```less
.foo {
  background: #900;
}
@import "this-is-valid.less";
```

## 1.文件扩展名
`@import` statements may be treated differently by Less depending on the file extension:

根据Less文件扩展名的不同，会用不同方式处理`@import`语句：

* 如果文件具有`.css`扩展名，那么它将被视为CSS并且`@ import`语句保持原样 (参见 [内联选项](#import-options-inline) below).
* 如果为_其他扩展名_它将被视为Less文件并被导入。
* 如果没有扩展名，则会追加“.less”，并将其作为Less文件导入。

例如：

```less
@import "foo";      // foo.less 被导入
@import "foo.less"; // foo.less 被导入
@import "foo.php";  // foo.php 作为Less文件被导入
@import "foo.css";  // 保留原本的导入声明
```

以下配置项可用于覆盖此行为。

# import配置项
> Less offers several extensions to the CSS `@import` CSS at-rule to provide more flexibility over what you can do with external files.

Less提供了几个CSS`@import`规则扩展项，在使用外部文件时提供更多的灵活性。

语法: `@import (keyword) "filename";`

以下导入指令已经被实现：

* `reference`: 使用一个Less文件，但不输出它
* `inline`: 在输出中包含源文件，但不处理它
* `less`: 将文件视为Less文件，而不管文件扩展名是什么
* `css`: 将文件视为css文件，而不管文件扩展名是什么
* `once`: 只包含文件一次（默认行为）
* `multiple`: 包含文件多次
* `optional`: 当找不到文件时继续编译

> 每个`@import`允许多个关键字，您必须使用逗号分隔关键字：

示例： `@import (optional, reference) "foo.less";`

## 1.reference
> 使用`@import (reference)）`导入外部文件，但不会将导入的样式添加到编译后的输出中，除非被引用。



发布 [v1.5.0]({{ less.master.url }}CHANGELOG.md)

示例： `@import (reference) "foo.less";`

想象一下，`reference`在导入的文件中用_reference flag_标记每个指令和选择器，正常导入，但是当生成CSS时，不会输出“引用”选择器（以及任何只包含引用选择器的媒体查询）。 除非使用引用样式，`reference`样式不会显示在生成的CSS中作为 [mixins](#mixins-feature) 或 [继承](#extend-feature).


此外，**`reference`**根据不同使用方法（mixin或extend）产生不同的结果：

* **[继承](#extend-feature)**： 当一个选择器继承时，只有新的选择器被标记为_not referenced_，并且出现在引入`@ import`语句的位置。
* **[mixins](#mixins-feature)**： 当`reference`样式被用作[隐式mixin](#mixins-feature)时，它的规则被混入，标记为“not reference”，并以正常的方式出现在引用的位置。



### 1-1.reference 示例
这样就可以通过执行如下操作从库[Bootstrap] [Bootstrap](https://github.com/twbs/bootstrap)中只引用特定的目标样式：

```less
.navbar:extend(.navbar all) {}
```

你将只从Bootstrap中提取`.navbar`相关的样式。


## 2.inline
> 使用 `@import (inline)`输出中包含外部文件，但不处理它们。



发布 [v1.5.0]({{ less.master.url }}CHANGELOG.md)

示例: `@import (inline) "not-less-compatible.css";`

当CSS文件可能不兼容Less时，你会使用这个; 虽然Less支持大多数已知的标准CSS，但它不支持某些地方的注释，并且不支持所有已知的CSS hacks。

所以你可以使用这个在输出中包含这个文件，这样所有的CSS都会在一个文件中。


## 3.less
> 忽略文件扩展名，使用`@import (less)`将导入的文件视为Less。



发布 [v1.4.0]({{ less.master.url }}CHANGELOG.md)

示例：

```less
@import (less) "foo.css";
```


## 4.css
> 使用`@import（css）`将输入的文件视为普通的CSS，而忽略文件的扩展名。 这意味着导入声明将保持原样。



发布 [v1.4.0]({{ less.master.url }}CHANGELOG.md)

示例：

```less
@import (css) "foo.less";
```
编译为

```less
@import "foo.less";
```


## 5.once
> `@import`语句的默认行为。 这意味着该文件只导入一次，该文件的后续import语句将被忽略。


发布 [v1.4.0]({{ less.master.url }}CHANGELOG.md)

这是`@import`语句的默认行为。

示例：

```less
@import (once) "foo.less";
@import (once) "foo.less"; // 此声明将会被忽略
```


## 6.multiple
> 使用`@import (multiple)` 来导入多个具有相同名称的文件。 与once的行为相反。



发布 [v1.4.0]({{ less.master.url }}CHANGELOG.md)

示例：

```less
// 文件: foo.less
.a {
  color: green;
}
// 文件: main.less
@import (multiple) "foo.less";
@import (multiple) "foo.less";
```
编译为

```less
.a {
  color: green;
}
.a {
  color: green;
}
```

## 7.optional
> 使用`@import (optional)`只允许在文件存在的情况下导入文件。 如果没有`optional`关键字Less引发FileError并在导入无法找到的文件时停止编译。

发布 [v2.3.0]({{ less.master.url }}CHANGELOG.md)
