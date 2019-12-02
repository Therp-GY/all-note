# SPRING学习

## 1.1	spring概述

### 1.1.1	what is spring

full-stack轻量级开源框架，以IOC（反转控制）和AOP（面向切面编程）为内核，提供了展现层Spring MVC 和 持久层 Spring JDBC 以及业务层事务管理等众多的企业级应用技术，还整合开源世界众多著名的第三方框架和类库

### 1.1.2	spring 体系结构

Core Container{Beans，Core，Context，Spel} 核心容器，重中之重

### 工厂模式解耦

#### 优点

1.使用这样配置的好处在于以后对于UserDao接口，如果我们想更换实现类，只要改动配置文件即可，代码中完全不需要修改。

2.这样在service层的实现类中以dao层接口引用具体dao层的实现类就不需要直接创建对象，直接使用工厂模式加泛型即可

#### 理念

应该做到编译期不依赖，运行时依赖

【插】 展现层（类似前端）调用业务层（类似服务），业务层调用持久层（类似数据库），这里就有了依赖关系

【插】业务层和持久层很少有可以修改的对象，因此推荐单例而不是多例来提高效率

1.需要配置文件来配置我们的service和dao配置的内容：唯一标识=全限定类名（key = value）

2.通过读取配置文件中配置的内容，反射创建对象（即不用new来创建对象，而使用Class.forName等反射措施）

反射：JAVA反射机制是在运行状态中，对于任意一个类。都能都知道这个类的所有属性和方法，对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称之为java语言的反射机制；

如：Class.forName（“类名”）而不是     .....（new  类）Class.forName：

```
//创建Class对象的方式三：(forName():传入时只需要以字符串的方式传入即可)
//通过Class类的一个forName（String className)静态方法返回一个Class对象，className必须是全路径名称；
//Class.forName()有异常：ClassNotFoundException
```

#### 具体操作

GY包下创立factory包，其下有beanfactory.class

在resource下有bean.properties配置文件，beanfactory.class导入配置文件，并将配置文件中key对应相应value的映射存入map中。

每次只需要调用其静态方法来获取所配置的对象



### IOC控制反转

概念：把创建对象的权利交给框架，它包括依赖注入（DI）和以来查找（DL）	

作用：削减计算机程序的耦合

#### 具体操作

porm.xml中dependence中得加入springframe，同时maven会提醒import changes

////////////////////////////////////////////////////////////////////

resource中添加<beans> .....<\beans>**将对象的创建交给spring来管理**

```java
<bean id = "accountService" class = "com.GY.service.impl.AcountServiceImpl"></bean><bean id = "accountDao" class = "com.GY.dao.impl.AcountDaoImpl"></bean>
```

进行配置文件的完成

///////////////////////////////////////////////////////////////////////

##### bean的细节

在配置文件中添加

###### bean的创建

1：写id和class，使用默认构造函数创建对象

2：使用普通工厂中的方法创建，或者说从某个类中的方法创建对象（方法return 类）

<bean id =     class  =  ><>

<bean id =    factory -class =   factory-method = ><>

3：使用工厂中的静态方法创建对象

<bean id =  class =  factory-method = >

###### bean的作用范围

scope属性： 

singleton：单例    prototype：多例的  request：作用于web应用的请求范围  session：作用于web应用的会话范围

###### bean的生命周期

单例对象：

​		出生：容器创建

​		活着：容器存在

​		死亡：容器销毁

多例对象：

​		出生：当我们使用对象时为我们创建

​		活着：对象只要在使用过程中一直活着

​		死亡：对象长时间不用，且没有别的对象引用时，就会用垃圾回收机制

////////////////////////////////////////////////

##### 依赖注入DI

在之前bean的id和class基础上注入各种属性数据等等

###### 注入类型

1.基本类型和string    

2.其他bean类型（在配置文件中或者注解配置过的bean）     

3.复杂类型/集合类型

###### 注入方式

1.构造函数注入

使用标签：construor-arg

标签出现位置：bean标签的内部

标签中的属性：

type：要注入的数据的数据类型

index：用于指定要注入的数据给构造函数中指定索引位置的参数赋值

name：用于指定给构造函数中指定名称的参数复制，type，index，name都是用于指定给构造函数中哪个参数赋值，显而易见name最最直白，也是常用的哪一个

value：用于提供基本类型和String类型的数据

ref:用于制定其他的bean类型数据，它指的就是spring的ioc核心容器中出现的bean对象 （例如Date数据类型不能直接通过name和value配置，应该通过新建bean类型，再用ref引到这个新的bean类型的id上去）

缺点：就算用不到的属性还得全部注入

2.set注入

涉及标签：property

出现位置：bean标签的内部

标签的属性：

​		name：用于指定注入时**所调用的set方法**（即原类中的set方法）名称

​		value：同构造函数

​		ref：同构造函数

例子：

<property name =  value = ref = ><>

3.给集合注入

list结构：在property标签下的子标签list array set都可以

map结构：在property标签下的子标签map props都可以：

////////////////////////////////////////////////////////////

```java
ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
```

ApplicationContext:会在读取配置文件时就创建所有对象

BeanFactory:用到才会创建

bean.xml 是配置文件，若在根目录下就直接写即可

/////////////////////////////////////////////////

```java
AcountService as = (AcountService)ac.getBean("accountService");
```

