---
layout: post
title: "Finding out databases & tables sizes in MySQL"
excerpt: >-
  How to find which databases or tables takes the most space in
  your MySQL instance.
categories: quickies
permalink: mysql-database-table-sizes
share: true
tags:
  - database
  - mysql
author: mb
image:
  path: /images/posts/mysql-database-size.jpg
  thumbnail: /images/posts/mysql-database-size-thumb.jpg
  caption: "Photo credit [Pexels](https://pixabay.com/fr/users/Pexels-2286921/)"
---

Sometimes it comes in handy to check what database or table takes a lot
of space in MySQL. For instance, if your backups starts to get too big,
you might want to check where the size goes.

Some frameworks are sometimes stuffing a lot of cache data in the
database itslef (I'm looking at you Drupal).

Here are a few SQL snippets to check where you precious SSD goes.

## Listing the 10 biggets databases

```sql
SELECT
    count(*) tables,
    table_schema,concat(round(sum(table_rows)/1000000,2),'M') rows,
    concat(round(sum(data_length)/(1024*1024*1024),2),'G') data,
    concat(round(sum(index_length)/(1024*1024*1024),2),'G') idx,
    concat(round(sum(data_length+index_length)/(1024*1024*1024),2),'G') total_size,
    round(sum(index_length)/sum(data_length),2) idxfrac
    FROM information_schema.TABLES
    GROUP BY table_schema
    ORDER BY sum(data_length+index_length) DESC LIMIT 10;
```

## Listing the 20 biggest tables (regardless of the database)

```sql
SELECT CONCAT(table_schema, '.', table_name),
       CONCAT(ROUND(table_rows / 1000000, 2), 'M')                                    rows,
       CONCAT(ROUND(data_length / ( 1024 * 1024 * 1024 ), 2), 'G')                    DATA,
       CONCAT(ROUND(index_length / ( 1024 * 1024 * 1024 ), 2), 'G')                   idx,
       CONCAT(ROUND(( data_length + index_length ) / ( 1024 * 1024 * 1024 ), 2), 'G') total_size,
       ROUND(index_length / data_length, 2)                                           idxfrac
FROM   information_schema.TABLES
ORDER  BY data_length + index_length DESC
LIMIT  20;
```

## Listing total data size for MySQL instance

```
SELECT CONCAT(SUM(ROUND(index_length / ( 1024 * 1024 * 1024 ), 2)),' G') idx,
       CONCAT(SUM(ROUND(( data_length + index_length ) / ( 1024 * 1024 * 1024 ), 2)),' G') total_size
FROM   information_schema.TABLES;
```

## Examining sizes per database engine

```sql
SELECT engine,
    count(*) tables,
    concat(round(sum(table_rows)/1000000,2),'M') rows,
    concat(round(sum(data_length)/(1024*1024*1024),2),'G') data,
    concat(round(sum(index_length)/(1024*1024*1024),2),'G') idx,
    concat(round(sum(data_length+index_length)/(1024*1024*1024),2),'G') total_size,
    round(sum(index_length)/sum(data_length),2) idxfrac
FROM information_schema.TABLES
GROUP BY engine
ORDER BY sum(data_length+index_length) DESC LIMIT 10;
```



