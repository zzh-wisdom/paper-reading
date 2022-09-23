# 对象存储

> NVM的空间管理

## 背景总结

持久性内存的动态分配被大量用于建立高性能的应用,从索引结构 [8, 23, 24, 26-28],事务性内存[13, 19, 20, 39],到内存数据库系统[1, 10, 25, 31].内存分配器通常针对**易失性内存(如 DRAM)进行良好的调整**,以实现低延迟,高扩展性和低碎片化[2, 17, 33].持久性内存(如英特尔Optane DIMMs[11])的采用使研究人员**重新思考分配器的设计和实现**.为持久性内存设计的分配器需要保持DRAM分配器的突出特点,以实现高性能内存管理. 更重要的是,它们应该**强制执行崩溃一致性**,以便在故障发生后能够安全地恢复分配的内存对象.

[8] Zhangyu Chen, Yu Huang, Bo Ding, and Pengfei Zuo. 2020. Lock-Free Concurrent
Level Hashing for Persistent Memory. In Proceedings of the 2020 USENIX Annual
Technical Conference (ATC). 799–812.

[23] Se Kwon Lee, K Hyun Lim, Hyunsub Song, Beomseok Nam, and Sam H Noh.
1.    WORT: Write Optimal Radix Tree for Persistent Memory Storage Systems.
In Proceedings of the 15th USENIX Conference on File and Storage Technologies
(FAST). 257–270.

[24] Se Kwon Lee, Jayashree Mohan, Sanidhya Kashyap, Taesoo Kim, and Vijay Chidambaram. 2019. Recipe: Converting Concurrent DRAM Indexes to PersistentMemory Indexes. In Proceedings of the 27th ACM Symposium on Operating Systems
Principles (SOSP). 462–477.

[26] Jihang Liu, Shimin Chen, and Lujun Wang. 2020. LB+ Trees: Optimizing Persistent
Index Performance on 3DXPoint Memory. Proceedings of the VLDB Endowment
13, 7 (2020), 1078–1090.
[27] Baotong Lu, Xiangpeng Hao, Tianzheng Wang, and Eric Lo. 2020. Dash: Scalable
Hashing on Persistent Memory. Proc. VLDB Endow. 13, 8 (2020), 1147–1161.
[28] Shaonan Ma, Kang Chen, Shimin Chen, Mengxing Liu, Jianglang Zhu, Hongbo
Kang, and Yongwei Wu. 2021. ROART: Range-Query Optimized Persistent ART.
In Proceedings of the 19th USENIX Conference on File and Storage Technologies
(FAST). 1–16.


[13] Andreia Correia, Pascal Felber, and Pedro Ramalhete. 2018. Romulus: Efcient
Algorithms for Persistent Transactional Memory. In Proceedings of the 30th on
Symposium on Parallelism in Algorithms and Architectures(SPAA). 271–282.

[19] Jinyu Gu, Qianqian Yu, Xiayang Wang, Zhaoguo Wang, Binyu Zang, Haibing
Guan, and Haibo Chen. 2019. Pisces: A Scalable and Efcient Persistent Transactional Memory. In 2019 USENIX Annual Technical Conference (ATC). USENIX
Association, 913–928.

[20] Qingda Hu, Jinglei Ren, Anirudh Badam, Jiwu Shu, and Thomas Moscibroda. 2017.
Log-Structured Non-Volatile Main Memory. In Proceedings of the 2017 USENIX
Annual Technical Conference (ATC). 703–717.（**看一看**）

[39] Kai Wu, Jie Ren, Ivy Peng, and Dong Li. 2021. ArchTM: Architecture-Aware,
High Performance Transaction for Persistent Memory. In Proceedings of the 19th
USENIX Conference on File and Storage Technologies (FAST). 141–153.


[1] Joy Arulraj, Andrew Pavlo, and Subramanya R. Dulloor. 2015. Let’s Talk About
Storage & Recovery Methods for Non-Volatile Memory Database Systems. In
Proceedings of the 2015 ACM SIGMOD International Conference on Management of
Data (SIGMOD). Association for Computing Machinery, 707–722.

[10] Intel Corporation. 2018. Redis. https://github.com/pmem/redis/tree/3.2-nvml/.

[25] Lenovo. 2018. Memcached-PMEM. https://github.com/lenovo/memcachedpmem/

[31] Ismail Oukid, Daniel Booss, Adrien Lespinasse, Wolfgang Lehner, Thomas Willhalm, and Grégoire Gomes. 2017. Memory Management Techniques for LargeScale Persistent-Main-Memory Systems. Proceedings of the VLDB Endowment 10,
11 (2017), 1166–1177.

