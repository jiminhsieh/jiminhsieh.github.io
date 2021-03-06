---
layout: post
title: "Be Careful Of Working with JDBC"
date: 2017-10-04
tags: 
    - java
    - scala
description: Some of things we need to know JDBC. 
comments: true
share: true
---

JDBC is blocking IO, no matter what we do with JDBC. When we wrap with Future, we just make it runs **asynchronously** in the background. Further, we get the result from Future, it still blocks our code when it doesn't finish.

When we use libraries contains connection pool, be careful of too many or too little of connections. Too many connections, it would crash database. Too little connections, your application would under performance, because of lots of queries would be blocked. How much connections should your application have? There are two perspectives we could consider.
* From database - Connections = (core_count * 2) + effective_spindle_count
    * This is the [suggestion from PostgreSQL](https://wiki.postgresql.org/wiki/Number_Of_Database_Connections).
* From application - Threads = CPU Cores * CPU Utilization * (1 + Wait Time / Compute Time)
    * This is the suggestion from Java Concurrency in Practice.
