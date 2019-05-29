##**Spring 控制反转与依赖注入**

&emsp;&emsp;加了@Autowired的接口属性才是IOC，即DI的范围比较广，所有的依赖注入不一定都是IOC，IOC是在编程过程中实现的，而DI是在编译过程中实现的。接口的实现某种就是为了解耦，不论是通过实现接口的实体类还是xml文件，都是为了解耦。


IoC(控制反转):  
　　首先想说说IoC（Inversion of Control，控制反转）。这是spring的核心，贯穿始终。所谓IoC，对于spring框架来说，就是由spring来负责控制对象的生命周期和对象间的关系。
这是什么意思呢，举个简单的例子，我们是如何找女朋友的？常见的情况是，我们到处去看哪里有长得漂亮身材又好的mm，然后打听她们的兴趣爱好、qq号、电话号、ip号、iq号………，想办法认识她们，投其所好送其所要，然后嘿嘿……这个过程是复杂深奥的，我们必须自己设计和面对每个环节。传统的程序开发也是如此，在一个对象中，如果要使用另外的对象，就必须得到它（自己new一个，或者从JNDI中查询一个），使用完之后还要将对象销毁（比如Connection等），对象始终会和其他的接口或类藕合起来。

　　那么IoC是如何做的呢？有点像通过婚介找女朋友，在我和女朋友之间引入了一个第三者：婚姻介绍所。婚介管理了很多男男女女的资料，我可以向婚介提出一个列表，告诉它我想找个什么样的女朋友，比如长得像李嘉欣，身材像林熙雷，唱歌像周杰伦，速度像卡洛斯，技术像齐达内之类的，然后婚介就会按照我们的要求，提供一个mm，我们只需要去和她谈恋爱、结婚就行了。简单明了，如果婚介给我们的人选不符合要求，我们就会抛出异常。整个过程不再由我自己控制，而是有婚介这样一个类似容器的机构来控制。Spring所倡导的开发方式就是如此，所有的类都会在spring容器中登记，告诉spring你是个什么东西，你需要什么东西，然后spring会在系统运行到适当的时候，把你要的东西主动给你，同时也把你交给其他需要你的东西。所有的类的创建、销毁都由 spring来控制，也就是说控制对象生存周期的不再是引用它的对象，而是spring。对于某个具体的对象而言，以前是它控制其他对象，现在是所有对象都被spring控制，所以这叫控制反转。

DI(依赖注入)：　　

　　IoC的一个重点是在系统运行中，动态的向某个对象提供它所需要的其他对象。这一点是通过DI（Dependency Injection，依赖注入）来实现的。比如对象A需要操作数据库，以前我们总是要在A中自己编写代码来获得一个Connection对象，有了 spring我们就只需要告诉spring，A中需要一个Connection，至于这个Connection怎么构造，何时构造，A不需要知道。在系统运行时，spring会在适当的时候制造一个Connection，然后像打针一样，注射到A当中，这样就完成了对各个对象之间关系的控制。A需要依赖 Connection才能正常运行，而这个Connection是由spring注入到A中的，依赖注入的名字就这么来的。那么DI是如何实现的呢？ Java 1.3之后一个重要特征是反射（reflection），它允许程序在运行的时候动态的生成对象、执行对象的方法、改变对象的属性，spring就是通过反射来实现注入的。

　　理解了IoC和DI的概念后，一切都将变得简单明了，剩下的工作只是在spring的框架中堆积木而已。
 
**例子讲解:**   
　　首先想说IoC之前，我们需要像以下方式写代码：</br>　
```
// Interface HelloWorld
public interface HelloWorld {
    public void sayHello();
}
 
// Class implements HelloWorld
public class SpringHelloWorld implements HelloWorld {
   public void sayHello()  {
           System.out.println("Spring say Hello!");
   }
}
 
// Other class implements HelloWorld
public class StrutsHelloWorld implements HelloWorld {
   public void sayHello()  {
           System.out.println("Struts say Hello!");
   }
}
 
 
// And Service class
public class HelloWorldService {
    
     // Field type HelloWorld
     private HelloWorld helloWorld;
    
     // Constructor HelloWorldService
     // It initializes the values for the field 'helloWorld'
     public HelloWorldService()  {
           this.helloWorld = new StrutsHelloWorld();
     }
 
}
```
　　显而易见的是 HelloWorldService 类管理创建HelloWorld对象。另外，在上述情况下，当 HelloWorldService 对象从它的构造创建时，HelloWorld对象也被创建了。它是从StrutsHelloWorld 创建。</br>
　　现在的问题是，您要创建一个HelloWorldService对象，HelloWorld对象也同时被创建，但它必须是SpringHelloWorld。所以 HelloWorldService 是控制“对象创建” Hello World 的。我们为什么不创建 Hello World 转让由第三方，而是使用HelloWorldService？因为我们有“反转控制”(IOC)的定义。并且IoC容器将充当管理者角色，创建了HelloWorldService 和 HelloWorld 。</br>
　　使用了IoC之后，我们需要像以下方式写代码：</br>　
  　**HelloWorldService.java**
```
package com.yiibai.tutorial.spring.helloworld;
  
public class HelloWorldService {
  
    private HelloWorld helloWorld;
  
    public HelloWorldService() {
  
    }
  
    public void setHelloWorld(HelloWorld helloWorld) {
        this.helloWorld = helloWorld;
    }
  
    public HelloWorld getHelloWorld() {
        return this.helloWorld;
    }
  
}
```
　　**beans.xml**
```
<beansxmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
                        http://www.springframework.org/schema/beans/spring-beans.xsd">
  
    <beanid="springHelloWorld"
        class="com.yiibai.tutorial.spring.helloworld.impl.SpringHelloWorld"></bean>
    <beanid="strutsHelloWorld"
        class="com.yiibai.tutorial.spring.helloworld.impl.StrutsHelloWorld"></bean>
  
  
    <beanid="helloWorldService"
        class="com.yiibai.tutorial.spring.helloworld.HelloWorldService">
        <propertyname="helloWorld"ref="springHelloWorld"/>
    </bean>
  
</beans>
```
　　IoC容器中，其作用是作为第三种角色，它的任务是创建 beans.xml 文件中声明的 Java Bean 对象。并通过setter方法注入依赖。</br>
　　在这个例子中，HelloWorldService 是一个java bean注入依赖。
    依赖注入的过程具体如下：</br>
&emsp;&emsp;Dependency Resolution Process</br>
&emsp;&emsp;The container performs bean dependency resolution as follows:</br>
&emsp;&emsp;The ApplicationContext is created and initialized with configuration metadata that describes all the beans. Configuration metadata can be specified by XML, Java code, or annotations.</br>
&emsp;&emsp;For each bean, its dependencies are expressed in the form of properties, constructor arguments, or arguments to the static-factory method (if you use that instead of a normal constructor). These dependencies are provided to the bean, when the bean is actually created.</br>
&emsp;&emsp;Each property or constructor argument is an actual definition of the value to set, or a reference to another bean in the container.</br>
&emsp;&emsp;Each property or constructor argument that is a value is converted from its specified format to the actual type of that property or constructor argument. By default, Spring can convert a value supplied in string format to all built-in types, such as int, long, String, boolean, and so forth.
&emsp;&emsp阅读源码可知，Spring依赖注入具体的源码实现由BeanWrapper与BeanWrapperImpl来完成。
&emsp;&emsp;[Spring 与 Spring Boot的区别](https://www.jianshu.com/p/ffe5ebe17c3a)