[11] Intel Corporation. 2020. Persistent Memory Development Kit. http://pmem.io/.


内存分配器：

[2] Emery D Berger, Kathryn S McKinley, Robert D Blumofe, and Paul R Wilson.
2000. Hoard: A Scalable Memory Allocator for Multithreaded Applications. ACM
Sigplan Notices 35, 11 (2000), 117–128.

[17] Jason Evans. 2021. jemalloc. https://github.com/jemalloc/jemalloc/.

[18] Inc Google. 2021. tcmalloc. https://github.com/google/tcmalloc.

[33] Bobby Powers, David Tench, Emery D Berger, and Andrew McGregor. 2019.
Mesh: Compacting Memory Management for C/C++ Applications. In Proceedings
of the 40th ACM SIGPLAN Conference on Programming Language Design and
Implementation. 333–346.

## Preserving Addressability Upon GC-Triggered Data Movements on Non-Volatile Memory

思想：NVM在垃圾回收时，通过间接指针来保持指针的有效性。实际上就是增加一层虚拟地址空间。

## NVAlloc: Rethinking Heap Metadata Management in Persistent Memory Allocators（ASPLOS ’22）

1. NVAlloc通过将slab中连续的数据块映射到存储在不同缓存线中的交错的元数据条目,消除了缓存线的重复刷新.
2. 它以顺序模式将小的元数据单元写入持久性记账（bookkeeping）日志,以消除持久性内存中堆元数据的随机访问
3. 它不使用静态的slab 隔离,而是支持slab morphing,这使得slab可以在不同大小的类别之间转换,以显著提高slab的使用率.

出发点：

- 现有的为持久性内存设计的分配器有很多与**堆元数据管理有关**的问题.首先,对堆元数据的小写入可能会导致高速缓存行的刷新.CPU缓存行的典型大小是64B [31].**在nvm_malloc中,一个位图的大小是8B.当 位图被反复更新时,同一高速缓存行应该被刷新以保持持久性**. 缓存行刷新的延迟比写的延迟高7.5倍[7].我们观察到,在四个著名的基准测试中,缓存行刷新的数量占到分配器引起的刷新操作总数的40.4%~99.7%(第3.1节).频繁的缓存行刷新操作导致持久性 内存分配器的性能下降.
- 其次,分配器的堆元数据往往在持久性内存中被随机访问.许多分配器(例如PMDK,PAllocator和Makalu[3])将堆划分为固定大小的块(例如4MB),以方便管理.他们在每个块的头空间维护记账元数据,该空间与数据空间分开,以避免元数据被错误地修改 .这种布局导致头被分布在整个堆空间中.在提供一连串的分配和取消分配请求后,分配者必须就地更新随机位于持久性内存中的头文件.最近的工作表明,持久性内存的随机访问性能比小写的顺序 访问性能要差得多[39, 40].因此,向堆元数据提供这些小的随机写 入,使分配器无法实现最佳性能.
- 第三,静态的Slab隔离导致持久性内存碎片化.这个问题在持久性 内存中更加严重,因为持久性堆是以文件的形式存储在DAX文件系统中的.它们不能通过重新启动系统来消除.所有持久性内存的分配器都使用大小分离的slab来分配小块.每个slab是一个或多个空闲block的容器,处理一个特定大小类的内存分配.分配给一个大小类的slab不能再用于其他大小类,即使这些板块大部分是空的, 而其他大小类的板块中也没有空闲空间[38].对于分配大小不断变化和频繁的 "删除 "操作的工作负载来说,这种隔离引起的碎片化增加了高达2.8倍的内存使用率(第3.2节).（相同的工作负载，需要更多的空间占用）

**分配器引起的高速缓存线重复刷新**：我们可以将重新刷新的距离量化为 访问独特缓存行的次数.例如,给定一个连续刷新的缓存行序列 (A,B,C,D,A),缓存行A的重新刷新距离为3.我们的实验表 明,当重新刷新距离从0增加到3时,缓存行重新刷新的延迟从800 ns 减少到500 ns. 在本文中,我们假设当一个缓存行的重新刷新距离 小于 4.否则,就会发生一次常规的刷新.我们选择4作为代表性的刷新距 离,因为我们观察到大多数缓存行的刷新距离都小于4,而且较 大的距离会导致较小的性能下降.缓存行重新刷新的平均延迟比持 久性内存中的随机和顺序写入分别高3倍和7倍[7].
>[7] Youmin Chen, Youyou Lu, Fan Yang, Qing Wang, Yang Wang, and Jiwu Shu. 2020. FlatStore: An Efcient Log-Structured Key-Value Storage Engine for Persistent Memory. In Proceedings of the Twenty-Fifth International Conference on Architectural Support for Programming Languages and Operating Systems (ASPLOS). 1077–1091.

