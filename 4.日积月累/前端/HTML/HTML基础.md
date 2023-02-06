# HTML

## HTML基础教程

### HTML简介
HTML 是用来描述网页的一种语言。

* HTML 指的是超文本标记语言 (Hyper Text Markup Language)
* HTML 不是一种编程语言，而是一种标记语言 (markup language)
* 标记语言是一套标记标签 (markup tag)
* HTML 使用标记标签来描述网页

HTML标记标签通常被称为HTML标签（HTML tag）
* HTML标签是由尖括号包围的关键词，比如`<html>`
* HTML标签通常是成对出现的，比如<b>和</b>
* 标签对中的第一个标签是开始标签，第二个标签是结束标签
* 开始和结束标签也被称为开放标签和闭合标签

HTML文档 = 网页
* HTML文档描述网页
* HTML文档包含HTML标签和纯文本
* HTML文档也被称为网页

web浏览器的作用是读取HTML文档，并以网页的形式显示出它们。浏览器不会显示HTML标签，而是使用标签来解释页面的内容：
```html
<html>
<body>
<h1>我的第一个标题</h1>

<p>我的第一个段落</p>

</body>
</html>
```
* `<html>` 与 `</html>` 之间的文本描述网页
* `<body>` 与 `</body>` 之间的文本是可见的页面内容
* `<h1>` 与 `</h1>` 之间的文本被显示为标题 
* `<p>` 与 `</p>` 之间的文本被显示为段落

## HTML元素
HTML文档是由HTML元素定义的  
HTML元素指的是从开始标签（start tag）到结束标签（end tag）的所有代码  
| 开始标签 | 元素内容 | 结束标签 |
| :-----| :---- | :----:|
| `<p>` | This is a paragraph | `</p>` |
| `<a href="default">` | This is a link | `</a>` |
| `<br/>` |   |   |

**HTML的元素语法**
* HTML 元素以开始标签起始
* HTML 元素以结束标签终止
* 元素的内容是开始标签与结束标签之间的内容
* 某些HTML元素具有空内容
* 空元素在开始标签中进行关闭（以开始标签的结束而结束）
* 大多数HTML元素可拥有属性

嵌套的HTML元素
大多数HTML元素可以被嵌套（可以包含其他HTML元素）
HTML文档由嵌套的HTML元素构成

```html
<html>

<body>
<p>This is my first parafraph.</p>
</body>

</html>
```
上面的例子包含三个HTML元素。  
HTML实例解释：  
`<p>`元素：  
`<p>This is my first paragraph.</p>`  
这个`<p>`元素定义了HTML文档中的一个段落。  
这个元素拥有一个开始标签`<p>`,以及一个结束标签`</p>`  
元素内容是：This is my first paragraph.  

`<body>`元素：
```  
<body>
<p>This is my first parafraph.</p>
</body>
```
这个`<body>`元素定义了HTML文档的主体。  
这个元素拥有一个开始标签`<body>`,以及一个结束标签`</body>`  
元素内容是另一个HTML元素 

`<html>`元素：
```  
<html>

<body>
<p>This is my first parafraph.</p>
</body>

</html>
```
这个`<html>`元素定义了整个HTML文档。  
这个元素拥有一个开始标签`<html>`,以及一个结束标签`</html>`  
元素内容是另一个HTML元素 

**空的HTML元素**  
没有内容的HTMl元素就被称为空元素。空元素是在开始标签中关闭的。  
`<br>`就是没有关闭标签的空元素（`<br>`标签定义换行）  
在XHTML、XML以及未来版本的HTML中，所有元素都必须被关闭。
在开始标签中添加斜杠，比如`<br/>`是关闭空元素的正确方法。
即使`<br>`在所有浏览器中都是有效的，但使用`<br/>`是更长远的保障

### HTML属性
HTML标签可以拥有属性。属性提供了有关HTML元素的更多信息。  
属性总是以名称/值对的形式出现，比如：name = "value"  
属性总是在HTML元素的**开始标签***中规定。  

属性实例：    
HTML链接由`<a>`标签定义。链接的地址在href属性中指定：  
`<a href= "http://www.w3school.com.cn"> This is a link </a>`

属性例子 1:  
`<h1>` 定义标题的开始。
`<h1 align="center">` 拥有关于对齐方式的附加信息。  
TIY : 居中排列标题  
属性例子 2:  
`<body>` 定义 HTML 文档的主体。  
`<body bgcolor="yellow">` 拥有关于背景颜色的附加信息。  
TIY : 背景颜色  
属性例子 3:  
`<table>` 定义 HTML 表格。（您将在稍后的章节学习到更多有关 HTML 表格的内容）  
`<table border="1">` 拥有关于表格边框的附加信息。  

**始终为属性值加引号**  
属性值应该始终被包括在引号内。双引号是最常用的，不过单引号也没有问题。  
在某些个别情况下，比如属性值本身就含有双引号，那么您必须使用单引号。  

### HTML标题
标题（Heading）是通过`<h1>-<h6>`等标签来进行定义的。  
`<h1>`定义最大的标题。`h6`定义最小的标题。  
实例：  
```html
<h1>This is a heading</h1>
<h2>This is a heading</h2>
<h3>This is a heading</h3>
```
浏览器会自动在标题的前后添加空行。  
默认情况下，HTML会自动地在块级元素前后添加一个额外的空行，比如段落、标题元素前后。  
搜索引擎使用标题为您的网页的结构和内容编制索引。  

**HTML水平线**
`<hr/>`标签在HTML页面中创建水平线。hr元素用来分隔内容。

**HTML注释**
可以将注释插入HTML代码中，这样可以提高其可读性，使代码更容易被别人理解。浏览器会忽略注释，不会显示他们。  
注释是这样写的：`<!--This is a comment -->`

### 段落
段落是通过<p>标签来定义的。  
实例：  
```html
<p>This is a paragraph</p>
<p>This is a another paragraph</p>
```
**HTML拆行**
如果您希望在不产生一个新段落的情况下进行换行，请使用`<br/>`标签：  
`<p>This is <br /> a para<br />graph with line breaks</p>`  
`<br />`元素是一个空的HTML元素。由于关闭标签没有任何意义，因此它没有结束标签。

**HTML输出**
我们无法确定HTML被显示的确切效果。屏幕的大小，以及对窗口的调整都可能导致不同的结果。  
对于HTML，您无法通过在HTML代码中添加额外的空格或者换行来改变输出的效果。  
当显示页面时，浏览器会移除源代码中多余的空格和空行。所有连续的空格和空行都会被算作一个空格。需要注意的是，HTML代码中所有连续的空行（换行）也被显示为一个空格。  

### HTML样式
style属性用于改变HTML元素的样式。  
style属性的作用：  
**提供了一种改变HTML元素样式的通用方法**  
样式是HTML4引入的。通过HTML样式，能够通过使用style属性直接将样式添加到HTML元素，或者间接地在独立的样式表中（CSS文件）进行定义。  

