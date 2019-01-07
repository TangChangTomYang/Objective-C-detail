#### category initialize 方法


<br>
#### initialize 方法调用时刻/ 调用机制/ 调用顺序
1>、`+(void)initialize; 方法` 会在类第一次接收到消息时被系统调用. 
2>  `+(void)initialize; 方法`是通过消息机制被系统调用的, 也就是说会优先调用分类中的`+(void)initialize; 方法`.
3> 如果子类和父类中都有`+(void)initialize; 方法`, 不论 子类有没有主动调用父类的`+(void)initialize; 方法`, 当子类在第一次接收到消息时, 会先调用父类的`+(void)initialize; 方法` 在调用子类的`+(void)initialize; 方法`.<br>
![](/assets/Snip20190107_1.png)



<br><br>
**关于消息机制的说明:**
我们在Objective-C 中调用对象方法/ 类方法 其实都是通过消息机制来实现的,比如: `[NSObject alloc]` 就是在发消息: 

```objc_msgSend(objc_getClass("NSObject"), @selector(alloc));```


<br>
#### initialize 方法的作用
因为 `initialize` 方法是在类第一次接收消息前调用, 因此,如果我们想要在类 初始化之前做点事, 那么`initialize` 方法是一个不错的选择.


<br><br>
#### category initialize 面试题

**1、`+(void)laod; 方法 和 +(void)initialize; ` 方法的区别是什么?**

1>  `+(void)laod; 方法` 会在类/ 分类加载到内存时,系统通过load 方法的指针分别调用, 类/ 分类中的load 都会调用, 有多少个laod 就调用多少个
2>  `+(void)initialize; 方法` 会在类第一次接收到消息时, 被系统通过消息机制调用, 只调用一个.
3> `+(void)laod; 方法` 和  `+(void)initialize; 方法` 都只会调用一次.


2、`+(void)laod; 方法 和 +(void)initialize; ` 方法在category 中的调用顺序是什么?

3、`+(void)laod; 方法 和 +(void)initialize; ` 方法 在出现继承后,他们之间的调用过程是怎样的?


