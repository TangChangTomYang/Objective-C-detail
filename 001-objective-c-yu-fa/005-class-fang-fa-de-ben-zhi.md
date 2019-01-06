#### class 方法的本质

![](/assets/Snip20190105_4.png)

其实, 上面的isa 的指向反应的是 runtime中的 **(Class object_getClass(id obj)** API 的真实指向.

我们平时调用的方法 **- (Class)class; ** 内部实现应该是如下:

```
- (Class)class{
    // 通过isa 指针获取类
    return object_getClass(self);// 相当与 self->isa
}
```

而 **+ (Class)class; **  的底层实现应该是

```
+ (Class)class{
    // 通过一个名字来获取一个类
    objc_getClass("Person");
}
```


