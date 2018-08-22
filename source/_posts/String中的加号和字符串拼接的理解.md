---
title: String中的加号和字符串拼接的理解
date: 2018-08-07 12:39:04
categories: 
- Java
tags: 
- String
---

<pre>
String str1 = new String("abc");
String str2 = "abc";
String str3 = "a" + "b" + "c";
String str4 = "ab";
String str5 = str4+ "c";
System.out.println(str1== str2);   //输出false
System.out.println(str2 == str3);  //输出true
System.out.println(str3 == str1);  //输出false
System.out.println(str2 == str5);  //输出false
</pre>

**str1** 通过new关键字来生成对象是在堆区进行的；

**str2** 在编译期间生成了字面常量和符号引用，运行期间字面常量"abc"被存储在运行时常量池（当然只保存了一份）。通过这种方式来将String对象跟引用绑定的话，JVM执行引擎会先在运行时常量池查找是否存在相同的字面常量，如果存在，则直接将引用指向已经存在的字面常量；否则在运行时常量池开辟一个空间来存储该字面常量，并将引用指向该字面常量。

**str3** 对于直接相加字符串，效率很高，因为在编译器便确定了它的值，也就是说形如"a"+"b"+"c"的字符串相加，在编译期间便被优化成了"abc"。

**str5** 是用str4+"c"的形式。这个时候str5的值不是编译时候能确定的，它已经不再会往常量池存放，是一个字符串变量。这个时候，底层是通过StringBuffer的append方法，最终返回new的String。所以str5的地址只的不是常量池区域的地址，而是指向堆内存中的区域。
