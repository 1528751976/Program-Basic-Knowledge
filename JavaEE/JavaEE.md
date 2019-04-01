# JavaEE

@toc

### JSP&Servlet部分

**1. jsp 和 servlet 有什么区别？**

* JSP是Java语言嵌入到Html代码中而来的，是Java和HTML组合成一个扩展名为.jsp的文件，其本质就是Servlet。但JVM不能识别JSP的代码，Web容器将JSP的代码编译成JVM能够识别的Java类。 
* Jsp更擅长页面显示,Servlet更擅长逻辑控制。Servlet则是个完整的Java类，这个类的Service方法用于生成对客户端的响应。Servlet的应用逻辑是在Java文件中，并且完全从表示层中的HTML里分离开来。 
* Servlet中没有内置对象，Jsp中的内置对象都是必须通过HttpServletRequest对象，HttpServletResponse对象以及HttpServlet对象得到.

**2. jsp 有哪些内置对象？作用分别是什么？**

JSP 共有以下9个内置的对象： 
* request 用户端请求，此请求会包含来自GET/POST请求的参数，并且提供了几个用于获取cookie, header, 和session数据的有用的方法。
* response 网页传回用户端的回应
* pageContext 管理网页的属性，提供了对JSP页面内所有的对象及名字空间（就是四大作用域空间，如page空间、request空间、session空间、application空间）的访问。
* session 与请求有关的会话期，Session可以存贮用户的状态信息。
* application servlet 类似于系统的全局变量，用于实现Web应用中的资源共享。
* out 用来传送回应的输出
* config servlet 存放JSP编译后的初始数据。
* page JSP网页本身
* exception 针对错误网页，未捕捉的例外。表示JSP页面运行时产生的异常和错误信息，该对象只有在错误页面（page指令中设定isErrorPage为true的页面）中才能够使用。

**3. 说一下 jsp 的 4 种作用域？**

* JSP的四大作用域：page、request、session、application。
* 这四大作用域，其实就是其九大内置对象中的四个，为什么说他们也是JSP的四大作用域呢？因为这四个对象都能存储数据，比如request.setAttribute()注意和request.setParameter()区分开来，一个是存储在域中的、一个是请求参数，session.setAttribute()、application其实就是SerlvetContext，自然也有setAttribute()方法。而page作用域的操作就需要依靠pageContext对象来进行了。在上面我们也有提到JSP的四大作用域，

   * page作用域：代表变量只能在当前页面上生效
   * request：代表变量能在一次请求中生效，一次请求可能包含一个页面，也可能包含多个页面，比如页面A请求转发到页面B
   * session：代表变量能在一次会话中生效，基本上就是能在web项目下都有效，session的使用也跟cookie有很大的关系。一般来说，只要浏览器不关闭，cookie就会一直生效，cookie生效，session的使用就不会受到影响。
   * application：代表变量能一个应用下(多个会话)，在服务器下的多个项目之间都能够使用。比如baidu、wenku等共享帐号。

**4. session 和 cookie 有什么区别？**

1）cookie以文本格式存储在浏览器上，存储量有限，只允许4KB；而会话存储在服务端，可以无限量存储多个变量并且比cookie更安全。
2）设置cookie时间可以使cookie过期。使用session-destory（），我们将会销毁会话。
3）cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗，考虑到安全应当使用session。
4）session会在一定时间内保存在服务器上。当访问增多，会比较占用服务器的性能，考虑到减轻服务器性能方面，应当使用COOKIE。

**5. 说一下 session 的工作原理？**

