# TinyProjects
一些小项目

项目名称 | 简介
---- | ----
miniShell | c语言实现shell的基本功能    
gee | 简单的路由框架，前缀树解析路径、实现中间件Next()功能
[geeCache](https://github.com/pojiang20/Geecache) | 简单的分布式缓存，LRU缓存淘汰、一致性哈希处理请求

#### raft相关
项目名称 | 简介
---- | ----
[elibenRaft](https://github.com/pojiang20/elibenRaft) | 学习来源于[Eli Bendersky's website](https://eli.thegreenplace.net/2020/implementing-raft-part-1-elections/)博客中的Implementing Raft系列文章

## 模型 
工作中的一些简单的结构/模型记录
#### splitTool
<img src="./static/img/splitTool.png" width="200">  

用于monogo手动分片，本质上是两个消费者处理消息，但消息可能需要再次处理。该模型的问题是，会发生死锁。因为消费者B的消息可能会返还给消费者A，这样当消费者A阻塞而此时消费者B返还消息给A，就会导致死锁，目前的解决办法的扩大消费者A的入队缓冲区。
- [ ] 有无更好的解决办法 

简单并发模型
