# JVM常用实践操作

## 一、JVM全局参数预览

- **java -XX:+PrintFlagsInitial** 
- **java -XX:+PrintFlagsFinal -version > flags.txt** 输出到文件

```commonlisp
[root@localhost /]# java -XX:+PrintFlagsInitial
[Global flags]
     intx ActiveProcessorCount                      = -1                                 
 bool UseCodeCacheFlushing                      = true                                {product}
     bool UseCompiler                               = true                                {product}
     bool UseCompilerSafepoints                     = true                                {product}
     bool UseCompressedClassPointers                = false                               {lp64_product}
     bool UseCompressedOops                         = false                               {lp64_product}
     bool UseConcMarkSweepGC                        = false                              
...... 会展示全部的JVM相关参数
bool:表示bool型参数
intx等其他基本都属于：key-value型参数
```

> 这个参数何有意义对于刚接触JVM操作的同学很有帮助，便于了解大体JVM有哪些参数。

## 二、试验堆大小指针压缩特性失效堆空间的最大值

- **java -Xmx32766m -XX:+PrintFlagsFinal 2> /dev/null | grep UseCompressedOops**

![image-20210207113608718](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210207113608718.png)

## 三、字符串常量池大小设置

- **-XX:StringTableSize=10090**

## 四、栈大小相关设置

- **-XX:+UseTLAB** 

  > 是否开启栈在堆中独占的缓存空间，TLAB空间的内存非常小，缺省情况下仅占有整个Eden空间的1%。

- **-XX:TLABWasteTargetPercent** 

  > 设置TLAB空间所占用Eden空间的百分比大小。

- **-XX:-ResizeTLAB**

  > 禁用ResizeTLAB。

- **-XX:TLABSize**

  > 手工指定一个TLAB的大小。

- **-Xss128KB**

  > 设置栈的大小

- **-XX:+PrintTLAB**

  > 可以跟踪TLAB的使用情况。一般不建议手工修改TLAB相关参数，推荐使用虚拟机默认行为。

```tex
总结一下虚拟机对象分配流程：首先如果开启栈上分配，JVM会先进行栈上分配，如果没有开启栈上分配或则不符合条件的则会进行TLAB分配，如果TLAB分配不成功，再尝试在eden区分配，如果对象满足了直接进入老年代的条件，那就直接分配在老年代。
```

## 五、查看GC日志，并输出GC日志文件以及堆的OOM日志

- **-Xmx20M -Xms20M** 

  > 简单设置一下堆的大小，便于测试。

- **-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=E:\dsz-git-work\common-docs\JVM\jvm-study-file\log_hprof\gc.hprof**

  > 当堆出现溢出，打印堆的日志hprof日志。

- **-XX:+PrintGCDetails -Xloggc:E:\dsz-git-work\common-docs\JVM\jvm-study-file\log_hprof\gc.log****

  > 打印GC实时日志，并输出到置顶日志文件。



## 六、 **Parallel Scavenge**收集器（吞吐量）

- **-XX:MaxGCPauseMillis**

  > 控制最大的垃圾收集停顿时间

- **-XX:GCTimeRatio**

  > 直接设置吞吐量的大小

```tex
吞吐量 = 运行用户代码的时间/(运行用户代码的时间+垃圾收集时间).
比如虚拟机总共运行了100分钟，垃圾收集时间用了1分钟，吞吐量=(100-1)/100=99%.
若吞吐量越大，意味着垃圾收集的时间越短，则用户代码可以充分利用CPU资源，尽快完成程序
的运算任务.
```



## 七、CMS

- **-XX:+UseConcMarkSweepGC**

  > 开启CMS收集器

- **-XX:CMSFullGCsBeforeCompaction**  设置多少次Full GC出发内存整理

