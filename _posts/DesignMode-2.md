---
title: 设计模式(Java)_2
date: 2016-05-20 12:12:55
tags:
- 设计模式
- Java
categories: 设计模式
---

> 上一篇：[设计模式(Java)_1](http://liwenquan.top/2016/05/19/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/)
> 下一篇：[设计模式(Java)_3](http://liwenquan.top/2016/05/20/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-3/)

# 结构型模式
Adapter ( 适配器模式 ) 
Bridge ( 桥接模式 ) 
Composite ( 组合模式 ) 
Decorator ( 装饰模式 ) 
Facade ( 外观模式 ) 
Flyweight ( 享元模式 ) 
Proxy ( 代理模式 ) 
<!--more-->
## 适配器模式

将一个类的接口转换成客户希望的另外一个接口。Adapter模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。
举例：上次有个朋友从美国给我带回一个微波炉，但因为美国的生活用电电压是110v,而中国的电压是220v,所以我不能使用，幸好朋友有先见之明，给我带回一个变压器，能把220v电压转换为110v电压，我才可以放心使用。这回懂得什么是适配器模式了吧。
适用性
    1.你想使一个已经存在的类，而它的接口不符合你的需求。

    2.你想创建一个可以复用的类，该类可以与其他不相关的类或不可预见的类（即那个接口可能不一定兼容的类）协同工作。（仅适用于对象Adapter）你想使用一些已经存在的子类，但是不可能对每一个都进行
    3.子类化以匹配它们的接口。对象适配器可以适配它的父类接口。
    4. 设计模式里的适配器模式和日常生活中的适配器的作用是完全一样的。
			
参与者

    1.Target（目标）
      定义Client使用的与特定领域相关的接口。
    2.Client（客户）
      与符合Target接口的对象协同
    3.Adaptee(被适配者)
      定义一个已经存在的接口，这个接口需要适配。

    4.Adapter（适配器）
      对Adaptee的接口与Target接口进行适配
类图：
 
适配器的分类：类适配器和对象适配器
（1）类适配器是通过继承类适配者类实现的，另外类适配器实现客户类所需要的接口。
（2）对象适配器包含一个适配者的引用，与类适配器相同，对象适配器也实现了客户类需要的接口。
例子
Target 
```java
public interface Target {

    void adapteeMethod();
    
    void adapterMethod();
}
```
Adaptee 
```java
public class Adaptee {

    public void adapteeMethod() {
        Syste.out.ptintln("Adaptee method!");
    }
}
```
Adapter 
  对象适配器：
```java
public class Adapter implements Target {

//引入被适配者
private Adaptee adaptee;
    
    public Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

	//实现目标接口
        public void adapteeMethod() {
		adaptee.adapteeMethod();
	}

	public void adapterMethod() {
		System.out.println("Adapter method!");
    }
}

  //类适配器：
  Public class Adapter extends Adapteee implements Target {
 //实现目标接口
        public void adapteeMethod() {
		adaptee.adapteeMethod();
	}

	public void adapterMethod() {
		System.out.println("Adapter method!");
    }
}
```
  
Client 
```java
public class Test {

    public static void main(String[] args) {
        Target target = new Adapter(new Adaptee());
        target.adapteeMethod();
        
        target.adapterMethod();
    }
}
```
result 

Adaptee method!
Adapter method!
## 桥接模式

    将抽象部分与它实现部分分离，使它们都可以独立地变化。
适用性

    1.你不希望在抽象和它的实现部分之间有一个固定的绑定关系。
      例如这种情况可能是因为，在程序运行时刻实现部分应可以选择或者切换。

    2.类的抽象以及它的实现都应该可以通过生成子类的方法加以扩充。
      这时Bridge模式使你可以对不同的抽象接口和实现部分进行组合，并分别对它们进行扩充。

    3.对一个抽象的实现部分的修改应对客户不产生影响，即客户的代码不必重新编译。

    4.正如在意图一节的第一个类图中所示的那样，有许多类要生成。
      这是一种类层次结构说明你必须将一个对象分解成两个部分。

5.若想在多个对象间共享实现（可能使用引用计数），但同时要求客户并不知道这一点。
6.如果一个系统需要在构件的抽象化角色和具体化角色之间增加更多的灵活性，避免在两个层次建立静态的联系。
7.设计要求实现化角色的任何改变不应当影响客户端，或者说实现化角色的改变对客户端完全透明的。
8.一个构件有多于一个的抽象化角色和实现化角色，系统需要他们之间进行动态耦合。
优点
1. 分离接口及其实现部分，这里实现了Abstration和Implementor的分离，有助于降低对实现部分的依赖性，从而产生更好的结构化系统。
2.提高可扩充行，可以独立的对Abstraction和Implementor层次结构进行扩充。			
参与者

    1.Abstraction
      定义抽象类的接口。
      维护一个指向Implementor类型对象的指针。

    2.RefinedAbstraction
      扩充由Abstraction定义的接口。

    3.Implementor
      定义实现类的接口，该接口不一定要与Abstraction的接口完全一致。
      事实上这两个接口可以完全不同。
      一般来讲，Implementor接口仅提供基本操作，而Abstraction则定义了基于这些基本操作的较高层次的操作。

    4.ConcreteImplementor
      实现Implementor接口并定义它的具体实现。
类图
 
例子
Abstraction 
```java
public abstract class Person {

    private Clothing clothing;
    
    private String type;

    public Clothing getClothing() {
        return clothing;
    }

    public void setClothing() {
        this.clothing = ClothingFactory.getClothing();
    }
    
    public void setType(String type) {
        this.type = type;
    }
    
    public String getType() {
        return this.type;
    }
    
    public abstract void dress();
}
```
RefinedAbstraction 
```java
public class Man extends Person {
    
    public Man() {
        setType("男人");
    }
    
    public void dress() {
        Clothing clothing = getClothing();
        clothing.personDressCloth(this);
    }
}

public class Lady extends Person {

    public Lady() {
        setType("女人");
    }
    
    public void dress() {
        Clothing clothing = getClothing();
        clothing.personDressCloth(this);
    }
}
```
Implementor
```java
public abstract class Clothing {

    public abstract void personDressCloth(Person person);
}
```
ConcreteImplementor
```java
public class jacket extends Clothing {

    public void personDressCloth(Person person) {
        System.out.println(person.getType() + "穿马甲");
    }
}

public class Trouser extends Clothing {

    public void personDressCloth(Person person) {
        System.out.println(Person.getType() + "穿裤子");
    }
}
```
Test 
```java
public class Test {

    public static void main(String[] args) {
        
        Person man = new Man();
        
        Person lady = new Lady();
        
        Clothing jacket = new Jacket();
        
        Clothing trouser = new Trouser();
        
        jacket.personDressCloth(man);
        trouser.personDressCloth(man);

        jacket.personDressCloth(lady);
        trouser.personDressCloth(lady);
    }
}
```
result 

男人穿马甲
男人穿裤子
女人穿马甲
女人穿裤子
## 组合模式
 
将对象组合成树形结构以表示"部分-整体"的层次结构。"Composite使得用户对单个对象和组合对象的使用具有一致性。"
 
适用性

    1.你想表示对象的部分-整体层次结构。

    2.你希望用户忽略组合对象与单个对象的不同，用户将统一地使用组合结构中的所有对象。
			
参与者

    1.Component
      为组合中的对象声明接口。
      在适当的情况下，实现所有类共有接口的缺省行为。
      声明一个接口用于访问和管理Component的子组件。
      (可选)在递归结构中定义一个接口，用于访问一个父部件，并在合*的情况下实现它。

    2.Leaf
      在组合中表示叶节点对象，叶节点没有子节点。
      在组合中定义节点对象的行为。

    3.Composite
      定义有子部件的那些部件的行为。
      存储子部件。
      在Component接口中实现与子部件有*的操作。

    4.Client
      通过Component接口操纵组合部件的对象。
5. Disk
   Disk是组合体的一个对象，或称一个部件，这个部件是个单独元素
类图
 
例子
Component 
```java
public abstract class Employer {

    private String name;
    
    public void setName(String name) {
        this.name = name;
    }
    
    public String getName() {
        return this.name;
    }
    
    public abstract void add(Employer employer);
    
    public abstract void delete(Employer employer);
    
    public List employers;
    
    public void printInfo() {
        System.out.println(name);
    }
    
    public List getEmployers() {
        return this.employers;
    }
}
```
Leaf 
```java
public class Programmer extends Employer {

    public Programmer(String name) {
        setName(name);
        employers = null;//程序员, 表示没有下属了
    }

    public vid add(Employer employer) {
        
    }

    public void delete(Employer employer) {
        
    }
}

public class ProjectAssistant extends Employer {

    public ProjectAssistant(String name) {
        setName(name);
        employers = null;//项目助理, 表示没有下属了
    }

    public void add(Employer employer) {
        
    }

    public void delete(Employer employer) {
        
    }
}
```
Composite 
```java
public class ProjectManager extends Employer {
    
    public ProjectManager(String name) {
        setName(name);
        employers = new ArrayList();
    }
    
    public void add(Employer employer) {
        employers.add(employer);
    }

    public void delete(Employer employer) {
        employers.remove(employer);
    }
}
```
Client 
```java
public class Test {

    public static void main(String[] args) {
        Employer pm = new ProjectManager("项目经理");
        Employer pa = new ProjectAssistant("项目助理");
        Employer programmer1 = new Programmer("程序员一");
        Employer programmer2 = new Programmer("程序员二");
        
        pm.add(pa);//为项目经理添加项目助理
        pm.add(programmer2);//*项目经理*加程序员
        
        List ems = pm.getEmployers();
        for (Employer em : ems) {
            System.out.println(em.getName());
        }
    }
}
```
result 

项目助理
程序员二
## 装饰模式

    动态地给一个对象添加一些额外的职责。就增加功能来说，Decorator模式相比生成子类更为灵活。
适用性

    1.在不影响其他对象的情况下，以动态、透明的方式给单个对象添加职责。

    2.处理那些可以撤消的职责。

    3.当不能采用生成子类的方法进行扩充时。
			
参与者

    1.Component
      定义一个对象接口，可以给这些对象动态地添加职责。

    2.ConcreteComponent
      定义一个对象，可以给这个对象添加一些职责。

    3.Decorator
      维持一个指向Component对象的指针，并定义一个与Component接口一致的接口。

    4.ConcreteDecorator
      向组件添加职责。
类图
 
例子
Component 
```java
public interface Person {

    void eat();
}
```
ConcreteComponent 
```java
public class Man implements Person {

	public void eat() {
		System.out.println("男人在吃");
	}
}
```
Decorator 
```java
public abstract class Decorator implements Person {

    protected Person person;
    
    public void setPerson(Person person) {
        this.person = person;
    }
    
    public void eat() {
        person.eat();
    }
}
```
ConcreteDectrator 
```java
public class ManDecoratorA extends Decorator {

    public void eat() {
        super.eat();
        reEat();
        System.out.println("ManDecoratorA类");
    }

    public void reEat() {
        System.out.println("再吃一顿饭");
    }
}

public class ManDecoratorB extends Decorator {
    
    public void eat() {
        super.eat();
        System.out.println("===============");
        System.out.println("ManDecoratorB类");
    }
}
```
Test 
```java
public class Test {

    public static void main(String[] args) {
        Man man = new Man();
        ManDecoratorA md1 = new ManDecoratorA();
        ManDecoratorB md2 = new ManDecoratorB();
        
        md1.setPerson(man);
        md2.setPerson(md1);
        md2.eat();
    }
}
```
result 

男人在吃
再吃一顿饭
ManDecoratorA类
\===============
ManDecoratorB类
## 外观模式
    外观模式定义了一个将子系统的一组接口集成在一起的高层接口，以提供一个一致的界面。通过这个界面，其他系统可以方便的调用子系统中的功能，而忽略子系统内部发生的变化。
适用性

    1.当你要为一个复杂子系统提供一个简单接口时。子系统往往因为不断演化而变得越来越复杂。大多数模式使用时都会产生更多更小的类。这使得子系统更具可重用性，也更容易对子系统进行定制，但这也给那些不需要定制子系统的用户带来一些使用上的困难。Facade可以提供一个简单的缺省视图，这一视图对大多数用户来说已经足够，而那些需要更多的可定制性的用户可以越过facade层。

    2.客户程序与抽象类的实现部分之间存在着很大的依赖性。引入facade将这个子系统与客户以及其他的子系统分离，可以提高子系统的独立性和可移植性。

    3.当你需要构建一个层次结构的子系统时，使用facade模式定义子系统中每层的入口点。如果子系统之间是相互依赖的，你可以让它们仅通过facade进行通讯，从而简化了它们之间的依赖关系。
			
参与者

    1.Facade
      知道哪些子系统类负责处理请求。
      将客户的请求代理给适当的子系统对象。

    2.Subsystemclasses
      实现子系统的功能。
      处理由Facade对象指派的任务。
      没有facade的任何相关信息；即没有指向facade的指针。
 类图
 
注意事项：
（1）在设计外观时，不需要增加额外的功能。
（2）不要从外观方法中返回子系统中的组件给客户。
（3）应用外观的目的是提供一个高层次的接口，而不是进行底层次的单独的业务执行。
例子
Facade 
```java
public class Facade {

    ServiceA sa;
    
    ServiceB sb;
    
    ServiceC sc;
    
    public Facade() {
        sa = new ServiceAImpl();
        sb = new ServiceBImpl();
        sc = new ServiceCImpl(); 
    }
    
    public void methodA() {
        sa.methodA();
        sb.methodB();
    }
    
    public void methodB() {
        sa.methodB();
        sc.methodC();
    }
    
    public void methodC() {
        sc.methodC();
        sa.methodA();
    }
}
```
Subsystemclasses
```java
public class ServiceAImpl implements ServiceA {

    public void methodA() {
        System.out.println("这是服务A");
    }
}

public class ServiceBImpl implements ServiceB {

    public void methodB() {
        System.out.println("这是服务B");
    
}

public class ServiceCImpl implements ServiceC {

    public void methodC() {
        System.out.println("这是服务C");
    }
}
```
Test 
```java
public class Test {
    
    public static void main(String[] args) {
    	ServiceA sa = new ServiceAImpl();
    	ServiceB sb = new ServiceBImpl();
        
        sa.methodA();
        sb.methodB();
        
        System.out.println("========");
        //facade
        Facade facade = new Facade();
        facade.methodA();
        facade.methodB();
    }
}
```
result 

这是服务A
这是服务B
\========
这是服务A
这是服务B
这是服务B
这是服务C
## 享元模式
 
    定义：运用共享技术有效地支持大量细粒度的对象。
适用性

    当都具备下列情况时，使用Flyweight模式：

    1.一个应用程序使用了大量的对象。

    2.完全由于使用大量的对象，造成很大的存储开销。

    3.对象的大多数状态都可变为外部状态。

    4.如果删除对象的外部状态，那么可以用相对较少的共享对象取代很多组对象。

    5.应用程序不依赖于对象标识。由于Flyweight对象可以被共享，对于概念上明显有别的对象，标识测试将返回真值。
			
参与者

    1.Flyweight
      描述一个接口，通过这个接口flyweight可以接受并作用于外部状态。

    2.ConcreteFlyweight
      实现Flyweight接口，并为内部状态（如果有的话）增加存储空间。
      ConcreteFlyweight对象必须是可共享的。它所存储的状态必须是内部的；即，它必须独立于ConcreteFlyweight对象的场景。

    3.UnsharedConcreteFlyweight
      并非所有的Flyweight子类都需要被共享。Flyweight接口使共享成为可能，但它并不强制共享。
      在Flyweight对象结构的某些层次，UnsharedConcreteFlyweight对象通常将ConcreteFlyweight对象作为子节点。

    4.FlyweightFactory
      创建并管理flyweight对象。
      确保合理地共享flyweight。当用户请求一个flyweight时，FlyweightFactory对象提供一个已创建的实例或者创建一个（如果不存在的话）。
优缺点：
1. 享元模式使得系统更加复杂，为了使对象可以共享，需要将一些状态外部化，这使得程序的逻辑复杂化。
2. 享元模式将享元对象的状态外部化，而读取外部状态使得运行时间稍微变长。
类
 
 
例子
Flyweight 
```java
public interface Flyweight {

    void action(int arg);
}
```
ConcreteFlyweight 
```java
public class FlyweightImpl implements Flyweight {

    public void action(int arg) {
        System.out.println("参数值: " + arg);
    }
}
```
FlyweightFactory 
```java
public class FlyweightFactory {

    private static Map flyweights = new HashMap();
    
    public FlyweightFactory(String arg) {
        flyweights.put(arg, new FlyweightImpl());
    }
    
    public static Flyweight getFlyweight(String key) {
        if (flyweights.get(key) == null) {
            flyweights.put(key, new FlyweightImpl());
        }
        return flyweights.get(key);
    }
    
    public static int getSize() {
        return flyweights.size();
    }
}
```
Test 
```java
public class Test {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        Flyweight fly1 = FlyweightFactory.getFlyweight("a");
        fly1.action(1);
        
        Flyweight fly2 = FlyweightFactory.getFlyweight("a");
        System.out.println(fly1 == fly2);
        
        Flyweight fly3 = FlyweightFactory.getFlyweight("b");
        fly3.action(2);
        
        Flyweight fly4 = FlyweightFactory.getFlyweight("c");
        fly4.action(3);
        
        Flyweight fly5 = FlyweightFactory.getFlyweight("d");
        fly4.action(4);
        
        System.out.println(FlyweightFactory.getSize());
    }
}
```
result 

参数值: 1
true
参数值: 2
*数值: 3
参数值: 4
4
## 代理模式
 

    定义：为其他对象提供一种代理以控制对这个对象的访问。
适用性

    1.远程代理（RemoteProxy）为一个对象在不同的地址空间提供局部代表。

    2.虚代理（VirtualProxy）根据需要创建开销很大的对象。

    3.保护代理（ProtectionProxy）控制对原始对象的访问。

    4.智能指引（SmartReference）取代了简单的指针，它在访问对象时执行一些附加操作。
			
参与者

    1.Proxy
      保存一个引用使得代理可以访问实体。若RealSubject和Subject的接口相同，Proxy会引用Subject。
      提供一个与Subject的接口相同的接口，这样代理就可以用来替代实体。
      控制对实体的存取，并可能负责创建和删除它。
      其他功能依赖于代理的类型：

    2.RemoteProxy负责对请求及其参数进行编码，并向不同地址空间中的实体发送已编码的请求。

    3.VirtualProxy可以缓存实体的附加信息，以便延迟对它的访问。

    4.ProtectionProxy检查调用者是否具有实现一个请求所必需的访问权限。

    5.Subject
      定义RealSubject和Proxy的共用接口，这样就在任何使用RealSubject的地方都可以使用Proxy。

    6.RealSubject
      定义Proxy所代表的实体。
7.使用代理原因：
（1）授权机制不同级别的用户对同一对象拥有不同的访问权利（如注册用户和游客）。
（2）某个客户端不能直接操作到某个对象，但又必须和那个对象有所互动。
（3）当那个对象是个很大的图片时，就可以采用代理代替那张大的图片。
（4）如果那个对象在Internet的远程服务端，也可以采用代理模式。
 8.搞清“局长”、“秘书”、“局长——秘书的关系”、“你”，他们就是代理的典型应用。
 
 类图:静态代理模式图
 
类图：动态代理模式图
 

例子
Proxy (静态代理示例)
```java
public class ProxyObject implements Object {

    Object obj;
    
    public ProxyObject() {
        System.out.println("这是代理类");
        obj = new ObjectImpl();
    }
    
    public void action() {
        System.out.println("代理开始");
        Obj.action();
        System.out.println("代理结束");
    }
}
```
Subject 
```java
public interface Object {

    void action();
}
```
RealSubject 
```java
public class ObjectImpl implements Object {

    public void action() {
        System.out.println("========");
        System.out.println("========");
        System.out.prrntln("这是被代理的类");
        System.out.println("========");
        System.out.println("========");
    }
}
```
Test 
```java
public class Test {

    public static void main() {
    	Object obj = new ProxyObject();
        obj.action();
    }
}
```
result 

这是代理类
代理开始
\========
\=*======
这是被代理的类
\========
\======*=
代理结束



Proxy (动态代理示例)
```java
public class LogHandler implements InvocationHandler {

	private Object targetObject;
	public Object newProxyInstance(Object targetObject){
		this.targetObject = targetObject;
		return Proxy.newProxyInstance(targetObject.getClass().getClassLoader(), targetObject.getClass().getInterfaces(), this);
		
	}
	public Object invoke(Object proxy, Method method, Object[] args)
			throws Throwable {
		System.out.println("start---->>>>"+method.getName());
		for(int i=0;i<args.length;i++){
			System.out.println(args[i]);
		}
		Object ret = null;
		try{
			//调用目标方法
		  ret = method.invoke(targetObject, args);
		  System.out.println("success---->>>>"+method.getName());
		}catch(Exception e){
			e.printStackTrace();
			  System.out.println("error---->>>>"+method.getName());
		}
		return ret;
	}

}
```

Subject 
```java
public interface UserManager {

	public void addUser(String id, String name);
	
	public void delUser(String id);
	
	public String findUser(String id);
}
```

RealSubject 
```java
public class UserManagerImple implements UserManager {

	public void addUser(String id, String name) {
	System.out.println("这是被代理的添加程序！");
	}
	
	public void delUser(String id){
		System.out.println("这是被代理的删除程序");
	}
	
	public String findUser(String id){
		System.out.println("这是被代理的有返回值的程序");
		return "张三";
	}
}
```
Test 
```java
public class Client {
	public static void main(String[] args) {
		LogHandler logHandler = new LogHandler();
		UserManager userManager = (UserManager) logHandler.newProxyInstance(new UserManagerImple());
	   
	    String name = userManager.findUser("0003");
	    System.out.println("========="+name);
	}
}

```
result 

start---->>>>findUser
0003
这是被代理的有返回值的程序
success---->>>>findUser
\=========张三