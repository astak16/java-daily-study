## `Java Web`开发发展历史

`Servlet` -> `jsp`(`mvc`) == `asp` -> `Structs`(已经没人用了)-> `Spring`(`IOC`、`AOP`) -> `SpringMVC` -> `SpringBoot`(约定大于配置)

`Hibernate`(`ORM`) == `EntityFranework`(微软的)

`mvc`思想的流行是源于`java`社区

`asp`是微软开发的，最初是`VB`后来是`C#`

`IOC`是编程的原则，是控制反转；`AOP`是面向切面编程

`IOC`和`AOP`已经贯穿到`String`的生态中了，我们在使用`Spring`和`SpringBoot`时，其内在的实现已经很多是`IOC`和`AOP`了。

在`SpringBoot`中它的容器要注入一些实例对象时，就是一种`IOC`的思想；还有过滤器和拦截器，都是`AOP`的思想。

`ORM`对象关系映射，避免使用`SQL`语法操作数据库，它的发扬光大就是从`Hubernate`开始流行起来的。

`Structs`的缺点是由于`action`作为入口；`SpringMVC`是由方法作为入口的

使用`SpringMVC`更加容易实现`RESTful`风格的`API`

`SpringBoot`更多时候是多种技术或框架的一种整合

现在`java`开发已经很少提`ssm`了，更多是说`SpringBoot`

`ssh`：`Structs2`、`Spring`、`Hibernate`

`ssm`：`SpringMVC`、`Spring`、`MyBatis`

## `SpringBoot`与面向编程

`SpringBoot`主要做了两件事情：

- 简化`SpringMVC`配置
  - 配置是个好东西，满足软件开发的灵活性，`SpringBoot`会内置一些配置。这里的配置在动态语言里是不存在的
  - 配置包含两个层面
    - 常规配置：日志存在什么地方、一个请求的超时时间是多少秒
    - 静态语言独有的配置：`Interface`，主要是`java`提倡面向接口/抽象编程，而不是面向具体编程。比如有`3`个类都是实现了同一个`Interface`接口，那最终实例化的是哪个类呢？这个很多时候就要通过配置去实现。
  - 没有`SpringBoot`之前，配置都是在`XML`中配置的，使用`SpringBoot`会自动装配。
- 集成了大量的第三方库。
  
`java`追求的是稳定性和代码的可维护性

学习`java`，`IOC`是绕不过去的概念，还有一个依赖注入

`IOC`和依赖注入严格意义上来说不是同一个概念，但是他们之间是有关系的。

学习`java`不能用学习动态语言的方法和思维来学习



作业：
1. 什么是`IOC`和`DI`
2. 怎么用`IOC`和`DI`
3. 为什么要有`ICO`和`DI`，他们到底解决了什么问题

关于`IOC`和`DI`公认的好文章：[Inversion of Control Containers and the Dependency Injection pattern](martinfowler.com/articles/injection.html)

`DI`是`IOC`的一种具体的实现，或者说是`IOC`具体的用途，`IOC`是一种抽象，`DI`是更具体的一种东西。
