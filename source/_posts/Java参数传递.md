---
title: Java参数传递
date: 2019-09-14 16:00:32
tags: Java
---
无论是对于基本数据类型，还是引用变量，Java中的参数传递都是**值复制**的传递过程。对于引用变量,复制指向**对象的首地址**,双方都可以通过自己的引用变量修改指向对象的相关属性。
而`String`是`immutable`对象,即不可变类。每次修改`String`对象的值，其实都是创建了一个新的`String`对象,再赋值为修改后的值。这也就是`String`对象和其他引用类型表现不同的原因。

```java
/**
 * @author qiuqiu
 */
public class Demo {

    private static int intStatic = 222;
    private static String stringStatic = "old string";
    private static StringBuilder stringBuilderStatic = new StringBuilder("old stringBuilder");

    private static void method(int intStatic){
        intStatic = 777;
    }

    private static void  method(){
        intStatic = 888;
    }

    private static void method(String stringStatic){
        stringStatic = "new string";
    }

    private static void method(StringBuilder stringBuilderStatic1, StringBuilder stringBuilderStatic2){
        stringBuilderStatic1.append(".method.first-");
        stringBuilderStatic2.append("method.second-");

        stringBuilderStatic1 = new StringBuilder("new stringBuilder");
        stringBuilderStatic1.append("new method's append");
    }

    public static void main(String[] args) {
        method(intStatic);
        method(stringStatic);
        method(stringBuilderStatic,stringBuilderStatic);

        System.out.println(intStatic);
        method();
        System.out.println(intStatic);

        System.out.println(stringStatic);
        System.out.println(stringBuilderStatic);
    }

}
```

程序输出为:

```java
222
888
old string
old stringBuilder.method.first-method.second-
```

> 《码出高效 Java开发手册》
