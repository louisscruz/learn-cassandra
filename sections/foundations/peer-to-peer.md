# Peer to Peer

Replication strategy relies on a client communicating with a leader.
That leader is then followed.
Additionally, that entire stack is then sharded.
This is far from ideal (due to latency in fail-overs and node partitions).

Because Cassandra relies on the coordinator node approach, node partitions are not a problem.
Both groups are able to function as expected.