* session机制是一种服务器端的机制，服务器使用一种类似于散列表的结构（也可能就是使用散列表）来保存信息。
* 当程序需要为某个客户端的请求创建一个session的时候，服务器首先检查这个客户端的请求里是否已包含了一个session标识 - 称为session id，如果已包含一个session id则说明以前已经为此客户端创建过session，服务器就按照session id把这个session检索出来使用（如果检索不到，可能会新建一个），如果客户端请求不包含session id，则为此客户端创建一个session并且生成一个与此session相关联的session id，session id的值应该是一个既不会重复，又不容易被找到规律以仿造的字符串，这个session id将被在本次响应中返回给客户端保存。 保存这个session id的方式可以采用cookie，这样在交互过程中浏览器可以自动的按照规则把这个标识发挥给服务器。
* 由于cookie可以被人为的禁止，必须有其他机制以便在cookie被禁止时仍然能够把session id传递回服务器。经常被使用的一种技术叫做URL重写，就是把session id直接附加在URL路径的后面，附加方式也有两种，一种是作为URL路径的附加信息，表现形式为[http://...../xxx;jsessionid=ByOK](https://blog.csdn.net/xxx;jsessionid=ByOK) ... 99zWpBng!-145788764 另一种是作为查询字符串附加在URL后面，表现形式为[http://...../xxx?jsessionid=ByOK](https://blog.csdn.net/xxx?jsessionid=ByOK) ... 99zWpBng!-145788764 这两种方式对于用户来说是没有区别的，只是服务器在解析的时候处理的方式不同，采用第一种方式也有利于把session id的信息和正常程序参数区分开来。  为了在整个交互过程中始终保持状态，就必须在每个客户端可能请求的路径后面都包含这个session id。
* 另一种技术叫做表单隐藏字段。就是服务器会自动修改表单，添加一个隐藏字段，以便在表单提交时能够把session id传递回服务器。
* 在谈论session机制的时候，常常听到这样一种误解“只要关闭浏览器，session就消失了”。其实可以想象一下会员卡的例子，除非顾客主动 对店家提出销卡，否则店家绝对不会轻易删除顾客的资料。对session来说也是一样的，除非程序通知服务器删除一个session，否则服务器会一直保 留，程序一般都是在用户做log off的时候发个指令去删除session。然而浏览器从来不会主动在关闭之前通知服务器它将要关闭，因此服务器根本不会有机会知道浏览器已经关闭，之所以会有这种错觉，是大部分session机制都使用会话cookie来保存session id，而关闭浏览器后这个session id就消失了，再次连接服务器时也就无法找到原来的session。如果服务器设置的cookie被保存到硬盘上，或者使用某种手段改写浏览器发出的 HTTP请求头，把原来的session id发送给服务器，则再次打开浏览器仍然能够找到原来的session。
* 恰恰是由于关闭浏览器不会导致session被删除，迫使服务器为seesion设置了一个失效时间，当距离客户端上一次使用session的时间超过这个失效时间时，服务器就可以认为客户端已经停止了活动，才会把session删除以节省存储空间。

**6. 如果客户端禁止 cookie 能实现 session 还能用吗？**

* 如果用户禁止cookie，服务器仍会将sessionId以cookie的方式发送给浏览器，但是，浏览器不再保存这个cookie(即sessionId)了。如果想继续使用session，需要采取其他方式来实现sessionId的跟踪。可以使用url重写来实现sessionId的跟踪。即：把会话ID附加在HTML页面中所有的URL上，这些页面作为响应发送给客户。这样，当用户单击URL时，会话ID被自动作为请求行的一部分发送回服务器。这种方法称为URL重写(URL rewriting)。
_**使用URL重写应该注意下面几点：**_
1. 如果使用URL重写，应该在应用程序的所有页面中，对所有的URL编码，包括所有的超链接和表单的action属性值。
2. 应用程序的所有的页面都应该是动态的。因为不同的用户具有不同的会话ID，因此在静态HTML页面中无法在URL上附加会话ID。
3. 所有静态的HTML页面必须通过Servlet运行，在它将页面发送给客户时会重写URL。

**7. 解释一下什么是 servlet？**

* Servlet是一种服务端的Java技术，可以生成动态WEB页面。Servlet具有更好的可移植性，更强大的功能，更高的效率，更好的安全性。
* Servlet 主要用于处理客户端传来的Http 请求，并返回一个响应，通常来说Servlet就是指HttpServlet，用于处理Http请求，其能够处理的请求有doGet()，doPost()，service()等方法，开发servlet时直接继承javax.servlet,http.HttpServlet.
* Servlet需要在web.xml中进行描述，例如。映射执行servlet的名字，配置servlet类，初始化参数，进行安全配置，URL映射和设置启动优先权。Servlet不仅可以生成HTML脚本输出，也可以生成二进制表单输出。
* 现在许多流行框架都离不开Servlet的支持，比如Spring 容器启动的时候，要在web.xml中装载Spring容器和Actioncontext来初始化Spring的一些参数。如依赖注入，数据库表的映射，初始化系统的安全配置，设置read等属性进行一些相关的操作。

**8. 说一说 Servlet 的生命周期?**

* servlet 有良好的生存期的定义， 包括加载和实例化、 初始化、 处理请求以及服务结束。
* 这个生存期由 javax.servlet.Servlet 接口的 init,service 和 destroy 方法表达。
* web 容器加载 servlet， 生命周期开始。 通过调用 servlet 的 init()方法进行 servlet 的初始化。
* 通过调用 service()方法实现， 根据请求的不同调用不同的 do***()方法。 结束服务， web 容
* 器调用 servlet 的 destroy()方法。

**9. servlrt API 中 forward()与 redirect()的区别？**

* forward是服务器端的转向也就是请求转发，而redirect是客户端的跳转也就是重定向  
* 使用forward浏览器的地址不会发生改变，而redirect会发生改变。 
* forward是一次请求中完成，而redirect是重新发起请求。
* forward是在服务器端完成，而不用客户端重新发起请求，效率较高。

1. 请求转发的特点：  只请求一次，而且属于内部跳转 。地址栏不会发生变化 。不允许访问外部资源 。绝对路径的/代表的是根目录之后 。效率偏高 。请求转发的语法：request.getRequestDispacher(地址).forward(请求对象,响应对象)
2. 重定向的特点：  整个过程发出两次请求 。地址栏会发生变化，并跳转到最新的页面，地址栏也是最新页面的地址 。允许访问外部资源，因为服务器已经响应回了浏览器，而且浏览器也发出了新的请求，由于HTTP是无状态的所以两次请求没有联系，第二次请求可以随意去任何网页  绝对路径的/代表的是端口号之后 。效率偏低，因为有两次请求，相对来说效率低 。重定向语法: response.sendRedirect(地址)

**10.什么情况下调用 doGet()和 doPost()？**

* 前端页面中的form是get就用doGet()，是post就是doPost() 。
* get和post是请求方式，一般get携带的信息量有限制，而且他的内容会在显示栏里面出现，不安全。post可携带大量数据，并且信息不回出现在显示栏里比较安全。当实现查询功能时适合用get，get效率比post高些。

**11.Request 对象的主要方法**

* setAttribute(String name,Object)：设置名字为name的request 的参数值 
* getAttribute(String name)：返回由name指定的属性值 
* getAttributeNames()：返回request 对象所有属性的名字集合，结果是一个枚举的实例 
* getCookies()：返回客户端的所有 Cookie 对象，结果是一个Cookie 数组 
* getCharacterEncoding() ：返回请求中的字符编码方式 
* getContentLength() ：返回请求的 Body的长度 
* getHeader(String name) ：获得HTTP协议定义的文件头信息 
* getHeaders(String name) ：返回指定名字的request Header 的所有值，结果是一个枚举的实例 
* getHeaderNames() ：返回所以request Header 的名字，结果是一个枚举的实例 
* getInputStream() ：返回请求的输入流，用于获得请求中的数据 
* getMethod() ：获得客户端向服务器端传送数据的方法 
* getParameter(String name) ：获得客户端传送给服务器端的有 name指定的参数值 
* getParameterNames() ：获得客户端传送给服务器端的所有参数的名字，结果是一个枚举的实例 
* getParameterValues(String name)：获得有name指定的参数的所有值 
* getProtocol()：获取客户端向服务器端传送数据所依据的协议名称 
* getQueryString() ：获得查询字符串 
* getRequestURI() ：获取发出请求字符串的客户端地址 
* getRemoteAddr()：获取客户端的 IP 地址 
* getRemoteHost() ：获取客户端的名字 
* getSession([Boolean create]) ：返回和请求相关 Session 
* getServerName() ：获取服务器的名字 
* getServletPath()：获取客户端所请求的脚本文件的路径 
* getServerPort()：获取服务器的端口号 
* removeAttribute(String name)：删除请求中的一个属性

**12.request.getAttribute()和 request.getParameter()有何区别?**

* getParameter 得到的都是 String 类型的，获取 POST/GET 传递的参数值，用于客户端重定向redirect时，即点击了链接或提交按扭时传值用。  
* getAttribute 可获取到对象容器中的数据值，返回的是 Object，需进行转换,可用 setAttribute 设置成任意对象，使用很灵活，用于服务器端重定向forward。，只能收到setAttribute 传过来的值，获取的是session。

**13.MVC 的各个部分都有那些技术来实现?如何实现?**

* model：应用的业务逻辑（如：数据库的操作），通过JavaBean实现（hibernate、mybatis、ibatis）
* view：视图层，用于与用户的交互，主要由jsp页面产生。（jsp、FreeMarker、前端三件套、Velocity ）
* controller：处理过程控制，一般是一个servlet。它可以分派用户的请求并选择恰当的视图以用于显示，，也可以解释用户的输入并将它们映射为模型层可执行的操作。（severlet、struts、spring）

**14.说出数据连接池的工作机制是什么?**

* 数据连接池是把数据库连接放到服务器上,比如tomcat上,那么每次操作数据库的时候就不需要再"连接"到数据库再进行相关操作,而是直接操作服务器上的"连接池"。
* 把数据库当做一条河,那么"连接池"就是一个"水池",这个水池里面的水是由事先架好的通向"小溪"的水管引进来的,所以,你想喝水的时候不必大老远地跑到小溪边上,而只要到这个水池。这样的话就可以提高"效率"，但是数据池一般是用在数据量比较大的项目，这样可以提高程序的效率，但就把相关的负荷加在了服务器上,因为这个"池"是在服务器上的，过于频繁的请求会使得服务器负载加重。

_**数据连接池怎么设置？**_

在数据库连接字段的时候可以设置
比如：“DataSource=Server;Initial;Catalog=db;UserID=test;Password=test;MaxPoolSize = 100;MinPoolSize= 15”这样连接池 最大为100，最小为15

* 一个项目对数据库连接的管理能显著影响到整个应用程序的伸缩性和健壮性，影响到程序的性能指标。数据库连接池正是针对这个问题提出来的。数据库连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用一个现有的数据库连接，而再不是重新建立一个；释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏。这项技术能明显提高对数据库操作的性能。
* 数据库连接池在初始化时将创建一定数量的数据库连接放到连接池中，这些数据库连接的数量是由最小数据库连接数来设定的。无论这些数据库连接是否被使用，连接池都将一直保证至少拥有这么多的连接数量。连接池的最大数据库连接数量限定了这个连接池能占有的最大连接数，当应用程序向连接池请求的连接数超过最大连接数量时，这些请求将被加入到等待队列中。

**_数据库连接池的最小连接数和最大连接数的设置要考虑到下列几个因素：_**
1. 最小连接数是连接池一直保持的数据库连接，所以如果应用程序对数据库连接的使用量不大，将会有大量的数据库连接资源被浪费；
2. 最大连接数是连接池能申请的最大连接数，如果数据库连接请求超过此数，后面的数据库连接请求将被加入到等待队列中，这会影响之后的数据库操作。
3. 如果最小连接数与最大连接数相差太大，那么最先的连接请求将会获利，之后超过最小连接数量的连接请求等价于建立一个新的数据库连接。不过，这些大于最小连接数的数据库连接在使用完不会马上被释放，它将被放到连接池中等待重复使用或是空闲超时后被释放。

**15.为什么要用 ORM? 和 JDBC 有何不一样?**

ORM的作用是在关系型数据库和对象之间作一个映射，这样，我们在具体的操作数据库的时候，就不需要再去和复杂的SQL语句打交道，只要像平时操作对象一样操作它就可以了 。
1. 代码复杂性：
* 用JDBC的API访问数据库，代码量较大，特别是访问字段较多的表的时候，代码显得繁琐，容易出错，程序员需要耗费大量的时间、精力去编写具体的数据库访问的SQL语句，还要十分小心其中大量重复的源代码是否有疏漏，并不能集中精力于业务逻辑开发上面。 
* ORM则建立了Java对象与数据库对象之间的影射关系，程序员不需要编写复杂的SQL语句，直接操作Java对象即可，从而大大降低了代码量，也使程序员更加专注于业务逻辑的实现。
2. 数据库对象连接问题
* 采用JDBC编程，必须保证各种关系数据之间不能出错，极其痛苦； ORM建立Java对象与数据库对象关系影射的同时，也自动根据数据库对象之间的关系创建Java对象的关系，并且提供了维持这些关系完整、有效的机制。
3. 系统架构问题
* 使用JDBC编程时，程序员必须知道用什么数据库、有哪些表、各个表字段、各个字段的类型是什么、表与表之间什么关系、创建了什么索引等等。 
* 使用ORM技术，可以将数据库层完全隐蔽，程序员只需要根据业务逻辑的需要调用Java对象的Getter和 Setter方法，即可实现对后台数据库的操作。把ORM搭建好后，把Java对象交给程序员去实现业务逻辑，使数据访问层与数据库层清晰分界。
4. 性能问题
* 假设要向数据库写入1000条SQL语句，采用JDBC编程效率低下。
* 采用ORM技术，会自动延迟向后台数据库发送SQL请求，降低通讯量，提高运行效率，也可以根据实际情况，将数据库访问操作合成，尽量减少不必要的数据库操作请求。

### 网络部分

**1. http 响应码 301 和 302 代表的是什么？有什么区别？**

1. 对用户：
* 301，302对用户来说没有区别，他们看到效果只是一个跳转，浏览器中旧的URL变成了新的URL。页面跳到了这个新的url指向的地方。
2. 对开发人员：
* 当网页A用301重定向转到网页B时，搜索引擎可以肯定网页A永久的改变位置，或者说实际上不存在了，搜索引擎就会把网页B当作唯一有效目标。
* 302转向可能会有URL规范化及网址劫持的问题。可能被搜索引擎判为可疑转向，甚至认为是作弊。

**_302重定向和网址劫持（URL hijacking）有什么关系呢？_**
* 这要从搜索引擎如何处理302转向说起。从定义来说，从网址A做一个302重定向到网址B时，意思是网址A随时有可能改主意，重新显示本身的内容或转向其他的地方。大部分的搜索引擎在大部分情况下，当收到302重定向时，一般只要去抓取目标网址就可以了，也就是说网址B。
* 实际上如果搜索引擎在遇到302转向时，百分之百的都抓取目标网址B的话，就不用担心网址URL劫持了。问题就在于，有的时候搜索引擎，并不能总是抓取目标网址。比如说Google，有的时候A网址很短，但是它做了一个302重定向到B网址，而B网址是一个很长的乱七八糟的URL网址，甚至还有可能包含一些问号之类的参数。很自然的，A网址更加用户友好，而B网址既难看，又不用户友好。这时Google很有可能会仍然显示网址A。这就造成了网址URL劫持的可能性。也就是说，一个不道德的人在他自己的网址A做一个302重定向到你的网址B，出于某种原因， Google搜索结果所显示的仍然是网址A，但是所用的网页内容却是你的网址B上的内容，这种情况就叫做网址URL劫持。

**2. 简述 tcp 和 udp的区别？**



**3. tcp 为什么要三次握手，两次不行吗？为什么？**



**4. 说一下 tcp 粘包是怎么产生的？**



**5. OSI 的七层模型都有哪些？**



**6. get 和 post 请求有哪些区别？**



**7. 如何实现跨域？**



### Spring部分

**1. 为什么要使用 spring？**

**2. 解释一下什么是 aop？**

**3. 解释一下什么是 ioc？**

**4. spring 有哪些主要模块？**

**5. spring 常用的注入方式有哪些？**

**6. spring 中的 bean 是线程安全的吗？**

**7. spring 支持几种 bean 的作用域？**

**8. spring 自动装配 bean 有哪些方式？**

**9. spring 事务实现方式有哪些？**

**10. 说一下 spring 的事务隔离？**

**11. 说一下 spring mvc 运行流程？**

**12. spring mvc 有哪些组件？**

**13. @RequestMapping 的作用是什么？**

**14. @Autowired 的作用是什么？**

### Hibernate部分

**1. hibernate 中如何在控制台查看打印的 sql 语句？**

**2. hibernate 有几种查询方式？**

**3. hibernate 实体类可以被定义为 final 吗？**

**4. 在 hibernate 中使用 Integer 和 int 做映射有什么区别？**

**5. hibernate 是如何工作的？**

**6. get()和 load()的区别？**

**7. 说一下 hibernate 的缓存机制？**

**8. hibernate 对象有哪些状态？**

**9. 在 hibernate 中 getCurrentSession 和 openSession 的区别是什么？**

**10. hibernate 实体类必须要有无参构造函数吗？为什么？**

###  MyBatis部分

**1. mybatis 中 #{}和 ${}的区别是什么？**

**2. mybatis 有几种分页方式？**

**3. RowBounds 是一次性查询全部结果吗？为什么？**

**4. mybatis 逻辑分页和物理分页的区别是什么？**

**5. mybatis 是否支持延迟加载？延迟加载的原理是什么？**

**6. 说一下 mybatis 的一级缓存和二级缓存？**

**7. mybatis 和 hibernate 的区别有哪些？**

**8. mybatis 有哪些执行器（Executor）？**

**9. mybatis 分页插件的实现原理是什么？**

**10. mybatis 如何编写一个自定义插件？**


### ZooKeeper部分

**1. zookeeper 是什么？**

**2. zookeeper 都有哪些功能？**

**3. zookeeper 有几种部署模式？**

**4. zookeeper 怎么保证主从节点的状态同步？**

**5. 集群中为什么要有主节点？**

**6. 集群中有 3 台服务器，其中一个节点宕机，这个时候 zookeeper 还可以使用吗？**

**7. 说一下 zookeeper 的通知机制？**


