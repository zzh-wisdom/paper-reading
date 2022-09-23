# paper-reading

- [1. TODO](#1-todo)
- [2. 参考文献](#2-参考文献)
  - [2.1. 方向展望](#21-方向展望)
  - [2.2. **NVMe SSD：**](#22-nvme-ssd)
  - [2.3. **flash SSD：**](#23-flash-ssd)
  - [2.4. **Optance SSD**](#24-optance-ssd)
  - [2.5. AEP](#25-aep)
  - [2.6. 多层缓存](#26-多层缓存)
  - [2.7. Flash SSD Cache](#27-flash-ssd-cache)
  - [2.8. 数据放置相关](#28-数据放置相关)
  - [2.9. 存储系统](#29-存储系统)
  - [2.10. 工程/测试](#210-工程测试)
  - [2.11. 混合系统](#211-混合系统)
  - [2.12. Cache 算法](#212-cache-算法)
  - [2.13. 已有系统研究](#213-已有系统研究)
  - [2.14. 工程实践](#214-工程实践)
  - [2.15. 可用的参考文献](#215-可用的参考文献)

论文阅读笔记

例如 fio、blktests 和 util-linux。

## 1. TODO

1. 关键字 分层存储管理 Hierarchical storage management
2. 3D Xpoint memory, nonvolatile memory, solid-state drive, Optane SSD, performance evaluation

## 2. 参考文献

### 2.1. 方向展望

舒继武，陆游游，张佳程，郑纬民. 基于非易失性存储器的存储系统技术研究进
展. 科技导报，2016，34（14）：86-94

### 2.2. **NVMe SSD：**

Qiumin Xu, Huzefa Siyamwala, Mrinmoy Ghosh, Tameesh Suri, Manu Awasthi, Zvika Guz, Anahita Shayesteh, and
Vijay Balakrishnan. 2015. Performance analysis of NVMe SSDs and their implication on real world databases. In
Proceedings of the 8th ACM International Systems and Storage Conference. ACM, 6.

### 2.3. **flash SSD：**

Mark Moshayedi and Patrick Wilkison. 2008. Enterprise SSDs. Queue 6, 4 (2008), 32–39.

Sang-Won Lee, Bongki Moon, and Chanik Park. 2009. Advances in flash memory SSD technology for enterprise
database applications. In Proceedings of the 2009 ACM SIGMOD International Conference on Management of Data.
ACM, 863–870.

Sang-Won Lee, Bongki Moon, Chanik Park, Jae-Myung Kim, and Sang-Woo Kim. 2008. A case for flash memory SSD
in enterprise database applications. In Proceedings of the 2008 ACM SIGMOD International Conference on Management
of Data. ACM, 1075–1086.

[1] F. Chen, D. A. Koufaty, and X. Zhang, “Understanding intrinsic
characteristics and system implications of flash memory based
solid state drives,” in Proc. 11th Int. Joint Conf. Meas. Model. Comput. Syst., 2009, pp. 181–192.
[2] C. Min, K. Kim, H. Cho, S.-W. Lee, and Y. I. Eom, “SFS: Random
write considered harmful in solid state drives,” in Proc. 10th USENIX Conf. File Storage Technol., Feb. 2012, pp. 139–154.
[3] J. Ou, J. Shu, Y. Lu, L. Yi, and W. Wang, “EDM: An enduranceaware data migration scheme for load balancing in SSD storage
clusters,” in Proc. 28th Int. Parallel Distrib. Process. Symp.,
May 2014, pp. 787–796.

### 2.4. **Optance SSD**

1. Jinfeng Yang, Bingzhe Li, David J. Lilja. Exploring Performance Characteristics of the Optane 3D-XPoint Storage Technology. ACM Transactions on Modeling and Performance Evaluation of Computing Systems, 2020, 5(1): 1-28
2. Frank T. Hady, Annie Foong, Bryan Veal, and Dan Williams. 2017. Platform storage performance with 3D XPoint technology. Proceedings of the IEEE 105, 9 (2017), 1822–1833.


Intel Corporation. 2014. Intel SSD DC P3700 Series. https://ark.intel.com/content/www/us/en/ark/products/series/
79628/intel-ssd-dc-p3700-series.html.

Intel Corporation. 2018. Product Brief: Intel Optane SSD DC P4800X Series. Retrieved from https://www.intel.com/content/www/us/en/solid-state-drives/optane-ssd-dc-p4800x-brief.html.

Intel Corporation. 2018. World’s Most Responsive Data Center SSD. Retrieved from https://www.intel.com/content/
dam/www/public/us/en/documents/product-briefs/optane-ssd-dc-p4800x-brief.pdf.

### 2.5. AEP

1. Takahiro Hirofuchi, Ryousei Takano. A Prompt Report on the Performance of Intel Optane DC Persistent Memory Module. IEICE Transactions on Information and Systems, 2020, 103(5): 1168-1172
2. Joseph Izraelevitz, Jian Yang, Lu Zhang, Juno Kim, Xiao Liu, Amirsaman Memaripour, Yun Joon Soh, Zixuan Wang,Yi Xu, Subramanya R. Dulloor, Jishen Zhao, and Steven Swanson. 2019. Basic performance measurements of the Intel Optane DC persistent memory module. arXiv preprint arXiv:1903.05714 (2019).
3. A. van Renen, L. Vogel, V. Leis, T. Neumann, and A. Kemper, “Persistent memory I/O primitives” CoRR, vol.abs/1904.01614, 2019.
4. Jiachen Zhang, Peng Li, Bo Liu, Trent G. Marbach, Xiaoguang Liu, and Gang Wang. 2018. Performance analysis of 3D XPoint SSDs in virtualized and non-virtualized environments. In 2018 IEEE 24th International Conference on Parallel and Distributed Systems (ICPADS’18). IEEE, 1–10.
5. Jian Yang, Juno Kim, Morteza Hoseinzadeh, Joseph Izraelevitz, and Steven Swanson. An empirical guide to the behavior and use of scalable persistent memory. arXiv preprint arXiv:1908.03583, 2019.

A. van Renen, V. Leis, A. Kemper, T. Neumann, T. Hashida, K. Oe, Y. Doi, L. Harada,
and M. Sato. Managing non-volatile memory in database systems. In SIGMOD,
2018

[55] JinyoungOhandYoungjinKwon.PersistentMemoryAwarePerfor- mance Isolation with Dicio. In Proceedings of the 12th ACM SIGOPS Asia-Pacific Workshop on Systems, pages 97–105, 2021.

[73]

### 2.6. 多层缓存

1. J. Arulraj, A. Pavlo, and K. T. Malladi. Multi-tier buffer management and storage system design for non-volatile memory. arXiv, 2019.
2.

E. J. O’Neil, P. E. O’Neil, and G. Weikum. The lru-k page replacement algorithm for database disk buffering. In SIGMOD, pages 297–306. ACM, 1993.

S. Kirkpatrick, C. D. Gelatt, and M. P. Vecchi. Optimization by
simulated annealing. science, 220(4598):671–680, 1983.

[11] Dejun Jiang, Yukun Che, Jin Xiong, and Xiaosong Ma. ucache: A utility-aware multilevel ssd cache manage- ment policy. In 2013 IEEE 10th International Con- ference on High Performance Computing and Commu- nications & 2013 IEEE International Conference on Embedded and Ubiquitous Computing, pages 391–398. IEEE, 2013.

[13] R. Koller, A. J. Mashtizadeh, and R. Rangaswami. Centaur: Host-side ssd caching for storage performance
control. In 2015 IEEE International Conference on
Autonomic Computing, pages 51–60, July 2015.

ARC
[17] Nimrod Megiddo and Dharmendra S. Modha. Arc: A
self-tuning, low overhead replacement cache. In Proceedings of the 2Nd USENIX Conference on File and
Storage Technologies, FAST ’03, pages 115–130, Berkeley, CA, USA, 2003. USENIX Association.

[20] Daniel Sanchez and Christos Kozyrakis. Vantage: scalable and efficient fine-grain cache partitioning. In Proceedings of the 38th annual international symposium on
Computer architecture, pages 57–68, 2011.

[21] P. Sharma, P. Kulkarni, and P. Shenoy. Per-vm page
cache partitioning for cloud computing platforms. In
2016 8th International Conference on Communication
Systems and Networks (COMSNETS), pages 1–8, Jan
2016.


NSDI21：Segcache

HPCA21 Designing a Cost-Effective Cache Replacement Policy using Machine Learning

### 2.7. Flash SSD Cache

[4] H. Kim and S. Ahn, “BPLRU: A buffer management scheme for
improving random writes in flash storage,” in Proc. 6th USENIX
Conf. File Storage Technol., 2008, pp. 16:1–16:14.
[5] H. Jung, H. Shim, S. Park, S. Kang, and J. Cha, “LRU-WSR: Integration of LRU and writes sequence reordering for flash memory,” IEEE Trans. Consum. Electron., vol. 54, no. 3, pp. 1215–1223,
Aug. 2008.
[6] S.-Y. Park, D. Jung, J.-U. Kang, J.-S. Kim, and J. Lee, “CFLRU: A
replacement algorithm for flash memory,” in Proc. Int. Conf. Compilers Archit. Synthesis Embedded Syst., 2006, pp. 234–241.
[7] Z. Fan, D. H. C. Du, and D. Voigt, “H-ARC: A non-volatile memory based cache policy for solid state drives,” in Proc. 30th Symp.
Mass Storage Syst. Technol., Jun. 2014, pp. 1–11.
[8] S. Kang, S. Park, H. Jung, H. Shim, and J. Cha, “Performance
trade-offs in using NVRAM write buffer for flash memory-based
storage devices,” IEEE Trans. Comput., vol. 58, no. 6, pp. 744–758,
Jun. 2009.
[9] H. Jo, J.-U. Kang, S.-Y. Park, J.-S. Kim, and J. Lee, “FAB: Flashaware buffer management policy for portable media players,”
IEEE Trans. Consum. Electron., vol. 52, no. 2, pp. 485–493,
May 2006.
[10] G. Wu, X. He, and B. Eckart, “An adaptive write buffer management scheme for flash-based SSDs,” ACM Trans. Storage, vol. 8,
no. 1, pp. 1:1–1:24, Feb. 2012.
[11] Q. Wei, C. Chen, and J. Yang, “CBM: A cooperative buffer management for SSD,” in Proc. 30th Symp. Mass Storage Syst. Technol.,
Jun. 2014, pp. 1–12.

### 2.8. 数据放置相关

[23] J. Li, Q. Wang, P. P. C. Lee, and C. Shi. An in-depth analysis of cloud block storage workloads in large-scale production. In Proc. of IEEE IISWC, 2020.

[24] Y. Zhang, P. Huang, K. Zhou, H. Wang, J. Hu, Y. Ji, and B. Cheng. OSCA: An online-model based cache
allocation scheme in cloud block storage systems. In Proc. of USENIX ATC, 2020.

[33] M. Shafaei, P. Desnoyers, and J. Fitzpatrick. Write amplification reduction in flash-based SSDs through extent-based temperature identification. In Proc. of USENIX HotStorage, 2016.

[43] J. Yang, S. Pei, and Q. Yang. WARCIP: Write ampli- fication reduction by clustering I/O pages. In Proc. of ACM SYSTOR, 2019.

[39] Q. Wang, J. Li, P. P. C. Lee, T. Ouyang, C. Shi, and L. Huang. Separating data via block invalidation time inference for write amplification reduction in log- structured storage. Technical report, The Chinese Uni- versity of Hong Kong, 2022. https://www.cse.cuhk. edu.hk/~pclee/www/pubs/tech_sepbit.pdf.

### 2.9. 存储系统

1. Assaf Eisenman, Darryl Gardner, Islam AbdelRahman, Jens Axboe, Siying Dong, Kim Hazelwood, Chris Petersen, Asaf Cidon, and Sachin Katti. 2018. Reducing DRAM footprint with NVM in Facebook. In Proceedings of the 13th EuroSys Conference. ACM, 42.
2. J. Arulraj, A. Pavlo, and S. Dulloor. Let’s talk about storage & recovery methods
for non-volatile memory database systems. In SIGMOD, pages 707–722, 2015.（基于模拟的NVM，关系数据库，不太相符）

[31] Badrish Chandramouli, Guna Prasaad, Donald Koss- mann, Justin Levandoski, James Hunter, and Mike Bar- nett. Faster: A concurrent key-value store with in-place updates. In Proceedings of the 2018 International Con- ference on Management of Data, pages 275–290, 2018.

[16] E. Lee, H. Kang, H. Bahn, and K. G. Shin, “Eliminating periodic
flush overhead of file I/O with non-volatile buffer cache,” IEEE
Trans. Comput., vol. 65, no. 4, pp. 1145–1157, Apr. 2016.

### 2.10. 工程/测试

MSR trace [42]
Dushyanth Narayanan, Austin Donnelly, and Antony Rowstron. 2008. Write off-loading: Practical power management for enterprise storage. ACM Transactions on Storage (TOS) 4, 3 (2008), 10.

the TPC-H queries were captured by blktrace [9] and analyzed.
Alan D. Brunelle. 2006. Block I/O layer tracing: blktrace. HP, Gelato-Cupertino, CA.

和Iometer[29]
Open Source Development Lab. Iometer.
[Online]. Available: http://iometer.org/

### 2.11. 混合系统

Tian Luo, Rubao Lee, Michael Mesnier, Feng Chen, and Xiaodong Zhang. 2012. hStorage-DB: Heterogeneity-aware
data management to exploit the full capability of hybrid storage systems. Proceedings of the Conference on Very Large
Data Bases (VLDB) Endowment 5, 10 (2012), 1076–1087

### 2.12. Cache 算法

Mustafa Canim, George A. Mihaila, Bishwaranjan Bhattacharjee, Kenneth A. Ross, and Christian A. Lang. 2010. SSD
bufferpool extensions for database systems. Proceedings of the VLDB Endowment 3, 1–2 (2010), 1435–1446.

驱逐决定会影响到缓存在失误率方面的有效性，因此成为许多先前工作的重点。

[22] Nathan Beckmann, Haoxian Chen, and Asaf Cidon. Lhd : Improving cache hit rate by maximizing hit density. In 15th USENIX Symposium on Networked Systems Design and Implementation (NSDI 18), pages 389–403, 2018.

[26] Aaron Blankstein, Siddhartha Sen, and Michael J Freed- man. Hyperbolic caching: Flexible caching for web applications. In 2017 USENIX Annual Technical Con- ference (USENIX ATC 17), pages 499–511, 2017.

[49] Conglong Li and Alan L Cox. Gd-wheel: a cost-aware replacement policy for key-value stores. In Proceedings of the Tenth European Conference on Computer Systems, pages 1–15, 2015.

[56] Elizabeth J O’neil, Patrick E O’neil, and Gerhard Weikum. The lru-k page replacement algorithm for database disk buffering. Acm Sigmod Record, 22(2):297– 306, 1993.

[69] Linpeng Tang, Qi Huang, Wyatt Lloyd, Sanjeev Kumar, and Kai Li. Ripq : Advanced photo caching on flash for facebook. In 13th USENIX Conference on File and Storage Technologies (FAST 15), pages 373–386, 2015.

关于cache算法

[27] LeeBreslau,PeiCao,LiFan,GrahamPhillips,andScott Shenker. Web caching and zipf-like distributions: Evi- dence and implications. In IEEE INFOCOM’99. Con- ference on Computer Communications. Proceedings. Eighteenth Annual Joint Conference of the IEEE Com- puter and Communications Societies. The Future is Now (Cat. No. 99CH36320), volume 1, pages 126–134. IEEE, 1999.

[39] Gil Einziger, Roy Friedman, and Ben Manes. Tinylfu: A highly efficient cache admission policy. ACM Trans- actions on Storage (ToS), 13(4):1–31, 2017.

[61] John T Robinson and Murthy V Devarakonda. Data cache management using frequency-based replacement. In Proceedings of the 1990 ACM SIGMETRICS confer- ence on Measurement and modeling of computer sys- tems, pages 134–142, 1990.

[64] Dimitrios N Serpanos and Wayne H Wolf. Caching web objects using zipf’s law. In Multimedia Storage and Archiving Systems III, volume 3527, pages 320–326. International Society for Optics and Photonics, 1998.

[35] Ludmila Cherkasova. Improving WWW proxies perfor- mance with greedy-dual-size-frequency caching policy. Hewlett-Packard Laboratories, 1998.

[1] Approximate counting algorithm. https: //en.wikipedia.org/wiki/Approximate_ counting_algorithm. Accessed: 2020-08-06.

[38] Gil Einziger, Ohad Eytan, Roy Friedman, and Ben Manes. Adaptive software cache management. In Pro- ceedings of the 19th International Middleware Confer- ence, pages 94–106, 2018.

### 2.13. 已有系统研究

Jian Xu and Steven Swanson. NOVA: A Log-structured File System for Hybrid Volatile/Non-volatile Main
Memories. In 14th USENIX Conference on File and Storage Technologies (FAST 16), pages 323–338, Santa
Clara, CA, February 2016. USENIX Association.

### 2.14. 工程实践

1. AEP的管理方法，ndctl。如何配置 interleaved
2. Intel Memory Latency Checker (MLC) reported.Intel MLC v3.6

### 2.15. 可用的参考文献

From bottom up, LSNVMM uses DAX [32] through a filesystem that allows direct access to physical NVMM device via a memory map

> [32] LINUX KERNEL ORGANIZATION, INC. Direct access for files. https://www.kernel.org/ doc/Documentation/filesystems/dax. txt, 2016.
