#### 面试题
**1、通过KVC修改属性的值会不会触发 KVO?**

会触发.
<br>(1)、因为系统触发KVO的顺序是这样的
step1、 -(void)willChangeValueForKey:(NSString *)key;
step2、 修改 对应的值
step3、 -(void)didChangeValueForKey:(NSString *)key;
step4、 KVO  触发
<br>(2)、KVC 在设置前会先调用 `-(void)willChangeValueForKey:(NSString *)key;` 在`修改属性值` 修改值完成后再调用 `-(void)didChangeValueForKey:(NSString *)key;` 因此KVC 也会触发 KVO 

<br>
**2、KVC的赋值取值是怎样的过程? 原理是什么?**

**赋值过程:**
(1)、先查询 `setKey:` 方法, 有即调用,如没找到,再找 `_setKey:`方法有则调用.
(2)、如果第一步没完成, 则询问方法`accessInstanceVariablesDirectly` 是否可以访问成员变量,如果不能访问则调用`setValue:ForUndefineKey:` 并抛出异常 `NSUnkonwnKeyException`, 如果可以直接访问成员变量,则执行第3步:
(3)查找`_key`有则赋值,无 查找`_isKey`有则赋值, 无 查找`key`有则赋值, 无 查找 `isKey`有责赋值,无则调用`setValue:ForUndefineKey:` 并抛出异常 `NSUnkonwnKeyException`.



<br>
**取值过程:**
(1)、依顺序挨个查找`getKey/ key/ isKey/ _key`方法, 找到了则取值.
(2)、如果没找到则 访问方法`accessInstanceVariablesDirectly` 是否可以直接访问成员变量, 不能则调用 `valueForUndefinekey:` 并抛出异常.
(3)、能则依次按顺序查找成员变量`_key/ _isKey/ key/ isKey` 顺序访问成元变量, 没找到则调用 `valueForUndefinekey:` 并抛出异常.
