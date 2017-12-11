### escape

> [URL-encoding](http://en.wikipedia.org/wiki/Percent-encoding)用于在输入的字符串中找到的特殊字符。

* 这些字符不被编码: `,`, `/`, `?`, `@`, `&`, `+`, `'`, `~`, `!` and `$`.
* 最常见的编码字符是: `\<space\>`, `#`, `^`, `(`, `)`, `{`, `}`, `|`, `:`, `>`, `<`, `;`, `]`, `[` and `=`.

参数: `string`: 需要转义的字符串。

返回值: `string` - 不带引号的转义字符串内容。

示例:

```less
escape('a=1')
```

编译为:

```css
a%3D1
```

注意：如果参数不是一个字符串，输出为未定义。 当前的实现为如果是颜色返回undefined，其他类型参数返回原输入。 不应该依赖这种行为，以后可能会改变这种实现。

### e

> CSS转义，替换为`~"value"`语法。

`e`期望以字符串作为参数，并返回不带引号的内容，但没有引号。 可以用来输出不是有效的CSS语法的CSS值，或者使用Less无法识别的专有语法。

参数: `string` - 需要转义的字符串。

返回值: `string` - 不带引号的转义字符串内容。

示例:

```less
filter: e("ms:alwaysHasItsOwnSyntax.For.Stuff()");
```

编译为:

```css
filter: ms:alwaysHasItsOwnSyntax.For.Stuff();
```

注意：函数也接受`~""`转义值和数字作为参数。 其他任何东西都会返回错误。

### % format

>  `%(string, arguments ...)` 格式化一个字符串。

第一个参数是带占位符的字符串。 所有占位符以百分号`%`开头，后跟字母 `s`,`S`,`d`,`D`,`a`, 或 `A`。 剩余的参数为替换占位符的表达式。 如果您需要输出百分比符号，请用另一个百分比'%%`将其转义。

如果需要将特殊字符转义为utf-8转义码，请使用大写字母占位符。
转义除了`()'~!`以外的所有特殊字符。 空格编码为`%20`。 小写字母占位符保留特殊字符。

占位符:
* `d`, `D`, `a`, `A` - 可以被任何类型的参数（颜色，数字，转义值，表达式...）所取代。 如果将它们与字符串结合使用，则将使用整个字符串 - 包括引号。 但是，在在字符串中放置引号时，引号不会被“/”和类似的东西转义。
* `s`, `S` - 可以被任何表达式替换。 如果使用字符串，只能使用字符串值 - 引号被忽略。

参数:

* `string`: 格式化带占位符的字符串,
* `anything`* : 用来替换占位符的值。

返回值: 被格式化的 `string`.

示例:

```less
format-a-d: %("repetitions: %a file: %d", 1 + 2, "directory/file.less");
format-a-d-upper: %('repetitions: %A file: %D', 1 + 2, "directory/file.less");
format-s: %("repetitions: %s file: %s", 1 + 2, "directory/file.less");
format-s-upper: %('repetitions: %S file: %S', 1 + 2, "directory/file.less");
```
编译为:

```css
format-a-d: "repetitions: 3 file: "directory/file.less"";
format-a-d-upper: "repetitions: 3 file: %22directory%2Ffile.less%22";
format-s: "repetitions: 3 file: directory/file.less";
format-s-upper: "repetitions: 3 file: directory%2Ffile.less";
```


### replace

> 替换字符串中的文本

发布 [v1.7.0]({{ less.master.url }}CHANGELOG.md)

参数:

* `string`: 要查找和替换的字符串。
* `pattern`: 要查找的字符串或正则表达式模式。
* `replacement`: 用来替换匹配模式的字符串。
* `flags`: （可选）正则表达式标志。

返回值: 带有替换值的字符串。

示例:

```less
replace("Hello, Mars?", "Mars\?", "Earth!");
replace("One + one = 4", "one", "2", "gi");
replace('This is a string.', "(string)\.$", "new $1.");
replace(~"bar-1", '1', '2');
```
编译为:

```css
"Hello, Earth!";
"2 + 2 = 4";
'This is a new string.';
bar-2;
```
