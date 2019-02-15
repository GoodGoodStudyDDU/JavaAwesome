#### 1. 实现原理
`Thread`类包含一个`threadLocals`字段，类型为`ThreadLocal.ThreadLocalMap`
`ThreadLocal.ThreadLocalMap`使用一个`Entry`数组来存储`ThreadLocal`变量及它的值
`Entry`继承自`WeakReference`，包含一个对`ThreadLocal`变量的弱引用以及`ThreadLocal`的值
每次调用`ThreadLocal::set()`时，实际上是调用`ThreadLocal.ThreadLocalMap::set()`

> Q: 为什么使用`WeakReference`
> A: 这样做可以让已经没有在代码中被强引用的`ThreadLocal`变量被GC回收。开发人员创建的`ThreadLocal`变量，会被`threadLocals`以map的形式保存下来，如果`Entry`不采用`WeakReference`，将会导致map中的`Entry`对象全部为强引用，从而导致`Entry`不会被GC回收（即使开发人员已经将`ThreadLocal`变量设置为`null`），这种情况将持续到`Thread`对象被回收(或者显示调用`ThreadLocal::remove()`）。

`InheritableThreadLocal`类，顾名思义，可以被继承的`ThreadLocal`，存储在`Thread`类的`inheritableThreadLocals`字段，在线程创建子线程时进行继承，采用浅拷贝的方式（`ThreadLocal`中引用对象的字段的变化会导致所有线程都变化），这里有潜在的并发问题。
如下：
```java
static class IntWrapper{
    int value;
    public IntWrapper(int v){
        this.value = v;
    }
    public int getValue() {
        return value;
    }
    public void setValue(int value) {
        this.value = value;
    }
    @Override
    public String toString() {
        return getValue()+"";
    }
}
public static void main(String[] args) throws InterruptedException {
    InheritableThreadLocal<IntWrapper> counter = new InheritableThreadLocal<>();
    Semaphore semaphore = new Semaphore(0);
    counter.set(new IntWrapper(1));
    Thread t0 = new Thread(()->{
        IntWrapper intWrapper = counter.get();
        intWrapper.setValue(2);
        System.out.println("t0 " + intWrapper.getValue());
        semaphore.release(1);
    });
    Thread t1 = new Thread(()->{
        System.out.println("t1 " + counter.get());
    });
    t0.start();
    semaphore.acquire();
    t1.start();
    while (t1.getState() != Thread.State.TERMINATED);
    System.out.println("main " + counter.get());
    System.out.println("finish");
}
/*
输出结果为：
t0 2
t1 2
main 2
finish
*/
```


#### 2. 使用场景
可以用于存储业务无关的全局变量