实例：  
```html
<html>

<body style="background-color:yellow">
<h2 style="background-color:red">This is a heading</h2>
<p style="background-color:green">This is a paragraph.</p>
</body>

<html>
```
**HTML样式实例-字体、颜色和尺寸**
```html
<html>

<body>
<h1 style="font-family:verdana">A heading</h1>
<p style="font-family:arial; color:red; font-size:20px;">A paragraph.</p>
</body>

</html>
```
**HTML样式实例-文本对齐**
text-align属性规定了元素值中文本的水平对齐方式：
```html
<html>

<body>
<h1 style="text-align:center">A heading</h1>
<p style=>The heading above is aligned to the center of this page.</p>
</body>

</html>
```

### HTML文本格式化
HTML可定义很多供格式化输出的元素，比如粗体和斜体字。  
1. 文本格式化
```html
<html>

<body>

<b>This text is bold</b>

<br />

<strong>This text is strong</strong>

<br />

<big>This text is big</big>

<br />

<em>This text is emphasized</em>

<br />

<i>This text is italic</i>

<br />

<small>This text is small</small>

<br />

This text contains
<sub>subscript</sub>

<br />

This text contains
<sup>superscript</sup>

</body>
</html>
```
2. 预格式文本
```html
<html>

<body>

<pre>
这是
预格式文本。
它保留了      空格
和换行。
</pre>

<p>pre 标签很适合显示计算机代码：</p>

<pre>
for i = 1 to 10
     print i
next i
</pre>

</body>
</html>
```
3. “计算机输出”标签
```html
<html>

<body>

<code>Computer code</code>
<br />
<kbd>Keyboard input</kbd>
<br />
<tt>Teletype text</tt>
<br />
<samp>Sample text</samp>
<br />
<var>Computer variable</var>
<br />

<p>
<b>注释：</b>这些标签常用于显示计算机/编程代码。
</p>

</body>
</html>
```
4. 地址
```html
<!DOCTYPE html>
<html>
<body>

<address>
Written by <a href="mailto:webmaster@example.com">Donald Duck</a>.<br> 
Visit us at:<br>
Example.com<br>
Box 564, Disneyland<br>
USA
</address>

</body>
</html>
```
5. 缩写和首字母缩写
```html
<html>

<body>

<abbr title="etcetera">etc.</abbr>
<br />
<acronym title="World Wide Web">WWW</acronym>

<p>在某些浏览器中，当您把鼠标移至缩略词语上时，title 可用于展示表达的完整版本。</p>

<p>仅对于 IE 5 中的 acronym 元素有效。</p>

<p>对于 Netscape 6.2 中的 abbr 和 acronym 元素都有效。</p>

</body>
</html>
```
6. 文字方向
```html
<html>

<body>

<p>
如果您的浏览器支持 bi-directional override (bdo)，下一行会从右向左输出 (rtl)；
</p>

<bdo dir="rtl">
Here is some Hebrew text
</bdo>

</body>
</html>
```
7. 块引用
```html
<html>

<body>

这是长的引用：
<blockquote>
这是长的引用。这是长的引用。这是长的引用。这是长的引用。这是长的引用。这是长的引用。这是长的引用。这是长的引用。这是长的引用。这是长的引用。这是长的引用。
</blockquote>

这是短的引用：
<q>
这是短的引用。
</q>

<p>
使用 blockquote 元素的话，浏览器会插入换行和外边距，而 q 元素不会有任何特殊的呈现。
</p>

</body>
</html>
```
8. 删除字效果和插入字效果
```html
<html>

<body>

<p>一打有 <del>二十</del> <ins>十二</ins> 件。</p>

<p>大多数浏览器会改写为删除文本和下划线文本。</p>

<p>一些老式的浏览器会把删除文本和下划线文本显示为普通文本。</p>

</body>
</html>
```

### HTML CSS
通过HTML4.0，所有格式化代码均可移出HTML文档，然后移入一个独立的样式表。
实例：  
1. HTML中的样式
```html
<html>

<head>
<style type="text/css">
h1 {color: red}
p {color: blue}
</style>
</head>

<body>
<h1>header 1</h1>
<p>A paragraph.</p>
</body>

</html>
```
2. 没有下划线的链接
```html
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312" />
<meta http-equiv="Content-Language" content="zh-cn" />
</head>

<body>

<a href="/example/html/lastpage.html" style="text-decoration:none">
这是一个链接！
</a>

</body>
</html>
```
3. 链接到一个外部的样式表
```html
<html>

<head>
<link rel="stylesheet" type="text/css" href="/html/csstest1.css" >
</head>

<body>
<h1>我通过外部样式表进行格式化。</h1>
<p>我也一样！</p>
</body>

</html>
```
**如何使用样式**
当浏览器读到一个样式表，它就会按照这个样式来对文档进行格式化。有以下三种方式来插入样式表：
1. 外部样式表  
   当样式需要被应用到很多页面的时候，外部样式表是理想的选择。使用外部样式表，你就可以通过更改一个文件来改变整个站点的外观。
   ```html
   <head>
   <link rel="stylesheet" type="text/css" href="mystyle.css">
   </head>
   ```

2. 内部样式表
   当单个文件需要特别样式时，就可以使用内部样式表。你可以在head部分通过`<style>`标签定义内部样式表。
   ```html
   <head>
   <style type="text/css">
   body {background-color: red}
   p {margin-left: 20px}
   </style>
   </head>
   ```
3. 内联样式
   当特殊的样式需要应用到个别元素时，就可以使用内联样式。使用内联样式的方法是在相关的标签中使用样式属性。样式属性可以包含任何CSS属性。以下实例显示出如何改变段落的颜色和左外边距。  
   ```html
   <p style="color: red; margin-left: 20px">
   This is a paragraph
   </p>
   ```

### HTML链接
HTML使用超级链接可以与网络上的另一个文档相连。  
几乎可以在所有的网页中找到链接。点击链接可以从一张页面跳转到另一张页面。  
实例：  
1. 超链接
   ```html
   <html>
   
   <body>
   
   <p>
   <a href="/index.html">本文本</a> 是一个指向本网站中的一个页面的链接。</p>
   
   <p><a href="http://www.microsoft.com/">本文本</a> 是一个指向万维网上的页面的链接。</p>
   
   </body>
   </html>
   ```
2. 用图片作为链接
   ```html
   <html>
   
   <body>
   <p>
   您也可以使用图像来作链接：
   <a href="/example/html/lastpage.html">
   <img border="0" src="/i/eg_buttonnext.gif" />
   </a>
   </p>
   
   </body>
   </html> 
   ```
   

**HTML链接-target属性**  
   使用Target属性，你可以定义被链接的文档在何处显示。  
   下面这行会在新窗口打开文档：  
   `<a href="http://www.w3school.com.cn/" taget="_blank">Visit W3School!</a>`  
**HTML链接-name属性**
name属性规定锚（anchor）的名称。  
您可以使用name属性创建HTML页面中的书签。  
书签不会以任何方式显示，它对读者是不可见的。  
当使用命名锚时，我们可以创建直接跳至该命名锚（比如页面中的某个小节）的链接，这样使用者就无需不停地滚动页面来寻找他们需要的信息了。

