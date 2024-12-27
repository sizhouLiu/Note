# Java速成

## 配置开发环境

4、配置Java开发环境时，需要设置环境变量path和classpath,设置path的作用是▁，设置classpath的作用是▁。

正确答案

第一空：在任何路径均可以访问JDK中的命令    第二空：设置类库路径

## 基本类型

java的char保存一个Unicode字符

## 数组

Java的数组有几个特点：
数组所有元素初始化为默认值，整型都是0，浮点型是0.0，布尔型是false；
数组一旦创建后，大小就不可改变。

## 引用类型

要判断引用类型的变量内容是否相等，必须使用`equals()`方法：

```
public class Main {
    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "HELLO".toLowerCase();
        System.out.println(s1);
        System.out.println(s2);
        if (s1.equals(s2)) {
            System.out.println("s1 equals s2");
        } else {
            System.out.println("s1 not equals s2");
        }
    }
}

```

执行语句`s1.equals(s2)`时，如果变量`s1`为`null`，会报`NullPointerException`：

要避免`NullPointerException`错误，可以利用短路运算符`&&`：



```
public class Main {
    public static void main(String[] args) {
        String fruit = "apple";
        switch (fruit) {
        case "apple":
            System.out.println("Selected apple");
            break;
        case "pear":
            System.out.println("Selected pear");
            break;
        case "mango":
            System.out.println("Selected mango");
            break;
        default:
            System.out.println("No fruit selected");
            break;
        }
    }
}

```

## 继承多态

为了让子类可以访问父类的字段，我们需要把`private`改为`protected`。用`protected`修饰的字段可以被子类访问：.

如果父类没有默认的构造方法，子类就必须显式调用`super()`并给出参数以便让编译器定位到父类的一个合适的构造方法。

这里还顺带引出了另一个问题：即子类*不会继承*任何父类的构造方法。子类默认的构造方法是编译器自动生成的，不是继承的。

## 包装类

八大基础数据类型（`int,float,double...`）是**不具备对象的特征**的，比如基本数据类型就**不能调用方法**，功能简单，为了让基本数据类型也具备对象的特征，就有了JAVA包装类。

我们一般将“基本数据类型转换成包装类”的过程叫做**装箱**，将“包装类转换成基本数据类型”的过程叫做**拆箱**。

## ![预览大图](https://data.educoder.net/api/attachments/207692)



## 抽象类

如果一个`class`定义了方法，但没有具体执行代码，这个方法就是抽象方法，抽象方法用`abstract`修饰。

因为无法执行抽象方法，因此这个类也必须申明为抽象类（abstract class）。

使用`abstract`修饰的类就是抽象类。我们无法实例化一个抽象类：

```
Person p = new Person(); // 编译错误
```

无法实例化的抽象类有什么用？

因为抽象类本身被设计成只能用于被继承，因此，抽象类可以强迫子类实现其定义的抽象方法，否则编译会报错。因此，抽象方法实际上相当于定义了“规范”。

例如，`Person`类定义了抽象方法`run()`，那么，在实现子类`Student`的时候，就必须覆写`run()`方法：



```
interface 
```



**因为静态方法属于`class`而不属于实例，因此，静态方法内部，无法访问`this`变量，也无法访问实例字段，它只能访问静态字段。**

通过实例变量也可以调用静态方法，但这只是编译器自动帮我们把实例改写成类名而已。

通常情况下，通过实例变量访问静态字段和静态方法，会得到一个编译警告。

静态方法经常用于工具类。例如：

- Arrays.sort()
- Math.random()

静态方法也经常用于辅助方法。注意到Java程序的入口`main()`也是静态方法。

## IO

7下列流中，（）是文件字符输出流  

A、FileWriter  B、FileReader  C、FileInputStream  D、FileOutputStream

正确答案： A  



## 多线程

#### 线程五种状态

新建状态（New）：线程对象被创建后，就进入了新建状态。例如，Thread thread = new Thread()。

就绪状态（Runnable）：也被称为“可执行状态”。线程对象被创建后，其他线程调用了该对象的start()方法，从而来启动该线程，例如，threa.start()。处于就绪状态下的线程，随时可能被CPU调度执行。

运行状态（Running）：线程获取CPU权限进行执行。需要注意的是，线程只能从就绪状态进入到运行状态。

阻塞状态（Blocked）：阻塞状态是线程因为某种原因放弃CPU使用权，暂时停止运行。直到线程进入就绪状态，才有机会转到运行状态

死亡状态（Dead）：线程执行完了或者因异常退出了run()方法，该线程结束生命周期。

#### Lock

保证一段代码的原子性就是通过加锁和解锁实现的。Java程序使用`synchronized`关键字对一个对象进行加锁：

synchronized

用synchronized修饰方法可以把整个方法变为同步代码块，synchronized方法加锁对象是this；

通过合理的设计和数据封装可以让一个类变为“线程安全”；

## GUI编程

#### GUI编程中容器与布局的概念及其常用的组件。

容器：是用来组织或容纳其他组件和容器的特殊组件，是用容器类（Container类）创建的对象。

布局：负责管理组件在容器中的排放顺序。

常用的组件：按钮（JButton）、单选按钮（JRadioButton）、复选按钮（JCheckBox）、标签（JLabel）、文本域（JTextField）、列表（JList）、组合框（JComboBox）、菜单（JMenu）等。

13Java中，（）接口可以处理ActionEvent事件 D

A、FocusListener   B、ComponentListener   C、WindowListener   D、ActionListener

正确答案： D  

当用户在JTextField组件内回车时，会触发▁事件。处理该事件的监听器需要实现接口▁，通过▁方法完成事件处理、最后需要在产生该事件的事件源上通过▁方法注册监听器。

正确答案

第一空： ActionEvent          第二空：ActionListenser

第三空 ：actionPerformed()        第四空：addActionListenser()  



简答题（全是概念 没办法 背吧）

1、类方法与实例方法有什么区别？类方法能调用实例方法吗？

正确答案

类方法是被static修饰的成员方法（1分）

没有被static修饰的成员方法是实例方法（1分）

类方法可以不需要重建实例对象，直接通过类名调用（1分）

类方法不能调用实例方法(1分)

main方法只调用静态变量。

## Java需要背下来的关键字 NND最讨厌这个

接口 interface implements

包 package 

public 

static 

try catch finally

abstract、assert、boolean、break、byte、case、catch、char、class、continue、default、do、double、else、enum、extends、final、finally、float、for、if、implements、import、int、interface、instanceof、long、native、new、package、private、protected、public、return、short、static、strictfp、super、switch、synchronized、this、throw、throws、transient、try、void、volatile、while。

final

Scanner

extends 