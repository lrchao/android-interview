### volatile的使用及其原理

volatile关键字就是Java中提供的另一种解决可见性和有序性问题的

一，使用  
1.防止重排序

~~~java
public class VolatileTest3 {
    int a = 0;
    boolean flag = false;

    public void write(){
        a = 1;                 // 1
        flag = true;           // 2
    }

    public void read(){
        if(flag){               // 3
            int i = a*a;        // 4
            System.out.println("i的值为："+i);
        }

    }
}
~~~

现在我们分析一下为什么要在变量singleton之间加上volatile关键字。要理解这个问题，先要了解对象的构造过程，实例化一个对象其实可以分为三个步骤：  
（1）分配内存空间。  
（2）初始化对象。   
（3）将内存空间的地址赋值给对应的引用。  

但是由于操作系统可以对指令进行重排序，所以上面的过程也可能会变成如下过程：

（1）分配内存空间。    
（2）将内存空间的地址赋值给对应的引用。      
（3）初始化对象    

如果是这个流程，多线程环境下就可能将一个未初始化的对象引用暴露出来，从而导致不可预料的结果。因此，为了防止这个过程的重排序，我们需要将变量设置为volatile类型的变量。

2.实现可见性

~~~java
public class VolatileTest implements Runnable {
    private boolean flag = false;
    @Override
    public void run() {
        while (!flag){

        }
        System.out.println("线程结束运行...");
    }
    public void setFlag(boolean flag) {
        this.flag = flag;
    }
    public static void main(String[] args) throws InterruptedException {
        VolatileTest v = new VolatileTest();
        Thread t1 = new Thread(v);
        t1.start();
        Thread.sleep(2000);
        v.setFlag(true);
    }
}
~~~

可以看到尽管在代码中调用了v.setFlag(false)方法，线程也没有结束运行。这是因为在上面的代码中，
实际上是有2个线程在运行，一个是main线程，一个是在main线程中创建的t1线程。因此我们可以看到在线程中的变量是互不可见的。 要理解线程中变量的可见性，我们需要先理解Java的内存模型。

二，原理    

![](../../picture/1_8yAaGbe5EFEaD7Ze_4zDGg.jpeg)

1.可见性实现

（1）修改volatile变量时会强制将修改后的值刷新的主内存中。   
（2）修改volatile变量后会导致其他线程工作内存中对应的变量值失效。因此，再读取该变量值的时候就需要重新从读取主内存中的值。     

通过这两个操作，就可以解决volatile变量的可见性问题。

2.有序性实现   

为了实现volatile内存语义，编译器在生成字节码时，会在指令序列中插入内存屏障来禁止特定类型处理器的重排序，在JMM中，内存屏障的插入策略如下：

- 在每个volatile写操作之前插入一个StoreStore屏障
- 在每个volatile写操作之后插入一个StoreLoad屏障
- 在每个volatile读操作之后插入一个LoadLoad屏障
- 在每个volatile读操作之后插入一个LoadStore屏障

StoreStore屏障可以保证在volatile写之前，前面所有的普通读写操作同步到主内存中   
StoreLoad屏障可以保证防止前面的volatile写和后面有可能出现的volatile度/写进行重排序    
LoadLoad屏障可以保证防止下面的普通读操作和上面的volatile读进行重排序    
LoadStore屏障可以保存防止下面的普通写操作和上面的volatile读进行重排序   

![](../../picture/1_29q1Bbuy6ZogCx_suaLaGQ.jpeg)

三，总结

必须同时满足下面两个条件才能保证在并发环境的线程安全：

（1）对变量的写操作不依赖于当前值。      
（2）该变量没有包含在具有其他变量的不变式中。   

