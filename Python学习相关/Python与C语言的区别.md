Python与C语言的区别



Python定义一个函数

def 函数名（）：

​	功能块....

​	.....

C语言
int 函数名（）
{	功能块........

​	.............

}

python函数调用
	def 函数名

函数名（）



import语句

```
import module1[, module2[,... moduleN]]
```

test.py 文件代码：

```
import support  
support.print_func("Runoob")
```

python ‘%’

```
#定义函数，并不会执行
def say_hello():
    num1 = 10
    num2 = 20
    result = num1 + num2
    print("%d" % result)
#调用函数，去执行函数封装的功能
say_hello()

```

C语言 ‘,’

```
#include<stdio.h>
int main:
{    
    int num1,num2,result;
    num1 = 10;
    num2 = 20;
    result = num1 + num2;
    printf("%d",result);   
    return 0;
}

```

#### 装饰器：

@装饰器函数名

def 被装饰函数名（）

作用等同于 装饰器函数名（被装饰函数名）