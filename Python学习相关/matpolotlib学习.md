# matpolotlib学习

matplotlib能将数据进行可视化,更直观的呈现，另一方面使数据更加客观、更具说服力

matplotlib是最流行的Python底层绘图库，主要做数据可视化图表,名字取材于MATLAB，模仿MATLAB构建

## 基本要点

axis轴,指的是x或者y这种坐标轴

![image-20230720105945245](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20230720105945245.png)

每个红色的点是坐标,把5个点的坐标连接成一条线,组成了一个折线图

#### 绘制图像

假设一天中每隔两个小时(range(2,26,2))的气温(℃)分别是[15,13,14.5,17,20,25,26,26,27,22,18,15]

```python
from matplotlib import pyplot as plt ->导入pyplot
x = range(2,26,2)
	#数据在x轴的位置，是一个可迭代对象
y= [15 , 13 , 14.5 ,17 , 2 , 25 , 26 , 26 ,24, 22 , 18 , 1]
	#数据在y轴的位置,是一个可迭代对象
	>x轴和y轴的数据一起组成了所有要绘制出的坐标
	>分别是(2,15),(4,13),(6,14.5),(8,17)......
	plt.plot(x,y) ->传入x和y,通过plot绘制出折线图
	plt.show()->在执行程序的时候展示图形
```



1. 设置图片大小(想要一个高清无码大图)
2. 保存到本地
3. 描述信息,比如x轴和y轴表示什么,这个图表示什么
4. 调整x或者y的刻度的间距
5. 线条的样式(比如颜色,透明度等)
6. 标记出特殊的点(比如告诉别人最高点和最低点在哪里)
7. 给图片添加一个水印(防伪,防止盗用)

#### 设置图片大小

```python
import matplotlib.pyplot as plt
fig = plt.figure(figsize=(20,8),dpi=80)
	-->figure图形图标的意思，在这里指的就是我们画的图
	-->通过实例化一个fgure并且传递参数,能够在后台自动使用该fgure实例
	--->在图像模糊的时候可以传入dpi参数,让图片更加清晰
x = range(2,26,2)
y = [15 ,13, 14.5,17,20, 25 , 26,26 ,24,22, 18 ,15]
plt.plot(x,y)
plt.savefig("./sig_size.png")->保存图片
		--->可以保存为svg这种矢量图格式,放大不会有锯齿
```

#### 调整X或者Y轴上的刻度

```python
import matplotlib.pyplot as plt
fig = plt.figure(figsize=(10,5))
x = range(2,26,2)
y = [15 , 13 , 14 ,5 , 17 , 20 , 25 , 26 ,26 , 24, 22, 18 , 15]
plt.plot(x,y)
plt.xticks(x)
->设置x的刻度
#plt.xticks(x[::27)
	->当刻度太密集时候使用列表的步长(间隔取值)来解决,matplotlib会自动帮我们对应
```

```python
import matplotlib.pyplot as plt
import random

plt.figure(figsize=(20,8))
x = range(120)
random.seed(10) #设置随机种子，让不同时候随机的得到的结果都一样y = [random.uniform(20,35) for i in range(120)]
plt.plot(x,y)
x_ticks = ["10点[}分".format(i) for i in x if i<60]
x_ticks += ["11点[]分",format(i-60) for i in x if i>60]
->设置x轴上的字符串的刻度
plt.xticks(x[::5],_x_ticks[::5],rotation=90)
->让列表x中的数据和_x_tcks上的数据都传入,最终会在x轴上一一对应的显示出来
>两组数据的长度必须一样,否则不能完全覆盖整个轴
>使用列表的切片,每隔5个选一个数据进行展示
>为了让字符串不会覆盖,使用rotation选项,让字符串旋转90°显示
```

matplotlib默认不支持中文字符，因为默认的英文字体无法显示汉字

查看linux/mac下面支持的字体:

fc-list   查看支持的字体fc-list :lang=zh 查看支持的中文(冒号前面有空格)

通过matplotlib.rc可以修改默认字体,具体方法参见源码(windows/linux)

通过matplotlib 下的font_manager可以解决