- **-XX:UseCMSCompactAtFullCollection**  默认开启CMS失败后的Full GC压缩机制

  > CMS GC要决定是否在full GC时做压缩，会依赖几个条件。其中，
  > 第一种条件，UseCMSCompactAtFullCollection 与 CMSFullGCsBeforeCompaction 是搭配使用的；前者目前默认就是true了，也就是关键在后者上。
  > 第二种条件是用户调用了System.gc()，而且DisableExplicitGC没有开启。
  > 第三种条件是young gen报告接下来如果做增量收集会失败；简单来说也就是young gen预计old gen没有足够空间来容纳下次young GC晋升的对象。
  > 上述三种条件的任意一种成立都会让CMS决定这次做full GC时要做压缩。
  >
  > CMSFullGCsBeforeCompaction 说的是，在上一次CMS并发GC执行过后，到底还要再执行多少次full GC才会做压缩。默认是0，也就是在默认配置下每次CMS GC顶不住了而要转入full GC的时候都会做压缩。 把CMSFullGCsBeforeCompaction配置为10，就会让上面说的第一个条件变成每隔10次真正的full GC才做一次压缩（**而不是每10次CMS并发GC就做一次压缩，目前VM里没有这样的参数**）。这会使full GC更少做压缩，也就更容易使CMS的old gen受碎片化问题的困扰。 本来这个参数就是用来配置降低full GC压缩的频率，以期减少某些full GC的暂停时间。CMS回退到full GC时用的算法是**mark-sweep-compact**，但compaction是可选的，不做的话碎片化会严重些但这次full GC的暂停时间会短些；这是个取舍。

- **-XX:CMSInitiatingOccupancyFraction=70 & -XX:+UseCMSInitiatingOccupancyOnly**

  >   这两个设置一般配合使用,一般用于『降低CMS GC频率或者增加频率、减少GC时长』的需求
  >
  >   -XX:CMSInitiatingOccupancyFraction=70 是指设定CMS在**堆内存占用率达到70%**的时候开始GC(因为CMS会有浮动垃圾,所以一般都较早启动GC)。
  >
  >   -XX:+UseCMSInitiatingOccupancyOnly 只是用设定的回收阈值(上面指定的70%),如果不指定,JVM仅在第一次使用设定值,后续则自动调整。
  >
  > #### -XX：+UseCMSInitiatingOccupancyOnly
  >
  > 我们用-XX+UseCMSInitiatingOccupancyOnly标志来命令JVM不基于运行时收集的数据来启动CMS垃圾收集周期。而是，当该标志被开启时，JVM通过CMSInitiatingOccupancyFraction的值进行每一次CMS收集，而不仅仅是第一次。然而，请记住大多数情况下，JVM比我们自己能作出更好的垃圾收集决策。因此，只有当我们充足的理由(比如测试)并且对应用程序产生的对象的生命周期有深刻的认知时，才应该使用该标志。

- **-XX:+CMSScavengeBeforeRemark重新标记前强制一次Y-GC**  

  >   在CMS GC前启动一次ygc，目的在于减少old gen对ygc gen的引用，降低remark时的开销-----一般CMS的GC耗时 80%都在remark阶段

- **CMSPermGenSweepingEnabled & CMSClassUnloadingEnabled** 

  > CMS默认情况下不会回收Perm区，通过参数CMSPermGenSweepingEnabled、CMSClassUnloadingEnabled ，可以让CMS在Perm区容量不足时对其回收。



## 八、查看当前虚拟机用的是什么垃圾收集器

- **java -XX:+PrintCommandLineFlags -version**

  ![image-20210207141452502](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210207141452502.png)

- **对照表**

  | 参数                   | 描述                                                         |
  | :--------------------- | :----------------------------------------------------------- |
  | UseSerialGC            | 虚拟机运行再Client模式下的默认值，打开此开关后，使用Serial+Serial Old的收集器组合进行内存回收 |
  | UseParNewGC            | 打开此开关后，使用ParNew+Serial Old的收集器组合进行内存回收  |
  | UseConcMarkSweepGC     | 打开此开关后，使用ParNew+CMS+Serial Old的收集器组合进行内存  |
  | UseParallelGC          | 虚拟机运行在Server模式下的默认值，打开此开关后，使用ParallelSeavenge+ParallelOld的收集器组合进行内存回收 |
  | UseParallelOldGC       | 打开此开关后，使用ParallelSeavenge+ParallelOld的收集器组合进行内存回收 |
  | SurvivorRatio          | 新生代中Eden区域与Survivor区域的容量比值，默认为8，代表Eden：Survivor=8：1 |
  | PretenureSizeThreshold | 直接晋升到老年代的对象大小，设置这个参数后，大于这个參数的对象将直接在老年代分配 |
  | MaxTenuringThreshold   | 晋升到老年代的对象年龄。每个对象在坚持过一次MinorGC之后，年龄就增加1，当超过这个参数值时就进人老年代 |

