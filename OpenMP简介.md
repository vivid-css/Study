# OpenMP简介

---
## 1.常用概念
**1.并发**：同一时间***间隔***内发生多件事（CPU在若干程序之间的复用）
**2.并行**：同一***时刻***在多个环境发生多件事，多个程序同一时刻可以在不同CPU同时执行
**3.并行计算**：一次执行多个指令（一个问题分成不同模块，每个模块由一个独立的处理器计算）
- 时间上的并行
- 空间上的并行：多个处理器并发进行计算

**4.进程**
- 广义：一个独立功能的程序关于摸个数据集合上的一次运行活动，事系统进行资源分配和调度的一个独立单位
- 侠义：正在运行的程序的实例

**5.线程**
- 操作系统能够进行运算调度的最小单位
- 包含在进程之中，是进程中的实际运作单位
- 一个进程中可以并发多个线程，每条线程并行执行不同的任务
- 同一进程中的多条线程将共享该进程中的全部系统资源
- 同一进程中的多个线程有各自的调用栈自己的寄存器环境，自己的线程本地存储

`一个程序至少包含一个进程，一个进程至少包含一个线程（主线程）`

**6.超线程**
- 模拟两个物理芯片，用单个处理器完成线程级的并行计算
- 若两个线程同时需要某个资源，则其中一个线程让出资源暂时挂起

`超线程的性能不能等于两个CPU的性能`

---
## 2.OpenMP
**1.概念**：面向共享内存的多线程并行编程接口
**2.编译指导语句**：
`#pragma omp <directive> [clause[[,]clause]...]`
- **dieective**:知道多个CPU共享任务，或多个CPU同步
- **clause**：可选子句，影响编译指导语句的执行

`换行符是必选项！`
`openmp只能并行化循环次数确定的for循环`

## 3.常用指令
|parallel|用在一个结构块之前，表示这段代码将被多个线程并行执行|
|---|-----|
|for|用于for循环语句之前，表示将循环计算任务分配到多个线程中并行执行，以实现任务分担，但必须保证每次循环之间无数据相关性|
|sections|用在要被并行执行的代码段之前，用于实现多个结构块语句的任务分担，可并行执行的代码段各自用section指令标出|
|critical|用在一段代码临界区之前，保证每次只有一个0penMP线程进入|
|single|用在并行域内，表示一段只被单个线程执行的代码|
|flush|保证各个0penMP线程的数据影像的一致性|
|barrier|用于并行域内代码的线程同步，线程执行到barrier时要停下等待，直到所有线程都执行到barrier时才继续往下执行|

## 4.常用子句

|private|指定一个或多个变量在每个线程中都有它自己的私有副本|
|---|---|
|shared|指定一个或多个变量为多个线程间的共享变量|
|default|用来指定并行域内的变量的使用方式，缺省是shared|
|reduction|用来指定一个或多个变量是私有的，并且在并行处理结束后这些变量要执行指定的归约运算，并将结果返回给主线程同名变量|


## 5.编程
- 使用openmp中的库函数时，要引用头文件
   ` #include <omp.h>`
- 库函数
 ![alt text](b241334ccf9aa85245ada60e173d4100.png)
![alt text](image.png)

- 变量作用域
  - 共享变量：所有线程共有的变量
  - 私有变量：个别线程拥有的变量
- private（变量列表）
  - 每个线程都有一个自己的私有副本，其他线程无法访问
  - private声明的变量在并行入口处没有定义（不继承并行区域外的同名原始变量的值）
- shared（变量列表）
  - 声明共享变量
  - 在并行区域内，所有线程共用这个变量的地址
- default（shared|none）
- reduction（运算符：变量列表）
  - 对有前后依赖的循环经行规约操作 



