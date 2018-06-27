
## LSM树（Log-Structured Merge Tree）存储引擎

[LSM](https://blog.csdn.net/u014774781/article/details/52105708)

LSM的思想，在于对数据的修改增量保持在内存中，达到指定的限制后将这些修改操作批量写入到磁盘中，相比较于写入操作的高性能，读取需要合并内存中最近修改的操作和磁盘中历史的数据，即需要先看是否在内存中，若没有命中，还要访问磁盘文件。原理：把一颗大树拆分成N棵小树，数据先写入内存中，随着小树越来越大，内存的小树会flush到磁盘中。磁盘中的树定期做合并操作，合并成一棵大树，以优化读性能。对应于使用LSM的Leveldb来说，对于一个写操作，先写入到memtable中，当memtable达到一定的限制后，这部分转成immutable memtable（不可写），当immutable memtable达到一定限制，将flush到磁盘中，即sstable.，sstable再进行compaction操作。
