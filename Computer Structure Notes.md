# 目录

[一、基础](#一、基础)

[二、层次存储](#二、层次存储)

[三、流水线](#三、流水线)

[四、高级流水线](#四、高级流水线)

[五、互连与通信](#五、互连与通信)

[六、向量处理](#六、向量处理)

[七、数据级并发性](#七、数据级并发性)



#一、基础

[分类](##分类)

[性能指标](##性能指标（如何评价一个处理机的性能？）)

[Amdahl定律](##Amdahl定律)

## 分类

### 按处理机性能分类

- 按大小划分：巨型、大型、中型、小型、微型
- 按用途划分：科学计算、事务处理、实时控制、工作站、服务器、家用计算机
- 按数据类型划分：定点计算机、浮点计算机、向量计算机、堆栈计算机等
- 按处理机个数和种类划分：
  - 单处理机
  - 并行处理机、多处理机、分布处理机
  - 关联处理机
  - 超标量处理机、超流水线处理机、VLIW处理机
  - 。。。
- 按所使用的器件划分：
  - 第一代：电子管
  - 第二代：晶体管
  - 第三代：集成电路
  - 第四代：大规模集成电路
  - 第五代：智能计算机

### 佛林分类法：按照指令流和数据流的多倍性特征分类

<table>
  <tr>
    <td>
      <img src="./pics/Basic/19.png" />
    </td>
    <td>
      <img src="./pics/Basic/7.png" />
    </td>
    <td>
      <img src="./pics/Basic/8.png" />
    </td>
  </tr>
  <tr>
    <td>
      <img src="./pics/Basic/9.png" />
    </td>
    <td>
      <img src="./pics/Basic/10.png" />
    </td>
    <td>
      <img src="./pics/Basic/20.png" />
    </td>
  </tr>
</table>

### 库克分类法:

<table>
  <tr>
    <td>
      <img src="./pics/Basic/11.png" />
    </td>
    <td>
      <img src="./pics/Basic/12.png" />
    </td>
  </tr>
</table>

### 冯泽云分类法：

<table>
  <tr>
    <td>
      <img src="./pics/Basic/13.png" />
    </td>
    <td>
      <img src="./pics/Basic/14.png" />
    </td>
    <td>
      <img src="./pics/Basic/15.png" />
    </td>
  </tr>
</table>

### 汉德勒分类法：

<table>
  <tr>
    <td>
      <img src="./pics/Basic/16.png" />
    </td>
    <td>
      <img src="./pics/Basic/17.png" />
    </td>
    <td>
      <img src="./pics/Basic/18.png" />
    </td>
  </tr>
</table>



## 性能指标（如何评价一个处理机的性能？）

- 执行时间与吞吐率
- MIPS、CPI、IPC
  - MIPS(Million Instructions Per Second) = $\frac{IC}{执行时间\times10^6} = \frac{Fz}{CPI} = IPC \times Fz$
    - IC：指令条数(Instruction counts)
    - Fz: 为处理机的工作主频
  - CPI(Cycles Per Instruction): 每条计算机指令执行所需的时钟周期
  - CPI = 总周期 / 指令数
  - IPC(Instruction Per Cycle) = 1 / CPI
- CPU时间 = IC * CPI * 时钟周期时间 
  - IC：指令条数(Instruction counts)
- 加速比：
  - [Amdahl定律](##Amdahl定律)



## Questions

<table>
	<tr>
		<td>
			<img src="./pics/Basic/1.png" />
    </td>
    <td>
    	<img src="./pics/Basic/2.png" />
   	</td>
  </tr>
</table>


## Amdahl定律

<table>
	<tr>
		<td>
			<img src="./pics/Basic/3.png" />
		</td>
		<td>
			<img src="./pics/Basic/4.png" />
		</td>
	</tr>
</table>

## Questions

<table>
	<tr>
		<td>
			<img src="./pics/Basic/5.png" />
		</td>
		<td>
			<img src="./pics/Basic/6.png" />
		</td>
	</tr>
  <tr>
		<td>
			<img src="./pics/Basic/21.png" />
		</td>
		<td rowspan=2>
      <img src="./pics/Basic/23.png" style="zoom:20%;" />
		</td>
	</tr>
  <tr>
		<td>
			<img src="./pics/Basic/22.png" />
		</td>
	</tr>
</table>






# 二、层次存储

[Cache](##Cache)

[Cache Optimization](#cache-optimization)

[Improving Cache Performance](#improving-cache-performance)

[Cache Friendly Code](##cache-friendly-code)

## Cache

### Cache映射规则

<table>
  <tr>
    <td>
      <img src="./pics/Cache/6.png" />
    </td>
    <td>
      <img src="./pics/Cache/7.png" />
    </td>
    <td>
      <img src="./pics/Cache/8.png" />
    </td>
  </tr>
</table>

### Cache替换算法

- Random
- LRU
- FIFO / Round-Robin
- NLRU: FIFO with exception for most recently used block or blocks
- One-bit LRU

### Cache写策略

- Write Through: to both cache and memory (with write buffers for faster write)
- Write back: only in cache, and its only writen to memory when its replaced

### Cache写缺失

- Write Allocate: The block will be loaded from memory to cache, and a write-hit action follows.
- No-write Allocate: The block is modified in the memory and not loaded into the cache.
- Tips: "Write back" caches generally use "write allocate" and "write through" often use "no-write allocate".

### Cache的性能分析

- **Hit Time(HT)** = time to find the retrieve data from current level cache

- **Miss Penalty(MP)** = average time to retrieve data on a current level miss (includes the possibility of misses on successive levels of memory hierachy)

- **Hit Rate(HR)** = % of requests that are found in current level cache

- **Miss Rate(MR)** = 1 - Hit Rate

- **Average Memory Access Time (AMAT)** 

  = **HT + MR * MP**

  = %instructions * (HT_inst + MR_inst * MP_inst) + %data * (HT_data + MR_data * MP_data)

  ("inst" stands for instructions, and "data" stands for data)

- **Memory Stall Cycles** = IC * (Misses / Instructions) * MP

- **CPU Excecution time** 

  = **(CPU clock cycles + Memory Stall Cycles) * Clock cycle time**

  = IC * (CPI + (仿存次数/IC) * MR * MP) * Clock cycle time

  = IC * (CPI + 每条指令的平均仿存次数 * MR * MP) * Clock cycle time



## Cache Optimization

### Cache Miss：

<table>
  <tr>
    <td>
      <img src="./pics/Cache/15.png" />
    </td>
    <td>
      <img src="./pics/Cache/1.png" />
    </td>
    <td>
      <img src="./pics/Cache/2.png" />
    </td>
  </tr>
</table>



## Improving Cache Performance

<img src="./pics/Cache/19.png" style="zoom:25%;" />

### Reducing Miss Rate:

- change block size - Larger Block Size, reduce Capacity misses but higher cost and longer hit time
- change Cache Internal Organization - Larger Caches, 
- change Associativity - Higher Associativity / Way Prediction(Pseudo Associativity)
- change Compiler
  - Merging Arrays - combine arrays into a single compound elements
  - Loop Interchange - change nesting of loops
  - Loop Fusion - combine 2 independent loops that have same looping and some variables overlap
  - Blocking: change the way of accessing array into going down the whole columns or rows.

### Reducing Miss Penalty:

- Multilevel Caches

  <img src="./pics/Cache/21.png" style="zoom:40%;" />

- Early Restart and Critical Word First

  <table>
    <tr>
    	<td>
        <center>
        	<img src="./pics/Cache/17.png" style="zoom:30%;" />
        </center>
      </td>
      <td>
        <center>
        	<img src="./pics/Cache/18.png" style="zoom:30%;" />
  			</center>
      </td>
    </tr>
  </table>

- Read Priority over Write on Miss: check Write Buffer before read, if no conflicts, read memory, else read from Write Buffer.
- Merging Write Buffer: multiword writes are faster than a single word writes => reduce write-buffer stalls.
- Victim Caches: add buffer to place data discarded from cache in the case it is needed again.

### Reducing Hit Time

- Small and Simple Caches: smaller the faster, and simpler like direct mapped cache is faster.

- Avoiding Address Translation

  <img src="./pics/Cache/20.png" style="zoom:40%;" />



## Cache Friendly Code

### Multi-Level Design

<table>
  <tr>
  	<td>
      <center>
      	<img src="./pics/Cache/22.png" style="zoom:30%;" />
      </center>
    </td>
    <td>
      <center>
      	<img src="./pics/Cache/23.png" style="zoom:30%;" />
			</center>
    </td>
  </tr>
</table>

### Matrix Multiplication

<img src="./pics/Cache/24.png" style="zoom: 50%;" />



## Questions

主存地址：tag｜index｜offset

cache地址：valid | tag ｜offset

<table>
  <tr>
    <td>
      <center>
      	<img src="./pics/Cache/3.png" style="zoom:35%; "/>
      </center>
    </td>
    <td>
      <center>
      	<img src="./pics/Cache/4.png" style="zoom:35%; "/>
      </center>
    </td>
  </tr>
  <tr>
    <td>
      <img src="./pics/Cache/9.png" />
    </td>
    <td>
      <img src="./pics/Cache/10.png" />
    </td>
  </tr>
  <tr>
    <td>
      <img src="./pics/Cache/11.png" />
    </td>
    <td rowspan=2>
      <img src="./pics/Cache/14.png" />
    </td>
  </tr>
  <tr>
    <td>
      <img src="./pics/Cache/16.png" style="zoom:10%;" />
    </td>
  </tr>
</table>
<table>
  <tr>
    <td>
    <img src="./pics/Cache/12.png" />
    </td>
    <td>
    <img src="./pics/Cache/13.png" style="zoom:50%;" />
    </td>
  </tr>
</table>








# 三、流水线

[线性流水线性能](##线性流水线性能)

[非线性流水线的调度技术](##非线性流水线的调度技术)

[超标量处理机](##超标量处理机)



## 线性流水线的性能

- 时空图和预约表

  <table>
    <tr>
    	<td>
        <img src="./pics/Pipeline/1.png" />
      </td>
      <td>
        <img src="./pics/Pipeline/2.png" />
      </td>
    </tr>
  </table>

- 吞吐率、加速比、效率

  <table>
    <tr>
      <td>
      	<img src="./pics/Pipeline/3.png" style="zoom: 30%;">
      </td>
      <td>
      	<img src="./pics/Pipeline/4.png">
      </td>
    </tr>
    <tr>
      <td>
      	<img src="./pics/Pipeline/5.png">
      </td>
      <td>
      	<img src="./pics/Pipeline/6.png">
      </td>
    </tr>
  </table>



## 非线性流水线的调度技术

<img src="./pics/Pipeline/7.png" style="zoom:25%;" />

- 一张预约表可能与多个流水线连接图相对应，反之亦然

### 非线性流水线的冲突

- 流水线的启动距离：连续输入两个任务之间的时间间隔

- 流水线的冲突：几个任务争用同一个流水段

- **无冲突调度方法：**

  <table>
    <tr>
      <td>
      	<img src="./pics/Pipeline/8.png" />
      </td>
      <td>
      	<img src="./pics/Pipeline/9.png" />
      </td>
    </tr>
    <tr>
      <td>
      	<img src="./pics/Pipeline/10.png" />
      </td>
      <td>
      	<img src="./pics/Pipeline/11.png" />
      </td>
    </tr>
    <tr>
  	  <td>
      	<img src="./pics/Pipeline/12.png" />
      </td>
      <td>
      	<img src="./pics/Pipeline/13.png" />
      </td>
    </tr>
  </table>

- **优化调度方法：**

  <table>
    <tr>
      <td>
      	<img src="./pics/Pipeline/14.png" />
      </td>
      <td>
      	<img src="./pics/Pipeline/15.png" />
      </td>
    </tr>
    <tr>
      <td>
      	<img src="./pics/Pipeline/16.png" />
      </td>
      <td>
      	<img src="./pics/Pipeline/17.png" />
      </td>
    </tr>
  </table>



## 超标量处理机

### Index

- [基本结构](###基本结构)
- [单发射与多发射](###单发射与多发射)
- [多流水线调度](###多流水线调度)
- [资源冲突](###资源冲突)
- [超标量处理机性能](###超标量处理机性能)

### 基本结构

- 一般流水线处理机：
  - 一条指令流水线
  - 一个多功能操作部件
- 多操作部件处理机：
  - 一条指令流水线
  - 多个独立的操作部件
- 超标量处理机典型结构：
  - 多条指令流水线

### 单发射与多发射

<table>
  <tr>
    <td>
    	<img src="./pics/Pipeline/18.png" />
    </td>
    <td>
    	<img src="./pics/Pipeline/19.png" />
    </td>
  </tr>
  <tr>
    <td>
    	<img src="./pics/Pipeline/20.png" />
    </td>
    <td>
    	<img src="./pics/Pipeline/21.png" />
    </td>
  </tr>
</table>

超标量处理机的指令级并行度：1<ILP<m，m为每个周期发射的指令条数

### 多流水线调度

<table>
  <tr>
    <center>
      <td>
        <img src="./pics/Pipeline/22.png" style="zoom:40%;" />
      </td>
      <td>
        <img src="./pics/Pipeline/23.png" style="zoom:40%;" />
      </td>
    </center>
  </tr>
  <tr>
    <center>
      <td>
        <img src="./pics/Pipeline/24.png" style="zoom:40%;" />
      </td>
    </center>
  </tr>
</table>

### 资源冲突

<table>
  <tr>
    <center>
      <td>
        <img src="./pics/Pipeline/25.png" style="zoom:40%;" />
      </td>
      <td>
        <img src="./pics/Pipeline/26.png" style="zoom:40%;" />
      </td>
    </center>
  </tr>
  <tr>
    <center>
      <td>
        <img src="./pics/Pipeline/27.png" style="zoom:40%;" />
      </td>
    </center>
  </tr>
</table>

### 超标量处理机性能

- **定义**
  - 单流水线普通处理机
    - ILP = (1, 1)
    - N条指令执行时长：$T(1, 1) = (k + N - 1) \times \delta t$
  - 超标量处理机
    - ILP = (m, 1)
    - N条指令执行时长：$T(m, 1) = (k + \frac{N - m}{m}) \times \delta t$
    - 相比于单流水线的加速比为：$S(m, 1) = \frac{T(1, 1)}{T(1, n)} = \frac{(k + N - 1) \times \delta t}{(k + \frac{N - m}{m}) \times \delta t}$
    - 加速比最大值：$S(m, 1)_{max} = ?$
  - 超流水线处理机
    - ILP = (1, n)
    - N条指令执行时长：$T(1, n) = (k + \frac{N-1}{n}) \times \delta t$
    - 相比于单流水线的加速比为：$S(1, n) = \frac{T(1, 1)}{T(1, n)} = \frac{(k + N - 1) \times \delta t}{(k + \frac{N-1}{n}) \times \delta t}$
    - 加速比最大值：$S(1, n)_{max} = n$
  - 超标量超流水线处理机
    - ILP = (m, n)
    - N条指令执行时长：$T(m, n) = (k + \frac{N - m}{m n})\times\delta{t}$
    - 相比于单流水线的加速比为：$S(m, n) = \frac{T(1, 1)}{T(1, n)} = \frac{(k + N - 1) \times \delta t}{(k + \frac{N-m}{mn}) \times \delta t} = \frac{mn(k + N - 1)}{mnk + N - m}$
    - 加速比最大值：$S(m, n)_{max} = mn$

- 超标量 vs 超流水线

  |                      | 超标量                       | 超流水线                             |
  | -------------------- | ---------------------------- | ------------------------------------ |
  | 提高处理机性能的方法 | 增加硬件资源来提高处理机性能 | 各部分硬件的重叠工作来提高处理机性能 |
  | 两种不同并行性       | 空间并行性                   | 时间并行性                           |

- 三种指令级并行处理机性能比较

  <img src="./pics/Pipeline/28.png" style="zoom:50%;" />







# 四、高级流水线

[流水线的静态调度](##流水线的静态调度)

[流水线动态调度](##流水线动态调度)

[寄存器重命名](##寄存器重命名)

[预测分支](##预测分支)



## 流水线的静态调度

### Definitions

<table>
  <tr>
  	<td>
    	<img src="./pics/Advance Pipeline/3.png" />
    </td>
    <td>
    	<img src="./pics/Advance Pipeline/4.png" />
    </td>
    <td>
    	<img src="./pics/Advance Pipeline/5.png" />
    </td>
  </tr>
</table>

### Steps for Compiler to preform UNROLL

<table>
  <tr>
    <td>
    	<img src="./pics/Advance Pipeline/1.png" />
    </td>
    <td>
    	<img src="./pics/Advance Pipeline/2.png" />
    </td>
  </tr>
</table>

### Multiple Issue

- Pipeline CPI = Ideal pipeline CPI + Structural Stalls + RAW stalls + WAR stalls + WAW stalls + Control stalls

- <img src="./pics/Advance Pipeline/6.png" style="zoom:50%;">

<table>
  <tr>
    <td>
    	<img src="./pics/Advance Pipeline/7.png" />
    </td>
    <td>
    	<img src="./pics/Advance Pipeline/8.png" />
    </td>
  </tr>
</table>



## 流水线动态调度

### Tomasulo's Algorithm

<table>
  <tr>
    <td>
    	<img src="./pics/Advance Pipeline/25.png" />
    </td>
    <td>
    	<img src="./pics/Advance Pipeline/26.png" />
    </td>
  </tr>
  <tr>
    <td>
    	<img src="./pics/Advance Pipeline/27.png" />
    </td>
    <td>
    	<img src="./pics/Advance Pipeline/28.png" />
    </td>
  </tr>
  <tr>
    <td>
    	<img src="./pics/Advance Pipeline/29.png" />
    </td>
    <td>
    	<img src="./pics/Advance Pipeline/9.png" />
    </td>
  </tr>
</table>



### Tomasulo with ROB

<table>
  <tr>
    <td>
    	<img src="./pics/Advance Pipeline/10.png" />
    </td>
    <td>
    	<img src="./pics/Advance Pipeline/11.png" />
    </td>
    <td>
    	<img src="./pics/Advance Pipeline/12.png" />
    </td>
  </tr>
</table>



## 寄存器重命名

1. 定义：

   <img src="./pics/Advance Pipeline/13.png" style="zoom:40%;" />

2. Renaming effects on WAR && RAW && WAW:

   <table>
     <tr>
       <td>
         <img src="./pics/Advance Pipeline/14.png" />
       </td>
       <td>
         <img src="./pics/Advance Pipeline/15.png" />
       </td>
       <td>
         <img src="./pics/Advance Pipeline/16.png" />
       </td>
     </tr>
   </table>

3. Dynamic Renaming:

   <table>
     <tr>
       <td>
         <img src="./pics/Advance Pipeline/17.png" />
       </td>
       <td>
         <img src="./pics/Advance Pipeline/18.png" />
       </td>
     </tr>
   </table>

4. Dynamic Renaming Example:

   <table>
     <tr>
       <td>
         <img src="./pics/Advance Pipeline/19.png" />
       </td>
       <td>
         <img src="./pics/Advance Pipeline/20.png" />
       </td>
     </tr>
     <tr>
     	<td>
         <img src="./pics/Advance Pipeline/21.png" />
       </td>
     </tr>
   </table>

5. Renaming Dependencies Senarios:

   <table>
     <tr>
       <td>
         <img src="./pics/Advance Pipeline/22.png" />
       </td>
       <td>
         <img src="./pics/Advance Pipeline/23.png" />
       </td>
     </tr>
     <tr>
     	<td>
         <img src="./pics/Advance Pipeline/24.png" />
       </td>
     </tr>
   </table>



## 预测分支

1. Branch Prediction Overall Info:

   <img src="./pics/Advance Pipeline/30.png" style="zoom:30%;"/>

2. One Bit Predictor

   <table>
     <tr>
       <td>
         <img src="./pics/Advance Pipeline/42.png" />
       </td>
       <td>
         <img src="./pics/Advance Pipeline/43.png" />
       </td>
     </tr>
     <tr>
       <td>
         <img src="./pics/Advance Pipeline/31.png" />
       </td>
       <td>
         <img src="./pics/Advance Pipeline/32.png" />
       </td>
     </tr>
   </table>

3. Two Bit Predictor:

   <table>
     <tr>
       <td>
       	<img src="./pics/Advance Pipeline/33.png" />
       </td>
       <td>
       	<img src="./pics/Advance Pipeline/34.png" />
       </td>
     </tr>
     <tr>
       <td>
       	<img src="./pics/Advance Pipeline/40.png" />
       </td>
       <td>
       	<img src="./pics/Advance Pipeline/41.png" />
       </td>
     </tr>
   </table>

4. Branch History Table:

   <img src="./pics/Advance Pipeline/35.png" style="zoom:30%;">

5. Branch Target Buffer:

   <table>
     <tr>
       <td>
         <img src="./pics/Advance Pipeline/36.png" />
       </td>
       <td>
         <img src="./pics/Advance Pipeline/37.png" />
       </td>
     </tr>
     <tr>
       <td>
         <img src="./pics/Advance Pipeline/38.png" />
       </td>
       <td>
         <img src="./pics/Advance Pipeline/39.png" />
       </td>
     </tr>
   </table>





# 五、互连与通信

## 互连网络概念

- 定义：

  ​	由开关元件按一定拓扑结构和控制方法构成的网络以诗心啊计算机系统内部多个处理机或多个功能部件间的相互连接

- 操作方式：
  - 同步通信
  - 异步通信
- 控制策略
  - 集中控制
  - 分布控制

- 交换方式：
  - 电路交换
  - 分组交换
  - Wormhole交换

- 网络拓扑结构：
  - 静态网络
  - 动态网络

## 静态网络

### 一些定义

- 结点度：与结点相连接的边数，结点度 = 入度 + 出度
- 距离：与两个结点之间相连的最少边数
- 网络直径：网络中任意两个结点间距离的最大值
- 网络规模：网络中结点数，表示该网络功能连结部件的多少
- 等分宽度：某一网络被切成相等的两半时，沿切口的最小边数称为该网络的等分宽度。
- 结点间的线长：两个结点间的线的长度
- 对称性：从任何结点看，拓扑结构都一样，这种网络实现和编程都很容易。

### 静态网络

- 定义：静态网络由点与点直接相连而成，这种连接方式在程序执行过程中不会改变。	

- 列举：线性阵列、环、带弦环、链接、树形、树形扩展（带环树、二叉胖树）、星形、网格型和环网型、llliac网环形网（2D-Torus）、脉动式阵列、超立方体、带环立方体、k元n-立方体网络

### 动态网络

- 定义：网络的开关元件有源、链路可通过设置这些开关的状态来重构。只有在网络边界上的开关元件才能与处理机相连。

#### 互连函数

- 排列：N个数的每一种由确定次序的放置方法叫做一个N排列

- 置换：把一个N排列编程另一个N排列的变换叫做N阶置换。

- 置换方式：

  1. 恒等函数
  2. 方体函数
  3. 洗牌函数
  4. 逆洗牌函数
  5. 蝶式
  6. PM2i函数（加减2i）

  <table>
    <tr>
      <td>
      	<img src="./pics/Connectivity/1.png" />
      </td>
      <td>
      	<img src="./pics/Connectivity/2.png" />
      </td>
      <td>
      	<img src="./pics/Connectivity/3.png" />
      </td>
    </tr>
    <tr>
      <td>
      	<img src="./pics/Connectivity/4.png" />
      </td>
      <td>
      	<img src="./pics/Connectivity/5.png" />
      </td>
      <td>
      	<img src="./pics/Connectivity/6.png" />
      </td>
    </tr>
    <tr>
      <td>
      	<img src="./pics/Connectivity/7.png" />
      </td>
      <td>
      	<img src="./pics/Connectivity/8.png" />
      </td>
      <td>
      	<img src="./pics/Connectivity/9.png" />
      </td>
    </tr>
    <tr>
      <td>
      	<img src="./pics/Connectivity/10.png" />
      </td>
      <td>
      	<img src="./pics/Connectivity/11.png" />
      </td>
    </tr>
  </table>

#### 多级互连网络

1. 多级网络的三要素：

   <table>
     <tr>
       <td>
         <img src="./pics/Connectivity/12.png" />
       </td>
       <td>
         <img src="./pics/Connectivity/13.png" />
       </td>
       <td>
         <img src="./pics/Connectivity/14.png" />
       </td>
     </tr>
   </table>

2. $\Omega$网：

   <table>
     <tr>
       <td>
       	<img src="./pics/Connectivity/15.png" />
       </td>
       <td>
       	<img src="./pics/Connectivity/16.png" />
       </td>
       <td>
       	<img src="./pics/Connectivity/17.png" />
       </td>
     </tr>
     <tr>
       <td>
       	<img src="./pics/Connectivity/18.png" />
       </td>
       <td>
       	<img src="./pics/Connectivity/19.png" />
       </td>
       <td>
       	<img src="./pics/Connectivity/20.png" />
       </td>
     </tr>
     <tr>
       <td>
       	<img src="./pics/Connectivity/21.png" />
       </td>
       <td>
       	<img src="./pics/Connectivity/22.png" />
       </td>
     </tr>
   </table>

3. 蝶式网络 && 其他连接方式

   <table>
     <tr>
       <td>
         <img src="./pics/Connectivity/23.png" />
       </td>
       <td>
         <img src="./pics/Connectivity/24.png" />
       </td>
       <td>
         <img src="./pics/Connectivity/25.png" />
       </td>
     </tr>
   </table>

### 通信问题

#### 基本术语与性能指标

1. 消息、包和片

   <table>
     <tr>
       <td>
       	<img src="./pics/Connectivity/26.png" />
       </td>
       <td>
       	<img src="./pics/Connectivity/27.png" />
       </td>
     </tr>
   </table>

2. 互连网络

   <table>
     <tr>
       <td>
       	<img src="./pics/Connectivity/28.png" style="zoom:20%;" />
       </td>
     </tr>
   </table>

3. 传输时延与吞吐量

   <table>
     <tr>
       <td>
       	<img src="./pics/Connectivity/29.png" style="zoom:20%;" />
       </td>
     </tr>
   </table>

4. 传输时延的公式

   <table>
     <tr>
       <td>
       	<img src="./pics/Connectivity/30.png" />
       </td>
       <td>
       	<img src="./pics/Connectivity/31.png" />
       </td>
     </tr>
     <tr>
       <td>
       	<img src="./pics/Connectivity/32.png" />
       </td>
       <td>
       	<img src="./pics/Connectivity/33.png" />
       </td>
     </tr>
   </table>

5. 网络的拓扑结构

   <table>
     <tr>
       <td>
       	<img src="./pics/Connectivity/34.png" style="zoom:20%;" />
       </td>
     </tr>
   </table>

6. 网络的寻径算法

   <table>
     <tr>
       <td>
       	<img src="./pics/Connectivity/35.png" style="zoom:20%;" />
       </td>
     </tr>
   </table>

7. 网络的流控制

   <table>
     <tr>
       <td>
       	<img src="./pics/Connectivity/36.png" style="zoom:20%;" />
       </td>
     </tr>
   </table>

#### 寻径算法

- 存储转发

  <table>
    <tr>
      <td>
      	<img src="./pics/Connectivity/37.png" />
      </td>
      <td>
      	<img src="./pics/Connectivity/38.png" />
      </td>
    </tr>
  </table>

- 虚拟直通

  <table>
    <tr>
      <td>
      	<img src="./pics/Connectivity/39.png" style="zoom:20%;" />
      </td>
    </tr>
  </table>

- 线路交换

  <table>
    <tr>
      <td>
      	<img src="./pics/Connectivity/40.png" />
      </td>
      <td>
      	<img src="./pics/Connectivity/41.png" />
      </td>
    </tr>
  </table>

- Wormhole交换

  <table>
    <tr>
      <td>
      	<img src="./pics/Connectivity/42.png" />
      </td>
      <td>
      	<img src="./pics/Connectivity/43.png" />
      </td>
    </tr>
    <tr>
      <td>
      	<img src="./pics/Connectivity/44.png" />
      </td>
      <td>
      	<img src="./pics/Connectivity/45.png" />
      </td>
    </tr>
    <tr>
      <td>
      	<img src="./pics/Connectivity/46.png" />
      </td>
      <td>
      	<img src="./pics/Connectivity/47.png" />
      </td>
    </tr>
  </table>

#### 死锁和虚拟通道





# 六、向量处理

## Examples

<table>
  <tr>
    <td>
      <img src="./pics/Vector Machine/1.png" />
    </td>
    <td>
    	<img src="./pics/Vector Machine/2.png" />
    </td>
  </tr>
</table>

## 向量的数据表示

<table>
  <tr>
    <td>
    	<img src="./pics/Vector Machine/3.png" />
    </td>
    <td>
    	<img src="./pics/Vector Machine/4.png" />
    </td>
  </tr>
</table>

​    	<img src="./pics/Vector Machine/5.png" />



## 向量处理

![](./pics/Vector Machine/6.png)



# 七、数据级并发性



## 阵列机

![](./pics/DLP/1.png)

![](./pics/DLP/2.png)

![](./pics/DLP/3.png)

![](./pics/DLP/4.png)

![](./pics/DLP/5.png)

![](./pics/DLP/6.png)

## 脉动阵列机

<table>
	<tr>
		<td>
			<img src="./pics/DLP/7.png" style="zoom:70%;"/>
		</td>
		<td>
			<img src="./pics/DLP/8.png" style="zoom:120%;"/>
		</td>
	</tr>
  <tr>
		<td>
			<img src="./pics/DLP/9.png" />
		</td>
		<td>
			<img src="./pics/DLP/10.png" />
		</td>
	</tr>
  <tr>
		<td>
			<img src="./pics/DLP/11.png" />
		</td>
		<td>
			<img src="./pics/DLP/12.png" />
		</td>
	</tr>
  <tr>
		<td>
			<img src="./pics/DLP/13.png" />
		</td>
	</tr>
</table>

