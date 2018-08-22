---
title: 浅谈String
date: 2018-08-07 12:39:04
categories: 
- Java
tags: 
- String
---

## String 类 ##
想要了解一个类，那就看它的源码。
<pre>
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {

    private final char value[];

    private int hash;

    private static final long serialVersionUID = -6849794470754667710L;

    private static final ObjectStreamField[] serialPersistentFields =
        new ObjectStreamField[0];

    public String() {
        this.value = "".value;
    }

    public String(String original) {
        this.value = original.value;
        this.hash = original.hash;
    }

    ...........

    public String substring(int beginIndex) {
        if (beginIndex < 0) {
            throw new StringIndexOutOfBoundsException(beginIndex);
        }
        int subLen = value.length - beginIndex;
        if (subLen < 0) {
            throw new StringIndexOutOfBoundsException(subLen);
        }
        return (beginIndex == 0) ? this : new String(value, beginIndex, subLen);
    }

    public String concat(String str) {
        int otherLen = str.length();
        if (otherLen == 0) {
            return this;
        }
        int len = value.length;
        char buf[] = Arrays.copyOf(value, len + otherLen);
        str.getChars(buf, len);
        return new String(buf, true);
    }

    ...........
}
</pre>

从中看出三点：

1. String是final类，意味着String类不能被继承，并且它的成员方法都默认为final方法。
2. String内部结构是final char数组，本质上字符串是通过char数组保存数据的。
3. String相关的substring、concat等操作都是返回新的String，原字符串没有改变。

这里总结一下：“String对象一旦被创建就是固定不变的了，对String对象的任何改变都不影响到原对象，相关的任何change操作都会生成新的对象”。

## 字符串常量池 ##
字符串的分配和其他对象分配一样，是需要消耗时间和空间，而且字符串我们使用的非常多。JVM为了提高性能和减少内存的开销，在实例化字符串的时候进行了一些优化：使用字符串常量池。每当我们创建字符串常量时，JVM会首先检查字符串常量池，如果该字符串已经存在常量池中，那么就直接返回常量池中的实例引用。如果字符串不存在常量池中，就会实例化该字符串并且将其放到常量池中。由于String字符串的不可变性我们可以十分肯定常量池中一定不存在两个相同的字符串（这点对理解上面至关重要）。

Java中的常量池，实际上分为两种形态：静态常量池和运行时常量池。
所谓静态常量池，即*.class文件中的常量池，class文件中的常量池不仅仅包含字符串(数字)字面量，还包含类、方法的信息，占用class文件绝大部分空间。
而运行时常量池，则是jvm虚拟机在完成类装载操作后，将class文件中的常量池载入到内存中，并保存在方法区中，我们常说的常量池，就是指方法区中的运行时常量池。

<pre>
String a = "javaString";
String b = "javaString";
</pre>

a、b和字面上的javaString都是指向JVM字符串常量池中的"javaString"对象，他们指向同一个对象。

<pre>
String c = new String("javaString");
</pre>
new关键字一定会产生一个对象javaString（注意这个javaString和上面的javaString不同），同时这个对象是存储在堆中。所以上面应该产生了两个对象：保存在栈中的c和保存堆中javaString。但是在Java中根本就不存在两个完全一模一样的字符串对象。故堆中的javaString应该是引用字符串常量池中javaString。所以c、javaString、池javaString的关系应该是：c--->javaString--->池javaString。整个关系如下：









