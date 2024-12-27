# Markdown基本语法速学

Markdown是一种轻量级标记语言，它以纯文本形式(易读、易写、易更改)编写文档，并最终以HTML格式发布。 Markdown也可以理解为将以MARKDOWN语法编写的语言转换成HTML内容的工具。

**优点**：

- 它是易读（看起来舒服）、易写（语法简单）、易更改纯文 本。处处体现着极简主义的影子。
- 兼容HTML，可以转换为HTML格式发布。
- 跨平台使用。
- 越来越多的网站支持Markdown。
- 更方便清晰地组织你的电子邮件。（Markdown-here, * Airmail）
- 摆脱Word（我不是认真的）。

**缺点**：

1、需要记一些语法（当然，是很简单。五分钟学会）。
2、有些平台不支持Markdown编辑模式。

**注**：本文的显示效果用的是[Typora](https://link.zhihu.com/?target=https%3A//typora.io/)Markdown编辑软件

## 一、标题

在想要设置为标题的文字前面加#来表示 一个#是一级标题，二个#是二级标题，以此类推。支持六级标题。

注：标准语法一般在#后跟个空格再写文字。

示例：

```text
# 这是一级标题
## 这是二级标题
### 这是三级标题
#### 这是四级标题
##### 这是五级标题
###### 这是六级标题
```

效果如下：



![img](https://pic2.zhimg.com/80/v2-73990f26218bea6c153127ee8f93fcc9_1440w.webp)



或者，在文本下方的行上，添加任意数量的==（标题级别1）或 --（标题级别2）的字符。

示例：

```text
这是一级标题
===============

这是二级标题
---------------	
```

效果如下：



![img](https://pic3.zhimg.com/80/v2-380f16b9b5f0d2e2477b1b83ccaaf616_1440w.webp)



## 二、字体

- 加粗

要加粗的文字左右分别用两个*号包起来

- 斜体

要倾斜的文字左右分别用一个*号包起来

- 斜体加粗

要倾斜和加粗的文字左右分别用三个*号包起来

- 删除线

要加删除线的文字左右分别用两个~~号包起来

示例：

```text
**这是加粗的文字**
*这是斜体的文字*`
***这是斜体加粗的文字***
~~这是加删除线的文字~~
```

效果如下：



![img](https://pic3.zhimg.com/80/v2-fb2d303cb27ca4854126cc8d599ddc2a_1440w.webp)



## 三、引用

在段落的每行或者只在第一行使用符号>,还可使用多个嵌套引用，

示例：

```text
>这是引用的内容
>>这是引用的内容
>>>>>>>>>>这是引用的内容
```

效果如下：



![img](https://pic3.zhimg.com/80/v2-c04991131f6201b80d7effce6b3a2902_1440w.webp)



## 四、分割线

三个或者三个以上的 - 或者 * 或者_都可以。

示例：

```text
---
----
***
*****
___
____
```

效果如下：



![img](https://pic3.zhimg.com/80/v2-f151778fe2501f06ea52dabf9de5481a_1440w.webp)



可以看到，显示效果是一样的。

## 五、图片

1）插入互联网上图片

语法：

```text
![图片描述](图片链接 ''图片title'')

图片描述就是显示在图片下面的文字，相当于对图片内容的解释。
图片title是图片的标题，当鼠标移到图片上时显示的内容。
注意：title可加可不加
```

注意：这个图片描述可以不写。

示例：

```text
![杀生丸](https://pic4.zhimg.com/80/v2-560623bcbaa438b1627578aa70f5b08c.png)
```

效果如下：



![img](https://pic1.zhimg.com/80/v2-560623bcbaa438b1627578aa70f5b08c_1440w.webp)

杀生丸



2）插入本地图片链接

语法：

```text
![图片描述](图片本地路径 ''图片title'')
图片描述就是显示在图片下面的文字，相当于对图片内容的解释。
图片title是图片的标题，当鼠标移到图片上时显示的内容。
注意：title可加可不加
```

注意：这个图片描述可以不写。

示例：

```text
![犬夜叉](./image/4b9565e605bccd55f8354609dcd7ebcb.jpg)
```

效果如下：



![img](https://pic1.zhimg.com/80/v2-a31f0bef1a26323d9bbde045495675ac_1440w.webp)

犬夜叉



## 六、超链接

语法：

```text
[超链接名](超链接地址 "超链接title")
注：title可加可不加
```

示例：

```text
[知乎](https://www.zhihu.com/)
[百度](https://www.baidu.com/)
```

效果如下：

[知乎](https://www.zhihu.com/)
[百度](https://link.zhihu.com/?target=https%3A//www.baidu.com/)

## 七、列表

**无序列表**

语法：
无序列表用 - + * 任何一种都可以

```text
- 无序列表内容
+ 无序列表内容
* 无序列表内容

注意：- + * 跟内容之间都要有一个空格
```

效果如下：



![img](https://pic2.zhimg.com/80/v2-a6107fab6ba40f944de680cfac7cbd79_1440w.webp)

可以看到，显示效果是一样的。



**有序列表**语法：
数字加点

```text
1. 有序列表内容
2. 有序列表内容
3. 有序列表内容

注意：序号跟内容之间要有空格
```

效果如下：



![img](https://pic3.zhimg.com/80/v2-8ddd16a96ef4b8cdb6cea7932511221a_1440w.webp)



**列表嵌套**

上一级和下一级之间敲一个Tab键即可

示例一、

```text
* 一级无序列表内容
    * 二级无序列表内容
    * 二级无序列表内容
    * 二级无序列表内容
```

效果如下：



![img](https://pic2.zhimg.com/80/v2-ecda3ccd3da75ceefab892ce545a5c09_1440w.webp)



示例二、

```text
* 一级无序列表内容
    1. 二级有序列表内容
    2. 二级有序列表内容
    3. 二级有序列表内容
```

效果如下：



![img](https://pic4.zhimg.com/80/v2-4c666970faaebec46984686ada2c5c07_1440w.webp)



示例三、

```text
1. 一级有序列表内容
    * 二级无序列表内容
    * 二级无序列表内容
    * 二级无序列表内容
```

效果如下：



![img](https://pic2.zhimg.com/80/v2-769a9dc43e646ba74dfcedcd421e2d7d_1440w.webp)



示例四、

```text
2. 一级有序列表内容
    1. 二级有序列表内容
    2. 二级有序列表内容
    3. 二级有序列表内容
```

效果如下：



![img](https://pic1.zhimg.com/80/v2-1a7bf11f37d7f75cae090827221644d4_1440w.webp)



## 八、表格

语法：

```text
|表头|表头|表头|
|---|:--:|---:|
|内容|内容|内容|
|内容|内容|内容|

第二行分割表头和内容。
- 有一个就行，为了对齐，多加了几个
文字默认居左
-两边加：表示文字居中
-右边加：表示文字居右
```

示例：

```text
|姓名|性别|分数|
|--|:--:|--:|
|小明|男|100|
|小红|女|89|
|小飞|男|88|
```

效果如下：



![img](https://pic3.zhimg.com/80/v2-6f8094a60812e5cd12bdd3aec2b69302_1440w.webp)



## 九、代码

语法：
单行代码：代码之间分别用一个反引号包起来

```text
   `代码内容`
```

代码块：
1.代码之间分别用三个反引号包起来，且两边的反引号单独占一行

```text
(```)
  代码...
  代码...
  代码...
(```)
注：为了防止转译，前后三个反引号处加了小括号，实际是没有的。这里只是用来演示，实际中去掉两边小括号即可。
```

2.使用 4 个空格或者一个制表符（Tab 键）。

```text
    代码...
    代码...
    代码...
```

示例：

单行代码

```text
`print("hello world!")`
```

代码块 方式一、

```text
(```)
package main

import "fmt"

func main() {
	fmt.Println("Hello, world. 你好，世界！ ")
}
(```)
```

方式二、

```text
    package main

    import "fmt"

    func main() {
        fmt.Println("Hello, world. 你好，世界！ ")
    }
```

效果如下：

单行代码



![img](https://pic2.zhimg.com/80/v2-cefaca0e56bcea731024134f9a3fd245_1440w.webp)



代码块



![img](https://pic4.zhimg.com/80/v2-49b319678fa5a807dbacae5025189317_1440w.webp)



代码块方式一与方式二显示效果是一样的

**语法高亮**

代码块要高亮显示，需要在前三个反引号后添加一种语言

示例：

```text
(```)go
package main

import "fmt"

func main() {
	fmt.Println("Hello, world. 你好，世界！ ")
}
(```)
```

效果如下：



![img](https://pic3.zhimg.com/80/v2-efab132673dd1e0ccbcfce4b12980cea_1440w.webp)



## 十、流程图

MarkDown中的流程图语法叫flow，该语法只有两个注意事项：

1. 定义元素
2. 连接定义好的元素

示例：

```text
(```)flow
st=>start: 开始
op=>operation: My Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op
(```)

注：为了防止转译，前后三个反引号处加了小括号，实际是没有的。这里只是用来演示，实际中去掉两边小括号即可。
```

效果如下：



![img](https://pic2.zhimg.com/80/v2-9cbb0622c32d59608a5d5470ccd43a11_1440w.webp)



## 十一、换行

方法1: 连续两个以上空格+回车

示例：

```text
Markdown是一种轻量级标记语言，它以纯文本形式(易读、易写、易更改)编写文档，  
并最终以HTML格式发布。 Markdown也可以理解为将以MARKDOWN语法编写的语言转换成HTML内容的工具。
```

效果如下：



![img](https://pic1.zhimg.com/80/v2-6403ebca3f478fda53d9cb8930f93858_1440w.webp)



方法2：使用html语言换行标签`<br>`：

示例：

```text
Markdown是一种轻量级标记语言，<br>它以纯文本形式(易读、易写、易更改)编写文档，  并最终以HTML格式发布。 Markdown也可以理解为将以MARKDOWN语法编写的语言转换成HTML内容的工具。
```

效果如下：



![img](https://pic2.zhimg.com/80/v2-970526be492a2b28e0fc8e8c2042e295_1440w.webp)



## 十二、缩进字符

推荐使用第三种缩进方式

```text
&nbsp;  缩进1/4中文
&ensp;  缩进半个中文，一个字符
&emsp;  缩进一个中文，2个字符
```

示例:

```text
&nbsp;你若安好，便是晴天。  
&ensp;你若安好，便是晴天。
&emsp;你若安好，便是晴天。
```

效果如下：



![img](https://pic4.zhimg.com/80/v2-5f1dbb9866a8e60c6b435b3bb0931c0f_1440w.webp)



## Reference

- [https://www.jianshu.com/p/191d1e21f7ed](https://link.zhihu.com/?target=https%3A//www.jianshu.com/p/191d1e21f7ed)
- [https://blog.csdn.net/u014061630/article/details/81359144](https://link.zhihu.com/?target=https%3A//blog.csdn.net/u014061630/article/details/81359144)
- [http://markdown.p2hp.com/basic](https://link.zhihu.com/?target=http%3A//markdown.p2hp.com/basic-syntax/)