#### 对象的 isa 指针指向/ superclass 指向哪里



<br>
#### 一、isa 指针

**1、 OC 中方法调用的本质: 给某个对象(实例对象/ 类对象)发消息**
```
@interface Person : NSObject{
    @public
    int _age;
}
-(void)instanceMethod;
+(void)classMethod;

@end

@implementation Person
-(void)instanceMethod{
    NSLog(@" 实例方法 调用");
}

-(void)instanceMethodCount:(int)count{
    NSLog(@" 实例方法 计数");
}


+(void)classMethod{
     NSLog(@" 类方法 调用");
}

+(void)classMethodCount:(int)count{
    NSLog(@"类方法 计数");
}
@end


int main(int argc, const char * argv[]) {
    @autoreleasepool {
        
        Person *pson  = [[Person alloc]init];
        pson -> _age = 10;
        
        //OC 中方法调用的本质, 其实就是给对象发消息
        [pson instanceMethod];
        //((void (*)(id, SEL))(void *)objc_msgSend)((id)pson, sel_registerName("instanceMethod"));
        //objc_msgSend(pson, sel_registerName("instanceMethod"));
        
        [pson instanceMethodCount:5];
        //((void (*)(id, SEL, int))(void *)objc_msgSend)((id)pson, sel_registerName("instanceMethodCount:"), 5);
        //objc_msgSend(pson, sel_registerName("instanceMethodCount:"), 5);
        
        
        [Person classMethod];
        //objc_getClass("Person") runtime API , 通过类名获取对应的类对象, 等价于 [Person class]
        //((void (*)(id, SEL))(void *)objc_msgSend)((id)objc_getClass("Person"), sel_registerName("classMethod"));
        //objc_msgSend(objc_getClass("Person"), sel_registerName("classMethod"));
        
        [Person classMethodCount:5];
        //((void (*)(id, SEL, int))(void *)objc_msgSend)((id)objc_getClass("Person"), sel_registerName("classMethodCount:"), 5);
        //objc_msgSend(objc_getClass("Person"), sel_registerName("classMethodCount:"), 5);
        
        
    }
    return 0;
}
```


<br> <br>
**2、oc 中 对象的 isa 指针**
<br>
![](/assets/Snip20190105_4.png)

0> OC中的3中对象(实例对象/ 类对象/ 元类对象) 都有isa 指针, 只有实例对象中有成员变量

1> 从上面的图我们可以看出, 实例对象(instance 对象)的isa 指针, 指向类对象(class 对象), 类对象的isa 指向元类对象

2> 类对象(class 对象)的isa 指针指向 元类对象(meta-class)

3> 当我们调用对象方法时,其实是给实例对象(instance)发消息,实例对象(instance)通过自己的isa的指针找到类对象(class),最后找到类对象(class)中的方法调用.

4> 当我们调用类方法时,其实是给类对象(class)发消息,类对象(class)通过自己的isa的指针找到元类对象(meta-class),最后找到元类对象(meta-class)中的方法调用.


<br>
#### 二、类对象的 superclass 指针

![](/assets/Snip20190105_5.png)

1、从上面可以看出, 每个class对象都有一个superclass 指针, 指向的是它的父类.
2、class对象的superclass指向的是其父类的class对象
3、superclass 的作用就一个, 就是查找父类




<br>
#### 三、类对象的 superclass 指针

![](/assets/Snip20190105_6.png)
1、从上面可以看出, 每一个元类对象(meta-class) 都有一个superclass.
2、meta-class 对象的superclass指针指向其父类的meta-class
3、superclass 的作用就是查找父类    




<br>
#### 四、 isa / superclass 指针总结

![](/assets/isasuperclass.png)

0> 实例对象只有isa 指针, 类对象和元类对象既有isa指针又有superclass指针.

1> 从上面的图可以看出, 实例对象的isa 指向的是 类对象, 类对象的isa 指针,指向元类对象, 元类对象的isa 都指向元类对象基类.

1.1> 注意: 虽然实例对象的 isa 指向 类对象, 类对象的 isa 指向 元类对象, 但是都不是直接指向.
 **查源码发现:**
实例对象的isa & ISA_MASK = 类对象地址
类对象的isa & ISA_MASK = 元类对象地址
![](/assets/Snip20190105_7.png)

2> 类对象的superclass 指针指向父类的类对象, 元类对象的superclass指针指向父类的 元类对象, 元类对象的基类的superclass 指向 类对象的基类.



