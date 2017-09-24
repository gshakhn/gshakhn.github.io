---
title:  "On multi-table index cluster tables AKA improving data locality for related Oracle tables"
date:   2017-09-24 12:45:00
---

I've recently skimmed [Designing Data-Intensive Applications](https://dataintensive.net/). The book includes some things that are obvious in hindsight, but I never really thought about before. I'm trying to get back into blogging, so figured I'd jot down a few notes on interesting topics.

I've been using Oracle for a DB professionally for the last 7+ years. I can write simple SQL queries, think I understand how indexes work, get annoyed at the 30 character identifier limit, and am constantly baffled by Oracle treating an empty string as null. I've never really dug into Oracle internals though. Designing Data-Intensive Applications is a nice overview of how DBs work internally.

One feature I didn't realize Oracle had was multi-table index cluster tables. If you have 2 tables that share an attribute and you query both of them on the attribute, Oracle can store the data for both tables in the same block. That makes the read for that data only require reading in one block.

That is a very high level overview. For more detail, read the [official Oracle documentation](https://docs.oracle.com/cloud/latest/db112/CNCPT/tablecls.htm#CNCPT608) and [this post](http://www.dba-oracle.com/oracle_tip_hash_index_cluster_table.htm).