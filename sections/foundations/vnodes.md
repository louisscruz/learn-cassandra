# Vnodes

Vnodes are virtual nodes.

Imagine a scenario where there is a hotspot in a ring.
A new node could be introduced, but it will have to evenly bifurcate the hot section of the ring, and it could potentially introduce a hot spot in a new ring (if the ring is already evenly divided).

Vnodes handle this situation well.
Basically put, each real node has many virtual nodes in the ring.
Each key range is split into many even sections that point to other nodes.
Those pointers are virtual nodes.

In version before 3.0, there are 256 Vnodes per node.
In later versions, less are needed by default, but there it is configurable.
The `num_tokens` section in the configuration allows for this to be changed.
