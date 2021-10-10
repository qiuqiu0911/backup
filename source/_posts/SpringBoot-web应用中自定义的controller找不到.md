---
title: SpringBoot web应用中自定义的controller找不到
date: 2020-02-01 12:30:25
tags: 
- Java
---
问题：自定义的`controller`增加`@RestController`注解后访问 仍返回404
{% asset_img 问题.png 问题 %}
原因：springboot 推荐`启动类`与`controller包`在同一层级，默认也是按这个扫描的
解决方案：
1. 可以放在同一层级，如下图
{% asset_img 同层级.png 同层级 %}
2. 也可以通过增加`@ComponentScan`注解解决
``` java
@SpringBootApplication
@ComponentScan(basePackages = "dev.qiuqiu.controller")
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```