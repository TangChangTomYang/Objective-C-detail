#### KVO 的底层实现/ 本质


**1、 KVO 的底层实现**
![](/assets/Snip20190106_1.png)

从上图我们可以发现:

1> instance 对象添加 KVO后, 系统会自动为当前的类派生一个 **NSKVONotifying_xxxx** 的子类, 并让当前实例对象的isa 指向这个 新的子类
2> 会重写 -(void)set方法/ -(Class)class 方法/  -(void)dealloc 方法等等.
3> 当我们修改设置了 KVO的属性值时, 就会调用子类的set方法,从而触发 KVO的监听方法
4> KVO 的本质是重写set,方法.

<br><br>
**2、 KVO 的底层实现应该是这样的**

```
#import "NSKVONotifying_Person.h"

@implementation NSKVONotifying_Person

// 添加了Person对象的ageKVO后, 系统会重写set方法
- (void)setAge:(int)age{

    //_NSSetxxxValueAndNotify();
    _NSSetIntValueAndNotify();
}

// 伪代码
void _NSSetIntValueAndNotify(){
    [self willChangeValueForKey:@"age"];
    [super setAge:age];
    [self didChangeValueForKey:@"age"];
}

- (void)didChangeValueForKey:(NSString *)key{
    // 通知监听器，某某属性值发生了改变
    [oberser observeValueForKeyPath:key ofObject:self change:nil context:nil];
}

@end
```



<br><br>
**3、 验证在添加KVO 后, 系统为原来的类动态派生了新的子类**
```
- (void)viewDidLoad {
    [super viewDidLoad];
    
    self.person1 = [[Person alloc] init];
    self.person1.age = 1;
    self.person2 = [[Person alloc] init];
    self.person2.age = 2;
    
    NSLog(@"添加KVO 前class:  %@",[self.person1 class]);
    NSLog(@"添加KVO 前class: %@",[self.person2 class]);
    
    NSLog(@"添加KVO 前object_getClass: %@",object_getClass(self.person1));
    NSLog(@"添加KVO 前object_getClass: %@",object_getClass(self.person2));
    
    // 给person1对象添加KVO监听
    NSKeyValueObservingOptions options = NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld;
    [self.person1 addObserver:self forKeyPath:@"age" options:options context:@"123"];
    
    NSLog(@"添加KVO  后 class:  %@",[self.person1 class]);
    NSLog(@"添加KVO  后 class: %@",[self.person2 class]);
    
    NSLog(@"添加KVO  后 object_getClass: %@",object_getClass(self.person1));
    NSLog(@"添加KVO  后 object_getClass: %@",object_getClass(self.person2));
    
    
    
// 打印结果:
添加KVO 前class: Person
添加KVO 前class: Person
添加KVO 前object_getClass: Person
添加KVO 前object_getClass: Person


添加KVO  后 class: Person
添加KVO  后 class: Person
添加KVO  后 object_getClass: NSKVONotifying_Person
添加KVO  后 object_getClass: Person
}
``` 






