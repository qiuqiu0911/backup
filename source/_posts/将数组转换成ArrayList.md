---
title: 将数组转换成ArrayList
date: 2019-07-14 22:36:47
tags: 
  - Java
  - 编程规范
---
1. ### 使用工具类转换时的坑
使用工具类`Arrays`将数组转换成`List`时,转换后的List不能直接调用`add()`方法增加元素。
{% codeblock lang:java %}
    public void testArrayToList(){
        String[] tests  = {"test1","test2","test3"};
        List<String> strings = Arrays.asList(tests);
        strings.add("1234");
        System.out.println(JSON.toJSONString(strings));
    }
{% endcodeblock %}

    此处会报错 `java.lang.UnsupportedOperationException`

<!--more-->
2. ### 原因分析
工具类`Arrays`的`asList()`方法,返回的是其一个内部类ArrayList, 该类没有重写`add()`方法
{% codeblock lang:java %}
    // Arrays.asList()方法
    /**
     * Returns a fixed-size list backed by the specified array.  (Changes to
     * the returned list "write through" to the array.)  This method acts
     * as bridge between array-based and collection-based APIs, in
     * combination with {@link Collection#toArray}.  The returned list is
     * serializable and implements {@link RandomAccess}.
     *
     * <p>This method also provides a convenient way to create a fixed-size
     * list initialized to contain several elements:
     * <pre>
     *     List&lt;String&gt; stooges = Arrays.asList("Larry", "Moe", "Curly");
     * </pre>
     *
     * @param <T> the class of the objects in the array
     * @param a the array by which the list will be backed
     * @return a list view of the specified array
     */
    @SafeVarargs
    @SuppressWarnings("varargs")
    public static <T> List<T> asList(T... a) {
        return new ArrayList<>(a);
    }
{% endcodeblock %}

    返回的内部类
{% codeblock lang:java %}
    /**
     * @serial include
     */
    private static class ArrayList<E> extends AbstractList<E>
        implements RandomAccess, java.io.Serializable
{% endcodeblock %}

    没有重写的抽象类方法
{% codeblock lang:java %}
    /**
     * {@inheritDoc}
     *
     * <p>This implementation always throws an
     * {@code UnsupportedOperationException}.
     *
     * @throws UnsupportedOperationException {@inheritDoc}
     * @throws ClassCastException            {@inheritDoc}
     * @throws NullPointerException          {@inheritDoc}
     * @throws IllegalArgumentException      {@inheritDoc}
     * @throws IndexOutOfBoundsException     {@inheritDoc}
     */
    public void add(int index, E element) {
        throw new UnsupportedOperationException();
    }

    /**
     * {@inheritDoc}
     *
     * <p>This implementation always throws an
     * {@code UnsupportedOperationException}.
     *
     * @throws UnsupportedOperationException {@inheritDoc}
     * @throws IndexOutOfBoundsException     {@inheritDoc}
     */
    public E remove(int index) {
        throw new UnsupportedOperationException();
    }
{% endcodeblock %}

3. ### 参考《阿里巴巴JAVA编程规范》
{% blockquote %}
5. 【强制】使用工具类 Arrays.asList()把数组转换成集合时，不能使用其修改集合相关的方 法，它的 add/remove/clear 方法会抛出 UnsupportedOperationException 异常。 说明:asList 的返回对象是一个 Arrays 内部类，并没有实现集合的修改方法。Arrays.asList 体现的是适配器模式，只是转换接口，后台的数据仍是数组。
       String[] str = new String[] { "you", "wu" };
List list = Arrays.asList(str); 第一种情况:list.add("yangguanbao"); 运行时异常。 第二种情况:str[0] = "gujin"; 那么list.get(0)也会随之修改。
{% endblockquote %}

4. ### 解决的办法
{% codeblock lang:java %}
List list = new ArrayList<>(Arrays.asList("a", "b", "c"))
{% endcodeblock %}