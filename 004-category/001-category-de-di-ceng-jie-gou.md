####  Category 的底层结构实现

**1、 类的每一个分类, 在编译后都会生成一份对应的C++ 文件,存储在内存中, 在整个程序的运行过程中有且只有一份**

下面是给`Person` 这个类写的一个分类,经过重写C++代码后的展示如下: 
![](/assets/Snip20190106_12.png)

从上面可以看出,每个分类都有一个自己的静态变量一直在内存中.

<br>
**2、 Category 中的 `对象方法/ 类方法/ 协议/ 属性`等, 是在程序执行过程中经过 runtime API 动态的 连接到对应的类中, 这样在类中才访问到 category 中的方法, 而不是程序一启动就加载.**


<br>
**3、Category 中的实例方法/ 类方法/ 协议/ 属性 的加载处理过程**
1> 通过 Runtime 加载某个类的所有的Category 数据
2> 把所有的Category 的方法/ 属性/ 协议数据, 合并到一个大的数组中.
3> 将合并后的分类数据(方法数组/ 属性数组/ 协议数组), 插入到原来类的数据前面(方法数据组前面/ 属性数组前面/ 协议数组前面).
4> 合并完后, 原来的类的数据数组(方法数组/ 属性数组/ 协议数组) 就变成了一个大数组, 大数组里面分类的数据就在最前面, 因此调用方法时, 分类方法会覆盖类的方法.

<br>
**4、源码的解读顺序:**
![](/assets/Snip20190106_13.png)


附加分类数据
```
// Attach method lists and properties and protocols from categories to a class.
// Assumes the categories in cats are all loaded and sorted by load order, 
// oldest categories first.
static void 
attachCategories(Class cls, category_list *cats, bool flush_caches)
{
    if (!cats) return;
    if (PrintReplacedMethods) printReplacements(cls, cats);

    bool isMeta = cls->isMetaClass();

    // fixme rearrange to remove these intermediate allocations
    method_list_t **mlists = (method_list_t **)
        malloc(cats->count * sizeof(*mlists));
    property_list_t **proplists = (property_list_t **)
        malloc(cats->count * sizeof(*proplists));
    protocol_list_t **protolists = (protocol_list_t **)
        malloc(cats->count * sizeof(*protolists));

    // Count backwards through cats to get newest categories first
    int mcount = 0;
    int propcount = 0;
    int protocount = 0;
    int i = cats->count;
    bool fromBundle = NO;
    while (i--) {
        auto& entry = cats->list[i];

        method_list_t *mlist = entry.cat->methodsForMeta(isMeta);
        if (mlist) {
            mlists[mcount++] = mlist;
            fromBundle |= entry.hi->isBundle();
        }

        property_list_t *proplist = 
            entry.cat->propertiesForMeta(isMeta, entry.hi);
        if (proplist) {
            proplists[propcount++] = proplist;
        }

        protocol_list_t *protolist = entry.cat->protocols;
        if (protolist) {
            protolists[protocount++] = protolist;
        }
    }

    auto rw = cls->data();

    prepareMethodLists(cls, mlists, mcount, NO, fromBundle);
    rw->methods.attachLists(mlists, mcount);
    free(mlists);
    if (flush_caches  &&  mcount > 0) flushCaches(cls);

    rw->properties.attachLists(proplists, propcount);
    free(proplists);

    rw->protocols.attachLists(protolists, protocount);
    free(protolists);
}
```
附加分类具体数据(将数据插入)
```
    void attachLists(List* const * addedLists, uint32_t addedCount) {
        if (addedCount == 0) return;

        if (hasArray()) {
            // many lists -> many lists
            uint32_t oldCount = array()->count;
            uint32_t newCount = oldCount + addedCount;
            setArray((array_t *)realloc(array(), array_t::byteSize(newCount)));
            array()->count = newCount;
            memmove(array()->lists + addedCount, array()->lists, 
                    oldCount * sizeof(array()->lists[0]));
            memcpy(array()->lists, addedLists, 
                   addedCount * sizeof(array()->lists[0]));
        }
        else if (!list  &&  addedCount == 1) {
            // 0 lists -> 1 list
            list = addedLists[0];
        } 
        else {
            // 1 list -> many lists
            List* oldList = list;
            uint32_t oldCount = oldList ? 1 : 0;
            uint32_t newCount = oldCount + addedCount;
            setArray((array_t *)malloc(array_t::byteSize(newCount)));
            array()->count = newCount;
            if (oldList) array()->lists[addedCount] = oldList;
            memcpy(array()->lists, addedLists, 
                   addedCount * sizeof(array()->lists[0]));
        }
    }
```
