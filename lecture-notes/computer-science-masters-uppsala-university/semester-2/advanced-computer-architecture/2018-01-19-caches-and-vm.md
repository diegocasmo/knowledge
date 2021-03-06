# Lecture: Caches and VM

### What Is A Cache?
- motivation behind caches is to avoid accessing data directly from the memory as it is slow
- a cache is a small memory that is in-between the CPU and memory
- before doing a lookup in memory, data is looked up in the cache
  - input to the cache is an address that would like to be looked at
  - cache will return data as well a signal 'hint' if the data can be trusted
  - if the signal 'hint' says the data cannot be trusted, then memory needs to be accessed to get the most up-to-date value and possibly update the cache
- in a direct mapped cache, a block has only one place it can appear in the cache (does not provide good uniform distribution of data)
- in a fully associative cache, a block can be placed anywhere (a fully associative cache has no index field)
- in a set associative cache, a block can be placed in a restricted set of places in the cache
  - if there are ``n`` blocks in a set, the cache placement is called ``n``-way set associative
  - direct mapped can be thought as a ``1``-way set associative
  - a fully associative cache with ``m`` blocks could be called ``m``-way set associative
- when the requested data is found in the cache, it is called a cache hit
- if the requested data is not found on the cache, it is called a cache miss
- the assigned cost per miss is called the miss penalty
- the miss rate is the fraction of cache accesses that result in a miss (i.e., number of accesses that miss divided by number of accesses)
- latency determines the time to retrieve the first word of a block, and bandwidth determines the time to retrieve the rest of this block

### Implementing Caches
- ``k = 2^10``, ``b`` = bit, ``B`` = byte
- how many index bits are need to represent a cache file that allows to save 1k entries?
  - ``10`` (i.e. 10 bits are needed to represent 1k unique values)
- what is a good indexing function?
  - uses address bits from the ``lsb`` (least significant bits) end which provide a better distribution of values (are more likely to change rapidly throughout the execution)
- hit: use the data provided from the cache
- not-hit: use data from memory and possibly store it in the cache
- given a 32 bit input, with its ``msb`` and ``lsb``, how to implement a direct-mapped cache that stores 1K entries:
  - toss away the last 2 ``lsb``
  - use next 10 ``lsb`` for the indexing function
  - read the value stored in cache identified by those 10 ``lsb``
  - compare 20 bits in the address tag with the part of the identifier that was not use for the indexing function
  - a valid bit that says if the space in the cache has been initialized or not
  - if the comparison is true and the valid bit is set, then it produces a single hit signal and the data stored in that location is sent back to the processor
  - memory overhead (how many useful bits v.s. how much extra-overhead): ``22/32 = 66%``
  - latency: SRAM + CMP + AND (latency it takes to make a SRAM access + time it takes to make comparison + time it takes to go through the AND gate)

### Judging Caches
- architecture is usually measured in terms of
  - CPI = cycles per instruction
  - IPC = instructions per cycle (= ``1/CPI ``)
- cache performance parameters
  - cache 'hit ratio' %
  - cache 'miss ratio' % (1 - cache 'hit ratio')
  - hit time (CPU cycles)
  - miss time (CPU cycles)
  - memory ratio % (fraction of instructions to access memory)
- cache optimizations categories
  - reducing the miss rate: larger block size, larger cache size, and higher associativity
  - reducing the miss penalty: multilevel caches and giving reads priority over writes
  - reducing the time to hit in the cache: avoid address translation when indexing the cache
