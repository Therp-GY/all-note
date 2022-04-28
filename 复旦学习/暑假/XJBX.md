# CLEAN CODE

为什么写坏代码：

1. 破罐子破摔，继承代码本就不好
2. 时间累计越来越差
3. 赶时间！！

关于取名：

1. 不要怕麻烦的，就是要清晰的
2. 命名清晰，有利于**索引查找**
3. 类为名词，函数为动词，且名字可以表达大致意思

代码组织：

1. 靠上为高层代码，下为低层代码，上调用下

函数编写：

1. 函数完成单一职责，完成的是同一个抽象层次上的任务而不是多个抽象层次

任务规划：

1. 使用#TODO表明哪里需要修改



# python 设计模式

## solid原则

- Single Responsibility Principle：单一职责原则

  一个类应该只有一个发生变化的原因

- Open Closed Principle：开闭原则

  一个软件实体，如类、模块和函数应该对扩展开放，对修改关闭

- Liskov Substitution Principle：里氏替换原则

  所有引用基类的地方必须能透明地使用其子类的对象

- Law of Demeter：迪米特法则

  如果两个软件实体无须直接通信，那么就不应当发生直接的相互调用，可以通过第三方转发该调用。其目的是降低类之间的耦合度，提高模块的相对独立性。

- Interface Segregation Principle：接口隔离原则

  1、客户端不应该依赖它不需要的接口。
   2、类间的依赖关系应该建立在最小的接口上。

- Dependence Inversion Principle：依赖倒置原则

  1、上层模块不应该依赖底层模块，它们都应该依赖于抽象。
   2、抽象不应该依赖于细节，细节应该依赖于抽象。

  **关于依赖倒置原则：**依赖于抽象得益于代码实现过程中一个接口可以代表实现该接口的所有类，使得代码简化而类又被抽象接口规范了

  在python中，因为在使用变量时不需要先定义，因此不像java一样（使用接口名定义变量来表示依赖于抽象）这么明显，python是人为的认定一个变量为接口即可，可以见简单工厂中的例子

  **为何要依赖抽象：**为何要依赖于抽象，因为客户端只需要知道抽象的接口，而不需要知道底层代码实现，底层只需要实现接口就行，两者隔离代码更好修改

  **细节依赖抽象：**即调用的都是接口

## 设计模式

- 创建型

  工厂模式，创建时使客户端和产品分离，解耦

- 结构型

  代理模式、外观模式使得客户端调用类时解耦

- 行为型

### 简单工厂

- 角色（类/接口）

  - 工厂角色（creator）
  - 抽象产品角色（Product）
  - 具体产品角色（Concrete Product）

  关系：具体产品实现抽象产品接口，工厂来创建产品

  并可以实现一定逻辑，因此客户端与具体产品创建隔离，只需要了解产品接口（依赖于抽象）

```python
'''
	define an interface
'''

class fatherInterface(metaclass = abc.ABCMeta):
    @abc.abstractmethod
    def pullshit(self, method):
        pass

class sister(fatherInterface):
    def __init__(self):
        self.a = "big shit"

    def pullshit(self, method):
        print(method + "shit!!!!")
        return self.a
    
class son(fatherInterface):
    def __init__(self):
        self.a = "small shit"

    def pullshit(self, method):
        print(method + "shit!!!!")
        return self.a
class shitFactory():

    def createShit(self, who):
        if who == 'brother':
            return brother()
        if who == 'sister':
            return sister()

factory = shitFactory()
someBody = factory.createShit('brother')
someBody.pullshit()

```

可以看到someBody我们人为认定是个抽象接口的调用，可以是sister也可以是brother，因此也展现了**依赖转置原则**

### 工厂

- 角色

  - 抽象工厂角色（Concrete creator）
  - 工厂角色（creator）
  - 抽象产品角色（Product）
  - 具体产品角色（Concrete Product）

  关系：具体产品实现抽象产品接口，工厂实现抽象工厂接口，**一个工厂只创建一个产品**

### 抽象工厂

- 角色
  - 抽象工厂角色（Concrete creator）
  - 工厂角色（creator）
  - 抽象产品角色（Product）
  - 具体产品角色（Concrete Product）

### 建造者模式

- 角色

  - 抽象建造者

  - 具体建造者

  - 指挥者

  - 产品

    建造者用于建造产品的组件，指挥者调用建造者的函数完成组装，返回产品

### 单例模式

