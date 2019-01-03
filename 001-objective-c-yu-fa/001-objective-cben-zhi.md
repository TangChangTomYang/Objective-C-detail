#### 一、Objective-C 本质

1、我们平时编写的Objective-C代码, 底层实现都是基于C/C++ 代码实现的.所以Objective-C的面向对象都是基于C/C++数据结构实现的.
<br><br>
2、Objective-C中的对象、类主要是基于C/C++中的结构体数据结构实现的.
![](/assets/ocbz.png)

<br><br>

3、Objective-C 代码转换为C/C++
1> 方式1: (不推荐)
```
clang -rewrite-objc main.m -o main.cpp

不推荐的原因:
1. 不同的平台代码的实现上都是有差异的(windows/iphone/mac 等)
2. 虽然上面的指令能将Objective-C 代码转换成C/C++的代码, 
 但是我们不知道它底层转换出来的代码是针对哪个平台的
 (是windows呢?  还是 mac 还是 iphone), 因此不便于我们具体分析问题
3. 即时是同一个平台(比如: windows) 32位机器和64位机器,其执行的C/C++ 
代码都是有差异的,因此上面的代码不恰当
```

![](/assets/maincpp.png)
方式2:(推荐)
```
xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.c -o main.m

使用Xcode sdk 提供的代码工具, 将Objective 装换成指定架构下的C/C++ 代码
```
<br><br>

4、NSObject 的底层实现

![](/assets/ocdcsx.png)
从上面可以看出, NSObject 的底层实现就是C++中的结构体(其实在C++ 类和结构体的用法差异很小), 因此可以得出结论, 一个NSObject 对象占用的大小就是结构体内部成员(结构体指针)的大小(64位 8个字节)







