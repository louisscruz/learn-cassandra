# Read-Path

This is a bit more complicated than the write path.

## Reading Data

We have both MemTables and SSTables.
Our data could be in many places, so some combination has to occur.

### Reading a MemTable

Each node stores (most likely) several partitions.
When a read occurs, there is a lookup on the key of the desired partition, which points to a collection of records.

The record we are trying to read might not be there.
In that case we need to look into the SSTables.

### Reading an SSTable

The SSTable is also comprised of many partitions.
In order to get a constant time read of the correct partition's data, a partition index is built (it must happen on flush).

If there ends up being many, many partitions stored in a single SSTable, then the partition index's token lookup becomes the bottleneck.
To get around this, a summary index is generated that points to a range of tokens with their appropriate offset.

With this, if a read comes in for partition 5, it would look up the partition in the summary index, which could look as follows:

| Range | ByteOffset |
| ----- | ---------- |
| 0 - 3 | 0          |
| 4 - 6 | 1224       |
| 7 - 9 | 5559       |

With this, we know that we need to look up the token in the SSTable with the token, and that token lies somewhere after the byte offset of 1224.
We do a linear read from the offset until we find the token of 5.

Once we get the byte offset from the partition index, we go to the corresponding byte offset in the SSTable.

If there are partitions that are frequently used, you can have a cache that is in front of all of these indices to find the exact location in order to bypass both the partition index and the summary index.

In front of all of this you can use a bloom filter.
The bloom filter, given a partition index, is used to tell whether the SSTable definitely does not have the partition or if it might have the partition.
This cuts down on the number of SSTables that we end up having to scan.
