### isnumber

> 如果值为数字返回 `true`，否则返回`false`。

参数: `value` - 需要被判断的值或变量。

返回值: 如果值为数字返回 `true`，否则返回`false`。

示例:

```less
isnumber(#ff0);     // false
isnumber(blue);     // false
isnumber("string"); // false
isnumber(1234);     // true
isnumber(56px);     // true
isnumber(7.8%);     // true
isnumber(keyword);  // false
isnumber(url(...)); // false
```


### isstring

>  如果值为字符串返回 `true`，否则返回`false`。

参数: `value` - 需要被判断的值或变量。

返回值: 如果值为字符串返回 `true`，否则返回`false`。

示例:

```less
isstring(#ff0);     // false
isstring(blue);     // false
isstring("string"); // true
isstring(1234);     // false
isstring(56px);     // false
isstring(7.8%);     // false
isstring(keyword);  // false
isstring(url(...)); // false
```


### iscolor

>  如果值为颜色返回 `true`，否则返回`false`。

参数: `value` - 需要被判断的值或变量。

返回值: 如果值为颜色返回 `true`，否则返回`false`。

示例:

```less
iscolor(#ff0);     // true
iscolor(blue);     // true
iscolor("string"); // false
iscolor(1234);     // false
iscolor(56px);     // false
iscolor(7.8%);     // false
iscolor(keyword);  // false
iscolor(url(...)); // false
```


### iskeyword

> 如果值是关键字返回`true`，否则返回`false`。

参数: `value` - 需要被判断的值或变量。

返回值: 如果值为关键字返回 `true`，否则返回`false`。

示例:

```less
iskeyword(#ff0);     // false
iskeyword(blue);     // false
iskeyword("string"); // false
iskeyword(1234);     // false
iskeyword(56px);     // false
iskeyword(7.8%);     // false
iskeyword(keyword);  // true
iskeyword(url(...)); // false
```


### isurl

> 如果值是url返回`true`，否则返回`false`。

参数: `value` - 需要被判断的值或变量。

返回值: 如果值是url返回`true`，否则返回`false`。

示例:

```less
isurl(#ff0);     // false
isurl(blue);     // false
isurl("string"); // false
isurl(1234);     // false
isurl(56px);     // false
isurl(7.8%);     // false
isurl(keyword);  // false
isurl(url(...)); // true
```


### ispixel

> 如果值是像素返回`true`，否则返回`false`。

参数: `value` - 需要被判断的值或变量。

返回值: 如果值是像素返回`true`，否则返回`false`。

示例:

```less
ispixel(#ff0);     // false
ispixel(blue);     // false
ispixel("string"); // false
ispixel(1234);     // false
ispixel(56px);     // true
ispixel(7.8%);     // false
ispixel(keyword);  // false
ispixel(url(...)); // false
```


### isem

> 如果是一个em值返回`true`，否则返回`false`。

参数: `value` - 需要被判断的值或变量。

返回值: 如果是一个em值返回`true`，否则返回`false`。

示例:

```less
isem(#ff0);     // false
isem(blue);     // false
isem("string"); // false
isem(1234);     // false
isem(56px);     // false
isem(7.8em);    // true
isem(keyword);  // false
isem(url(...)); // false
```


### ispercentage

> 如果是一个百分比值返回`true`，否则返回`false`。

参数: `value` - 需要被判断的值或变量。

返回值: 如果是一个百分比值返回`true`，否则返回`false`。

示例:

```less
ispercentage(#ff0);     // false
ispercentage(blue);     // false
ispercentage("string"); // false
ispercentage(1234);     // false
ispercentage(56px);     // false
ispercentage(7.8%);     // true
ispercentage(keyword);  // false
ispercentage(url(...)); // false
```


### isunit

> 如果值是以指定单位表示的数字，则返回`true`，否则返回`false`。

参数:
* `value` - 需要被判断的值或变量。
* `unit` - 用一个单位标识符（可选引用）测试。

如果值是以指定单位表示的数字，则返回`true`，否则返回`false`。

示例:

```less
isunit(11px, px);  // true
isunit(2.2%, px);  // false
isunit(33px, rem); // false
isunit(4rem, rem); // true
isunit(56px, "%"); // false
isunit(7.8%, '%'); // true
isunit(1234, em);  // false
isunit(#ff0, pt);  // false
isunit("mm", mm);  // false
```


### isruleset

> 如果某个值是规则集，则返回`true`;否则返回`false`。

参数:
* `value` - 需要被判断的变量。

返回值: 如果值是一个规则集，则为`true`，否则为`false`。

示例:

```less
@rules: {
    color: red;
}

isruleset(@rules);   // true
isruleset(#ff0);     // false
isruleset(blue);     // false
isruleset("string"); // false
isruleset(1234);     // false
isruleset(56px);     // false
isruleset(7.8%);     // false
isruleset(keyword);  // false
isruleset(url(...)); // false
```
