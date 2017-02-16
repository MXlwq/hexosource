---
title: 设计模式(Java)_1
date: 2016-05-19 11:16:54
tags:
- 设计模式
- Java
categories: 设计模式
---

> 20160725更新
> 下一篇：[设计模式(Java)_2](http://liwenquan.top/2016/05/20/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-2/)

# 设计模式Design Patterns
> 可复用面向对象软件的基础

总体来说设计模式分为三大类：

* 创建型模式，共五种：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。

* 结构型模式，共七种：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。

* 行为型模式，共十一种：策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。

![](http://dl.iteye.com/upload/attachment/0083/1179/57a92d42-4d84-3aa9-a8b9-63a0b02c2c36.jpg)

## 什么是设计模式
1. 设计模式是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性。
代码规范
1. 首先是要注意注释文档的格式，注释文档将用来生成HTML格式的代码报告，所以注释文档必须书写在类、域、构造函数、方法、定义之前。
2. 注释文档由两部分组成——描述、块标记。描述部分用来书写类的作用或者相关信息，块标记部分必 
3. 注释的种类：文件头注释、构造函数注释、域注释、方法注释和定义注释。文件头注释需要注明该文件的创建时间、文件名、命名空间信息。构造函数注释采用描述部分注明构造函数的作用。方法注释采用描述部分注明方法的功能，块标记注明方法的参数、返回值、异常等信息。

<!--more-->
面向对象：
面向对象OO = 面向对象的分析OOA  + 面向对象的设计OOD  + 面向对象的编程OOP

接口：
接口好比一种模板，这种模板定义了对象必须实现的方法，其目的就是让这些方法可以作为接口实例被引用。接口不能被实例化。类可以实现多个接口并且通过这些实现的接口被索引。接口变量只能索引实现该接口的类的实例。

接口和抽象的区别：

1. abstract class 在Java语音中表示的是一种继承关系，一个类只能使用一次继承关系。但是一个类却可以实现多个interface.
2. 在abstract class 中可以有自己的数据成员，也可以有非abstract的成员方法，而在interface中，只能够有静态的不能被修改的数据成员（也就是必须是static final的，不过在interface中一般不定义数据成员），所有的成员方法都是abstract的。
3. Abstract class和interface所反映出的设计理念不同。其实abstract classs表示的是"is---a"关系，interface表示的是“like---a”关系。
4. 实现抽象类和接口的类必须实现其中的所有方法。抽象类中可以有非抽象方法。接口中则不能有实现方法。
5. 接口中定义的变量默认是public static final型，且必须给其初值，所以实现类中不能重新定义，也不能改变其值。
6. 抽象类中的变量默认是friendly型，其值可以再子类中重新定义，也可以重新赋值。
7. 接口中的方法默认都是public,abstract类型的。

接口和委托的区别：
接口可以包含属性、索引、方法以及事件。但委托不能包含事件。

# 创建型模式
FactoryMethod ( 工厂方法 ) 
AbstractFactory ( 抽象工厂 ) 
Singleton ( 单态模式 ) 
Builder ( 建造者模式 ) 
Prototype ( 原型模式 ) 
## 工厂方法

定义一个用于创建对象的接口，让子类决定实例化哪一个类。FactoryMethod使一个类的实例化延迟到其子类。
适用性

1. 当一个类不知道它所必须创建的对象的类的时候。
2. 当一个类希望由它的子类来指定它所创建的对象的时候。
3. 当想将创建对象的职责委托给多个帮助子类中的某一个，并且希望将哪一个帮助子类是代理者这一信息局部化的时候。		
 
1. Product定义工厂方法所创建的对象的接口。
2. ConcreteProduct实现Product接口。
3. Creator声明工厂方法，该方法返回一个Product类型的对象*Creator也可以定义一个工厂方法的缺省实现，它返回一个缺省的ConcreteProduct对象。可以调用工厂方法以创建一个Product对象。
4.ConcreteCreator重定义工厂方法以返回一个ConcreteProduct实例。

例子
product 
```java
public interface Work {

    void doWork();
}
```
ConcreteProduct 
```java
public class StudentWork implements Work {

    public void doWork() {
        System.out.println("学生做作业!");
    }

}

public class TeacherWork implements Work {

    public void doWork() {
        System.out.println("老师审批作业!");
    }

}
```
Creator 
```java
public interface IWorkFactory {

    Work getWork();
}
```
ConcreteCreator 
```java
public class StudentWorkFactory implements IWorkFactory {

    public Work getWork() {
        return new StudentWork();
    }

}

public class TeacherWorkFactory implements IWorkFactory {

    public Work getWork() {
        return new TeacherWork();
    }

}
```
Test 
```java
public class Test {

    public static void main(String[] args) {
        IWorkFactory studentWorkFactory = new StudentWorkFactory();
        studentWorkFactory.getWork().doWork();
        
        IWorkFactory teacherWorkFactory = new TeacherWorkFactory();
        teacherWorkFactory.getWork().doWork();
    }

}
```
result 

学生做作业!
老师审批作业!
## 抽象工厂
 

    提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。
适用性

    1.一个系统要独立于它的产品的创建、组合和表示时。

    2.一个系统要由多个产品系列中的一个来配置时。

    3.当你要强调一系列相关的产品对象的设计以便进行联合使用时。

    4.当你提供一个产品类库，而只想显示它们的接口而不是实现时。
			
参与者

    1.AbstractFactory
      声明一个创建抽象产品对象的操作接口。

    2.ConcreteFactory
      实现创建具体产品对象的操作。

    3.AbstractProduct
      为一类产品对象声明一个接口。

    4.ConcreteProduct
      定义一个将被相应的具体工厂创建的产品*象。
      实现AbstractProduct接口。

    5.Client
      仅使用由AbstractFactory和AbstractProduct类声明的接口
类图
 
例子
AbstractFactory 
```java
public interface IAnimalFactory {

    ICat createCat();
	
    IDog createDog();
}
```
ConcreteFactory 
```java
public class BlackAnimalFactory implements IAnimalFactory {

    public ICat createCat() {
        reture new BlackCat();
    }

    public IDog createDog() {
        return new BlackDog();
    }
}

public class WhiteAnimalFactory implements IAnimalFactory {

    public ICat createCat() {
        return new WhiteCat();
    }

    public IDog createDog() {
        return new WhiteDog();
    }

}
```
AbstractProduct 
```java
public interface ICat {

    void eat();
}

public interface IDog {

    void eat();
}
```
Concreteproduct 
```java
public class BlackCat implements ICat {

    public void eat() {
        System.out.println("The black cat is eating!");
    }

}

public class WhiteCat implements ICat {

    public void eat() {
        System.out.println("The white cat is eating!");
    }

}

public class BlackDog implements IDog {

    public void eat() {
        System.out.println("The black dog is eating");
    }

}

public class WhiteDog implements IDog {

    public void eat() {
        System.out.println("The white dog is eating!");
    }

}
```
Client 
```java
public static void main(String[] args) {
    IAnimalFactory blackAnimalFactory = new BlackAnimalFactory();
    ICat blackCat = blackAnimalFactory.createCat();
    blackCat.eat();
    IDog blackDog = blackAnimalFactory.createDog();
    blackDog.eat();
    
    IAnimalFactory whiteAnimalFactory = new WhiteAnimalFactory();
    ICat whiteCat = whiteAnimalFactory.createCat();
    whiteCat.eat();
    IDog whiteDog = whiteAnimalFactory.createDog();
    whiteDog.eat();
}
```
result 

The black cat is eating!
The black dog is eating!
The white cat is eating!
The white dog is eating!
## 建造者模式
 
    将一个复杂对象的构成与它的表示分离，使同样的构建过程可以创建不同的表示。
适用性

    1.当创建复杂对象的算法应该独立于该对象的组成部分以及它们的装配方式时。

    2.当构造过程必须允许被构造的对象有不同的表示时。
			
参与者

    1.Builder
      为创建一个Product对象的各个部件指定抽象接口。

    2.ConcreteBuilder
      实现Builder的接口以构造和装配该产品的各个部件。
      定义并明确它所创建的表示.
      提供一个检索产品的接口。

    3.Director
      构造一个使用Builder接口的对象。

    4.Product
      表示被构造的复杂对象。ConcreteBuilder创建该产品的内部表示并定义它的装配过程。
      包含定义组成部件的类，包括将这些部件装配成最终产品的接口。
类图
 
 
例子
Builder 
```java
public interface PersonBuilder {

    void buildHead();
    
    void buildBody();
    
    void buildFoot();

    Person buildPerson();
}
```
ConcreteBuilder 
```java
public class ManBuilder implements PersonBuilder {

    Person person;
    
    public ManBuilder() {
        person = new Man();
    }
    
    publoc void buildBody() {
        person.setBody("建造男人的身体");
    }

    public void buildFoot() {
        person.setFoot("建造男人的脚");
    }

    public void buildHead() {
        person.setHead("建造男人的头");
    }

    public Person buildPerson() {
        reture person;
    }
}
```
Director 
```java
public class PersonDirector {

    public Person constructPerson(PersonBuilder pb) {
        pb.buildHead();
        pb.buildBody();
        pb.buildFoot();
        return pb.buildPerson();
    }
}
```
Product 
```java
public class Person {

    private String head;
    
    private String body;
    
    private String foot;

    public String getHead() {
        return head;
    }

    public void setHead(String head) {
        this.head = head;
    }

    public String getBody() {
        return body;
    }

    public void setBody(String body) {
        this.body = body;
    }

    public String getFoot() {
        return foot;
    }

    public void setFoot(String foot) {
        this.foot = foot;
    }
}

public class Man extends Person {

}
```
Test 
```java
public class Test{
    
    public static void main(String[] args) {
        PersonDirector pd = new PersonDirector();
        Person person = pd.constructPerson(new ManBuilder());
        System.out.println(person.getBody());
        System.out.println(person.getFoot());
        System.out.println(person.getHead());
    }
}
```
result 

建造男人的身体
建造男人的脚
建造男人的头
## 单态模式
 
定义：对象只要利用自己的属性完成了自己的任务，那该对象就是承担了责任，除了维持了自身的一致性，该对象无需承担其他任何责。如果该对象还承担着其他责任，而其他对象又依赖于该特定对象所承担的责任，我们就需要得到该特定对象。
适用性

    1.当类只能有一个单例而且客户可以从一个众所周知的访问点访问它时。

2.当这个唯一实例应该是通过子类化可扩展的，并且客户应该无需更改代码就能使用一个扩展的实例时。
3. 将类的责任集中到唯一的单体对象中，确保该类只有一个实例，并且为该类提供一个全局访问点，这就是单体模式的目的。
			
参与者

    Singleton
      定义一个Instance操作，允许客户访问它的唯一实例。Instance是一个类操作。
      可能负责创建它自己的唯一实例。
分类：
1.饿汉式单体类：类被加载的时候将自己初始化，更加安全些。
	private static XConfigReader instance = new  XConfigReader();
	private void XConfigReader(){}
	private static XConfigReader getInstance(){
		return instance;
	}
2.懒汉式单体类：在第一次被引用的时候将自己初始化。提高了效率。
```java
private static XConfigReader instance = null;
private XConfigReader(){
		SAXReader reader = new SAXReader();
		InputStream in = Thread.currentThread().getContextClassLoader().getResourceAsStream("sys-config.xml");
		try {
			
			Document doc = reader.read(in);
			
			//取得jdbcConfig相关配置
			Element driverNameElt = (Element) doc.selectObject("/config/db-info/driver-name");
			Element urlElt = (Element) doc.selectObject("/config/db-info/url");
			Element userNameElt = (Element) doc.selectObject("/config/db-info/user-name");
			Element passwordElt = (Element) doc.selectObject("/config/db-info/password");
			
			//设置jdbcConfig相关配置
			jdbcConfig.setDriverName(driverNameElt.getStringValue());
			jdbcConfig.setUrl(urlElt.getStringValue());
			jdbcConfig.setUserName(userNameElt.getStringValue());
			jdbcConfig.setPassword(passwordElt.getStringValue());
			System.out.println("读取jdbcConfig--------------------》》"+jdbcConfig);
		} catch (DocumentException e) {
			e.printStackTrace();
		}
	}
	public static synchronized XConfigReader getInstance(){
		if(instance == null){
			instance = new XConfigReader();
		}{
		return instance;
	}
}
```
3.多例模式：除了可以提供多个实例，其他和单体没有区别，包括有上限多例模式和没有上限的多例模式。
类图
  

 例子
Singleton 
```java
public class Singleton {
    
    private static Singleton sing;

    private Singleton() {
        
    }
    
    public static Singleton getInstance() {
        if (sing == null) {
            sing = new Singleton();
        }
        return sing;
    }
}
```
Test 
```java
public class Test {
    
    public static void main(String[] args) {
        Singleton sing = Singleton.getInstance();
        Singleton sing2 = Singleton.getInstance();
        
        System.out.println(sing);
        System.out.println(sing2);
    }
}
```
result 

singleton.Singleton@1c78e57
singleton.Singleton@1c78e57
## 原型模式
 

用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。
 
适用性

    1.当一个系统应该独立于它的产品创建、构成和表示时。

    2.当要实例化的类是在运行时刻指定时，例如，通过动态装载。

    3.为了避免创建一个与产品类层次平行的工厂类层次时。

    4.当一个类的实例只能有几个不同状态组合中的一种时。

    建立相应数目的原型并克隆它们可能比每次用合适的状态手工实例化该类更方便一些。			
参与者

    1. Prototype
       声明一个克隆自身的接口。

    2. ConcretePrototype
       实现一个克隆自身的操作。

    3. Client
       让一个原型克隆自身从而创建一个新的对象。
原型模式优缺点：
优点：1. Prototype模式允许动态增加或减少产品类。由于创建产品类实例的方法是产批类内部具有的，因此增加新产品对整个结构没有影响。
      2. Prototype模式提供了简化的创建结构。工厂方法模式常常需要有一个与产品类等级结构相同的等级结构，而Prototype模式就不需要这样。
      3. Prototype模式具有给一个应用软件动态加载新功能的能力。由于Prototype的独立性高，可以很容易动态加载新功能而不影响老系统。
      4. 产品类不需要非得有任何事先确定的等级结构，因为Prototype模式适用于任何的等级结构。
缺点：每一个类必须配备一个克隆方法，而且这个克隆方法需要对类的功能进行通盘考虑，这对全新的类来说不是很难，但对已有的类进行改造时，不一定是件容易的事。
类图
 
例子
Prototype 
```java
public class Prototype implements Cloneable {

    private String name;
    
    public void setName(String name) {
        this.name = name;
    }
    
    public String getName() {
        return this.name;
    }

    public Object clone(){
        try {
            return super.clone();
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}
ConcretePrototype 

public class ConcretePrototype extends Prototype {

    public ConcretePrototype(String name) {
        setName(name);
    }
}
```
Client 
```java
public class Test {

    public static void main(String[] args) {
        Prototype pro = new ConcretePrototype("prototype");
        Prototype pro2 = (Prototype)pro.clone();
        System.out.println(pro.getName());
        System.out.println(pro2.getName());
    }
}
```
result 

prototype
prototype