对 管理1GB的活堆数据,现有的分配器需要的内存用量高达2.8GB.这 个结果表明持久性内存的利用率严重不足.原因是现有分配器中使 用的静态板块隔离,通过分配更多的其他大小类别的板块来响应 请求大小的变化[21].它不能使用不同大小等级的现有板块中的 自由空间.这是因为分配器在运行时不能改变一个板块的大小类别 ,直到它完全空闲. 持久性内存中的静态板块隔离比易失性内存中的影响更大,因为 内存碎片不能通过重新启动来消除.
> [21] Mark S Johnstone and Paul R Wilson. 1998. The Memory Fragmentation Problem: Solved? ACM Sigplan Notices 34, 3 (1998), 26–36.

本文设计：强调有效地消除缓存行刷新和小的随机写入,并减轻堆元数据管理中的板块引起的内存碎片.

1. NVAlloc使用从数据块到其相应的堆元数据的交错内存映射,以及线程本地缓存中链接列表的交错布局,以避免重复访问同一CPU缓存行.
2. 由于原地元数据更新会导致持久性内存中的随机访问,而写缓冲区的大小是有限的[40]（即元数据量是有限的）,我们增加了一个持久性的记账日志, 以顺序模式存储小的元数据更新（这里我们可以利用之前那篇论文关于log的优化方法）.因此,我们从malloc()和free()的关键 路 径 中 完 全 移 除 随 机 元 数 据 访 问.
3. 它 支 持 slab  morphing,在slab变换过程中,两个大小级别的块可以共同位于 一个slab中.因此,低内存使用率的slab中的自由空间可以得到很好的利用,slab元数据管理的运行时间开销为4.5%.当一个板块大 部分时间是空闲的,但不能用来为其他大小类别的请求提供服务时,板块变形是自动启用的.

为了避免内存泄漏，我们采用了其他分配器[11,31,37]中使用的nvalloc_malloc_to()和nvalloc_free_from()API，分别在持久内存上进行原子分配和释放对象

NVAllc-log基于WAL，保证崩溃一致性，恢复快，但前台性能较差
NVAlloc-GC基于重启扫描的方式解决内存泄漏问题，前台性能好，但恢复较慢。
> 这两种方式的优劣在ROART论文中有总结，可以对比那种方式好。

## Log-Structured Non-Volatile Main Memory(ATC 17)

背景：

1. 传统分配器的空间碎片问题，甚至造成只有50%的空间利用率

相比之下，日志结构的方法很容易避免内部碎片化，因为新的分配被紧凑地附加到日志端。它通过移动分配的数据和巩固空闲空间，而不需要暂停这个过程。

2. 过多的写入流量，以往的log-structure内存管理器将数据和log单独分开（当然这里也有可能是undo log），数据需要先写log在写到原本的数据位置，需要写两遍，导致NVM过度磨损。

我们选择一个相对较小的块大小（如32KB），因为典型的NVMM写入量很小；此外，小尺寸的单个块可以以增量方式更快地被清理和回收。

低于20%就进行空间碎片整理。

单独的日志：我们观察到内存存储总是比内存分配具有更好的局部性。这意味着将它们混合在一个日志中可能会增加日志清洗成本，并减少发生的机会快速清洁。因此，我们为每个线程设计了单独的日志，服务内存存储的更新日志，服务内存分配的分配日志和只存储墓碑的deallocation日志（启发：alloc和update log可以每个线程单独一个，并按照cacheline对齐减少reflush。free log可以交错的方式）

## 启发总结

我认为持久性内存分配器，不能想内存那样只负责管理空间分配，它和DRAM不同，DRAM分配器不需要考虑故障恢复。而NVM的分配器很有可能因为故障而导致空间泄漏，比如用户分配一块内存空间后，还没来的及将指针持久化，就崩溃了，那么该空间就会泄漏。所以NVM的空间管理器不应该仅仅实现常规的分配功能，而应类似pmemobj库那样，提供对象的接口（事务的修改接口，但开销很大）。或者提供额外的接口与上层应用配合实现崩溃一致性。