命名锚的语法：  
首先，我们在HTML文档中对锚进行命名（创建一个书签）  
`<a name="tips">基本的注意事项 - 有用的提示</a>`    
然后,我们在同一个文档中创建指向该锚的链接：  
`<a href="#tips">有用的提示</a>`  
您也可以在其他页面中创建指向该锚的链接：  
`<a href="http://www.w3school.com.cn/html_link.as#tips">有用的提示</a>`  
在上面的代码中，我们将#符合和锚的名称添加到了URL的末端，就可以直接链接到tips这个命名锚了。

### HTML图像
**图像标签（`<img>`）和源属性（Src）**  
在HTML中，图像由`<img>`标签定义。  
<img>是空标签，意思是说，它只包含属性，并且没有闭合标签。  
要在页面上显示图像，你需要使用源属性（src）。src指“source”。源属性的值是图像的URL地址。
定义图像的语法是：  
`<img src="url" />`  
URL指存储图像的位置。如果名为“boat.gif”的图像位于www.w3school.com的images目录中，那么其URL为http://www.w3school.com.cn/images/boat.gif.

浏览器将图像显示在文档中图像标签出现的地方。如果你将图像标签置于两个段落之间，那么浏览器会首先显示第一个段落，然后显示图片，最后显示第二段。

**替换文本属性（Alt）**
alt属性用来为图像定义一串预备的可替换的文本。替换文本属性的值是用户定义的。
<igm src = "boat.gif" alt="Big Boat">  
在浏览器无法载入图像时，替换文本属性告诉读者他们失去的信息。此时，浏览器将显示这个替代性的文本而不是图像。为页面上的图像都加上替换文本属性是个好习惯，这样有助于更好的显示信息。

1. 背景图片（本例演示如何向 HTML 页面添加背景图片。）
   ```html
   <html>
   
   <body background="/i/eg_background.jpg">
   
   <h3>图像背景</h3>
   
   <p>gif 和 jpg 文件均可用作 HTML 背景。</p>
   
   <p>如果图像小于页面，图像会进行重复。</p>
   
   </body>
   </html>
   ```
2. 排列图片（本例演示如何在文字中排列图像。）
   ```html
   <html>
   
   <body>
   
   <h2>未设置对齐方式的图像：</h2>
   
   <p>图像 <img src ="/i/eg_cute.gif"> 在文本中</p>
   
   <h2>已设置对齐方式的图像：</h2>
   
   <p>图像 <img src="/i/eg_cute.gif" align="bottom"> 在文本中</p>
   
   <p>图像 <img src ="/i/eg_cute.gif" align="middle"> 在文本中</p>
   
   <p>图像 <img src ="/i/eg_cute.gif" align="top"> 在文本中</p>
   
   <p>请注意，bottom 对齐方式是默认的对齐方式。</p>
   
   </body>
   </html>
   ```
3. 浮动图像（本例演示如何使图片浮动至段落的左边或右边。）
   ```html
   <html>
   
   <body>
   
   <p>
   <img src ="/i/eg_cute.gif" align ="left"> 
   带有图像的一个段落。图像的 align 属性设置为 "left"。图像将浮动到文本的左侧。
   </p>
   
   <p>
   <img src ="/i/eg_cute.gif" align ="right"> 
   带有图像的一个段落。图像的 align 属性设置为 "right"。图像将浮动到文本的右侧。
   </p>
   
   </body>
   </html>
   ```
4. 调整图像尺寸（本例演示如何将图片调整到不同的尺寸。）
   ```html
   <html>
   
   <body>
   
   <img src="/i/eg_mouse.jpg" width="50" height="50">
   
   <br />
   
   <img src="/i/eg_mouse.jpg" width="100" height="100">
   
   <br />
   
   <img src="/i/eg_mouse.jpg" width="200" height="200">
   
   <p>通过改变 img 标签的 "height" 和 "width" 属性的值，您可以放大或缩小图像。</p>
   
   </body>
   </html>
   ```
5. 创建图像映射（本例显示创建带有可供点击区域的图像地图。其中的每个区域都是一个超级链接。）
   ```html
   <html>
   <body>
   
   <p>请点击图像上的星球，把它们放大。</p>
   
   <img
   src="/i/eg_planets.jpg"
   border="0" usemap="#planetmap"
   alt="Planets" />
   
   <map name="planetmap" id="planetmap">
   
   <area
   shape="circle"
   coords="180,139,14"
   href ="/example/html/venus.html"
   target ="_blank"
   alt="Venus" />
   
   <area
   shape="circle"
   coords="129,161,10"
   href ="/example/html/mercur.html"
   target ="_blank"
   alt="Mercury" />
   
   <area
   shape="rect"
   coords="0,0,110,260"
   href ="/example/html/sun.html"
   target ="_blank"
   alt="Sun" />
   
   </map>
   
   <p><b>注释：</b>img 元素中的 "usemap" 属性引用 map 元素中的 "id" 或 "name" 属性（根据浏览器），所以我们同时向 map 元素添加了 "id" 和 "name" 属性。</p>
   
   </body>
   </html>
   ```
6. 把图像转换为图像映射本例显示如何把一幅普通的图像设置为图像映射.
   ```html
   <!DOCTYPE html>
   <html>
    
   <body>
   
   <p>请把鼠标移动到图像上，看一下状态栏的坐标如何变化。</p>
   
   <a href="/example/html/html_ismap.html">
   <img src="/i/eg_planets.jpg" ismap />
   </a>
   
   </body>
   </html>
   ```

