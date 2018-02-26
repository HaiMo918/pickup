# 解决hive查询表死锁的问题

1. 登录到master服务器
	
		ssh -i yintech_hdp_dev_rsa root@master.bigdata.ytx.com
		
2. 连接本地mysql元数据库

		mysql -uhive -pHive_123456
		
3. 制定数据库

		show databases;
		use hive;
		
4. 根据表名（或库名）查询是否死锁

		select * from HIVE_LOCKS where HL_TABLE = 'query_table';
		或
		select * from HIVE_LOCKS where HL_DB = 'query_db';
		
5. 根据表名（库名）删除死锁

		delete from HIVE_LOCKS where HL_TABLE = 'query_table';
		或
		delete from HIVE_LOCKS where HL_DB = 'query_db';