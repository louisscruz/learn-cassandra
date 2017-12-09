# Replication

It is recommended that you use a replication factor of three.
Data is replicated, by default, to its neighbor.
With a replication factor of three, the data is copied to its neighbor and its neighbor's neighbor.

## Multi-Center Replication

In this scenario, you can have multiple ring regions.
Replication can be distributed through a network topology strategy.
This approach basically bifurcates a single set of replication assignments across two centers.

```cql
CREATE KEYSPACE killrvideo
WITH REPLICATION = {
'class': 'NetworkTopologyStrategy',
'dc-west': 2, 'dc-east': 3
}
```

The above configuration would act as follows:

1. Data comes into a given node on the west coast.
2. A consistent hash key is generated for that data.
3. That data is then written to two nodes in the west coast.
4. Asynchonously, the data is also sent to the east coast.
5. A random node coordinates that data to be replicated across three nodes in its own center.

With this setup, you're now protected from entire-region failures.
