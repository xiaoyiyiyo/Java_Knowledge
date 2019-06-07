#### Data Structure
##### *1. Log-structured* 
###### **主要构成**

* **append-only log**
* **hash index**

![79e3448da5805a72a328e39dc82053ae.png](evernotecid://D7132B30-5FF2-4477-A261-F127579C311C/appyinxiangcom/24850787/ENResource/p5)

###### **Issues**

* 提升寻找数据的速度
用到hash index，value记录byte offset。 
整个hashmap放在内存中
&nbsp;

* 避免数据量超过磁盘大小
拆分成variable-size segment
可以删除或关闭已满的segment
compaction segment里的数据 或 多个segments (可后台运行)
![b9b84af7b99117c0d3a1c83f9ff7de00.png](evernotecid://D7132B30-5FF2-4477-A261-F127579C311C/appyinxiangcom/24850787/ENResource/p2)
![6d83631d54b1db101e8273171b616d58.png](evernotecid://D7132B30-5FF2-4477-A261-F127579C311C/appyinxiangcom/24850787/ENResource/p4)
&nbsp;

* 如何删除记录
打上删除的标记，compaction的时候会删除打上删除标记的所有以前的该相关的记录(key一样)
&nbsp;

* database crash
在每个segment里存储各自的hashmap副本
&nbsp;

* 部分写入的问题
会校验checksums，删除掉不完整的记录
&nbsp;

* 并发控制
单线程写，并发读
            
###### **Advantage**

* sequential write is mush faster than random writes
(Comparing B-Trees and TSM-Trees)
&nbsp;

* Concurrency and crash recovery are much simpler if segment files are append- only or immutable
( you don’t have to worry about the case where a crash happened while a value was being overwritten, leaving you with a file con‐ taining part of the old and part of the new value spliced together.)
&nbsp;

* Merging old segments avoids the problem of data files getting fragmented over time.

###### **Limitations**
* the hash table in the memory. May be out of luck
&nbsp;
* Range queries are not efficient
&nbsp;

##### *2. SSTables and LSM-Trees*

###### ***SSTable*** 
>(Sorted String table)
>* the sequence of key-value pairs is sorted by key
>* each key only appears once within each merged segment file

&nbsp;

**Advantages**
* Merging segments is simple and efficient.
(MergeSort algorithm)
![f251163e027cb0d6c90d3d00ef5fc985.png](evernotecid://D7132B30-5FF2-4477-A261-F127579C311C/appyinxiangcom/24850787/ENResource/p6)
*When if the same key appears in several input segments?*
We can keep the value from the most recent segment and discard the values in older segments.
&nbsp;

* In order to find a particular key in the file, you no longer to keep an index of all the keys in memory.
&nbsp;

**Constructing and maintaining SSTables**
* on disk: B-Trees
* in memory: red-black trees or AVL trees

*One problem: if the database crashes, the most recent writes are lost;*
We can keep a separate log on disk to which every write is immediately appended, just like in the previous section.