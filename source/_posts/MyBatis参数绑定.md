---
title: MyBatis参数绑定
date: 2019-07-15 20:53:01
tags: 
- Java
- MyBatis
- 编程规范
---
合理使用数据库访问框架提供的防注入机制，可以有效防止SQL注入。
如Mybatis提供的`#{}`绑定参数，从而防止SQL注入。
`${}`相当于使用字符串拼接SQL

>   mybatis中的#和$的区别：
    1、#将传入的数据都当成一个字符串，会对自动传入的数据加一个双引号。
    如：where username=#{username}，如果传入的值是111,那么解析成sql时的值为where username="111", 如果传入的值是id，则解析成的sql为where username="id".　
    2、$将传入的数据直接显示生成在sql中。
    如：where username=${username}，如果传入的值是111,那么解析成sql时的值为where username=111；
    如果传入的值是;drop table user;，则解析成的sql为：select id, username, password, role from user where username=;drop table user;
    3、#方式能够很大程度防止sql注入，$方式无法防止Sql注入。
    4、$方式一般用于传入数据库对象，例如传入表名.
    5、一般能用#的就别用$，若不得不使用“${xxx}”这样的参数，要手工地做好过滤工作，来防止sql注入攻击。
    6、在MyBatis中，“${xxx}”这样格式的参数会直接参与SQL编译，从而不能避免注入攻击。但涉及到动态表名和列名时，只能使用“${xxx}”这样的参数格式。所以，这样的参数需要我们在代码中手工进行处理来防止注入。
    【结论】在编写MyBatis的映射语句时，尽量采用“#{xxx}”这样的格式。若不得不使用“${xxx}”这样的参数，要手工地做好过滤工作，来防止SQL注入攻击。
`https://www.cnblogs.com/mmzs/p/8398405.html`