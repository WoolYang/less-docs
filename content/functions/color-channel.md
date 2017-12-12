### hue

> 在HSL颜色空间中提取颜色对象的色调通道。

参数: `color` - a color object.

返回值: `integer` 0-360

示例: `hue(hsl(90, 100%, 50%))`

编译为: `90`


### saturation

> 在HSL颜色空间中提取颜色对象的饱和度通道。

参数: `color` - 一个颜色对象。

返回值: `percentage` 0-100

示例: `saturation(hsl(90, 100%, 50%))`

编译为: `100%`


### lightness

> 提取HSL颜色空间中颜色对象的亮度通道。

参数: `color` - 一个颜色对象。

返回值: `percentage` 0-100

示例: `lightness(hsl(90, 100%, 50%))`

编译为: `50%`


### hsvhue

> 提取HSV颜色空间中的颜色对象的色调通道。

参数: `color` - 一个颜色对象。

返回值: `integer` `0-360`

示例: `hsvhue(hsv(90, 100%, 50%))`

编译为: `90`


### hsvsaturation

> 提取HSV颜色空间中颜色对象的饱和度通道。

参数: `color` - 一个颜色对象。

返回值: `percentage` 0-100

示例: `hsvsaturation(hsv(90, 100%, 50%))`

编译为: `100%`


### hsvvalue

> 提取HSV颜色空间中颜色对象的值通道。

参数: `color` - 一个颜色对象。

返回值: `percentage` 0-100

示例: `hsvvalue(hsv(90, 100%, 50%))`

编译为: `50%`


### red

> 提取颜色对象的红色通道。

参数: `color` - 一个颜色对象。

返回值: `float` 0-255

示例: `red(rgb(10, 20, 30))`

编译为: `10`


### green

> 提取颜色对象的绿色通道。

参数: `color` - 一个颜色对象。

返回值: `float` 0-255

示例: `green(rgb(10, 20, 30))`

编译为: `20`


### blue

> 提取颜色对象的蓝色通道。

参数: `color` - 一个颜色对象。

返回值: `float` 0-255

示例: `blue(rgb(10, 20, 30))`

编译为: `30`


### alpha

> 提取颜色对象的Alpha通道。

参数: `color` - 一个颜色对象。
返回值: `float` 0-1

示例: `alpha(rgba(10, 20, 30, 0.5))`

编译为: `0.5`


### luma

> 计算颜色对象的[亮度](http://en.wikipedia.org/wiki/Luma_%28video%29)(感知亮度)。

用途**SMPTE C / Rec. 709**系数，如[WCAG 2.0](http://www.w3.org/TR/2008/REC-WCAG20-20081211/#relativeluminancedef)中所建议的。 这个计算也用在对比度函数中。

在v1.7.0之前，没有gamma校正的情况下计算亮度，使用亮度函数来计算这些“旧”值。

参数: `color` - 一个颜色对象。

返回值: `percentage` 0-100%

示例: `luma(rgb(100, 200, 30))`

编译为: `44%`


### luminance

> 计算没有gamma校正的亮度值（这个函数在v1.7.0之前被命名为luma）

参数: `color` - 一个颜色对象。

返回值: `percentage` 0-100%

示例: `luminance(rgb(100, 200, 30))`

编译为: `65%`
