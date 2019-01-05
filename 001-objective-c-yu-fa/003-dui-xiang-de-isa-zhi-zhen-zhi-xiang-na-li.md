#### 对象的 isa 指针指向哪里?


1、 OC 中方法调用的本质: 给某个对象(实例对象/ 类对象)发消息
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
2、oc 中 对象的 isa 的指针
![](/assets/Snip20190105_4.png)

1> 从上面的图我们可以看出, 实例对象(instance 对象)的isa 指针, 指向类对象(class 对象), 类对象(class 对象)的isa 指针指向 元类对象(meta-class)

2> 当我们调用对象方法时,其实是给实例对象(instance)发消息,实例对象(instance)通过自己的isa的指针找到类对象(class),最后找到类对象(class)中的方法调用.

3> 当我们调用类方法时,其实是给类对象(class)发消息,类对象(class)通过自己的isa的指针找到元类对象(meta-class),最后找到元类对象(meta-class)中的方法调用.


