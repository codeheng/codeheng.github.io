---
comments: true
---

> Web指所有基于 HTTP 或者其他超文本传输协议开发的应用，包含网页、App、API接口等
>
> 通过 HTTP等文本协议，在客户端和服务端之间进行数据交换。客户端需要将服务端传出的数据展示渲染出来，服务端需要将客户端传入的数据进行对应的处理

## XSS

> Cross-Site Scripting，跨站脚本攻击。浏览器中插入一段恶意的JavaScript脚本，从而窃取你的隐私信息或者仿冒你进行操作

攻击类型：**反射型XSS、基于DOM的以及持久(存储)型XSS**

!!! Note "对比"

    === "反射型XSS"

        浏览器 --> 后端  --> 浏览器

        `</h3><script>alert('xss');</script><h3>`

        通过开头的`</h3>`和结尾的`<h3>`，将原本的`<h3>`标签进行闭合，中间通过`<script>`标签插入JS代码并执行，就完成了整个反射型XSS的流程

        适合前后端一体化应用中，必须用户点击带有特定参数的链接才能引起

    === "基于DOM的XSS"
        
        URL --> 浏览器

        和反射型类似，只不过不需要经过服务端，而是通过JS修改DOM

    === "持久性XSS"

        浏览器 --> 后端  --> 数据库  --> 后端 --> 浏览器

        将恶意的JS脚本写入正常的服务端数据库中，只要用户使用业务功能，就会被注入JS脚本

        存储用户的输入并对它们进行展示的地方，都可能出现持久型XSS。e.g. 搜索结果、评论


通过XSS，攻击者能做什么？

- 窃取Cookie：通过`document.cookie`获得Cookie信息
- 未授权操作: 利用JS特性，直接代替用户在HTML进行各类操作
- 键盘记录和钓鱼：伪造登陆框

如何进行防护？(XSS防护的核心原则就是验证)

- 验证输入 或 验证输出
    * 展示用户内容的时候去进行验证，而不是当用户输入的时候就去验证
    * 验证过程，优先采用编码的方式来完成
- 检测和过滤：白名单规则
- CSP(内容安全策略): 在服务端返回的HTTP header 里面添加一个CSP选项

## SQL注入

> 通过构造一些恶意的输入参数，在应用拼接SQL语句的时候，去篡改正常的SQL语意，从而执行黑客所控制的SQL查询功能

```sql
1. 修改where语句（通过" or ""="）

SELECT * FROM Users WHERE Username ="admin" AND Password ="123456"

SELECT * FROM Users WHERE Username ="" AND Password ="" or ""=""

（""=""的结果是 true，当有一个 or 是true，最终结果就一定是true）

2. 执行任意语句：利用分号将原本的SQL语句进行分割

INSERT INTO Users (Username, Password) VALUES("test","000000"); SELECT * FROM Users;
```

**通过SQL注入，能做什么？**

- 绕过验证
- 任意篡改数据
- 窃取数据(拖库)
    * `SELECT * FROM Users WHERE UserId = 1 UNION SELECT * FROM Users`
- 消耗资源: 利用while打造死循环操作

**如何防止SQL注入？**

- PreparedStatement：将SQL语句的解析和实际执行过程分开，解析：数据库驱动

```java
String sql = "SELECT * FROM Users WHERE UserId = ?";
PreparedStatement statement = connection.prepareStatement(sql);
statement.setInt(1, userId); 
ResultSet results = statement.executeQuery();
```

- 使用存储过程
  * 和上述类似，存储过程防注入是将解析SQL的过程，由数据库驱动转移到了数据库本身
- 验证输入：防护效果最弱

## CSRF/SSRF

> **跨站请求伪造攻击(CSRF)**：黑客通过JS脚本窃取你保存在网页中的身份信息(Cookie中)，通过仿冒你，让你的浏览器发起伪造的请求，最终执行黑客定义的操作
> 
> （黑客控制浏览器发起伪造攻击）

- 通过自动提交表单的形式来发起攻击的，通过抓包分析出相关接口所需要的参数，从而构造对应的 form 表单 （CSRF 在传播和攻击成本上都低于 XSS）

防护办法1：**CSRF Token**

- CSRF Token每次用户正常访问页面时，服务端随机生成返回给浏览器。每一次接口调用，携带均不同。黑客无法提前猜测，即无法构造正确的表单

防护办法2：**二次验证**

- 已登录但仍需输入独立的支付密码（仅用户知道，黑客无法获取）

> **服务端请求伪造(SSRF)**：黑客控制服务端发起一个其定义的内网请求，从而获取到内网数据 

服务器有代理请求的功能：用户输入URL(如某个图片资源)，然后server会向这个URL发起请求，通过访问其他的服务端资源来完成正常的页面展示

- 利用这个代理发起请求的功能，黑客可以提交一个内网的地址，实现对内网任意服务的访问
    * （即内网穿透：让外网的设备找到处于内网的设备）
- 黑客可以利用SSRF向内网发起任意的HTTP请求，后果：
    * **内网探测**
    * **文件读取**：选择不同的协议类型，读取server的文件内容e.g.`file:// `

!!! Example "内网探测举例"

    请求的地址: `https://image.baidu.com/search/detail?objurl=http://s1.sinaimg.cn/picture.jpg`

    - `http://s1.sinaimg.cn/picture.jpg`会返回图片，所以网页会正常展示

    黑客构造一个恶意的请求地址：`https://image.baidu.com/search/detail?objurl=127.0.0.1:3306`

    - 不断重复尝试不同的IP和端口号，一点一点探测出整个内网的结构

常见防护手段：

- **白名单限制**：目标URL进行限制
- **协议和资源类型限制**
- **请求端限制** ：尽量使用POST，避免GET