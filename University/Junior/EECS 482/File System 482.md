## L16 FS

Hardware is heterogenous. File System is the abstraction of uniform file store.

**Driver**

The "bridge" between hardware and OS.

#### Disk

**Performance**

1. **Queue**: wait for the disk to be free
2. **Positioning**: move the disk arm
3. **Access**: transfer data from/to disk

**Efficiency** = transfer time / (positioning time + transfer time)

##### Flash RAM

Faster read (25$\mu s$) but slow write & lower power & better shock resistance

#### File Systems

File systems ensure data persistency across power outrages, machine crashes, etc.

##### FS Interface

Create, Delete, Read\<file, offset\>, Write\<file, offset\>

##### Indexed files

Similar to the concept of [[Virtual Memory 482#Paging|paging]], represent file as array of block pointers.

**Speed up sequential access**

Allocate new blocks that are on the same or nearby track/cylinder.

**Multi-level indexed files**

File = tree of block pointers



## L17-18 Transaction

Use a tree to store the mapping, which naturally represent directories.

#### Directory

Mapping information for a set of files: name -> file header's disk block.

User files belongs to the user, directories (can be viewed as OS files) belong to the OS.

#### File caching

How to improve performance? Cache data in memory.

Write through/ Write back (may fail to write some data if a crash happens)

#### Reliability

File system must ensure reliability, durability, and atomicity.

##### Shadowing

Keep two versions, switch the pointer to new version after all updates are done.

However, doing this is very costly

##### Logging

**Write-ahead logging** #link 484

1. Write updates to append-only log *before* applying updates to file system.
2. Copy new data from log to the in-place version of the file system.

##### Log-Structured File System

TBA

#### Inode

```c++
struct fs_direntry {  
    char name[FS_MAXFILENAME + 1];         // name of this file or directory  
    uint32_t inode_block;                  // disk block that stores the inode  
                                           // for this file or directory (0 if
                                           // this direntry is unused)};                                               

struct fs_inode {  
    char type;                             // file ('f') or directory ('d')  
    char owner[FS_MAXUSERNAME + 1];        // owner of this file or directory  
    uint32_t size;                         // size of this file or directory  
                                           // in blocks    uint32_t 
	blocks[FS_MAXFILEBLOCKS];              // array of data blocks for this  
                                           // file or directory};
};
```



## L19 RAID

Redundant Array of Inexpensive Disk.

Use many disks in parallel to increase storage bandwidth, improve reliability.

**Striping**

![[Pasted image 20221224132407.png]]

Fast, but low reliability

**Mirroring**

![[Pasted image 20221224132424.png]]

**Parity**

![[Pasted image 20221224132520.png]]

In reach stripe, use one block to store parity data as XOR of all data blocks in stripe.

Pros: much higher reliability

Cons: little degradation in write, some storage overhead, not ideal for small writes
