1. hashmap实现

* jdk1.8之前（不包括）:

采用数组方式实现，hash冲突算法采用拉链法，扩容采用全map rehash的方式

* jdk1.8及之后:

采用数据方式实现，hash冲突算法采用拉链法（链长大于8时，会进行树形化转换为红黑树，树节点小于6时进行反树形化），扩容分为两个步骤：链重建和链迁移，链重建将原先的链分成两个链，一个链依然为原hash槽的链，另一个链为原hash值两倍槽的链，直接迁移即可。

2. hashmap容量为2^n原因
这个和hash函数有关，一般hash函数都采用取余算法，但是取余计算较慢，对于2^n的模来说，可以直接采用按位与运算，得到余数。按位与运算显然比取余性能更好。

3. hashtable与hashmap区别
hashtable是同步版本的hashmap，但是方法同步都采用的synchronized关键字进行同步，性能较差。一般同步map推荐使用concurrenthashmap。

4. concurrenthashmap实现方式，1.7与1.8的区别
1.7采用分段锁，1.8使用cas+synchronized