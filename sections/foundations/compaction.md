# Compaction

Each SSTable could have five files associated with it:

* Bloom Filter
* Key Cache
* Partition Summary
* Partition Index
* SSTable

Because SSTables are immutable, the size of these tables can become quite sizable.

Compaction is an in-memory merge sort, where newer records are copied in favor of older records.

If there is a tombstone (delete record) at time of compaction, and if the `gc_grace` period has passed, then that record is not copied into the new SSTable.

The reduction in historical data, and the reduction of unnecessary data, makes our reads significantly faster and saves us space.
