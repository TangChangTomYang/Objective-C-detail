#### block


#### 一、 block 基础回顾

1、 什么是block, block 的几种使用情景

```
^{
    //block 要执行的代码
}; // 这就是一个最简单的block, 无参数block

^(int a, int b){
    // 使用a 
    // 使用b
}; // 这时一个带有参数的block
```

![](/assets/Snip20190109_1.png)





<br>
#### 二、 block的本质

1、 block 本质上也是一个oc 对象, 它的内部也有一个 isa 指针.
2、 block 是一个封装了函数调用,以及函数调用环境的OC 对象