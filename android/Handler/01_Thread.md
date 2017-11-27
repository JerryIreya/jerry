# 一、进程和线程概念

## (一)、进程

> 进程是资源分配的最小单位，而线程是CPU调度的最小单位。一个进程中最少有一个线程，名叫主线程。进程是程序执行的一个实例，比如说10个用户同时执行Chrome浏览器，那么就有10个独立的进程(尽管它们共享同一个相同的代码)。

### 1、进程的特点

每个进程都有自己独立的一块内存空间、一组系统资源。其内部数据和状态都是独立的。多进程可以提高CPU的效率，在同一个时间内执行多个程序，即并发执行。但是从严格意义上讲，也不是绝对的同一时刻执行多个程序，只不过CPU在执行时，通过时间片等调度算法在不同进程间高速切换。

>所以总结来说：进程由操作系统调度，简单而且稳定，进程之间的隔离性好，一个进程的崩溃不会影响其他进程的执行。

### 2、进程的缺点

>一般来说，进程消耗的内存比较大，进程切换代价很高，进程切换也像线程切换一样，需要保持上一个进程的上下文环境。比如在web编程中，如果一个进程处理一个请求的话，如果要提高并发量，就要增加进程数，而进程数量受内存和切换代价限制。

## (二)、线程

>线程是进程的一个实体，是CPU调度和分配的基本单位，它是比进程更能独立运行的基本单位，线程自己基本上不拥有系统资源，只拥有一点在运行中不可缺少的资源(如：程序计数器、栈)，但是它可与同属一个进程的其他线程共享进程所拥有的全部资源。

同类的多线程共享同一块内存空间和一组系统资源，线程本身的数据通常只有CPU的寄存器数据，以及一个供程序执行的堆栈。线程在切换时负荷小(和进程切换相比)，因此，线程也称为轻负荷进程。一个进程中可以包含多个线程。

在JVM中，本地方法栈、虚拟机栈和程序计数器是线程隔离的，而堆区和方法区是线程共享的。

## (三)、进程线程的区别

>地址空间：线程是进程内的一个执行单元;进程至少有一个线程;一个进程内的多线程它们共享进程的地址空间;进程自己拥有独立的地址空间。
> 资源拥有：进程是资源分配和拥有的单位，同一进程内的多个线程共享进程的资源。
>线程是CPU调度的基本单位，进程是资源分配的单位。
>二者均可并发执行

* 并发：多个事件在同一时间段内一起执行
* 并行：多个事件在同一时间同时执行

## (四)、多线程


为了进一步提高CPU的利用率，多线程便诞生了。一个程序可以运行多个线程，多个线程可以同时执行，从整个应用角度上看，这个应用好像肚子拥有多个CPU一样。虽然多线程进一步提高了应用的执行效率，但是由于线程之间会共享内存资源，这会导致一些资源同步的问题，另外，线程之间切换也会对资源有所消耗。

这里需要注意的是，如果一台电脑只有一个CPU核心，那么多线程也并没有真正的"同时"运行，它们之间需要通过相互切换来共享CPU核心，所以，只有一个CPU核心的情况下，多线程不会提高应用效率。但是，现在计算机一般都会有多个CPU，并且每个CPU可能还会有多个核心，所以现在硬件资源条件下，多线程编程可以极大地提高应用的效率。