```python
class Singleton:
    def __new__(cls, *args, **kwargs):
        if not hasattr(cls, "_instance"):
            cls._instance = super(Singleton, cls).__new__(cls)
        return cls._instance
class MyClass(Singleton):
    def __init__(self, name):
        self.name = name


a = MyClass("GY")
b = MyClass("GYY")

print(a.name)
print(b.name)

```

依靠SingleTon的类属性`_instance`判断类是否被创建，一旦创建则`_instance`被赋值，之后所有创建的类都指向同一个类

### 适配器模式

将一个类的接口转换成客户希望的另一个接口

实现方式：

1. 类适配器：多继承方法

   ```python
   import abc
   class Payment(metaclass=abc.ABCMeta):
       @abc.abstractmethod
       def pay(self, money):
           pass
       
   class AnotherPay:
       def cost(self, money):
           print(money)
   
   # 类适配器
   class Adapter(Payment, AnotherPay):
       def pay(self, money):
           self.cost(money)
   ```

2. 对象适配器：组合方法

   组合方法可以对多个使用cost的类进行适配

   ```python
   # 对象适配器
   class PaymentAdapter(Payment):
       def __init__(self, payment):
           self.payment = payment
       def pay(self, money):
           self.payment.cost(money)
           
   p = PaymentAdapter(AnotherPay())
   p.pay(100)
   ```

### 桥模式

将一个事物的两个维度分离，使其可以独立地变化

 实现方法：

- 对事物两个维度都建立抽象接口，同时一个维度作为另一个维度的属性，比如B为A的属性，则我们可分别实现A,B接口，而二者交互只需要在A中调用B提供的接口即可。客户端只需要新建A类并传入B属性就可以完成使用

  A可以看作杯柄，B可以看作杯身，两者连接，而又可以互相扩展

- 角色

  1. 抽象（cupInterface）
  2. 细化抽象（bigCup）
  3. 实现者（favourInterface）
  4. 具体实现者（threeSugar）

应用：

- 比如杯子有小杯、中杯、大杯，口味有三分、五分、七分，则杯子和口味桥接，如果有新杯子和新口味则只需要添加新杯子类和新口味类即可。使用时新建一个新杯子类并且传入新口味。

```python
from abc import ABCMeta, abstractmethod


class cupInterface(metaclass=ABCMeta):
    def __init__(self, favour):
        self.favour = favour

    @abstractmethod
    def getSize(self):
        pass

    def taste(self):
        self.getSize()
        self.favour.getFavour()


class favourInterface(metaclass=ABCMeta):
    @abstractmethod
    def getFavour(self):
        pass


class bigCup(cupInterface):
    def getSize(self):
        print("big cup")


class middleCup(cupInterface):
    def getSize(self):
        print("middle cup")


class threeSugar(favourInterface):
    def getFavour(self):
        print("3 sweet")


class fiveSugar(favourInterface):
    def getFavour(self):
        print("5 sweet")


class sevenSugar(favourInterface):
    def getFavour(self):
        print("7 sweet")


if __name__ == '__main__':
    cup1 = bigCup(threeSugar())
    cup1.taste()
    cup2 = middleCup(fiveSugar())
    cup2.taste()

```

### 组合模式

用处有限

定义：

- 组合模式允许你将对象组合成树形结构来表现”部分-整体“的层次结构，使得客户以一致的方式处理单个对象以及对象的组合。

实现

- 组合模式实现的最关键的地方是——简单对象和复合对象必须实现相同的接口。这就是组合模式能够将组合对象和简单对象进行一致处理的原因。
- 角色
  - 组合部件（Graph）：它是一个抽象角色，为要组合的对象提供统一的接口。
  - 叶子（Point）：在组合中表示子节点对象，叶子节点不能有子节点。
  - 合成部件（Line,Picture）：定义有枝节点的行为，用来存储部件，实现在Component接口中的有关操作。
  - 客户端

```python
from abc import ABCMeta, abstractmethod


class Graph(metaclass=ABCMeta):

    @abstractmethod
    def draw(self):
        pass


class Point(Graph):
    def __init__(self, a, b):
        self.a = a
        self.b = b

    def __str__(self):
        return "点(%s, %s)" % (self.a, self.b)

    def draw(self):
        print(str(self))

class Line(Graph):
    def __init__(self, point_a, point_b):
        self.point_a = point_a
        self.point_b = point_b

    def __str__(self):
        return "线(%s, %s)" % (self.point_a, self.point_b)

    def draw(self):
        print(str(self))

class Pic(Graph):
    def __init__(self, iter):
        self.children = []
        for graph in iter:
            self.children.append(graph)
    
    def draw(self):
        for graph in self.children:
            graph.draw()        

```