### HTML表格
表格  
表格由`<table>`标签来定义。每个表格均有若干行（由`<tr>`标签定义），每行被分割为若干单元格（由`<td>`标签定义）。字母td指表格数据（table data），即数据单元格的内容。数据单元格可以包含文本、图片、列表、段落、表单、水平线、表格等等。
```html
<table border="1">
<tr>
<td>row 1, cell 1</td>
<td>row 1, cell 2</td>
</tr>
<tr>
<td>row 2, cell 1</td>
<td>row 2, cell 2</td>
<tr>
</table>
```
表格和边框属性  
如果不定义边框属性，表格将不显示边框。有时这很有用，但大多数时候，我们希望显示边框。
使用边框属性来显示一个带有边框的表格：  
```html
<table border="1">
<tr>
<td>Row 1, cell 1</td>
<td>Row 1, cell 2</td>
</tr>
</table>
```
表格中的表头
```html
<html>

<body>

<h4>表头：</h4>
<table border="1">
<tr>
  <th>姓名</th>
  <th>电话</th>
  <th>电话</th>
</tr>
<tr>
  <td>Bill Gates</td>
  <td>555 77 854</td>
  <td>555 77 855</td>
</tr>
</table>

<h4>垂直的表头：</h4>
<table border="1">
<tr>
  <th>姓名</th>
  <td>Bill Gates</td>
</tr>
<tr>
  <th>电话</th>
  <td>555 77 854</td>
</tr>
<tr>
  <th>电话</th>
  <td>555 77 855</td>
</tr>
</table>

</body>
</html>
```
空单元格
```html
<html>

<body>

<table border="1">
<tr>
  <td>Some text</td>
  <td>Some text</td>
</tr>
<tr>
  <td></td>
  <td>Some text</td>
</tr>
</table>

<p>正如您看到的，其中一个单元没有边框。这是因为它是空的。在该单元中插入一个空格后，仍然没有边框。</p>

<p>我们的技巧是在单元中插入一个 no-breaking 空格。</p>

<p>no-breaking 空格是一个字符实体。如果您不清楚什么是字符实体，请阅读关于字符实体的章节。</p>

<p>no-breaking 空格由和号开始 ("&")，然后是字符"nbsp"，并以分号结尾(";")。</p>

</body>
</html>
```
有标题的单元格
```html 
<html>

<body>

<h4>这个表格有一个标题，以及粗边框：</h4>

<table border="6">
<caption>我的标题</caption>
<tr>
  <td>100</td>
  <td>200</td>
  <td>300</td>
</tr>
<tr>
  <td>400</td>
  <td>500</td>
  <td>600</td>
</tr>
</table>

</body>
```
表格内的标签(本例演示如何显示在不同的元素内显示元素。)
```html
<html>

<body>

<table border="1">
<tr>
  <td>
   <p>这是一个段落。</p>
   <p>这是另一个段落。</p>
  </td>
  <td>这个单元包含一个表格：
   <table border="1">
   <tr>
     <td>A</td>
     <td>B</td>
   </tr>
   <tr>
     <td>C</td>
     <td>D</td>
   </tr>
   </table>
  </td>
</tr>
<tr>
  <td>这个单元包含一个列表：
   <ul>
    <li>苹果</li>
    <li>香蕉</li>
    <li>菠萝</li>
   </ul>
  </td>
  <td>HELLO</td>
</tr>
</table>

</body>
</html>
```
单元格边距
```html
<html>

<body>

<h4>没有 cellpadding：</h4>
<table border="1">
<tr>
  <td>First</td>
  <td>Row</td>
</tr>   
<tr>
  <td>Second</td>
  <td>Row</td>
</tr>
</table>

<h4>带有 cellpadding：</h4>
<table border="1" 
cellpadding="10">
<tr>
  <td>First</td>
  <td>Row</td>
</tr>   
<tr>
  <td>Second</td>
  <td>Row</td>
</tr>
</table>

</body>
</html>
```
单元格间距：
```html
<html>

<body>

<h4>没有 cellspacing：</h4>
<table border="1">
<tr>
  <td>First</td>
  <td>Row</td>
</tr>   
<tr>
  <td>Second</td>
  <td>Row</td>
</tr>
</table>

<h4>带有 cellspacing：</h4>
<table border="1" 
cellspacing="10">
<tr>
  <td>First</td>
  <td>Row</td>
</tr>   
<tr>
  <td>Second</td>
  <td>Row</td>
</tr>
</table>

</body>
</html>
```
向表格添加背景颜色或者背景图像
```html
<html>

<body>

<h4>背景颜色：</h4>
<table border="1" 
bgcolor="red">
<tr>
  <td>First</td>
  <td>Row</td>
</tr>   
<tr>
  <td>Second</td>
  <td>Row</td>
</tr>
</table>

<h4>背景图像：</h4>
<table border="1" 
background="/i/eg_bg_07.gif">
<tr>
  <td>First</td>
  <td>Row</td>
</tr>   
<tr>
  <td>Second</td>
  <td>Row</td>
</tr>
</table>

</body>
</html>
```
向单元表格添加背景颜色或者背景图像
```html
<html>

<body>

<h4>单元背景：</h4>  
<table border="1">
<tr>
  <td bgcolor="red">First</td>
  <td>Row</td>
</tr>   
<tr>
  <td 
  background="/i/eg_bg_07.gif">
  Second</td>
  <td>Row</td>
</tr>
</table>

</body>
</html>
```
在表格单元中排列内容
```html
<html>

<body>

<table width="400" border="1">
 <tr>
  <th align="left">消费项目....</th>
  <th align="right">一月</th>
  <th align="right">二月</th>
 </tr>
 <tr>
  <td align="left">衣服</td>
  <td align="right">$241.10</td>
  <td align="right">$50.20</td>
 </tr>
 <tr>
  <td align="left">化妆品</td>
  <td align="right">$30.00</td>
  <td align="right">$44.45</td>
 </tr>
 <tr>
  <td align="left">食物</td>
  <td align="right">$730.40</td>
  <td align="right">$650.00</td>
 </tr>
 <tr>
  <th align="left">总计</th>
  <th align="right">$1001.50</th>
  <th align="right">$744.65</th>
 </tr>
</table>

</body>
</html>
```
框架(frame)属性
```html
<html>
<body>

<p><b>注释：</b>frame 属性无法在 Internet Explorer 中正确地显示。</p>

<p>Table with frame="box":</p>
<table frame="box">
  <tr>
    <th>Month</th>
    <th>Savings</th>
  </tr>
  <tr>
    <td>January</td>
    <td>$100</td>
  </tr>
</table>

<p>Table with frame="above":</p>
<table frame="above">
  <tr>
    <th>Month</th>
    <th>Savings</th>
  </tr>
  <tr>
    <td>January</td>
    <td>$100</td>
  </tr>
</table>

<p>Table with frame="below":</p>
<table frame="below">
  <tr>
    <th>Month</th>
    <th>Savings</th>
  </tr>
  <tr>
    <td>January</td>
    <td>$100</td>
  </tr>
</table>

<p>Table with frame="hsides":</p>
<table frame="hsides">
  <th>Savings</th>
  </tr>
  <tr>
    <td>January</td>
    <td>$100</td>
  </tr>
</table>

<p>Table with frame="vsides":</p>
<table frame="vsides">
  <tr>
    <th>Month</th>
    <th>Savings</th>
  </tr>
  <tr>
    <td>January</td>
    <td>$100</td>
  </tr>
</table>

</body>
</html><tr>
    <th>Month</th>
```

### HTML列表
HTML支持有序、无序和定义列表  
**无序列表**  
无序列表是一个项目的列表，此列表项目使用粗体原点（典型的小黑圆圈）进行标记。  
无序列表始于`<ul>`标签，每个列表项始于`<li>`
```html
<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>
```
列表项内部可以使用段落、换行符、图片、链接以及其他列表等等。  
**有序列表**
同样，有序列表也是一列项目，列表项目使用数字进行标记。  
有序列表始于`<ol>`标签。每个列表始于`<li>`标签。
```html
<ol>
<li>Coffee</li>
<li>Milk</li>
</ol>
```
列表项内部可以使用段落、换行符、图片、链接以及其他列表等等。
**定义列表**
自定义列表不仅仅是一列项目，而是项目及其注释的组合。
自定义列表`<dl>`标签开始。每个自定义列表项以`<dt>`开始。每个自定义列表项的定义以`<dd>`开始。
```html
<dl>
<dt>Coffee</dt>
<dd>Black hot drink></dd>
<dt>Milk</dt>
<dd>White cold drink></dd>
</dl>
```
定义列表项内部可以使用段落、换行符、图片、链接以及其他列表等等。

## HTML块
**可以通过`<div>`和`<span>`将HTML元素组合起来。**  

