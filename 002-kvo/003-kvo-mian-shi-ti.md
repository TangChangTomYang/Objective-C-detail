#### KVO 面试题

**1、iOS用什么方式实现对一个对象的KVO? (KVO 的本质是什么?)**
1> 利用runtime API 动态生成一个子类, 并修改 instance 的isa 指向这个子类
2> 重写 set方法/ -(Class)class 方法/ -dealloc 方法 等
3> 当调用set方法时,里面会调用Foundation 的NSSetxxxValueAndNotify 函数, NSSetxxxValueAndNotify内部会 调用 willChangeValueForKey/ 原类的set方法/ didChangeValueForKey (内部触发KVO)

<br>
**2、如何手动触发KVO?**

```
[self.person willChangeValueForKey:@"age"];
[self.person didChangeValueForKey:@"age"];
// 必须先调用 willChangeValueForKey 在 调用  didChangeValueForKey 才能 触发KVO 
```

<br>
**3、那些属性可以监听KVO**
只要有set方法的属性都可以监听

<br>
**4、 直接修改成员变量会触发KVO码? **
不会触发KVO. 但是通过KVC 直接修改会触发.
