# Introduction to CQL

Cassandra Query Language is used to create keyspaces, tables, and core datatypes.
It looks very similar to SQL, though there aren't joins, groups, or aggregates.

## Keyspaces

These are containers.
It's primary purpose is to store replication information.

An example would be:

```cql
CREATE KEYSPACE killrvideo
WITH REPLICATION = {
  'class': 'SimpleStrategy',
  'replication_factor': 1
};
```

## Use

This is how you can switch between keyspaces.

```cql
USE killrvideo;
```

## Tables

Tables contain data, columns, and primary keys.
The primary keys show uniqueness of records.

### Data Types

#### `text`

This defaults to UTF8, not ASCII.
`varchar` is the same as text.

#### `int`

These are signed and 32 bits.

#### `timestamp`

This stores the date and time.
To do so, it uses a 64 bit integer that stores how many seconds have elapsed since Jan. 1, 1970.

#### `UUID` and `TIMEUUID`

These are critical for the use of Cassandra.
They are both 128 bit numbers.

`TIMEUUID`s are sortable.
Also, they take off the first 64 bits to arrive at the timestamp.
This ensures that if multiple entries are made at the same time, they won't collide.

In general, these are important because distributed systems can't rely on sequential ids.

You can call both `uuid()` and `now()`.

## Insert

```cql
INSERT INTO users (user_id, first_name, last_name)
VALUES (uuid(), 'Louis', 'Cruz');
```

## Select

This is very similar to SQL, except that complex `WHERE` clauses (joins) are not allowed.

```cql
SELECT *
FROM users;

SELECT first_name, last_name
FROM users;

SELECT *
FROM users
WHERE user_id = 121212123-1232-1232-1232-123212322
```

## Copy

This command allows you to import and export CSV.
Use `WITH HEADER=true` to skip the first line of the file.

```cql
COPY table1 (column1, column2) FROM 'table1data.csv';

COPY table1 (column1, column2) FROM 'table1data.csv'
WITH HEADER=true;
```
