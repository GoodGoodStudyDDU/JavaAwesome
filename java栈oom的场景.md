一般情况下，java栈是很难发生oom的，通常发生的异常是StackOverFlow。
发生StackOverFlow的场景有如下两个：
1. 栈增长到无法继续分配内存
2. 栈增长到栈最大限制（由`-Xss`参数设置）

发生OOM的场景:
1. 新建线程时，没用空间为新线程的栈分配内存

OOM解决方法:
1. 减少`-Xss`
2. 减少线程数