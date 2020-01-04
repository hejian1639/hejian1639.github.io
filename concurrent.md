#目的
之前就想做一期关于高并发相关的分享，主要是面向新人的，因为之前修改前人代码，感觉即使是老员工其实也也会有所收获吧
#资料

咱们的话题主要以java为主，也会提一下c或者go做一下对比。下面列了一下参考的资料。

![book](https://img1.doubanio.com/view/subject/l/public/s29405037.jpg)

![book](https://static001.infoq.cn/resource/image/98/15/981342d3b328a5e4ca7051c69a1d3b15.jpg?x-oss-process=image/resize,h_496)

#如何评价代码好坏
先聊一个题外话，什么是评价代码好坏的标准，要衡量这点我把它分为主观的和客观的。
下面的话题以满足需求为前提

##主观
主观方面以理念为主，例如可读性可维护性，可维护性等。
###可读性
可读性会被很多初级开发奉为信条，这里并不是去反对可读性，但是人们对可读性的判断依据比较模糊，也不好衡量

###可维护性
有时候我们被要求对代码添加各种注释，因为有些领导认为有了足够多的注释代码的可读性就会提高，可维护性也会高很多，这样组织对抗人员变动风险的能力就会高很多。
现实很不幸这仍然是一个主观一厢情愿的想法，实际上大多数的程序员在接手别人的代码时，第一个念头往往是重写，因为读代码的成本有时高于维护
这不是说不要大家写注释，毕竟可读性高的代码理解成本低，重写起来也容易。

##客观
客观方面考察方式会比较直观，以模块的可扩展性，解藕为考察重点

###封装性
模块化是衡量代码可维护性的一个有效标尺，它让你的代码具有了复用性，如果有很多雷同代码大块的出现，是应该反思一下了。

###可扩展性
唯一不变的就是需求的变化，这是在软件界算是一个真理了。这个世界应该没有能适应所有变化的设计，如果非说有那估计就是无设计，毕竟人们对不存在的东西很难挑出毛病。不过程序猿们还是有追求的，不想要在不断的搬砖过程中蹉跎，所以就有了上面的模块化的概念，但是也不是随便模块化，这里面衡量标准就是是不是每次应对需求都要改变底层模块。
总结，代码层次中越底层的变化越慢，越上层的越贴近需求变化。

###性能
上面都是个人观点而且和这次分享没有直接关系，下面进入主题，性能。
这是一个比较大的话题，不过程序猿们为了追求极致的性能可能会把可读性和可维护性抛于脑后，后面会举例说明

##举例
[Turbo Pascal](https://zh.wikipedia.org/zh-hans/Turbo_Pascal)曾被誉为编译速度最快的编译器，作者是[Anders Hejlsberg](https://zh.wikipedia.org/wiki/%E5%AE%89%E5%BE%B7%E6%96%AF%C2%B7%E6%B5%B7%E5%B0%94%E6%96%AF%E4%BC%AF%E6%A0%BC)(C#语言作者，typescript语言作者)，纯汇编打造的编译器性能不用说了，只是当年大神离开Borland去微软之后Delphi产品线也停了，因为没人能继续维护那段代码了。。。

做过图形相关开发的人都会感觉OpenGL的API好难用啊，OpenGLES更难用，h5的canvas不是挺好的嘛，干嘛还设计那么复杂找麻烦吗

MapReduce最早是Google提出的大数据处理的软件架构，这种抽象方式从规则角度规避了并发中的同步问题，在模型抽象期间强制开发者考虑隔离性间，当然副作用也有，就是不直观，对于强调可读性的人来说可能也不算好的设计

#并发

前面之所以讨论了一些代码评价和性能相关问题是因为好的设计和坏的设计对并发的效率也会有比较大的影响，这是比较直观且客观的

##目的
并发的目的有两个：

- 同时提供多个服务
- 因为io操作的速度和cpu不匹配

##解决方案

###进程
这次不多讨论使用进程的并发相关设计，涉及IPC还是一块很大的领域

###线程
线程本质和进程的共同点是可以分到单独的cpu时间片，只是存储不隔离，这点即是灵活的地方，也是它的原罪。

###同步

####基本概念
1. 内存模型抽象

 虽然讲的是并发问题，但是避免不了和内存打交道。这里不会介绍太多虚拟机层面的东西，只是将内存简化为两部分共享存储（内存）和局域存储（寄存器），便于对可见性的理解

 ![screenshot](memory.png)

2. 重新排序 
 - 编译优化 
 - 指令并行重排
 - 内存系统重现排序

 ![screenshot](instruct.png)
 
     
    重新排序在多线程情况下会出现难以直观理解的现象
    
    <table style="width:100%">
      <tr>
        <th> Process A </th>
        <th> Process B </th> 
      </tr>
      <tr>
        <td> a = 1 <br> x = b </td>
        <td> b = 1 <br> y = a </td>
      </tr>
      <tr>
        <td colspan='2'>初始状态：a = b = 0 <br> 可能出现结果：x = y = 0</td>
      </tr>
    </table>



    
3. 数据依赖性

 即使有重新排序的可能，但是仍然遵循数据依赖规则
![screenshot](dependency.png)
    
    
4. 内存屏障

 除了数据依赖规则还可以通过cpu指令来阻止重新排序

    |屏障类型|指令示例|指令示例|
    |:-:|:-:|:-:|
    |LoadLoad屏障|Load1; LoadLoad; Load2|在Load2及后续读取操作要读取的数据被访问前，保证Load1要读取的数据被读取完毕|
    |StoreStore屏障|Store1; StoreStore; Store2|在Store2及后续写入操作执行前，保证Store1的写入操作对其它处理器可见|
    |LoadStore屏障|Load1; LoadStore; Store2|在Store2及后续写入操作被刷出前，保证Load1要读取的数据被读取完毕|
    |StoreLoad屏障| Store1; StoreLoad; Load2|在Load2及后续所有读取操作执行前，保证Store1的写入对所有处理器可见|
  
3. happen-before
 
 为了化简对内存模型的理解难度，提出了happen before规则
 
 定义:
 
 - 如果一个操作happens-before另一个操作，那么第一个操作的执行结果将对第二个操作可见，而且第一个操作的执行顺序排在第二个操作之前。 
 - 两个操作之间存在happens-before关系，并不意味着一定要按照happens-before原则制定的顺序来执行。如果重排序之后的执行结果与按照happens-before关系来执行的结果一致，那么这种重排序并不非法。

 
 规则:
 
 - 程序次序规则：一个线程内，按照代码顺序，书写在前面的操作先行发生于书写在后面的操作；
 - 锁定规则：一个unLock操作先行发生于后面对同一个锁额lock操作；
 - volatile变量规则：对一个变量的写操作先行发生于后面对这个变量的读操作；
 - 传递规则：如果操作A先行发生于操作B，而操作B又先行发生于操作C，则可以得出操作A先行发生于操作C；
 - 线程启动规则：Thread对象的start()方法先行发生于此线程的每个一个动作；
 - 线程中断规则：对线程interrupt()方法的调用先行发生于被中断线程的代码检测到中断事件的发生；
 - 线程终结规则：线程中所有的操作都先行发生于线程的终止检测，我们可以通过Thread.join()方法结束、Thread.isAlive()的返回值手段检测到线程已经终止执行；
 - 对象终结规则：一个对象的初始化完成先行发生于他的finalize()方法的开始；
     
    ```java
    //thread 1
    a = 1;
    b = a;
     
    //tread 2
    if(b == 1){
        assert(a == 1)
    }
    ```
    ```java
    //thread 1
    a = 1;
    b = 1;
     
    //tread 2
    if(b == 1){
        assert(a == 1)
    }
    ```

####同步原语 
- 静态初始化

这点经常被人忽略，但是类加载线程安全，这点是虚拟机保证的

```java
public class Resource {
    private static Resource resource;

    static {
        resource = new Resource();

    }

    public static Resource getInstance() {
        return resource;
    }
}

```

- ####synchronized

 synchronized编译成字节码后，是通过monitorenter（入锁）和monitorexit（出锁）两个指令实现的，具体过程如下：

![screenshot](https://upload-images.jianshu.io/upload_images/1529069-cb6b5eae2b68efd9.png?imageMogr2/auto-orient/strip|imageView2/2/w/347)

- ####volatile

 volatile除了可以阻止编译优化还会自动加入内存屏障
 
 - 写操作
 
 ![screenshot](https://upload-images.jianshu.io/upload_images/1529069-333fa42581e1aa03.png?imageMogr2/auto-orient/strip|imageView2/2/w/568)
 
 - 读操作
 
 ![screenshot](https://upload-images.jianshu.io/upload_images/1529069-35ae750988d40a79.png?imageMogr2/auto-orient/strip|imageView2/2/w/640)
- ####lock

 锁可以理解为将synchronized拆成两部分，所以自带内存屏障的语义，即读操作不能重排到lock之前，写操作不能重排到unlock之后

- ####atomic

###优化实例

###协程


###并发模型
- 流式模型
- Actor模型

