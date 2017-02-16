---
title: 设计模式(Java)_3
date: 2016-05-20 23:12:55
tags:
- 设计模式
- Java
categories: 设计模式
---

> 上一篇：[设计模式(Java)_2](http://liwenquan.top/2016/05/20/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-2/)

# 行为型模式
Chain of Responsibility ( 责任链模式 ) 
Command ( 命令模式 ) 
Interpreter ( 解释器模式 ) 
Iterator ( 迭代器模式 ) 
Mediator ( 中介者模式 ) 
Memento ( 备忘录模式 ) 
Observer ( 观察者模式 ) 
State ( 状态模式 ) 
Strategy ( 策略模式 ) 
TemplateMethod ( 模板方法 ) 
Visitor ( 访问者模式 ) 
<!--more-->
## 责任链模式

    使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。将这些对象连成一*链，
    并*着这条链传递该请求，直到有一个对象处理它为止。
    
    这一模式的想法是，给多个对象处理一个请求的机会，从而解耦发送者和接受者.
  适用性

    1.有多个的对象可以处理一个请求，哪个对象处理该请求运行时刻自动确定。

    2.你*在不明确指定接收者的情况下，向多个对象中的一个提交一个请求。

    3.可处理一个请求的对象集合应被动态指定。
			
  参与者

    1.Handler
      定义一个处理请求的接口。
      （可选）实现后继链。

    2.ConcreteHandler
      处理它所负责的请*。
      可访问它的后继者。
      如果可处理该*求，就处理*；否则将该请求转发给它的后继者。

    3.Client
      向链上的具体处理者(ConcreteHandler)对象提交请求。
  类图
 
  例子
Hand*er 
```java
public interface RequestHandle {

    void handleRequest(R*quest request);
}
```
ConcreteHandler 
```java
public class HRRequestHandle implements RequestHandle {

    public void handleRequest(Request request) {
        if (request instanceof DimissionRequest) {
            System.out.println("要离职, 人事审批!");
        } 

        System.out.println("请求完*");
    }
}

public class PMRequestHandle implements RequestHandle {

    Req*estHandle rh;
    
    public PMRequestHandle(RequestHandle *h) {
        this.rh = rh;
    }
    
    public void handle*equest(Request request) {
        if (request instanceof AddMoneyRequest) {
            System.out.println("要加薪, 项目经理审批!*);
        } else {
            rh.handleRequest(request);
        }
    }
}

public class TLRequestHandle implements RequestHandle {

    RequestHandle rh;
    
    public TLRequestHandle(RequestHand*e rh) {
        this.rh = rh;
    }

    public void handleRequest(Request request) {
        if (request instanceof LeaveRe*uest) {
            System.ou*.println("要请假, 项目组长审批!");
        } else {
            rh.handleRequest(request);
        }
    }
}
```
Client 
```java
public *lass Test {

    
    public static v*id main(String[] args) {
        RequestHa*dle hr = *ew HRRequ*stHandle();
        Requ*stHandle pm = new P*RequestHandle(hr);
        RequestHandle tl = new TLRequestHandle(pm);
        
        //team leader处理离职请求
        Request request = new DimissionRequest()*
        tl.handleRequest(request);
        
        System.out.println("===========");
        //team leader处理加薪请求
        request = new AddMoneyRequest();
        tl.handleRequ*st(request);
        
        System.out.println("========");
        //项目经理上理辞职请求
        requ*st = ne* Dimissio*Request();
        pm.handleRequest(request);
    }
}
```
result 

要离职, 人事审批!
请求完毕
\=======*===
要加薪, 项目经理审批!
\========
要离职, 人事审批!
请求完毕
## 命令模式

    将一个请求封装为一个对象，从而使你可用不同的请求对客户进行参数化；对请求排队或记录请求日志，以及支持可撤消的操作。
适用性

    1.抽象出待执行的动作以参数化某对象。

    2.在不同的时刻指定、排列和执行请求。

    3.支持取消操作。

    4.支持修改日志，这样当系统崩溃时，这样修改可以被重做一遍。

    5.用构建在原语操作上的高层操作构造一个系统。
			
参与者

    1.Command
      声明执行操作的接口。

    2.ConcreteCommand
      将一个接收者对象绑定于一个动作。
      调用接收者相应的操作，以实现Execute。

    3.Client
      创建一个具体命令对象并设定它的接收者。

    4.Invoker
      要求该命令执行这个请求。

    5.Receiver
      知道如何实施与执行一个请求相关的操作。任何类都可能作为一个接收者。
    6.理解五个角色：球队老板，老板的命令（接口），教练，命令的内容，球员。这五个角色的关系，自然就懂命令模式了。
类图
 
例子
Command 
```java
public abstract class Command {
    
    protected Receiver receiver;
    
    public Command(Receiver receiver) {
        this.receiver = receiver;
    }
    
    public abstract void execute();
}
```
ConcreteCommand 
```java
public class CommandImpl extends Command {

    public CommandImpl(Receiver receiver) {
        super(receiver);
    }
    
    public void execute() {
        receiver.request();
    }
}
```
Invoker 
```java
public class Invoker {

    private Command command;
    
    public void setCommand(Command ccmmand) {
        this.command = command;
    }
    
    public void execute() {
        command.execute();
    }
}
```
Receiver 
```java
public class Receiver {

    public void receive() {
        System.out.println("This is Receive class!");
    }
}
```
Test 
```java
public class Test {

    public static void main(String[] args) {
        Receiver rec = new Receiver();
        Command cmd = new CommandImpl(rec);
        Invoker i = new Invoker();
        i.setCommand(cmd);
        i.execute();
    }
}
```
result 

This is Receive class!
## 解释器模式

    给定一个语言，定义它的文法的一种表示，并定义一个解释器，这个解释器使用该表示来解释语言中的句子。
适用性

    当有一个语言需要解释执行,并且你可将该语言中的句子表示为一个抽象语法树时，可使
    用解释器模式。而当存在下情况时该模式效果最好：

    1.该文法简单对于复杂的文法,文法的*层次变得庞大而无法管理。

    2.效率不是一个关键问题最高效的解释器通常不是通过直接解释语法分析树实现的,而是首先将它们转换成另一种形式。
			
参与者

    1.AbstractExpression(抽象表达式)
      声明一个抽象的解释操作，这个接口为抽象语法树中所有的节点所共享。

    2.TerminalExpression(终结符表达式)
      实现与文法中的终结符相关联的解释操作。
      一个句子中的每个终结符需要该类的一个实例。

    3.NonterminalExpression(非终结符表达式)
      为文法中的非终结符实现解释(Interpret)操作。

    4.Context（上下文）
      包含解释器之外的一些全局信息。

    5.Client（客户）
      构建(或被给定)表示该文法定义的语言中这个特定的句子的抽象放法树。
      该抽象语法树由NonterminalExpression和TerminalExpression的实例装配而成。
      调用解*操作。
类图
 
例子
AbstractExpression 
```java
pu*lic abstract class Expression {

    abstract void interpret(Context ctx);
}
```
Expression 
```java
public class AdvanceExpression extends Expression {

    void interpret(Context ctx) {
        System.out.println("这是高级解析器!");
    }
}

public class SimpleExpression extends Expression {

    void interpret(Context ctx) {
        System.out.println("这是普通解析器!");
    }
}
```
Context 
```java
public class Context {

    private String content;
    
    private List list = new ArrayList();
    
    public void setContent(String content) {
        this.content = content;
    }
    
    public String getContent() {
        return this.content;
    }
    
    public void add(Expression eps) {
        list.add(eps);
    }
    
    public List getList() {
        return list;
    }
}
```
Test 
```java
public class Test {

    public static void main(String[] args) {
        Context ctx = new Context();
        ctx.add(new SimpleExpression());
        ctx.add(new AdvanceExpression());
        ctx.add(new SimpleExpression());
        
        for (Expression eps : ctx.getList()) {
            Eps.interpret(ctx);
        }
    }
}
```
result 

这是普通解析器!
这是高级解析器!
这是普通解析器!
## 迭代器模式

    给定一个语言，定义它的文法的一种表示，并定义一个解释器，这个解释器使用该表示来解释语言中的句子。
适用性

    1.访问一个聚合对象的内容而无需暴露它的内部表示。

    2.支持对聚合对象的多种遍历。

    3.为遍历不同的聚合结构提供一的统一的接口(即,支持多态迭代)。
			
参与者

    1.Iterator
      迭代器定义访问和遍历元素的接口。

    2.ConcreteIterator
      具有迭代器实现迭代器接口。
      对该聚合遍历时跟踪当前位置。

    3.Aggregate
      聚合定义创建相应迭代器对象的接口。

    4.ConcreteAggregate
      具体聚合实现创建相应迭代器的接口，该操作返回ConcreteIterator的一个适当的实例.
类图
 
例子
Iterator 
```java
public interface Iterator {

    Object next();
    
    void first();
    
    void last();
    
    boolean hasNext();
}
```
ConcreteIterator 
```java
public class IteratorImpl implements Iterator {

    private List list;
    
    private int index;
    
    public IteratorImpl(List list) {
        index = 0;
        this.list = list;
    }
    
    public void first() {
        index = 0;
    }

    public void last() {
        index = list.getSize();
    }

    public Object next() {
        Object obj = list.get(index);
        index++;
        return obj;
    }

    public boolean hasNext() {
        return index < list.getSize();
    }
}
```
Aggregate 
```java
public interface List {

    Iterator iterator();
    
    Object get(int index);
    
    int getSize();
    
    void add(Object obj);
}
```
ConcreteAggregate 
```java
public class ListImpl implements List {

    private Object[] list;
    
    private int index;
    
    private int size;
    
    public ListImpl() {
        index = 0;
        size = 0;
        list = new Object[100];
    }
    
    public Iterator iterator() {
        return new IteratorImpl(this);
    }
    
    public Object get(int index) {
        return list[index];
    }
    
    public int getSize() {
        return this.size;
    }
    
    public void add(Object obj) {
        list[index++] = obj;
        size++;
    }
}
```
Test 
```java
public class Test {

    public static void main(String[] args) {
        List list = new ListImpl();
        list.add("a");
        list.add("b");
        list.add("c");
        //第一种迭代方式
        Iterator it = list.iterator();
        while (it.hasNext()) {
            System.out.println(it.next());
        }
        
        System.out.println("=====");
        //第二种迭代方式
        for (int i = 0; i < list.getSize(); i++) {
            System.out.println(list.get(i));
        }
    }
}
```
result 

a
b
c
\=====
a
b
c
## 中介者模式

    用一个中介对象来封装一系列的对象交互。中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。
适用性

    1.一组对象以定义良好但是复杂的方式进行通信。产生的相互依赖关系结构混乱且难以理解。

    2.一个对象引用其他很多对象并且直接与这些对象通信,导致难以复用该对象。

    3.想定制一个分布在多个类中的行为，但又不想生成太多的子类。
			
参与者

    1.Mediator
      中介者定义一个接口用于与各同事（Colleague）对象通信。

    2.ConcreteMediator
      具有中介者通过协调各同事对象实现协作行为。
      了解并维护它的各个同事。

    3.Colleagueclass
      每一个同事类都知道它的中介者对象。
      每一个同事对象在需与其他的同事通信的时候需与它的中介者通信
类图
 
例子
Mediator 
```java
public abstract class Mediator {

    public abstract void notice(String content);
}
ConcreteMediator 
```java
public class ConcreteMediator extends Mediator {

    private ColleagueA ca;
    
    private ColleagueB cb;
    
    public ConcreteMediator() {
        ca = new ColleagueA();
        cb = new Col*eagueB();
    }
    
    public void notice(String content) {
        if (content.equals("boss")) {
            //老板来了, 通知员工A
            ca.action();
        }
        if (content.equals("client")) {
            //客户来了, 通知前台B
            cb.action();
        }
    }
}
Colleagueclass 
```java
public class ColleagueA extends Colleague {

    
    public void action(） {
        System.out.println("普通员工努力工作");
    }
}
public class ColleagueB extends Colleague {

    public void action() {
        System.out.println("前台注意了!");
    }
}
Test 
```java
public class Test {

    public static void main(String[] args) {
        Mediator med = new ConcreteMediator();
        //老板来了
        med.notice("boss");
        
        //客户来了
        med.notice("client");
    }
}
```
result 

普通员工努力工作
前台注意了!
## 备忘录模式

    在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样以后就可将该对象恢复到原先保存的状态。
适用性

    1.必须保存一个对象在某一个时刻的(部分)状态,这样以后需要时它才能恢复到先前的状态。

    2.如果一个用接口来让其它对象直接得到这些状态，将会暴露对象的实现细节并破坏对象的封装性。
			
参与者

    1.Memento
      备忘录存储原发器对象的内部状态。

    2.Originator
      原发器创建一个备忘录,用以记录当前时刻的内部状态。
      使用备忘录恢复内部状态.

    3.Caretaker
      负责保存好备忘录。
      不能对备忘录的内部进行操作或检查。
类图
 
例子
Memento 
```java
public class Memento {

    private String state;

    public Memento(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }

    public void setState(String state) {
        this.state = state;
    }
}
Originator 
```java
public class Originator {

    private String state;

    public String getState() {
        return state;
    }

    public void setState(String state) {
        this.state = state;
    }
    
    public Memento createMemento() {
        return new Memento(state);
    }
    
    public void setMemento(Memento memento) {
        state = memento.getState();
    }
    
    public void showState(){
        System.out.println(state);
    }
}
Caretaker 
```java
public class Caretaker {
    
    private Memento memento;
    
    public Memento getMemento(){
        return this.memento;
    }
    
    public void setMemento(Memento memento){
        this.memento = memento;
    }
}
```
Test 
```java
public class Test {

    public static void main(String[] args) {
        Originator org = new Originator();
        org.setState("开会中");
        
        Caretaker ctk = new Caretaker();
        ctk.setMemento(org.createMemento());//将数据封装在Caretaker
        
        org.setState("睡觉中");
        org.showState();//显示
        
        org.setMemento(ctk.getMemento());//将数据重新导入
        org.showState();
    }
}
```
result 

睡觉中
开会中
## 观察者模式

    定义对象间的一种一对多的依赖关系,当一个对象的状态发生改变时,所有依赖于它的对象都得到通知并被自动更新。
适用性

    1.当一个抽象模型有两个方面,其中一个方面依赖于另一方面。
      将这二者封装成独立的对象中以使它们可以各自独立地改变和复用。

    2.当对一个对象的改变需要同时改变其它对象,而不知道具体有多少对象有待改变。

    3.当一个对象必须通知其它对象，而它又不能假定其它对象是谁。
			
参与者

    1.Subject（目标）
      目标知道它的观察者。可以有任意多个观察者观察同一个目标。
      提供注册和删除观察者对象的接口。

    2.Observer（观察者）
      为那些在目标发生改变时需获得通知的对象定义一个更新接口。

    3.ConcreteSubject（具体目标）
      将有关状态存入各ConcreteObserver对象。
      当它的状态发生改变时,向它的各个观察者发出通知。

    4.ConcreteObserver（具体观察者）
      维护一个指向ConcreteSubject对象的引用。
      存储有关状态，这些状态应与目标的状态保持一致。
      实现Observer的更新接口可使自身状态与目标的状态保持一致
类
 
 
例子
Subject 
```java
public abstract class Citizen {
    
    List pols;
    
    String help = "normal";
    
    public void setHelp(String help) {
        this.help = help;
    }
    
    public String getHelp() {
        return this.help;
    }
    
    abstract void sendMessage(String help);

    public void setPolicemen() {
        this.pols = new ArrayList();
    }
    
    public void register(Policeman pol) {
        this.pols.add(pol);
    }

    public void unRegister(Policeman pol) {
        this.pols.remove(pol);
    }
}
```
Observer 
```java
public interface Policeman {

    void action(Citizen ci);
}
```
ConcreteSubject 
```java
public class HuangPuCitizen extends Citizen {

    public HuangPuCitizen(Policeman pol) {
        setPolicemen();
        Register(pol);
    }
    
    public void sendMessage(String help) {
        setHelp(help);
        for(int i = 0; i < pols.size(); i++) {
            Policeman pol = pols.get(i);
            //通知警察行动
            pol.action(this);
        }
    }
}

public class TianHeCitizen extends Citizen {

    public TianHeCitizen(Policeman pol) {
        setPolicemen();
        register(pol);
    }
    
    public void sendMessage(String help) {
        setHelp(help);
        for (int i = 0; i < pols.size(); i++) {
            Policeman pol = pols.get(i);
            //通知警察行动
            pol.action(this);
        }
    }
}
```
ConcreteObserver 
```java
public class HuangPuPoliceman implements Policeman {

    public void action(Citizen ci) {
        String help = ci.getHelp();
        if (help.equals("normal")) {
            System.out.println("一切正常, 不用出动");
        }
        if (help.equals("unnormal")) {
            System.out.println("有犯罪行为, 黄埔警察出动!");
        }
    }
}

public class TianHePoliceman implements Policemen {

    public void action(Citizen ci) {
        String help = ci.getHelp();
        if (help.equals("mormal")) {
            System.out.println("一切正常, 不用出动");
        }
        if (help.equals("unnormal")) {
            System.out.println("有犯罪行为, 天河警察出动!");
        }
    }
}
```
Test 
```java
public class Test{

    public static void main(String[] args) {
        Policeman thPol = new TianHePoliceman();
        Policeman hpPol = new HuangPuPoliceman();
        
        Citizen citizen = new HuangPuCitizen(hpPol);
        citizen.sendMessage("unnormal");
        citizen.sendMessage("normal");
        
        System.out.println("===========");
        
        citizen = new TianHeCitizen(thPol);
        citizen.sendMessage("normal");
        citi*en.sendMessage("unnormal");
    }
}
result 

有犯罪行为, 黄埔警察出动!
一切正常, 不用出动
\===========
一切正常, 不用出动
有犯罪行为, 天河警察出动!
## 状态模式

    定义对象间每一种一对多的依赖关系,当一个对象的状态发生改变时,所有依赖于它的对象都得到通知并被自动更新。
适用性

    1.一个对象的行为取决于它的状态,并且它必须在运行时刻根据状态改变它的行为。

    2.一个操作中含有庞大的多分支的条件语句，且这些分支依赖于该对象的状态。
      这个状态通常用一个或多个枚举常量表示。
      通常,有多个操作包含这一相同的条件结构。
      State模式将每一个条件分支放入一个独立的类中。
      这使得你可以根据对象自身的情况将对象的状态作为一个对象，这一对象可以不依赖于其他对象而独立变化。
			
参与者

    1.Context
      定义客户感兴趣的接口。
      维护一个ConcreteState子类的实例，这个实例定义当前状态。

    2.State
      定义一个接口以封装与Context的一个特定状态相关的行为。

    3.ConcreteStatesubclasses
      每一子类实现一个与Context的一个状态相关的行为。
类图
 
 
例子
Context 
```java
public class Context {

    private Weather weather;

    public void setWeather(Weather weather) {
        this.weather = weather;
    }

    public Weather getWeather() {
        return this.weather;
    }

    public String weatherMessage() {
        return weather.getWeather();
    }
}
```
State 
```java
public interface Weather {

    String getWeather();
}
```
ConcreteStatesubclasses 
```java
public class Rain implements Weather {

    public String getWeather() {
        return "下雨";
    }
}

public class Sunshine implements Weather {

    public String getWeather() {
        return "阳光";
    }
}
```
Test 
```java
public class Test{

    public static void main(String[] args) {
        Context ctx1 = new Context();
        ctx1.setWeather(new Sunshine());
        System.out.println(ctx1.weatherMessage());

        System.out.println("===============");

        Context ctx2 = new Context();
        ctx2.setWeather(new Rain());
        System.out.println(ctx2.weatherMessage());
    }
}
result 

阳光
===============
下雨
## 策略模式

    定义一系列的算法,把它们一个个封装起来,并且使它们可相互替换。本模式使得算法可独立于使用它的客户而变化。
适用性

    1.许多相关的类仅仅是行为有异。“策略”提供了一种用多个行为中的一个行为来配置一个类的方法。

    2.需要使用一个算法的不同变体。

    3.算法使用客户不应该知道的数据。可使用策略模式以避免暴露复杂的、与算法相关的数据结构。

    4.一个类定义了多种行为,并且这些行为在这个类的操作中以多个条件语句的形式出现。
      将相关的条件分支移入它们各自的Strategy类中以代替这些条件语句。
			
参与者

    1.Strategy
      定义所有支持的算法的公共接口。Context使用这个接口来调用某ConcreteStrategy定义的算法。

    2.ConcreteStrategy
      以Strategy接口实现某具体算法。

    3.Context
      用一个ConcreteStrategy对象来配置。
      维护一个对Strategy对象的引用。
      可定义一个接口来让Stategy访问它的数据。
类图
 
例子
Strategy 
```java
public abstract class Strategy {

    public abstract void method();
}
```
ConcreteStrategy 
```java
public class StrategyImplA extends Strategy {

    public void method() {
        System.out.println("这是第一个实现");
    }
}

public class StrategyImplB extends Strategy {

    public void method() {
        System.out.println("这是第二个实现");
    }
}

public class StrategyImplC extends Strategy {

    public void method() {
        System.out.println("这是第三个实现");
    }
}
```
Context 
```java
public class Context {

    Strategy stra;
    
    public Context(Strategy stra) {
        this.stra = stra;
    }
    
    public void doMethod() {
        stra.method();
    }
}
```
Test 
```java
public class Test {
    
    public static void main(String[] args) {
        Context ctx = new Context(new StrategyImplA());
        ctx.doMethod();
        
        ctx = new Context(new StrategyImplB());
        ctx.doMethod();
        
        ctx = new Context(new StrategyImplC());
        ctx.doMethod();
    }
}
result 

这是第一个实现
这是第二个实现
这是第三个实现
## 模板方法
 

    定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。
    
    TemplateMethod使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。
适用性

    1.一次性实现一个算法的不变的部分，并将可变的部分留给子类来实现。

    2.各子类中公共的行为应被提取出来并集中到一个公共父类中以避免代码重复。
      首先识别现有代码中的不同之处，并且将不同之处分离为新的操作。
      最后，用一个调用这些新的操作的模板方法来替换这些不同的代码。

    3.控制子类发展。
			
参与者

    1.AbstractClass
      定义抽象的原语操作（primitiveoperation），具体的子类将重定义它们以实现一个算法的各步骤。
      实现一个模板方法,定义一个算法的骨架。
      该模板方法不仅调用原语操作，也调用定义在AbstractClass或其他对象中的操作。

    *.ConcreteClass
      实现*语操作以完成算法中与特定子类相关的步骤。
模板方法的缺陷：
1. 概念复杂性：从面向对象方法的本质来讲，父类负责抽象，子类负责具体，而模板方法恰好反过来了，父类实现了大部分功能，而子类实现少数抽象方法，然后有父类调用。违反了面向对象的一般性思维的原因是这个模式的初衷并不是抽象，而是最大限度的重用。
2. 实现复杂性：这种重用方式的代价就是每个子类身上背负了父类强加给它的包袱。
类图
 
  例子
AbstractClass 
```java
public abstract class Template {

    public abstract void print();
    
    public void update() {
        System.out.println("开始打印");
        for (int i = 0; i < 10; i++) {
            print();
        }
    }
}
```
ConcreteClass 
```java
public class TemplateConcrete extends Template {

    @override
    public void print() {
        System.out.println("这是子类的实现");
    }
}
Test 
```java
public class Test {

    public static void main(String[] args) {
        Template temp = new TemplateConcrete();
        temp.update();//控制反转，调用父亲的方法
    }
}
```
result 

开始打印
这是子类的实现
这是子类的实现
这是子类的实现
这是子类的实现
这是子类的实现
这是子类的实现
这是子类的实现
这是子类的实现
这是子类的实现
这是子类的实现