![](http://upload-images.jianshu.io/upload_images/5713484-0f7a599138b3862f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在Java程序中，JVM负责线程的调度。线程调度是按照特定的机制为多个线程分配CPU的使用权。

调度的模式有两种：分时调度和抢占式调度。分时调度是所有线程轮流获得CPU使用权，并平均分配每个线程占用CPU的时间;抢占式调度是根据线程的优先级别来获取CPU的使用权。JVM的线程调度模式采用了抢占式模式。

# 二、Android线程的实现

>Android线程，一般地就是指Android虚拟机线程，而虚拟机线程是由通过系统调用而创建的Linux线程。纯粹的Linux线程与虚拟机线程区别在于虚拟机线程具有运行Java代码的Runtime。

在Android中，我们经常需要启动一个新的线程，这些线程大多从Thread这个类继承

## (一)、Thread类

[Thread类](http://androidxref.com/6.0.1_r10/xref/libcore/libart/src/main/java/java/lang/Thread.java)

    public class Thread implements Runnable {
      .....
    }

通过上面代码，我们知道Thread类实现了 [Runnable](http://androidxref.com/6.0.1_r10/xref/libcore/luni/src/main/java/java/lang/Runnable.java) ，侧面也说明线程是"可执行的代码"。

    public  interface Runnable {
      public abstract void run();
    }

Runnable是一个接口类，唯一提供的方法就是run()。

### 1、Thread的使用

一般情况下，我们是这样使用Thread的

#### (1)、继承Thread

    public  MyThread extends Thread{
    }
    MyThread  mt=new MyThread();
    mt.start();

#### (2)、直接使用Runnable

Thread的关键就是Runnable，因此下面是另一个常见的用法

    new Thread(Runnable runnable).start();

### 2、Thread的常用方法

![](http://upload-images.jianshu.io/upload_images/5713484-4657ce630772034a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3、Thread的常用字段

    volatile ThreadGroup group;
    volatile boolean daemon;
    volatile String name;
    volatile int priority;
    volatile long stackSize;
    Runnable target;
    private static int count = 0;

    /**
     * Holds the thread's ID. We simply count upwards, so
     * each Thread has a unique ID.
     */
    private long id;

    /**
     * Normal thread local values.
     */
    ThreadLocal.Values localValues;  

* **group** :每一个线程都属于一个group，当线程被创建时，就会加入一个特定的group，当线程运行结束，会从这个group移除。  
* **deamon**:当前线程是不是守护线程，守护线程只会在没有非守护线程运行下才会运行  
* name：线程名称  
* priority：线程优先级，Thread的线程优先级取值范围为[1,10],默认优先级为5  
* stackSize：线程栈大小，默认是0，即使用默认的线程栈大小(由dalvik中的全局变量gDvm.stackSize决定)  
* target：一个Runnable对象，Thread的run()方法中会转调target的run()方法，这是线程真正处理事务的地方  
* id：线程id，通过递增的count得到该id，如果没有显示给线程设置名字，那么就会使用Thread+id作为当前线程的名字。注意这里不是真正的线程id，即在logcat中打印的tid并不是这个id，那tid是指dalvik线程id  
* localValues：本地线程存储(TLS)数据

### 4、create()方法

Thread一共有9个构造函数，其中8个里面最终都是调用了create()方法

在Thread.java 402行

    /**
     * Initializes a new, existing Thread object with a runnable object,
     * the given name and belonging to the ThreadGroup passed as parameter.
     * This is the method that the several public constructors delegate their
     * work to.
     *
     * @param group ThreadGroup to which the new Thread will belong
     * @param runnable a java.lang.Runnable whose method <code>run</code> will
     *        be executed by the new Thread
     * @param threadName Name for the Thread being created
     * @param stackSize Platform dependent stack size
     * @throws IllegalThreadStateException if <code>group.destroy()</code> has
     *         already been done
     * @see java.lang.ThreadGroup
     * @see java.lang.Runnable
     */
    private void create(ThreadGroup group, Runnable runnable, String threadName, long stackSize) {
        //步骤一
        Thread currentThread = Thread.currentThread();

        //步骤二
        if (group == null) {
            group = currentThread.getThreadGroup();
        }

        if (group.isDestroyed()) {
            throw new IllegalThreadStateException("Group already destroyed");
        }

        this.group = group;

        synchronized (Thread.class) {
            id = ++Thread.count;
        }

        if (threadName == null) {
            this.name = "Thread-" + id;
        } else {
            this.name = threadName;
        }

        this.target = runnable;
        this.stackSize = stackSize;

        this.priority = currentThread.getPriority();

        this.contextClassLoader = currentThread.contextClassLoader;

        // Transfer over InheritableThreadLocals.
        if (currentThread.inheritableValues != null) {
            inheritableValues = new ThreadLocal.Values(currentThread.inheritableValues);
        }

        // add ourselves to our ThreadGroup of choice
        //步骤三  
        this.group.addThread(this);
    }
