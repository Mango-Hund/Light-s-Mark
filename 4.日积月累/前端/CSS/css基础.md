## CSS 基础教程

### CSS简介
CSS概述:  
* CSS指层叠样式表(Cascading Style Sheets)
* 样式定义如何显示HTML元素
* 样式通常存储在样式表中
* 把样式添加到HTML4.0中,是为了解决内容与表现分离的问题
* 外部样式表可以极大提高工作效率
* 外部样式表通常存储在CSS文件中
* 多个样式可层叠为一

### CSS基础语法
CSS规则由两个主要的部分构成:选择器,以及一条或多条声明.  
`selector{declaration;declaration2;...declarationN}`  
选择器通常是需要改变样式的HTML元素.  
每条声明由一个属性和一个值组成.  
属性是希望设置的样式属性(style attribute).每个属性有一个值.属性和值被冒号分开.    
`selector{property:value}`  
下面这行代码的作用是将h1元素内的文字颜色定义为红色,同时将字体大小设置为14像素.  
在这个例子中,h1是选择器,color和font-size是属性,red和14px是值.    
`h1{color:red; font-size:14px;}` 

**值的不同写法和单位**  
除了英文单词red,我们还可以使用十六进制的颜色值#ff0000:  
`p{color:#ff0000;}`  
为了节约字节,我们可以使用CSS的缩写形式:  
`p{color:#f00}`  
我们还可以通过两种方法使用RGB值:    
`p{color:rgb(255,0,0)}`  
`p{color:rgb(100%,0%,0%)}`  
请注意,当使用RGB百分比时,即使当值为0时也要写百分比符号.但是在其他的情况下就不需要这么做了.比如说,当尺寸为0像素时,0之后不需要使用px单位  

记得写引号  
如果值为若干单词,则要给值加引号:    
`p{font-family:"sans serif";}`    

**多重声明:**  
如果定义不止一个声明,则需要用分好把每个声明分开.下面的例子展示出如何定义一个红色文字的居中段落.最后一条规则是不需要加分号的,因为分号在英语中是一个分隔符合,不是结束符号.然而,大多数有经验的设计师会在每条声明的末尾都加上分号,这么做的好处是,当你从现有的规则中增减声明时,会尽可能地减少出错的可能性.    
`p{text-align:center; color:red}`  
你应该在每行只描述一个属性,这样可以增强样式定义的可读性,就像这样:  
```css
p {
  text-align: center;
  color: black;
  font-family: arial;
}
```

**空格和大小写**  
大多数样式表包含不止一条规则,而大多数规则包含不止一个声明.多重声明和空格的使用使得样式表更容易被编辑:  
```css
body {
  color: #000;
  background: #fff;
  margin: 0;
  padding: 0;
  font-family: Georgia, Palatino, serif;
  }
```
是否包含空格不会影响CSS在浏览器的工作效果,同样,与XHTML不同,CSS对大小写不敏感.不过存在一个例外:如果涉及到与HTML文档一起工作的话,class和id名称对大小写是敏感的.  

### CSS高级语法
**选择器的分组**   
你可以对选择器进行分组,被分组的选择器可以分享相同的声明.用逗号将需要分组的选择器分开.在下面的例子中,我们对所有的标题元素进行了分组.所有的标题元素都是绿色的.  
```css
h1,h2,h3,h4,h5,h6{
    color:green;  
}
```

**继承及其问题**  
根据CSS,子元素从父元素继承属性.但是它并不总按照此方式工作.  
```css
body{
    font-family:Verdana,sans-serif;  
}
```
根据上面这条规则,站点的body元素将使用Verdana字体(假设访问者的系统中存在该字体的话).  
通过CSS继承,子元素将继承最高级元素(在本例中是body)多拥有的属性(这些子元素诸如p,td,ul,li,dl,dt和dd)不需要另外的规则.所有body的子元素都应该显示Verdana字体,子元素的子元素也是一样.  
但是有一些旧浏览器无法理解继承,就需要单独再去写一遍规则.  

如果你不希望 "Verdana, sans-serif" 字体被所有的子元素继承，又该怎么做呢？比方说，你希望段落的字体是 Times。没问题。创建一个针对 p 的特殊规则，这样它就会摆脱父元素的规则：  
```css
body  {
     font-family: Verdana, sans-serif;
     }

td, ul, ol, ul, li, dl, dt, dd  {
     font-family: Verdana, sans-serif;
     }

p  {
     font-family: Times, "Times New Roman", serif;
     }
```

