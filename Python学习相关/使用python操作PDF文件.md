# 使用python操作PDF文件  

前天我哥有个PDF文件要洗一下表格数据，作为大冤种弟弟的我很不幸被抓去干这个工作

![image 20230414105535141](https://s1.ax1x.com/2023/04/14/ppzM5hq.png) 	

依然是学习部分 之前有学习过操作word文档帮助我舍友处理DOCX文件，现在又出现了PDF，很快办公四件套就要学完了。

操作PDF有很多库可以使用这里我使用了pdfplumber那么显然

安装模块

```
pip install pdfplumber
```

导入模块

```
import pdfplumber
```

## [PDFPlumber使用入门](https://blog.csdn.net/weixin_48629601/article/details/107224376)

### pdfplumber简介

（1）可以方便地获取pdf的各种信息，包括文本、表格、图表、尺寸等，它不支持修改或生成pdf，也不支持对pdf扫描件的处理

（2）pdfplumber中有两个基础类，PDF和Page。前者用来处理整个文档，后者用来处理整个页面

### 实操步骤：

（1） 使用pdfplumber提取表格文本

![image 20230414105134487](https://s1.ax1x.com/2023/04/14/ppzMo90.png)

```python
import pdfplumber
from openpyxl import Workbook
import pandas as pd
with pdfplumber.open("D:\垃圾项目\PDFtest.pdf") as pdf:
    page28 = pdf.pages[2]
    text = page28.extract_text()
    print(text)
```

结果如图

![image 20230414105046820](https://s1.ax1x.com/2023/04/14/ppzM7cT.png)

### ①使用 pdfplumber.open("path/to/file.pdf") 读取pdf，返回一个pdfplumber.PDF类实例

PS.加载带密码的pdf需要传入参数password，例如：pdfplumber.open("file.pdf", password = "test")

### ②pdfplumber.PDF类介绍

Ⅰ.metadata属性：从PDF的Info中获取元数据键 /值对字典。 通常包括“ CreationDate”，“ ModDate”，“ Producer”等。

Ⅱ.pages属性：一个包含多个pdfplumber.Page实例的列表，每一个实例代表PDF每一页的信息。

Ⅲ.len(pdf.pages)——读取页数；first_page=pdf.pages[0]——选取页码

### ③pdfplumber.Page类介绍

Ⅰ.page_number属性：页码顺序，从第一页的1开始，第二页为2，依此类推

Ⅱ.width属性/.height属性：页面宽度/宽度

Ⅲ.extract_text(x_tolerance=0, y_tolerance=0)方法：将页面的所有字符对象整理到一个字符串中。

- 若其中一个字符的x1与下一个字符的x0之差大于x_tolerance，则添加空格。
- 若其中一个字符的doctop与下一个字符的doctop之差大于y_tolerance，则添加换行符。

 Ⅳ.extract_tables(table_settings) 方法：从页面中提取表格数据

- find_tables(table_settings={})：返回Table对象的列表。Table对象提供对.cells，.rows和.bbox属性以及.extract(x_tolerance = 3, y_tolerance = 3)方法的访问。
- extract_tables(table_settings={})：返回从页面上找到的所有表中提取的文本，并以结构table -> row -> cell的形式表示为列表列表的列表。
- extract_table(table_settings={})：返回从页面上最大的表中提取的文本，以列表列表的形式显示，结构为row -> cell。 （如果多个表具有相同的大小——以单元格的数量来衡量——此方法将返回最接近页面顶部的表
- debug_tablefinder(table_settings={})：返回TableFinder类的实例，可以访问.edges，.intersections，.cells和.tables属性

提取表格示例：![image 20230414105535141](https://s1.ax1x.com/2023/04/14/ppzM5hq.png) 

```python
import pdfplumber
from openpyxl import Workbook
import pandas as pd
with pdfplumber.open("D:\垃圾项目\PDFtest.pdf") as pdf:
    page28 = pdf.pages[2]# 选取页码
    first_page = pdf.pages[7]  # 指定页码
    table = first_page.extract_tables()
    print(table)
```

![image 20230414105710838](https://s1.ax1x.com/2023/04/14/ppzMx41.png)

之后将这些数据转成DataFrame对象

```
import pdfplumber
from openpyxl import Workbook
import pandas as pd
with pdfplumber.open("D:\垃圾项目\PDFtest.pdf") as pdf:
    page28 = pdf.pages[2]# 选取页码
    first_page = pdf.pages[7]  # 指定页码
    table = first_page.extract_tables()
    table2 =table[0]
    table_df = pd.DataFrame(table2)
    print(table_df)
```

![image 20230414110138052](https://s1.ax1x.com/2023/04/14/ppzMjE9.png)

保存到CSV文件中（因为csv文件小）

```python
import pdfplumber
from openpyxl import Workbook
import pandas as pd
with pdfplumber.open("D:\垃圾项目\PDFtest.pdf") as pdf:
    # 打开文件
    for i in range(7,48):
        first_page = pdf.pages[i]  # 指定页码
        #table1 = page01.extract_table()  # 提取单个表格

        table = first_page.extract_tables()
        table_2 = table[0]
        table_df = pd.DataFrame(table_2)
        print(table_df)
        try:
            table_df.to_csv(r'D:\垃圾项目\PDF_test2.csv',mode='a', header=False)
        except:
            print("写入成功！")
```

![image 20230414110359889](https://s1.ax1x.com/2023/04/14/ppzMLB4.png)

你以为这就完了嘛？nonono客户需求的是每个单元格最后一行的数据 要实现客户需求~~~~~

![image 20230414105710838](https://s1.ax1x.com/2023/04/14/ppzMx41.png)

我想了一圈用python处理的话非常麻烦 因为我对这个东西并不精通 要做许多判断

观察会发现数据里是写了\n的，这时候就要无敌的Excal出手了 Excal其实算是一个低代码平台，可以编程。

那么套用excal公式

```
=TRIM(RIGHT(SUBSTITUTE(A2,CHAR(10),REPT(" ",200)),200))
```

##### 公式说明：

- **SUBSTITUTE（A2，CHAR（10），REPT（“”，200）**：此SUBSTITUTE函数用于查找单元格A10中的所有换行符（char2），并将每个换行符替换为200个空格字符。
- **RIGHT（SUBSTITUTE（A2，CHAR（10），REPT（“”，200）），200）**：RIGHT函数用于从SUBSTITUTE函数返回的新文本的右侧提取200个字符。
- **修剪（）**：此TRIM函数从RIGHT函数返回的文本中删除所有前导空格。

Excal很强大，如果你会用的话其实很多事情都可以快速解决 。没必要在程序设计上死磕， 自己写了一对BUG还要调试浪费很多时间（我浪费了一个多小时）😔

![image 20230414111629008](https://s1.ax1x.com/2023/04/14/ppzMquF.png)

最终处理结果![image 20230414111700142](https://s1.ax1x.com/2023/04/14/ppzMvNR.png)

## 结语：

与上次搞docx一样虽然是些小工作但是能想到办法快速完成那就算是自我价值的一点实现 最近比较忙 一大堆事情压在头上不轻松对前方路途还是一片迷茫也不知道干啥。学习进度落了下来 ，捣鼓这些小玩意就权当休息了，以后的路很长。我觉得应该停一下好好想一想