获取对象

#### 基于注解的配置

同样有用于创建对象的，用于注入数据的，用于改变作用范围的，和生命周期相关的

配置文件中修改beans标签，从而可以添加context标签来扫描添加注解的包

**Component:**

​	作用：用于把当前类存入spring容器

​	属性：value ：用于指定bean的id。当我们不写时，它的默认值是当前类名，且首字母改小写

Controller表现层，Service业务层，Repository持久层

以上三个和Component作用和属性一模一样，只是可以使层次清晰	

**Autowired：**

​	作用：自动按照类型注入。只要容器中有唯一的一个bean对象类型和要注入的变量类型匹配，就可以注入成功。如果没有，则报错。如果有多个，就先按照类型匹配，再在这些对象中按照变量名称中查找，找不到就报错。

​	出现位置：可以是变量上，也可以是方法上。

​	细节：在使用注解注入时，set方法就不是必须的了。

​	**补**：spring的IOC是个map结构

​	key：accountService   					value：public class 				AcountServiceImpl implements AcountService

​	而@Autowired

​		private  AcountService acount = null

​		**访问修饰符**  **数据类型**     **变量名称**   **null**

​	而Autowired就是按照数据类型寻找，同时实现类可以看成接口类型，因此可以访问。

**Qualifier：**

作用：在按照类中注入的基础之上再按照名称注入。它在给类成员注入时不能单独使用。但是在给方法参数注入时可以。

属性：value：用于指定bean的id

例子：

@Autowired		//用于注入

@Qualifier（“accountDao1”）	//用于选择

**Resource：**

name：

作用：直接按照bean的id注入。可以独立使用。因此简化了Auto和Qual的混合使用

**注意：**

以上倒数三个都只能注入其他基本bean类型的数据。而基本类型和String类型无法使用上述实现。另外，集合类型的注入只能通过xml实现

**value：**

作用：用于注入基本类型和String类型

属性：value： 用于指定数据的值。它可以使用spring中SpEl（也就是spring的eL表达式）SpEl的写法:$（表达式）

**和改变作用范围相关的**

**Scope：**

和xml配置相同

**和生命周期相关的**

**preDestroy：**

用于指定销毁方法（销毁时执行）

**PostConstruct：**

用于指定初始化方法（初始化时执行）

#### 误区

之前想在别的类中获取bean作为组件，不能直接在这个类new ClassPathXmlApplicationContext，应该在配置文件中依靠配置bean直接添加这个组件。

## IDEA小插叙

### 创建MAVEN PROJECT

groupId：项目组织的唯一标识，实际对应JAVA的包的结构，是main目录里java的目录结构

例如：你的公司是mycom，有一个项目为myapp，那么groupId就应该是com.mycom.myapp. 

artifactedId：项目的唯一标识符，实际对应项目的名称，就是项目根目录的名称。 

例如：定义了当前maven项目在组中唯一的ID,比如，myapp-util,myapp-domain,myapp-web等。 

靠groupId, artifactId, version，package，classifier等maven坐标元素，只要在pom.xml中配置好dependancy的groupId，artifact，verison，classifier，maven就能到相应仓库中实现，

**这也反映了maven的机理和作用**

### MAVEN

**概念：**用了Maven，所需的JAR包就不能再像往常一样，自己找到并下载下来，用IDE导进去就完事了，Maven用了一个项目依赖 
(Dependency)的概念，用俗话说，就是我的项目需要用你这个jar包，就称之为我的项目依赖你这个包，换句话说，你这个JAR包就是我这个项目
的Dependency。

Maven的repository，说白了就是dependency的仓库，它按照一定的规则将dependency存放起来，以作缓存，如果本机的 repository找不到某个dependency，它就会自动去找到网上其它相关联的repository，找到的话将其下载至本地，那么下次它就不再去其它地方下载了，直接从本地获取

**最快捷方法：**

**GOOGLE搜索：maven 你需的jar包名称 repository**

比如我要做EJB，我要找jboss-j2ee.jar的Dependency

就在GOOGLE里输入

maven jboss-j2ee repository

在结果的第一条，进去你就可以在页面里找到下面这段

<dependency>
    <groupId>jboss</groupId>
    <artifactId>jboss-j2ee</artifactId>
    <version>4.0.2</version>
</dependency> 

你把上面这段代码贴到你的Maven项目的pom适当的位置去，然后运行maven，Maven就会自动下载所需的jar及相关的pom信息，你不用管它，Maven会帮你下载，并放到适当的位置。

## 问题

静态代码块；

### 泛型

就是将类型由原来的具体的类型参数化，类似于方法中的变量参数，此时类型也定义成参数形式（可以称之为类型形参），然后在使用/调用时传入具体的类型（类型实参）。

泛型的本质是为了参数化类型（在不创建新的类型的情况下，通过泛型指定的不同类型来控制形参具体限制的类型）。也就是说在泛型使用过程中，操作的数据类型被指定为一个参数，这种参数类型可以用在类、接口和方法中，分别被称为泛型类、泛型接口、泛型方法。

### 接口多态

接口 名字 =  new 类名

其中这个新创建的实例只有接口中的方法，其他此类中的方法不能用

接口被创建对象？？d

Class.forName().**newInstance()**

```java
private AcountDao acountDao = new AcountDaoImpl();  
```

