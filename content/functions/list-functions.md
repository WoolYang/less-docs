### length

> 返回值列表中的元素数。

参数: `list` - 逗号或空格分隔的值列表。
返回值: 列表中的整数个元素。

示例: `length(1px solid #0080ff);`
编译为: `3`

示例:

```less
@list: "banana", "tomato", "potato", "peach";
n: length(@list);
```

编译为:

```
n: 4;
```

### extract

> 返回列表中指定位置的值。

参数:
`list` - 逗号或空格分隔的值列表。
`index` - 一个整数，指定要返回的列表元素的位置。
返回值: 列表中指定位置的值。

示例: `extract(8px dotted red, 2);`
编译为: `dotted`

示例:

```less
@list: apple, pear, coconut, orange;
value: extract(@list, 3);
```

编译为:

```
value: coconut;
```
