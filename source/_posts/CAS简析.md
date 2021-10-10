---
title: CAS简析
date: 2019-07-14 22:45:10
tags:
- CAS
- Java
---
1. ### CAS结构体系
从结构体系看，CAS包括两部分：CAS Server和CAS Client。
- CAS Server负责完成对用户的认证工作, 需要独立部署, CAS Server 会处理用户名 / 密码等凭证 (Credentials)。
- 负责处理对客户端受保护资源的访问请求，需要对请求方进行身份认证时，重定向到CAS Server进行认证。（原则上，客户端应用不再接受任何的用户名密码等 Credentials）。CAS Client 与受保护的客户端应用部署在一起，以 Filter 方式保护受保护的资源。
{% asset_img cas_architecture.png CAS结构体系 %}

2. ### CAS凭证验证
{% asset_img cas_clip_image001.jpg CAS凭证验证过程 %}

 CAS Client 与受保护的客户端应用部署在一起，以Filter方式保护Web应用的受保护资源，过滤从客户端过来的每一个Web请求，同时，CAS Client 会分析HTTP请求中是否包含请求Service Ticket( ST上图中的Ticket) ，如果没有，则说明该用户是没有经过认证的；于是CAS Client 会重定向用户请求到 CAS Server（Step 2），并传递Service（要访问的目的资源地址）。 Step 3是用户认证过程，如果用户提供了正确的Credentials， CAS Server随机产生一个相当长度、唯一、不可伪造的Service Ticket，并缓存以待将来验证，并且重定向用户到Service 所在地址（附带刚才产生的Service Ticket ）, 并为客户端浏览器设置一个Ticket Granted Cookie（TGC）；CAS Client 在拿到Service和新产生的 Ticket过后，在Step 5和Step6中与CAS Server进行身份核实，以确保 Service Ticket 的合法性。

3. ### CAS术语
CAS系统中设计了5中票据：TGC、ST、PGT、PGTIOU、PT。
- Ticket-granting cookie(TGC)：存放用户身份认证凭证的cookie，在浏览器和CAS Server间通讯时使用，并且只能基于安全通道传输（Https），是CAS Server用来明确用户身份的凭证；
- Service ticket(ST)：服务票据，服务的惟一标识码,由CAS Server发出（Http传送），通过客户端浏览器到达业务服务器端；一个特定的服务只能有一个惟一的ST；
- Proxy-Granting ticket（PGT）：由CAS Server颁发给拥有ST凭证的服务，PGT绑定一个用户的特定服务，使其拥有向CAS Server申请，获得PT的能力；
- Proxy-Granting Ticket I Owe You（PGTIOU）:作用是将通过凭证校验时的应答信息由CAS Server 返回给CAS Client，同时，与该PGTIOU对应的PGT将通过回调链接传给Web应用。Web应用负责维护PGTIOU与PGT之间映射关系的内容表；
- Proxy Ticket (PT)：是应用程序代理用户身份对目标程序进行访问的凭证；