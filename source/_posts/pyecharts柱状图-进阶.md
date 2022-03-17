---
title: pyecharts柱状图-进阶
date: 2021-12-14 23:42:19
categories:
    - Python数据分析
tags:
    - Python
    - pyechart
    - 柱状图
cover: /img/柱状图进阶.jpg
---
封面跟初阶的是兄弟俩，估计很多人没体验过独自一人漫无目的行走几十公里，然后平静地等待日出。
关于录制gif:

[电脑如何录制GIF](https://zhuanlan.zhihu.com/p/101336247)

我总算知道了为什么有人倾向于拿python做网站。这图像导出来的动画效果真棒！交互感爆炸

废话不多说，开始正题：

### 数据堆叠柱状图
```
from pyecharts import options as opts
from pyecharts.charts import Bar
l1=['星期一','星期二','星期三','星期四','星期五','星期七','星期日']
l2=[100,200,300,400,500,400,300]
l3=[300,400,500,400,300,200,100]
bar = (
    Bar()
    .add_xaxis(l1)
    .add_yaxis("l2", l2, stack="stack1")
    .add_yaxis("l3", l3, stack="stack1")
    .set_global_opts(title_opts=opts.TitleOpts(title="数据堆叠"))
)
bar.render_notebook()
```
效果：
![1639496949(1)](https://user-images.githubusercontent.com/59725125/146031828-cf767897-e3a4-448c-bd62-99186f4fd4ea.png)
### 柱状图和折线图合并
```
import pyecharts.options as opts
from pyecharts.charts import Bar, Line

x_data = ["1月", "2月", "3月", "4月", "5月", "6月", "7月", "8月", "9月", "10月", "11月", "12月"]
y_data = [2.6,5.9,9.0,26.4,28.7,70.7,175.6,182.2,48.7,18.8,6.0,2.3,]
bar = (
    Bar(init_opts=opts.InitOpts(width="1000px", height="500px"))
    .add_xaxis(xaxis_data=x_data)
    .add_yaxis(
        "降水量",
        y_data,
        label_opts=opts.LabelOpts(is_show=False),
    )
    .set_global_opts(
        tooltip_opts=opts.TooltipOpts(
            is_show=True, trigger="axis", axis_pointer_type="cross"
        ),
        xaxis_opts=opts.AxisOpts(
            type_="category",
            axispointer_opts=opts.AxisPointerOpts(is_show=True, type_="shadow"),
        ),
        yaxis_opts=opts.AxisOpts(
            name="水量",
            type_="value",
            min_=0,
            max_=250,
            interval=50,
            axislabel_opts=opts.LabelOpts(formatter="{value} ml"),
            axistick_opts=opts.AxisTickOpts(is_show=True),
            splitline_opts=opts.SplitLineOpts(is_show=True),
        ),
    )
)

line = (
    Line()
    .add_xaxis(xaxis_data=x_data)
    .add_yaxis(
        series_name="降水量",
        yaxis_index=0,
        y_axis=[2.6,5.9,9.0,26.4,28.7,70.7,175.6,182.2,48.7,18.8,6.0,2.3,],
        label_opts=opts.LabelOpts(is_show=False),
    )
)

bar.overlap(line).render_notebook()
```
效果：
![1639497662(1)](https://user-images.githubusercontent.com/59725125/146034013-a814dcc8-8e15-441a-b271-ca06b6caea67.png)
yaxis_opts=opts.AxisOpts(）中可以设置纵坐标起止范围和间隔


### 双纵坐标柱状图
```
import pyecharts.options as opts
from pyecharts.charts import Bar, Line
x_data = ["1月", "2月", "3月", "4月", "5月", "6月", "7月", "8月", "9月", "10月", "11月", "12月"]
y_data = [2.0, 4.9,7.0, 23.2,25.6,76.7,135.6,162.2,32.6,20.0, 6.4,3.3,]

bar = (
    Bar(init_opts=opts.InitOpts(width="1000px", height="600px"))
    .add_xaxis(xaxis_data=x_data)
    .add_yaxis(
        "蒸发量",
        y_data,
        yaxis_index=0,
        label_opts=opts.LabelOpts(is_show=False),
    )
    .add_yaxis(
        "平均温度",
        [2.0, 2.2, 3.3, 4.5, 6.3, 10.2, 20.3, 23.4, 23.0, 16.5, 12.0, 6.2],
        yaxis_index=1,
        label_opts=opts.LabelOpts(is_show=False),
    )
    .extend_axis(
        yaxis=opts.AxisOpts(
            name="温度",
            type_="value",
            min_=0,
            max_=25,
            interval=5,
            axislabel_opts=opts.LabelOpts(formatter="{value} °C"),
        )
    )
    .set_global_opts(
        tooltip_opts=opts.TooltipOpts(
            is_show=True, trigger="axis", axis_pointer_type="cross"
        ),
        xaxis_opts=opts.AxisOpts(
            type_="category",
            axispointer_opts=opts.AxisPointerOpts(is_show=True, type_="shadow"),
        ),
        yaxis_opts=opts.AxisOpts(
            name="水量",
            type_="value",
            min_=0,
            max_=250,
            interval=50,
            axislabel_opts=opts.LabelOpts(formatter="{value} ml"),
            axistick_opts=opts.AxisTickOpts(is_show=True),
            splitline_opts=opts.SplitLineOpts(is_show=True),
        ),
    )
)
bar.render_notebook()
```
效果：
![1639532596(1)](https://user-images.githubusercontent.com/59725125/146107489-66230bce-0566-4855-a40b-1184e281af98.png)

extend_axis:增加了以温度为刻度的纵坐标轴

add_yaxis:yaxis_index=0表示该数据用第一个坐标轴，yaxis_index=1表示该数据用第二个坐标轴
### 为柱状图添加背景图片
```
from pyecharts import options as opts
from pyecharts.charts import Bar
from pyecharts.commons.utils import JsCode
from pyecharts.faker import Faker
l2=[100,200,300,400,500,400,300]
l3=[300,400,500,400,300,200,100]
bar = (
    Bar(
        init_opts=opts.InitOpts(
            bg_color={"type": "pattern", "image": JsCode("img"), "repeat": "no-repeat"}
        )
    )
    .add_xaxis(Faker.choose())
    .add_yaxis("商家A", l2)
    .add_yaxis("商家B", l3)
    .set_global_opts(
        title_opts=opts.TitleOpts(
            title="Bar-背景图基本示例",
            subtitle="我是副标题",
            title_textstyle_opts=opts.TextStyleOpts(color="white"),
        )
    )
)
bar.add_js_funcs(
    """
    var img = new Image(); img.src = 'https://user-images.githubusercontent.com/59725125/145532785-07737376-50bd-4754-8fe2-2aa7ed01fa4d.jpg';
    """
)
bar.render_notebook()
```
效果：
![1639532813(1)](https://user-images.githubusercontent.com/59725125/146107860-b729379a-136e-403e-a9d5-a013d7a7f792.png)
只需更改img.src中图片url地址即可更换背景

### 为柱状图添加动画

```
from pyecharts import options as opts
from pyecharts.charts import Bar
from pyecharts.faker import Faker

l1=[100,200,300,400,500,400,300]
l2=[300,400,500,400,300,200,100]
bar = (
    Bar(
        init_opts=opts.InitOpts(
            animation_opts=opts.AnimationOpts(
                animation_delay=1000, animation_easing="bounceIn"
            )
        )
    )
    .add_xaxis(Faker.choose())
    .add_yaxis("商家A", l1)
    .add_yaxis("商家B", l2)
    .set_global_opts(title_opts=opts.TitleOpts(title="Bar-动画配置基本示例", subtitle="我是副标题"))
)
bar.render_notebook()
```
![GIF 2021-12-15 9-53-28](https://user-images.githubusercontent.com/59725125/146108606-283939f5-acca-4aca-8000-56cfc5450dec.gif)
