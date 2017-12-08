# Lecture: Transactions, Concurrency Control, Recovery

Readings:
  - Chapter 20, Fundamentals of database systems, Seventh Edition (R. Elmasri, S. Navathe).
  - Chapter 15, Database System Concepts, Sixth Edition (Avi Silberschatz, Henry F. Korth, S. Sudarshan).

### Transactions
- single-user DBMS: at most one user at a time can use the system (home computer)
- multiuser DBMS: many users can access the system (database) concurrently (airline reservations system)
- multiprogramming
  - allows operating system to execute multiple processes concurrently
  - executes commands from one process, then suspends that process and executes commands from another process, etc
- transaction
  - is a unit of program execution that accesses and possibly updates various data items
  - a transaction must apply to a consistent database
  - during transaction execution the database may be inconsistent
  - when the transaction is committed, the database must be consistent
- transaction processing systems
  - systems with large databases and hundreds of concurrent users
  - require high availability and fast response time
- transaction state
  - active, the initial state; the transaction stays in this state while it is executing
  - partially committed, after the final statement has been executed
  - failed, after the discovery that normal execution can no longer proceed
  - aborted, after the transaction has been rolled back and the database restored to its state prior to the start of the transaction. Two options after it has been aborted:
    - restart the transaction (only if no internal logical error)
    - kill the transaction
  - committed, after successful completion
- read and write operations
  - ``read_item(X)``
    - reads a database item named ``X`` into a program variable named ``X``
    - process includes finding the address of the disk block, and copying to and from a memory buffer
  - ``write_item(X)``
    - writes the value of program variable ``X`` into the database item named ``X``
    - process includes finding the address of the disk block, copying to and from a memory buffer, and storing the updated disk block back to disk
- read set of a transaction: set of all items read
- write set of a transaction: set of all items written

### Concurrency Control
- transactions submitted by various users may execute concurrently
  - access and update the same database items
  - some form of concurrency control is needed
- issues that may arise
  - the lost update problem
    - occurs when two transactions that access the same database items have operations interleaved
    - results in incorrect value of some database items
  - the temporary update problem
    - occurs when one transaction updates a database item and then the transaction fails for some reason
    - meanwhile, the updated item is accessed (read) by another transaction before it is changed back (or rolled back) to its original value
  - the incorrect summary problem
    - if one transaction is calculating an aggregate summary function on a number of database items while other transactions are updating some of these items, the aggregate function may calculate some values before they are updated and others after they are updated

### Recovery
- committed transaction: effect recorded permanently in the database
- aborted transaction: does not affect the database
- types of transaction failures
  - computer failure (system crash)
  - transaction or system error
  - local errors or exception conditions detected by the transaction
  - concurrency control enforcement
  - disk failure
  - physical problems or catastrophes
- system must keep sufficient information to recover quickly from the failure
  - disk failure or other catastrophes have long recovery times
- system must keep track of when each transaction starts, terminates, commits, and/or aborts
  - ``BEGIN_TRANSACTION``
  - ``READ`` or ``WRITE``
  - ``END_TRANSACTION``
  - ``COMMIT_TRANSACTION``
  - ``ROLLBACK`` (or ``ABORT``)
- the system log
  - system log keeps track of transaction operations
  - sequential, append-only file
  - not affected by failure (except disk or catastrophic failure)
  - log buffer: main memory buffer
  - log file is backed up periodically
  - undo and redo operations based on log possible
- commit point of a transaction occurs when all operations that access the database have completed successfully
- transaction writes a commit record into the log, if system failure occurs, can search for transactions with recorded ``start_transaction`` but no commit record

### Desirable Properties of Transactions
- ACID properties
  - atomicity
    - transaction performed in its entirety or not at all
  - consistency preservation
    - takes database from one consistent state to another (if we transfer money from ``A`` to ``B``, the consistency requirement is that ``A+B`` remains the same)
  - isolation
    - not interfered with by other transactions
  - durability or permanency
    - changes must persist in the database

### Characterizing Schedules Based on Recoverability
- schedule or history
  - sequences that indicate the chronological order in which instructions of concurrent transactions are executed
  - operations from different transactions can be interleaved in the schedule
- total ordering of operations in a schedule
  - for any two operations in the schedule, one must occur before the other

### Characterizing Schedules Based on Serializabilty
- serializable schedules: always considered to be correct when concurrent transactions are executing
  - in a serial schedule instructions of transactions are not interleaved; one transaction is executed completely before the other
- places simultaneous transactions in series
  - transaction ``T1`` before ``T2``, or vice versa
- problem with serial schedules
  - limit concurrency by prohibiting interleaving of operations
  - unacceptable in practice
  - solution: determine which schedules are equivalent to a serial schedule and allow those to occur
- serializable schedule of ``n`` transactions
  - equivalent to some serial schedule of same ``n`` transactions
- conflict equivalence: relative order of any two conflicting operations is the same in both schedules
- serializable schedules
  - schedule ``S`` is serializable if it is conflict equivalent to some serial schedule ``S'``
- precedence graph: a directed graph where the vertices are the transactions (names)
  - A schedule is conflict serializable if and only if its precedence graph is acyclic.
- being serializable is different from being serial
- serializable schedule gives benefit of concurrent execution (without giving up any correctness)
- difficult to test for serializabilty in practice
  - factors such as system load, time of transaction submission, and process priority affect ordering of operations
- DBMS enforces protocols
  - set of rules to ensure serializabilty
