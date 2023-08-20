---
categories:
  - 编程语言
  - 前端
  - html5
---
# html引入脚本

在html中添加script脚本有两种方法，直接将javascript代码添加到html中与添加外部js文件，这两种方法都比较常用，大家可以根据自己需要自由选择

在html中添加`<script>`脚本的方法：

1、可以直接将javascript代码添加到html中

复制代码
代码如下:

```
<script type="text/javascript"> 
//javascritp代码 
</script>
123
```

当解释器嵌入`<script>`代码时，html页面的处理也会被暂时停止。

2、添加外部js文件

复制代码
代码如下:

```
<script type="text/javascript" src="外部.js代码的地址"></script>
1
```

注意事项：
① 在XHTML文档中，可以省略`</script>`标签，例如：

复制代码
代码如下:

② 最好将`<script>`标签放在`</body>`标签之前,这样能使浏览器更快的加载页面。

③在使用`<script>`嵌入JavaScript代码时，不要在代码中的任何地方出现`“</script>”`字符串。例如，浏览器加载下面的代码就会产生错误:

复制代码
代码如下:

```
<script type="text/javascript"> 
function function1(){ 
alert("</script>"); 
}
1234
```

因为按照解析嵌入式代码的规则，当浏览器遇到字符串“”时，就会认为那是结束的标签。如果想在代码中添加字符串，可以写成
http://www.duopintech.com/