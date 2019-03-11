#### 一、Objective-C (实例对象)本质

**1、我们平时编写的Objective-C代码, 底层实现都是基于 C/C++ 代码实现的. 所以 Objective-C 的面向对象都是基于 C/C++ 数据结构实现的.**


<br> 
**2、Objective-C 中的对象、类 主要是基于 C/C++ 中的 结构体 实现的.**
![](/assets/ocbz.png)


<br>
**3、Objective-C 代码转换为C/C++**

- **1> 方式1: (不推荐)**

 ```
 clang -rewrite-objc main.m -o main.cpp
 ```
 
 **不推荐的原因:**
 - 1>. 不同的平台,代码的实现上都是有差异的(如: windows/iphone/mac 等)
 - 2>. 虽然上面的指令能将 Objective-C 代码转换成C/C++的代码, 但是我们不知道它底层转换出来的代码是针对哪个平台的 (是windows呢?  还是mac, 还是 iphone), 因此不便于我们具体分析问题
 - 3> 即使是同一个平台(比如: windows), 32位机器和64位机器,其执行的 C/C++ 代码都是有差异的,因此上面的代码不恰当
 ![](/assets/maincpp.png)

<br>
- **方式2:(推荐)**

 ```
 xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.c -o main.m
 
 使用Xcode sdk 提供的代码工具, 将Objective 转换成指定架构下的C/C++ 代码
 
 如果要连接框架
 xcrun -sdk iphoneos clang -arch arm64 -framework UIKit -rewrite-objc main.c -o main.m
 
 使用: -framework UIkit 导入要使用的框架
 
 ```




<br><br>
**4、NSObject 的底层实现(NSObject 的底层布局)**

![](/assets/ocdcsx.png)

**从上面可以看出, NSObject 的底层实现就是C++中的结构体
(其实在 C++ 中, 类class 和 结构体struct 的用法差异很小,几乎等同, 只是默认访问权限不同而已 )**


<br>
**5、 一个NSObject 对象最少占用多少内存**

- **1> 从第4点我们猜测一个 NSObject对象的大小就是其底层实现的结构体的大小, 因此可能是 8个字节(64位), 但实际占用16字节, 详解如下:**

 ![](/assets/dxdx.png)
 <br>
**经过查看源码发现, CoreFoudaiton 规定 一个 NSObject 对象最少会分配16字节(小于16时取16)**
- **2> 其实我们也可以通过Xcode 的内存查看工具来验证, 一个对象的大下`debug -> debug workflow -> viewMemory(查看对象内存)`**
![](/assets/Snip20190311_1.png)



<br>
**6、关于结构体内存大小**

- **因为内存对齐的原因, 结构体的大小必须是最大成员大小的倍数<br>(比如: 下面两个结构体的大小相同)**

```


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



<br>
**7、 关于实例对象的大小 和 分配内存大小的说明**

- **1> 获取实例对象成员大小和 `sizeof()` 一样(是最大成员变量大小的倍数)**
![](/assets/Snip20190104_2.png)

- **2> 获取指针分配空间大小 (16 的倍数)**
![](/assets/Snip20190104_3.png)

- **3> 详解如下:**

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

- **4> mallocSize 分配内存的对齐方式(16 的 倍数)**
![](/assets/Snip20190104_6.png)




<br>
**8、Objective-C 中类的实例对象的大小说明**

如下图:
![](/assets/Snip20190104_5.png)

- **1>  结构体的大小是结构体内最大的成员变量的倍.**
- **2> 一个结构体 或者 对象,系统分配大大小是 16 的整数倍.**
- **3> 实例对象中只有成员变量, 类方法存储在类对象中,对象方法存储在元类对象中**
- **4>  一个类可以有多个实例对象,但是有且只有一个类对象和一个元类对象.**