- **各JDK默认的收集器组合**
  - JDK1.7 默认垃圾收集器Parallel Scavenge（新生代）+ Parallel Old（老年代）
  - JDK1.8 默认垃圾收集器Parallel Scavenge（新生代）+ Parallel Old（老年代）
  - JDK1.9 默认垃圾收集器G1



## 九、G1

- -XX: +UseG1GC 开启G1垃圾收集器

- -XX: G1HeapReginSize 设置每个Region的大小，是2的幂次，1MB-32MB之间

- -XX:MaxGCPauseMillis 最大停顿时间

- -XX:ParallelGCThread 并行GC工作的线程数

- -XX:ConcGCThreads 并发标记的线程数

- -XX:InitiatingHeapOcccupancyPercent 默认45%，代表GC堆占用达到多少的时候开始垃圾收集



## 十、jinfo JVM参数相关操作

- **jps -l** 查看java相关进程

  ![image-20210207150004390](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210207150004390.png)

- **jinfo -flflag flagName PID** 查看具体java虚拟机的参数

  - **jinfo -flag MaxHeapSize** 6529 查看6529虚拟机的最大堆内存

    ![image-20210207150213424](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210207150213424.png)

  - **jinfo -flag UseG1GC 6529** 查看6529虚拟机是否开启G1回收机制

    ![image-20210207150313717](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210207150313717.png)

- **动态修改某个JVM的参数**(参数只有被标记为manageable的flflags可以被实时修改)

  - **jinfo -flag [+|-] PID** 
  - **jinfo -flag <name>=<value> PID**

- **jinfo -flags 6529** 查看历史修改过的参数

  ```
  Attaching to process ID 6529, please wait...
  Debugger attached successfully.
  Server compiler detected.
  JVM version is 25.261-b12
  Non-default VM flags: 
  -XX:CICompilerCount=2 
  -XX:InitialHeapSize=94371840 
  -XX:MaxHeapSize=1484783616 
  -XX:MaxNewSize=494927872 
  -XX:MinHeapDeltaBytes=524288 
  -XX:NewSize=31457280 
  -XX:OldSize=62914560 
  -XX:+PrintGC 
  -XX:+PrintGCDetails 
  -XX:+PrintGCTimeStamps 
  -XX:+UseCompressedClassPointers 
  -XX:+UseCompressedOops 
  -XX:+UseFastUnorderedTimeStamps 
  -XX:+UseParallelGC 
  Command line:  -XX:+PrintGCDetails -XX:+PrintGCDetails -Xloggc:./gc.log
  
  ```

  

## 十一、jstat JVM性能分析相关操作

- **jstat -class 6529 2000 10**

  > 查看某个java进程的类装载信息，每2秒输出一次，共输出5次

  ![image-20210207152158212](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210207152158212.png)

- **jstat -gc 6529 2000 5 ** 垃圾回收统计

  > 每隔2秒查看一次GC情况，共输出5次

  ![image-20210207152505091](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210207152505091.png)

  ```tex
  S0C：第一个幸存区的大小
  S1C：第二个幸存区的大小
  S0U：第一个幸存区的使用大小
  S1U：第二个幸存区的使用大小
  EC：伊甸园区的大小
  EU：伊甸园区的使用大小
  OC：老年代大小
  OU：老年代使用大小
  MC：方法区大小
  MU：方法区使用大小
  CCSC:压缩类空间大小
  CCSU:压缩类空间使用大小
  YGC：年轻代垃圾回收次数
  YGCT：年轻代垃圾回收消耗时间
  FGC：老年代垃圾回收次数
  FGCT：老年代垃圾回收消耗时间
  GCT：垃圾回收消耗总时间
  ```

  