- why do you miss in a cache? (three C's)
  - compulsory miss (touching data for the first time)
  - capacity miss (the cache is too small)
  - conflict misses (non-ideal cache implementation)
- compulsory misses are independent of cache size, while capacity misses decrease as capacity increases, and conflict misses decrease as associativity increases

### Cache Size and Lines
- cache size refers to the size of the data it can the cache can store
- a larger cache size can help avoiding capacity misses
- cache line is the smallest unit of memory than can be transferred between the main memory and the cache
- temporal locality refers to the likelihood of accessing the same data again soon
- spatial locality refers to the likelihood of accessing nearby data soon
- spatial locality is explored by large cache lines
- using a larger cache line can help avoiding compulsory misses
- small cache: smaller cache line is better
- large cache: larger cache line is better

### Associativity
- two-way associative cache
  - cache is made up of sets that can fit two blocks each
  - the index is used to find the set, and the address tag helps find the block within the set
- pros
  - avoid conflict misses
- cons
  - slower access time
  - more complex implementation comparators, muxes, ...
  - higher dynamic power consumption
- fully associative cache
  - no index is needed since a cache block can go anywhere in the cache
  - every tag must be compared when finding a block in the cache, but block placement is very flexible
  - very expensive
  - only used for small caches
- associative caches remove conflict misses

### Optimizing Caches
- how to choose which cache to replace?
  - least-recently used (LRU) algorithm
    - throw out the longest unused cache line
    - requires to keep more information to maintain a history about the sets
    - implementing a true LRU replacement for highly associative caches is expensive
  - not most recently used
    - remember who used it last
  - pseudo-LRU
    - based on course time stamps
  - random replacement
- handling dirty cache lines
  - write-through: information is written to both the block in the cache and the block in the lower-level memory
    - always write through to memory
    - data will never by dirty
    - no write-backs (read misses never result in writes to the lower level)
  - write-back caches: information is written only to the block in the cache. The modified cache block is written to main memory only when it is replaced
    - a 'dirty bit' per cache line indicates an altered cache line
    - write data back to memory at replacement, called write-back
- power consumption
  - static power or leakage power
    - current leaks through transistors
    - proportional to the number of transistors used
    - proportional to cache capacity
  - dynamic power
    - extra energy needed to read SRAMs bits
    - proportional to the number of SRAM bits read
  - which power dominates?
    - associative and fast first-level cache (L1) cache: dynamic
    - large slower cache last-level cache (LLC): static
- L1 caches are implemented as fast-cache and sometimes have smaller cache lines than L2
- L3 caches consumption is dominated by static power and are often shared by many CPUs (cores)
- the hardware pre-fetcher tries to anticipate what data is going to be accessed next
  - improves memory-level parallelism MLP
- sequential pre-fetching
  - sequential streams to a  page
  - some number of pre-fetch streams supported
  - often only for L2 and L3
- PC-based pre-fetching
  - detects strides from te same PC
  - often for L1 caches
- adjacency pre-fetching
  - on a miss, also bring in the neighboring cache line
  - often only for L2 and L3
- cache line: data chunk move to/from a cache
- cache set: fraction of the cache identified by the index
- associativity: number of alternative storage places for a cache line
- replacement policy: picking the victim to throw out from a set (LRU/Random/Nehalem)
- temporal locality: likelihood to access the same data again soon
- spatial locality: likelihood to access nearby data again soon
- how much to replicate data among the different cache levels?
  - inclusive
    - install a copy of the data in L1 and keep the data in L2
    - when the L2 data is replaced, force eviction from L1
    - if the data is not in L2, it is not in L1 either
  - non-inclusive
    - install a copy of the data in L1 and keep the data in L2
    - when the L2 data is replaced, L1 data survives
    - if the data is not in L2, it may be in L1
  - exclusive
    - move the data from L2 to L1
    - at L1 replacement, move the data back to L2
    - data is either in L1 or L2, but never in both
- a victim cache can help reduce the number of conflict misses
- sub-blocking lowers the memory overhead and avoids problems with false sharing
- sub-blocking will not explore as much spatial locality, and still poor utilization of SRAM (fewer sparse 'things' allocated)
- skewed-associative cache reads fewer bits from the SRAM on a cache lookup (dynamic energy)

### Memory
- DRAM (dynamic random access memory)
  - 1 transistor/bit
  - error prone and slow
  - refresh and pre-charge overhead and complexity
  - may require error detection and correction
  - organization in rows and columns
    - need to supply the RAS (row address strobe) and CAS (column address strobe)
  - periodic 'refresh' of memory cells
  - reading out the first piece of data is slow (other data from the same line is 'streamed')
  - spatial locality in accesses make DRAM perform faster on average
- SRAM (static random access memory)
  - about 4-6 transistors/bit (faster to access)
- big v.s. little endian
  - big endian: a word is saved starting from the most significant bit
  - little endian: a word is saved starting from the least significant bit

### Virtual Memory System
- the virtual memory divides the physical memory into blocks and allocates them to different processes
  - virtual memory means some objects may reside on disk
  - the processor produces virtual addresses that are translated by a combination of hardware and software to physical addresses, which access main memory
- an address space is broken into fixed-size blocks, called pages
- an address fault is analogous to a cache miss
- an address space is divided into segments
  - text segment stores instructions
  - data segment stores static data
  - heap segment is data dynamically allocated by the program
- there's no index function, so it can be thought as a fully associative cache
- multi-level translation table is used to reduce the storage requirement for the page table
- a page table entry (PTE) contains protection attribute
- translation look-aside buffer (TLB)
  - caches recent address translations
  - contains protection attribute bits
- aliasing problem with virtual caches
  - the same physical page may be accessed using different virtual addresses (aliasing)
  - different physical addresses may be referred to by the same virtual address (homonymes)
- virtually indexes physically tagged (VIPT)
  - TLB is placed in parallel with the cache
  - need to guarantee that all aliases have the same index
- VM: replacement strategy
  - FIFO: fist-in first-out
  - approximation to LRU
- VM: write strategy
  - write back
  - write through is impossible to use
    - many small writes
    - not enough bandwidth to disk
