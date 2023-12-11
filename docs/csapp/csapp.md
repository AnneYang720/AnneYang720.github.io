# CSAPP


## Bits, Bytes and Ints


## Floating Point


## Machine Program
### Basics

```
movq Source,Dest
```
Operand Types: Immediate, Register, Memory

Memory Addressing Modes:
```
D(Rb,Ri,S)  Mem[Reg[Rb]+S*Reg[Ri]+D]
D:Constant“displacement” 1, 2 or 4 bytes	  
Rb: Base register: Any of 16 integer registers	  
Ri: Index register: Any, except for %rsp
S: Scale: 1, 2, 4 or 8
```

Two	Operand	Instructions:
```
leaq Src,Dst
addq, subq, imulq, salq, sarq, shrq, xorq, andq, orq
```

One	Operand Instruc�ons:
```
incq Dest   Dest = Dest + 1
decq Dest   Dest = Dest ­‐ 1
negq Dest   Dest = -­Dest
notq Dest   Dest = ~Dest
```

### Control
Condition codes
```
cmpq Src2,Src1
testq Src2,Src1
```
Sigle bit registers:
- CF Carry Flag (for unsigned) 
- SF Sign Flag (for signed)	  
- ZF Zero Flag 
- OF Overflow Flag (for signed)
Conditional branches
```
jX
```
Conditional move
```
cmovq
```

### Procedures
stack structure
- $%rsp$ stack top
- push Src
- pop Dest

### Data
- Arrays
- Structures
- Float Points


## The Memory Hierarchy
Traditional Bus Structure Connecting CPU and Memory
![](../images/csapp/bus_structure.png)

Memory Layout
!!!!!!!!where the hell is this pic from???????

Memory Hierarchy
- L0 Regs
- L1 L1 cache(SRAM)
- L2 L2 cache(SRAM)
- L3 L3 cache(SRAM)
- L4 Main memory(DRAM)
- L5 Local secondary storage(local disk)
- L6 Remote secondary storage(eg. web servers)

## Cache Memories
![](../images/csapp/cache_read.png)


## ECF - Excetion Control Flow


### Excep&onal Control Flow	
Low level mechanisms
- Exceptions

Higher level mechanisms
- Process context switch
- Signals	
- Nonlocal jumps

### Exceptions
Asynchronous Excep&ons
- Interrupts (eg. Timer interrupt, I/O interrupt from external device)

Synchronous	Exceptions
- Traps	
    - Intentional	
    - Examples: system calls, breakpoint traps, special instructions
    - Returns control to “next” instruction	
- Faults
    - Unintentional but possibly recoverable
    - Examples: page faults(recoverable), protection faults(unrecoverable),	floating point exceptions	
    - Either re-executes faulting (“current”) instruction or aborts
- Aborts
    - Unintentional and unrecoverable
    - Examples: illegal instruction, parity error, machine check	
    - Aborts current program	

### Processes
Key abstractions:
1. Logical control flow
    - Each program seems to have exclusive use of the CPU
    - Provided by kernel mechanism called context switching
2. Private address space
    - Each program seems to	have exclusive use of main memory
    - Provided by kernel mechanism called virtual memory

Context Switching: Processes are managed by a shared chunk of memory-resident OS code called the kernel

### Process Control
Process States:
- Running
- Stopped
- Terminated

Terminating Processes:
```
void exit(int status)
```

Creating Processes:
```
int fork(void)
```
Returns 0 to the child process, child’s PID to parent process

- *wait*: Synchronizing with Children
- *waitpid*: Waiting for a Specific Process
- *execve*: Loading and Running Programs

## System Level I/O
File Types:
- Regular file
- Directory
- Socket
- Others
    - Named pipes(FIFOs)
    - Symbolic Links
    - Character and block devices

Opening Files:
- Opening a file informs the kernel that you are getting ready to access that file
- Returns a small identifying integer **file descriptor**
- Each process created by a Linux shell begins life with three open files associated with a termial: stgin, stdout and stderr


## Virtual Memory
From virtual memory address to physical address:
![](../images/csapp/vm_symbol.png)
![](../images/csapp/vm_e2e.png)



## Storage Allocation

### Basic Concepts
![](../images/csapp/storage_basic_policy.png)

### Free Lists
![](../images/csapp/storage_all_methods.png)

Implicit free lists
![](../images/csapp/storage_implicit_coalesce.png)
![](../images/csapp/storage_implicit_summary.png)

Explicit free lists: among the free blocks using pointers

Segregated free lists : Each size class of blocks has its own free list

### Garbage collection
Memory as a graph & Mark and Sweep Collecting

### Memory-related perils and piBalls
- Dereferencing bad pointers
- Reading uninitialized memory
- Overwriting memory
- Referencing nonexistent variables
- Freeing blocks multiple times	
- Referencing freed blocks
- Failing to free blocks


