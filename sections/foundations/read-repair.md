# Read Repair

Entropy occurs.

## Proper Reads

When a read is done, the fastest node returns the data, but the slower nodes only return digests of the data.

If the data matches the rest of the digests, then consistency is achieved and a response can be given.

## Improper Reads

Read repair occurs when the coordinator gets responses that are not the same.
Data comes back from the fastest node, and the digests come from the other nodes.
Let's assume that the digests do not match.
The coordinator compares the timestamps.
The coordinator will request the data of the slower nodes and get their associated timestamps.
Once it has all of the timestamps, the coordinator will send the latest data to the client, then it will copy the newest data over to the other nodes asynchronously.

## Read Repair Chance

In the case that a read is done at less than a level of `ALL`, then Cassandra will perform a read repair sometimes.
It will do a background check on a subset of the replicas to ensure that the data is consistent.
By default, this occurs 10% of the time.

## Nodetool Repair

This allows you to manually ensure consistency.
This is typical used when a node has been offline for a while.

You should aim to run this once within each `gc_grace` period.
By default, that is ten days.

By running this manually, you reduce reliance on read repair.

