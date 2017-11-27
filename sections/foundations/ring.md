# Ring

The range of the ring goes from -2^63 to (2^63)-1.

The partitioner determines how the data is distributed.

When a new node is added to a ring, all of the other nodes recalculate their token ranges and begin sending the appropriate data to the new node.

Nodes have four states in the ring:

* Joining
* Reading
* Up
* Down

When data is being streamed to the new node, it is not ready to be in use.

The drivers need to be aware of which nodes are available in the ring (and what their statuses are).
Having a driver aware of the appropriate node can bypass the entire coordination phase.
