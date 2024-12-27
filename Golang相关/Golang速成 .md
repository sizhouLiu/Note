# Golang速成

----

golang里的main函数必须要导入package main 

```go
package main 
```

## 导包

```go
import(
	"fmt"
	"time"
)
```

## 函数

 函数的{ 一定是 和函数名在同一行的，否则编译错误

golang中的表达式，加";"，和不加 都可以，建议是不加

```go
func(){
    fmt.println("hello Go!")
    time.sleep(1*time.Second)

}
```

## 变量的声明方式

```go
func main(){
//方法-:声明一个变量 默认的值是0
var a int
fmt.Println("a="，a)fmt.Printf("type ofa=&T\n",a)
//方法二:声明一个变量，初始化一个值var b int = 100fmt.Printin("b =",b)fmt.Printf("type of b=&T\n",b)
var bb string ="abcd"
fmt.Printf("bb =%s,type of bb = &T\n", bb, bb)
//方法三:在初始化的时候，可以省去数据类型，通过值自动匹配当前的变量的数据类型varc=100
fmt.Println("c="，c)fmt.Printf("type of c = &T\n", c)
var cc="abcd"
fmt.Printf("cc=%s,type of cc=%T\n",cc，cc)
//方法四:(常用的方法)省去var关键字，直接自动匹配e :=100fmt.Println("e="，e)fmt.Printf("type of e=&T\n",e)
f :="abcd"
fmt.Println("f=”，f)
fmt.Printf("type of f=&T\n",f)
            }
```

###  全局变量的声明

```go
//声明全局变量 方法一、方法二、方法三是可以的
var gA int = 100
var gB=200
//用方法四来声明全局变量//:= 只能够用在 函数体内来声明//gc := 200
```

### 多变量 声明

```go
var xx，yy int=100，200
fmt.Println"xx="，xx，"，yy ="，yy)
var kk,ll= 100，"Aceld"
fmt.Println("kk="，kk,"，l="，ll)


//多行的多变量声明var(vv int = 100jj bool = true
fmt.Println(
    "vv="，,"，jj="，jj
)
```

## **常量**

> 常量是一个简单值的标识符，在程序运行时，不会被修改的量。
>
> 常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型。

常量的定义格式：

```go
const identifier [type] = value
```

你可以省略类型说明符 [type]，因为编译器可以根据变量的值来推断其类型。

- 显式类型定义：

- ```go
  const b string = "abc"
  ```

- 隐式类型定义：

```go
const b = "abc"
```

常量还可以用作枚举：

```go
const (
    Unknown = 0
    Female = 1
    Male = 2
)
```

数字 0、1 和 2 分别代表未知性别、女性和男性。

常量可以用len(), cap(), unsafe.Sizeof()常量计算表达式的值。常量表达式中，函数必须是内置函数，否则编译不过：

```go
package main


import "unsafe"
const (
    a = "abc"
    b = len(a)
    c = unsafe.Sizeof(a)
)


func main(){
    println(a, b, c)
}
```

输出结果为：abc, 3, 16

> unsafe.Sizeof(a)输出的结果是16 。
>
> 字符串类型在 go 里是个结构, 包含指向底层数组的指针和长度,这两部分每部分都是 8 个字节，所以字符串类型大小为 16 个字节。

## 函数

Go 函数可以返回多个值，例如：

```go
package main


import "fmt"


func swap(x, y string) (string, string) {
   return y, x
}


func main() {
   a, b := swap("Mahesh", "Kumar")
   fmt.Println(a, b)
}
```

#### init函数与import

首先我们看一个例子：init函数：

init 函数可在package main中，可在其他package中，可在同一个package中出现多次。

**main函数**

main 函数只能在package main中。

**执行顺序**



golang里面有两个保留的函数：init函数（能够应用于所有的package）和main函数（只能应用于package main）。这两个函数在定义时不能有任何的参数和返回值。



虽然一个package里面可以写任意多个init函数，但这无论是对于可读性还是以后的可维护性来说，我们都强烈建议用户在一个package中每个文件只写一个init函数。



go程序会自动调用init()和main()，所以你不需要在任何地方调用这两个函数。每个package中的init函数都是可选的，但package main就必须包含一个main函数。



程序的初始化和执行都起始于main包。

如果main包还导入了其它的包，那么就会在编译时将它们依次导入。有时一个包会被多个包同时导入，那么它只会被导入一次（例如很多包可能都会用到fmt包，但它只会被导入一次，因为没有必要导入多次）。



当一个包被导入时，如果该包还导入了其它的包，那么会先将其它包导入进来，然后再对这些包中的包级常量和变量进行初始化，接着执行init函数（如果有的话），依次类推。



等所有被导入的包都加载完毕了，就会开始对main包中的包级常量和变量进行初始化，然后执行main包中的init函数（如果存在的话），最后执行main函数。下图详细地解释了整个执行过程：

![31-init.png](https://cdn.nlark.com/yuque/0/2022/png/26269664/1650528765014-63d3d631-428e-4468-bc95-40206d8cd252.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_60%2Ctext_5YiY5Li55YawQWNlbGQ%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fformat%2Cwebp%2Fresize%2Cw_1125%2Climit_0)

Lib1.go

```go
package InitLib1

import "fmt"

func init() {
    fmt.Println("lib1")
}
```

Lib2.go

```go
package InitLib2

import "fmt"

func init() {
    fmt.Println("lib2")
}
```

main.go

```go
package main

import (
    "fmt"
    _ "GolangTraining/InitLib1"
    _ "GolangTraining/InitLib2"
)

func init() {
    fmt.Println("libmain init")
}

func main() {
    fmt.Println("libmian main")
}
```

输出：

```bash
lib1
lib2
libmain init
libmian main
```



#### 匿名导包

##### import_”fmt“

给fmt包起一个别名，匿名，无法使用当前包的方法，但是会执行当前的包内部的init()方法

##### import aa “fmt”

给fmt包起一个别名，aa，aa.Println()来直接调用。

##### import . “fmt” 

将当前fmt包中的全部方法，导入到当前本包的作用中，fmt包中的全部的方法可以直接使用API来调用，不需要 

#### 函数参数

函数如果使用参数，该变量可称为函数的形参。

形参就像定义在函数体内的局部变量。

调用函数，可以通过两种方式来传递参数：

##### 值传递

值传递是指在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。

默认情况下，Go 语言使用的是值传递，即在调用过程中不会影响到实际参数。

以下定义了 swap() 函数：

```go
/* 定义相互交换值的函数 */
func swap(x, y int) int {
   var temp int


   temp = x /* 保存 x 的值 */
   x = y    /* 将 y 值赋给 x */
   y = temp /* 将 temp 值赋给 y*/


   return temp;
}
```

```go
package main


import "fmt"


func main() {
   /* 定义局部变量 */
   var a int = 100
   var b int = 200


   fmt.Printf("交换前 a 的值为 : %d\n", a )
   fmt.Printf("交换前 b 的值为 : %d\n", b )


   /* 通过调用函数来交换值 */
   swap(a, b)


   fmt.Printf("交换后 a 的值 : %d\n", a )
   fmt.Printf("交换后 b 的值 : %d\n", b )
}


/* 定义相互交换值的函数 */
func swap(x, y int) int {
   var temp int


   temp = x /* 保存 x 的值 */
   x = y    /* 将 y 值赋给 x */
   y = temp /* 将 temp 值赋给 y*/


   return temp;
}
```

以下代码执行结果为：

交换前 a 的值为 : 100

交换前 b 的值为 : 200

交换后 a 的值 : 100

交换后 b 的值 : 200

##### 引用传递(指针传递) 

指针

Go 语言中指针是很容易学习的，Go 语言中使用指针可以更简单的执行一些任务。

接下来让我们来一步步学习 Go 语言指针。

我们都知道，变量是一种使用方便的占位符，用于引用计算机内存地址。

Go 语言的取地址符是 &，放到一个变量前使用就会返回相应变量的内存地址。

以下实例演示了变量在内存中地址：

```go
package main


import "fmt"


func main() {
   /* 定义局部变量 */
   var a int = 100
   var b int= 200


   fmt.Printf("交换前，a 的值 : %d\n", a )
   fmt.Printf("交换前，b 的值 : %d\n", b )


   /* 调用 swap() 函数
   * &a 指向 a 指针，a 变量的地址
   * &b 指向 b 指针，b 变量的地址
   */
   swap(&a, &b)


   fmt.Printf("交换后，a 的值 : %d\n", a )
   fmt.Printf("交换后，b 的值 : %d\n", b )
}


func swap(x *int, y *int) {
   var temp int
   temp = *x    /* 保存 x 地址上的值 */
   *x = *y      /* 将 y 值赋给 x */
   *y = temp    /* 将 temp 值赋给 y */
}
```

#### defer

> defer语句被用于预定对一个函数的调用。可以把这类被defer语句调用的函数称为延迟函数。
>
> defer作用：
>
> - 释放占用的资源
> - 捕捉处理异常
> - 输出日志
>
> 结果
>
> 如果一个函数中有多个defer语句，它们会以LIFO（后进先出）的顺序执行。栈

```go
func Demo(){
	defer fmt.Println("1")
	defer fmt.Println("2")
	defer fmt.Println("3")
	defer fmt.Println("4")
}
func main() {
	Demo()
}
```

```bash
4
3
2
1
```

return  和 defer在同一个函数中 return先执行 defer后执行

因为defer 是在函数的生命周期结束后执行 类似于Python的“ __del__”函数



#### recover错误拦截

> 运行时panic异常一旦被引发就会导致程序崩溃。
>
> Go语言提供了专用于“拦截”运行时panic的内建函数“recover”。它可以是当前的程序从运行时panic的状态中恢复并重新获得流程控制权。
>
> **注意：**recover只有在defer调用的函数中有效。
>
> **示例代码**
>
> **结果**
>
>  
>
> 如果程序没有异常，不会打印错误信息。

```go
func recover interface{}
```

```go
package main

import "fmt"

func Demo(i int) {
	//定义10个元素的数组
	var arr [10]int
	//错误拦截要在产生错误前设置
	defer func() {
		//设置recover拦截错误信息
		err := recover()
		//产生panic异常  打印错误信息
		if err != nil {
			fmt.Println(err)
		}
	}()
	//根据函数参数为数组元素赋值
	//如果i的值超过数组下标 会报错误：数组下标越界
	arr[i] = 10

}

func main() {
	Demo(10)
	//产生错误后 程序继续
	fmt.Println("程序继续执行...")
}
```

## 切片 slice

### 数组



### slice（动态数组）

**slice**

Go 语言切片是对数组的抽象。



Go 数组的长度不可改变，在特定场景中这样的集合就不太适用，Go中提供了一种灵活，功能强悍的内置类型切片`("动态数组")`,与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大。



#### 定义切片



你可以声明一个未指定大小的数组来定义切片：

```go
var identifier []type
```

切片不需要说明长度。



或使用make()函数来创建切片:

```go
var slice1 []type = make([]type, len)


也可以简写为


slice1 := make([]type, len)
```



也可以指定容量，其中capacity为可选参数。



```go
make([]T, length, capacity)
```

#### 切片初始化



```go
s :=[] int {1,2,3 }
```

直接初始化切片，[]表示是切片类型，{1,2,3}初始化值依次是1,2,3.其cap=len=3

```go
s := arr[:]
```

初始化切片s,是数组arr的引用

```go
s := arr[startIndex:endIndex]
```

将arr中从下标startIndex到endIndex-1 下的元素创建为一个新的切片

```go
s := arr[startIndex:]
```

缺省endIndex时将表示一直到arr的最后一个元素

```go
s := arr[:endIndex]
```

缺省startIndex时将表示从arr的第一个元素开始

```go
s1 := s[startIndex:endIndex]
```

通过切片s初始化切片s1

```go
s :=make([]int,len,cap)
```

通过内置函数make()初始化切片s,[]int 标识为其元素类型为int的切片

#### 切片容量的增加

切片的长度和容量不同，长度表示左指针至右指针之间的距离，容量表示左指针至底层数组未尾的距离。

![image-20241117185646836](https://stupid-blog-img.oss-cn-beijing.aliyuncs.com/Blog/image-20241117185646836.png)

切片扩容机制， append的时候，如果长度增加后超过容量，则容量增加2倍



#### len() 和 cap() 函数

切片是可索引的，并且可以由 len() 方法获取长度。

切片提供了计算容量的方法 cap() 可以测量切片最长可以达到多少。

以下为具体实例：

```go
package main


import "fmt"


func main() {
   var numbers = make([]int,3,5)


   printSlice(numbers)
}


func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```

#### 空(nil)切片



一个切片在未初始化之前默认为 nil，长度为 0，实例如下：

```go
package main


import "fmt"


func main() {
   var numbers []int


   printSlice(numbers)


   if(numbers == nil){
      fmt.Printf("切片是空的")
   }
}


func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```

#### 切片截取



可以通过设置下限及上限来设置截取切片*[lower-bound:upper-bound]*，实例如下：

```go
package main


import "fmt"


func main() {
   /* 创建切片 */
   numbers := []int{0,1,2,3,4,5,6,7,8}   
   printSlice(numbers)


   /* 打印原始切片 */
   fmt.Println("numbers ==", numbers)


   /* 打印子切片从索引1(包含) 到索引4(不包含)*/
   fmt.Println("numbers[1:4] ==", numbers[1:4])


   /* 默认下限为 0*/
   fmt.Println("numbers[:3] ==", numbers[:3])


   /* 默认上限为 len(s)*/
   fmt.Println("numbers[4:] ==", numbers[4:])


   numbers1 := make([]int,0,5)
   printSlice(numbers1)


   /* 打印子切片从索引  0(包含) 到索引 2(不包含) */
   number2 := numbers[:2]
   printSlice(number2)


   /* 打印子切片从索引 2(包含) 到索引 5(不包含) */
   number3 := numbers[2:5]
   printSlice(number3)


}


func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```

#### append() 和 copy() 函数

如果想增加切片的容量，我们必须创建一个新的更大的切片并把原分片的内容都拷贝过来。

下面的代码描述了从拷贝切片的 copy 方法和向切片追加新元素的 append 方法。

```go
package main


import "fmt"


func main() {
   var numbers []int
   printSlice(numbers)


   /* 允许追加空切片 */
   numbers = append(numbers, 0)
   printSlice(numbers)


   /* 向切片添加一个元素 */
   numbers = append(numbers, 1)
   printSlice(numbers)


   /* 同时添加多个元素 */
   numbers = append(numbers, 2,3,4)
   printSlice(numbers)


   /* 创建切片 numbers1 是之前切片的两倍容量*/
   numbers1 := make([]int, len(numbers), (cap(numbers))*2)


   /* 拷贝 numbers 的内容到 numbers1 */
   copy(numbers1,numbers)
   printSlice(numbers1)   
}


func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```

## map（dict）

map和slice类似，只不过是数据结构不同，下面是map的一些声明方式。

```go
package main
import (
    "fmt"
)

func main() {
    //第一种声明
    var test1 map[string]string
    //在使用map前，需要先make，make的作用就是给map分配数据空间
    test1 = make(map[string]string, 10) 
    test1["one"] = "php"
    test1["two"] = "golang"
    test1["three"] = "java"
    fmt.Println(test1) //map[two:golang three:java one:php]


    //第二种声明
    test2 := make(map[string]string)
    test2["one"] = "php"
    test2["two"] = "golang"
    test2["three"] = "java"
    fmt.Println(test2) //map[one:php two:golang three:java]

    //第三种声明
    test3 := map[string]string{
        "one" : "php",
        "two" : "golang",
        "three" : "java",
    }
    fmt.Println(test3) //map[one:php two:golang three:java]


    
    language := make(map[string]map[string]string)
    language["php"] = make(map[string]string, 2)
    language["php"]["id"] = "1"
    language["php"]["desc"] = "php是世界上最美的语言"
    language["golang"] = make(map[string]string, 2)
    language["golang"]["id"] = "2"
    language["golang"]["desc"] = "golang抗并发非常good"
    
    fmt.Println(language) //map[php:map[id:1 desc:php是世界上最美的语言] golang:map[id:2 desc:golang抗并发非常good]]


    //增删改查
    // val, key := language["php"]  //查找是否有php这个子元素
    // if key {
    //     fmt.Printf("%v", val)
    // } else {
    //     fmt.Printf("no");
    // }

    //language["php"]["id"] = "3" //修改了php子元素的id值
    //language["php"]["nickname"] = "啪啪啪" //增加php元素里的nickname值
    //delete(language, "php")  //删除了php子元素
    fmt.Println(language)
}
```



遍历

```go
for key,value := range cityMap{
    fmt.println(key,value)
}
```



## 面向对象特性



#### 方法

假设有两个方法，一个方法的接收者是指针类型，一个方法的接收者是值类型，那么：



- 对于值类型的变量和指针类型的变量，这两个方法有什么区别？
- 如果这两个方法是为了实现一个接口，那么这两个方法都可以调用吗？
- 如果方法是嵌入到其他结构体中的，那么上面两种情况又是怎样的？

```go
package main


import "fmt"


//定义一个结构体
type T struct {
    name string
}


func (t T) method1() {
    t.name = "new name1"
}


func (t *T) method2() {
    t.name = "new name2"
}


func main() {


    t := T{"old name"}


    fmt.Println("method1 调用前 ", t.name)
    t.method1()
    fmt.Println("method1 调用后 ", t.name)


    fmt.Println("method2 调用前 ", t.name)
    t.method2()
    fmt.Println("method2 调用后 ", t.name)
}
```

```bash
method1 调用前  old name
method1 调用后  old name
method2 调用前  old name
method2 调用后  new name2
```

当调用`t.method1()`时相当于`method1(t)`，实参和行参都是类型 T，可以接受。此时在`method1`()中的t只是参数t的值拷贝，所以`method1`()的修改影响不到main中的t变量。



当调用`t.method2()`=>`method2(t)`，这是将 T 类型传给了 *T 类型，go可能会取 t 的地址传进去：`method2(&t)`。所以 `method1`() 的修改可以影响 t。



T 类型的变量这两个方法都是拥有的。

```go
package main


import "fmt"
import "math"


type Point struct{ X, Y float64 }


//这是给struct Point类型定义一个方法
func (p Point) Distance(q Point) float64 {
    return math.Hypot(q.X-p.X, q.Y-p.Y)
}


func main() {


    p := Point{1, 2}
    q := Point{4, 6}


    distanceFormP := p.Distance   // 方法值(相当于C语言的函数地址,函数指针)
    fmt.Println(distanceFormP(q)) // "5"
    fmt.Println(p.Distance(q))    // "5"


    //实际上distanceFormP 就绑定了 p接收器的方法Distance


    distanceFormQ := q.Distance   //
    fmt.Println(distanceFormQ(p)) // "5"
    fmt.Println(q.Distance(p))    // "5"


    //实际上distanceFormQ 就绑定了 q接收器的方法Distance
}
```

如果类名首字母大写，表示其他包也能够访问

如果说类的属性首字母大写，表示该属性是对外能够访问的，否则的话只能够类的内部访问










































