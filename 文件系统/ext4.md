# EXT4

## Extent Tree

1. inode中最多可以直接存放3个extent
2. 使用一个inode flag标记基于extent file还是基于indirect block的file
3. 如果大于3个extend，则转换为一个B-Tree
4. 最后找到的extent将缓存在内存中

