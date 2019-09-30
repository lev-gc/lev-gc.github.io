---
title: 数据库UID的一种生成策略 [PostgreSQL]
date: 2017-08-28 23:14:00
tags: [DataBase]
---
本文提供一种PostgreSQL的UID生成策略，参考自[Instagram公开的一种方案](https://instagram-engineering.com/sharding-ids-at-instagram-1cf5a71e5a5c)，除了可以确定产生的ID是唯一值以外，还有以下几种优点：

- 生成的ID按照时间排序（但排序结果跟插入顺序不一定一致）
- ID长度为64bit，不会过大
- 支持数据库集群（通过定义分片ID保证不同分片之间生成的ID不会重复）

可以为数据库字段赋予默认值为下面的`next_id()`函数，即可为每条记录产生唯一的值：

```sql
-- CREATE SEQUENCE
CREATE SEQUENCE public.table_id_seq
 INCREMENT 1
 MINVALUE 1
 MAXVALUE 9223372036854775807
 START 1
 CACHE 1
 CYCLE;

-- RESET VALUE OF SEQUENCE
SELECT setval('public.table_id_seq', 1, true);

-- CREATE FUNCTION
CREATE OR REPLACE FUNCTION public.next_id(OUT result bigint) AS $$
DECLARE
    our_epoch bigint := 1451577600000;
    now_millis bigint;
    seq_id bigint;  
    shard_id int := 1;
BEGIN
    SELECT nextval('public.table_id_seq') % 1024 INTO seq_id;
    SELECT FLOOR(EXTRACT(EPOCH FROM clock_timestamp()) * 1000) INTO now_millis;
    result := (now_millis - our_epoch) << 23;
    result := result | (shard_id << 10);
    result := result | (seq_id);
END;
$$ LANGUAGE PLPGSQL;
```

第一部分SQL是创建一个循环的SEQUENCE，第二部分是用来重置SEQUENCE的值，第三部分是创建供调用的函数。其中函数部分，`now_millis`用来获取当前时间的毫秒值；`our_epoch`定义了一个指代业务开始的时间毫秒值，可自定义，为了降低毫秒数的初始值；`shard_id`是为分布式数据库定义的一个分片ID，每个数据库（分片）的ID不能一样，为了确保多个数据库（分片）之间不会生成相同的ID；`seq_id`是通过SEQUENCE生成的一个自增长的序列。具体数据存储如下：

- 41bits存储毫秒时间
- 13bits存储逻辑分片的ID
- 10bits存储序列对1024求模的结果

可以让每个分片在每毫秒里能产生1024个唯一的ID，对RDBMS的性能来说是足够的。当然日后如果有并发量达到每秒10万以上的性能，也可以调整后面求模的数值并调整存储位的分布。
