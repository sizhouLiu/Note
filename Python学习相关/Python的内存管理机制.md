# Python的内存管理机制

## 内存的申请

python申请内存会调用_PyObject_Malloc 函数来进行函数的申请

当申请内存大于512个byte的时候会调用C的malloc函数来申请内存

小于512时就会调用下面的pymalloc_alloc。在调用pymalloc_alloc时会首先进行一个内存对齐。他会把你申请的内存大小都改为8的倍数，比如你申请1~8个byte内存他都会给你8个8byte，这样做的目的也是为了减少内存碎片的产生。当你对齐结束后，Python会去找对应大小的Pool里找一个没有使用的一个Block丢给你，这样就完成了一次内存的分配。同样的返还的时候也是返还到对应的Pool里而不是直接Free。会被加入到Pool里边的FreeBolck链表中去，标记成可用状态。这样做的好处就是不需要再去找系统申请空间。

Python一般不会直接Free内存，只会在Arena里面没有一个Pool的时候free空间，所以Python里对于空间的free都是在Arena这个层级进行操作的

![image-20240104172510193](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20240104172510193.png)

## python的内存管理层级

为了减少内存碎片 python有一套自己的内存管理机制

![image-20240104221757082](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20240104221757082.png)

### Block

一共有三个层级 最小的一层叫做Block

### Pool:

4k和一个内存页一样大,每一个Pool里都有相同大小的Block，一个Pool和另一个Pool之间他们拥有的Block的大小不一样大，但是每一个Pool的内部他们Block的大小都是相同的，

### Arena

Arena里有一个usedPools，usedPools是一个保存许多Pool指针的数组，而这个数组的下标对应着这个Pool的Block的大小 比如第0个Pool的Block的大小就是8个byte第1个就是16个Byte

256kArena里存放了许多的Pool 当Pool从没有使用到使用时 Block才会被使用，每一个Pool的大小是一样的但是Block的大小不一样，因此每一个Pool里存放的Block的数量也是不一样的。当Python 开始运行时会一般会建立16个Arena。在申请空间时候会优先给空间占据比较满的Arena使用 因此Arena实际上是Sort过的，这样就会保证Python不会使内存需求一直上升

## 垃圾回收garba coolction

Python的一个最简单的垃圾回收机制是统计引用计数，也就是你每次引用一些东西的时候Python解释器会帮你数引用数，但是有的时候会出现循环引用的情况，导致释放了对象以后对象的一些属性依然在引用着对方，导致应用技术失效，这个时候就会用garba coolction来处理循环引用的问题

### 容器(container)

所谓容器，就是可以引用其他对象的对象，比如List，Tuple 而 String int 就不是一个容器，也就不存在循环引用的问题。

所有容器类型的Object，在建立的时候就会告诉他你是一个容器 然后将这个Object放到垃圾回收的双向链表里，这也就是为什么Python找不到Object以后垃圾回收依然能找到这个Object

每一次触发垃圾回收的时候就会查找双向循环链表里有没有只剩下循环引用的容器，然后释放这个容器

### genneration

为什么要有genneration 

因为在编程的过程中有大量的Object的生命周期非常短，用完就可以扔了，少部分变量生命周期非常长，如果我们把这些变量一直都放在链表里边，每次垃圾回收都需要检查一遍这些生命周期非常长的对象，而去判断一个变量是否循环引用是一非常耗时的工作，如果这些对象一直在这个链表里，程序会越跑越慢，因此我们会把这些生命周期非常长的对象复制到另一个genneration里 

在一个容器被创建时永远先存放在genneration0里边，每700次将这些东西合并到genneration1 genneration1 执行10次以后合并到genneration2里，达到一个分级管理的效果

但是genneration2 本身的对象大概率是不会被回收的，因此Cpython对垃圾回收做了一个优化的机制   genneration2中没有检测的对象的数量占总数量的25%以上的时候才会触发genneration2的垃圾回收