HTML块元素：  
大多数HTML元素被定义为块级元素或内联元素。  
块级元素在浏览器显示时，通常以新行来开始（和结束）  
例子：`<h1>,<p>,<ul>,<table>`  

HTML内联元素：  
内联元素在显示时通常不会以新行开始。  
例子：`<b>,<td>,<a>,<img>`  

HTML`<div>`元素 定义文档中的分区或节（division/section）   
HTML`<div>`元素是块级元素，它是可用于组合其他HTML元素的容器。  
`<div>`元素没有特定的含义。除此之外由于它属于块级元素，浏览器会在其前后显示折行。  
如果同CSS一同使用，`<div>`元素可用于对大的内容块设置样式属性。  
`<div>`元素的另一个常见的用途是文档布局。它取代了使用表格定义布局的老式方式。使用`<table>`元素进行文档布局不是表格的正确用法。`<table>`元素的作用是显示表格化的数据。  

HTML`<span>`元素 定义 span，用来组合文档中的行内元素。   
HTML`<span>`元素是内联元素，可用作文本的容器。  
`<span>`元素也没有特定的含义。  
当与CSS一同使用时，`<span>`元素可用于为部分文本设置样式属性。  

### HTML类
对HTML进行分类(设置类)，使我们能够为元素的类定义CSS样式。  
为相同的类设置相同的样式，或者为不同的类设置不同的样式。  
实例：  
```html
<!DOCTYPE html>
<html>
<head>
<style>
.cities{
  background-color:black;
  color:white;
  margin:20px;
  padding:20px;
}
</style>
<head>

<body>

<div class="cities">
<h2>London</h2>
<p>
London is the capital city of England.
It is the most populous city in the United Kindom,
with a metropolitan area of over 13 million inhabitants.
</p>
</div>

</body>
</html>
```
分类块级元素  
HTML `<div>` 元素是块级元素。它能够用作其他 HTML 元素的容器。

设置 `<div>` 元素的类，使我们能够为相同的 `<div>` 元素设置相同的类。
```html
<!DOCTYPE html>
<html>
<head>
<style>
.cities{
  background-color:black;
  color:white;
  margin:20px;
  padding:20px;
}
</style>
</head>

<body>

<div class="cities">
<h2>London</h2>
<p>London is the capital city of England.
It is the most populous citu in the United Kindom,
with a metropolitan area of over 13 million inhabitants.</p>
</div>

<div class="cities">
<h2>Paris</h2>
<p>Paris is the capital and most populous city of France.</p>
</div>

</body>
</html>
```
分类行内元素
HTML`<span>`元素是行内元素，能够用作文本的容器。  
设置`<span>`元素的类，能够为相同的`<span>`元素设置相同的样式。  
```html
<!DOCTYPE html>
<html>
<head>
<style>
  span.red{color:red}
</style>
</head>
<body>

<h1>My<span class = "red">Important</span> Heading</h1>

</body>
</html>
```

### HTML布局
网站常常以多列显示内容（就像杂志和报纸）  
使用`<div>`元素的HTML布局  
注释：`<div>`元素常用作布局工具，因为能够轻松地通过CSS对其进行定位。  
这个例子使用了四个`<div>`元素来创建多列布局：
```html
<body>

<div id="header">
<h1>City Gallery</h1>
</div>

<div id="nav">
London<br>
Paris<br>
Tokyo<br>
</div>

<div id="section">
<h1>London</h1>
<p>
London is the capital city of England. It is the most populous city in the United Kingdom, with a metropolitan area of over 13 million inhabitants.
</p>
<p>
Standing on the River Thames, London has been a major settlement for two millennia, its history going back to its founding by the Romans, who named it Londinium.
</p>
</div>

<div id="footer">
Copyright W3school.com.cn
</div>

</body>
```
CSS
```css
<style>
#header{
  background-color:black;
  color:white;
  text-align:center;
  padding:5px;
}
#nav{
  line-height:30px;
  background-color:#eeeeee;
  height:300px;
  width:100px;
  float:left;
  padding:5px;
}
#section{
  width:350px;
  float:left;
  padding:10px;
}
#footer{
  background-color:black;
  color:white;
  clear:both;
  text-align:center;
  padding:5px;
}
</style>
```
使用HTML5的网站布局  
HTML5提供了新语义元素定义了网页的不同部分：  
|header|定义文档或节的页眉|
|----|----|
|nav|定义导航链接的容器|
|section|定义文档中的节|
|article|定义独立的自包含文章|
|aside|定义内容之外的内容（例如侧栏）|
|footer|定义文档或节的页脚|
|details|定义额外的细节|
|summary|定义details元素的标题|
使用表格的 HTML 布局：
注释：`<table>` 元素不是作为布局工具而设计的。  

`<table>` 元素的作用是显示表格化的数据。  

使用 `<table>` 元素能够取得布局效果，因为能够通过 CSS 设置表格元素的样式：  
```html
<body>

<table class="lamp">
<tr>
  <th>
     <img src="/images/lamp.jpg" alt="Ndte" style="height:32px; wodth: 32px">
  </th>
  <td>
     The table element was not designed to be a layout tool.
  </td>
<tr>
</table>

</body>
```
CSS
```css
<style>
table.lamp{
  width:100%;
  border:1px solid #d4d4d4;
}
table.lamp th, td{
  padding:10px;
}
table.lamp td}{
  width:40px;
}
</style>
```

### HTML响应式设计
* RWD指的是响应式Web设计（Responsive Web Design）
* RWD能够以可变尺寸传递网页
* RWD对于平板和移动设备是必需的

创建您自己的响应式设计  
创建响应式设计的一个方法，是自己来创建它：  
```html
<!DOCTYPE html>
<html lang="en-US">
<head>
<style>
.city{
  float:left;
  margin: 5px;
  padding: 15px;
  width:300px;
  height: 300px;
  border: 1px solid black;
}
</style>
</head>

<body>

<h1>W3School Demo</h1>
<h2>Resize this reponsible page!</h2>
<br>

<div class="city">
<h2>London</h2>
<p>London is the capital city of England.</p>
<p>It is the most populous city in the United Kingdom,
with a metropolitan area of over 13 million inhabitants.</p>
</div>

<div class="city">
<h2>Paris</h2>
<p>Paris is the capital and most populous city of France.</p>
</div>

<div class="city">
<h2>Tokyo</h2>
<p>Tokyo is the capital of Japan, the center of the Greater Tokyo Area,
and the most populous metropolitan area in the world.</p>
</div>

</body>
</html>
```
使用Bootstrap  
另一个创建响应式设计的方法，是使用现成的CSS框架。  
Bootstrsap是最流行的开发响应式web的HTML，CSS和JS框架。
Bootstrap帮助您开发在任何尺寸都外观出众的站点。
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <link rel="stylesheet"
  href="http://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
</head>

<div class="container">
<div class="jumbotron">
  <h1>W3School Demo</h1>
  <p>Resize this reponsive page</p>
</div>
</div>

