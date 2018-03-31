---
title: "Implementing Database Indexing"
date: 2014-09-24T19:02:29-05:00
draft: false
---

A database can be thought of as a table, where each row is an entry, and each column is a field of that entry.

When you want to find something in that table, naturally, you would look through each row and check the attributes to see if that's what you're looking for. The problem is that, as the size of the database grows, the time it takes to search this way grows linearly, a complexity of O(n).

In a database, each entry that is added is given a unique and increasing ID number. So if we know the ID number of the entry we want to look up, we can implement a binary search through the database to find that entry. Binary search gives a time complexity of O(log(n)) - much better than the linear O(n) we saw earlier.

However, in most cases, we don't know the ID number, but instead, we'll only know the attributes we're looking for. Since the database is currently only sorted by the unique ID number, we'll need another way to make searching by attributes faster than our O(n) method.

### Indexing

Indexing is a way of organizing data by fields other than the unique ID given to each entry. Because sorted or organized data can be searched much more quickly than unsorted data, indexing allows for an efficient trade-off of space for speed.

By using an index, we can look up the ID number via the indexed field, and use that ID number to look up the entry in the original database table.

A possible data structure for indexing could be a hash table. Hash tables allow for instantaneous read and write to look up for the ID number. However, hash tables use a hashing algorithm that requires exact matches. Since useful databases would allow queries to search for inexact matches, like "select all where name starts with 'a'", a hash table would not allow for that kind of query.

### B-Tree
A better way to organize indexes is to implement a B-Tree. A B-Tree is short for Balanced-Tree (commonly mistaken as Binary Tree), which is a tree structure that balances itself, where each leaf, or terminal node, is equidistant to the root. Because of this quality, searching through a B-Tree is consistent and very fast.

Among other advantages over a hash, since a B-Tree sorts the keys and values, you can use comparators like <=, <, >, >=, and BETWEEN.

An example implementation of a database using a B-Tree index can be found here:
https://github.com/tranhungt/mini-db/tree/master/lib

