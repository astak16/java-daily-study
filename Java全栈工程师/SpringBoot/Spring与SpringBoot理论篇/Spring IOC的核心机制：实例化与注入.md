把对象注入到容器里大概有两方式
- `XML`
- 注解
  - 编程方式
  
## `stereotype annotations`
`stereotype annotations`：模式注解

`@Component`：组件/类/`bean`加入到`IOC`容器中去

这三个目前在功能上和`@Component`没有任何区别，更多的时候他们是用于表明一个类的作用。一个类如果没有明确的目的就用`@Component`。
`@Service`：是一种服务
`@Controller`：是一种控制器
`@Repository`：仓储，在访问数据库的时候，通常会使用

`@Configuration`：可以把一组类加入到容器中去。