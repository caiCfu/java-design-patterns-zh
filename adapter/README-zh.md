---
134 / 5,000
翻译结果
布局：图案
标题：适配器
文件夹：适配器
永久链接：/模式/适配器/
类别：结构
语言：en
标签：
- 四人帮
---

## 也被称为
包装器

## 意图
将一个类的接口转换为客户期望的另一个接口。 适配器让类一起工作
由于接口不兼容，否则不能。

## 解释

真实世界的例子

> 考虑到您的存储卡上有一些图片，您需要将它们传输到您的计算机上。 要传输它们，您需要某种与您的计算机端口兼容的适配器，以便您可以将存储卡连接到您的计算机。 在这种情况下，读卡器是一个适配器。
> 另一个例子是著名的电源适配器； 三脚插头不能连接到两脚插座，它需要使用与两脚插座兼容的电源适配器。
> 还有一个例子是翻译者将一个人说的话翻译成另一个人

简单来说

> 适配器模式允许您在适配器中包装一个原本不兼容的对象，以使其与另一个类兼容。

维基百科说

> 在软件工程中，适配器模式是一种软件设计模式，它允许将现有类的接口用作另一个接口。 它通常用于使现有类在不修改其源代码的情况下与其他类一起使用。

**程序示例**

考虑一个只能使用划艇而根本不能航行的船长。

首先，我们有接口 `Rowing Boat` 和 `Fishing Boat`

```java
public interface RowingBoat {
  void row();
}

@Slf4j
public class FishingBoat {
  public void sail() {
    LOGGER.info("The fishing boat is sailing");
  }
}
```

船长希望 `RowingBoat` 接口的实现能够移动

```java
public class Captain {

  private final RowingBoat rowingBoat;
  // default constructor and setter for rowingBoat
  public Captain(RowingBoat rowingBoat) {
    this.rowingBoat = rowingBoat;
  }

  public void row() {
    rowingBoat.row();
  }
}
```

现在假设海盗要来了，我们的船长需要逃跑，但只有一艘渔船可用。 我们需要创建一个适配器，允许船长使用他的划艇技能操作渔船。

```java
@Slf4j
public class FishingBoatAdapter implements RowingBoat {

  private final FishingBoat boat;

  public FishingBoatAdapter() {
    boat = new FishingBoat();
  }

  @Override
  public void row() {
    boat.sail();
  }
}
```

而现在“船长”可以使用“渔船”来躲避海盗了。

```java
var captain = new Captain(new FishingBoatAdapter());
captain.row();
```

## 类图
![alt text](./etc/adapter.urm.png "Adapter class diagram")

## 适用性
使用适配器模式时

* 你想使用一个现有的类，它的接口与你需要的不匹配
* 你想创建一个与不相关或不可预见的类合作的可重用类，即不一定具有兼容接口的类
* 你需要使用几个现有的子类，但是通过子类化每个人来适应它们的接口是不切实际的。 对象适配器可以适配其父类的接口。
* 大多数使用第三方库的应用程序都使用适配器作为应用程序和第三方库之间的中间层，将应用程序与库解耦。 如果必须使用另一个库，则只需要新库的适配器，而无需更改应用程序代码。

## 教程

* [Dzone](https://dzone.com/articles/adapter-design-pattern-in-java)
* [Refactoring Guru](https://refactoring.guru/design-patterns/adapter/java/example)
* [Baeldung](https://www.baeldung.com/java-adapter-pattern)

## 结果
类和对象适配器有不同的权衡。 类适配器

* 通过提交具体的 Adaptee 类使 Adaptee 适应 Target。 因此，当我们想要适配一个类及其所有子类时，类适配器将无法工作。
* 让适配器重写一些 Adaptee 的行为，因为 Adapter 是 Adaptee 的子类。
* 只引入一个对象，不需要额外的指针间接到达适配者。

对象适配器

* 让单个适配器与许多 Adaptee 一起工作——即 Adaptee 本身及其所有子类（如果有）。 适配器还可以一次向所有 Adaptee 添加功能。
* 更难覆盖 Adaptee 的行为。 它将需要对 Adaptee 进行子类化，并使 Adapter 引用子类而不是 Adaptee 本身。


## 现实世界的例子

* [java.util.Arrays#asList()](http://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#asList%28T...%29)
* [java.util.Collections#list()](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#list-java.util.Enumeration-)
* [java.util.Collections#enumeration()](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#enumeration-java.util.Collection-)
* [javax.xml.bind.annotation.adapters.XMLAdapter](http://docs.oracle.com/javase/8/docs/api/javax/xml/bind/annotation/adapters/XmlAdapter.html#marshal-BoundType-)


## 学分

* [Design Patterns: Elements of Reusable Object-Oriented Software](https://www.amazon.com/gp/product/0201633612/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0201633612&linkCode=as2&tag=javadesignpat-20&linkId=675d49790ce11db99d90bde47f1aeb59)
* [J2EE Design Patterns](https://www.amazon.com/gp/product/0596004273/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0596004273&linkCode=as2&tag=javadesignpat-20&linkId=48d37c67fb3d845b802fa9b619ad8f31)
* [Head First Design Patterns: A Brain-Friendly Guide](https://www.amazon.com/gp/product/0596007124/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0596007124&linkCode=as2&tag=javadesignpat-20&linkId=6b8b6eea86021af6c8e3cd3fc382cb5b)
* [Refactoring to Patterns](https://www.amazon.com/gp/product/0321213351/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321213351&linkCode=as2&tag=javadesignpat-20&linkId=2a76fcb387234bc71b1c61150b3cc3a7)
