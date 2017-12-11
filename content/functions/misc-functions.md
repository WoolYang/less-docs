### color

> 解析颜色，将表示颜色的字符串转换为颜色值。

参数: `string`:表示颜色值的字符串。

返回值: `color`

示例: `color("#aaa");`

编译为: `#aaa`

### image-size

> 从文件中获取图像尺寸。

参数: `string`: 所需获取尺寸的文件。

返回值: `dimension`

示例: `image-size("file.png");`

编译为: `10px 10px`

注意：这个功能需要由每个环境来实现。 它目前仅在node环境中可用。

添加版本: v2.2.0

### image-width

> 从文件中获取图像宽度。

参数: `string`: 所需获取尺寸的文件。

返回值: `dimension`

示例: `image-width("file.png");`

编译为: `10px`

注意：这个功能需要由每个环境来实现。 它目前仅在node环境中可用。

添加版本: v2.2.0

### image-height

>从文件中获取图像高度。

参数: `string`: 所需获取尺寸的文件。

返回值: `dimension`

示例: `image-height("file.png");`

编译为: `10px`

注意：这个功能需要由每个环境来实现。 它目前仅在node环境中可用。

添加版本: v2.2.0

### convert

> 将数字从一个单位转换为另一个单位。

第一个参数为一个带单位的数字，第二个参数为单位。 如果这些单位是兼容的，则数字被转换。 如果它们不兼容，则第一个参数将不被修改。

