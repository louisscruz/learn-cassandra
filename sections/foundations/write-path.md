# Write-Path

The write-path for Cassandra is much simpler than in relational databases.

## What Happens on Write

1. The data is sent to the appropriate node(s).
2. On a given node, there is a server process running on the JVM.
3. The data is then written to the commit log on disk.
4. Then the data is written to a MemTable.
5. The write is then acknowledged.

As more data is added, the MemTable adheres to the modeling.
For instance, if the data model specifies that the data is ordered by city, then it will be that way in the MemTable.
The CommitLog will not be this way though.
It is append-only, where the records are just stored by time.

Eventually, the MemTable is flushed to disk as an SSTable.
This happens because we do not have an infinite amount of memory.
When the SSTable stores its entries in the same order as in the MemTable.
SSTables are entirely immutable.

This practice does not rely on a single, large file, unlike relational databases.
Using these SSTables is a practice of sparse storage.

If you happen to use a hard disk on your server, use two hard drives: one for your commit log and one for the SSTables.
Doing so will ensure that the disk head is not constantly moving from the back of the commit log to whatever random points are needed for reads.

## Compaction

Eventually, you'll have too many SSTables to keep things performant.
To combat this, there is a process called compaction that joins these SSTables.

## Timestamps

In Cassandra, the last write wins, so any updates or deletions are properly read.
