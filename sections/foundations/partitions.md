# Partitions

Partitions allow for us to create ordering of data.
The partitioner generates the key for the placement of data based on the incoming data.
This relies on the primary key.

Take the following data in RDBMS format:

```
(id, name, state)
1 A TX
2 B NY
3 C TX
4 D CA
5 E CA
6 F NY
```

We might choose to partition this data by state:

```
CA Partition:
4 D CA
5 E CA

NY Partition:
2 B NY
6 F NY

TX Partition:
1 A TX
3 C TX
```

To arrive at this, we could attempt to set `PRIMARY KEY (state)`, but states aren't unique, so that wouldn't work.
To get this to work, we would need to mix in a unique value, in the case the ID, like so: `PRIMARY KEY ((state), id)`.
