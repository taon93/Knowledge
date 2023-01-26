- Vector vs ArrayList: Vector is synchronized - may be used for multithreading functions.
- Set don't guarantee order. 
- LinkedHashSet supports order.
- PriorityQueue: Queue in which elements are inserted according to their natural ordering - sorted
- ----
### Multithreaded collections
- Synchronized vs Concurrent collections: Synchronized collections are older and use synchronizaed blocks as the method
of achieving safe concurrency - this method place severe restrictions on concurrency in threads, thus causing performance
loss, Concurrent collections are more robust and faster - they were introduced after java 5 and use mechanisms such as:
  - Copy on write
  - Compare and Swap
  - Locks
- **Copy on write**: all values stored in collection are stored in immutable array, new array is created on each modification 
of the collection. Read operations are not synchronized. Implementations: `CopyOnWriteArrayList` & `CopyOnWriteSet`. Typically
used in Subject/Observer pattern, where observers rarely change. 
- **CompareAndSwap**: Instead of locking/synchronizing entire method, during `CompareAndSwap` when member of the variable 
is changed, it is first cached then swaped and then current value is compared with cached one. If values don't match
(because other thread also modified this variable) then operation is retried or skipped - depending on the use case. 
`ConcurrentLinkedQueue` uses this approach.
- **Locking**: Instead of locking each `synchronized` methods in one synchronization block - thus effectively blocking all methods
even if only one is currenly used by one thread; you use `ReentrantLock's` - this lock enables finer granulation of locking blocks -
so you have more flexibility and less waiting time between threads. `CopyOnWriteArrayList` uses this approach.
- ----
> **Initial capacity of Java Collection:** number of buckets in the hash table at the time of collection creation.   
> **Load factor:** Ratio between number of elements in the collection and number of buckets - capacity. Default load factor
> of 0.75 offers good tradeof between time and space cost. Reducing load factor may improve time efficiency but will affect
> space taken by the collection. 
- `UnsuportedOperationException` in collections: Java Collections implement `Collection` interface - thus they must provide
implementation for each method from this interface. Some Java Collections are meant to be used in specific use cases and don't
support some operations: when they will be requested to do operation that they don't support they will throw this exception.
Example: `Arrays.asList` returns fixed size list - when there will be attempt to add or remove from this collection
exception will be thrown. 
- `Arrays.asList` vs `List.of`: `Arrays.asList` wrapps initial array into fixed size list: additions and removals are not supported
and also list still references this array: changes made to this array will reflect on the list:
```
Integer[] array = new Integer[]{1,2,3};
List<Integer> list = Arrays.asList(array);
array[0] = 1000;
assertThat(list.get(0)).isEqualTo(1000); // !!!
```
list returned by the `Arrays.asList` is mutable. `List.of` is immutable, that is the copy of the provided input array,
also `null` values are not allowed. 