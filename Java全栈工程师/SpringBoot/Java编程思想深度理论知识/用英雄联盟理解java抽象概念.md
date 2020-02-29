需要解决的问题是，每当需要新增一个类时，尽可能不去改变`Main`中的代码`，要让`Main`不去更改的话，就要保证他的稳定性。

相对稳定主要是要解决两个问题
- 统一方法调用
- 统一对象实例化

下面从四个版本逐步演变这个过程

## 第一版

`3`个英雄人物：

```java
public class Diana {
  public void q() { System.out.println("Diana Q"); }

  public void w() { System.out.println("Diana W"); }

  public void e() { System.out.println("Diana E"); }

  public void r() { System.out.println("Diana R");}
}

public class Irelia {
  public void q() { System.out.println("Irelia Q"); }

  public void w() { System.out.println("Irelia W"); }

  public void e() { System.out.println("Irelia E"); }

  public void r() { System.out.println("Irelia R"); }
}

public class Camille {
  public void q() { System.out.println("Camille Q"); }

  public void w() { System.out.println("Camille W"); }

  public void e() { System.out.println("Camille E"); }

  public void r() { System.out.println("Camille R"); }
}
```

主方法中根据用户输入选择相应的英雄人物

```java
public static void main(String[] args) {
  String name = getPlayerInput();
  switch(name){
    case "Diana":
      Diana diana = new Diana();
      diana.r();
      break;
    case "Irelia":
      Irelia irelia = new Irelia();
      irelia.r();
      break;
    case "Camille":
      Camille camille = new Camille();
      camille.r();
      break;
  }
}

private static String getPlayerInput() {
  System.out.println("Enter a Hero's Name");
  Scanner scanner = new Scanner(System.in);
  return scanner.nextLine();
}
```

这个方法的问题是，每次新增一个英雄，都要去改`Main`中的代码，不满足开闭原则的，但是这样写代码是我们最为习惯写代码的方式，我们在考虑问题的时候很自然的就把代码写成了这个样子了。

## 第二版：`interface`优化

声明一个接口：`ISkill`

```java
public interface ISkill {
  void q();

  void w();

  void e();

  void r();
}
```

让所有英雄都实现这个接口。

```java
public class Diana implements ISkill {
  public void q() { System.out.println("Diana Q"); }

  public void w() { System.out.println("Diana W"); }

  public void e() { System.out.println("Diana E"); }

  public void r() { System.out.println("Diana R");}
}

public class Irelia implements ISkill {
  public void q() { System.out.println("Irelia Q"); }

  public void w() { System.out.println("Irelia W"); }

  public void e() { System.out.println("Irelia E"); }

  public void r() { System.out.println("Irelia R"); }
}

public class Camille implements ISkill {
  public void q() { System.out.println("Camille Q"); }

  public void w() { System.out.println("Camille W"); }

  public void e() { System.out.println("Camille E"); }

  public void r() { System.out.println("Camille R"); }
}
```

`Main`方法中把英雄用`interface`统一起来，实现方法调用

```java
public static void main(String[] args) throws Exception {
  String name = getPlayerInput();
  ISkill iSkill;
  switch (name) {
    case "Diana":
      iSkill = new Diana();
      break;
    case "Irelia":
      iSkill = new Irelia();
      break;
    case "Camille":
      iSkill = new Camille();
      break;
    default:
      throw new Exception();
  }
  iSkill.r();
}

