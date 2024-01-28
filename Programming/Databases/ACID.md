### Transactions: 
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
### Atomicity: 
**All the queries in the transaction must succeed**. If any query fails for any reason, entire 
transaction will be rollback. Before the commit: all the successful queries should rollback