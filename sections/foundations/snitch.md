# Snitch

Snitching is the process of determining where data should go.

There are many different snitching processes.

## Simple Snitching

The default process, simple snitch, is not good for production.
It will evenly distribute data, even across data centers.

## Property File Snitch

Property file snitch requires that you explicitly list all nodes with their IP addresses.
This stinks.

## Gossiping Property File Snitch

This is the one to use in most cases.
You declare the data center and rack for the current node.
Adding a new node is then very easy.

## Dynamic Snitch

This is something that is used by default on all snitch strategies.
It essentially determines which nodes are under more pressure than the others and routes traffic based on that.
