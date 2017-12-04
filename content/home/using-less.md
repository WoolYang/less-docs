---
title: Server-side Usage
---

> Less 可以通过npm在命令行中使用， 以及作为浏览器的脚本文件下载，或在各种各样的第三方工具中使用。 查看[使用]({{resolve 'usage'}}) 章节获取更多具体信息。

## 1.安装

在服务器上安装Less的最简单的方法是通过npm，[node.js](http://nodejs.org/)软件包管理器，如下所示：

```bash
$ npm install -g less
```

## 2.命令行用法

安装完成后，可以从命令行调用编译器，如下所示：

```bash
$ lessc styles.less
```

这将输出编译的CSS文件到`stdout`。 要将CSS结果保存到你选择的文件，请使用：

```bash
$ lessc styles.less styles.css
```

如果要输出压缩的CSS文件，你可以使用[`clean-css`插件](https://github.com/less/less-plugin-clean-css)。 安装插件后，用`--clean-css`选项指定输出压缩的CSS文件： 

```bash
$ lessc --clean-css styles.less styles.min.css
```

要查看所有命令行选项，请不带参数运行`lessc`，或者参阅[使用方法]({{resolve 'usage'}})。

## 3.代码中用法

你可以从node中调用Less编译器，如下所示：

```js
var less = require('less');

less.render('.class { width: (1 + 1) }', function (e, output) {
  console.log(output.css);
});
```

输出如下：

```css
.class {
  width: 2;
}
```

## 4.配置

你可以将一些配置选项传递给编译器：

```js
var less = require('less');

less.render('.class { width: (1 + 1) }',
    {
      paths: ['.', './lib'],  // 指定@import指令的搜索路径
      filename: 'style.less', // 指定一个文件名称, 以更好的输出错误信息
    },
    function (e, output) {
       console.log(output.css);
    });
```

更多相关信息，请参阅[使用方法]({{resolve 'usage'}})。

## 5.第三方工具

其他工具的详细信息，请参见[使用方法]({{resolve 'usage'}})章节。

<!-- # Command-line with Rhino
> Each Less release contains also rhino-compatible version.

Command line rhino version requires two files:
* less-rhino-&lt;version&gt;.js - compiler implementation,
* lessc-rhino-&lt;version&gt;.js - command line support.

Command to run the compiler:
````
java -jar js.jar -f less-rhino-<version>.js lessc-rhino-<version>.js styles.less styles.css
````

This will compile styles.less file and save the result to styles.css file. The output file parameter is optional. If it is missing, less will output the result to `stdout`.-->

# 浏览器端用法

> 在浏览器中使用less.js对开发很有帮助， 但不推荐用于生产环境

客户端安装是Less最简单的安装方式，不过这种方式只适用于开发环境，因为在生产环境中，系统的性能和可靠性尤为重要， _所以我们建议使用node.js或其他第三方工具来预编译_。

首先，通过将`rel`属性设置为"`stylesheet/less`"来连接你的 `.less` 样式表：

```html
<link rel="stylesheet/less" type="text/css" href="styles.less" />
```

然后, [下载 less.js](https://github.com/less/less.js/archive/master.zip)并将其包含在页面的`<head>`元素的`<script></script>`标签中：

```html
<script src="less.js" type="text/javascript"></script>
```

### 提示

* 确保在script脚本之前包含样式表。
* 当你链接多个`.less`样式表时，每个样式表都是独立编译的。因此，在样式表中定义的任何变量，mixins或名称空间都不能被其他任何样式表访问。
* 由于浏览器加载外部资源的同源策略需要[启用CORS](http://enable-cors.org/)。

## 1.浏览器选项

选项通过在`<script src="less.js"></script>` **之前** 之前将其设置在全局`less` 对象上来定义的：

``` html
<!-- set options before less.js script -->
<script>
  less = {
    env: "development",
    async: false,
    fileAsync: false,
    poll: 1000,
    functions: {},
    dumpLineNumbers: "comments",
    relativeUrls: false,
    rootpath: ":/a.com/"
  };
</script>
<script src="less.js"></script>
```

或者为了简洁起见，可以将它们设置为脚本和链接标记的属性（需要JSON.parse浏览器支持或polyfill）。

``` html
<script src="less.js" data-poll="1000" data-relative-urls="false"></script>
<link data-dump-line-numbers="all" data-global-vars='{ myvar: "#ddffee", mystr: "\"quoted\"" }' rel="stylesheet/less" type="text/css" href="less/styles.less">
```

详细了解[浏览器选项](usage/#using-less-in-the-browser-setting-options)