相当于层层递归调用

### 外观模式

简单且常用

定义：

- 为子系统中的一组接口提供一个一致的界面，用来访问子系统中的一群接口。

实现：

- 角色
  - Facade：负责子系统的的封装调用
  - Subsystem Classes：具体的子系统，实现由外观模式Facade对象来调用的具体任务

即新建一个类将子系统封装起来，并调用子系统的方法完成功能实现。客户端调用外观类即可而不需要知道子系统内的细节

### 代理模式

定义：

- 为其它对象提供一种代理以控制对这个对象的访问

实现方法：

- 角色
  1. 代理对象（remoteComputer）
  2. 抽象代理(Proxy)
  3. 具体代理(VirtualProxy)

- 用一个代理类装载被代理接口的一个实例，并提供访问接口

```python
from abc import ABCMeta, abstractmethod

# 代理对象
class remoteComputer():
    def __init__(self):
        self.f = 101

    def readFile(self):
        return self.f

    def writeFile(self, content):
        print("writing")
        f = content


class Proxy(metaclass=ABCMeta):

    @abstractmethod
    def get_item(self):
        pass

    @abstractmethod
    def set_item(self):
        pass

# 代理类1
class VirtualProxy(Proxy):
    def __init__(self):
        self.computer = None

    def get_item(self):
        # 防止过度占用资源
        if self.computer is None:
            self.computer = remoteComputer()
        return self.computer.readFile()

    def set_item(self, content):
        # 防止过度占用资源
        if self.computer is None:
            self.computer = remoteComputer()
        self.computer.writeFile(content)
        # 还可以增加别的逻辑.....

# 代理类2        
class  OtherProxy(Proxy):
    def __init__(self):
        self.computer = remoteComputer()

    def get_item(self):
        return self.computer.readFile()

    def set_item(self, content):
        raise PermissionError("无访问权限")
```

### 责任链模式

定义：

- 责任链模式（Chain of Responsibility Pattern）为请求创建了一个接收者对象的链。这种模式给予请求的类型，对请求的发送者和接收者进行解耦。这种类型的设计模式属于行为型模式。

实现方法：

- 角色
  1. 抽象处理者
  2. 具体处理者
  3. 客户端
- 下一位处理者作为上一位处理者的属性，从而完成解耦（不需要知道其它所有处理者）

```python
from abc import ABCMeta, abstractmethod


class Handler(metaclass=ABCMeta):
    @abstractmethod
    def deal(self, money):
        pass

class Father(Handler):

    def deal(self, money):
        if money > 1000000000:
            print("fuck away")
        else:
            print("ok, I will give you")

class Mother(Handler):
    def __init__(self):
        self.nextHandler = Father()

    def deal(self, money):
        if money > 100000:
            print("No")
            self.nextHandler.deal(money)
        else:
            print("ok, I will give you")

class Son(Handler):

    def __init__(self):
        self.nextHandler = Mother()
    def deal(self, money):
        if money > 100:
            print("No")
            self.nextHandler.deal(money)
        else:
            print("ok, I will give you")

handler = Son()
handler.deal(1000000000)
```

### 观察者模式

定义：

- 当对象间存在一对多关系时，则使用观察者模式（Observer Pattern）。比如，当一个对象被修改时，则会自动通知依赖它的对象

角色：

- 抽象主题
- 具体主题 -- 发布者
- 抽象观察者
- 具体观察者 -- 订阅者

```python
from abc import ABCMeta, abstractmethod


class Observer(metaclass=ABCMeta):
    @abstractmethod
    def update(self, notice):
        pass


class Notice:
    def __init__(self):
        self.observers = []

    def attach(self, observer):
        self.observers.append(observer)

    def detach(self, observer):
        self.observers.remove(observer)

    def notify(self):
        for obs in self.observers:
            obs.update()


class StaffNotice(Notice):

    def __init__(self, company_info=None):
        super().__init__()
        self.company_info = company_info

    @property
    def company_info(self):
        return self.__company_info

    @company_info.setter
    def company_info(self, info):
        self.__company_info = info


class Staff(Observer):
    def __init__(self):
        self.company_info = None

    def update(self, notice):
        self.company_info = notice.company_info

```