```python
import matplotlib.pyplot as plt
import random
impart matplotlib
from matplotlib import font_manager
#这个字体设置为全局设置，只用在该处修改即可(windows和linux下有效
# fant = {}' family':'microsoft Yahei"
#				'size':'10'}
# matplotlib.rc("font",**font)

#设置中文字体(指定具体的字体文件路径,然后在需要显示中文的地方添加fontproperties参数)
my font = font manager,FontProperties(fname="/Svstem/Library/Fonts/pingFang,ttc
plt.xticks(x[::5],x ticks[::5],rotation=90,fontproperties=my_font)
```

#### 给图像添加描述信息

```python
#设置中文字体my_font = font_manager,FontProperties(fname="/System/Library/Fonts/PingFang.ttc"!
plt.xticks(x[::5],_x_ticks[::5],rotation=90,fontproperties=my_font)
plt.xlabel("时间”,fontproperties=my_font) #设置x轴的label
plt.ylabel("温度(()",fontproperties=my_font) #设置y轴的label
plt.title("10点到12点每分钟的时间变化情况",fontproperties=my_font) 设置title
```

## 常用统计图

#### 折线图:

以折线的上升或下降来表示统计数量的增减变化的统计图

特点:能够显示数据的变化趋势，反映事物的变化情况。(变化)![image-20230720112819317](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20230720112819317.png)



#### 直方图:

由一系列高度不等的纵向条纹或线段表示数据分布的情况。 一般用横轴表示数据范围，纵轴表示分布情况。

特点:绘制连续性的数据,展示一组或者多组数据的分布状况(统计)

![image-20230720112822890](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20230720112822890.png)

#### 条形图:

排列在工作表的列或行中的数据可以绘制到条形图中。

特点:绘制连离散的数据,能够一眼看出各个数据的大小,比较数据之间的差别。(统计)

![image-20230720112825218](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20230720112825218.png)

#### 散点图:

用两组数据构成多个坐标点，考察坐标点的分布,判断两变量之间是否存在某种关联或总结坐标点的分布模式。

特点:判断变量之间是否存在数量关联趋势,展示离群点(分布规律)

![image-20230720112830730](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20230720112830730.png)



#### 折线图的应用场景

- 呈现公司产品(不同区域)每天活跃用户数
- 呈现app每天下载数量
- 呈现产品新功能上线后,用户点击次数随时间的变化
- 呈现员工每天上下班时间

#### 绘制散点图

寻找出气温和随时间(天)变化的某种规律

```python
a = [11,17,16,11,12,11,12,6,6,7,8,9,12,15,14,17,18,21,16,17,20,14,15,15,15,19,21,22,22,22,23]
b = [26,26,28,19,21,17,16,19,18,20,20,19,22,23,17,20,21,20,22,15,11,15,5,13,17,10,11,13,12,13,6]
plt.scatter(x,y)
```

![image-20230720113144983](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20230720113144983.png)

#### 散点图的应用场景

- 不同条件(维度)之间的内在关联关系
- 观察数据的离散聚合程度

#### 绘制条形图

```python
a = ["战狼2","速度与激情8","功夫瑜伽","西游伏妖篇","变形金刚5：最后的骑士","摔跤吧！爸爸","加勒比海盗5：死无对证","金刚：骷髅岛","极限特工：终极回归","生化危机6：终章","乘风破浪","神偷奶爸3","智取威虎山","大闹天竺","金刚狼3：殊死一战","蜘蛛侠：英雄归来","悟空传","银河护卫队2","情圣","新木乃伊",]

b=[56.01,26.94,17.53,16.49,15.45,12.96,11.8,11.61,11.28,11.12,10.49,10.3,8.75,7.55,7.32,6.99,6.88,6.86,6.58,6.23] 单位:亿
_x = range(len(a)
_y = b
plt.bar(_x,b,width=.2,color="orange")
           ->bar绘制条形图，只能接受含数字的可迭代对象
           ->width表示长条的宽度,默认0.8
plt.xticks(_x,a,fontproperties=my_font,rotation=90)

```

![image-20230720113446447](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20230720113446447.png)

