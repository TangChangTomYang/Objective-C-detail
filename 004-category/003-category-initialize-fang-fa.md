#### category initialize 方法


<br>
#### initialize 方法调用的机制说明
1>、`+(void)initialize; 方法` 会在类第一次接收到消息时被系统调用. 
2>  `+(void)initialize; 方法`是通过消息机制被系统调用的, 也就是说会优先调用分类中的`+(void)initialize; 方法`.
3> 如果子类和父类中都有`+(void)initialize; 方法`, 不论 子类有没有主动调用父类的`+(void)initialize; 方法`, 当子类在第一次接收到消息时, 会先调用父类的`+(void)initialize; 方法` 在调用子类的`+(void)initialize; 方法`.<br>
![](/assets/Snip20190107_1.png)



<br><br>
**关于消息机制的说明:**
我们在Objective-C 中调用对象方法/ 类方法 其实都是通过消息机制来实现的,比如: `[NSObject alloc]` 就是在发消息: 

```objc_msgSend(objc_getClass("NSObject"), @selector(alloc));```


<br><br>
#### category initialize 面试题

1、`+(void)laod; 方法 和 +(void)initialize; ` 方法的区别是什么?

2、`+(void)laod; 方法 和 +(void)initialize; ` 方法在category 中的调用顺序是什么?

3、`+(void)laod; 方法 和 +(void)initialize; ` 方法 在出现继承后,他们之间的调用过程是怎样的?


