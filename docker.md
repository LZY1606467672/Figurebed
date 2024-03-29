## 网站记录
[基于 JWT + Refresh Token 的用户认证实践](https://zhuanlan.zhihu.com/p/52300092)

[JWT中RefreshToken的应用场景及刷新token的具体实现思路](https://blog.csdn.net/isFuong/article/details/114946766)

[springboot整合redis发送手机验证码注册登录](https://cloud.tencent.com/developer/article/1640052)

[过滤器，拦截器，监听器具体应用上的区别](https://www.zhihu.com/question/35225845)

[Java HashMap工作原理及实现](https://yikun.github.io/2015/04/01/Java-HashMap%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86%E5%8F%8A%E5%AE%9E%E7%8E%B0/)


在HashMap中，为什么不能使用基本数据类型作为key？

其实和HashMap底层的存储原理有关，HashMap存储数据的特点是：无序、无索引、不能存储重复元素。

存储元素采用的是hash表存储数据，每存储一个对象的时候，都会调用其hashCode()方法，算出其hash值，如果相同，则认为是相同的数据，直接不存储，如果hash值不同，则再调用其equals方法进行比较，如果返回true，则认为是相同的对象，不存储，如果返回false，则认为是不同的对象，可以存储到HashMap集合中。

 之所以key不能为基本数据类型，则是因为基本数据类型不能调用其hashcode()方法和equals()方法，进行比较，所以HashMap集合的key只能为引用数据类型，不能为基本数据类型，可以使用基本数据类型的包装类，例如Integer Double等。

当然，在HashMap存储自定义对象的时候，需要自己再自定义的对象中重写其hashCode()方法和equals方法，才能保证其存储不重复的元素，否则将存储多个重复的对象，因为每new一次，其就创建一个对象，内存地址是不同的。

HashMap中key是可以为null, 只能存储一个null, 因为计算key的hash值的时候，如果key为null， 则其hash值为0



面试官：我们都知道在使用HashMap时，经常使用String类型或者Integer类型作为key，那我们可以使用自定义的对象吗？

应聘者：可以，能用其他对象，必须是immutable的，但是自实现的类必须Override两个方法：equals()和hashCode()。因为为了要计算hashCode()，就要防止键值改变，如果键值在放入时和获取时返回不同的hashcode的话，那么就不能从HashMap中找到你想要的对象；

面试官：那为什么String类型天生就可以做为键值呢？

应聘者：因为String是不可变的，也是final的，而且已经重写了equals()和hashCode()方法了；

可变对象

可变对象是指创建后自身状态能改变的对象。换句话说，可变对象是该对象在创建后它的哈希值可能被改变。
