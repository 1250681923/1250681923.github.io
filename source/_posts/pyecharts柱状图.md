---
title: pyecharts柱状图-初阶
date: 2021-12-14 23:11:16
categories:
    - Python数据分析
tags:
    - Python
    - pyechart
    - 柱状图
cover: /img/柱状图.jpg
---
封面为比亚里茨海边瞭望塔上南方风景。本文为小白准备，老手可以划走。

## Pyecharts简介
pyecharts主要基于Web浏览器进行显示，绘制的图形比较多，包括折线图、柱状图、饼图、漏斗图 地图和极坐标图等。使用pyecharts绘图代码量很少，但绘制的图形比较美观。

pyecharts 分为 v0.5.X 和 v1 两个大版本，v0.5.X 和 v1 间不兼容，v1 是一个全新的版本 v0.5.X支持 Python2.7，3.4+。

0.5.x 版本将不再进行维护，v1仅支持 Python3.6+。

柱状图基于pyecharts v1.9.1 版本进行展示(2021.12月最新版本)

## pyecharts柱状图/条形图

### 基本柱状图/条形图
```
from pyecharts import options as opts
from pyecharts.charts import Bar
l1=['星期一','星期二','星期三','星期四','星期五','星期七','星期日']
l2=[100,200,300,400,500,400,300]
bar = (
    Bar()
    .add_xaxis(l1)
    .add_yaxis("基本柱状图", l2)
    .set_global_opts(title_opts=opts.TitleOpts(title="Bar-基本示例", subtitle="我是副标题"))
)
bar.render_notebook()
```
效果：
![1639495180(1)](https://user-images.githubusercontent.com/59725125/146026506-deb7ee71-80bf-4e6d-82f7-d48381537067.png)
add_xaxis:添加横坐标，需传入列表 add_yaxis:添加纵坐标，需传入列表，切列表元素为数值

### 添加坐标轴名称

```
from pyecharts import options as opts
from pyecharts.charts import Bar
l1=['星期一','星期二','星期三','星期四','星期五','星期七','星期日']
l2=[100,200,300,400,500,400,300]
bar = (
    Bar()
    .add_xaxis(l1)
    .add_yaxis("基本柱状图", l2)
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Bar-基本示例"),
        yaxis_opts=opts.AxisOpts(name="人流量"),
        xaxis_opts=opts.AxisOpts(name="星期"),)
)
bar.render_notebook()
```
效果：
![1639495315(1)](https://user-images.githubusercontent.com/59725125/146026901-6d0d797f-53a9-482f-aaad-30f4c514da14.png)

### 多个纵坐标的柱状图/条形图
```
from pyecharts import options as opts
from pyecharts.charts import Bar
l1=['星期一','星期二','星期三','星期四','星期五','星期七','星期日']
l2=[100,200,300,400,500,400,300]
l3=[300,400,500,400,300,200,100]
bar = (
    Bar()
    .add_xaxis(l1)
    .add_yaxis("l2", l2)
    .add_yaxis("l3", l3)
    .set_global_opts(title_opts=opts.TitleOpts(title="Bar-基本示例", subtitle="我是副标题"),
                    toolbox_opts=opts.BrushOpts(),)
)
bar.render_notebook()
```
效果：
![1639495591(1)](https://user-images.githubusercontent.com/59725125/146027670-457ee5af-2cc5-495d-a9bd-b296b54a5a0e.png)

### 设置柱状图间隔和颜色
```
from pyecharts import options as opts
from pyecharts.charts import Bar
l1=['星期一','星期二','星期三','星期四','星期五','星期七','星期日']
l2=[100,200,300,400,500,400,300]
bar = (
    Bar()
    .add_xaxis(l1)
    .add_yaxis("l2",l2,category_gap=0, color='#FFFF00')
    .set_global_opts(title_opts=opts.TitleOpts(title="Bar-基本示例", subtitle="我是副标题"))
)
bar.render_notebook()
```
效果：
![1639495712(1)](https://user-images.githubusercontent.com/59725125/146028054-066d95fb-297f-4a54-b222-03db77a1a212.png)
category_gap:设置间隔

color：设置柱状图颜色

### 横向柱状图

```
from pyecharts import options as opts
from pyecharts.charts import Bar
l1=['星期一','星期二','星期三','星期四','星期五','星期七','星期日']
l2=[100,200,300,400,500,400,300]
l3=[300,400,500,400,300,200,100]
bar = (
    Bar()
    .add_xaxis(l1)
    .add_yaxis("l2", l2)
    .add_yaxis("l3", l3)
    .reversal_axis()
    .set_series_opts(label_opts=opts.LabelOpts(position="right"))
    .set_global_opts(title_opts=opts.TitleOpts(title="横向柱状图"))
)
bar.render_notebook()
```
效果：
![1639495792(1)](https://user-images.githubusercontent.com/59725125/146028303-53e84163-6d6e-4c6c-954d-2aab78ce0809.png)
reversal_axis将图形反转

position="right"表示将数值在图形右侧显示，同理left、center分别表示左侧和中间

### 显示最大值、最小值和平均值

#### 标记线

```
from pyecharts import options as opts
from pyecharts.charts import Bar
import random
l1=['星期一','星期二','星期三','星期四','星期五','星期七','星期日']
l2=[100,200,300,400,500,400,300]
bar = (
    Bar()
    .add_xaxis(l1)
    .add_yaxis("l2", l2)
    .set_global_opts(title_opts=opts.TitleOpts(title="标记线柱状图"))
    .set_series_opts(
        label_opts=opts.LabelOpts(is_show=False),
        markline_opts=opts.MarkLineOpts(
            data=[
                opts.MarkLineItem(type_="min", name="最小值"),
                opts.MarkLineItem(type_="max", name="最大值"),
                opts.MarkLineItem(type_="average", name="平均值"),
            ]
        ),
    )
)
bar.render_notebook()
```
效果：
![1639495892(1)](https://user-images.githubusercontent.com/59725125/146028623-886d6e1e-acd5-4093-bb50-f3bcbb14f450.png)

#### 标记点

```
from pyecharts import options as opts
from pyecharts.charts import Bar
import random
l1=['星期一','星期二','星期三','星期四','星期五','星期七','星期日']
l2=[100,200,300,400,500,400,300]
bar = (
    Bar()
    .add_xaxis(l1)
    .add_yaxis("l2", l2)
    .set_global_opts(title_opts=opts.TitleOpts(title="标记线柱状图"))
    .set_series_opts(
        label_opts=opts.LabelOpts(is_show=False),
        markpoint_opts=opts.MarkPointOpts(
            data=[
                opts.MarkPointItem(type_="min", name="最小值"),
                opts.MarkPointItem(type_="max", name="最大值"),
                opts.MarkPointItem(type_="average", name="平均值"),
            ]
        ),
    )
)
bar.render_notebook()
```
效果：
![1639495962(1)](https://user-images.githubusercontent.com/59725125/146028852-56edef75-ad29-45d2-9663-6ae4d28075b7.png)


### 旋转x轴坐标

```
from pyecharts import options as opts
from pyecharts.charts import Bar
import random
l1=['很长很长很长很长很长的坐标轴{}'.format(i) for i in range(10)]
l2=[random.choice(range(10,100,10)) for i in range(10)]
bar = (
    Bar()
    .add_xaxis(l1)
    .add_yaxis("l2", l2)
    .set_global_opts(xaxis_opts=opts.AxisOpts(axislabel_opts=opts.LabelOpts(rotate=-15)),
                     title_opts=opts.TitleOpts(title="Bar-旋转X轴标签", subtitle="解决标签名字过长的问题"))
)
bar.render_notebook()
```
效果：
![1639496028(1)](https://user-images.githubusercontent.com/59725125/146029116-cc13d521-795d-47a3-b07a-3b2620f967a8.png)
rotate=-15表示将坐标轴逆时针旋转15度

### 横坐标缩放

#### 整体缩放（type_="inside"）
鼠标滑轮转动可以缩放！！效果很不错！！
```
from pyecharts import options as opts
from pyecharts.charts import Bar
import random
l1=['{}日'.format(i) for i in range(1,31)]
l2=[random.choice(range(100,3100,100)) for i in range(1,31)]
bar = (
    Bar()
    .add_xaxis(l1)
    .add_yaxis("l2", l2)
    .set_global_opts(title_opts=opts.TitleOpts(title="区域缩放柱状图"),
                     datazoom_opts=opts.DataZoomOpts(type_="inside"))
)
bar.render_notebook()
```
效果：
![1639496242(1)](https://user-images.githubusercontent.com/59725125/146029659-710258ed-7ef1-423f-9f6e-788f8bcb8e5b.png)
暂时没有动图，小伙伴们可以自己跑一边，鼠标滚轮可以控制缩放！！

#### 左右滑动缩放
更上面一样可以缩放，只不过是下方有个缩放条，可以滑动。
```
from pyecharts import options as opts
from pyecharts.charts import Bar
import random
l1=['{}日'.format(i) for i in range(1,31)]
l2=[random.choice(range(100,3100,100)) for i in range(1,31)]
bar = (
    Bar()
    .add_xaxis(l1)
    .add_yaxis("l2", l2)
    .set_global_opts(title_opts=opts.TitleOpts(title="区域缩放柱状图"),
                     datazoom_opts=opts.DataZoomOpts(type_="slider"))
)
bar.render_notebook()
```
效果：
![1639496381(1)](https://user-images.githubusercontent.com/59725125/146030112-5f0746ea-6143-4c06-950e-3cf951a597d1.png)