一般会将介质按照4MB的chunk进行分割，感觉chunk的元数据管理可以集中存放，防止小的随机写。可以和基于log的方式进行对比。
> 但slab的元数据，只能分散了，因为不同的chunk划分的方式不一样，无法集中

元数据：Interleaved Mapping


持久性NVM分配器相关参考：

[3] Kumud Bhandari, Dhruva R Chakrabarti, and Hans-J Boehm. 2016. Makalu: Fast
Recoverable Allocation of Non-Volatile Memory. ACM SIGPLAN Notices 51, 10
(2016), 677–694.

[4] Wentao Cai, Haosen Wen, H Alan Beadle, Chris Kjellqvist, Mohammad Hedayati,
and Michael L Scott. 2020. Understanding and Optimizing Persistent Memory
Allocation. In Proceedings of the 2020 ACM SIGPLAN International Symposium on
Memory Management (ISMM). 60–73.

[15] Anthony Demeri, Wook-Hee Kim, R Madhava Krishnan, Jaeho Kim, Mohannad
Ismail, and Changwoo Min. 2020. Poseidon: Safe, Fast and Scalable Persistent
Memory Allocator. In Proceedings of the 21st International Middleware Conference
(Middleware). 207–220.

[30] Iulian Moraru, David G Andersen, Michael Kaminsky, Niraj Tolia, Parthasarathy
Ranganathan, and Nathan Binkert. 2013. Consistent, Durable, and Safe Memory
Management for Byte-Addressable Non-Volatile Main Memory. In Proceedings of
the First ACM SIGOPS Conference on Timely Results in Operating Systems (TRIOS).
1–17.

[31] Ismail Oukid, Daniel Booss, Adrien Lespinasse, Wolfgang Lehner, Thomas Willhalm, and Grégoire Gomes. 2017. Memory Management Techniques for LargeScale Persistent-Main-Memory Systems. Proceedings of the VLDB Endowment 10,
11 (2017), 1166–1177.

[37] David Schwalb, Tim Berning, Martin Faust, Markus Dreseler, and Hasso Plattner.
1.    nvm malloc: Memory Allocation for NVRAM. ADMS@ VLDB 15 (2015),
61–72.

对于分配小对象,板块被广泛用于现有的分配器中,包括易失性 内存分配器(如jemalloc-).5.2.1[17]和tcmalloc-2.9.1[18])和持久性内存分配器(例如Makalu[3]，Ralloc[4]，nvm_malloc[37]和PMDK-1.11[11]).

我们运行fragmentation基准测试[35](我们称之为Fragbench)来研究流行的分配 器的内存使用.Fragbench有三个执行阶段.之前,删除,和之 后.在之前和之后阶段,Fragbench使用预先定义的大小分布中的 对象分配5GB的内存,并随机删除现有的对象,以保持实时数据量不 超过1GB.在删除阶段,Fragbench随机地删除了对象.这三个阶 段是按顺序执行的.我们改变了对象的大小分布和删除对象的比例 ,在四个有代表性的工作负载2(W1-W4,如表1所示)中,这些工 作负载来自于基准,以涵盖现实世界应用的广泛特点.类似的工作 负载已被用于先前的研究(即RAMCloud[35],PAllocator[31] 和日志结构的NVMM[20]).

> [20] Qingda Hu, Jinglei Ren, Anirudh Badam, Jiwu Shu, and Thomas Moscibroda. 2017. Log-Structured Non-Volatile Main Memory. In Proceedings of the 2017 USENIX Annual Technical Conference (ATC). 703–717.

[46] RUMBLE, S. M., KEJRIWAL, A., AND OUSTER- HOUT, J. Log-structured memory for DRAM- based storage. In Proceedings of the 12th USENIX Conference on File and Storage Technologies (2014), FAST ’14, pp. 1–16.

[9] CHATZISTERGIOU, A., CINTRA, M., AND VI- GLAS, S. D. REWIND: Recovery Write-ahead sys-
tem for In-memory Non-volatile Data-structures. Proc. VLDB Endow. 8, 5 (Jan. 2015), 497–508.

分类log
[29] LEE, C., SIM, D., HWANG, J., AND CHO, S. F2FS: A new file system for flash storage. In 13th USENIX Conference on File and Storage Technolo- gies (2015), FAST ’15, pp. 273–286.

[52] XU, J., AND SWANSON, S. NOVA: A log- structured file system for hybrid volatile/non- volatile main memories. In Proceedings of the 14th Usenix Conference on File and Storage Technolo- gies (2016), FAST ’16, pp. 323–338.