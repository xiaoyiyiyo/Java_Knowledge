# Data Structure

## *1. Log-structured*

### **主要构成**

* **append-only log**
* **hash index**

![Figure 3-1](https://github.com/xiaoyiyiyo/Java_Knowledge/blob/master/DDIA/png/Figure%203-1.png)

### **Issues**

* 提升寻找数据的速度
用到hash index，value记录byte offset。 
整个hashmap放在内存中

* 避免数据量超过磁盘大小
拆分成variable-size segment
可以删除或关闭已满的segment
compaction segment里的数据 或 多个segments (可后台运行)
![Figure 3-2](https://github.com/xiaoyiyiyo/Java_Knowledge/blob/master/DDIA/png/Figure%203-2.png)
![Figure 3-3](https://github.com/xiaoyiyiyo/Java_Knowledge/blob/master/DDIA/png/Figure%203-3.png)

* 如何删除记录  
打上删除的标记，compaction的时候会删除打上删除标记的所有以前的该相关的记录(key一样)

* database crash  
在每个segment里存储各自的hashmap副本

* 部分写入的问题  
会校验checksums，删除掉不完整的记录

* 并发控制  
单线程写，并发读

### **Advantage**

* sequential write is mush faster than random writes
(Comparing B-Trees and TSM-Trees)

* Concurrency and crash recovery are much simpler if segment files are append- only or immutable
( you don’t have to worry about the case where a crash happened while a value was being overwritten, leaving you with a file con‐ taining part of the old and part of the new value spliced together.)

* Merging old segments avoids the problem of data files getting fragmented over time.

### **Limitations**

* the hash table in the memory. May be out of luck

* Range queries are not efficient

## *2. SSTables and LSM-Trees*

### ***SSTable*** 

>(Sorted String table)
>
> * the sequence of key-value pairs is sorted by key
> * each key only appears once within each merged segment file

#### **Advantages**

* Merging segments is simple and efficient.
(MergeSort algorithm)
![Figure 3-4](https://github.com/xiaoyiyiyo/Java_Knowledge/blob/master/DDIA/png/Figure%203-4.png)

    _When if the same key appears in several input segments?_  
    We can keep the value from the most recent segment and discard the values in older segments.

* In order to find a particular key in the file, you no longer to keep an index of all the keys in memory.

#### **Constructing and maintaining SSTables**

* on disk: B-Trees
* in memory: red-black trees or AVL trees

*One problem: if the database crashes, the most recent writes are lost;*  
We can keep a separate log on disk to which every write is immediately appended, just like in the previous section.

### **LSM-tree**

> Log-Structured Merge-Tree
>
> The algorithm of merging and compacting sorted files

#### Performance optimizations

* slow when looking up keys that do not exist  
use additional ***Bloom filters***

* strategies to determine the order and timing of how SSTables are compacted and merged  
***size-tiered and leveled compaction***

    >size-tiered: (HBase)  
    >&emsp; newer and smaller SSTables are successively merged into older and larger SSTables
    >
    >Leveled compaction: (LevelDB and RocksDB)  
    >&emsp;the key range is split up into smaller SSTables and older data is moved into separate levels