### CSS派生选择器  
**通过依据元素在其位置上下文关系来定义样式,你可以使得标记更加简洁** 
在CSS1中,通过这种方式来应用规则的选择器被称为上下文选择器(contextual selectors),这是由于它们依赖上下文关系来应用或者避免某项规则.在CSS2中,它们被称为派生选择器.  

派生选择器允许你根据文档的上下文关系来确定某个标签的样式.通过合理地使用派生选择器,我们可以使HTML代码变得更加整洁.  

比方说,你希望列表中的strong元素变为斜体字,而不是通常的粗体字,可以定义: 
```css
li strong{
    font-style:italic;
    font-weight:normal;
}
```
请注意标记为`<strong>`的蓝色代码的上下文关系:  
```
<p><strong>我是粗体字，不是斜体字，因为我不在列表当中，所以这个规则对我不起作用</strong></p>

<ol>
<li><strong>我是斜体字。这是因为 strong 元素位于 li 元素内。</strong></li>
<li>我是正常的字体。</li>
</ol>
```
在上面的例子中，只有 li 元素中的 strong 元素的样式为斜体字，无需为 strong 元素定义特别的 class 或 id，代码更加简洁.  

再看一下下面的CSS规则:  
```css
strong {
     color: red;
     }

h2 {
     color: red;
     }

h2 strong {
     color: blue;
     }
```
下面是它施加影响的HTML:  
```css
<p>The strongly emphasized word in this paragraph is<strong>red</strong>.</p>
<h2>This subhead is also red.</h2>
<h2>The strongly emphasized word in this subhead is<strong>blue</strong>.</h2>
```

### CSS id选择器  
id选择器可以为标有特定id的HTML元素指定特定的样式.   
id选择器以"#"来定义.  
下面的两个id选择器,第一个可以定义元素的颜色为红色,第二个定义元素的颜色为绿色:  
```css
#red {color:red;}
#green {color:green;}
```
下面的 HTML 代码中，id 属性为 red 的 p 元素显示为红色，而 id 属性为 green 的 p 元素显示为绿色。  
```css
<p id="red">这个段落是红色的.</p>  
<p id="green">这个段落是绿色的</p>  
```
id属性只能在每个HTML文档中出现一次.  

在现在布局中,id选择器常常用来构建派生选择器.  
```css
#sidebar p {
	font-style: italic;
	text-align: right;
	margin-top: 0.5em;
    }
```
上面的样式只会应用于出现在 id 是 sidebar 的元素内的段落。这个元素很可能是 div 或者是表格单元，尽管它也可能是一个表格或者其他块级元素。它甚至可以是一个内联元素，比如 `<em></em>` 或者 `<span></span>`，不过这样的用法是非法的，因为不可以在内联元素 `<span> `中嵌入 `<p>`.  

**一个选择器, 多种用法**  
即使被标注为 sidebar 的元素只能在文档中出现一次，这个 id 选择器作为派生选择器也可以被使用很多次：  
```css
#sidebar p {
	font-style: italic;
	text-align: right;
	margin-top: 0.5em;
	}

#sidebar h2 {
	font-size: 1em;
	font-weight: normal;
	font-style: italic;
	margin: 0;
	line-height: 1.5;
	text-align: right;
	}
```
在这里，与页面中的其他 p 元素明显不同的是，sidebar 内的 p 元素得到了特殊的处理，同时，与页面中其他所有 h2 元素明显不同的是，sidebar 中的 h2 元素也得到了不同的特殊处理。  

**单独的选择器**  
id选择器即使不被用来创建派生选择器,它也可以独立发挥作用:  
```css
#sidebar {
	border: 1px dotted #000;
	padding: 10px;
	}
```
根据这条规则，id 为 sidebar 的元素将拥有一个像素宽的黑色点状边框，同时其周围会有 10 个像素宽的内边距（padding，内部空白）。老版本的 Windows/IE 浏览器可能会忽略这条规则，除非你特别地定义这个选择器所属的元素  
```css
div#sidebar {
	border: 1px dotted #000;
	padding: 10px;
	}
```

