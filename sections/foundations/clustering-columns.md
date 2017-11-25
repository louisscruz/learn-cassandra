# Clustering Columns

Clustering allows for the sorting of data.

## Clustering Columns in Primary Keys

Imagine that we have a `PRIMARY KEY (state, id)`

```cql
(id, name, state, city)
1 D  TX Houston
2 C  TX Dallas
3 L  TX Snyder
4 I  TX Austin
5 A  TX Dallas
6 L2 TX El Paso
7 D2 TX Austin
8 L3 TX San Antonio
9 C2 TX Houston
```

The partition key for Texas in this case could have been, say 83.

Any argument after the first argument is a clustering column.

We could then sort by the city by doing the following: `PRIMARY KEY ((state), city, id)`

```cql
(id, name, state, city)
4 I  TX Austin
7 D2 TX Austin
2 C  TX Dallas
5 A  TX Dallas
6 L2 TX El Paso
1 D  TX Houston
9 C2 TX Houston
8 L3 TX San Antonio
3 L  TX Snyder
```

Or we could sort by name as follows: `PRIMARY KEY ((state), name, id)`

```cql
(id, name, state, city)
5 A  TX Dallas
2 C  TX Dallas
9 C2 TX Houston
1 D  TX Houston
7 D2 TX Austin
4 I  TX Austin
3 L  TX Snyder
6 L2 TX El Paso
8 L3 TX San Antonio
```

We could perform a sub-sort on the city, then the name as follows: `PRIMARY KEY ((state), city, name, id)`

```cql
(id, name, state, city)
7 D2 TX Austin
4 I  TX Austin
5 A  TX Dallas
2 C  TX Dallas
6 L2 TX El Paso
9 C2 TX Houston
1 D  TX Houston
8 L3 TX San Antonio
3 L  TX Snyder
```

## Querying Clustering Columns

When querying clustering columns data, you must provide the partition key, since that specifies where the data is located.

You can perform either equality (=) or range queries (<, >) on clustering columns.
However, all equality comparisons must come before inequality comparisons.

Since data is sorted on disk, range searches are a binary search followed by a linear read.

## Default Ordering

Clustering columns defaults to ascending order.

The ordering can be overridden with `WITH CLUSTERING ORDER BY(x DESC, y ASC)`.

When doing this, you must include all columns including and up to the columns you wish to order descending.

For instance, `id` is assumed to be ascending below:

```cql
CREATE TABLE users (
  state text,
  city text,
  name text,
  id uuid,
  PRIMARY KEY((state), city, name, id)
  WITH CLUSTERING ORDER BY(city DESC, name ASC)
)
```

This is very performant, since it changes the manner in which data is written to disk, giving fast lookups.

## Allow Filtering

Allow filtering should almost never be done. It assumes that you have to look at the entire cluster, and not just the specific partition.
