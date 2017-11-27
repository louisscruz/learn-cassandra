## Node

The node is the lowest component in the Cassandra architecture.

Cassandra is written in Java, so each node is running on the JVM.

It can be expected that a single node can handle 3000 - 5000 transactions/second/core.
Each node can handle 1 - 3 terabytes.

At all costs, avoid network storage.

## Nodetool

This is located at `bin/nodetool`.

You can pass arguments of `help`, `info`, `status`, and many more.