### CSS类选择器
在CSS中,类选择器以一个点号显示:  
```css
.center{text-align:center}
```
在上面的例子中,所有拥有center类的HTML元素均为居中.  
在下面的HTML代码中,h1和p元素都有center类,这意味着两者都遵守"center"选择器中的规则.  
```css
<h1 class="center">
This heading will be center-alignd
</h1>

<p class="center">
This paragraph will also be center-aligned.
</p>
```
类名的第一个字符不能使用数字！它无法在 Mozilla 或 Firefox 中起作用  
和 id 一样，class 也可被用作派生选择器:  
```css
.fancy td{
    color:#f60;
    background:#666;
}
```
在上面这个例子中，类名为 fancy 的更大的元素内部的表格单元都会以灰色背景显示橙色文字。（名为 fancy 的更大的元素可能是一个表格或者一个 div）  

元素也可以基于它们的类而被选择：
```css
td.fancy {
	color: #f60;
	background: #666;
	}
```
在上面的例子中，类名为 fancy 的表格单元将是带有灰色背景的橙色。  

### CSS属性选择器
对带有指定属性的HTML元素设置样式.  
可以为拥有指定属性的HTML元素设置样式,而不仅限于class和id属性.  

**属性选择器**  
下面的例子为带有 title 属性的所有元素设置样式：  
```css
[title]
{
color:red;
}
```

**属性和值选择器**  
下面的例子为 title="W3School" 的所有元素设置样式：  
```css
[title=W3School]
{
border:5px solid blue;
}
```

**属性和值选择器 - 多个值**  
下面的例子为包含指定值的 title 属性的所有元素设置样式。适用于由空格分隔的属性值：  
```css
[title~=hello]{color:red;}
```
下面的例子为带有包含特定值的lang属性的所有元素设置样式.适用于由连字符分隔的属性值.
```css
[lang|=en]{color:red;}
```

**设置表单的样式**   
属性选择器在为不带有class和id的表单设置样式时特别有用:  
```css
input[type="text"]
{
  width:150px;
  display:block;
  margin-bottom:10px;
  background-color:yellow;
  font-family: Verdana, Arial;
}

input[type="button"]
{
  width:120px;
  margin-left:35px;
  display:block;
  font-family: Verdana, Arial;
}
```

### 如何创建CSS
如何插入样式表  
当读到一个样式表时,浏览器会根据它格式化HTML文档.  
外部样式表:  
当样式需要应用很多页面时,外部样式表是理想的选择.在使用外部样式表的情况下,你可以通过改变一个文件来改变整个网站的外观.每个页面使用`<link>`标签链接到样式表.`<link>`标签在(文档的)头部:  
```css
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css"/>
</head>
```
浏览器会从文件mystyle.css中读到样式声明,并根据它来格式文档.  
外部样式表可以在任何文本编辑器中进行编辑.文件不能包含的html标签,样式表应该以.css扩展名进行保存.下面是一个样式表的例子:  
```css
hr{color:sienna;}
p{margin-left:20px;}
body{background-image:url("images/back40.gif");}
```
不要在属性值与单位之间留有空格.  

**内部样式表**   
当单个文档需要特殊的样式时,就应该使用内部样式表.你可以使用`<style>`标签在文档头部定义内部样式表:  
```css
<head>
<style type="text/css">
hr{color:sienna;}
p{margin-left:20px;}
body{background-image:url("image/back40.gif");}
</style>
<head>  
```

**内联样式**    
由于要将表现和内容混杂在一起,内联样式会损失掉样式表的许多优势.要使用内联样式,你需要在相关的标签内使用(style)属性.
```css
<p style="color: sienna; margin-left: 20px">
This is a paragraph
</p>
```

**多重样式**  
如果某些属性在不同的样式表中被同样的选择器定义,那么属性值将从更具体的样式表中被继承过来.  
例如,外部样式表有对h3选择器的三个属性:  
```css
h3 {
  color: red;
  text-align: left;
  font-size: 8pt;
  }
```
而内部样式表拥有针对 h3 选择器的两个属性：  
```css
h3 {
  text-align: right; 
  font-size: 20pt;
  }
```
假如拥有内部样式表的这个页面同时与外部样式表链接，那么 h3 得到的样式是：
```css
{color :red; 
text-align: right; 
font-size: 20pt;}
```
即颜色属性将被继承于外部样式表，而文字排列（text-alignment）和字体尺寸（font-size）会被内部样式表中的规则取代。
