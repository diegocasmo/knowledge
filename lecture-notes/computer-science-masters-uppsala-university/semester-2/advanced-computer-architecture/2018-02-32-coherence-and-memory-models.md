# Lecture: Coherence and Mem Models

### Multiprocessors and Coherent Memory
- models of parallelism
  - processes
    - a parallel execution, where each process has its own process state
  - threads
    - parallel threads of control inside a process
    - there are some thread-shared state (i.e. VA -> PA mapping)
- common property: each thread has its own PC (program counter)
- automatic data replication: automatically replicate data in many caches if many threads are interested in the same piece of data
  - this creates issue while writing: need to implement a coherent write
- write invalidate: detects when a thread writes data, and invalidates any other cached value of it in other threads
- cache-to-cache transfer: when a cache asks for data, instead of going to memory, let the cache with the value transfer it
- write back: upon replacement, need to make sure the copy in memory is updated with the latest value (only if there was a write)
- coherence defines a property of a specific piece of data
  - ~= there can be many copies of data, but only 1 value at any given point in time (this definition is a little bit too strict)
  - there is a single global order of value changes to each datum
- often coherence is kept at the cache line level (the unit of transfer)

### Snooping Coherence
- two coherence options
  - snoop-based ('broadcast protocol')
  - directory-based ('point to point protocol')
- bus snoop state machines
  - MOSI
    - m = modified: my dirty copy is the only cached copy (value stored in the cache is different from the one stored in memory)
    - s = shared: I have a clean copy, others may also have a copy
    - o = owner: I have a dirty copy, others may also have a copy
    - i = invalid: I have no valid copy in my cache (including cache miss)
  - bus transactions from others
    - BUSrts: ReadToShare (reading the data)
    - BUSrtw: ReadToWrite (reading the data with the intention to modify it right away)
    - BUSinv: Invalidating other caches copies
    - BUSwb: write data back to memory
- CPU access state machine
  - it's a MOSI state machine, but the transactions are different
  - CPUread: caused by a load instruction
  - CPUwrite: caused by a store or atomic instruction
  - CPUrepl: caused by a replacement of this cache line

### Sequential consistency
- coherence miss: the cache would have had the data unless it had been unvalidated by someone else
- upgrade misses (only for writes): the cache would have had a writable copy, but answered a read and 'downgraded' itself to read-only state
- false sharing: coherence/downgrade is caused by a shared cache line and not by shared data
- where memory models matter?
  - flag synchronization
  - causality
  - Dekker's algorithm (mutual exclusion)
- coherence defines per-datum value change order
- memory model defines the value change order for all the data

### Other Coherence Alternatives
- common cache states
  - e = exclusive: my clean copy is the only cache copy
- coherence alternatives
  - MSI
    - writeback to memory on a cache2cache
    - there's no 'O' state
  - MOESI
    - the first reader will go to 'E' a can later become a writer cheaply
  - updated-bases MOSI
    - instead of sending an invalidate signal, update the cache lines that need to be invalidated
    - can avoid coherence misses
    - may consume a large amount of snoop bandwidth
