# Transactions: 
Transactions are the group of queries that are bundled together, even if you run single query, 
it will be in the transaction. **They are one unit of work**. Transactions have following livecycle: 
- START: Transaction must be implicitly started
- COMMIT or ROLLBACK: all queries are executed in the transaction, in any point of time they
may be rollback due to some procedure/user decision/error or if not all transactions are 
committed: preserved to the database, if they are rollback all operations are just reversed
or if there was middle step of preserving changes to local memory, this memory is flushed. 
Different SQL DB implementations optimize for diffrent things, and may do things differently.
At the start of the transaction, we receive snapshot of transaction, which state will not be 
changed until end of it, thats why only read transactions are also common. Transactions may
fail during commit - they are really hard to rollback then. The faster commits are the smaller
chance that they will crash/rollback. 

----
# Atomicity: 
**All the queries in the transaction must succeed**. If any query fails for any reason, entire 
transaction will be rollback. Before the commit: all the successful queries should rollback

----
# Isolation: 
Isolation is a property of the databases that tells about which updates to the databases (other transactions) 
are seen during the transaction. There are the levels of isolation, each one stricter than the previous:
## Isolation Levels: 
### Read uncommitted: 
Transaction sees all modifications that are made to the databases: even those not yet committed in different transactions.
It is fast but does not give any protection from read phenomena
### Read committed: 
Transaction sees only modifications that were committed, if some transactions commit during the lasting of this transaction, it 
will see the changes to the database introduced by those transactions. Default isolation level. Helps with dirty reads but nothing except it.
### Repeatable read: 
Rows remain unchanged during the transaction — this is usually achieved by locking rows affected by said transaction: until it ends, 
no changes may be made by other transactions to these rows. Don't protect from Phantom reads.
### Snapshot:
Each query in transaction sees changes that have been commited up to the start of the transaction, effectively snapshot of the database
is made at the start of the transaction (this level is implemented, for example, by PostgresSql). Secure but heavy on resources.
### Serializable transactions:
Transactions are made synchronously: one after another: slow but secure

## Read phenomena: 
**Problems or inconsistencies that may occur if transactions are not isolated from each other, all of them are in the scope of a single transaction**
### Dirty reads: 
**Situation in which one transaction reads data that were modified but not yet committed by second transaction.**
#### Example:
1. One transaction starts and reads some values in the data table
2. Second transaction starts and modifies data that was read by the first transaction
3. Transaction One reads once again modified data and gets different results, then it commits
4. Second transaction may commit - this will be not as bad, but also may rollback, effectively changes are rolled backed
and there is no trace after this change: there is no information why second read in the first transaction outcome was changed.
### Non-repeatable reads: 
**Situation in which you one read in the transaction and other read in the same transaction give different outcomes. But they are valid - not as in the dirty read**
#### Example:
1. One transaction starts and reads some values in the data table
2. The Second transaction starts and modifies data that was read by the first transaction
3. The Second transaction commits
4. The First transaction reads modified data and gets different results.
### Lost updates:
**Situation in which you write something in your transaction, then try to read this value, but it is already changed in the same transaction**
#### Example:
1. One transaction starts and updates some data in the database
2. the Second transaction starts and updates same data in the database, **important:** both of these transactions read and update value
   taken before the transaction started: they do not see updates made by one another.
3. The Second transaction commits
4. The First transaction tries to read updated value but gets different results than expected: updates made by the second transaction not by the first
### Phantom reads:
**Situation in which you read data in transactions two times, and it changes due to change in the table itself (for example inserts)**
#### Example:
1. One transaction starts and reads some values in the data table, using some `WHERE` clause (reads scope)
2. The Second transaction starts and inserts data to the table read by the first transaction that fits the scope of the first transaction
3. Second transaction commits
4. The First transaction reads again the same scope and gets a different result: no data was changed, but new rows were introduced.
## Database implementations of isolation:
1. Pessimistic: Locking to avoid lost updates: slow and heavy on resources but secure
2. Optimistic: No locks, just tracking changes and failing transactions if both modify the same data.
----
# Consistency:
## Consistency in data: 
What you have actually on the disc: are your data in the database consistent with the data model. **This is defined by the user - database model**.
Most of the time it comes to the referential integrity - if data refered to/from other table have sense. 
### Example: 
Case in which some table is related to another table by foreign key, and have column that specifies number of rows in this other table that is related 
to particular row in this table. In related table number of rows that reference that row not adds up to this value.

## Consistency in reads: 
Your data on the database may be consistent but you receive inconsistent reads due to having more than one database and differences 
during syncing different instances of your database.
### Example:
You update some data in the database to the value of X, this value is stored in one entity of the database. The next transaction may read said value from 
other entity of the database, change may not yet be populated to other databases and returned value may be pre update. Eventually there will be consistency 
but you may have to wait.
----
# Durability:
This is property of databases that talks about how much persistant is data that was stored to the database. Durability is slow. Some databases write 
to the disk only once every time, if crash or power outage will happen between those writes, every change from this span of time will be lost.
## Methods to achieve durability:
### WAL: write ahead log
Some databases write to disk only compressed deltas of changes not actual data, to reduce size of writes - when change in database is made, delta is automatically preserved
in the memory, in addition to that, actual database snapshot is preserved every once in the while.
### Asynchronous snapshot
Snapshots are preserved asynchronously, taking only part of databases resources.
### Append only file: AOF 
Similar to WAL: redis uses this.
## OS cache: 
Write request in OS usually goes through OS cache: instead of preserving data to memory in this moment, OS will store it in the cache, when enough data 
will be gathered, it will flush cached data to the memory in batch. Problem: all cached data is prone to deletion while crash or power outage occurs.
To work around this behaviour, there is `Fsync` OS command that forces write to disc - but it is expensive and slows commits.