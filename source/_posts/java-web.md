---
title: java_web
date: 2023-06-01 20:16:22
categories: 
 -java
---

___

<!-- more -->

# 一、JDBC

jdbc就是java操作关系型数据库的一套api

## 1.1 jdbc的api

DriverManager:注册驱动，获取数据库连接
connection:获取执行sql的对象,事务管理
statement：执行sql语句 excuteUPdate
ResultSet:封装了查询语句的结果
preparedstatement:预编译sql语句并执行，防止sql注入

# 二、实体类使用的数据类型

尽量使用integer 因为其默认为空而int默认为0

# 三、Mybatisa参数封装

1、接受参数select *from tb_brand where id=#{id}; 
#{}或${}者为参数占位符，括号内的id与相应接口参数保持一致

#{}会将该部分替换为？ 而${}不会，会存在sql注入
参数传递要用#{}

# 四、表单提交

form:
两个属性:
	action:指定表单提交的url
	*表单项要想被提交，必须要指定其name属性
	method:指定表单提交的方式
	1.get 默认
	*请求的参数会拼接在url的后面，而url有长度限制，即请求参数有限制
	2.post
	*请求参数会在http的请求体中，请求参数无限制

# 五、spring

scope="singleton" 只创建一个bean  应用加载，创建容器时创建
scope="prototype"创建多个bean     使用对象时候创建

# 六、依赖注入

IOC的具体实现：坐等框架将持久层（dao）对象传入给业务层（service）
如何注入：构造方法（有参构造），set方法

@Autowired注解是按照数据类型从spring容器中匹配的，所以不需要指定bean的id
@Qualifier("") 注解是按照bean的id从spring容器中匹配的，但是需要结合@Autowired一起使用
@Resource相当于@Autowired和@Qualifier("") 
@Configuration 标志该类是核心配置类
@ComponentScan("com.yanjie") 扫描该包下面的所有注解
@Bean("dataSource")//spring会将当前方法的返回值以指定名称存储到spring中；用来声明第三方的bean对象
@requestParam(name="name")String username
方法形参名与请求参数名不一致时，传递参数时将name映射到username上

当传递数组参数是，后端接受想要是list，则需要使用@requestParam
@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
传递时间参数时，来指定传递前端参数的格式

@RequestBody
用处一：作用在controller上，将方法返回值直接响应，若返回值位实体对象/集合，将会转换为json格式相应
用处二：可以将一个json格式的数据封装到一个实体对象中，传递json时使用

@ResponseBody  返回的数据不是html标签的页面，而是其他某种格式的数据时（如json、xml等）使用

@RestController=@Controller+@ResponseBody

@PathVariable Integer id 获取url中的路径的参数，@RequestMapping("/pathParam/{id}")

@Override是Java5的元数据，自动加上去的一个标志，告诉你说下面这个方法是从父类/接口 继承过来的，
需要你重写一次，这样就可以方便阅读,可以不写.

@controller 控制器（注入服务）
用于标注控制层，相当于struts中的action层

@service 服务（注入dao）
用于标注服务层，主要用来进行业务的逻辑处理

@repository（实现dao访问）
用于标注数据访问层，也可以说用于标注数据访问组件，即DAO组件.

@component （把普通pojo实例化到spring容器中，相当于配置文件中的 <bean id="" class=""/>）
泛指各种组件，就是说当我们的类不属于各种归类的时候（不属于@Controller、@Services等的时候），我们就可以使用@Component来标注这个类。
上面四个可使用value=“”指定bean的名字，例如@controller（value=“”）
没有指定名字，默认是类名首字母小写

@primary 当bean的名字一样时，加上该注解，提高优先级，让当前bean生效

@transational 将方法添加到spring事务管理中 默认只有runtime error异常回滚，
添加参数rollbackfor=true可回滚所有异常.

@Aspect 面向切面编程是，标识当前类为AOP类
@Around 指明哪些方法加入aop

lombok里面的有参构造和无参构造
@NoArgsConstructor
@RequiredArgsConstructor

@ControllerAdvice() aop思想的一种实现，你告诉我需要拦截规则，我帮你把他们拦下来，
具体你想做更细致的拦截筛选和拦截之后的处理，你自己通过@ExceptionHandler、@InitBinder 或 @ModelAttribute这三个注解以及被其注解的方法来自定义。

# 七、ajax （asynchronous (异步的) js and xml）

详情参考

[https://www.w3school.com.cn/js/js_ajax_intro.asp]: 

作用：
不刷新页面更新网页
在页面加载后从服务器请求数据
在页面加载后从服务器接收数据
在后台向服务器发送数据

步骤：
网页中发生一个事件（页面加载、按钮点击）
由 JavaScript 创建 XMLHttpRequest 对象
XMLHttpRequest 对象向 web 服务器发送请求
服务器处理该请求
服务器将响应发送回网页
由 JavaScript 读取响应
由 JavaScript 执行正确的动作（比如更新页面）

# 八、axios 原生ajax的封装，更加方便 

[]: https://www.axios-http.cn/

区别：
1.post请求更安全；post请求不会作为url的一部分，不会被缓存、保存在服务器日志、以及浏览器浏览记录中，get请求的是静态资源，则会缓存，如果是数据，则不会缓存。
2、post请求发送的数据更大，get请求有url长度限制。
3、post请求能发送更多的数据类型，get请求只能发送ASCII字符。
4、传参方式不同。
5、get产生一个TCP数据包；post产生两个。

# 九、MyBatis

@Mapper 使用在三层架构中的dao层（mybatis称为mapper层）
 在运行时，会自动生成该接口的实现类对象，并且将该对象交给IOC容器管理