```python
列表a中电影分别在2017-09-14(b_14), 2017-09-15(b_15), 2017-09-16(b_16)三天的票房,展示列表中电影本身的票房以及同其他电影的数据对比情况

a = ["猩球崛起3：终极之战","敦刻尔克","蜘蛛侠：英雄归来","战狼2"]
b_16 = [15746,312,4497,319]
b_15 = [12357,156,2045,168]
b_14 = [2358,399,2358,362]


plt.bar(_x，b_14，width=_bar_width,label="9月14")
plt.bar([i + _bar_width for i in _x], b_15, width=_bar_width,label="g月15")
plt.bar([i +bar_width * 2 for i in _x], b_16, width=_bar_width,label="9月16"->为什么要这样做呢
x_ticks = [i + _bar_width for i in _x]
->为什么xticks要这样设置呢
plt.xticks(_x ticks, a, fontproperties=my_font)
```

![绘制直方图](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20230720113631209.png)

#### 条形图的应用场景

- 数量统计
- 频率统计(市场饱和度)

#### 绘制直方图

```python
a=[131,  98, 125, 131, 124, 139, 131, 117, 128, 108, 135, 138, 131, 102, 107, 114, 119, 128, 121, 142, 127, 130, 124, 101, 110, 116, 117, 110, 128, 128, 115,  99, 136, 126, 134,  95, 138, 117, 111,78, 132, 124, 113, 150, 110, 117,  86,  95, 144, 105, 126, 130,126, 130, 126, 116, 123, 106, 112, 138, 123,  86, 101,  99, 136,123, 117, 119, 105, 137, 123, 128, 125, 104, 109, 134, 125, 127,105, 120, 107, 129, 116, 108, 132, 103, 136, 118, 102, 120, 114,105, 115, 132, 145, 119, 121, 112, 139, 125, 138, 109, 132, 134,156, 106, 117, 127, 144, 139, 139, 119, 140,  83, 110, 102,123,107, 143, 115, 136, 118, 139, 123, 112, 118, 125, 109, 119, 133,112, 114, 122, 109, 106, 123, 116, 131, 127, 115, 118, 112, 135,115, 146, 137, 116, 103, 144,  83, 123, 111, 110, 111, 100, 154,136, 100, 118, 119, 133, 134, 106, 129, 126, 110, 111, 109, 141,120, 117, 106, 149, 122, 122, 110, 118, 127, 121, 114, 125, 126,114, 140, 103, 130, 141, 117, 106, 114, 121, 114, 133, 137,  92,121, 112, 146,  97, 137, 105,  98, 117, 112,  81,  97, 139, 113,134, 106, 144, 110, 137, 137, 111, 104, 117, 100, 111, 101, 110,105, 129, 137, 112, 120, 113, 133, 112,  83,  94, 146, 133, 101,131, 116, 111,  84, 137, 115, 122, 106, 144, 109, 123, 116, 111,111, 133, 150]

```

组数要适当,太少会有较大的统计误差,大多规律不明显

![image-20230720113900073](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20230720113900073.png)

```python
bin_width = 3 #设置组距为3num_bins = int((max(a)-min(a))/bin_width) #分为多少组
plt.hist(a, num_bins)->传入需要统计的数据，以及组数即可#plt.hist(a,[min(a)+ixbin_widthforin range(num bins)->可以传入一个列表,长度为组数,值为分组依据,当组距不均匀的时候使用
# plt.hist(a, num_bins,normed=1
->normed:bool 是否绘制频率分布直方图,默认为频数直方图plt.xticks(list(range(min(a),max(a)[::bin_width],rotation=45)
plt.grid(True，linestyle ="-.",alpha=0.5) #显示网格,alpha为透明度
```

#### 直方图应用场景

- 用户的年龄分布状态
- 一段时间内用户点击次数的分布状态
- 用户活跃时间的分布状态

## 常见问题

- 应该选择那种图形来呈现数据
- matplotlib.plot(x,y)
- matplotlib.bar(x,y)
- matplotlib.scatter(x,y)
- matplotlib.hist(data,bins,normed)
- xticks和yticks的设置
- label和titile,grid的设置
- 绘图的大小和保存图片

## 流程总结

1. 明确问题
2. 选择图形的呈现方式
3. 准备数据
4. 绘图和图形完善

## [更多图像样式](http://matplotlib.org/gallery/index.html
)