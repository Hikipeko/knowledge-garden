### L2 Threads

#### Process

The process is the OS abstraction of execution. #abstraction A process is a running program.

A process (identified by PID) contains all the state for a running program:

1. Execution: set of threads
2. Memory: an address space (the memory used by the program as it runs)

##### Address Space

**Cross-thread state**

1. Code and input data
2. Allocated memory
3. Open files, network, connections

**Per-thread state**

1. Execution stack (and a stack pointer)
2. The program counter (PC) indicating the next instruction
3. A set of general-purpose registers with current values

![[Pasted image 20221223142139.png]]

#### Thread

Unit of concurrency. The application gets the #abstraction that a thread is run on a CPU.

Why threads? They are fast, and sometimes easy to code!

##### Blocking I/Os

Other threads can progress when one thread is issues blocking I/Os. 

#### Concurrency 并发

A way to reason about you program as a collection of communicating threads.

Different programs can run on a single CPU. Functions execute one after the other.

#### Parallelism 并行

A model of program execution in which execution happens simultaneously. Different CPUs runs different threads (or programs).



### L3 Synchronization

Ordering within thread is sequential, but there're many ways of thread interleaving that may lead to different behaviors.

#### Atomic Operations

1. Indivisible, i.e., either all happens, or none
2. No events from other threads can occur in between (isolation?) #link 484

E.g.: memory load and store

#### Debugging Multi-Threaded Programs

Challenging due to non-deterministic interleaving.

##### Heisenbug

A bug that occurs non-deterministically.

#### Synchronization

Constrain the ways of interleaving between threads so that all possible interleaving produce a correct result.

#### Critical Section

Section of code that needs to be run atomically w.r.t shared resources.

#### Lock

A lock prevents another thread from entering the critical section.

Acquire a lock before critical section and release the lock after it.

##### Invariant

Stable state that is always true.

Hold a lock until invariants are true again.

##### Hand-Over-Hand Locking

Lock next node before releasing last node. #link file system

#### Conditional Variable

Problem: you want a thread to wait for another to do something, and you don't want busy waiting.

![[Pasted image 20221223151515.png]]

But where should we unlock?

Conditional variable provide ordering constraints, a "happens-before" relation.

**wait()**

A CV enable thread to sleep inside a critical section, by atomically (1-3)

1. Release the lock
2. Put thread onto waiting list
3. Go to sleep
4. After being woken, call lock()

Each CV is associated with a lock, having a list of waiting threads.

**signal()**

Wake up one thread waiting on this CV, does nothing if no waiting threads.

**broadcast()**

Wake up all waiting threads.

#### Monitor

Monitor = a lock + condition variables associated with that lock

### L4-5 Programing with Monitors

1. List the shared data
2. Assign locks to each group of shared data
3. Find the order constraints
4. Assign a condition variable for each situation
5. Use `while (!condition) { wait }
6. Call signal() or broadcast() when some waiting thread should wake up

```c++
mutex.lock()
while (!condition()) {
	cv.wait();
}

// do stuff

