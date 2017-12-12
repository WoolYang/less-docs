> 这些操作与Photoshop，Fireworks或GIMP等图像编辑器中的混合模式_相似_（但不一定完全相同），因此您可以使用它们使CSS颜色与您的图像匹配。

### multiply

> 两种颜色相乘。 来自两种颜色中的每一种的对应RGB通道相乘在一起，然后除以255.结果是较暗的颜色。

参数:

* `color1`: 一个颜色对象。
* `color2`: 一个颜色对象。

返回值: `color`

**示例**:

```less
multiply(#ff6600, #000000);
```
![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#000000:#ffffff/text:000000)
![Color 3](holder.js/100x40/#000000:#ffffff/text:000000)

```less
multiply(#ff6600, #333333);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#333333:#ffffff/text:333333)
![Color 3](holder.js/100x40/#331400:#ffffff/text:331400)

```less
multiply(#ff6600, #666666);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#666666:#ffffff/text:666666)
![Color 3](holder.js/100x40/#662900:#ffffff/text:662900)

```less
multiply(#ff6600, #999999);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#999999:#ffffff/text:999999)
![Color 3](holder.js/100x40/#993d00:#ffffff/text:993d00)

```less
multiply(#ff6600, #cccccc);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#cccccc:#000000/text:cccccc)
![Color 3](holder.js/100x40/#cc5200:#ffffff/text:cc5200)

```less
multiply(#ff6600, #ffffff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ffffff:#000000/text:ffffff)
![Color 3](holder.js/100x40/#ff6600:#ffffff/text:ff6600)

```less
multiply(#ff6600, #ff0000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ff0000:#ffffff/text:ff0000)
![Color 3](holder.js/100x40/#ff0000:#ffffff/text:ff0000)

```less
multiply(#ff6600, #00ff00);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#00ff00:#ffffff/text:00ff00)
![Color 3](holder.js/100x40/#006600:#ffffff/text:006600)

```less
multiply(#ff6600, #0000ff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#0000ff:#ffffff/text:0000ff)
![Color 3](holder.js/100x40/#000000:#ffffff/text:000000)


### screen

> 做相反的“乘”。 结果是更明亮的颜色。

参数:

* `color1`: 一个颜色对象。
* `color2`: 一个颜色对象。

返回值: `color`

示例:

```less
screen(#ff6600, #000000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#000000:#ffffff/text:000000)
![Color 3](holder.js/100x40/#ff6600:#ffffff/text:ff6600)

```less
screen(#ff6600, #333333);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#333333:#ffffff/text:333333)
![Color 3](holder.js/100x40/#ff8533:#ffffff/text:ff8533)

```less
screen(#ff6600, #666666);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#666666:#ffffff/text:666666)
![Color 3](holder.js/100x40/#ffa366:#ffffff/text:ffa366)

```less
screen(#ff6600, #999999);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#999999:#ffffff/text:999999)
![Color 3](holder.js/100x40/#ffc299:#000000/text:ffc299)

```less
screen(#ff6600, #cccccc);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#cccccc:#000000/text:cccccc)
![Color 3](holder.js/100x40/#ffe0cc:#000000/text:ffe0cc)

```less
screen(#ff6600, #ffffff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ffffff:#000000/text:ffffff)
![Color 3](holder.js/100x40/#ffffff:#000000/text:ffffff)

```less
screen(#ff6600, #ff0000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ff0000:#ffffff/text:ff0000)
![Color 3](holder.js/100x40/#ff6600:#ffffff/text:ff6600)

```less
screen(#ff6600, #00ff00);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#00ff00:#ffffff/text:00ff00)
![Color 3](holder.js/100x40/#ffff00:#ffffff/text:ffff00)

```less
screen(#ff6600, #0000ff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#0000ff:#ffffff/text:0000ff)
![Color 3](holder.js/100x40/#ff66ff:#000000/text:ff66ff)


### overlay

> 结合`multiply`和`screen`的效果。 有条件地使光通道更亮，暗通道更暗。 **注意**：条件的结果由第一个颜色参数确定。

参数:

* `color1`: 基色对象。 也是决定性的颜色，使结果更轻或更深。
* `color2`: _覆盖_的颜色对象。

返回值: `color`

示例:

```less
overlay(#ff6600, #000000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#000000:#ffffff/text:000000)
![Color 3](holder.js/100x40/#ff0000:#ffffff/text:ff0000)

```less
overlay(#ff6600, #333333);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#333333:#ffffff/text:333333)
![Color 3](holder.js/100x40/#ff2900:#ffffff/text:ff2900)

```less
overlay(#ff6600, #666666);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#666666:#ffffff/text:666666)
![Color 3](holder.js/100x40/#ff5200:#ffffff/text:ff5200)

```less
overlay(#ff6600, #999999);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#999999:#ffffff/text:999999)
![Color 3](holder.js/100x40/#ff7a00:#ffffff/text:ff7a00)

```less
overlay(#ff6600, #cccccc);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#cccccc:#000000/text:cccccc)
![Color 3](holder.js/100x40/#ffa300:#ffffff/text:ffa300)

```less
overlay(#ff6600, #ffffff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ffffff:#000000/text:ffffff)
![Color 3](holder.js/100x40/#ffcc00:#ffffff/text:ffcc00)

```less
overlay(#ff6600, #ff0000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ff0000:#ffffff/text:ff0000)
![Color 3](holder.js/100x40/#ff0000:#ffffff/text:ff0000)

```less
overlay(#ff6600, #00ff00);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#00ff00:#ffffff/text:00ff00)
![Color 3](holder.js/100x40/#ffcc00:#ffffff/text:ffcc00)

```less
overlay(#ff6600, #0000ff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#0000ff:#ffffff/text:0000ff)
![Color 3](holder.js/100x40/#ff0000:#ffffff/text:ff0000)


### softlight

> 与`overlay`类似，但是避免纯黑色导致纯黑色，纯白色导致纯白色。

参数:

* `color1`: 一个颜色对象_soft light_另一个。
* `color2`: 被_soft lighten_颜色对象。

返回值: `color`

示例:

```less
softlight(#ff6600, #000000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#000000:#ffffff/text:000000)
![Color 3](holder.js/100x40/#ff2900:#ffffff/text:ff2900)

```less
softlight(#ff6600, #333333);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#333333:#ffffff/text:333333)
![Color 3](holder.js/100x40/#ff4100:#ffffff/text:ff4100)

```less
softlight(#ff6600, #666666);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#666666:#ffffff/text:666666)
![Color 3](holder.js/100x40/#ff5a00:#ffffff/text:ff5a00)

```less
softlight(#ff6600, #999999);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#999999:#ffffff/text:999999)
![Color 3](holder.js/100x40/#ff7200:#ffffff/text:ff7200)

```less
softlight(#ff6600, #cccccc);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#cccccc:#000000/text:cccccc)
![Color 3](holder.js/100x40/#ff8a00:#ffffff/text:ff8a00)

```less
softlight(#ff6600, #ffffff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ffffff:#000000/text:ffffff)
![Color 3](holder.js/100x40/#ffa100:#ffffff/text:ffa100)

```less
softlight(#ff6600, #ff0000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ff0000:#ffffff/text:ff0000)
![Color 3](holder.js/100x40/#ff2900:#ffffff/text:ff2900)

```less
softlight(#ff6600, #00ff00);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#00ff00:#ffffff/text:00ff00)
![Color 3](holder.js/100x40/#ffa100:#ffffff/text:ffa100)

```less
softlight(#ff6600, #0000ff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#0000ff:#ffffff/text:0000ff)
![Color 3](holder.js/100x40/#ff2900:#ffffff/text:ff2900)


### hardlight

> 与`overlay`相同，但颜色角色相反。

参数:

* `color1`: _overlay_的颜色对象。
* `color2`: 基色对象。 也是决定性的颜色，使结果更轻或更深。

返回值: `color`

示例:

```less
hardlight(#ff6600, #000000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#000000:#ffffff/text:000000)
![Color 3](holder.js/100x40/#000000:#ffffff/text:000000)

```less
hardlight(#ff6600, #333333);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#333333:#ffffff/text:333333)
![Color 3](holder.js/100x40/#662900:#ffffff/text:662900)

```less
hardlight(#ff6600, #666666);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#666666:#ffffff/text:666666)
![Color 3](holder.js/100x40/#cc5200:#ffffff/text:cc5200)

```less
hardlight(#ff6600, #999999);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#999999:#ffffff/text:999999)
![Color 3](holder.js/100x40/#ff8533:#ffffff/text:ff8533)

```less
hardlight(#ff6600, #cccccc);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#cccccc:#000000/text:cccccc)
![Color 3](holder.js/100x40/#ffc299:#ffffff/text:ffc299)

```less
hardlight(#ff6600, #ffffff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ffffff:#000000/text:ffffff)
![Color 3](holder.js/100x40/#ffffff:#000000/text:ffffff)

```less
hardlight(#ff6600, #ff0000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ff0000:#ffffff/text:ff0000)
![Color 3](holder.js/100x40/#ff0000:#ffffff/text:ff0000)

```less
hardlight(#ff6600, #00ff00);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#00ff00:#ffffff/text:00ff00)
![Color 3](holder.js/100x40/#00ff00:#ffffff/text:00ff00)

```less
hardlight(#ff6600, #0000ff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#0000ff:#ffffff/text:0000ff)
![Color 3](holder.js/100x40/#0000ff:#ffffff/text:0000ff)


### difference

> 逐个通道地从第一种颜色中减去第二种颜色。 负值被倒置。 减去黑色的结果没有变化; 减去白色导致颜色反转。

参数:

* `color1`: 作为被减数的颜色对象。
* `color2`: 作为减数的颜色对象。

返回值: `color`

示例:

```less
difference(#ff6600, #000000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#000000:#ffffff/text:000000)
![Color 3](holder.js/100x40/#ff6600:#ffffff/text:ff6600)

```less
difference(#ff6600, #333333);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#333333:#ffffff/text:333333)
![Color 3](holder.js/100x40/#cc3333:#ffffff/text:cc3333)

```less
difference(#ff6600, #666666);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#666666:#ffffff/text:666666)
![Color 3](holder.js/100x40/#990066:#ffffff/text:990066)

```less
difference(#ff6600, #999999);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#999999:#ffffff/text:999999)
![Color 3](holder.js/100x40/#663399:#ffffff/text:663399)

```less
difference(#ff6600, #cccccc);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#cccccc:#000000/text:cccccc)
![Color 3](holder.js/100x40/#3366cc:#ffffff/text:3366cc)

```less
difference(#ff6600, #ffffff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ffffff:#000000/text:ffffff)
![Color 3](holder.js/100x40/#0099ff:#ffffff/text:0099ff)

```less
difference(#ff6600, #ff0000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ff0000:#ffffff/text:ff0000)
![Color 3](holder.js/100x40/#006600:#ffffff/text:006600)

```less
difference(#ff6600, #00ff00);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#00ff00:#ffffff/text:00ff00)
![Color 3](holder.js/100x40/#ff9900:#ffffff/text:ff9900)

```less
difference(#ff6600, #0000ff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#0000ff:#ffffff/text:0000ff)
![Color 3](holder.js/100x40/#ff66ff:#000000/text:ff66ff)


### exclusion

> 与`difference`相似的效果。

参数:

* `color1`: 作为被减数的颜色对象。
* `color2`: 作为减数的颜色对象。

返回值: `color`

示例:

```less
exclusion(#ff6600, #000000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#000000:#ffffff/text:000000)
![Color 3](holder.js/100x40/#ff6600:#ffffff/text:ff6600)

```less
exclusion(#ff6600, #333333);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#333333:#ffffff/text:333333)
![Color 3](holder.js/100x40/#cc7033:#ffffff/text:cc7033)

```less
exclusion(#ff6600, #666666);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#666666:#ffffff/text:666666)
![Color 3](holder.js/100x40/#997a66:#ffffff/text:997a66)

```less
exclusion(#ff6600, #999999);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#999999:#ffffff/text:999999)
![Color 3](holder.js/100x40/#668599:#ffffff/text:668599)

```less
exclusion(#ff6600, #cccccc);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#cccccc:#000000/text:cccccc)
![Color 3](holder.js/100x40/#338fcc:#ffffff/text:338fcc)

```less
exclusion(#ff6600, #ffffff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ffffff:#000000/text:ffffff)
![Color 3](holder.js/100x40/#0099ff:#ffffff/text:0099ff)

```less
exclusion(#ff6600, #ff0000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ff0000:#ffffff/text:ff0000)
![Color 3](holder.js/100x40/#006600:#ffffff/text:006600)

```less
exclusion(#ff6600, #00ff00);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#00ff00:#ffffff/text:00ff00)
![Color 3](holder.js/100x40/#ff9900:#ffffff/text:ff9900)

```less
exclusion(#ff6600, #0000ff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#0000ff:#ffffff/text:0000ff)
![Color 3](holder.js/100x40/#ff66ff:#000000/text:ff66ff)


### average

> 按每个通道（RGB）计算两种颜色的平均值。

参数:

* `color1`: 一个颜色对象。
* `color2`: 一个颜色对象。

返回值: `color`

示例:

```less
average(#ff6600, #000000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#000000:#ffffff/text:000000)
![Color 3](holder.js/100x40/#803300:#ffffff/text:803300)

```less
average(#ff6600, #333333);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#333333:#ffffff/text:333333)
![Color 3](holder.js/100x40/#994d1a:#ffffff/text:994d1a)

```less
average(#ff6600, #666666);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#666666:#ffffff/text:666666)
![Color 3](holder.js/100x40/#b36633:#ffffff/text:b36633)

```less
average(#ff6600, #999999);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#999999:#ffffff/text:999999)
![Color 3](holder.js/100x40/#cc804d:#ffffff/text:cc804d)

```less
average(#ff6600, #cccccc);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#cccccc:#000000/text:cccccc)
![Color 3](holder.js/100x40/#e69966:#ffffff/text:e69966)

```less
average(#ff6600, #ffffff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ffffff:#000000/text:ffffff)
![Color 3](holder.js/100x40/#ffb380:#000000/text:ffb380)

```less
average(#ff6600, #ff0000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ff0000:#ffffff/text:ff0000)
![Color 3](holder.js/100x40/#ff3300:#ffffff/text:ff3300)

```less
average(#ff6600, #00ff00);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#00ff00:#ffffff/text:00ff00)
![Color 3](holder.js/100x40/#80b300:#ffffff/text:80b300)

```less
average(#ff6600, #0000ff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#0000ff:#ffffff/text:0000ff)
![Color 3](holder.js/100x40/#803380:#ffffff/text:803380)

### negation

> 对`difference`相反的效果。

结果是更明亮的颜色。 **注意**：_opposite_效果并不意味着由_addition_操作产生的_inverted_效果。

参数:

* `color1`: A color object to act as the minuend.
* `color2`: A color object to act as the subtrahend.

返回值: `color`

示例:

```less
negation(#ff6600, #000000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#000000:#ffffff/text:000000)
![Color 3](holder.js/100x40/#ff6600:#ffffff/text:ff6600)

```less
negation(#ff6600, #333333);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#333333:#ffffff/text:333333)
![Color 3](holder.js/100x40/#cc9933:#ffffff/text:cc9933)

```less
negation(#ff6600, #666666);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#666666:#ffffff/text:666666)
![Color 3](holder.js/100x40/#99cc66:#ffffff/text:99cc66)

```less
negation(#ff6600, #999999);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#999999:#ffffff/text:999999)
![Color 3](holder.js/100x40/#66ff99:#ffffff/text:66ff99)

```less
negation(#ff6600, #cccccc);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#cccccc:#000000/text:cccccc)
![Color 3](holder.js/100x40/#33cccc:#ffffff/text:33cccc)

```less
negation(#ff6600, #ffffff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ffffff:#000000/text:ffffff)
![Color 3](holder.js/100x40/#0099ff:#ffffff/text:0099ff)

```less
negation(#ff6600, #ff0000);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#ff0000:#ffffff/text:ff0000)
![Color 3](holder.js/100x40/#006600:#ffffff/text:006600)

```less
negation(#ff6600, #00ff00);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#00ff00:#ffffff/text:00ff00)
![Color 3](holder.js/100x40/#ff9900:#ffffff/text:ff9900)

```less
negation(#ff6600, #0000ff);
```

![Color 1](holder.js/100x40/#ff6600:#ffffff/text:ff6600)
![Color 2](holder.js/100x40/#0000ff:#ffffff/text:0000ff)
![Color 3](holder.js/100x40/#ff66ff:#ffffff/text:ff66ff)
