---
title: Java String 字符串初始化
date: 2018-08-08 15:39:04
categories: 
- Java
tags: 
- String
---
本文摘录于 [占小狼](https://www.jianshu.com/p/2f209af80f84 "占小狼")，写的不错。

字符串可以通过两种方式进行初始化：字面常量和String对象。

## 一、字面常量 ##
<pre>
public class Test {
    public static void main(String[] args) {
        String a = "java";
        String b = "java";
        String c = "ja" + "va";
    }
}
</pre>

通过 "javap -c" 命令查看字节码指令实现：

![](https://i.imgur.com/765iJZf.png)

其中ldc指令将int、float和String类型的常量值从常量池中推送到栈顶，所以a和b都指向常量池的”java”字符串。通过指令实现可以发现：变量a、b和c都指向常量池的 “java” 字符串，表达式 “ja” + “va” 在编译期间会把结果值”java”直接赋值给c。

## 二、String对象 ##
<pre>
public class Test {
    public static void main(String[] args) {
        String a = "java";
        String b = new String("java");
    }
}
</pre>

字节码实现如下：


![](https://i.imgur.com/vqcQ0kH.png)

其中3 ~ 9行指令对应代码 String c = new String("java"); 实现：

1、第3行new指令，在Java堆上为String对象申请内存；


2、第7行ldc指令，尝试从常量池中获取”java”字符串，如果常量池中不存在，则在常量池中新建”java”字符串，并返回；


3、第9行invokespecial指令，调用构造方法，初始化String对象。


其中String对象中使用char数组存储字符串，变量a指向常量池的”java”字符串，变量c指向Java堆的String对象，且该对象的char数组指向常量池的”java”字符串，所以很显然 a != c，如下图所示：


![](https://i.imgur.com/O0p3MGt.png)

## 三、字面量 + String对象 ##

<pre>
public class StringTest {    
	public static void main(String[] args) {        
		String a = "hello ";        
		String b = "world";        
		String c = a + b;        
		String d = "hello world";
    }
}
</pre>

字节码实现如下：


![](https://i.imgur.com/4QnihVU.png)

其中6 ~ 21行指令对应代码 String c = a + b; 实现：

1、第6行new指令，在Java堆上为StringBuilder对象申请内存；

2、第10行invokespecial指令，调用构造方法，初始化StringBuilder对象；

3、第14、18行invokespecial指令，调用append方法，添加a和b字符串；

4、第21行invokespecial指令，调用toString方法，生成String对象。

通过指令实现可以发现，字符串变量的连接动作，在编译阶段会被转化成StringBuilder的append操作，变量c最终指向Java堆上新建String对象，变量d指向常量池的”hello world”字符串，所以 c != d。

不过有种特殊情况，当final修饰的变量发生连接动作时，虚拟机会进行优化，将表达式结果直接赋值给目标变量：
<pre>
public class StringTest {    
	public static void main(String[] args) {        
		final String a = "hello ";        
		final String b = "world";        
		String c = a + b;        
		String d = "hello world";    
	}
}
</pre>
指令实现如下：

![](https://i.imgur.com/ouuccN5.png)