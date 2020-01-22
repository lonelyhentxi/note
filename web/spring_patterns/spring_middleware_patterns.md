# Spring Middleware Patterns

[参考](https://segmentfault.com/a/1190000021823564)

## Filter

在 `Servlet 容器` 层面，实现上基于函数回调，对请求进行过滤

- 无路径无顺序
    - 使用：`@Componenet`
    - 说明：实现 `Filter` 接口，并覆盖 `doFilter` 方法即可
- 有路径无顺序
    - 使用：`@WebFilter` + `@ServletComponentScan`
    - 说明：通过设置注解设置过滤器的匹配路径
- 有路径有顺序
    - 使用：`@Configuration`
    - 说明：完全通过配置类实现，不通过注解注入 IOC 容器，只通过配置类进行配置

## Listener

在 `Servlet 容器` 层面，监听 Web 应用中的某些对象和信息的生命周期动作，并作出相应。

根据监听对象，将监听器分为三类：

- `ServletContext`：
    - 生命周期：对应 `application`，实现接口 `ServletContextListener`，在整个服务中只有一个
    - 应用：在 Web 服务关闭时销毁。可用于数据缓存等
- `HttpSession`：
    - 生命周期：对应 `session`，实现接口 `HttpSessionListener`，在会话起始时创建，会话被一段关闭后销毁
    - 应用：可用作获取在线用户数
- `ServletRequest`：
    - 生命周期：对应 `request`，实现接口 `ServletRequestListener`，在客户发送请求时创建，用于封装请求数据，请求处理完毕后销毁。
    - 应用：可用作封装用户信息

## Interceptor

依赖于 Spring 框架，基于 Java 的动态代理，使用方式如下：

1. 声明 `interceptor` 的类：通过实现 `HandlerIncerptor` 接口和对应 `preHandle`，`postHandle`，`afterCompletion` 方法
2. 通过配置类配置拦截器：通过实现 `WebMvcConfigurer` 接口并实现对应 `addInterceptors` 方法

## AOP

AOP 需要单独引入 jar 包，操作通过注解实现即可：


- `@Aspect`：将一个 java 类定义为切面类。
- `@Pointcut`：定义一个切入点，可以是一个规则表达式，比如下例中某个 package 下的所有函数，也可以是一个注解等。
- `@Before`：在切入点开始处切入内容。
- `@After`：在切入点结尾处切入内容。
- `@AfterReturning`：在切入点 return 内容之后处理逻辑。
- `@Around`：在切入点前后切入内容，并自己控制何时执行切入点自身的内容。原则上可以替代 `@Before` 和 `@After`。
- `@AfterThrowing`：用来处理当切入内容部分抛出异常之后的处理逻辑。
- `@Order(100)`：AOP 切面执行顺序， `@Before` 数值越小越先执行，`@After` 和 `@AfterReturning` 数值越大越先执行。
