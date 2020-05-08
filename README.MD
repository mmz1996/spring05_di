**spring的依赖注入di**
Dependency Injection

IOC降低程序之间耦合（依赖关系）
依赖关系的管理：以后都交给spring来维护

在当前类中需要用到其他类的对象，由spring为我们提供，我们只需要在配置文件中说明
依赖关系的维护就叫做依赖注入

**依赖注入类型**
能注入的数据：有三种

基本类型和String
其他的bean类型（在配置文件中配置过的bean）
复杂类型（集合类型）



**注入的方式**

> 使用构造函数

标签 constructor-arg  标签中的属性
type属性：指定要注入的数据的数据类型，该数据类型也是构造函数某个或者某些参数的类型（并不能独立实现）
index: 指定要注入的数据给构造函数中指定索引位置的参数赋值。参数索引的位置是从0开始
name: 用于指定给构造函数中指定名称的参数赋值
以上三个用于指定给构造函数中的哪个参数进行赋值，name是常用的。

value用于给基本类型和String类型，数据
ref 用于指定其他bean类型的数据，指的是在spring中ioc核心容器中出现过的bean对象
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200508130857288.png)

特点：在获取bean对象，注入数据时必须的操作，否则对象无法创建成功
弊端：改变了bean对象的实例化方式，使我们在创建对象时，如果用不到数据，也必须提供



> 使用set方法
> 
property标签，出现位置bean标签的内部
name/value/ref 同上面构造函数一样。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200508131604279.png)

```bash
<bean id="accountService2" class="com.mmz.service.impl.AccountServiceImpl2">
       <property name="name" value="test" ></property>
        <property name="age" value="21"></property>
        <property name="birthday" ref="now"></property>
    </bean>
```
优势：创建对象没有明确的限制，可以直接使用默认构造方法
弊端：如果某个成员必须有值，则set方法无法保证一定注入


使用注解提供（单独讲）


**集合的注入**


用于给list结构集合注入的标签，有list array set
用于给map结构集合注入的标签 map props
结构相同，标签可以互换

```bash
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">


    <bean id="accountService" class="com.mmz.service.impl.AccountServiceImpl">
        <constructor-arg name="name" value="test"></constructor-arg>
        <constructor-arg name="age" value="18"></constructor-arg>
        <constructor-arg name="birthday" ref="now"></constructor-arg>
    </bean>

<!--    //配置日期对象-->
    <bean id="now" class="java.util.Date"></bean>


    <bean id="accountService2" class="com.mmz.service.impl.AccountServiceImpl2">
       <property name="name" value="test" ></property>
        <property name="age" value="21"></property>
        <property name="birthday" ref="now"></property>
    </bean>


    <bean id="accountService3" class="com.mmz.service.impl.AccountServiceImpl3">
        <property name="myStrs">
            <array>
                <value>AAA</value>
                <value>BBB</value>
                <value>CCC</value>
            </array>
        </property>

        <property name="myList">
            <list>
                <value>AAA</value>
                <value>BBB</value>
                <value>CCC</value>
            </list>
        </property>

        <property name="mySet">
            <set>
                <value>AAA</value>
                <value>BBB</value>
                <value>CCC</value>
            </set>
        </property>

        <property name="myMap">
            <map>
                <entry key="testA" value="AAAA"></entry>
                <entry key="testB">
                    <value>BBBB</value>
                </entry>
            </map>
        </property>

        <property name="myProps">
            <props>
               <prop key="testC">CCC</prop>
               <prop key="testD">DDD</prop>
            </props>
        </property>
    </bean>

</beans>


```
