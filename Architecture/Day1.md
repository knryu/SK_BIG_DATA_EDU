<pre><code>
training@elephant:~$ sudo ln -s /usr/share/java/mysql-connector-java.jar /opt/cloudera/parcels/CDH-5.3.2-1.cdh5.3.2.p0.10/lib/sqoop/lib/
ln: target `/opt/cloudera/parcels/CDH-5.3.2-1.cdh5.3.2.p0.10/lib/sqoop/lib/' is not a directory: No such file or directory
training@elephant:~$ sudo ln -s /usr/share/java/mysql-connector-java.jar /opt/cloudera/parcels/CDH-5.9.0-1.cdh5.9.0.p0.23/lib/sqoop/lib/
training@elephant:~$ readlink -f /opt/cloudera/parcels/CDH-5.9.0-1.cdh5.9.0.p0.23/lib/sqoop/lib/mysql-connector-java.jar 
/usr/share/java/mysql-connector-java-5.1.34.jar
training@elephant:~$ sqoop help
Warning: /opt/cloudera/parcels/CDH-5.9.0-1.cdh5.9.0.p0.23/bin/../lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
19/05/14 23:07:07 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.9.0
usage: sqoop COMMAND [ARGS]

Available commands:
  codegen            Generate code to interact with database records
  create-hive-table  Import a table definition into Hive
  eval               Evaluate a SQL statement and display the results
  export             Export an HDFS directory to a database table
  help               List available commands
  import             Import a table from a database to HDFS
  import-all-tables  Import tables from a database to HDFS
  import-mainframe   Import datasets from a mainframe server to HDFS
  job                Work with saved jobs
  list-databases     List available databases on a server
  list-tables        List available tables in a database
  merge              Merge results of incremental imports
  metastore          Run a standalone Sqoop metastore
  version            Display version information

See 'sqoop help COMMAND' for information on a specific command.
training@elephant:~$ sqoop list-databases --connect jdbc:mysql://localhost --username training --password training
Warning: /opt/cloudera/parcels/CDH-5.9.0-1.cdh5.9.0.p0.23/bin/../lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
19/05/14 23:08:51 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.9.0
19/05/14 23:08:51 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/05/14 23:08:51 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
information_schema
movielens
mysql
test
training
training@elephant:~$ sqoop list-databases --connect jdbc:mysql://localhost/movielens --username training --password training
Warning: /opt/cloudera/parcels/CDH-5.9.0-1.cdh5.9.0.p0.23/bin/../lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
19/05/14 23:09:25 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.9.0
19/05/14 23:09:25 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/05/14 23:09:25 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
information_schema
movielens
training@elephant:~$ sqoop import --connect jdbc:mysql://localhost/movielens --table movie --fields-terminated-by '\t' --username training --password training
Warning: /opt/cloudera/parcels/CDH-5.9.0-1.cdh5.9.0.p0.23/bin/../lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
19/05/14 23:12:36 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.9.0
19/05/14 23:12:36 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/05/14 23:12:36 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
19/05/14 23:12:36 INFO tool.CodeGenTool: Beginning code generation
19/05/14 23:12:37 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `movie` AS t LIMIT 1
19/05/14 23:12:37 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `movie` AS t LIMIT 1
19/05/14 23:12:37 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /opt/cloudera/parcels/CDH/lib/hadoop-mapreduce
Note: /tmp/sqoop-training/compile/5bf0d322df2103b5f75095a2f2eac108/movie.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
19/05/14 23:12:38 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-training/compile/5bf0d322df2103b5f75095a2f2eac108/movie.jar
19/05/14 23:12:38 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/05/14 23:12:38 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/05/14 23:12:38 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/05/14 23:12:38 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/05/14 23:12:38 INFO mapreduce.ImportJobBase: Beginning import of movie
19/05/14 23:12:39 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/05/14 23:12:39 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/05/14 23:12:39 INFO client.RMProxy: Connecting to ResourceManager at horse/172.31.16.221:8032
19/05/14 23:12:41 INFO db.DBInputFormat: Using read commited transaction isolation
19/05/14 23:12:41 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`id`), MAX(`id`) FROM `movie`
19/05/14 23:12:41 INFO db.IntegerSplitter: Split size: 987; Num splits: 4 from: 1 to: 3952
19/05/14 23:12:41 INFO mapreduce.JobSubmitter: number of splits:4
19/05/14 23:12:41 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1557895603363_0002
19/05/14 23:12:42 INFO impl.YarnClientImpl: Submitted application application_1557895603363_0002
19/05/14 23:12:42 INFO mapreduce.Job: The url to track the job: http://horse:8088/proxy/application_1557895603363_0002/
19/05/14 23:12:42 INFO mapreduce.Job: Running job: job_1557895603363_0002
19/05/14 23:12:49 INFO mapreduce.Job: Job job_1557895603363_0002 running in uber mode : false
19/05/14 23:12:49 INFO mapreduce.Job:  map 0% reduce 0%
19/05/14 23:12:55 INFO mapreduce.Job:  map 75% reduce 0%
19/05/14 23:12:59 INFO mapreduce.Job:  map 100% reduce 0%
19/05/14 23:12:59 INFO mapreduce.Job: Job job_1557895603363_0002 completed successfully
19/05/14 23:12:59 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=587128
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=412
		HDFS: Number of bytes written=102052
		HDFS: Number of read operations=16
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=8
	Job Counters 
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=16043
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=16043
		Total vcore-seconds taken by all map tasks=16043
		Total megabyte-seconds taken by all map tasks=16428032
	Map-Reduce Framework
		Map input records=3881
		Map output records=3881
		Input split bytes=412
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=149
		CPU time spent (ms)=3380
		Physical memory (bytes) snapshot=772087808
		Virtual memory (bytes) snapshot=6254284800
		Total committed heap usage (bytes)=704643072
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=102052
19/05/14 23:12:59 INFO mapreduce.ImportJobBase: Transferred 99.6602 KB in 19.8384 seconds (5.0236 KB/sec)
19/05/14 23:12:59 INFO mapreduce.ImportJobBase: Retrieved 3881 records.
training@elephant:~$ hdfs dfs -ls movie
Found 5 items
-rw-r--r--   3 training supergroup          0 2019-05-14 23:12 movie/_SUCCESS
-rw-r--r--   3 training supergroup      24596 2019-05-14 23:12 movie/part-m-00000
-rw-r--r--   3 training supergroup      24740 2019-05-14 23:12 movie/part-m-00001
-rw-r--r--   3 training supergroup      26371 2019-05-14 23:12 movie/part-m-00002
-rw-r--r--   3 training supergroup      26345 2019-05-14 23:12 movie/part-m-00003
training@elephant:~$ hdfs dfs -tail movie/part-m-00000
riday	1940
952	Around the World in 80 Days	1956
953	It's a Wonderful Life	1946
954	Mr. Smith Goes to Washington	1939
955	Bringing Up Baby	1938
956	Penny Serenade	1941
957	Scarlet Letter, The	1926
958	Lady of Burlesque	1943
959	Of Human Bondage	1934
960	Angel on My Shoulder	1946
961	Little Lord Fauntleroy	1936
962	They Made Me a Criminal	1939
963	Inspector General, The	1949
964	Angel and the Badman	1947
965	39 Steps, The	1935
966	Walk in the Sun, A	1945
967	Outlaw, The	1943
968	Night of the Living Dead	1968
969	African Queen, The	1951
970	Beat the Devil	1954
971	Cat on a Hot Tin Roof	1958
972	Last Time I Saw Paris, The	1954
973	Meet John Doe	1941
974	Algiers	1938
975	Something to Sing About	1937
976	Farewell to Arms, A	1932
977	Moonlight Murder	1936
978	Blue Angel, The	0
979	Nothing Personal	1995
980	In the Line of Duty 2	1987
981	Dangerous Ground	1997
982	Picnic	1955
983	Madagascar Skin	1995
984	Pompatus of Love, The	1996
985	Small Wonders	1996
986	Fly Away Home	1996
987	Bliss	1997
988	Grace of My Heart	1996
training@elephant:~$ sqoop import --connect jdbc:mysql://localhost/movielens --table movierating --fields-terminated-by '\t' --username training --password training
Warning: /opt/cloudera/parcels/CDH-5.9.0-1.cdh5.9.0.p0.23/bin/../lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
19/05/14 23:14:31 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.9.0
19/05/14 23:14:31 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/05/14 23:14:31 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
19/05/14 23:14:31 INFO tool.CodeGenTool: Beginning code generation
19/05/14 23:14:31 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `movierating` AS t LIMIT 1
19/05/14 23:14:31 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `movierating` AS t LIMIT 1
19/05/14 23:14:31 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /opt/cloudera/parcels/CDH/lib/hadoop-mapreduce
Note: /tmp/sqoop-training/compile/d96c833a40a3bdc33326201eaaf41955/movierating.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
19/05/14 23:14:33 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-training/compile/d96c833a40a3bdc33326201eaaf41955/movierating.jar
19/05/14 23:14:33 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/05/14 23:14:33 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/05/14 23:14:33 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/05/14 23:14:33 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/05/14 23:14:33 WARN manager.CatalogQueryManager: The table movierating contains a multi-column primary key. Sqoop will default to the column userid only for this job.
19/05/14 23:14:33 WARN manager.CatalogQueryManager: The table movierating contains a multi-column primary key. Sqoop will default to the column userid only for this job.
19/05/14 23:14:33 INFO mapreduce.ImportJobBase: Beginning import of movierating
19/05/14 23:14:33 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/05/14 23:14:34 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/05/14 23:14:34 INFO client.RMProxy: Connecting to ResourceManager at horse/172.31.16.221:8032
19/05/14 23:14:35 WARN hdfs.DFSClient: Caught exception 
java.lang.InterruptedException
	at java.lang.Object.wait(Native Method)
	at java.lang.Thread.join(Thread.java:1281)
	at java.lang.Thread.join(Thread.java:1355)
	at org.apache.hadoop.hdfs.DFSOutputStream$DataStreamer.closeResponder(DFSOutputStream.java:862)
	at org.apache.hadoop.hdfs.DFSOutputStream$DataStreamer.closeInternal(DFSOutputStream.java:830)
	at org.apache.hadoop.hdfs.DFSOutputStream$DataStreamer.run(DFSOutputStream.java:826)
19/05/14 23:14:36 INFO db.DBInputFormat: Using read commited transaction isolation
19/05/14 23:14:36 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`userid`), MAX(`userid`) FROM `movierating`
19/05/14 23:14:36 INFO db.IntegerSplitter: Split size: 1509; Num splits: 4 from: 1 to: 6040
19/05/14 23:14:36 INFO mapreduce.JobSubmitter: number of splits:4
19/05/14 23:14:36 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1557895603363_0003
19/05/14 23:14:36 INFO impl.YarnClientImpl: Submitted application application_1557895603363_0003
19/05/14 23:14:36 INFO mapreduce.Job: The url to track the job: http://horse:8088/proxy/application_1557895603363_0003/
19/05/14 23:14:36 INFO mapreduce.Job: Running job: job_1557895603363_0003
19/05/14 23:14:42 INFO mapreduce.Job: Job job_1557895603363_0003 running in uber mode : false
19/05/14 23:14:42 INFO mapreduce.Job:  map 0% reduce 0%
19/05/14 23:14:50 INFO mapreduce.Job:  map 75% reduce 0%
19/05/14 23:14:56 INFO mapreduce.Job:  map 100% reduce 0%
19/05/14 23:14:56 INFO mapreduce.Job: Job job_1557895603363_0003 completed successfully
19/05/14 23:14:57 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=587300
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=446
		HDFS: Number of bytes written=11553408
		HDFS: Number of read operations=16
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=8
	Job Counters 
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=21260
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=21260
		Total vcore-seconds taken by all map tasks=21260
		Total megabyte-seconds taken by all map tasks=21770240
	Map-Reduce Framework
		Map input records=1000205
		Map output records=1000205
		Input split bytes=446
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=323
		CPU time spent (ms)=12490
		Physical memory (bytes) snapshot=1070559232
		Virtual memory (bytes) snapshot=6270558208
		Total committed heap usage (bytes)=924844032
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=11553408
19/05/14 23:14:57 INFO mapreduce.ImportJobBase: Transferred 11.0182 MB in 22.7561 seconds (495.8068 KB/sec)
19/05/14 23:14:57 INFO mapreduce.ImportJobBase: Retrieved 1000205 records.
training@elephant:~$ hdfs dfs -ls movierating
Found 5 items
-rw-r--r--   3 training supergroup          0 2019-05-14 23:14 movierating/_SUCCESS
-rw-r--r--   3 training supergroup    2767800 2019-05-14 23:14 movierating/part-m-00000
-rw-r--r--   3 training supergroup    2821346 2019-05-14 23:14 movierating/part-m-00001
-rw-r--r--   3 training supergroup    3177995 2019-05-14 23:14 movierating/part-m-00002
-rw-r--r--   3 training supergroup    2786267 2019-05-14 23:14 movierating/part-m-00003
training@elephant:~$ hdfs dfs -ls movierating/part-m-00000
-rw-r--r--   3 training supergroup    2767800 2019-05-14 23:14 movierating/part-m-00000
training@elephant:~$ hdfs dfs -tail movierating/part-m-00000
509	3565	2
1509	3566	3
1509	32	4
1509	356	4
1509	39	4
1509	2916	4
1509	1973	2
1509	1036	4
1509	1037	5
1509	364	1
1509	366	3
1509	45	3
1509	367	4
1509	1042	3
1509	512	4
1509	514	4
1509	50	3
1509	52	4
1509	379	3
1509	2791	3
1509	3596	4
1509	524	4
1509	527	4
1509	3744	1
1509	70	3
1509	71	3
1509	539	2
1509	76	4
1509	3751	2
1509	2012	3
1509	1210	5
1509	3753	2
1509	3755	2
1509	1219	4
1509	543	4
1509	546	3
1509	547	4
1509	2024	4
1509	1222	4
1509	2026	2
1509	2028	5
1509	551	2
1509	1089	4
1509	558	2
1509	3911	4
1509	3916	2
1509	2974	1
1509	1094	3
1509	708	2
1509	1097	4
1509	1240	5
1509	1242	4
1509	3785	3
1509	2046	3
1509	2985	4
1509	2986	2
1510	1249	5
1510	1259	5
1510	3	3
1510	1261	1
1510	1269	4
1510	908	5
1510	920	5
1510	2291	3
1510	2455	4
1510	2456	3
1510	1673	4
1510	3450	4
1510	2657	2
1510	266	4
1510	2858	5
1510	480	5
1510	1327	4
1510	804	1
1510	2160	4
1510	1544	3
1510	891	3
1510	1580	5
1510	110	5
1510	1597	3
1510	144	4
1510	1965	3
1510	1976	1
1510	2792	2
1510	3753	5
1510	2014	3
1510	2023	3
1510	1094	5
1510	3923	3
</code></pre>