<div class="container">
<div class="row">
  <div class="col-md-4">
    <h2>London</h2>
    <p>London is the capital city of England.</p>
    <p>It is the most populous city in the United Kingdom,
    with a metropolitan area of over 13 million inhabitants.</p>
  </div>
  <div class="col-md-4">
    <h2>Paris</h2>
    <p>Paris is the capital and most populous city of France.</p>
  </div>
  <div class="col-md-4">
    <h2>Tokyo</h2>
    <p>Tokyo is the capital of Japan, the center of the Greater Tokyo Area,
    and the most populous metropolitan area in the world.</p>
  </div>
</div>
</div>

</body>
</html>
```
### HTML框架
通过使用框架，你可以在同一个浏览器窗口中显示不止一个页面。  

垂直框架   
本例演示：如何使用三份不同的文档制作一个垂直框架。
```html
<html>

<frameset cols="25%,50%,25%">

  <frame src="/example/html/frame_a.html">
  <frame src="/example/html/frame_b.html">
  <frame src="/example/html/frame_c.html">

</frameset>

</html>
```
水平框架  
本例演示：如何使用三份不同的文档制作一个水平框架。
```html
<html>

<frameset rows="25%,50%,25%">

  <frame src="/example/html/frame_a.html">
  <frame src="/example/html/frame_b.html">
  <frame src="/example/html/frame_c.html">

</frameset>

</html>
```
**框架**  
通过使用框架，你可以在同一个浏览器窗口中显示不止一个页面。每份HTML文档称为一个框架，并且每个框架都独立于其他的框架。  
使用框架的坏处：  
* 开发人员必须同时跟踪更多的HTML文档
* 很难去打印整张页面
框架结构标签`<frameset>`
* 框架结构标签`<frameset>`定义如何将窗口分割为框架
* 每个frameset定义了一系列行或列  
* rows/columns的值规定了每行或每列占据屏幕的面积

**框架标签（Frame）**  
Frame标签定义了放置在每个框架中的HTML文件。  
在下面的这个例子中，我们设置了一个两列的框架集。第一列被设置占据浏览器窗口的25%。第二列被设置为占据浏览器窗口的75%。HTML文档“frame_a.htm”被置于第一个列中，而HTML文档"frame_b.htm"被置于第二个列中：
```html
<frameset cols="25%,75%">
  <frame src="frame_a.htm">
  <frame src="frame_b.htm">
<frameset>
```
假如一个框架有可见边框，用户可以拖动边框来改变它的大小。为了避免这种情况发生，可以在 `<frame>` 标签中加入：noresize="noresize"。    
为不支持框架的浏览器添加 `<noframes>` 标签。    
重要提示：不能将 `<body></body>` 标签与 `<frameset></frameset>` 标签同时使用！不过，假如你添加包含一段文本的 `<noframes>` 标签，就必须将这段文字嵌套于 `<body></body>` 标签内。（在下面的第一个实例中，可以查看它是如何实现的。）  
如何使用 `<noframes>` 标签
```html
<html>

<frameset cols="25%,50%,25%">
  <frame src="/example/html/frame_a.html">
  <frame src="/example/html/frame_b.html">
  <frame src="/example/html/frame_c.html">

<noframes>
<body>您的浏览器无法处理框架！</body>
</noframes>

</frameset>

</html>
```
混合框架结构
```html
<html>

<frameset rows="50%,50%">

<frame src="/example/html/frame_a.html">

<frameset cols="25%,75%">
<frame src="/example/html/frame_b.html">
<frame src="/example/html/frame_c.html">
</frameset>

</frameset>

</html>
```
含有 noresize="noresize" 属性的框架结构
```html
<html>

<frameset cols="50%,*,25%">
  <frame src="/example/html/frame_a.html" noresize="noresize" />
  <frame src="/example/html/frame_b.html" />
  <frame src="/example/html/frame_c.html" />
</frameset>

</html>
```
导航框架  
本例演示如何制作导航框架。导航框架包含一个将第二个框架作为目标的链接列表。名为 "contents.htm" 的文件包含三个链接。  
```html
<html>

<frameset cols="120,*">

  <frame src="/example/html/html_contents.html">
  <frame src="/example/html/frame_a.html" name="showframe">

</frameset>

</html>
```
内联框架  
```html
<html>

<body>

<iframe src="/i/eg_landscape.jpg"></iframe>

<p>一些老的浏览器不支持 iframe。</p>
<p>如果得不到支持，iframe 是不可见的。</p>


</body>
</html>
```
跳转至框架内的一个指定的节  
本例演示两个框架。其中的一个框架设置了指向另一个文件内指定的节的链接。这个"link.htm"文件内指定的节使用 `<a name="C10">` 进行标识。  
```html
<html>

<frameset cols="20%,80%">

 <frame src="/example/html/frame_a.html">
 <frame src="/example/html/link.html#C10">

</frameset>

</html>
```
使用框架导航跳转至指定的节  
本例演示两个框架。左侧的导航框架包含了一个链接列表，这些链接将第二个框架作为目标。第二个框架显示被链接的文档。导航框架其中的链接指向目标文件中指定的节。  
```html
<html>

<frameset cols="180,*">

<frame src="/example/html/content.html">
<frame src="/example/html/link.html" name="showframe">

</frameset>