cv.signal();
mutex.unlock();
```

##### The 12 Commandments of Synchronization

1. Thou shalt name your synchronization variables properly.
2. Don't try to create your own way of synchronization.
3. Thou shalt use monitors and condition variables instead of semaphores whenever possible.
4. Thou shalt not mix semaphores and condition variables.
5. Thou shalt not busy-wait.
6. All shared state must be protected.
7. Thou shalt grab the monitor lock upon entry to, and release it upon exit from, a procedure.
8. Honor thy shared data with an invariant, which your code may assume holds when a lock is successfully acquired and your code must make true before the lock is released.
9. Thou shalt cover thy naked waits.
10. Thou shalt guard your wait predicates in a while loop. Thou shalt never guard a wait statement with an if statement.
11. Thou shalt not split predicates.
12. Thou shalt help make the world a better place for the creator’s mighty synchronization vision.

![[Pasted image 20221223160935.png]]

#### Reader-Writer Locks

Allow multiple readers, if no threads are writing data.

**Implementing reader-writer locks with monitors**

![[Pasted image 20221223161642.png]]



### L6 Semaphores

No need for writing semaphore code, but might appear in some old OS codes.

TBA



### L7 Thread Implementation

#### State of a Thread

![[Pasted image 20221223162142.png]]

##### Thread Control Block

When a thread is not running, we have to store its private state in TCB.

The TCBs are put in a ready queue.

#### Switching Threads

1. Current thread yields control to OS
2. OS choose next thread to run
3. OS saves state of current thread from CPU to TCB (save registers, PC, stack pointer, involves tricky assembly-language code)
4. OS loads context of next thread from TCB
5. OS runs next thread

##### Returning Control to OS

**Internal events**

1. Thread calls lock(), wait()
2. Thread traps to OS (e.g. for I/O)
3. Thread yield()

**External events**

Interruption, a hardware events that transfer control from CPU to OS's interrupt handler.

#### Create

When we create a thread, we should construct its TCB as if it had been running and got paused.

1. Allocate and initialize new TCB
2. Allocate and initialize new stack
3. Add TCB to ready queue

#### Join

Sometimes a parent wants to wait for child to finish, and we implement this as join.



### L8 Lock Implementation

##### Atomic Read-Modify-Write

**Test-And-Set

Atomically write 1 to a memory location and return old value

```c++
lock() {
	disable interrupts
	if (status == FREE) {
		statuc = BUSY
	} else {
		add thread to waiting queue
		switch to next ready thread
	}
	enable interrupts
}

unlock() {
	disable interrupts
	status = FREE
	if (waiting threads) {
		move a waiting thread to ready queue
		status = BUSY
	}
	enable interrupts
}
```



### L9 Switch Invariant

Disable interrupts before switch, enable interrupt after switch

![[Pasted image 20221223165953.png]]

On a multi-CPU system, the lock waiting queue is shared among CPUs. Thus disabling interrupts insufficient to ensure atomicity if there are multiple CPUs.

```c++
lock() {
	disable interrupts
	while (test_and_set(guard)) {} // atomic
	
	if (status == FREE) {
		statuc = BUSY
	} else {
		add thread to waiting queue
		switch to next ready thread
	}
	guard = 0
	enable interrupts
}

unlock() {
	disable interrupts
	while (test_and_set(guard)) {} // atomic
	
	status = FREE
	if (waiting threads) {
		move a waiting thread to ready queue
		status = BUSY
	}
	guard = 0
	enable interrupts
}
```

Use disable/enable interrupts and `test_and_set(guard)` to protect critical section inside synchronization code.

#### Deadlock #link 484

When an execution is over-constrained.

Resources: things needed by a thread that it waits for (locks, disk space, memory, CPU)

**Deadlock**

Cyclical waiting for resources which prevents progress, causing starvation.

##### Detect and Fix

Kill thread, grab resources, roll back execution

##### Prevent

**Conditions**

Limited resources, no preemption, hold and wait, cyclical chain

Three ways of prevention:

1. Grab all resources needed at beginning
2. If cannot get a resource, release all and start over
3. Impose global ordering of resources



### L10 Banker's Algorithm

Eliminate hold-and-wait

Declare resources at the beginning, but don't actually acquire them. Only give the resource if its' ==safe==. Safe means I can guarantee that all threads can finish.

A request of resource is allowed iff there is some sequential ordering that fulfills everyone's max resource, i.e. every thread can finish.

However, Banker's algorithm only applies to quantitative resources, not to locks.



### L15 Scheduling

When >1 threads are ready which one should we run?

##### Goals

1. **Minimize average response time**: elapsed time to do each job.
2. **Maximize throughput**: rate at which jobs complete in the system
3. **Fairness**: share CPU among threads in equitable manner.

##### Starvation

Some threads cannot make progress.

Readers can starve writers, tons of high priority threads can starve low priority ones.

##### FCFS

First come first served.

##### Round Robin

FIFO + preemptions, each thread take turns to execute.

Pros: improve average response time for short jobs

##### STCF

Shortest time to completion first, preempt current job if shorter job arrives.

Gives optimal response time, but is very unfair and need knowledge of future.

Estimate run time based on the time already running.

##### Priority

Assign external priority to each job.

Run high-priority jobs before low-priority ones.

Problem: priority inversion.

Solution of starvation: if a job hasn't run for time $t$, boost priority.




































