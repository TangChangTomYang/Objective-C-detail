#### 一、Objective-C (实例对象)本质

**1、我们平时编写的Objective-C代码, 底层实现都是基于C/C++ 代码实现的.所以Objective-C的面向对象都是基于C/C++数据结构实现的.**





<br> 
**2、Objective-C中的对象、类主要是基于C/C++中的结构体数据结构实现的.**
![](/assets/ocbz.png)





<br>
**3、Objective-C 代码转换为C/C++**

**1> 方式1: (不推荐)**
```
clang -rewrite-objc main.m -o main.cpp

不推荐的原因:
1>. 不同的平台代码的实现上都是有差异的(如: windows/iphone/mac 等)
2>. 虽然上面的指令能将Objective-C 代码转换成C/C++的代码, 
 但是我们不知道它底层转换出来的代码是针对哪个平台的
 (是windows呢?  还是mac, 还是 iphone), 因此不便于我们具体分析问题
3>. 即时是同一个平台(比如: windows) 32位机器和64位机器,其执行的C/C++ 
代码都是有差异的,因此上面的代码不恰当
```

![](/assets/maincpp.png)
**方式2:(推荐)**
```
xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.c -o main.m

使用Xcode sdk 提供的代码工具, 将Objective 装换成指定架构下的C/C++ 代码

如果要连接框架
xcrun -sdk iphoneos clang -arch arm64 -framework UIKit -rewrite-objc main.c -o main.m

使用: -framework UIkit 导入要使用的框架

```
<br><br>

**4、NSObject 的底层实现**

![](/assets/ocdcsx.png)
从上面可以看出, NSObject 的底层实现就是C++中的结构体(其实在C++ 中类和结构体的用法差异很小)


<br><br>
**5、 一个NSObject 对象占用多少内存**
1> 从第4点我们猜测一个NSObject对象的大小就是其底层实现的结构体的大小,因此可能是 8个字节(64位), 但是不一定对, 详解如下:
![](/assets/dxdx.png)

经过查看源码发现, NSObject 中一个对象最少会分配16字节(小于16时取16)




<br><br>
**6、关于结构体内存大小**
因为内存对齐的原因, 结构体的大小必须是最大成员大小的倍数
```
也就是说下面两个结构体的大小相同

struct person{
 Class isa; // 8
 int age; // 4
} // 内存对齐, 是8的倍数, 因此大小为16;

struct studen{
  Class isa; //8
  int age; //4 
  int num; // 4
} // 内存对齐, 是8 的倍数, 因此是16

```

<br><br>
**7、 关于实例对象的大小和分配内存大小的说明**
1> 获取实例对象大小(最大成员变量倍数)和sizeof()一样
![](/assets/Snip20190104_2.png)

2> 指针分配空间大小 (16 的倍数)
![](/assets/Snip20190104_3.png)

3> 详解如下:
```
#import <Foundation/Foundation.h>

#import <objc/runtime.h> //class_getInstanceSize
#import <malloc/malloc.h> // 指针堆分配空间大小

int main(int argc, const char * argv[]) {
    
    NSObject *obj = [[NSObject alloc] init];
    //1. 获取一个实例对象,成员对象占用的大小
    // 注意: class_getInstanceSize 获取的是实例对象成员的大小, 是内存对齐过后的大小
    // 结构体内存对齐的原则是, 结构体的大小必须是结构体内最大成员的倍数
    size_t instanceSize = class_getInstanceSize([obj class]);
    NSLog(@"NSObject 实例的大小 %zd",instanceSize);
    
    //2.获得obj 指针分配的大小(堆空间大小)
    //注意:  malloc_size  获取的是分配空间大小, 最小为16, 如果大于16 就是16的倍数
    size_t mallocSize = malloc_size((__bridge const void *)obj);
    NSLog(@" 获取 一个指针分配的内存大小 %zd",mallocSize);
  
    return 0;
}
```

mallocSize 分配内存的对齐方式(16 的 倍数)
![](/assets/Snip20190104_6.png)


<br><br>
**8、Objective-C 中类的实例例对象的大小说明**

如下图:
![](/assets/Snip20190104_5.png)

在 Objective-C 中类的实例对象中只存储的是成员变量, 对象方法和类方法不会存储在实例对象中, 对象方法存储在类对象中一个类只有一份, 类方法存储在元类对象中一个类对象只有一个元类对象.



<br><br>

**总结:**
(1)、一个结构体的大小是结构体内最大的成员变量的倍.
(2)、一个结构体或者对象,系统分配大大小是 16 的整数倍.
(3)、一个结构体或者对象,系统分配大大小, 至少16.





