# 阅读笔记

## Basic performance measurements of the Intel Optane DC persistent memory module (2020)

给出了Optance SSD与NAND SSD的各个纬度的性能对比：

1. 4KB随机/顺序读写延迟
2. 吞吐量随队列深度的变化
3. 在背景压力负载下的IO延迟](#3-在背景压力负载下的io延迟)
4. 未对齐请求大小的性能
5. 垃圾回收的影响
6. 读干扰问题
7. 尾延迟

## Platform Storage Performance With 3D XPoint Technology (2017)

主要科普3D XPoint的三种使用方式：

1. 存储
2. 内存
3. 持久性内存

测试表明在一些常用的应用中Optance SSD你NAND SSD具有更高的性能和Qos

一些和上一遍论文相似的结论：

1. 控制器将单个4KB的IO分散到多个3D  XPoint内存通道中,以利用多个内存芯片的吞吐量, 为单个IO提供低延迟。这与基于nand的固态硬盘形成鲜明 对比,后者通常访问单个芯片以满足4KB(或更大)的IO
2. Optane固态硬盘包括一个用于从**主机到介质的正常读写的纯硬件路径**,避免了nand固态硬盘中经常出现的来自固件的额外延迟（即没有Read Buffer）。
3. Optane固态硬盘还能够就地写入数据,避免了nand固态硬盘所需的擦写驱动的垃圾收集。

## Performance Analysis of 3D XPoint SSDs in Virtualized and non-Virtualized Environments (2018)

测量了Optance SSD虚拟和非虚拟环境下的性能。我们这里只关注非虚拟环境。

和其他论文的测试结果类似，延迟上，4kb的随机/顺序读写都是13-15us。带宽上读2.5GB/s写2.1GB/s。而NAND SSD由于使用的是SATA接口，顺序读、随机写、顺序写延迟为30-50us。随机读延迟达到124us。读写带宽分别只有542MB/s和412MB/s

当IOPS率达到 95%的上限时,NAND,RAMDISK和Optane的延迟分别比 其基本水平增加了80%,54%和**25%**.这表明Optane在内部 花了更少的时间来处理数据.因此,Optane的延迟可以在 高并发工作负载中保持最佳状态.此外,当Optane达到其 IOPS极限(约58万)时,相应的吞吐量约为2.2  GB/s,这非常接近其传输带宽(2.5  GB/s).相反,NAND在达到其70k的IOPS上限时,得到 了280MB的吞吐量,这比其带宽(542MB/s)小得多.因 此,我们得出结论,Optane具有更好的并行性和可扩展性 ,在高并发工作负载方面表现更好.

> 这篇论文没什么东西

