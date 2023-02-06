# Bokeh简介
Bokeh是用于现代浏览器端展示的交互式可视化库。它擅长于：

* 现代浏览器端进行交互式可视化
* 独立的HTML文档，或者服务器后端应用
* 富于表现力的多种图形
* 大型，动态或流数据
* 适配 python (或者 Scala，R，...)

最重要的是：

Bokeh的目标是以D3.js风格提供优雅简洁的图形展示，并与大数据或流数据进行高效交互。Bokeh可以帮助快速轻松地创建交互图，仪表板，和数据应用。

# 基本绘图
这个章节主要讲解 bokeh.plotting 接口。这个接口是一个 "mid-level" 接口，主要思想如下描述：  
**从简单的默认图表开始（使用合理的默认工具、网格和轴），添加标记和其他视觉属性直接与数据绑定的图形。**

## 导入和设置
使用bokeh.plotting接口的时候,需要导入一些通用模块:
* 使用figure函数创建plot对象.
* 调用函数ouptput_file,output_notebook,and outbook_server(可以组合使用)告诉Bokeh如何显示或者保存输出.
* 执行show和save显示或者保存图表.


```python
from bokeh.io import output_notebook, show
from bokeh.plotting import figure
```

现在，我们在Jupyter notebook里调用 output_notebook()。我们只需要调用一次，然后所有后续对 show() 的调用都会在Notebook里内嵌显示。


```python
output_notebook()
```

## 基本的散点图
这一节,会明白如何使用Bokeh的各种标记创建简单的散点图.


```python
#create a new plot with default tools, using figure
p = figure(plot_width=400, plot_height=400)
#add a circle renderer with a size, color, and alpha
p.circle([1,2,3,4,5],[6,7,2,4,5], size=15, line_color="navy", fill_color="orange", fill_alpha=10)
#所有的Bokeh标记都具有 size 属性。Circles 还具有 radius 属性。
#fill_alpha是颜色深度
show(p)
```

为了将散点图变成正方形标记而不是圆


```python
# create a new plot using figure
p = figure(plot_width=400, plot_height=400)

# add a square renderer with a size, color, alpha, and sizes
p.hex([1, 2, 3, 4, 5], [6, 7, 2, 4, 5], size=[10, 15, 20, 25, 30], color="firebrick", alpha=0.6)

show(p) # show the results
```

注意，在上面的例子中，我们还为每个标记指定不同的大小。 **一般来说，所有标记符号的属性都可以通过这种形式 "向量化"** 。 还要注意的是，我们通过 color 参数同时设置了线条和填充颜色。这是 bokeh.plotting 的一个特殊便利用法。
Bokeh有很多标记类型: 

|||||
|----|----|----|----|
|.asterisk()|星形|.inverted_triangle()|倒三角形|
|.circle()|圆形|.sqaure()|正方形|
|.circle_cross()|圆形+十字|.square_cross()|正方形+十字|
|.circle_x()|圆形+叉|.sqaure_x|正方形+叉|
|.cross()|十字|.triangle()|三角形|
|.diamond()|菱形|.x()|叉|
|.diamond_cross()|菱形+十字|.hex|六边形|
|.dot()|点图|...|...|

## 线形图
### 基本线形图


```python
# create a new plot(with a title) using figure
p = figure(plot_width=400, plot_height=400, title="My First Line Plot")

#add a line renderer
p.line([1,2,3,4,5],[6,7,2,4,5], line_width=2)

show(p)
```

### 阶跃图


```python
p = figure(plot_width=400, plot_height=400)

# add a steps renderer
p.step([1, 2, 3, 4, 5], [6, 7, 2, 4, 5], line_width=2, mode="center")

show(p)
```

### 多线图
有时一次画多条线是有用的。这可以通过multi_line()方法完成:


```python
p = figure(plot_width=400, plot_height=400)

p.multi_line([[1, 3, 2], [3, 4, 6, 6]], [[2, 1, 4], [4, 7, 8, 5]],
             color=["firebrick", "navy"], alpha=[0.8, 0.3], line_width=4)

show(p)
```

### 缺失点线图
NaN值可以传递给line()和multi_line()符号。在这种情况下，您以单个逻辑行对象结束，当呈现时，它有多个不相交的组件:


```python
p = figure(plot_width=400, plot_height=400)
# add a line renderer with a NaN
nan = float('nan')
p.line([1, 2, 3, nan, 4, 5], [6, 7, 2, 4, 4, 5], line_width=2)

show(p)
```

### 堆叠线图
在某些情况下，希望将在公共索引上对齐的行堆叠起来(例如百分比的时间序列)。vline_stack()和hline_stack()便利方法可用于完成此任务。


```python
from bokeh.models import ColumnDataSource
source = ColumnDataSource(data=dict(
    x=[1, 2, 3, 4, 5],
    y1=[1, 2, 4, 3, 4],
    y2=[1, 4, 2, 2, 3],
))
p = figure(plot_width=400, plot_height=400)

p.vline_stack(['y1', 'y2'], x='x', source=source)

show(p)
```

## 直方图和矩形图
### 直方图
要通过指定(中心)x坐标、宽度以及顶部和底部端点来绘制直方图，使用vbar()函数


```python
p = figure(plot_width=400, plot_height=400)
p.vbar(x=[1, 2, 3], width=0.5, bottom=0,
       top=[1.2, 2.5, 3.7], color="firebrick")

show(p)
```

通过指定(中心)y坐标、高度和左右端点来绘制水平条，使用hbar()函数


```python
p = figure(plot_width=400, plot_height=400)
p.hbar(y=[1, 2, 3], height=0.5, left=0,
       right=[1.2, 2.5, 3.7], color="navy")
show(p)
```

### 堆叠直方图


```python
source = ColumnDataSource(data=dict(
    y=[1, 2, 3, 4, 5],
    x1=[1, 2, 4, 3, 4],
    x2=[1, 4, 2, 2, 3],
))
p = figure(plot_width=400, plot_height=400)

p.hbar_stack(['x1', 'x2'], y='y', height=0.8, color=("grey", "lightgrey"), source=source)

show(p)
```

### 矩形
要通过指定左、右、上、下位置绘制以轴为单位的矩形("四边")，使用quad()函数


```python
p = figure(plot_width=400, plot_height=400)
p.quad(top=[2, 3, 4], bottom=[1, 2, 3], left=[1, 2, 3],
       right=[1.2, 2.5, 3.7], color="#B3DE69")

show(p)
```

若要通过指定中心点、宽度、高度和角度绘制任意矩形，使用rect()函数


```python
from math import pi
p = figure(plot_width=400, plot_height=400)
p.rect(x=[1, 2, 3], y=[1, 2, 3], width=0.2, height=40, color="#CAB2D6",
       angle=pi/3, height_units="screen")
show(p)
```

### 六边形阵列


```python
import numpy as np

from bokeh.util.hex import axial_to_cartesian


q = np.array([0,  0, 0, -1, -1,  1, 1])
r = np.array([0, -1, 1,  0,  1, -1, 0])

p = figure(plot_width=400, plot_height=400, toolbar_location=None)
p.grid.visible = False

p.hex_tile(q, r, size=1, fill_color=["firebrick"]*3 + ["navy"]*4,
           line_color="white", alpha=0.5)

x, y = axial_to_cartesian(q, r, 1, "pointytop")

p.text(x, y, text=["(%d, %d)" % (q,r) for (q, r) in zip(q, r)],#ip() 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的列表
       text_baseline="middle", text_align="center")

show(p)
```

下面一个更现实的例子使用hexbin()函数计算每个bin的计数，并绘制彩色映射计数:


```python
from bokeh.transform import linear_cmap
from bokeh.util.hex import hexbin

n = 50000
x = np.random.standard_normal(n)
y = np.random.standard_normal(n)

bins = hexbin(x, y, 0.1)#0.1是尺寸

p = figure(tools="wheel_zoom,reset", match_aspect=True, background_fill_color='#440154')
p.grid.visible = False

p.hex_tile(q="q", r="r", size=0.1, line_color=None, source=bins,
           fill_color=linear_cmap('counts', 'Viridis256', 0, max(bins.counts)))# 域 调色板 最小值 最大值

show(p)
```

## 有向面积图


有向区域是两个共享公共索引的序列之间的填充区域。例如，一个垂直方向区域有一个x坐标数组和两个y坐标数组，分别是y1和y2，它们将被填充在两者之间。

### 单块有向面积


```python
p = figure(plot_width=400, plot_height=400)

p.varea(x=[1, 2, 3, 4, 5],
        y1=[2, 6, 4, 3, 5],
        y2=[1, 4, 2, 2, 3])

show(p)
```

### 堆叠图


