#### OC对象分类(实例对象、类对象、元类对象)


<br>
#### 一、实例对象

**1、 instance 实例对象**
**instance对象就是通过 类 alloc 出来的对象,每次alloc 都会产生一个新的对象, 不同的instance对象内存地址是不同的.**
```
NSObject *obj1 = [[NSObject alloc] init];
NSObject *obj2 = [[NSObject alloc] init];
```
**obj1 和 obj2 都是 NSObject 的实例对象, 他们占用不同的内存**


<br>
**2、 instance 对象在内存中存储的信息包括:**
- **1> isa 指针(因为所有的实例对象都继承自NSObject)**
- **2> 其它所有的成员变量**
- **3> 不包含类方法和对象方法**



****
<br>
#### 二、 类对象 

**1、什么是类对象?**
**类对象就是用来描述 实例对象的对象, 在内存中,类对象有且只有一份**


<br>
**2、获取类对象的方法(3种)**

```
NSObject *obj = [[NSObject alloc] init];
Class objCls = [obj class];
Class objCls3 = object_getClass(obj);
Class objCls2 = [NSObject class];

// 0x7fffae929140, 0x7fffae929140, 0x7fffae929140
NSLog(@"%p, %p, %p", objCls,objCls2,objCls3);
// 从上面的打印可以看出, 每个类在内存中有且只有一个类对象(Class 对象)
```


<br>
**3、 类对象中存储的信息主要包含有:**
- **1> isa 指针**
- **2> superClass 指针**
- **3> 属性信息(@property)**
- **4> 实例对象方法(instance method) 带`-` 的方法**
- **5> 类的协议信息(@protocol)**
6> 类的成员变量信息(ivar) , 比如: 成员变量的类型, 名称等等





****
<br>
#### 三、 元类对象(meta-class)

**1、获取某个类的元类对象(meta-class)**
```
#import <objc/runtime.h>
// 获取类 对象的几种方式
NSObject obj = [[NSObject alloc] init];

Class objCls1 = [NSObject class];
Class objCls2 = [obj class];
Class objCls3 = object_getClass(obj);

// 通过类 获取元类对象(meta-class),把类对象作为参数,通过运行时方法获取元类对象
Class MetaCls1 = object_getClass(objCls1);
Class MetaCls2 = object_getClass(objCls2);
Class MetaCls3 = object_getClass(objCls3);

每个类,在内存中有且仅有一个 元类对象(meta-class)

// 运行时方法,判断一个类是否为元类对象

class_isMetaClass(cls);
```

<br>
**2、类对象和元类对象的内存结构是一样的**

**注意:** 
虽然类对象和元类对象的内存结构是一样的,但是并并能说明有一样的数据

**3、元类对象中存储的信息主要有**
- **1> isa 指针**
- **2> superclass 指针**
- **3> 类方法(classMethod) , `+` 开头的方法.**
![](/assets/Snip20190104_9.png)



****
<br>
#### 四、 object_getClass()内部实现


```
runtime API
// 更具名字(类的名子)获取Class
Class objc_getClass(const char * aClassName);

//更具一个对象(实例对象 或 类对象)获取Class, 通过实例对象获取的是类对象, 通过类对象获取的是元类对象
Class object_getClass(id  obj) ;
```

![](/assets/Snip20190105_2.png)
<br>
![](/assets/Snip20190105_1.png)













****
<br><br>
#### 五、 Objective-C 中3种类型的对象中存储的信息


![](/assets/Snip20190105_3.png)


































































































































































































