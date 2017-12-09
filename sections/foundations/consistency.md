# Consistency

## CAP Theorem

Consistency is basically gone when you start using several servers to store data.

Cassandra provides availability and partition tolerance, but certain features allow you to control the level of consistency.

For every read and write, the client specifies the consistency level.
If `CL=ONE`, then the cordinator will return a response after a single node responds positively.
If `CL=QUORUM`, then the cordinator will return a response after the majority of nodes respond positively.
If `CL=ALL`, then all the nodes will have to respond.
That's not resilient at all.
If a node were to be unavailable, then you can't respond until it becomes available.

## Consistency Levels

### Strong Consistency

In this scenario:

1. All writes occur with `WRITE CL=ALL`.
2. All reads are able to use `READ CL=ONE`.

### Consistency Level Quorum

In this scenario:

1. All writes occur with `WRITE CL=QUORUM`.
2. All reads are able to use `READ CL=QUORUM`.

### Consistency Level One

In this scenario:

1. All writes occur with `WRITE CL=ONE`.
2. All reads occur with `READ CL=ONE`.

This is very unlikely to achieve any meaningful level of consistency.
This setup can give you a great level of availability, however.

## Consistency Across Data Centers

Using `QUORUM` across multiple data centers would cause a major performance hit.
Instead, use `LOCAL QUORUM`.
This will ensure that a quorum is only achieved in the local data center.

## Consistency Settings

In order of weakest to strongest:

* `ANY`
* `ALL`
* `ONE`, `TWO`, `THREE`
* `QUORUM`
* `LOCAL_ONE`
* `LOCAL_QUORUM`
* `EACH_QUORUM`