参见 [单位](#misc-functions-unit)不做转换的情况下改变单位。

_可转换的单位_:

* 长度: `m`, `cm`, `mm`, `in`, `pt`, `pc`,
* 时间: `s`, `ms`,
* 角度: `rad`, `deg`, `grad`, `turn`.

参数:
* `number`: 带单位的浮点数。
* `identifier`, `string`或者 `escaped value`: 单位

返回值: `number`

示例:

```less
convert(9s, "ms")
convert(14cm, mm)
convert(8, mm) // 不可转换的单位类型
```

编译为:

```
9000ms
140mm
8
```


### data-uri

> 如果用ieCompat选项打开并且资源太大，或者如果在浏览器中使用该函数，则将资源内联进样式表并返回到`url()`。 如果没有指定MIME类型，node将使用MIME包来决定正确的MIME类型。

参数:
* `mimetype`: （可选）MIME类型字符串。
* `url`: 要内联的文件的URL。

如果没有指定mimetype，data-uri函数会从文件名后缀中猜出来。 Text文本和SVG文件按照utf-8编码，其他都会按base64编码。

如果提供了mimetype，且mimetype参数以base64结尾，则函数使用base64。 例如，`image / jpeg; base64`按照编base64码，而`text / html`按照utf-8编码。

示例: `data-uri('../data/image.jpg');`

编译为: `url('data:image/jpeg;base64,bm90IGFjdHVhbGx5IGEganBlZyBmaWxlCg==');`

浏览器中编译为: `url('../data/image.jpg');`

示例: `data-uri('image/jpeg;base64', '../data/image.jpg');`

编译为: `url('data:image/jpeg;base64,bm90IGFjdHVhbGx5IGEganBlZyBmaWxlCg==');`

示例: `data-uri('image/svg+xml;charset=UTF-8', 'image.svg');`

编译为: `url("data:image/svg+xml;charset=UTF-8,%3Csvg%3E%3Ccircle%20r%3D%229%22%2F%3E%3C%2Fsvg%3E");`

### default

> 仅可在guard条件下使用，在没有其他mixin匹配的情况下返回“true”，否则返回“false”。

示例：

```less
.mixin(1)                   {x: 11}
.mixin(2)                   {y: 22}
.mixin(@x) when (default()) {z: @x}

div {
  .mixin(3);
}

div.special {
  .mixin(1);
}
```
编译为：

```css
div {
  z: 3;
}
div.special {
  x: 11;
}
```

It is possible to use the value returned by `default` with guard operators. For example `.mixin() when not(default()) {}` will match only if there's at least one more mixin definition that matches`.mixin()` call:

可以使用`default`返回的值给guard操作符。 例如`.mixin() when not(default()) {}`只会在匹配至少一个`.mixin()`mixin时定义才会调用：

```less
.mixin(@value) when (ispixel(@value)) {width: @value}
.mixin(@value) when not(default())    {padding: (@value / 5)}

div-1 {
  .mixin(100px);
}

div-2 {
  /* ... */
  .mixin(100%);
}
```
编译为：

```css
div-1 {
  width: 100px;
  padding: 20px;
}
div-2 {
  /* ... */
}
```

允许在相同的guard条件下或在具有相同名称的mixins的不同条件下进行多个`default()`调用：

```less
div {
  .m(@x) when (default()), not(default())    {always: @x}
  .m(@x) when (default()) and not(default()) {never:  @x}

  .m(1); // OK
}
```
然而，如果使用`default()`检测到多个mixin定义之间的*潜在*冲突，Less会抛出错误：

```less
div {
  .m(@x) when (default())    {}
  .m(@x) when not(default()) {}

  .m(1); // 报错
}
```
在上面的例子中，因为它们递归地相互依赖，不能确定每个`default()`调用应该返回什么值。

多重`default()` 高级用法：

```less
.x {
  .m(red)                                    {case-1: darkred}
  .m(blue)                                   {case-2: darkblue}
  .m(@x) when (iscolor(@x)) and (default())  {default-color: @x}
  .m('foo')                                  {case-1: I am 'foo'}
  .m('bar')                                  {case-2: I am 'bar'}
  .m(@x) when (isstring(@x)) and (default()) {default-string: and I am the default}

  &-blue  {.m(blue)}
  &-green {.m(green)}
  &-foo   {.m('foo')}
  &-baz   {.m('baz')}
}
```
编译为：

```css
.x-blue {
  case-2: #00008b;
}
.x-green {
  default-color: #008000;
}
.x-foo {
  case-1: I am 'foo';
}
.x-baz {
  default-string: and I am the default;
}
```

`default`函数作为Less内置函数_只能在guard表达式中使用_。 如果在mixin guard条件之外使用，则将其解释为常规CSS值：

示例：

```less
div {
  foo: default();
  bar: default(42);
}
```
编译为：

```css
div {
  foo: default();
  bar: default(42);
}
```


### unit

> 删除或更改尺寸的单位

参数：
* `dimension`: 一个带尺寸或者不带尺寸的数字。
* `unit`: （可选）要更改的单位，或者如果省略，将删除单位。

参见 [转换](#misc-functions-convert) 用于转换单位。

示例： `unit(5, px)`

编译为： `5px`

示例： `unit(5em)`

编译为： `5`



### get-unit

> 返回一个数字的单位。

如果参数包含带有单位的数字，则该函数返回其单位。 没有单位的参数会返回一个空值。

参数：
* `number`: 一个带尺寸或者不带单位的数字。

示例： `get-unit(5px)`

编译为： `px`

示例： `get-unit(5)`

编译为： ` //nothing` 



### svg-gradient

> 生成多颜色svg渐变。

Svg-gradient函数生成多颜色Svg-gradient。 它必须至少有三个参数。 第一个参数指定渐变类型和方向，其余参数列表颜色及其位置。 第一个和最后一个指定颜色的位置是可选的，其余颜色必须指定位置。

方向必须是 `to bottom`, `to right`, `to bottom right`, `to top right`, `ellipse` 或者 `ellipse at center` 其中之一。方向可以被指定为转义值`~'to bottom'` 和空格分隔的词语列表`to bottom`。

方向必须跟着两个或更多的颜色。 它们可以在列表中提供，也可以在不同的参数中指定每种颜色。

参数 - 颜色在列表中：
* `escaped value` 或 `list of identifiers`: 方向
* `list` - 所有的颜色和他们在列表中的位置

参数 - 颜色在参数中：
* `escaped value` 或 `list of identifiers`: 方向
* `color [percentage]` : 第一种颜色及其相对位置（位置可选）
* `color percent` : （可选）第二种颜色及其相对位置
* ...
* `color percent` : （可选）第n种颜色及其相对位置
* `color [percentage]` : 最后一个颜色及其相对位置（位置可选）

返回值：带有“URI编码”的渐变的`url`。

示例 - 多颜色在列表中：
```less
div {
  @list: red, green 30%, blue;
  background-image: svg-gradient(to right, @list);
}
```

等同于颜色在参数中：
```less
div {
  background-image: svg-gradient(to right, red, green 30%, blue);
}
```

同样的结果：
```css
div {
  background-image: url('data:image/svg+xml,%3C%3Fxml%20version%3D%221.0%22%20%3F%3E%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20version%3D%221.1%22%20width%3D%22100%25%22%20height%3D%22100%25%22%20viewBox%3D%220%200%201%201%22%20preserveAspectRatio%3D%22none%22%3E%3ClinearGradient%20id%3D%22gradient%22%20gradientUnits%3D%22userSpaceOnUse%22%20x1%3D%220%25%22%20y1%3D%220%25%22%20x2%3D%22100%25%22%20y2%3D%220%25%22%3E%3Cstop%20offset%3D%220%25%22%20stop-color%3D%22%23ff0000%22%2F%3E%3Cstop%20offset%3D%2230%25%22%20stop-color%3D%22%23008000%22%2F%3E%3Cstop%20offset%3D%22100%25%22%20stop-color%3D%22%230000ff%22%2F%3E%3C%2FlinearGradient%3E%3Crect%20x%3D%220%22%20y%3D%220%22%20width%3D%221%22%20height%3D%221%22%20fill%3D%22url(%23gradient)%22%20%2F%3E%3C%2Fsvg%3E');
}
```
注意：在2.2.0之前的版本中，结果是`base64`编码。

生成的背景图像左侧为红色，宽度为30％时为绿色，结束时为蓝色。 Base64编码的部分包含以下svg-gradient：
```xml
<?xml version="1.0" ?>
<svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="100%" height="100%" viewBox="0 0 1 1" preserveAspectRatio="none">
    <linearGradient id="gradient" gradientUnits="userSpaceOnUse" x1="0%" y1="0%" x2="100%" y2="0%">
        <stop offset="0%" stop-color="#ff0000"/>
        <stop offset="30%" stop-color="#008000"/>
        <stop offset="100%" stop-color="#0000ff"/>
    </linearGradient>
    <rect x="0" y="0" width="1" height="1" fill="url(#gradient)" />
</svg>
```

