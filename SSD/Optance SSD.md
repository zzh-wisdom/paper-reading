# Optance SSD

<https://zhuanlan.zhihu.com/p/27343166>
<https://pcper.com/2017/06/how-3d-xpoint-phase-change-memory-works/>

接口是PCIe 3.0 * 4 lanes，NVMe的界面，根据我们前面介绍，理论最高速度是4GB/s。官方标称速度为2400MB/s，我们实际测试结果在也符合标称。

因为**没有NAND需要的擦除-写入操作，所以可以原地写入**，所以不需要GC，也没有写放大，写入性能曲线异常平滑，这也是服务器异常看重的。

