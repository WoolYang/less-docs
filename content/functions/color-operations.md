颜色操作通常与正在更改的值保持相同的单位参数，并将百分比作为绝对值处理，因此将10％值增加10％会得到20％。 选项方法参数设置为百分比的相对值。 当使用相对百分比时，10％的值增加10％，结果是11％。 数值不会被包裹，并被限制在允许的范围内。 在显示返回值的地方，除了十六进制版本之外，我们还使用格式来清楚说明每个函数的功能。

### saturate

> HSL颜色空间中的颜色饱和度增加绝对量。

参数:

* `color`: 一个颜色对象。
* `amount`: 百分比0-100％。
* `method`: 可选，设置 `relative`为相对于当前值的校准。

返回值: `color`

示例: `saturate(hsl(90, 80%, 50%), 20%)`

编译为: `#80ff00 // hsl(90, 100%, 50%)`

![Color 1](holder.js/100x40/#80e619:#000000/text:80e619) ➜ ![Color 2](holder.js/100x40/#80ff00:#000000/text:80ff00)

### desaturate

> 将HSL颜色空间中的颜色饱和度降低绝对值。

参数:

* `color`: 一个颜色对象。
* `amount`: 百分比0-100％。
* `method`: 可选，设置 `relative`为相对于当前值的校准。

返回值: `color`

示例: `desaturate(hsl(90, 80%, 50%), 20%)`

编译为: `#80cc33 // hsl(90, 60%, 50%)`

![Color 1](holder.js/100x40/#80e619:#000000/text:80e619) ➜ ![Color 2](holder.js/100x40/#80cc33:#000000/text:80cc33)

### lighten

> 增加HSL颜色空间中颜色的亮度绝对量。

参数:

* `color`: 一个颜色对象。
* `amount`: 百分比0-100％。
* `method`: 可选，设置 `relative`为相对于当前值的校准。

返回值Returns: `color`

示例: `lighten(hsl(90, 80%, 50%), 20%)`

编译为: `#b3f075 // hsl(90, 80%, 70%)`

![Color 1](holder.js/100x40/#80e619:#000000/text:80e619) ➜ ![Color 2](holder.js/100x40/#b3f075:#000000/text:b3f075)

### darken

> 减少HSL颜色空间中颜色的亮度绝对量。

参数:

* `color`: 一个颜色对象。
* `amount`: 百分比0-100％。
* `method`: 可选，设置 `relative`为相对于当前值的校准。

返回值: `color`

示例: `darken(hsl(90, 80%, 50%), 20%)`

编译为: `#4d8a0f // hsl(90, 80%, 30%)`

![Color 1](holder.js/100x40/#80e619:#000000/text:80e619) ➜ ![Color 2](holder.js/100x40/#4d8a0f:#000000/text:4d8a0f)

### fadein

> Decrease the transparency (or increase the opacity) of a color, making it more opaque.

Has no effect on opaque colors. To fade in the other direction use `fadeout`.

Parameters:

* `color`: 一个颜色对象。
* `amount`: 百分比0-100％。
* `method`: 可选，设置 `relative`为相对于当前值的校准。

返回值: `color`

示例: `fadein(hsla(90, 90%, 50%, 0.5), 10%)`

编译为: `rgba(128, 242, 13, 0.6) // hsla(90, 90%, 50%, 0.6)`


### fadeout

> 增加颜色的透明度（或降低不透明度），使其不透明度降低。 要淡出另一个方向使用`fadein`。

示例:

* `color`: 一个颜色对象。
* `amount`: 百分比0-100％。
* `method`: 可选，设置 `relative`为相对于当前值的校准。

返回值: `color`

示例: `fadeout(hsla(90, 90%, 50%, 0.5), 10%)`

编译为: `rgba(128, 242, 13, 0.4) // hsla(90, 90%, 50%, 0.4)`


### fade

> 设置颜色的绝对不透明度。 可以应用于颜色，不管它们是否已经具有不透明度值。

参数:

* `color`:  一个颜色对象。
* `amount`: 百分比0-100％。

返回值: `color`

示例: `fade(hsl(90, 90%, 50%), 10%)`

编译: `rgba(128, 242, 13, 0.1) //hsla(90, 90%, 50%, 0.1)`


### spin

> 向任一方向旋转颜色的色调角度。

当角度范围是0-360时，为一个360取余操作，所以你可以传入更大（或负）的值，并且它们将围绕例如 360度和720度的角度会产生相同的结果。 请注意，颜色是通过RGB转换传递的，它不保留灰度的色调值（因为在没有饱和度时色相没有意义），所以请确保用保留色调的方式应用函数，例如这样：

```less
@c: saturate(spin(#aaaaaa, 10), 10%);
```

替换为：

```less
@c: spin(saturate(#aaaaaa, 10%), 10);
```

颜色总是以RGB值的形式返回，因此将`spin`应用于灰度值将不会执行任何操作。

参数:

* `color`: 一个颜色对象。
* `angle`: 多个度数旋转（+或 - ）。

返回值: `color`

示例:

```less
spin(hsl(10, 90%, 50%), 30)
spin(hsl(10, 90%, 50%), -30)
```

编译为:

```css
#f2a60d // hsl(40, 90%, 50%)
#f20d59 // hsl(340, 90%, 50%)
```

![Color 1](holder.js/100x40/#f2330d:#000000/text:f2330d) ➜ ![Color 2](holder.js/100x40/#f2a60d:#000000/text:f2a60d)

![Color 1](holder.js/100x40/#f2330d:#000000/text:f2330d) ➜ ![Color 2](holder.js/100x40/#f20d59:#000000/text:f20d59)

### mix

> 以不同比例混合两种颜色。 计算中包含不透明度。

参数:

* `color1`: 一个颜色对象。
* `color2`: 一个颜色对象。
* `weight`: 可选，两种颜色之间的百分比平衡点，默认为50％。

返回值: `color`

示例:

```less
mix(#ff0000, #0000ff, 50%)
mix(rgba(100,0,0,1.0), rgba(0,100,0,0.5), 50%)
```

编译为:

```css
#800080
rgba(75, 25, 0, 0.75)
```

![Color 1](holder.js/100x40/#ff0000:#ffffff/text:ff0000) + ![Color 2](holder.js/100x40/#0000ff:#ffffff/text:0000ff) ➜ ![Color 3](holder.js/100x40/#800080:#ffffff/text:800080)

### tint

> 以不同比例混合颜色与白色，和调用 ``mix(#ffffff, @color, @weight)`` 一样。

参数:

* `color`: 一个颜色对象。
* `weight`: 可选，颜色和白色之间的百分比平衡点，默认为50％。

返回值: `color`

示例:

```less
no-alpha: tint(#007fff, 50%); 
with-alpha: tint(rgba(00,0,255,0.5), 50%); 
```

编译为:

```css
no-alpha: #80bfff;
with-alpha: rgba(191, 191, 255, 0.75);
```

![Color 1](holder.js/100x40/#ff00ff:#ffffff/text:ff00ff) ➜ ![Color 2](holder.js/100x40/#ff80ff:#ffffff/text:ff80ff)

### shade

> 以不同比例混合颜色与黑色，和调用 ``mix(#000000, @color, @weight)``一样。

参数:

* `color`: 一个颜色对象。
* `weight`: 可选，颜色和白色之间的百分比平衡点，默认为50％。

返回值: `color`

示例:

```less
no-alpha: shade(#007fff, 50%); 
with-alpha: shade(rgba(00,0,255,0.5), 50%); 
```

编译为:

```css
no-alpha: #004080;
with-alpha: rgba(0, 0, 64, 0.75);
```

![Color 1](holder.js/100x40/#ff00ff:#ffffff/text:ff00ff) ➜ ![Color 2](holder.js/100x40/#800080:#ffffff/text:800080)

### greyscale

> 去除HSL颜色空间中所有颜色的饱和度，和调用 `desaturate(@color, 100%)`一样。

Because the saturation is not affected by hue, the resulting color mapping may be somewhat dull or muddy; [`luma`](#color-channel-luma) may provide a better result as it extracts perceptual rather than linear brightness, for example `greyscale('#0000ff')` will return the same value as `greyscale('#00ff00')`, though they appear quite different in brightness to the human eye.

参数: `color`: 一个颜色对象。

返回值: `color`

示例: `greyscale(hsl(90, 90%, 50%))`

编译为: `#808080 // hsl(90, 0%, 50%)`

![Color 1](holder.js/100x40/#80f20d:#000000/text:80f20d) ➜ ![Color 2](holder.js/100x40/#808080:#000000/text:808080)

请注意，生成的灰色看起来比原来的绿色更暗，即使它的亮度值相同。

与使用`luma`比较（用法不同，因为`luma`返回一个单一的值，而不是一个颜色）：


```less
@c: luma(hsl(90, 90%, 50%));
color: rgb(@c, @c, @c);
```

编译为: `#cacaca`

![Color 1](holder.js/100x40/#80f20d:#000000/text:80f20d) ➜ ![Color 2](holder.js/100x40/#cacaca:#000000/text:cacaca)

这一次灰色的亮度看起来和绿色一样，尽管其价值实际上更高。

### contrast

> 选择两种颜色中的哪一种与另一种颜色形成最大对比。

这对于确保颜色在背景下可读取是有用的，这对于可访问性合规性也是有用的。 这个功能的工作原理与[SASS在Compass中的对比度功能](http://compass-style.org/reference/compass/utilities/color/contrast/)相同。 依据[WCAG 2.0](http://www.w3.org/TR/2008/REC-WCAG20-20081211/#relativeluminancedef)， 使用伽马校正的[luma](#color-channel-luma)值来比较颜色，而不是它们的亮度。

可以按照任意顺序提供明暗参数 - 该功能将自动计算亮度值并指定明暗，这意味着您不能使用此功能通过颠倒顺序来选择*最少*对比色。

参数:

* `color`: 要比较的颜色对象。
* `dark`: 可选 - 指定的深色（默认为黑色）。
* `light`: 可选 - 指定的浅色（默认为白色）。
* `threshold`: 可选 - 百分比0-100％，指定从“暗”到“亮”转换的位置（默认为43％，与SASS匹配）。 这是用来以某种方式偏向对比度，例如让你决定50％的灰色背景是否会产生黑色或白色的文字。 一般情况下，对于“较亮”的调色板，这个设置较低，对于“较暗”的调色板设置得较高。

返回值: `color`

示例:

```less
p {
    a: contrast(#bbbbbb);
    b: contrast(#222222, #101010);
    c: contrast(#222222, #101010, #dddddd);
    d: contrast(hsl(90, 100%, 50%), #000000, #ffffff, 30%);
    e: contrast(hsl(90, 100%, 50%), #000000, #ffffff, 80%);
}
```

编译为:

```css
p {
    a: #000000 // black
    b: #ffffff // white
    c: #dddddd
    d: #000000 // black
    e: #ffffff // white
}
```
这些示例使用上述计算的颜色作为背景和前景; 您可以看到，尽管可以使用阈值来允许低对比度的结果，但是最终的结果并不是白底白字，也不是黑底黑字，如上例所示：

![Color 1](holder.js/100x40/#bbbbbb:#000000/text:000000)
![Color 1](holder.js/100x40/#222222:#ffffff/text:ffffff)
![Color 1](holder.js/100x40/#222222:#dddddd/text:dddddd)
![Color 1](holder.js/100x40/#80ff00:#000000/text:000000)
![Color 1](holder.js/100x40/#80ff00:#ffffff/text:ffffff)
