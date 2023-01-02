## L11 Paging

#### Address space Abstraction

1. **Virtual memory**: an address space can be larger than the physical memory.
2. **Address independence**: same numeric address can be used in different address space.
3. **Protection** one process can't access data in another process's address space.

##### MMU

Memory Mange Unit, a hardware implementation translated all memory references.

![[Pasted image 20221223202207.png]]

#### Old Translation Methods

##### Base and Bounds

Cons: limited by physical memory, external fragmentation.

##### External fragmentation

Processes come and go, leaving a mishmash of available memory regions.

![[Pasted image 20221223202507.png]]

##### Segmentation

![[Pasted image 20221223202606.png]]

###### Segmentation Fault

A segment is not allocated.

#### Paging

Memory are allocated in fixed-size units (pages).

Virtual address = Virtual page number + Offset

Use a [[Memory System 370#Page Table|page table]] to translate virtual page into physical page.

##### Context switch

The page table should be changed during a context switch. We only need to change Page Table Base Register (PTBR). #indirection

##### Paging Out

Each virtual page can be in physical memory or "paged out" to disk, represented by *resident* bit.

The *valid* bit indicate whether the virtual page is valid, the *read/write* bits are for protection.

##### Multi-level paging

Represents page table as a tree, saving space.

##### [[Memory System 370#Translation Lookaside Buffer|TLB]]

Has to clear the TLB table in context switch (TLB flush).



## L12 Eviction

Which page to evict when you need a free page?

##### FIFO

Replace page brought into memory longest time ago. However, these pages might be ones that are frequently used.

##### [[Memory System 370#LRU|LRU]]

However, LRU is hard to implement exactly.

#### Clock Replacement Algorithm

MMU maintain a "referenced" bit for each resident page. It is set by MMU when page is accessed, and can be cleared by OS.

When we have to evict, evict the first page that is no referenced.

The newly referenced page is put at the end of the block queue.

![[Pasted image 20221223205630.png]]

Since page table is physical implementation, we want to reduce state as much as possible, and let OS store the whole meta data.

1. No valid bit: invalid pages are non-resident
2. No resident bits: clear r/w bits to imply non-resident
3. No dirty bit: use write bit to imply dirty
4. No referenced bit: clear r/w bits to imply not referenced



## L13 Kernel vs. User

Kernel sets up page table for process and manages their virtual memory.

#### Kernel's Address Space

Where do we store page tables?

1. In physical memory
2. In kernel's virtual address space

Difference: PTBR is physical vs. virtual

Pros of 2: can page out user page tables. Note that kernel page table must be kept in physical memory. (Otherwise we can never translate kernel virtual address to physical address)

We can evict kernel's virtual pages except code for handling paging in/out.

Kernel can directly issue untranslated address (bypass MMU), and can map physical memory into a portion of its virtual address space.

#### Kernel Access User's Address Space

We can map kernel address space into every process's address space, so that trap to kernel doesn't change address space. Instead, it just enables access to both OS and user parts of that address space.

![[Pasted image 20221223221529.png|300]]

#### Kernel vs. User Mode

Only kernel can modify the translation data (page table).

Hardware support: CPU has a mode bit

##### Switching from user process into kernel

**Faults (traps) and interrupts**

Time interrupts, page faults

**System calls**

Process management, I/O, reboot

When you call `cin` in C++, it calls `read()`, which executes assembly-language instruction `syscall`. It traps to kernel at pre-specified location.

**Trap**

Hardware ==atomically==

1. Set mode bit to kernel
2. Save registers, PC, SP
3. Changes SP to kernel stack
4. Changes to kernel's address space
5. Jumps to exception handler

##### Interrupt Vector Tables

Store the location of code segment for interrupt handling.

No one can change this table after boot, so transitions are limited, and security is ensured.



## L14 Multi-Process and Fork

#### Multi-Process Issues

How to partition physical memory allocation among processes? What if many large processes all actively used their entire address space?

##### Thrashing

Performance degrades rapidly as miss rate goes up.

S: longer time slice

##### Working Set

All pages used in last T seconds. The sum of all working sets should fit in memory.

Increase time slice until working set fits in the memory.

#### Unix Process Creation

**Exec**

Replace current address space with specified program

#### Fork

Create a copy of current process

```c++
// Fork uses return code to differentiate
if (fork() == 0) {
	exec(); // child
} else {
	parent
}
```

##### Copy on Write

Copy the entire address space is expensive. Fork only copies the page table. When store a page with refcnt > 1:

1. Make a copy of the page with refcnt of one
2. Modify PTE of modifier to point to new page
3. Decrement refcnt of old page
