 ![image](https://github.com/yrz1995Whu/SpringStudy/blob/master/img/aop.PNG)<br/>
&emsp;&emsp;面向切片编程，即对业务中的公共部分取出来，然后统一添加在各个事务中<br/>
&emsp;&emsp;[Spring Aop专业术语理解](https://blog.csdn.net/q982151756/article/details/80513340)</br>
&emsp;&emsp;[Spring Aop例子解析](https://baijiahao.baidu.com/s?id=1613310315603029991&wfr=spider&for=pc)</br>
&emsp;&emsp;AOP有常见的两种方式，一种为XML，一种为注解+@AspectJ  [Spring AOP两种实现方式](https://www.cnblogs.com/xiaoxi/p/5981514.html)
&emsp;&emsp;动态代理分为Cglib与JDK，其中jdk代理的类必须要实现接口，cglib代理的类可以不实现。但是cglib动态代理是通过字节码底层继承要代理类来实现（如果被代理类被final关键字所修饰，那么抱歉会失败），在创建代理这一块没有JDK动态代理快，但是运行速度比JDK动态代理要快。二者各有千秋。

 
