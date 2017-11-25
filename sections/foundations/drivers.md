## Drivers

Drivers handle how Cassandra is able to be communicated with.
DataStax has drivers in many different languages.

In Python, you'd do the following to get a basic cluster:

```python
from cassandra.cluster import Cluster
cluster = Cluster()
session = cluster.connect('killrvideo')
```

You create a cluster object, then use the cluster object to obtain a session.
The session then manages the connection to the cluster.
It is also able to know/determine performance characteristics of your nodes.

Once you have a session, you can execute queries with it:

```python
result = session.execute("SELECT * FROM users WHERE userid=12c987ac-foij-woij-foij-36478762k")[0]
print result.firstname, result.lastname, result.email
```

And that would output `Louis Cruz louis@test.com`.

There are also query builders to prevent SQL injection.
