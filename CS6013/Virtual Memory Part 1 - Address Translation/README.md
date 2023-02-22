# Virtualization

> This lecture talking about: **Virtual memory**.

* Virtual CPU: Illusion of private CPU registers.
* Virtual RAM: Illusion of private addresses and memory.

**`Uni-programming`** : One process runs at a time.
Disadvantages:

* Only one process runs at a time
* Process can destroy OS.

## Multiprogramming Goals

* Transparency
  * Processes are not aware that memory is shared.
  * Works regardless of number and/or location of processes.
* Protection
  * Secrecy - Cannot read data of OS or other processes.
  * Integrity - Cannot change data of OS or other processes.
* Efficiency
  * Do not waste memory resources (minimize fragmentation).
* Sharing
  * Cooperating processes can share portions of address space.

## Address Space

Process set of addresses (map to bytes).
Address space has static and dynamic components.

* Static : Code and some global variables.
* Dynamic: Stack and Heap.

## Dynamic Memory

Why do processes need dynamic memory allocation?

* Do not know amount of memory needed at compile time.
* Must be pessimistic when allocating memory statically.
* Allocate for worst case; storage used inefficiently.

Complex data structures: lists and trees

``` c++
    struct my_t *p = (struct my_t *)malloc(sizeof(struct my_t));
```

Two types of dynamic allocation:

//TODO: Add link point to stack section.

1. Stack
2. Heap

## Stack Organization

Memory freed in opposite order from alloc

* alloc(A); alloc(B); alloc(C);
* free(C);
* alloc(D);
* free(D); free(B); free(A);

> Last in First out.
> free C if enter D then free D next and so on.

Where are stack used?
OS uses stack for procedure **call frames**. (local variables and parameters).

``` c++
    void main() {
        int A = 0;
        foo(A);
        printf("A: %d\n", A);
    }

    void foo(int Z) {
        int A = 2;
        Z = 5;
        printf("A: %d Z: %d\n", A, Z);
    }
```

## Heap Organization

Memory region where alloc/free are explicit

* Heap consists of allocated areas and free areas (holes).

Advantage:

* Alloc lifetime is independent of call/ret
* Works for all data structures

Disadvantages:

* Allocation can be slow.
* End up with small chunks of free space - fragmentation.
* Leaks (forgotten free), double free.

OS role in managing heap:
OS gives big chunks free memory to process (sbrk/mmap); library manages individual allocations (malloc/free).

![Heap](/CS6013/Virtual%20Memory%20Part%201%20-%20Address%20Translation/Images/Heap.png)

## Virtualizing Memory

How to run multiple processes when addresses are "hardcoded" into process binaries.

Possible solutions:

* Time sharing
  * Time share CPU: Context Switching(save all register into RAM))
  * Time share RAM: Backup to disk
    **Time sharing is not good.**
    Problem: Ridiculously poor performance due to slow speeds of memory and data transfer.

    **Alternative:** space sharing
    At same time, space of memory is divided across processes.

    Remainder of solutions all use space sharing.

* Static relocation
  Idea: OS rewrites programs before loading in memory.
  Make different process use different addresses/pointers.
  Change jumps, loads of static data.
  **Drawback**: If a process grows over time, it rewrite every single address on the runtime(or risk violation of other processes) which could be very slow. Cannot move address space after it has been placed.
* Dynamic relocation.(Base & Bound, Segmentation, Paging)
  Goal: Relocation ar **runtime** and protect processes from one another.
  Requires hardware support: ***<ins>`Memory Management Unit (MMU)`</ins>***
  MMU dynamically changes every process address at every memory reference.
  * Process generates **logical** or virtual addresses in their address space.
  * Memory hardware uses **physical** or **real** addresses.
  ![Dynamic relocation](/CS6013/Virtual%20Memory%20Part%201%20-%20Address%20Translation/Images/Dynamic%20relocation.png)