- **jstat -gccapacity 6529** 堆内存统计

  ![image-20210207154826943](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210207154826943.png)

  ```tex
  NGCMN：新生代最小容量
  NGCMX：新生代最大容量
  NGC：当前新生代容量
  S0C：第一个幸存区大小
  S1C：第二个幸存区的大小
  EC：伊甸园区的大小
  OGCMN：老年代最小容量
  OGCMX：老年代最大容量
  OGC：当前老年代大小
  OC:当前老年代大小
  MCMN:最小元数据容量
  MCMX：最大元数据容量
  MC：当前元数据空间大小
  CCSMN：最小压缩类空间大小
  CCSMX：最大压缩类空间大小
  CCSC：当前压缩类空间大小
  YGC：年轻代gc次数
  FGC：老年代GC次数
  ```



- **jstat -gcnew 6529**  新生代垃圾回收统计

  ![image-20210207155230363](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210207155230363.png)

  ```tex
  S0C：第一个幸存区大小
  S1C：第二个幸存区的大小
  S0U：第一个幸存区的使用大小
  S1U：第二个幸存区的使用大小
  TT:对象在新生代存活的次数
  MTT:对象在新生代存活的最大次数
  DSS:期望的幸存区大小
  EC：伊甸园区的大小
  EU：伊甸园区的使用大小
  YGC：年轻代垃圾回收次数
  YGCT：年轻代垃圾回收消耗时间
  ```

  

- **jstat -gcnewcapacity 6529**  新生代内存统计

  ![image-20210207155513251](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210207155513251.png)

  ```tex
  NGCMN：新生代最小容量
  NGCMX：新生代最大容量
  NGC：当前新生代容量
  S0CMX：最大幸存1区大小
  S0C：当前幸存1区大小
  S1CMX：最大幸存2区大小
  S1C：当前幸存2区大小
  ECMX：最大伊甸园区大小
  EC：当前伊甸园区大小
  YGC：年轻代垃圾回收次数
  FGC：老年代回收次数
  ```

  

- **jstat  -gcold 6529**  老年代垃圾回收统计

  ![image-20210207155654963](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210207155654963.png)

  ```tex
  MC：方法区大小
  MU：方法区使用大小
  CCSC:压缩类空间大小
  CCSU:压缩类空间使用大小
  OC：老年代大小
  OU：老年代使用大小
  YGC：年轻代垃圾回收次数
  FGC：老年代垃圾回收次数
  FGCT：老年代垃圾回收消耗时间
  GCT：垃圾回收消耗总时间
  ```

- **jstat -gcoldcapacity 6529** 老年代内存统计

  ![image-20210207155919092](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210207155919092.png)

  ```tex
  OGCMN：老年代最小容量
  OGCMX：老年代最大容量
  OGC：当前老年代大小
  OC：老年代大小
  YGC：年轻代垃圾回收次数
  FGC：老年代垃圾回收次数
  FGCT：老年代垃圾回收消耗时间
  GCT：垃圾回收消耗总时间
  ```

- **jstat -gcmetacpacity 6529**  元数据空间统计

  ![image-20210207160136427](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210207160136427.png)

  ```tex
  MCMN: 最小元数据容量
  MCMX：最大元数据容量
  MC：当前元数据空间大小
  CCSMN：最小压缩类空间大小
  CCSMX：最大压缩类空间大小
  CCSC：当前压缩类空间大小
  YGC：年轻代垃圾回收次数
  FGC：老年代垃圾回收次数
  FGCT：老年代垃圾回收消耗时间
  GCT：垃圾回收消耗总时间
  ```

- **jstat -gcutil 6529** 总结垃圾回收统计

  ![image-20210207160332041](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210207160332041.png)

  ```tex
  S0：幸存1区当前使用比例
  S1：幸存2区当前使用比例
  E：伊甸园区使用比例
  O：老年代使用比例
  M：元数据区使用比例
  CCS：压缩使用比例
  YGC：年轻代垃圾回收次数
  FGC：老年代垃圾回收次数
  FGCT：老年代垃圾回收消耗时间
  GCT：垃圾回收消耗总时间
  ```

- **jstat -printcompilation 6529**  JVM编译方法统计

  ![image-20210207160617320](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210207160617320.png)

  ```tex
  Compiled：最近编译方法的数量
  Size：最近编译方法的字节码数量
  Type：最近编译方法的编译类型。
  Method：方法名标识。
  ```

  

## 十二、jstack 堆栈分析操作

































