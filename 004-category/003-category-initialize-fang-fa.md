#### category initialize 方法


<br>
#### initialize 方法调用时刻/ 调用机制/ 调用顺序
1>、`+(void)initialize; 方法` 会在类第一次接收到消息时被系统调用. 
2>  `+(void)initialize; 方法`是通过消息机制被系统调用的, 也就是说会优先调用分类中的`+(void)initialize; 方法`.
3> 如果子类和父类中都有`+(void)initialize; 方法`, 不论 子类有没有主动调用父类的`+(void)initialize; 方法`, 当子类在第一次接收到消息时, 会先调用父类(如果父类没调过)的`+(void)initialize; 方法` 再调用子类的`+(void)initialize; 方法`
4> 如果子类第一次接收消息是,子类没有initialize, 那么系统会调用父类的initialize来初始化子类,保证每个类在第一次接收消息时都会调用一次initialize方法.<br>
![](/assets/Snip20190107_1.png)

源码查看顺序
![](/assets/Snip20190107_2.png)


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
1> load 的方法是在类加载到内存 中是被系统调用,而initialize 是在类第一次接收到消息时调用, load 在  initialize 之前调用

3、`+(void)laod; 方法 和 +(void)initialize; ` 方法 在出现继承后,他们之间的调用过程是怎样的?
1> 类中的load 和分类中的load 都会被调用, 父类中的load比子类中的load 先调用, 类中的load 比分类中的load先调用.
2> initialize 是通过消息机制调用的, 分类中的initialize 会覆盖类中的initialize, 如果子类和父类中都有initialize, 父类中的initialize 先调用, 子类中的initialize 后调用

4、 load方法和initialize方法的最大区别是什么? 
1> load方法是通过方法指针直接调用的, 而initialize方法是通过消息机制调用的.
2> laod 方法是在类/ 分类加载到内存是调用的, 而initialize 方法是在类第一次接收到消息时调用的.
3> 每个类在第一次接收到消息后都会调用一次initialize 方法, 如果自己没有initialize, 就会调用父类的initialize. 即是说, 父类的initialize 方法可能被调用多次.
