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