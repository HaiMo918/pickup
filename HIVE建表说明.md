# HIVE建表说明

## 物理表

	CREATE TABLE `TableName`(
		`id` string COMMENT 'This is primary key',
		`col1` string COMMENT 'The remark of col1',
		`col2` string COMMENT 'The remark of col2'
	) COMMENT 'The remark of table'
	PARTITIONED BY (create_day date)
	CLUSTERED BY(id) INTO 6 BUCKETS
	STORED AS ORC
	TBLPROPERTIES (
		'orc.compress'='ZLIB', 
		'transactional'='true'
	);

## 临时表

	CREATE TEMPORARY TABLE `TableName`(
		`id` string COMMENT 'This is primary key',
		`col1` string COMMENT 'The remark of col1',
		`col2` string COMMENT 'The remark of col2'
	) COMMENT 'The remark of table'
	PARTITIONED BY (create_day date)
	CLUSTERED BY(id) INTO 6 BUCKETS
	STORED AS ORC
	TBLPROPERTIES (
		'orc.compress'='ZLIB', 
		'transactional'='true'
	);

---

__Notice__

1. 分桶属性必须是哈希值散列的，例如记录唯一标识id
2. 分区属性不是必需的，但在记录数大的表中建议以日期作为分区属性
3. 需要对表进行事务操作，必需开启事务属性
4. 指定以ORC格式存储
5. 必须添加必要的属性说明和表说明
6. 禁止update分区属性

---

___Q&A___

1. 为什么使用分区属性？

	```
	在Hive Select查询中一般会扫描整个表内容，会消耗很多时间做没必要的工作。有时候只需要扫描表中关心的一部分数据，因此建表时引入了partition概念。分区表指的是在创建表时指定的partition的分区空间。 
	Hive可以对数据按照某列或者某些列进行分区管理，所谓分区我们可以拿下面的例子进行解释。当前互联网应用每天都要存储大量的日志文件，几G、几十G甚至更大都是有可能。存储日志，其中必然有个属性是日志产生的日期。在产生分区时，就可以按照日志产生的日期列进行划分。把每一天的日志当作一个分区。 
	将数据组织成分区，主要可以提高数据的查询速度。至于用户存储的每一条记录到底放到哪个分区，由用户决定。即用户在加载数据的时候必须显示的指定该部分数据放到哪个分区。
	```

2. 为什么使用分桶属性？

	```
	对于每一个表或者分区， Hive可以进一步组织成桶，也就是说桶是更为细粒度的数据范围划分。Hive也是 针对某一列进行桶的组织。Hive采用对列值哈希，然后除以桶的个数求余的方式决定该条记录存放在哪个桶当中。
	把表（或者分区）组织成桶（Bucket）有两个理由：
	（1）获得更高的查询处理效率。桶为表加上了额外的结构，Hive 在处理有些查询时能利用这个结构。具体而言，连接两个在（包含连接列的）相同列上划分了桶的表，可以使用 Map 端连接 （Map-side join）高效的实现。比如JOIN操作。对于JOIN操作两个表有一个相同的列，如果对这两个表都进行了桶操作。那么将保存相同列值的桶进行JOIN操作就可以，可以大大较少JOIN的数据量。
	（2）使取样（sampling）更高效。在处理大规模数据集时，在开发和修改查询的阶段，如果能在数据集的一小部分数据上试运行查询，会带来很多方便。
	```

3. 为什么要开启事务属性？

	```
	Hive从0.14版本开始支持事务和行级更新，但缺省是不支持的，需要一些附加的配置。要想支持行级insert、update、delete，需要配置Hive支持事务。
	```

4. 为什么使用ORC格式存储？

	```
	ORC文件格式是一种Hadoop生态圈中的列式存储格式，用于降低Hadoop数据存储空间和加速Hive查询速度。ORC具有以下一些优势:
	（1）ORC是列式存储，有多种文件压缩方式，并且有着很高的压缩比。
	（2）文件是可切分（Split）的。因此，在Hive中使用ORC作为表的文件存储格式，不仅节省HDFS存储资源，查询任务的输入数据量减少，使用的MapTask也就减少了。
	（3）提供了多种索引，row group index、bloom filter index。
	（4）ORC可以支持复杂的数据结构（比如Map等）
	（5）只有ORC支持事务操作
	```