```python
source = ColumnDataSource(data=dict(
    x=[1, 2, 3, 4, 5],
    y1=[1, 2, 4, 3, 4],
    y2=[1, 4, 2, 2, 3],
))
p = figure(plot_width=400, plot_height=400)

p.varea_stack(['y1', 'y2'], x='x', color=("grey", "lightgrey"), source=source)

show(p)
```

### 图块和多边形


```python
p = figure(plot_width=400, plot_height=400)

# add a patch renderer with an alpha and line width
p.patch([1, 2, 3, 4, 5], [6, 7, 8, 7, 3], alpha=0.5, line_width=2)

show(p)
```

### 多个图块


```python
p = figure(plot_width=400, plot_height=400)

p.patches([[1, 3, 2], [3, 4, 6, 6]], [[2, 1, 4], [4, 7, 8, 5]],
          color=["firebrick", "navy"], alpha=[0.8, 0.3], line_width=2)

show(p)
```

### 缺失点
与line()和multi_line()一样，可以将NaN值传递给patch()和patch()。在这种情况下，你会得到一个逻辑图块对象，当渲染时，它会有多个分离的组件:


```python
p = figure(plot_width=400, plot_height=400)

# add a patch renderer with a NaN value
nan = float('nan')
p.patch([1, 2, 3, nan, 4, 5, 6], [6, 7, 5, nan, 7, 3, 6], alpha=0.5, line_width=2)

show(p)
```

基本多边形


```python
p = figure(plot_width=400, plot_height=400)
p.multi_polygons(xs=[[[[1, 1, 2, 2]]]],
                 ys=[[[[3, 4, 4, 3]]]])

show(p)
```

### 有洞多边形
下面的例子展示了如何从三个x和y点序列中生成一个带孔的多边形。第一个序列表示多边形的外部，下面的序列表示孔:


```python
p = figure(plot_width=400, plot_height=400)
p.multi_polygons(xs=[[[ [1, 2, 2, 1], [1.2, 1.6, 1.6], [1.8, 1.8, 1.6] ]]],
                 ys=[[[ [3, 3, 4, 4], [3.2, 3.6, 3.2], [3.4, 3.8, 3.8] ]]])

show(p)
```

### 有独立部分的多边形
有时一个多边形是由多个多边形几何组成的。下面的示例演示了如何从多个x和y点序列生成多边形符号。序列中的每一项都代表了多边形的一部分:


```python
p = figure(plot_width=400, plot_height=400)
p.multi_polygons(xs=[[[ [1, 1, 2, 2], [1.2, 1.6, 1.6], [1.8, 1.8, 1.6] ], [ [3, 4, 3] ]]],
                 ys=[[[ [4, 3, 3, 4], [3.2, 3.2, 3.6], [3.4, 3.8, 3.8] ], [ [1, 1, 3] ]]])

show(p)
```

## 椭圆
ellipse()字形方法接受与rect()相同的属性，但呈现椭圆形状:


```python
p = figure(plot_width=400, plot_height=400)
p.ellipse(x=[1, 2, 3], y=[1, 2, 3], width=[0.2, 0.3, 0.1], height=0.3,
          angle=pi/3, color="#CAB2D6")

show(p)
```

## 图像
您可以使用image()、image_rgba()和image_url()符号方法在Bokeh图形上显示图像。可以使用悬停工具对图像符号进行交互式检查，以查看任意像素的值。有关如何启用图像悬停的更多信息，请参阅用户指南中的图像悬停部分。   
### 原始RGBA数据
下面的例子展示如何使用image_rgba方法显示原始RGBA数据


```python
from __future__ import division #导入后"/"执行的是精确除法而不是截断除法
import numpy as np
#set up some data
N = 20
img = np.empty((N,N), dtype=np.uint32)
view = img.view(dtype=np.uint8).reshape((N,N,4))
for i in range(N):
    for j in range(N):
        view[i,j,0] = int(i/N*255)  #red
        view[i,j,1] = 158           #green
        view[i,j,2] = int(j/N*255)  #blue
        view[i,j,3] = 255           #alpha
# create a new plot (with a fixed range) using figure
p = figure(x_range=[0,10], y_range=[0,10])

# add an RGBA image renderer
p.image_rgba(image=[img], x=[0], y=[0], dw=[10], dh=[10])#x, y 偏移 dw\dh是宽和高

show(p)
```

### Colormapped图片
还可以提供标量值数组，并通过使用image()符号方法让Bokeh在浏览器中自动对数据着色。


```python
x = np.linspace(0, 10, 250)
y = np.linspace(0, 10, 250)
xx, yy = np.meshgrid(x, y)
d = np.sin(xx)*np.cos(yy)

p = figure(plot_width=400, plot_height=400)
p.x_range.range_padding = p.y_range.range_padding = 0 

p.image(image=[d], x=0, y=0, dw=10, dh=10, palette="Spectral11", level="image")
p.grid.grid_line_width = 0.5

show(p)
```

## 段和射线
### 段


```python
p = figure(plot_width=400, plot_height=400)
p.segment(x0=[1, 2, 3], y0=[1, 2, 3], x1=[1.2, 2.4, 3.1],
          y1=[1.2, 2.5, 3.7], color="#F4A582", line_width=3)

show(p)
```

### 射线


```python
p = figure(plot_width=400, plot_height=400)
p.ray(x=[1, 2, 3], y=[1, 2, 3], length=45, angle=[30, 45, 60],
      angle_units="deg", color="#FB8072", line_width=2)

show(p)
```

## 楔形和弧
### 弧
在绘制一个简单的直线圆弧时，Bokeh提供了arc()符号方法，它接受半径、start_angle和end_angle来确定位置。另外，direction属性决定在开始和结束的角度之间是顺时针渲染(“clock”)还是逆时针渲染(“anticlock”)。


```python
p = figure(plot_width=400, plot_height=400)
p.arc(x=[1, 2, 3], y=[1, 2, 3], radius=0.1, start_angle=0.4, end_angle=4.8, color="navy")

show(p)
```

### 楔形
wedge()方法与arc()的属性相同，但是呈现一个填充楔形:


```python
p = figure(plot_width=400, plot_height=400)
p.wedge(x=[1, 2, 3], y=[1, 2, 3], radius=0.2, start_angle=0.4, end_angle=4.8,
        color="firebrick", alpha=0.6, direction="clock")

show(p)
```

annular_wedge()符号方法类似于arc()，但绘制一个已填充区域。它接受一个inner_radius和outer_radius，而不只是半径:


```python
p = figure(plot_width=400, plot_height=400)
p.annular_wedge(x=[1, 2, 3], y=[1, 2, 3], inner_radius=0.1, outer_radius=0.25,
                start_angle=0.4, end_angle=4.8, color="green", alpha=0.6)

show(p)
```

最后，可以使用接受inner_radius和outer_radius的annulus()方法绘制填充环:


```python
p = figure(plot_width=400, plot_height=400)
p.annulus(x=[1, 2, 3], y=[1, 2, 3], inner_radius=0.1, outer_radius=0.25,
          color="orange", alpha=0.6)

show(p)
```

## 网状图


```python
import networkx as nx

G = nx.desargues_graph()
from bokeh.models.graphs import from_networkx
from bokeh.models import Range1d, Plot

# We could use figure here but don't want all the axes and titles
plot = Plot(x_range=Range1d(-1.1,1.1), y_range=Range1d(-1.1,1.1))

# Create a Bokeh graph from the NetworkX input using nx.spring_layout
graph = from_networkx(G, nx.spring_layout, scale=2, center=(0,0))
plot.renderers.append(graph)

# Set some of the default node glyph (Circle) properties
graph.node_renderer.glyph.update(size=20, fill_color="orange")

show(plot)
```

    BokehDeprecationWarning: Importing from_networkx from bokeh.models.graphs is deprecated and will be removed in Bokeh 3.0. Import from bokeh.plotting instead


```python
from bokeh.models.graphs import NodesAndLinkedEdges
from bokeh.models import Circle, HoverTool, MultiLine

G = nx.karate_club_graph()

# We could use figure here but don't want all the axes and titles
plot = Plot(x_range=Range1d(-1.1,1.1), y_range=Range1d(-1.1,1.1))

# Create a Bokeh graph from the NetworkX input using nx.spring_layout
graph = from_networkx(G, nx.spring_layout, scale=2, center=(0,0))
plot.renderers.append(graph)

# Blue circles for nodes, and light grey lines for edges
graph.node_renderer.glyph = Circle(size=25, fill_color='#2b83ba')
graph.edge_renderer.glyph = MultiLine(line_color="#cccccc", line_alpha=0.8, line_width=2)

# green hover for both nodes and edges
graph.node_renderer.hover_glyph = Circle(size=25, fill_color='#abdda4')
graph.edge_renderer.hover_glyph = MultiLine(line_color='#abdda4', line_width=4)

# When we hover over nodes, highlight adjecent edges too
graph.inspection_policy = NodesAndLinkedEdges()

plot.add_tools(HoverTool(tooltips=None))

show(plot)
```

    BokehDeprecationWarning: Importing from_networkx from bokeh.models.graphs is deprecated and will be removed in Bokeh 3.0. Import from bokeh.plotting instead

