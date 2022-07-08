### java 收藏
[排序算法全家桶（Java实现）](https://blog.csdn.net/sheng0113/article/details/122186577)

[深入挖掘 ArrayList工作原理及其实现](https://blog.csdn.net/sheng0113/article/details/122615432)

[Java集合：浅谈LinkedList](https://blog.csdn.net/sheng0113/article/details/122774435)

[Java static关键字你了解多少？](https://blog.csdn.net/sheng0113/article/details/121507661)


java this关键字：  https://blog.csdn.net/sheng0113/article/details/122643864
1.this关键字可以用来访问本类的属性、方法、构造器
2.this关键字用来区分当前类的属性和局部变量
3.访问成员方法的语法：this.方法名（参数列表）
4.this不能在类定义的外部使用，this关键字肯定要与对象相关联的，只能在类定义的方法中使用
5.this关键字不能用在静态方法中：static方法是类方法，它先于任何的实例对象存在

[Spring中Bean的实例化方式](https://blog.csdn.net/sheng0113/article/details/124359714)

[Spring注解@Resource和@Autowired区别对比](https://www.cnblogs.com/think-in-java/p/5474740.html)

[彻底理解事务的4个隔离级别 ](https://www.cnblogs.com/jycboy/p/transaction.html) (https://www.51cto.com/article/697668.html)

[Spring Security实现短信验证码登录](https://www.jianshu.com/p/a7c5ee9fb998)


## java创建对象的几种方式
1、new关键字

2、Class.newInstance
这是我们运用反射创建对象时最常用的方法。Class类的newInstance使用的是类的public的无参构造器。因此也就是说使用此方法创建对象的前提是必须有public的无参构造器才行
```java
public class Main {
    public static void main(String[] args) throws Exception {
        Person person = Person.class.newInstance();
        System.out.println(person); // Person{name='null', age=null}
    }
}
```

3、Constructor.newInstance
本方法和Class类的newInstance方法很像，但是比它强大很多。 java.lang.relect.Constructor类里也有一个newInstance方法可以创建对象。我们可以通过这个newInstance方法调用有参数（不再必须是无参）的和私有的构造函数（不再必须是public）。


4、Clone
无论何时我们调用一个对象的clone方法，JVM就会创建一个新的对象，将前面的对象的内容全部拷贝进去，用clone方法创建对象并不会调用任何构造函数。 要使用clone方法，我们必须先实现Cloneable接口并复写Object的clone方法（因为Object的这个方法是protected的，你若不复写，外部也调用不了呀）。
```java
public class Person implements Cloneable {
	...
	// 访问权限写为public，并且返回值写为person
    @Override
    public Person clone() throws CloneNotSupportedException {
        return (Person) super.clone();
    }
    ...
}

public class Main {

    public static void main(String[] args) throws Exception {
        Person person = new Person("fsx", 18);
        Object clone = person.clone();

        System.out.println(person);
        System.out.println(clone);
        System.out.println(person == clone); //false
    }

}
```

5、反序列化
当我们序列化和反序列化一个对象，JVM会给我们创建一个单独的对象，在反序列化时，JVM创建对象并不会调用任何构造函数。

为了反序列化一个对象，我们需要让我们的类实现Serializable接口。
```java
public class Main {
    public static void main(String[] args) throws Exception {
        Person person = new Person("fsx", 18);
        byte[] bytes = SerializationUtils.serialize(person);

        // 字节数组：可以来自网络、可以来自文件（本处直接本地模拟）
        Object deserPerson = SerializationUtils.deserialize(bytes);
        System.out.println(person);
        System.out.println(deserPerson);
        System.out.println(person == deserPerson);
    }
}
```

附：关于两种newInstance方法的区别？
1、Class类位于java的lang包中，而Constructor是java反射机制的一部分
2、Class类的newInstance只能触发无参数的构造方法创建对象，而构造器类的newInstance能触发有参数或者任意参数的构造方法来创建对象。
3、Class类的newInstance需要其构造方法是public的或者对调用方法可见的，而构造器类的newInstance可以在特定环境下调用私有构造方法来创建对象。
4、Class类的newInstance抛出类构造函数的异常，而构造器类的newInstance包装了一个InvocationTargetException异常。
说明：Class类本质上调用了反射包Constructor中无参数的newInstance方法，捕获了InvocationTargetException，将构造器本身的异常抛出



### java面试博客：
##### [两年Java开发工作经验面试总结](https://blog.csdn.net/a754315344/article/details/105384589)
##### [骆昊Java面试](https://blog.csdn.net/jackfrued/article/details/44921941)
##### [Java 最常见的 200+ 面试题：面试必备](https://blog.csdn.net/sufu1065/article/details/88051083)
##### [两年Java开发工作经验面试总结 囧辉](https://joonwhee.blog.csdn.net/article/details/71437307)
##### [外包公司能去吗？进了外包如何翻盘？](https://blog.csdn.net/v123411739/article/details/124580750)
##### [程序员在外包公司工作怎么样？](https://blog.csdn.net/fms5201314/article/details/108302103)
##### [java面试感受，越来越难](https://zhuanlan.zhihu.com/p/68829038)


