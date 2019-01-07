#### category 添加属性

<br>
#### 一、属性 @property 知识回顾
<br>
1、 当我们给某个类(比如: Person),添加一个属性时(@property (nonatomic ,assign) int age)时, 其实系统帮我们做了3件事情.
1> 添加一个属性( `int _age;` ).
2> 添加get 和set的声明( ` -(int)age; 和 -(void)setAge:(int)age;`)
3> 添加get 和 set的实现 
即:

![](/assets/Snip20190107_9.png)


2、 其实分类里面也是由属性 @property 的
1> 当我们给某个分类(比如: Person(test)),添加一个属性时(@property (nonatomic ,assign) int age)时, 系统只会帮我们做了1件事情--添加get 和 set方法的声明

即: 

![](/assets/Snip20190107_13.png)

<br><br>
#### Category 面试题

1 Category 能否添加成员变量? 如果可以怎么添加?