## 地理图
将数据集与真实世界上下文联系起来非常有用。你可以像任何其他类型的数据一样绘制地理数据，例如 [Texas Unemployment example](https://bokeh.pydata.org/en/latest/docs/gallery/texas.html) 的例子。但Bokeh提供了一些在地理坐标绘图的专门机制：
1. [GMapPlot](https://bokeh.pydata.org/en/latest/docs/user_guide/geo.html#google-maps-support): Bokeh 基于 Google Maps 绘图

2. [TileSource](https://bokeh.pydata.org/en/latest/docs/user_guide/geo.html#tile-providers), 特别是wmtstilesource：允许数据被覆盖于任何地图平铺显示服务器和自定义服务器的数据上，包括： `Google Maps, Stamen, MapQuest, OpenStreetMap, ESRI`,

3. [GeoJSONDataSource](https://bokeh.pydata.org/en/dev/docs/user_guide/geo.html#geojson-datasource): 可使 GeoJSON 类型数据用于Bokeh绘图，就像 ColumnDataSource 类型。

### WMTS Tile Source
WTMS是最常见的平铺地图数据Web标准，即地图由标准大小的图块拼接而成，如此则可以在一个给定的缩放级别构造整个地图。WTMS使用Web Mercator格式，从英国的格林尼治向北向西测量距离，这样容易计算但不会扭曲全球形状。

首先，我们创建一个覆盖美国的空白Bokeh图，范围按米计算：


```python
from bokeh.plotting import figure
from bokeh.models.tiles import WMTSTileSource

# web mercator coordinates
USA = x_range,y_range = ((-13884029,-7453304), (2698291,6455972))

p = figure(tools='pan, wheel_zoom', x_range=x_range, y_range=y_range)
p.axis.visible = False
```

bokeh.models.tiles 中提供了几个WTMS地图源。在这里展示通过使用格式字符串接口，Bokeh如何根据所需的缩放，X和Y值从供应商处请求图片：


```python
url = 'http://a.basemaps.cartocdn.com/dark_all/{Z}/{X}/{Y}.png'
attribution = "Tiles by Carto, under CC BY 3.0. Data by OSM, under ODbL"

p.add_tile(WMTSTileSource(url=url, attribution=attribution))
show(p)
```

这就是在图表中放入地图数据要做的全部！当然，您通常还希望显示其他数据，除非您只想使用服务器提供的地图。现在，只要您能得到Web Mercator格式的坐标，您可以把任何您平常使用Bokeh图表想表达的东西添加到地图上。例如:


```python
import pandas as pd
import numpy as np

def wgs84_to_web_mercator(df, lon="lon", lat="lat"):
    """Converts decimal longitude/latitude to Web Mercator format"""
    k = 6378137
    df["x"] = df[lon] * (k * np.pi/180.0)
    df["y"] = np.log(np.tan((90 + df[lat]) * np.pi/360.0)) * k
    return df

df = pd.DataFrame(dict(name=["Austin", "NYC"], lon=[-97.7431,-74.0059], lat=[30.2672,40.7128]))
wgs84_to_web_mercator(df)
p.circle(x=df['x'], y=df['y'], fill_color='orange', size=10)
show(p)
```

## 结合多个图形
将多个函数组合在单一绘图上，就是在单个图形上调用多个函数方法:


```python
x = [1, 2, 3, 4, 5]
y = [6, 7, 8, 7, 3]


p = figure(plot_width=400, plot_height=400)

# add both a line and circles on the same plot
p.line(x, y, line_width=2)
p.circle(x, y, fill_color="white", size=8)

show(p)
```

## 设置范围
默认情况下，散景会尝试自动设置图形的数据边界，以紧贴数据。有时，您可能需要显式地设置图形的范围。这可以通过使用Range1d对象来设置x_range或y_range属性来完成，这个对象给出了你想要的范围的起始点和结束点:


```python
from bokeh.models import Range1d
# create a new plot with a range set with a tuple
p = figure(plot_width=400, plot_height=400, x_range=(0, 20))

# set a range using a Range1d
p.y_range = Range1d(0, 15)

p.circle([1, 2, 3, 4, 5], [2, 5, 8, 2, 7], size=10)

show(p)
```

范围还有一个bounds属性，它允许您指定您不希望用户能够平移/缩放的范围。


```python
# set a range using a Range1d
p.y_range = Range1d(0, 15, bounds=(0, None))
```

## 指定轴的类型
上面的示例都使用默认的线性轴。这个轴适用于许多需要在线性尺度上显示数值数据的图。在其他情况下，您可能拥有分类数据，或者需要在日期时间或日志尺度上显示数值数据。本节演示在使用 bokeh.plotting 如何指定轴类型。
### 分类轴
通过为绘图范围之一指定一个范围(或要转换为一个元素的列表)来创建类别轴。下面是一个简单的示例，详细信息请参见处理分类数据。


```python
factors = ["a", "b", "c", "d", "e", "f", "g", "h"]
x = [50, 40, 65, 10, 25, 37, 80, 60]

p = figure(y_range=factors)

p.circle(x, factors, size=15, fill_color="orange", line_color="green", line_width=3)

show(p)
```

### 时间轴

在处理timeseries数据或任何涉及日期或时间的数据时，最好有一个轴来显示适合不同日期和时间尺度的标签。
我们已经了解了如何使用figure()函数来创建使用 bokeh.plotting的图形。这个函数接受x_axis_type和y_axis_type作为参数。若要指定日期时间轴，请将这两个参数的值传递给“datetime”。


```python
import pandas as pd
import bokeh.sampledata
from bokeh.sampledata.stocks import AAPL

df = pd.DataFrame(AAPL)
df['date'] = pd.to_datetime(df['date'])


 #create a new plot with a datetime axis type
p = figure(plot_width=800, plot_height=250, x_axis_type="datetime")

p.line(df['date'], df['close'], color='navy', alpha=0.5)

show(p)
```

### 对数尺度轴
当处理指数增长或多个数量级的数据时，通常需要在对数尺度上有一个轴。另一个场景涉及到绘制具有幂律关系的数据，当需要在两个轴上使用对数尺度时。

正如我们在上面看到的，figure()函数接受x_axis_type和y_axis_type作为参数。要指定一个日志轴，将这些参数的值传递给“log”。

默认情况下，计算对数轴范围以适应正值数据。若要设置自己的范围，请参阅Setting Ranges一节。


```python
x = [0.1, 0.5, 1.0, 1.5, 2.0, 2.5, 3.0]
y = [10**xx for xx in x]

# create a new plot with a log axis type
p = figure(plot_width=400, plot_height=400, y_axis_type="log")

p.line(x, y, line_width=2)
p.circle(x, y, fill_color="white", size=8)

show(p)
```

### 共轭轴
可以向单个绘图区添加表示不同范围的多个轴。为此，在extra_x_range和extra_y_range属性中配置“extra”命名范围。然后，可以在添加新的字形方法时引用这些已命名的范围，也可以使用Plot上的add_layout方法添加新的轴对象。下面给出一个例子:


```python
from numpy import arange, linspace, pi, sin
from bokeh.models import LinearAxis, Range1d

x = arange(-2*pi, 2*pi, 0.1)
y = sin(x)
y2 = linspace(0, 100, len(y))
p = figure(x_range=(-6.5, 6.5), y_range=(-1.1, 1.1))

p.circle(x, y, color="red")

p.extra_y_ranges = {"foo": Range1d(start=0, end=100)}
p.circle(x, y2, color="blue", y_range_name="foo")
p.add_layout(LinearAxis(y_range_name="foo"), 'left')

show(p)
```

# 样式和主题
## 颜色和属性
### 颜色 
有许多地方可能需要指定颜色。Bokeh可以各种不同的方式指定颜色：
* 任意一个147种CSS颜色名称,如'green','indigo'
* 十六进制的RGB(A)值,如,'#FF0000','#444444444'
* 整数3元组(r,g,b),其中整数位于0和255之间
* 4元组(r,g,b,a),其中r,g,b是0与255之间的整数,a是0与1之间的浮点数
### 属性
无论用哪个API (models, plotting, charts) 创建Bokeh图表，都可以通过设置Bokeh模型对象的属性来定型其视觉样式。视觉样式属性分为三种类型：line, fill, text。相关的代码和示例完整信息，请参见用户指南的 [视觉样式属性](http://bokeh.pydata.org/en/latest/docs/user_guide/styling.html) 章节。
#### Line属性
设置线条的视觉外观。最常见的是 line_color, line_alpha, line_width， line_dash。
#### Fill属性
设置填充区域的外观：fill_color， fill_alpha。
#### Text属性
设置文本行的视觉外观。最常见的是 text_font, text_font_size, text_color, text_alpha 

有时属性名会有一个前缀，为了区分同一对象的不同line属性，或为了名称更能表达意思。例如，为了设置轮廓线的宽度，可以用 `myplot.outline_line_width = 2`。

## Plots（框图）
可以设置plot的多个顶层属性（outline, border等）[框图](http://bokeh.pydata.org/en/latest/docs/user_guide/styling.html#plots)


```python
# create a new plot with a title
p = figure(plot_width=400, plot_height=400)
p.outline_line_width = 7
p.outline_line_alpha = 0.3
p.outline_line_color = "navy"

p.circle([1,2,3,4,5], [2,5,8,2,7], size=10)

show(p)
```

## Glyphs（标记符号）
我们可以对glyph对象的视觉属性进行设计。当使用 bokeh.plotting 时，通常在调用glyph方法时指定，如：

`p.circle(line_color="red", fill_alpha=0.2, ...)`

也可以直接设置glyph对象的属性。Glyph对象是 GlyphRenderer 找到的对象，由 Plot.add_glyph 和 circle, rect 这样的 bokeh.plotting glyph方法返回。我们看一个例子：


```python
p = figure(plot_width=400, plot_height=400)

# keep a reference to the returned GlyphRenderer
r = p.circle([1,2,3,4,5], [2,5,8,2,7])

r.glyph.size = 50
r.glyph.fill_alpha = 0.2
r.glyph.line_color = "firebrick"
r.glyph.line_dash = [5, 1]
r.glyph.line_width = 2

show(p)
```

### 选择和非选择 视觉效果
你可以控制被选择的glyphs看起来是怎么样的。“被选择”的点根据GlyphRenderer的.selection_glyph属性选项显示：

```r.selection_glyph = Circle(fill_alpha=1, fill_color="firebrick", line_color=None)```

如果设置了“非选择”，则未被选择的点按照GlyphRenderer的.nonselection_glyph属性选项显示：

```r.nonselection_glyph = Circle(fill_alpha=0.2, fill_color="grey", line_color=None)```

使用bokeh.plotting接口时，将这些视觉属性传递给glyph方法比较容易，如下所示。glyph方法将创建的选择或非选择glyph并渲染。


```python
p = figure(plot_width=400, plot_height=400, tools="tap", title="Select a circle")
renderer = p.circle([1, 2, 3, 4, 5], [2, 5, 8, 2, 7], size=50,

                    # set visual properties for selected glyphs
                    selection_color="firebrick",

                    # set visual properties for non-selected glyphs
                    nonselection_fill_alpha=0.2,
                    nonselection_fill_color="grey",
                    nonselection_line_color="firebrick",
                    nonselection_line_alpha=1.0)

show(p)
```

可以指定被“inspected”的glyph的视觉外观，例如，由一个悬停工具。这通过设置GlyphRenderer的一个可选属性hover_glyph完成：

```r.hover_glyph = Circle(fill_alpha=1, fill_color="firebrick", line_color=None)```

如果使用bokeh.plotting，则可以通过给glyph方法传递hover_fill_alpha参数。让我们看一个采用"hline"模式配置HoverTool的例子。


```python
from bokeh.models.tools import HoverTool
from bokeh.sampledata.glucose import data

subset = data.loc['2010-10-06']

x, y = subset.index.to_series(), subset['glucose']

# Basic plot setup
p = figure(width=600, height=300, x_axis_type="datetime", title='Hover over points')

p.line(x, y, line_dash="4 4", line_width=1, color='gray')

cr = p.circle(x, y, size=20,
              fill_color="grey", hover_fill_color="firebrick",
              fill_alpha=0.05, hover_alpha=0.3,
              line_color=None, hover_line_color="white")

p.add_tools(HoverTool(tooltips=None, renderers=[cr], mode='hline'))

show(p)
```

## Axes（轴）

要指定Axis的视觉样式，你首先必须找到Axis对象。最简单的方法是使用Plot的一些便利方法：axis, xaxis, yaxis。这些方法返回轴对象的列表：
```python
>>> p.xaxis
[<bokeh.models.axes.LinearAxis at 0x106fa2390>]
```
但是，您可以设置列表中所有元素的属性，就像它是一个对象一样：
```python
p.xaxis.axis_label = "Temperature"
p.axis.major_label_text_color = "orange"
```
这些被称为“splattable”列表，以及它们的tab补全。


```python
from math import pi

p = figure(plot_width=400, plot_height=400)
p.x([1,2,3,4,5], [2,5,8,2,7], size=10, line_width=2)

p.xaxis.major_label_orientation = pi/4
p.yaxis.major_label_orientation = "vertical"

show(p)
```


```python
p = figure(plot_width=400, plot_height=400)
p.asterisk([1,2,3,4,5], [2,5,8,2,7], size=12, color="olive")

# change just some things about the x-axes
p.xaxis.axis_label = "Temp"
p.xaxis.axis_line_width = 3
p.xaxis.axis_line_color = "red"

# change just some things about the y-axes
p.yaxis.axis_label = "Pressure"
p.yaxis.major_label_text_color = "orange"
p.yaxis.major_label_orientation = "vertical"

# change things on all axes 刻度
p.axis.minor_tick_in = -3
p.axis.minor_tick_out = 5

show(p)
```

## Grids(网格)
[Grids](http://bokeh.pydata.org/en/latest/docs/user_guide/styling.html#grids)


```python
p = figure(plot_width=400, plot_height=400)
p.circle([1,2,3,4,5], [2,5,8,2,7], size=10)

# change just some things about the x-grid
p.xgrid.grid_line_color = None

# change just some things about the y-grid
p.ygrid.grid_line_alpha = 0.5
p.ygrid.grid_line_dash = [6, 4]

show(p)
```


```python
p = figure(plot_width=400, plot_height=400)
p.circle([1,2,3,4,5], [2,5,8,2,7], size=10)

# change just some things about the x-grid
p.xgrid.grid_line_color = None

# change just some things about the y-grid
p.ygrid.band_fill_alpha = 0.1
p.ygrid.band_fill_color = "navy"

show(p)
```

# 添加注释
有时，我们想给图表添加一些视觉提示（边界线，阴影区域，标签和箭头等），以增强某些效果。Bokeh有几个类似的注释类型。通常，要添加注释，我们直接创建“low level”注释对象，并使用add_layout添加到我们的Plot、Figure 或 Chart中。让我们看看一些具体的例子。
## Spans
Spans是“无限”的垂直线或水平线。在创建它们时，指定要跨越的dimension（即width或height）、线的外观视觉属性以及绘制该直线所需的位置。让我们来看一个示例，将两个水平span添加到一个简单的图中：


```python
import numpy as np
from bokeh.models.annotations import Span

x = np.linspace(0, 20, 200)
y = np.sin(x)

p = figure(y_range=(-2, 2))
p.line(x, y)

upper = Span(location=1, dimension='width', line_color='olive', line_width=4)
p.add_layout(upper)

lower = Span(location=-1, dimension='width', line_color='firebrick', line_width=4)
p.add_layout(lower)

show(p)
```

## Box Annotations（Box注释）


有时您可能需要通过绘制一个阴影框来突出图中的某些区域。BoxAnnotation可以做到，其通过如下属性以及任一line和fill视觉属性来控制外观：
* top
* left
* bottom
* right

如果某个维度坐标空着不指定，就可以得到“无限”Box。例如，不指定top，Box将一直延伸到绘图区域的顶部，无论发生任何平移或缩放。

让我们来看一个示例，在图中添加一些阴影框：


```python
import numpy as np
from bokeh.models.annotations import BoxAnnotation

x = np.linspace(0, 20, 200)
y = np.sin(x)

p = figure(y_range=(-2, 2))
p.line(x, y)

# region that always fills the top of the plot
upper = BoxAnnotation(bottom=1, fill_alpha=0.1, fill_color='olive')
p.add_layout(upper)

# region that always fills the bottom of the plot
lower = BoxAnnotation(top=-1, fill_alpha=0.1, fill_color='firebrick')
p.add_layout(lower)

# a finite region
center = BoxAnnotation(top=0.6, bottom=-0.3, left=7, right=12, fill_alpha=0.1, fill_color='navy')
p.add_layout(center)

show(p)
```

## Label（标签）
Label注释允许您轻松地绘图中添加单个文本标签。要显示的位置和文本被通过x, y和text设置：
```python
Label(x=10, y=5, text="Some Label")
```
默认单位是“data space”，但x_units和y_units可以设置为"screen"来用相对位置定位标签。标签还可以用x_offset和y_offset来设置相对于x和y的偏移距离以确定最终位置。

Label对象具备标准的text，line（border_line）和fill（background_fill）属性。line和fill属性应用于文本周围的边界框：
```python
Label(x=10, y=5, text="Some Label", text_font_size="12pt", 
      border_line_color="red", background_fill_color="blue")
```


```python
from bokeh.models.annotations import Label
from bokeh.plotting import figure

p = figure(x_range=(0,10), y_range=(0,10))
p.circle([2, 5, 8], [4, 7, 6], color="olive", size=10)

label = Label(x=5, y=7, x_offset=12, text="Second Point", text_baseline="middle") #offset
p.add_layout(label)

show(p)
```

## LabelSet（标签集）
LabelSet注释允许您一次创建许多标签，例如，给一系列标记符号创建标签注释。其与Label类似，不同之处是接受ColumnDataSource类型数据为 source属性，而且x和y指向数据源的列，例如x="col2"（但也可以是固定值，如x=10）。


```python
from bokeh.models import ColumnDataSource, LabelSet


source = ColumnDataSource(data=dict(
    temp=[166, 171, 172, 168, 174, 162],
    pressure=[165, 189, 220, 141, 260, 174],
    names=['A', 'B', 'C', 'D', 'E', 'F']))

p = figure(x_range=(160, 175))
p.scatter(x='temp', y='pressure', size=8, source=source)
p.xaxis.axis_label = 'Temperature (C)'
p.yaxis.axis_label = 'Pressure (lbs)'

labels = LabelSet(x='temp', y='pressure', text='names', level='glyph',
                  x_offset=5, y_offset=5, source=source, render_mode='canvas')


p.add_layout(labels)

show(p)
```

## Arrows(箭头)
Arrow 注释允许您在图中 “指向” 不同的东西，而且结合 label 标签使用特别有用。

例如，创建一个箭头，从点（0,0）指向点（1,1）：
```python
p.add_layout(Arrow(x_start=0, y_start=0, x_end=1, y_end=0))
```
这个箭头是缺省的指向箭头尾端的 OpenHead 箭头。其他种类的箭头包括 NormalHead 和 VeeHead。箭头头部类型可以通过设置 Arrow 对象的 start 和 end 属性设置：
```python
p.add_layout(Arrow(start=OpenHead(), end=VeeHead(), 
             x_start=0, y_start=0, x_end=1, y_end=0))
```
这将创建一个双端箭头，箭头是“open”类型，箭尾是“Vee”类型。箭头部可通过line和fill的标准属性设置来控制其外观。看一个例子

`OpenHead(line_color="firebrick", line_width=4)`


```python
from bokeh.models.annotations import Arrow
from bokeh.models.arrow_heads import OpenHead, NormalHead, VeeHead

p = figure(plot_width=600, plot_height=600)

p.circle(x=[0, 1, 0.5], y=[0, 0, 0.7], radius=0.1,
         color=["navy", "yellow", "red"], fill_alpha=0.1)

p.add_layout(Arrow(end=OpenHead(line_color="firebrick", line_width=4),
                   x_start=0, y_start=0, x_end=1, y_end=0))

p.add_layout(Arrow(end=NormalHead(fill_color="orange"),
                   x_start=1, y_start=0, x_end=0.5, y_end=0.7))

p.add_layout(Arrow(end=VeeHead(size=35), line_color="red",
                   x_start=0.5, y_start=0.7, x_end=0, y_end=0))

show(p)
```

## Legends(图例)
### Simple Legends(简单图例)


```python
import numpy as np

x = np.linspace(0, 4*np.pi, 100)
y = np.sin(x)

p = figure(height=400)

p.circle(x, y, legend="sin(x)")
p.line(x, 2*y, legend="2*sin(x)", line_dash=[4, 4], line_color="orange", line_width=2)

show(p)
```

    BokehDeprecationWarning: 'legend' keyword is deprecated, use explicit 'legend_label', 'legend_field', or 'legend_group' keywords instead
    BokehDeprecationWarning: 'legend' keyword is deprecated, use explicit 'legend_label', 'legend_field', or 'legend_group' keywords instead

## Color bars（彩色柱状图）


```python
from bokeh.sampledata.autompg import autompg
from bokeh.models import LinearColorMapper, ColorBar
from bokeh.palettes import Viridis256

source = ColumnDataSource(autompg)
color_mapper = LinearColorMapper(palette=Viridis256, low=autompg.weight.min(), high=autompg.weight.max())

p = figure(x_axis_label='Year', y_axis_label='MPG', tools='', toolbar_location='above')
p.circle(x='yr', y='mpg', color={'field': 'weight', 'transform': color_mapper}, size=20, alpha=0.6, source=source)

color_bar = ColorBar(color_mapper=color_mapper, label_standoff=12, location=(0,0), title='Weight')
p.add_layout(color_bar, 'right')

show(p)
```

# 数据源和数据转换
## 概要
我们已经看到Bokeh怎么能够很好地与Python列表、NumPy数组、Pandas序列一起工作。在底层，这些输入数据被转换为Bokeh的ColumnDataSource类型。这个数据类型是Bokeh使用的核心数据源对象。虽然Bokeh经常通过透明的方式创建该类型对象，但有些时候也需要通过显式的方式创建。
## 创建数据源
### 通过 Python Dicts 创建
从 bokeh.models 导入 ColumnDataSource：

`from bokeh.models import ColumnDataSource`

ColumnDataSource是从列名（字符串）到值序列的映射。下面是一个简单的例子。映射是通过传入Python dict建立的，其字符串键作为列名，对应的Python list作为值序列。该值序列也可以是NumPy数组，或Pandas序列。

**注意：ColumnDataSource 中所有的列必须等长。**


```python
source = ColumnDataSource(data={
    'x' : [1, 2, 3, 4, 5],
    'y' : [3, 7, 8, 5, 1],
})
```

到目前，我们已经通过直接传入文本列表或数组数据的方式调用函数，如p.circle。当我们这样做时，Bokeh会自动帮我们创建一个 ColumnDataSource。但我们也可以显式地把 ColumnDataSource 作为 source 参数传入glyph函数。当我们这样做时，如果我们想要给一个属性（如"x" 或 "y" 或 "fill_color"）指定一个值序列，我们传入对应的列名：


```python
p = figure(plot_width=400, plot_height=400)
p.circle('x', 'y', size=20, source=source)
show(p)
```

### 通过 Pandas DataFrames 创建
通过Pandas Dataframe创建 ColumnDataSource 对象也很容易。只需要在创建 ColumnDataSource 的时候把Dataframe传入就行：


```python
from bokeh.sampledata.iris import flowers as df

source = ColumnDataSource(df)
```

现在我们就可以使用这个对象绘制上例中的图了，把对应的列名传入glyph函数：


```python
p = figure(plot_width=400, plot_height=400)
p.circle('petal_length', 'petal_width', source=source)
show(p)
```

## 转换数据
### 标记
还可以将分类数据映射到标记类型。下面的示例展示了如何使用factor_mark()在输入数据中显示不同的标记或不同的类别。它还演示了使用factor_cmap()对相同的类别进行着色:


```python
from bokeh.transform import factor_cmap, factor_mark
from bokeh.sampledata.iris import flowers
SPECIES = ['setosa', 'versicolor', 'virginica']
MARKERS = ['hex', 'circle_x', 'triangle']

p = figure(title = "Iris Morphology")
p.xaxis.axis_label = 'Petal Length'
p.yaxis.axis_label = 'Sepal Width'

p.scatter("petal_length", "sepal_width", source=flowers, legend_field="species", fill_alpha=0.4, size=12,
          marker=factor_mark('species', MARKERS, SPECIES),
          color=factor_cmap('species', 'Category10_3', SPECIES))

show(p)
```

factor_mark()转换主要只对散点符号方法有用，因为只有散点符号可以由标记类型参数化。

### 自定义JS变换
除了上面的内置转换之外，还有一个CustomJSTransform，它允许指定任意的JavaScript代码来执行对ColumnDataSource数据的转换步骤。通常会提供v_func(用于“向量化”函数)(更不常见的情况是，可能还需要一个等价的标量func)。v_func代码应该期望变量xs中的一个输入数组，并返回一个JavaScript数组，其中包含转换后的值


```python
#v_func = """
#    const first = xs[0]
#    const norm = new Float64Array(xs.length)
#    for (let i = 0; i < xs.length; i++) {
#        norm[i] = xs[i] / first
#    }
#    return norm
#"""
#normalize = CustomJSTransform(v_func=v_func)

#plot.line(x='aapl_date', y=transform('aapl_close', normalize), line_width=2,
#          color='#cf3c4d', alpha=0.6,legend="Apple", source=aapl_source)
```

## 过滤数据
通常需要关注已从较大数据集下采样或过滤的数据的一部分。bokeh允许您指定表示数据子集的数据源的视图。通过拥有数据源的视图，底层数据不需要更改，并且可以在图之间共享。视图由一个或多个筛选器组成，这些筛选器选择应绑定到特定符号的数据源行。

要使用数据子集进行绘图，可以创建一个CDSView，并将其作为视图参数传递给图形上的呈现器添加方法，如Figure .circle。CDSView有两个属性，源和过滤器。source是与视图关联的ColumnDataSource。过滤器是过滤器对象的列表，下面列出并描述。
```python
from bokeh.models import ColumnDataSource, CDSView

source = ColumnDataSource(some_data)
view = CDSView(source=source, filters=[filter1, filter2])

p = figure()
p.circle(x="x", y="y", source=source, view=view)
```

### 索引过滤
IndexFilter是最简单的过滤器类型。它有一个索引属性，它是一个整数列表，这些整数是你想要包含在图中的数据的索引。


```python
from bokeh.layouts import gridplot
from bokeh.models import CDSView, ColumnDataSource, IndexFilter

source = ColumnDataSource(data=dict(x=[1, 2, 3, 4, 5], y=[1, 2, 3, 4, 5]))
view = CDSView(source=source, filters=[IndexFilter([0, 2, 4])])

tools = ["box_select", "hover", "reset"]
p = figure(plot_height=300, plot_width=300, tools=tools)
p.circle(x="x", y="y", size=10, hover_color="red", source=source)

p_filtered = figure(plot_height=300, plot_width=300, tools=tools)
p_filtered.circle(x="x", y="y", size=10, hover_color="red", source=source, view=view)

show(gridplot([[p, p_filtered]]))
```

### 布尔过滤器
布尔过滤器通过其布尔值属性中的真值或假值列表从数据源中选择行。


```python
from bokeh.models import BooleanFilter, CDSView, ColumnDataSource


source = ColumnDataSource(data=dict(x=[1, 2, 3, 4, 5], y=[1, 2, 3, 4, 5]))
booleans = [True if y_val > 2 else False for y_val in source.data['y']]
view = CDSView(source=source, filters=[BooleanFilter(booleans)])

tools = ["box_select", "hover", "reset"]
p = figure(plot_height=300, plot_width=300, tools=tools)
p.circle(x="x", y="y", size=10, hover_color="red", source=source)

p_filtered = figure(plot_height=300, plot_width=300, tools=tools,
                    x_range=p.x_range, y_range=p.y_range)
p_filtered.circle(x="x", y="y", size=10, hover_color="red", source=source, view=view)

show(gridplot([[p, p_filtered]]))
```

### 群滤波器
GroupFilter允许您从数据集中选择具有分类变量特定值的行。GroupFilter有两个属性，column_name (ColumnDataSource中的列名称)和group(要选择的列的值)。


```python
from bokeh.layouts import gridplot
from bokeh.models import CDSView, ColumnDataSource, GroupFilter
from bokeh.sampledata.iris import flowers

source = ColumnDataSource(flowers)
view1 = CDSView(source=source, filters=[GroupFilter(column_name='species', group='versicolor')])

plot_size_and_tools = {'plot_height': 300, 'plot_width': 300,
                        'tools':['box_select', 'reset', 'help']}

p1 = figure(title="Full data set", **plot_size_and_tools)
p1.circle(x='petal_length', y='petal_width', source=source, color='black')

p2 = figure(title="Setosa only", x_range=p1.x_range, y_range=p1.y_range, **plot_size_and_tools)
p2.circle(x='petal_length', y='petal_width', source=source, view=view1, color='red')

show(gridplot([[p1, p2]]))
```

# 演示布局

在前面的章节中，我们学习了如何使用不同类型的数据创建单个图表。但我们经常想绘制不止一幅图。Bokeh 图表可以单独嵌入在HTML文件中，但把多个图表合并在一个Bokeh内置的Layout里也是很容易的。我们将在本章学习如何做到这一点。

下面的单元格定义了一些我们将在示例中使用的数据变量。


```python
x = list(range(11))
y0, y1, y2 = x, [10-i for i in x], [abs(i-5) for i in x]
```

## 行与列
bokeh.layouts 模块提供了 row 和 column 功能以便在垂直和水平布局中排列图表。下面的例子把三个图表排成一排。


```python
from bokeh.layouts import row 

# create a new plot
s1 = figure(width=250, plot_height=250)
s1.circle(x, y0, size=10, color="navy", alpha=0.5)

# create another one
s2 = figure(width=250, height=250)
s2.triangle(x, y1, size=10, color="firebrick", alpha=0.5)

# create and another
s3 = figure(width=250, height=250)
s3.square(x, y2, size=10, color="olive", alpha=0.5)

# show the results in a row
show(row(s1, s2, s3))
```

## Grids Layout for Plots(网格布局)
gridplot()函数可用于在网格布局中安排散景图。gridplot()还将所有工具收集到一个工具栏中，当前活动的工具对于网格中的所有绘图都是相同的。通过传递None而不是plot对象，可以在网格中留下“ ”空间。


```python
from bokeh.layouts import gridplot
x = list(range(11))
y0 = x
y1 = [10 - i for i in x]
y2 = [abs(i - 5) for i in x]

# create three plots
s1 = figure(background_fill_color="#fafafa")
s1.circle(x, y0, size=12, alpha=0.8, color="#53777a")

s2 = figure(background_fill_color="#fafafa")
s2.triangle(x, y1, size=12, alpha=0.8, color="#c02942")

s3 = figure(background_fill_color="#fafafa")
s3.square(x, y2, size=12, alpha=0.8, color="#d95b43")

# make a grid
grid = gridplot([[s1, s2], [None, s3]], plot_width=250, plot_height=250)

show(grid)
```

为了方便，您还可以传递一个绘图列表，并指定网格中所需的列数。例如

`gridplot([s1, s2, s3], ncols=2)`

默认情况下，gridplot将把每个子图中的所有工具合并到一个附加到网格的工具栏中。要禁用此行为，可以将选项merge_tools设置为False。

## Sizing Mode
### Modes
可布图散景对象可使用以下大小调整模式单独配置:  
**“fixed”**
组件没有响应。它将保持其原始的宽度和高度，而不管随后的任何浏览器窗口调整事件。

**“stretch_width”**
组件将响应地调整大小以拉伸到可用的宽度，而不保持任何长宽比。组件的高度取决于组件的类型，可以是固定的，也可以适合组件的内容。

**“stretch_height”**
组件将响应地调整大小以拉伸到可用的高度，而不保持任何宽高比。组件的宽度取决于组件的类型，可以是固定的，也可以适合组件的内容。

**“stretch_both”**
组件是完全响应，独立的宽度和高度，并将占据所有可用的水平和垂直空间，即使这改变了高宽比的组件。

**“scale_width”**
组件将响应地调整大小以拉伸到可用宽度，同时保持原始或提供的高宽比。

**“scale_height”**
组件将响应地调整大小以拉伸到可用的高度，同时保持原始或提供的高宽比。

**“scale_both”**
组件将响应调整大小到可用的宽度和高度，同时保持原始或提供的高宽比。

一般来说，根据模式的不同，宽度和高度可能需要同时提供其中之一或两者。(例如，对于stretch_width模式，必须提供所需的固定高度)。

注意，诸如行和列之类的布局对象将把它们配置的大小调整模式传递给它们自己没有显式设置sizing_mode的任何子对象。


```python
x = np.linspace(0, 10, 100)
y = np.sin(x)

source = ColumnDataSource(data=dict(x=x, y=y))

plot = figure(
    y_range=(-10, 10), tools='', toolbar_location=None,
    title="Sliders example")
plot.line('x', 'y', source=source, line_width=3, line_alpha=0.6)

amp_slider = Slider(start=0.1, end=10, value=1, step=.1, title="Amplitude")
freq_slider = Slider(start=0.1, end=10, value=1, step=.1, title="Frequency")
phase_slider = Slider(start=0, end=6.4, value=0, step=.1, title="Phase")
offset_slider = Slider(start=-5, end=5, value=0, step=.1, title="Offset")

callback = CustomJS(args=dict(source=source, amp=amp_slider, freq=freq_slider, phase=phase_slider, offset=offset_slider),
                    code="""
    const data = source.data;
    const A = amp.value;
    const k = freq.value;
    const phi = phase.value;
    const B = offset.value;
    const x = data['x']
    const y = data['y']
    for (var i = 0; i < x.length; i++) {
        y[i] = B + A*Math.sin(k*x[i]+phi);
    }
    source.change.emit();
""")

amp_slider.js_on_change('value', callback)
freq_slider.js_on_change('value', callback)
phase_slider.js_on_change('value', callback)
offset_slider.js_on_change('value', callback)

widgets = column(amp_slider, freq_slider, phase_slider, offset_slider)
show(row(widgets, plot))
```


    ---------------------------------------------------------------------------
    
    NameError                                 Traceback (most recent call last)
    
    <ipython-input-113-9b31cc8e3ab2> in <module>
          9 plot.line('x', 'y', source=source, line_width=3, line_alpha=0.6)
         10 
    ---> 11 amp_slider = Slider(start=0.1, end=10, value=1, step=.1, title="Amplitude")
         12 freq_slider = Slider(start=0.1, end=10, value=1, step=.1, title="Frequency")
         13 phase_slider = Slider(start=0, end=6.4, value=0, step=.1, title="Phase")


    NameError: name 'Slider' is not defined


# 关联和交互
## 链接的行为
链接图以增加图之间的交互性通常很有用。
### 链接平移
通常需要在多个情节中链接平移或缩放动作。要启用此特性，只需在plot()调用之间共享range对象。


```python
from bokeh.layouts import gridplot
x = list(range(11))
y0 = x
y1 = [10-xx for xx in x]
y2 = [abs(xx-5) for xx in x]

# create a new plot
s1 = figure(plot_width=250, plot_height=250, title=None)
s1.circle(x, y0, size=10, color="navy", alpha=0.5)

# create a new plot and share both ranges
s2 = figure(plot_width=250, plot_height=250, x_range=s1.x_range, y_range=s1.y_range, title=None)
s2.triangle(x, y1, size=10, color="firebrick", alpha=0.5)

# create a new plot and share only one range
s3 = figure(plot_width=250, plot_height=250, x_range=s1.x_range, title=None)
s3.square(x, y2, size=10, color="olive", alpha=0.5)

p = gridplot([[s1, s2, s3]], toolbar_location=None)

# show the results
show(p)
```

### 链接选择
在bokeh中链接刷是通过在函数渲染器之间共享数据源来表达的。这就是bokeh所需要理解的，在一个函数上操作的选择必须传递给共享同一源的所有其他函数。要了解链接选择如何扩展到只绘制来自数据源的数据子集的函数呈现器，请参阅[带过滤数据的链接选择](https://docs.bokeh.org/en/latest/docs/user_guide/data.html#userguide-data-linked-selection-with-filtering)


```python
from bokeh.layouts import gridplot
from bokeh.models import ColumnDataSource
from bokeh.plotting import figure

x = list(range(-20, 21))
y0 = [abs(xx) for xx in x]
y1 = [xx**2 for xx in x]

# create a column data source for the plots to share
source = ColumnDataSource(data=dict(x=x, y0=y0, y1=y1))

TOOLS = "box_select,lasso_select,reset,help"

# create a new plot and add a renderer
left = figure(tools=TOOLS, plot_width=300, plot_height=300, title=None)
left.circle('x', 'y0', source=source)

# create another new plot and add a renderer
right = figure(tools=TOOLS, plot_width=300, plot_height=300, title=None)
right.circle('x', 'y1', source=source)

p = gridplot([[left, right]])

show(p)
```

### 链接属性
还可以使用js_link方法将Bokeh模型属性的值链接在一起，以便它们保持同步。下面的例子将一个圆形符号半径链接到一个滑块部件的值:


```python
from bokeh.layouts import column
from bokeh.models import Slider

plot = figure(plot_width=400, plot_height=400)
r = plot.circle([1,2,3,4,5,], [3,2,5,6,4], radius=0.2, alpha=0.5)

slider = Slider(start=0.1, end=2, step=0.01, value=0.2)
slider.js_link('value', r.glyph, 'radius')

show(column(plot, slider))
```

## 图例交互
添加到bokeh的图例可以进行交互，因此单击或轻击图例条目将隐藏或弱化图例中的相应符号。通过将图例上的click_policy属性设置为“隐藏”或“弱化”，可以激活这些模式。

### 隐藏图例
有时，我们希望能够通过单击图例中的条目来隐藏字形。在散景中，这可以通过将图例click_policy属性设置为“hide”来实现，如下例所示:

```python
import pandas as pd
from bokeh.palettes import Spectral4
from bokeh.sampledata.stocks import AAPL, GOOG, IBM, MSFT

p = figure(plot_width=800, plot_height=250, x_axis_type="datetime")
p.title.text = 'Click on legend entries to hide the corresponding lines'

for data, name, color in zip([AAPL, IBM, MSFT, GOOG], ["AAPL", "IBM", "MSFT", "GOOG"], Spectral4):
    df = pd.DataFrame(data)
    df['date'] = pd.to_datetime(df['date'])
    p.line(df['date'], df['close'], line_width=2, color=color, alpha=0.8, legend_label=name)

p.legend.location = "top_left"
p.legend.click_policy="hide"

show(p)
```

其他时候，legend交互最好能弱化一个图形，而不是完全隐藏它。在本例中，将click_policy属性设置为“弱化”。此外，还需要指定“muted glyph”的视觉属性。通常，这与选中和未选中符号或悬停检查的方式完全相同。在下面的例子中，muted_alpha=0.2和muted_color=color被传递给circle，以指定哑线应该用低alpha哑线图形绘制。


```python
import pandas as pd

from bokeh.palettes import Spectral4
from bokeh.sampledata.stocks import AAPL, GOOG, IBM, MSFT

p = figure(plot_width=800, plot_height=250, x_axis_type="datetime")
p.title.text = 'Click on legend entries to mute the corresponding lines'

for data, name, color in zip([AAPL, IBM, MSFT, GOOG], ["AAPL", "IBM", "MSFT", "GOOG"], Spectral4):
    df = pd.DataFrame(data)
    df['date'] = pd.to_datetime(df['date'])
    p.line(df['date'], df['close'], line_width=2, color=color, alpha=0.8,
           muted_color=color, muted_alpha=0.2, legend_label=name)

p.legend.location = "top_left"
p.legend.click_policy="mute"

show(p)
```

## 添加小部件
小部件是交互式控件，可以添加到散景应用程序中，为可视化提供前端用户界面。它们可以驱动新的计算、更新绘图并连接到其他编程功能。当与Bokeh服务器一起使用时，小部件可以运行任意的Python代码，从而支持复杂的应用程序。小部件也可以通过浏览器的Javascript运行时在独立的HTML文档中使用，而无需使用Bokeh服务器。
### 回调函数
要使用小部件，必须将它们添加到文档中并定义它们的回调。小部件可以直接添加到文档根中或嵌套在布局中。有两种使用小部件功能的方法:
* CustomJS回调(参见JavaScript回调)。这种方法将工作在独立的HTML文档或bokeh服务器应用程序。
* 使用bokeh serve启动一个bokeh服务器，并使用.on_change设置事件处理程序(或者对于某些小部件，使用.on_click)。

### 示例
下面的小节将收集使用所有内置小部件的简短但完整的示例。通过查看浏览器的Javascript控制台日志，可以观察到许多示例打印的输出。

#### 按钮


```python
from bokeh.models import Button, CustomJS

button = Button(label="Foo", button_type="success")
button.js_on_click(CustomJS(code="console.log('button: click!', this.toString())"))

show(button)
```

#### 复选框按钮组


```python
from bokeh.models import CheckboxButtonGroup, CustomJS

LABELS = ["Option 1", "Option 2", "Option 3"]

checkbox_button_group = CheckboxButtonGroup(labels=LABELS, active=[0, 1])
checkbox_button_group.js_on_click(CustomJS(code="""
    console.log('checkbox_button_group: active=' + this.active, this.toString())
"""))

show(checkbox_button_group)
```

#### 复选框


```python
from bokeh.models import CheckboxGroup, CustomJS

LABELS = ["Option 1", "Option 2", "Option 3"]

checkbox_group = CheckboxGroup(labels=LABELS, active=[0, 1])
checkbox_group.js_on_click(CustomJS(code="""
    console.log('checkbox_group: active=' + this.active, this.toString())
"""))

show(checkbox_group)
```

#### 颜色选择器


```python
from bokeh.layouts import column
from bokeh.models import ColorPicker
from bokeh.plotting import Figure

plot = Figure(x_range=(0, 1), y_range=(0, 1), plot_width=350, plot_height=350)
line = plot.line(x=(0,1), y=(0,1), color="black", line_width=4)

picker = ColorPicker(title="Line Color")
picker.js_link('color', line.glyph, 'line_color')

show(column(plot, picker))
```

#### 数据表
Bokeh提供了一个基于SlickGrid的复杂数据表小部件。注意，由于表是用数据源对象配置的，因此共享此数据源的任何绘图都将自动在绘图和表之间链接选择(即使在静态HTML文档中也是如此)。


```python
from datetime import date
from random import randint

from bokeh.models import ColumnDataSource, DataTable, DateFormatter, TableColumn

data = dict(
        dates=[date(2014, 3, i+1) for i in range(10)],
        downloads=[randint(0, 100) for i in range(10)],
    )
source = ColumnDataSource(data)

columns = [
        TableColumn(field="dates", title="Date", formatter=DateFormatter()),
        TableColumn(field="downloads", title="Downloads"),
    ]
data_table = DataTable(source=source, columns=columns, width=400, height=280)

show(data_table)
```

#### 时间选择器


```python
from bokeh.models import CustomJS, DatePicker

date_picker = DatePicker(title='Select date', value="2019-09-20", min_date="2019-08-01", max_date="2019-10-30")
date_picker.js_on_change("value", CustomJS(code="""
    console.log('date_picker: value=' + this.value, this.toString())
"""))

show(date_picker)
```

#### 日期范围滑块


```python
from datetime import date

from bokeh.io import show
from bokeh.models import CustomJS, DateRangeSlider

date_range_slider = DateRangeSlider(value=(date(2016, 1, 1), date(2016, 12, 31)),
                                    start=date(2015, 1, 1), end=date(2017, 12, 31))
date_range_slider.js_on_change("value", CustomJS(code="""
    console.log('date_range_slider: value=' + this.value, this.toString())
"""))

show(date_range_slider)
```

#### div
用于标签中显示支持HTML的文本的小部件:


```python
from bokeh.models import Div

div = Div(text="""Your <a href="https://en.wikipedia.org/wiki/HTML">HTML</a>-supported text is initialized with the <b>text</b> argument.  The
remaining div arguments are <b>width</b> and <b>height</b>. For this example, those values
are <i>200</i> and <i>100</i>, respectively.""",
width=200, height=100)

show(div)
```

#### 下拉选项
单击时显示互斥项目的下拉列表的按钮。


```python
from bokeh.models import CustomJS, Dropdown

menu = [("Item 1", "item_1"), ("Item 2", "item_2"), None, ("Item 3", "item_3")]

dropdown = Dropdown(label="Dropdown button", button_type="warning", menu=menu)
dropdown.js_on_event("menu_item_click", CustomJS(code="console.log('dropdown: ' + this.item, this.toString())"))

show(dropdown)
```

#### 选择文件


```python
from bokeh.models import FileInput

file_input = FileInput()

show(file_input)
```

#### 多项选择
一个多选择小部件，在一个紧凑的水平布局中显示多个可用选项:


```python
from bokeh.models import CustomJS, MultiChoice

OPTIONS = ["foo", "bar", "baz", "quux"]

multi_choice = MultiChoice(value=["foo", "baz"], options=OPTIONS)
multi_choice.js_on_change("value", CustomJS(code="""
    console.log('multi_choice: value=' + this.value, this.toString())
"""))

show(multi_choice)
```

#### 多选一


```python
from bokeh.models import CustomJS, MultiSelect

OPTIONS = [("1", "foo"), ("2", "bar"), ("3", "baz"), ("4", "quux")]

multi_select = MultiSelect(value=["1", "2"], options=OPTIONS)
multi_select.js_on_change("value", Custofrom bokeh.models import CustomJS, RadioButtonGroup

LABELS = ["Option 1", "Option 2", "Option 3"]

radio_button_group = RadioButtonGroup(labels=LABELS, active=0)
radio_button_group.js_on_click(CustomJS(code="""
    console.log('radio_button_group: active=' + this.active, this.toString())
"""))

show(radio_button_group)mJS(code="""
    console.log('multi_select: value=' + this.value, this.toString())
"""))

show(multi_select)
```


      File "<ipython-input-130-9b7fbcaac8f1>", line 6
        multi_select.js_on_change("value", Custofrom bokeh.models import CustomJS, RadioButtonGroup
                                                     ^
    SyntaxError: invalid syntax

#### 段落
用于在HTML标签中显示文本块的小部件:


```python
from bokeh.models import Paragraph

p = Paragraph(text="""Your text is initialized with the 'text' argument.  The
remaining Paragraph arguments are 'width' and 'height'. For this example, those values
are 200 and 100, respectively.""",
width=200, height=100)

show(p)
```

####  密码输入框


```python
from bokeh.models import CustomJS, PasswordInput

password_input = PasswordInput(placeholder="enter password...")
password_input.js_on_change("value", CustomJS(code="""
    console.log('password_input: value=' + this.value, this.toString())
"""))

show(password_input)
```

#### Pre文本
用于在HTML 标签中显示一块预格式化文本的小部件:`</pre>`


```python
from bokeh.models import PreText

pre = PreText(text="""Your text is initialized with the 'text' argument.

The remaining Paragraph arguments are 'width' and 'height'. For this example,
those values are 500 and 100, respectively.""",
width=500, height=100)

show(pre)
```

#### 单选按钮组

单选按钮组一次最多可以有一个选择的按钮:


```python
from bokeh.models import CustomJS, RadioButtonGroup

LABELS = ["Option 1", "Option 2", "Option 3"]

radio_button_group = RadioButtonGroup(labels=LABELS, active=0)
radio_button_group.js_on_click(CustomJS(code="""
    console.log('radio_button_group: active=' + this.active, this.toString())
"""))

show(radio_button_group)
```

#### 单选组
一个单选组使用标准单选按钮外观:


```python
from bokeh.models import CustomJS, RadioGroup

LABELS = ["Option 1", "Option 2", "Option 3"]

radio_group = RadioGroup(labels=LABELS, active=0)
radio_group.js_on_click(CustomJS(code="""
    console.log('radio_group: active=' + this.active, this.toString())
"""))

show(radio_group)
```

#### 范围滑块


```python
from bokeh.models import CustomJS, RangeSlider

range_slider = RangeSlider(start=0, end=10, value=(1,9), step=.1, title="Stuff")
range_slider.js_on_change("value", CustomJS(code="""
    console.log('range_slider: value=' + this.value, this.toString())
"""))

show(range_slider)
```

#### 选择


```python
from bokeh.models import CustomJS, Select

select = Select(title="Option:", value="foo", options=["foo", "bar", "baz", "quux"])
select.js_on_change("value", CustomJS(code="""
    console.log('select: value=' + this.value, this.toString())
"""))

show(select)
```

#### 滑块

滑块可以配置“开始”和“结束”值、“步长”、“初始值”和“标题”:


```python
from bokeh.models import CustomJS, Slider

slider = Slider(start=0, end=10, value=1, step=.1, title="Stuff")
slider.js_on_change("value", CustomJS(code="""
    console.log('slider: value=' + this.value, this.toString())
"""))

show(slider)
```

#### 微调控制项


```python
import numpy as np

from bokeh.io import show
from bokeh.layouts import column, row
from bokeh.models import Spinner


x = np.random.rand(10)
y = np.random.rand(10)

p = figure(x_range=(0, 1), y_range=(0, 1))
points = p.scatter(x=x, y=y, size=4)

spinner = Spinner(title="Glyph size", low=1, high=40, step=0.5, value=4, width=80)
spinner.js_link('value', points.glyph, 'size')

show(row(column(spinner, width=100), p))
```

#### 选项卡


```python
from bokeh.models import Panel, Tabs
from bokeh.plotting import figure

p1 = figure(plot_width=300, plot_height=300)
p1.circle([1, 2, 3, 4, 5], [6, 7, 2, 4, 5], size=20, color="navy", alpha=0.5)
tab1 = Panel(child=p1, title="circle")

p2 = figure(plot_width=300, plot_height=300)
p2.line([1, 2, 3, 4, 5], [6, 7, 2, 4, 5], line_width=3, color="navy", alpha=0.5)
tab2 = Panel(child=p2, title="line")

show(Tabs(tabs=[tab1, tab2]))
```

#### 文本区域输入
用于从用户那里收集多行文本的小部件:


```python
from bokeh.models import CustomJS, TextAreaInput

text_area_input = TextAreaInput(value="default", rows=6, title="Label:")
text_area_input.js_on_change("value", CustomJS(code="""
    console.log('text_area_input: value=' + this.value, this.toString())
"""))

show(text_area_input)
```


```python
from bokeh.models import CustomJS, TextInput

text_input = TextInput(value="default", title="Label:")
text_input.js_on_change("value", CustomJS(code="""
    console.log('text_input: value=' + this.value, this.toString())
"""))

show(text_input)
```

####  开关按钮


```python
from bokeh.models import CustomJS, Toggle

toggle = Toggle(label="Foo", button_type="success")
toggle.js_on_click(CustomJS(code="""
    console.log('toggle: active=' + this.active, this.toString())
"""))

show(toggle)
```




<div class="bk-root" id="fdd5b0bc-87b7-481c-8161-64f0dc4141ce" data-root-id="91819"></div>



