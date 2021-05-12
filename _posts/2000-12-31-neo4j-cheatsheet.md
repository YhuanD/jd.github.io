---
layout: post
title: neo4j cheatsheet (持续更新)
<!-- date: 2020-05-30 12:25:06 -0700 -->
<!-- tags: linux cheatsheet -->
<!-- comments: true -->
categories: KG
---


* TOC
{:toc}

cyther basics
==============

1\. 添加节点： `CREATE (:person{name:'XXX', sex:'XX'})`

2\. 添加关系： 

```sh
MATCH(a:cmp),(b:executive)
WHERE a.seccode='XXX' AND b.name='XXX'
CREATE(a)-[r:董事长]->(b)
```

5\. 查询某个节点及该节点连接的所有节点（任何关系）：

```sh
MATCH (a:cmp {seccode: 'XXX'})-[]-(b:executive)
RETURN a, b
```
