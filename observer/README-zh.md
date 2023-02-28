---
布局：图案
标题：观察者
文件夹：观察者
永久链接：/模式/观察者/
类别：行为
语言：英文
标签：
- 四人帮
- 反应性
---

## 也被称为

家属，发布订阅

## 意图

定义对象之间的一对多依赖关系，这样当一个对象改变状态时，它的所有
家属会收到通知并自动更新。

＃＃ 解释

真实世界的例子

> 在遥远的土地上生活着霍比特人和兽人。 他们俩大部分时间都在户外，所以他们
> 密切关注天气变化。 可以说他们一直在观察
> 天气。

通俗地说

> 注册为观察者，接收对象的状态变化。

维基百科说

> 观察者模式是一种软件设计模式，其中一个称为主题的对象，
> 维护其依赖者列表，称为观察者，并自动通知他们任何状态
> 改变，通常是通过调用他们的方法之一。

**程序示例**

让我们首先介绍一下 `WeatherObserver` 界面和我们的种族，`Orcs` 和 `Hobbits`。

```java
public interface WeatherObserver {

  void update(WeatherType currentWeather);
}

@Slf4j
public class Orcs implements WeatherObserver {

  @Override
  public void update(WeatherType currentWeather) {
    LOGGER.info("The orcs are facing " + currentWeather.getDescription() + " weather now");
  }
}

@Slf4j
public class Hobbits implements WeatherObserver {

  @Override
  public void update(WeatherType currentWeather) {
    switch (currentWeather) {
      LOGGER.info("The hobbits are facing " + currentWeather.getDescription() + " weather now");
    }
  }
}
```

然后是不断变化的“天气”。

```java
@Slf4j
public class Weather {

  private WeatherType currentWeather;
  private final List<WeatherObserver> observers;

  public Weather() {
    observers = new ArrayList<>();
    currentWeather = WeatherType.SUNNY;
  }

  public void addObserver(WeatherObserver obs) {
    observers.add(obs);
  }

  public void removeObserver(WeatherObserver obs) {
    observers.remove(obs);
  }

  /**
   * Makes time pass for weather.
   */
  public void timePasses() {
    var enumValues = WeatherType.values();
    currentWeather = enumValues[(currentWeather.ordinal() + 1) % enumValues.length];
    LOGGER.info("The weather changed to {}.", currentWeather);
    notifyObservers();
  }

  private void notifyObservers() {
    for (var obs : observers) {
      obs.update(currentWeather);
    }
  }
}
```

这是实际的完整示例。

```java
    var weather = new Weather();
    weather.addObserver(new Orcs());
    weather.addObserver(new Hobbits());
    weather.timePasses();
    weather.timePasses();
    weather.timePasses();
    weather.timePasses();
```

程序输出：

```
The weather changed to rainy.
The orcs are facing rainy weather now
The hobbits are facing rainy weather now
The weather changed to windy.
The orcs are facing windy weather now
The hobbits are facing windy weather now
The weather changed to cold.
The orcs are facing cold weather now
The hobbits are facing cold weather now
The weather changed to sunny.
The orcs are facing sunny weather now
The hobbits are facing sunny weather now
```

## 类图

![alt text](./etc/observer.png "Observer")

## 适用性

在以下任一情况下使用观察者模式：

* 当一个抽象有两个方面时，一个依赖于另一个。 将这些方面封装在
  单独的对象让您可以独立地改变和重用它们。
* 当对一个对象的更改需要更改其他对象，而您不知道需要更改多少对象时
  被改变。
* 当一个对象应该能够通知其他对象而不假设这些对象是谁
  对象是。 换句话说，您不希望这些对象紧密耦合。

## 已知用途

* [java.util.Observer](http://docs.oracle.com/javase/8/docs/api/java/util/Observer.html)
* [java.util.EventListener](http://docs.oracle.com/javase/8/docs/api/java/util/EventListener.html)
* [javax.servlet.http.HttpSessionBindingListener](http://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpSessionBindingListener.html)
* [RxJava](https://github.com/ReactiveX/RxJava)

## 学分

* [Design Patterns: Elements of Reusable Object-Oriented Software](https://www.amazon.com/gp/product/0201633612/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0201633612&linkCode=as2&tag=javadesignpat-20&linkId=675d49790ce11db99d90bde47f1aeb59)
* [Java Generics and Collections](https://www.amazon.com/gp/product/0596527756/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0596527756&linkCode=as2&tag=javadesignpat-20&linkId=246e5e2c26fe1c3ada6a70b15afcb195)
* [Head First Design Patterns: A Brain-Friendly Guide](https://www.amazon.com/gp/product/0596007124/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0596007124&linkCode=as2&tag=javadesignpat-20&linkId=6b8b6eea86021af6c8e3cd3fc382cb5b)
* [Refactoring to Patterns](https://www.amazon.com/gp/product/0321213351/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321213351&linkCode=as2&tag=javadesignpat-20&linkId=2a76fcb387234bc71b1c61150b3cc3a7)
