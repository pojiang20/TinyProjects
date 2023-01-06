#### 数据获取层
数据获取方式由通过配置文件决定，数据的从rsDB中获取，规则是从bucketDB中获取。

全量元数据获取：建立一个BucketSession，Find(nil)获取迭代器遍历整个表，不停的next。
lcc规则前缀获取：从bucketDB中获取bucketRule，以公共前缀对bucketRule分组为ruleGroup，筛选每个ruleGroup中的最大prefix范围，以最大范围查询rsDB来获取数据，ruleGroup间并发执行。
#### 规则匹配层
遍历rs数据，一条rsEntry，和ruleGroup下多条bucketRule匹配，以prefix最长匹配作为该条rsEntry的bucketRule，{rsEntry,bucketRule}发送给channel

方案1是所有rs数据匹配所有bucketRule的prefix，方案2是同组的rs数据匹配同组的bucketRule的prefix。

#### 数据缓存
从channel获取{rsEntry,bucketRule}，判断是否需要执行动作。如果需要执行动作，则将该rsEntry追加到bucketRule作为key的队列中。当队列长度到达上限则Flush调用rsApiClient。

如果Flush成功，则记录该bucketRule对应的key进度。

#### rs访问层
提供batch([]rsTask)接口，内部是/batch发送http请求，并有重传机制处理请求失败，写入成功则返回nil。

#### 断点重传
以ruleGroup的id作为文件名存储到本地文件，本地文件中的key是bucketRule的最小key。