private static String getPlayerInput() {
  System.out.println("Enter a Hero's Name");
  Scanner scanner = new Scanner(System.in);
  return scanner.nextLine();
}
```

第二版代码和第一版相比，仅仅是少了几行代码，它既没有把`switch case`干掉，也没有解决开闭原则的问题。

即使用`interface`把英雄统一了起来，也无法回避需要新增`switch case`的缺点。

虽然`interface`无法直接解决开闭原则的问题，但是`interface`的优势和好处也是能体会出来的。

**单纯的`interface`在某种程度上可以统一方法的调用，但不能统一对象的实例化。**

面向对象主要做两件事情：
- 实例化对象
- 调用方法（完成业务逻辑）

在真实的项目中，方法的调用肯定非常多，在这种情况下把方法的调用统一起来，集中在一个接口的上，是很有意义的。

不管是设计模式，比如工厂方法、工程模式，还是`IOC`和`DI`都是在为了让`new`变的更加抽象，而不是很具体。

为什么`IOC`和`DI`没有去关注方法统一调用，是因为方法统一调用使用最基础的`interface`就可以解决了，比较难解决的是对象实例化。

如果要让一段代码比较稳定，就不能出现`new`这个关键字。

**只有一段代码中没有`new`的出现，才能保持代码的相对稳定，才能逐步实现`OCP`。**

**上面这句话只是表象，实质是一段代码如果要保持稳定，就不应该负责对象的实例化。**

**对象实例化是不可消除的。**

**把对象实例化的过程，转移到其他的代码片段里。**

## 第三版：简单工厂模式

和第二版不同的是，第三版需要增加一个工厂类，让这个工厂类来具体负责具体的实例化过程。

先增一个`factory`包，下面新增一个`HeroFactory`类，它主要是用来生产或者实例化这个英雄类的。

这个工厂方法主要是用来生产英雄的对象。

```java
public static ISkill getHero(String name) throws Exception {
  ISkill iSkill;
  switch (name) {
    case "Diana":
      iSkill = new Diana();
      break;
    case "Irelia":
      iSkill = new Irelia();
      break;
    case "Camille":
      iSkill = new Camille();
      break;
    default:
      throw new Exception();
  }
  return iSkill;
}
```

`Main`方法中调用`HeroFactory`来生产英雄对象。

```java
public static void main(String[] args) throws Exception {
  String name = getPlayerInput();
  ISkill iSkill = HeroFactory.getHero(name);
  iSkill.r();
}

private static String getPlayerInput() {
  System.out.println("Enter a Hero's Name");
  Scanner scanner = new Scanner(System.in);
  return scanner.nextLine();
}
```

未来在新增英雄的时候，`Main`方法的代码是很稳定的，不需要在改了。

所以对于`Main`方法来说，它就完成了`OCP`的原则。

虽然未来再次新增英雄是，`Main`方法中代码是不需要改动的，但是新增的`HeroFactory`中的代码还是要改动的，这是不是伪需求呢？

保持了一段代码的稳定性，引入了另外一个文件，让另外一个文件变得不稳定了。这个有意义吗？

**代码中总是会存在不稳定，隔离这些不稳定，保证其他的代码是稳定的。**

所以说这个稳定具有一定的相对性。

`IOC`其实就是把所有的不稳定，封装和隔离到一块，保证其他地方的代码都是稳定的。

负责生产对象只是`IOC`的一部分。

**不稳定的原因是，软件中存在着变化，变化造成了不稳定**

对于软件系统来讲，变化分为两大类：
- 用户的操作是不稳定
- 自身软件的业务需求造成的不稳定

隔离不稳定就是在隔离变化，把变化隔离在一块代码里，整个系统就会变得比较稳定。

为什么这里会有`switch case`，是因为用户只能输入字符串，我们需要根据用户的输入去创建对应的对象，如果用户能直接输入一个对象传给我们的程序，把我们的程序不是就没有`switch case`了。```

## 第四版

**计算机里的代码其实是现实世界的一些规律，或者是一些业务的映射。**

```java
//反射
public static ISkill getHero(String name) throws Exception {
  String classStr = "factory.hero." + name;
  Class<?> cla = Class.forName(classStr);   //元类，类是对象的抽象，元类是描述类的
  Object obj = cla.newInstance();
  return (ISkill)obj;
}
```

`JDK9`以上已经废弃了`newInstance()`这个方法，用的是`clazz.getDeclaredConstructor().newInstance()`来代替。

## 总结

- **单纯的`interface`在某种程度上可以统一方法的调用，但不能统一对象的实例化。**
- **面向对象主要做两件事情：实例化对象、调用方法（完成业务逻辑）**
**只有一段代码中没有`new`的出现，才能保持代码的相对稳定，才能逐步实现`OCP`。**
- **上面这句话只是表象，实质是一段代码如果要保持稳定，就不应该负责对象的实例化。**
- **对象实例化是不可消除的。**
- **把对象实例化的过程，转移到其他的代码片段里。**
- **代码中总是会存在不稳定，隔离这些不稳定，保证其他的代码是稳定的。**
- **不稳定的原因是，软件中存在着变化，变化造成了不稳定**

