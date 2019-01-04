#### OC对象分类


1、 instance 实例对象

instance对象就是通过 类 alloc 出来的对象,每次alloc 都会产生一个新的对象.

```
NSObject *obj1 = [[NSObject alloc] init];
NSObject *obj2 = [[NSObject alloc] init];
```
obj1 和 obj2 都是NSObject 的实例对象, 他们占用不同的内存


<br><br>
2、 instance 对象在内存中存储的信息包括:
1> isa 指针(因为所有的实例对象都继承自NSObject)
2> 其他所有的成员变量
3> 不包含类方法和对象方法