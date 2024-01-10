## L11 MapReduce

**MapReduce** is a distributed system for computation. E.g. it can be used to build a search engine index table.

1. Distribute tasks to multiple machines
2. Partition and distribute input
3. Fault tolerance (Workers send periodic heartbeat message to Manager)

##### Pipeline

1. **Partition**: provided by MapReduce framework. Divide large input into smaller ones.
2. **Map**: a program written by the programmer. Output is `<key>\t<value>` pairs, group by keys.
3. **Group**: provided by MapReduce framework. Sort & merge.
4. **Reduce**: a program written by the programmer.


**Fault tolerance**



Functions have no side effects

#### P4 MapReduce

![[p4-1.jpg]]

**Manager map**

1. Partition the input files using round robin and send to workers.

**Worker map**

1. Run map on each file while partitioning output into temp directory.
2. Sort the files in temp directory and move to the output directory.

**Manager reduce**

1. Send the partition files to the corresponding worker.

**Worker reduce**

1. Merge (sort) all the partitions as the stdin of the reduce program.
2. Run reduce and output to the output directory.



## L12 Google File System

Intuition: we want to store huge files on different machines (serve more requests) while ensuring consistency.

Solution: distributed system for storage

**GFS**

Files stored as series of chunks

**Read**

1. Client send request of a file
2. Main server responds with location of each chunk of the file
3. Client get the entire file by sending request to the server for each chunk

**Concurrent write problem**

When two clients write to different chunks, inconsistency might happen.

Write need to be coordinated: every chunk server should have same value for the same chunk.

**GFS write forwarding**

 <img src="./image/12.jpg" style="zoom:30%;" />

**Relaxed consistency**

**Append**

Atomic

**Fault tolerance**

Use heartbeat message

Chunk server failure

Main server failure
