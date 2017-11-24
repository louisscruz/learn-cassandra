# Overview

## Consistent Hashing

Cassandra makes use of consistent hashing.
This hashing determines where a given model will be placed on the ring of nodes.

## Replication Factor

In Cassandra, you can set a replication factor (RF).
This RF determines how many other nodes each write will be copied to.

For example, imagine there are four nodes, where each has also acts as a replicant store, and where RF = 3:

| Server | Replicates |
| :----: | :--------: |
| A      | C & D      |
| B      | A & D      |
| C      | B & C      |
| D      | A & B      |

In this scenario, if a write is made to node A, then it is also copied to nodes B and D.
Note that the RF represents how many nodes in total the data is copied to.

Hinted handoff is something that is used when a node is down.

## Consistency Levels

Each query can set its own consistency.

There are three settings:

* All
* Quorum
* One

These settings basically determine how many different nodes have to be queried for a response to be given (with all the data being consistent).
In other words, how many replicas are required before we respond with and OK status?

The two most popular settings are one and quorum.
Quorum just specifies that a majority of the nodes must respond.
With RF = 3, that would mean that two nodes must respond.

## Multiple Data Centers

Asynchronous data replication means that it's very easy to have multiple data centers.
The consistency levels in a multi data center configuration would specify that consistency is locally all, quorum, or one.

## The Write Path

Writes can be written to any node in the cluster.
The node that happens to be communicated with initially is called the coordinator.
That node acts as a coordinator for that given request.

After a write request is assigned a coordinator, two things happen:

1. The write is written to a commit log on the disk, which is append-only (very fast).
2. After it's written to the commit log, it is written to a memtable, which is an in-memory store.

Once these things happen, a response can be sent to the client.

Every column gets a timestamp on write.

The memtable is occasionally flushed to disk, into an sstable.

Deletes are a special write case called a tombstone.

## SSTables

SSTables are immutable files for row storage that are written to disk.

Compaction takes smaller tables and joins them.
During this process, only the latest timestamp is kept.

The same column can be in multiple SSTables.

This process makes backups very simple.

## The Read Path

Just as with writes, the initial node is the coordinator.
The coordinator contacts the nodes with the requested key.
On each node, data is pulled from the SSTables and merged.
Compaction may not have occurred yet, since it is run as a background process.
After the merging occurs, any un-flushed memtable data is merged, and then the response is given.

If the consistency level for the request is less than all, then there is also a process that occurs called read repair.
After about 10% (by default) of these kinds of reads, a background job will occur to ensure that all the data is in sync.
