# Lecture: Basic File Structures, Hashing, and Modern Storage Architectures

Readings: Chapter 16, Fundamentals of database systems, Seventh Edition (R. Elmasri, S. Navathe).

### Placing File Records on Disk
- a database is stored as a collection of files. Each file is a sequence of records. A record is a sequence of fields
- recall a block is the unit of data transfer between disk and memory
- two ways to store records on blocks
  - spanned
    - larger than a single block
    - pointer at end of the first block points to block containing the remainder of record
  - unspanned
    - records not allowed to cross block boundaries
- a database system seeks to minimize the number of block transfers between the disk and memory
  - this can be achieved by keeping as many blocks as possible in main memory
- buffer: portion of main memory available to store copies of disk blocks
- buffer manager: subsystem responsible for allocating buffer space in main memory
- blocking factor: average number of records per block per file
  - ``bfr=floor(B/R)``, where ``B`` is the block size in bytes, and ``R`` is the record size in bytes
- operation on files
  - retrieval operations: no change to file data
  - update operations: file change by insertion, deletion, or modification
  - records selected based on selection condition

### Organization of Record in Files
- heap: a record can be placed anywhere in the file where there's space
  - records are placed in file in order of insertion
  - inserting a new record is very efficient
  - searching for a record is linear
  - average blocks to access a specific record: ``b/2`` (``b`` is # of blocks)
- sequential: store records in sequential order, based on the value of the search key of each record
  - records sorted by an ordering field. This is suitable for applications that require sequential processing of the entire file
  - reading is records is very efficient (can use binary search)
  - finding next record is also efficient
  - deletion: use pointer chains
  - insertion: locate position where the record is to be inserted (need to check if there's free space or not). In either case, pointer chain must be updated
  - need to organize file from time to time to restore sequential order
  - average blocks to access a specific record: ``log_2(b)`` (``b`` is # of blocks)
- hashing: a hash function computed on some attribute of each record. The result specifies in which block of the file the record should be placed
  - average blocks to access a specific record: 1
  - collision: hash field value for inserted record hashes to address already containing a different record
  - collision resolution: process of finding another position for a record when a collision occurs
    - open addressing: check subsequent positions in order until an unused (empty) position is found
    - multiple hashing: apply a second hash function if the first results in a collision
    - chaining: various overflow locations are kept
  - hashing techniques with dynamic file expansion
    - extensible hashing: file performance does not degrade as file grows
    - dynamic hashing: maintains tree-structured directory
    - linear hashing: allows hash file to expand and shrink buckets without needing a directory
