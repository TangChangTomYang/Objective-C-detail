#### category 添加属性


#### 一、属性property 知识回顾
1、 当我们给某个类(比如: Person),添加一个属性时(@property (nonatomic ,assign) int age)时, 其实系统帮我们做了3件事情.
1> 添加一个属性( `int _age;` ).
2> 添加get 和set的声明( ` -(int)age; 和 -(void)setAge:(int)age;`)
3> 添加get 和 set的实现 
即:

![](/assets/Snip20190107_9.png)



<br><br>
#### Category 面试题

1 Category 能否添加成员变量? 如果可以怎么添加?


