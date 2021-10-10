---
title: Java泛型
date: 2019-07-22 21:46:20
tags: Java
---
1. ### 泛型只在编译阶段有效

    泛型只在编译阶段有效，是为了解决不确定具体对象类型的问题。
```java
List<String> stringArrayList = new ArrayList<String>();
List<Integer> integerArrayList = new ArrayList<Integer>();
System.out.println(stringArrayList.getClass().equals(integerArrayList.getClass()));

Generic<String> stringGeneric = new Generic<>("test");
Generic<Integer> integerGeneric = new Generic<>(123);
System.out.println(stringGeneric.getClass().equals(integerGeneric.getClass()));
```
    输出为
```java
true
true
```
    说明Java中的泛型，只在编译阶段有效。在编译过程中，正确检验泛型结果后，会将泛型的相关信息擦除，并且在对象进入和离开方法的边界处添加类型检查和类型转换的方法。也就是说，泛型信息不会进入到运行时阶段。
<!--more-->
2.  ###  约定俗成的符号
    - `E`代表`Element`，用于集合中的元素
    - `T`代表 `the Type of object`，表示某个类
    - `K`代表`Key`，`V`代表`Value`，用于键值对元素。
    - `?`表示不确定类型

3. ###  泛型类
> 泛型类型用于类的定义中，被称为泛型类。通过泛型可以完成对一组类的操作对外开放相同的接口。最典型的就是各种容器类，如：List、Set、Map。
```java
class 类名称 <泛型标识：可以随便写任意标识号，标识指定的泛型的类型>{
  private 泛型标识 /*（成员变量类型）*/ var; 
  .....

}
```
    一个简单的泛型类
```java
class Generic<T> {
    private T key;
    public Generic(T key) {
        this.key = key;
    }
    public T getKey() {
        return key;
    }
}
```
    对比非泛型类,使用`Object`作为参数类型
```java
public class NoGeneric {
    private Object key;
    public NoGeneric(Object key){
        this.key = key;
    }
    public Object getKey() {
        return key;
    }
}
```
    泛型类的应用避免了强制转换的风险，提升了安全性和代码的可读性。
```java
Generic<String> stringGeneric = new Generic<>("test");
Generic<Integer> integerGeneric = new Generic<>(123);
// 不需要进行强制类型转换
String key = stringGeneric.getKey();
Integer key1 = integerGeneric.getKey();

NoGeneric stringNoGeneric = new NoGeneric("test");
NoGeneric integerNoGeneric = new NoGeneric(123);
String key2 = (String) stringNoGeneric.getKey();
Integer key3 = (Integer) integerNoGeneric.getKey();
```
4. ### 泛型方法
定义方法：
`修饰符<泛型标识>返回类型 方法名(泛型标识 参数名){    } `
如
```java
public <T> void test(T param){
    System.out.println(param.toString());
}
```
    反例: 这不是一个泛型方法，这是一个普通的方法，只不过使用了泛型通配符?
```java
    public void test(Generic<?> obj){
        System.out.println("泛型测试 key value is " + obj.getKey());
    }
```
5. ### 泛型通配符
`<? extends T>` 的场景是put功能受限, `<? super T>`的场景是get功能受限
