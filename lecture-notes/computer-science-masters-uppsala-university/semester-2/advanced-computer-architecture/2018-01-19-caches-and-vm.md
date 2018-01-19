# Lecture: Caches and VM

Readings: CLRS3 Chapter 3

### What Is A Cache?
- motivation behind caches is to avoid accessing data directly from the memory as it is slow
- a direct-mapped cache has one entry per page (does not provide good uniform distribution of data)
- a cache is a small memory that is in-between the CPU and memory
- before doing a lookup in memory, data is looked up in the cache
  - input to the cache is an address that would like to be looked at
  - cache will return data as well a signal 'hint' if the data can be trusted
  - if the signal 'hint' says the data cannot be trusted, then memory needs to be accessed to get the most up-to-date value and possibly update the cache

### Implementing Caches
- ``k = 2^10``, ``b`` = bit, ``B`` = byte
- how many index bits are need to represent a cache file that allows to save 1k entries?
  - ``10`` (i.e. 10 bits are needed to represent 1k unique values)
- what is a good indexing function?
  - uses address bits from the ``lsb`` (least significant bits) end which provide a better distribution of values (are more likely to change rapidly throughout the execution)
- hit: use the data provided from the cache
- not-hit: use data from memory and possibly store it in the cache
- given a 32 bit input, with its ``msb`` and ``lsb``, how to implement a direct-mapped cache:
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
  - ...
- ``CPI = 1 - mem_ratio +
        mem_ratio * (hit_ratio * hit_time) +
        mem_ratio * (1 - hit_ratio) * miss_time``
- how get more effective caches?
  - larger cache (more capacity)
  - cache block size (larger cache lines)
  - more placement choice (more associativity)
  - innovative caches (victim, skewed, ...)
  - cache hierarchies (L1, L2, L3, CMR)
  - latency-hiding (weaker memory models)
  - latency-avoiding (pre-fetching)
  - cache avoiding (cache bypass)
  - optimized application/compiler
  - ...
- why do you miss in a cache? (three C's)
  - compulsory miss (touching data for the first time)
  - capacity miss (the cache is too small)
  - conflict misses (non-ideal cache implementation)
