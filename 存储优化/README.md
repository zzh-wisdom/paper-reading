# 存储优化

## Reducing DRAM Footprint with NVM in Facebook(2018)

> 该论文使用的是NVM块设备，应该是Optance SSD

**流行的基于ssd的键值存储区消耗了大量的DRAM，以提供高性能的数据库操作。然而，DRAM对于数据中心提供商来说可能会很昂贵**，特别是考虑到它最近的全球供应短缺导致了DRAM成本的增加。

像RocksDB和LevelDB这样的现代键值存储高度依赖于DRAM，即使使用持久存储介质(例如flash)存储数据也是如此。因为DRAM仍然比f快大约1000×因此，这些系统通常利用DRAM来缓存热索引和对象。

To give a sense of the price difference between NVM and DRAM, consider that on October 23, 2017, Amazon.com lists the price for an Intel Optane NVM device with 16 GB at $39, compared to $170 for 16 GB of DDR4 DRAM 2400MHz. Optance NVM价格比DRAM便宜

1. 分区的元数据索引，类似多级页表，减少DRAM需要缓存的元数据量

为了探索轮询的潜力，我们使用pvsync2I/O引擎运行了Fio，该引擎提供了轮询支持。图21(a)显示了在使用轮询时可用NVM带宽比使用中断的增加，然而，持**续的轮询会使整个核心的CPU消耗量达到100%**，如图21(b).所示为了克服这个问题，我们使用了一个混合轮询策略。在发出I/O后，**进程休眠一个固定的时间阈值，直到开始轮询**，如图20(c).所示图21(a)和图21(b)显示了带宽和CPU阈值为5us的混合轮询策略的执行。

一种动态混合轮询技术。内核不是使用预先配置的时间阈值，而是收集过去I/O的平均延迟，并基于其触发轮询。等待过去平均时间的某个百分比后，才开始轮询，例如等待平均时间的50%才开始轮询，这个比例可以通过参数控制。

## Let’s Talk About Storage & Recovery Methods for Non-Volatile Memory Database Systems(2015)

在本文中，我们评估了OLTP DBMS的不同存储和恢复方法，从纯NVM的存储层次开始。我们在一个DBMS中实现了三种存储引擎结构：(1)带日志的就地更新，(2)不带日志的读写拷贝更新，以及(3)日志结构的更新。

这些结果还表明，NVM优化的就地更新是最理想的方法，因为它具有最低的开销，对设备的磨损最小，并且允许DBMS几乎即时重启。

> 太老的论文了，感觉没必要看

## Separating Data via Block Invalidation Time Inference for Write Amplification Reduction in Log-Structured Storage （fast22）

它将写入的块分离为用户写入的块和GC重写的块。它通过推断每个区块的BIT来进一步分离每一组用户写的区块和GC改写的区块，从而对区块进行细粒度的分离，使其成为具有相似BIT的群体。

> 区块失效时间（block invalidation time, BIT）（即一个区块被一个活区块失效的时间；又称死亡时间[16]）。

数学论证一个理想数据放置策略的形式。

指出，太多开放的段（log）会造成介质的随机写，导致性能降低。

1. 用户写入的区块一般都有很短的生命期。SepBIT首先将区块分为用户编写的区块和GC重写的区块，因为它们的BIT模式不同。
2. Our intuition is that any user-written block that invalidates a short-lived block is also likely to be a short-lived block (§3.2).
3. Our intuition is that any GC-rewritten block with a smaller age has a higher probability to have a short residual lifespan