</html>
```

### HTML内联框架 
添加iframe的语法  
`<iframe src="URL"><iframe>`  
URL指向隔离页面的位置  

iframe设置高度和宽度  
height和width属性用于规定iframe的高度和宽度。  
属性值的默认单位是像素，但也可以用百分比来设定（比如80%）  
`<iframe src="demo_iframe.htm" width="200" height="200"></iframe>`

iframe删除边框  
frameborder属性规定是否显示iframe周围的边框。  
设置属性值为"0"就可以移除边框：
`<iframe src="demo_iframe.htm" frameborder="0"></iframe>`

使用iframe作为链接的目标  
iframe可用作链接的目标（target）  
链接的target属性必须引用iframe的name属性：
```html
<iframe src="demo_iframe.htm" name="iframe_a"></iframe>
<p><a href="http://www.w3school.com.cn" target="iframe_a">W3School.com.cn</a></p>
```

### HTML背景
`<body>`拥有两个配置背景的标签。背景可以是颜色或者图像。  
背景颜色（Bgcolor）   
背景颜色属性将背景设置为某种颜色。属性值可以是十六进制数、RGB值或颜色名。  
```html
<body bgcolor="#000000">
<body bgcolor="rgb(0,0,0)">
<body bgcolor="black">
```
以上的代码均将背景颜色设置为黑色。  

背景（Background）  
背景属性将背景设置为图像。属性值为图形的URL。如果图像尺寸小于浏览器窗口，那么图像将在整个浏览器窗口进行复制。  
```html
<body background="clouds.gif">
<body background="http://www.w3school.com.cn/clouds.gif">
```
URL可以是相对地址，如第一行代码。也可以使绝对地址，如第二行代码。  

提示：如果你打算使用背景图片，你需要紧记一下几点：  

* 背景图像是否增加了页面的加载时间。小贴士：图像文件不应超过 10k。
* 背景图像是否与页面中的其他图象搭配良好。
* 背景图像是否与页面中的文字颜色搭配良好。
* 图像在页面中平铺后，看上去还可以吗？
* 对文字的注意力被背景图像喧宾夺主了吗？  

### HTML脚本
`<script>`标签用于定义客户端脚本，比如JavaScript。  
scrpt元素既可以包含脚本语句，也可以通过src属性指向外部脚本文件。  
必需的Type属性规定脚本的MIME类型。   
Javascript最常用于图片操作、表单验证以及内容动态更新。  
下面的脚本会向浏览器输出"Hello World"：  
```html
<script type="text/javascript">
document.write("Hello World!")
</script>
```

`<noscript>`标签   
`<noscript>` 标签提供无法使用脚本时的替代内容，比方在浏览器禁用脚本时，或浏览器不支持客户端脚本时。  

noscript 元素可包含普通 HTML 页面的 body 元素中能够找到的所有元素。  

只有在浏览器不支持脚本或者禁用脚本时，才会显示 noscript 元素中的内容：  
```html
<script type="text/javascript">
document.write("Hello World!")
</script>
<noscript>Your browser does not support JavaScript!</noscript>
```

### HTML文件路径
|路径|描述|
|----|----|
|`<img src="picture.jpg">`|picture.jpg 位于与当前网页相同的文件夹|
|`<img src="images/picture.jpg">`|picture.jpg 位于当前文件夹的 images 文件夹中|
|`<img src="/images/picture.jpg">`|picture.jpg 当前站点根目录的 images 文件夹中|
|`<img src="../../../picture.jpg">`|	picture.jpg 位于当前文件夹的上一级文件夹中|

HTML文件路径  
文件路径描述了网站文件夹结构中某个文件的位置。  

文件路径会在链接外部文件时被用到：  

* 网页
* 图像
* 样式表
* JavaScript

### HTML头部元素
`<head>` 元素是所有头部元素的容器。`<head>` 内的元素可包含脚本，指示浏览器在何处可以找到样式表，提供元信息，等等。  
以下标签都可以添加到 head 部分：`<title>`、`<base>`、`<link>`、`<meta>`、`<script>` 以及 `<style`>。

**HTML`<title>`元素**  
`<`title>` 标签定义文档的标题。  
title 元素在所有 HTML/XHTML 文档中都是必需的。  
title 元素能够：
* 定义浏览器工具栏中的标题
* 提供页面被添加到收藏夹时显示的标题
* 显示在搜索引擎结果中的页面标题
```html
<!DOCTYPE html>
<html>
<head>
<title>Title of the document</title>
</head>

<body>
The content of the document......
</body>

</html>
```

**HTML `<base>` 元素**  
`<base>` 标签为页面上的所有链接规定默认地址或默认目标（target）：
```html
<head>
<base href="http://www.w3school.com.cn/images/" />
<base target="_blank" />
</head>
```

**HTML`<link>` 元素**  
`<link>` 标签定义文档与外部资源之间的关系。  
`<link>` 标签最常用于连接样式表：  
```html
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css" />
</head>
```

**HTML `<style>` 元素**  
`<style>` 标签用于为 HTML 文档定义样式信息。  
您可以在 `style` 元素内规定 HTML 元素在浏览器中呈现的样式：  
```html
<head>
<style type="text/css">
body {background-color:yellow}
p {color:blue}
</style>
</head>
```

**HTML `<meta>` 元素**
元数据（metadata）是关于数据的信息。  
`<meta>`标签提供关于 HTML 文档的元数据。元数据不会显示在页面上，但是对于机器是可读的。  
典型的情况是，meta 元素被用于规定页面的描述、关键词、文档的作者、最后修改时间以及其他元数据。  
`<meta>` 标签始终位于 head 元素中。  
元数据可用于浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他 web 服务。  

针对搜索引擎的关键词  
一些搜索引擎会利用 meta 元素的 name 和 content 属性来索引您的页面。  
下面的 meta 元素定义页面的描述：  
`<meta name="description" content="Free Web tutorials on HTML, CSS, XML" />`  
下面的 meta 元素定义页面的关键词：  
`<meta name="keywords" content="HTML, CSS, XML" />`  
name 和 content 属性的作用是描述页面的内容。  

### HTML字符实体  
HTML中的预留字符必须被替换为字符实体。  
**HTML 实体**
在 HTML 中，某些字符是预留的。  
在 HTML 中不能使用小于号（<）和大于号（>），这是因为浏览器会误认为它们是标签。  
如果希望正确地显示预留字符，我们必须在 HTML 源代码中使用字符实体（character entities）。  
字符实体类似这样：  
```
&entity_name;

或者

&#entity_number;
```
如需显示小于号，我们必须这样写：`&lt`; 或 `&#60`;  

**不间断空格（non-breaking space）**  
HTML 中的常用字符实体是不间断空格(`&nbsp;`)。  
浏览器总是会截短 HTML 页面中的空格。如果您在文本中写 10 个空格，在显示该页面之前，浏览器会删除它们中的 9 个。如需在页面中增加空格的数量，您需要使用 `&nbsp`; 字符实体。 

### HTML URL
URL 也被称为网址  (URL - Uniform Resource Locator)  
当您点击 HTML 页面中的某个链接时，对应的 `<a>` 标签指向万维网上的一个地址。  
统一资源定位器（URL）用于定位万维网上的文档（或其他数据）。  
网址，比如 `http://www.w3school.com.cn/html/index.asp`，遵守以下的语法规则：  
`scheme://host.domain:port/path/filename`  
解释：  
* scheme - 定义因特网服务的类型。最常见的类型是 http
* host - 定义域主机（http 的默认主机是 www）
* domain - 定义因特网域名，比如 w3school.com.cn
* :port - 定义主机上的端口号（http 的默认端口号是 80）
* path - 定义服务器上的路径（如果省略，则文档必须位于网站的根目录中）。
* filename - 定义文档/资源的名称

|Scheme|访问|用于...|
|----|----|----|
|http|超文本传输协议|以 http:// 开头的普通网页。不加密|
|https|安全超文本传输协议|安全网页。加密所有信息交换|
|ftp|文件传输协议|用于将文件下载或上传至网站|
|file||您计算机上的文件|

## 常用的声明  
HTML5  
`<!DOCTYPE html>`  
HTML 4.01  
`<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">`   
XHTML 1.0  
`<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">`

## HTML表单  

### HTML表单
HTML表单用于搜集不同类型用户输入  
`<form>`元素  
HTML表单用于收集用户输入。  
`<form>`元素定义HTML表单：  
实例：  
```html
<form>
.
form elements
.
</form>
```
HTML表单包含单元素  
表单元素指的是不同类型的input元素、复选框、单选按钮、提交按钮等。  
`<input>`元素  
`<input>`元素是最重要的表单元素。  
`<input>`元素有很多形态，根据不同的tpye属性。  
这是本章使用的类型：
* text 定义常规文本输入
* radio 定义单选按钮输入（选择多个选择之一）
* submit 定义提交按钮（提交表单）  

