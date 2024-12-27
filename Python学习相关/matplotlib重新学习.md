# matplotlib重新学习

## 基本要点

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

## matplotlib的简单使用



```
x = np.linspace(-1,1,50)
y = 2**x
plt.plot(x,y)
plt.show
```

#### plt.figure

规定一个一个窗口 下面的所有的步骤都会在这个窗口进行显示

figure(num=None, figsize=None, dpi=None, facecolor=None, edgecolor=None, frameon=True)
num:图像编号或名称，数字为编号 ，字符串为名称
figsize:指定figure的宽和高，单位为英寸；
dpi参数指定绘图对象的分辨率，即每英寸多少个像素，缺省值为80 1英寸等于2.5cm,A4纸是 21*30cm的纸张
facecolor:背景颜色
edgecolor:边框颜色
frameon:是否显示边框

### 坐标轴

#### plt.xlim((left,right))

更换坐标轴的左右端

#### plt.ytick(LIST,LIST)

在刻度上替换为对应范围的文字，第一个是刻度位置 第二个list是文字

如果需要插入一些数学符号需要r“$$”在此类编写 具体语法参考LaTeX

#### ax = plt.gca()

gcs = get current axis 获取轴，用来操作轴的属性和位置。



#### ax.spines["right"].set_color("none")#设置边框颜色

```
ax = plt.gca()
ax.spines["right"].set_color("none")#设置边框颜色
# ax.spines["left"].set_color("none")
ax.spines["top"].set_color("none")
# ax.spines["bottom"].set_color("none")
ax.xaxis.set_ticks_position("bottom")# 用哪一个坐标轴来代替xaxis
ax.yaxis.set_ticks_position("left")
ax.spines["left"].set_position(('data',250)) # 设置y坐标轴的位置
ax.spines["bottom"].set_position(('data',8))# 设置x 坐标在轴的位置
plt.plot(x, y)
plt.plot(x**2,y)
```

### 图例

在plt.plot的时候在可以添加label的的参数

之后plt.legend的时候会自动调用这个label生成图例

当然有时候我们不想要全部的图例这个时候可以给这个图例的添加返回值对象，达到对特定的线段显示图例的效果

```
l1, = plt.plot(x, y,label ="asd")
l2, = plt.plot(x**2,y,color="red",linewidth=1.0,linestyle="--",label ="down")
plt.legend(handles=[l1,l2,],loc="lower left") #显示图例
```

需要注意的是如果要传入特定的线段对象的话返回值命名需要在后面加一个，用于解包

#### plt.legend

*plt.legend( )*中有*handles*、*labels*和*loc*三个参数，其中：

**handles**需要传入你所画线条的**实例对象**，

**labels**是图例的名称（能够覆盖在plt.plot( )中label参数值）

**loc**代表了图例在整个坐标轴平面中的位置（一般选取'best'这个参数值）

*loc = 'XXX'*

这里的'XXX'代表了坐标面中的九个位置，例如loc = 'center'表示坐标平面中心位置，九种参数值及所对应位置如下图所示

![img](https://pic2.zhimg.com/80/v2-ebb05194b09240342c53b6590f3a43a5_1440w.webp)

***loc = (x, y)***

（x, y）表示图例左下角的位置，这是最灵活的一种放置图例的方法，慢慢调整，总会找到你想要的放置图例的位置

### 注解

#### plt.scatter(x,y) 

标注一个点

#### plt.annotate()

plt.annotate()用于标注文字

##### weight 设置字体线型

{‘ultralight’, ‘light’, ‘normal’, ‘regular’, ‘book’, ‘medium’, ‘roman’, ‘semibold’, ‘demibold’, ‘demi’, ‘bold’, ‘heavy’, ‘extra bold’, ‘black’}

##### color 设置字体颜色

- {‘b’, ‘g’, ‘r’, ‘c’, ‘m’, ‘y’, ‘k’, ‘w’}
- ‘black’,'red’等
- [0,1]之间的浮点型数据
- RGB或者RGBA, 如: (0.1, 0.2, 0.5)、(0.1, 0.2, 0.5, 0.3)等

##### arrowprops 箭头参数,参数类型为字典dict

- width：箭头的宽度(以点为单位)
- headwidth：箭头底部以点为单位的宽度
- headlength：箭头的长度(以点为单位)
- shrink：总长度的一部分，从两端“收缩”
- facecolor：箭头颜色

##### bbox给标题增加外框：

- boxstyle：方框外形
- facecolor：(简写fc)背景颜色
- edgecolor：(简写ec)边框线条颜色
- edgewidth：边框线条大小



```python
a = [11,17,16,11,12,11,12,6,6,7,8,9,12,15,14,17,18,21,16,17,20,14,15,15,15,19,21,22,22,22,23]
b = [26,26,28,19,21,17,16,19,18,20,20,19,22,23,17,20,21,20,22,15,11,15,5,13,17,10,11,13,12,13,6]
plt.scatter(x,y)
```



## 绘制条形图

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

```
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

## 绘制直方图

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

