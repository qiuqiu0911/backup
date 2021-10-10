---
title: BeanUtils拷贝属性报错IllegalArgumentException
date: 2019-12-06 10:58:39
tags:
- Java
---
`org.springframework.beans.BeanUtils`提供了一个拷贝类属性的方法`copyProperties()`,在两个对象具有相同属性时，可以方便的将一个对象的属性值复制给另一个对象的对应属性。

如 `Source`与`Target`均含有一个名为`id`的属性，通过`BeanUtils.copyProperties(source, target);`可以将source的id属性复制给target。
```java
    class Source {
        public Integer getId() {
            return id;
        }
        public void setId(Integer id) {
            this.id = id;
        }
        Integer id;

    }

    class Target {
        public int getId() {
            return id;
        }

        public void setId(int id) {
            this.id = id;
        }
        int id;
    }
    
    @Test
    public void testCopyProperties() {
        Source source = new Source();
        Target target = new Target();
    //  source.setId(123);
        BeanUtils.copyProperties(source, target);
        System.out.println(JSONObject.toJSON(target));
    }
```
但是，如果`source`的id属性为`null`, `BeanUtils.copyProperties(source, target);`方法会抛出异常`IllegalArgumentException`
```java
org.springframework.beans.FatalBeanException: Could not copy property 'id' from source to target; nested exception is java.lang.IllegalArgumentException
	at org.springframework.beans.BeanUtils.copyProperties(BeanUtils.java:690)
    at org.springframework.beans.BeanUtils.copyProperties(BeanUtils.java:602)
```
这个是目标的bean实体没有确定。如果javaBean中但属性含有基本类型，而model模型的对应的属性值为null，就会出现这个异常。
**解决办法**： javaBean中的属性值不要使用基本类型。

参考《阿里巴巴Java开发手册》
>8. 关于基本数据类型与包装数据类型的使用标准如下:
**1) 【强制】所有的POJO类属性必须使用包装数据类型。**
2) 【强制】RPC方法的返回值和参数必须使用包装数据类型。
3) 【推荐】所有的局部变量使用基本数据类型。
说明:POJO 类属性没有初值是提醒使用者在需要使用时，必须自己显式地进行赋值，任何
NPE 问题，或者入库检查，都由使用者来保证。
正例:数据库的查询结果可能是 null，因为自动拆箱，用基本数据类型接收有 NPE 风险。 反例:比如显示成交总额涨跌情况，即正负 x%，x 为基本数据类型，调用的 RPC 服务，调用
不成功时，返回的是默认值，页面显示为 0%，这是不合理的，应该显示成中划线。所以包装 数据类型的 null 值，能够表示额外的信息，如:远程调用失败，异常退出。

