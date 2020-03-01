`SpringBoot`内部原理就是工厂原理加上反射组成的。

改动配置文件是否违反`OCP`？

**配置文件属于系统外部的，而不属于代码本身。**

`IOC`：控制反转
`DI`：依赖注入
`DIP`：依赖倒置

## `DIP`
`DIP`：依赖倒置，全称`Dependency Inversion Principle`
- 高层模块不应该依赖低层模块，两者都应该依赖抽象
- 抽象不应该起来细节
- 细节应该依赖抽象

怎么定义高层和底层，在软件世界中，高层是抽象，低层是具体的实现。

那什么是倒置？

在正常编写代码过程中，随手实例化了一个对象，`new`了一个对象，就是和具体的对象绑定在一起了，这就是依赖了具体没有依赖抽象。而现在反过来了，不在让代码依赖具体，而是让它依赖接口，这就是一个倒置。

依赖倒置的应用：这里`iSkill`没有依赖具体的类，是靠`HeroFactory`获得具体的英雄。
```java
ISkill iSkill = HeroFactory.getHero(name);
iSkill.r();
```

## `DI`

`DI`：依赖注入，全称`Dependency Injection`

我们谈面向对象，就是对象与对象之间相互作用，从而构建整个系统，对象与对象之间相互作用必定会产生依赖的，依赖是不可避免的。

关键是产生这个依赖是有多种多样的
- 最普通的就是用`new`生产一个对象，这就有一个具体实例化的过程
- 不在使用`new`生产一个对象，而是让容器把这个对象注入进来

依赖注入应用：属性注入，构造注入，接口注入
```java
// 属性注入
public class A {
  private IC ic;
  public void print() {
    this.ic.print();
  }
  public void setIc(IC ic) {
    this.ic = ic;
  }
}

// 构造注入
public class A {
  private IC ic;
  public A(IC ic) {
    this.ic = ic;
  }
  public void print() {
    this.ic.print();
  }
}
```

在`SpringBoot`里面，它的容器就是`factory`工厂模式，再加上反射机制实现的。

## `IOC`

`IOC`概念是非常抽象和模糊的，它只是讲述一种思想，没有具体的实现

具体的实现是`DI`，有些地方说是应用，有些又说是另一种描述。

`A`类里面`new C`，那这里主控类是哪个？`A`实例化了`C`，这个主控类就是`A`。

用了`DI`依赖注入后，有了容器后，主控方是`Container`。

之前控制过程都是`A`来做的，但是现在这个控制过程都交给了`Container`去做了，这就是控制反转。

[IOC终极奥义](https://class.imooc.com/lesson/1197#mid=29465)，第一次没有听懂。