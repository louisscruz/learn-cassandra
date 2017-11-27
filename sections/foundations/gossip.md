# Gossip

Each node initiates a gossip round every few seconds.

In this process, it picks one to three nodes to gossip with.
They can gossip with any other node in the cluster.
Downed nodes are slightly favored in this.
Nodes do not track which nodes they'd previously gossiped with.

Gossip is used to transmit metadata (about load, heartbeat, etc.).

When one node gossips with another, it compares its own data with the data of the other node.
A manifest is created that says what needs to be updated on the original node and what needs to be updated on the other nodes.
This manifest is sent back to the original node and updated.
Updates occur on the original node, and the updates to other nodes go out.
