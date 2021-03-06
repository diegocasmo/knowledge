# Lecture: Disk Storage

Readings: Chapter 16, Fundamentals of database systems, Seventh Edition (R. Elmasri, S. Navathe).

### Storage Hierarchy
- Data must be stored physically on some computer storage medium. Computer storage media forms a 'storage hierarchy'
- Primary storage (RAM, Cache)
  - includes storage media that can be operated directly by the computer's CPU
  - usually provides fast access, but it's limited in storage
  - the contents of main memory are lost in case of power failure or system crash
- Secondary storage (Magnetic disks, flash memory, solid-state drives)
  - non-volatile memory that is not directly accessible by the CPU
- Tertiary storage (CD ROM, DVD)
  - removable media which is typically used in today's systems as off-line storage for archiving databases

### Memory Hierarchies and Storage Devices
- Data resides and is transported throughout a hierarchy of storage media
- Cache memory
  - RAM
    - fastest and most costly form of storage; volatile; managed by the computer system hardware
  - Main memory (DRAM)
    - fast access
    - generally too small (or too expensive) to store the entire database
    - volatile: contents of main memory are usually lost if a power failure or system crash occurs
- Mass storage
  - Magnetic disks (CD-ROM, DVD, tape drives)
    - primary medium for the long-term storage of data; typically stores entire database
    - data must be moved from disk to main memory for access, and written back for storage
    - survives power failures and system crashes
- Flash memory
  - nonvolatile: does not lose stored data when the device is powered down

### Storage Organization of Databases
- Persistent data
  - most databases
  - persists over long periods of time
- Transient data
  - exists only during program execution
- File organization
  - determines how records are physically placed on the disk
  - determines how records are accessed

### Secondary Storage Devices
- Hardware description of disk devices
  - Magnetic disks are used to store large amounts of data. The device that holds the disk is also referred as hard disk drive (HDD)
  - Bit is the most basic unit of data on a disk. Bits are grouped into bytes (usually 8 bits)
  - Disk capacity measures the storage size of a disk (number of bytes it can store)
  - Disks might be single sided (stores information on one of its surface), or double sided (stores information in both surfaces)
  - To increase the capacity, disks are assembled into disk pack (a layered grouping of hard disks)
  - Circles in the HDD are called tracks. These tracks can be divided into blocks or sectors (smaller sections of the track)
  - During formatting, disk tracks are set to equal-sized disk blocks. Blocks are separated by inter-block gaps
  - Transfer of data between main memory and disk takes place in units of disk blocks (the hardware address of a block is supplied to the disk I/O hardware)
  - The buffer is used in read and write operations
  - Read/write head is the hardware mechanism that reads or writes a block in the disk
- Techniques for efficient data access
  - Data buffering: buffering of data is done in memory such that new data can be held in a buffer while old data is processed
  - Proper organization of data on disk: keep related data on contiguous blocks to avoid unnecessary movement of the read/write arm
  - Reading data ahead of request: whenever a block is requested, blocks from the rest of the track are also read
  - Proper scheduling of I/O requests: total access time can be minimized by scheduling the requests such that the  arm only moves in one direction
  - Use of log disks to temporarily hold writes
  - Use of SSDs or flash memory for recovery purposes