## Network Programming
Sockets
![](../images/csapp/network_socket.png)

Sockets Interface:
![](../images/csapp/network_socket_interface.png)

```
int getaddrinfo(const char *host,   /* Hostname or address */
    const char *service,            /* Port or service name */
    const struct addrinfo *hints,   /* Input parameters */
    struct addrinfo **result)       /* Output linked list */ 

int socket(int domain, int type, int protocol) 
int bind(int sockfd, SA *addr, socklen_t addrlen);
int listen(int sockfd, int backlog)
int accept(int listenfd, SA *addr, int *addrlen)
int connect(int clientfd, SA *addr, socklen_t addrlen)
```
![](../images/csapp/network_descriptor.png)


## Concurrent Programming
### Approaches for Writing Concurrent Servers 
Allow server to handle multiple clients concurrently 
1. Process-based 
    - Kernel automatically interleaves multiple logical flows
    - Each flow has its own private address space 
2. Event-based 
    - Programmer manually interleaves multiple logical flows
    - All flows share the same address space
    - Uses technique called I/O multiplexing 
3. Thread-based
    - Kernel automatically interleaves multiple logical flows
    - Each flow shares the same address space
    - Hybrid of of process-based and event-based

![](../images/csapp/concurrent_process.png)
![](../images/csapp/concurrent_threads.png)

### Approaches comparison
Summary: Approaches to Concurrency 
1. Process-based
    - Hard to share resources: Easy to avoid unintended sharing
    - High overhead in adding/removing clients
2. Event-based
    - Tedious and low level
    - Total control over scheduling
    - Very low overhead
    - Cannot create as fine grained a level of concurrency
    - Does not make use of multi-core
3. Thread-based
    - Easy to share resources: Perhaps too easy
    - Medium overhead
    - Not much control over scheduling policies
    - Difficult to debug: Event orderings not repeatable 


## Synchronization
### Mapping Variable Instances to Memory 
1. Global variables
    - Def: Variable declared outside of a function
    - <span style="color: red;">Virtual memory contains exactly one instance of any global variable</span>
2. Local variables
    - Def: Variable declared inside function without static attribute
    - <span style="color: red;">Each thread stack contains one instance of each local variable</span>
3. Local static variables
    - Def: Variable declared inside function with the static attribute
    - <span style="color: red;">Virtual memory contains exactly one instance of any local static variable</span>

### Semaphores
1. <span style="color: red;">Semaphore</span>: non-negative global integer synchronization variable. Manipulated by P and V operations. <span style="color: red;">Semaphore invariant: (s >= 0)</span>
2. P(s)
    - If s is nonzero, then decrement s by 1 and return immediately.
        - Test and decrement operations occur atomically (indivisibly)
    - Ifs is zero, then suspend thread until s becomes nonzero and the thread is restarted by a V operation.
    - After restarting, the P operation decrements s and returns control to the caller.
3. V(s):
    - Increment s by 1.
        - Increment operation occurs atomically
    - If there are any threads blocked in a P operation waiting for s to become non-zero, then restart exactly one of those threads, which then completes its P operation by decrementing s

#### Using Semaphores for Mutual Exclusion
Terminology:
1. <span style="color: red;">Binary semaphore</span>: semaphore whose value is always 0 or 1
2. <span style="color: red;">Mutex</span>: binary semaphore used for mutual exclusion
    - P operation: <span style="color: red;">"locking"</span> the mutex
    - V operation: <span style="color: red;">"unlocking"</span> or <span style="color: red;">"releasing"</span> the mutex
    - <span style="color: red;">"Holding"</span> a mutex: locked and not yet unlocked
3. <span style="color: red;">Counting semaphore</span>: used as a counter for set of available resources

```C
mutex = 1

P(mutex)
cnt++
V(mutex)
```

### Crucial concept: Thread Safety
- Functions called from a thread must be thread-safe
- Def: A function is thread-safe if it will always produce correct results when called repeatedly from multiple concurrent threads
- Classes of thread-unsafe functions:
    - Class 1: Functions that do not protect shared variables
    - Class 2: Functions that keep state across multiple invocations
    - Class 3: Functions that return a pointer to a static variable
    - Class 4: Functions that call thread-unsafe functions O


## Thread-Level Parallelism
### Parallel Computing Hardware
- Multicore
    - Multiple separate processors on single chip
- Hyperthreading
    - Efficient execution of multiple threads on single core
    ![](../images/csapp/parallelism_hyperthreading.png)

### Thread-Level Parallelism
Splitting program into independent tasks
    Example 1: Parallel summation
Divide-and conquer parallelism
    Example 2: Parallel quicksort