#### category initialize 方法

1、`+(void)initialize; 方法` 会在类第一次接收到消息时调用. 也就是说在第一次调用类方法或者对象方法时,就会调用initialize 方法.

1> 什么是消息, 我们在Objective-C 中调用对象方法/ 类方法 其实都是通过消息机制来实现的,比如: `[NSObject alloc]` 就是在发消息: `objc_msgSend(objc_getClass("NSObject"), @selector(alloc));



#### category initialize 面试题

1、`+(void)laod; 方法 和 +(void)initialize; ` 方法的区别是什么?

2、`+(void)laod; 方法 和 +(void)initialize; ` 方法在category 中的调用顺序是什么?

3、`+(void)laod; 方法 和 +(void)initialize; ` 方法 在出现继承后,他们之间的调用过程是怎样的?