文本输入  
`<input type="text">` 定义用于文本输入的单行输入字段：  
```html
<form>
 First name:<br>
<input type="text" name="firstname">
<br>
 Last name:<br>
<input type="text" name="lastname">
</form>
```
单选按钮输入   
`<input type="radio">` 定义单选按钮。  
单选按钮允许用户在有限数量的选项中选择其中之一：  
实例：  
```html
<form>
<input type="radio" name="sex" value="male" checked>Male
<br>
<input type="radio" name="sex" value="female">Female
</form>
```
提交按钮  
`<input type="submit">` 定义用于向表单处理程序（form-handler）提交表单的按钮。

表单处理程序通常是包含用来处理输入数据的脚本的服务器页面。

表单处理程序在表单的 action 属性中指定：

实例 
```html
<form action="action_page.php">
First name:<br>
<input type="text" name="firstname" value="Mickey">
<br>
Last name:<br>
<input type="text" name="lastname" value="Mouse">
<br><br>
<input type="submit" value="Submit">
</form>
```
Action 属性  
action 属性定义在提交表单时执行的动作。  
向服务器提交表单的通常做法是使用提交按钮。  
通常，表单会被提交到 web 服务器上的网页。  
在上面的例子中，指定了某个服务器脚本来处理被提交表单：  
`<form action="action_page.php">`
如果省略 action 属性，则 action 会被设置为当前页面。  

Method 属性  
method 属性规定在提交表单时所用的 HTTP 方法（GET 或 POST）：  
`<form action="action_page.php" method="GET">`  
`<form action="action_page.php" method="POST">`  
**何时使用 GET？**  
您能够使用 GET（默认方法）：  
如果表单提交是被动的（比如搜索引擎查询），并且没有敏感信息。  
当您使用 GET 时，表单数据在页面地址栏中是可见的：  
`action_page.php?firstname=Mickey&lastname=Mouse`  
注释：GET 最适合少量数据的提交。浏览器会设定容量限制。  
**何时使用 POST？**
您应该使用 POST：  
如果表单正在更新数据，或者包含敏感信息（例如密码）。  
POST 的安全性更加，因为在页面地址栏中被提交的数据是不可见的。   

Name 属性  
如果要正确地被提交，每个输入字段必须设置一个 name 属性。  
本例只会提交 "Last name" 输入字段：  
```html
<form action="action_page.php">
First name:<br>
<input type="text" value="Mickey">
<br>
Last name:<br>
<input type="text" name="lastname" value="Mouse">
<br><br>
<input type="submit" value="Submit">
</form>
```

用 `<fieldset>` 组合表单数据    
`<fieldset>` 元素组合表单中的相关数据  

`<legend>` 元素为 `<fieldset>` 元素定义标题。  
```html
<form action="action_page.php">
<fieldset>
<legend>Personal information:</legend>
First name:<br>
<input type="text" name="firstname" value="Mickey">
<br>
Last name:<br>
<input type="text" name="lastname" value="Mouse">
<br><br>
<input type="submit" value="Submit"></fieldset>
</form> 
```
* accept-charset	规定在被提交表单中使用的字符集（默认：页面字符集）。
* action	规定向何处提交表单的地址（URL）（提交页面）。
* autocomplete	规定浏览器应该自动完成表单（默认：开启）。
* enctype	规定被提交数据的编码（默认：`url-encoded`）。
* method	规定在提交表单时所用的 HTTP 方法（默认：GET）。
* name	规定识别表单的名称（对于 DOM 使用：`document.forms.name`）。
* novalidate	规定浏览器不验证表单。
* target	规定 action 属性中地址的目标（默认：_self）

### HTML表单元素
`<input>` 元素
最重要的表单元素是`<input>` 元素。  
`<input>`元素根据不同的 type 属性，可以变化为多种形态。

`<select>` 元素（下拉列表）  
`<select>` 元素定义下拉列表：  
```html
<select name="cars">
<option value="volvo">Volvo</option>
<option value="saab">Saab</option>
<option value="fiat">Fiat</option>
<option value="audi">Audi</option>
</select>
```
`<option>` 元素定义待选择的选项。    
列表通常会把首个选项显示为被选选项。    
您能够通过添加 selected 属性来定义预定义选项。  

`<textarea>` 元素
`<textarea>` 元素定义多行输入字段（文本域）：  
```html
<textarea name="message" rows="10" cols="30">
The cat was playing in the garden.
</textarea>
```

`<button>` 元素  
`<button>` 元素定义可点击的按钮：  
`<button type="button" onclick="alert('Hello World!')">Click Me!</button>`  

HTML5 表单元素  
HTML5 增加了如下表单元素：  
`<datalist>`  
`<keygen>`  
`<output>`  
注释：默认地，浏览器不会显示未知元素。新元素不会破坏您的页面    
`<datalist> `元素为 `<input>` 元素规定预定义选项列表。  
用户会在他们输入数据时看到预定义选项的下拉列表。  
`<input>` 元素的 list 属性必须引用 `<datalist>` 元素的 id 属性。  
```html
<form action="action_page.php">
<input list="browsers">
<datalist id="browsers">
   <option value="Internet Explorer">
   <option value="Firefox">
   <option value="Chrome">
   <option value="Opera">
   <option value="Safari">
</datalist> 
</form>
```

### HTML画布  
canvas 元素用于在网页上绘制图形。  
HTML5 的 canvas 元素使用 JavaScript 在网页上绘制图像。  
画布是一个矩形区域，您可以控制其每一像素。  
canvas 拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法。  

向 HTML5 页面添加 canvas 元素。  
规定元素的 id、宽度和高度：  
```html
<canvas id="myCanvas" width="200" height="100"></canvas>
```
通过 JavaScript 来绘制  
canvas 元素本身是没有绘图能力的。所有的绘制工作必须在 JavaScript 内部完成：  
```html
<script type="text/javascript">
var c=document.getElementById("myCanvas");
var cxt=c.getContext("2d");
cxt.fillStyle="#FF0000";
cxt.fillRect(0,0,150,75);
</script>
```
JavaScript 使用 id 来寻找 canvas 元素：   
`var c=document.getElementById("myCanvas")`  
然后，创建 context 对象：    
`var cxt=c.getContext("2d")`  
getContext("2d") 对象是内建的 HTML5 对象，拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法。  
下面的两行代码绘制一个红色的矩形：  
```html
cxt.fillStyle="#FF0000";
cxt.fillRect(0,0,150,75);
```
fillStyle 方法将其染成红色，fillRect 方法规定了形状、位置和尺寸。  
上面的 fillRect 方法拥有参数 (0,0,150,75)。  
意思是：在画布上绘制 150x75 的矩形，从左上角开始 (0,0)。  
如下图所示，画布的 X 和 Y 坐标用于在画布上对绘画进行定位。  

### HTML对象
`<object>` 的作用是支持 HTML 助手（插件）。  
HTML 助手（插件）  
辅助应用程序（helper application）是可由浏览器启动的程序。辅助应用程序也称为插件。  
辅助程序可用于播放音频和视频（以及其他）。辅助程序是使用 `<object>` 标签来加载的。  
使用辅助程序播放视频和音频的一个优势是，您能够允许用户来控制部分或全部播放设置。  
大多数辅助应用程序允许对音量设置和播放功能（比如后退、暂停、停止和播放）的手工（或程序的）控制。  

