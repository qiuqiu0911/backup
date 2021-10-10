---
title: 更优雅的关闭资源try-with-resources
date: 2019-09-12 18:12:07
tags: 
  - Java
  - 编程规范
---
`try-with-resources`是 JDK 7 中一个新的异常处理机制，它能够很容易地关闭在`try-catch`语句块中使用的资源。所谓的资源(resource)是指在程序完成后，必须关闭的对象。`try-with-resources`语句确保了每个资源在语句结束时关闭。所有实现了`java.lang.AutoCloseable`接口的对象(其中，它包括实现了`java.io.Closeable` 的所有对象),可以使用作为资源。

在没有使用`try-with-resources`语法糖时:
```java
    private String test(String path) {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(path));
            return br.readLine();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (null != br) {
                    br.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return null;
    }
```
使用后:
```java
    private String test2(String path) {
        try (BufferedReader br = new BufferedReader(new FileReader(path));) {
            return br.readLine();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }
```
反编译发现: 
- try 块没有发生异常时，自动调用 close 方法，如果发生异常，catch 块捕捉并处理异常。
- try 块发生异常，然后自动调用 close 方法，如果 close 也发生异常，catch 块只会捕捉 try 块抛出的异常，close 方法的异常会在catch 中被压制，但是你可以在catch块中，用 Throwable.getSuppressed 方法来获取到压制异常的数组。

```java
    private String test(String path) {
        BufferedReader br = null;

        try {
            br = new BufferedReader(new FileReader(path));
            String var3 = br.readLine();
            return var3;
        } catch (IOException var13) {
            var13.printStackTrace();
        } finally {
            try {
                if (null != br) {
                    br.close();
                }
            } catch (IOException var12) {
                var12.printStackTrace();
            }
        }
        return null;
    }

    private String test2(String path) {
        try {
            BufferedReader br = new BufferedReader(new FileReader(path));
            Throwable var3 = null;

            String var4;
            try {
                var4 = br.readLine();
            } catch (Throwable var14) {
                var3 = var14;
                throw var14;
            } finally {
                if (br != null) {
                    if (var3 != null) {
                        try {
                            br.close();
                        } catch (Throwable var13) {
                            var3.addSuppressed(var13);
                        }
                    } else {
                        br.close();
                    }
                }
            }
            return var4;
        } catch (IOException var16) {
            var16.printStackTrace();
            return null;
        }
    }
```

在处理必须关闭的资源时，使用`try-with-resources`语句替代`try-finally`语句。 生成的代码更简洁，更清晰，并且生成的异常更有用。 `try-with-resources`语句在编写必须关闭资源的代码时会更容易，也不会出错，而使用`try-finally`语句实际上是不可能的。

参考: https://blog.csdn.net/weixin_40255793/article/details/80812961