# Scalable Persistent Memory File System with Kernel-Userspace Collaboration

一种新的直接访问文件系统体系结构，其主要目标是**可伸缩性**。Kuco利用了三种关键技术——**协作索引、两级锁定和版本化读取**——来将耗时的任务，如路径名解析和并发控制，从内核卸载到用户空间，从而避免了内核处理瓶颈。在Kuco的基础上，我们介绍了KucoFS的设计和实现，然后实验表明KucoFS在广泛的实验中具有优异的性能；重要的是，KucoFS在元数据操作方面比现有文件系统高出一个数量级，并且充分利用了数据操作的设备带宽。

