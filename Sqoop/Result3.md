

3.
Finally save in /loudacre/accounts/CA only clients whose state is from California. Save the file in avro format and compressed using snappy. From the terminal, display some of the records that you just imported. Take a screenshot and save it as CA_only.

<pre><code>
[training@localhost scripts]$ sqoop import --connect jdbc:mysql://localhost/loudacre --username training --password training --table accounts --columns "acct_num,first_name,last_name" --target-dir /loudacre/accounts/CA --fields-terminated-by "\t" --as-avrodatafile --compression-codec org.apache.hadoop.io.compress.SnappyCodec --where " state='CA'"
19/04/07 23:31:49 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/04/07 23:31:49 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/04/07 23:31:49 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
19/04/07 23:31:49 INFO tool.CodeGenTool: Beginning code generation
19/04/07 23:31:49 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 23:31:49 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 23:31:49 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-training/compile/f8554b65b91d1fba7fba7dcc586f4596/accounts.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
19/04/07 23:31:52 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-training/compile/f8554b65b91d1fba7fba7dcc586f4596/accounts.jar
19/04/07 23:31:52 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/04/07 23:31:52 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/04/07 23:31:52 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/04/07 23:31:52 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/04/07 23:31:52 INFO mapreduce.ImportJobBase: Beginning import of accounts
19/04/07 23:31:52 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
19/04/07 23:31:52 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/04/07 23:31:53 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 23:31:53 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 23:31:53 INFO mapreduce.DataDrivenImportJob: Writing Avro schema file: /tmp/sqoop-training/compile/f8554b65b91d1fba7fba7dcc586f4596/accounts.avsc
19/04/07 23:31:53 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/04/07 23:31:53 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
19/04/07 23:31:55 INFO db.DBInputFormat: Using read commited transaction isolation
19/04/07 23:31:55 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`acct_num`), MAX(`acct_num`) FROM `accounts` WHERE (  state='CA' )
19/04/07 23:31:55 INFO db.IntegerSplitter: Split size: 32439; Num splits: 4 from: 1 to: 129760
19/04/07 23:31:55 INFO mapreduce.JobSubmitter: number of splits:4
19/04/07 23:31:55 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1554688341612_0008
19/04/07 23:31:56 INFO impl.YarnClientImpl: Submitted application application_1554688341612_0008
19/04/07 23:31:56 INFO mapreduce.Job: The url to track the job: http://localhost:8088/proxy/application_1554688341612_0008/
19/04/07 23:31:56 INFO mapreduce.Job: Running job: job_1554688341612_0008
19/04/07 23:32:04 INFO mapreduce.Job: Job job_1554688341612_0008 running in uber mode : false
19/04/07 23:32:04 INFO mapreduce.Job:  map 0% reduce 0%
19/04/07 23:32:11 INFO mapreduce.Job:  map 25% reduce 0%
19/04/07 23:32:17 INFO mapreduce.Job:  map 50% reduce 0%
19/04/07 23:32:23 INFO mapreduce.Job:  map 75% reduce 0%
19/04/07 23:32:29 INFO mapreduce.Job:  map 100% reduce 0%
19/04/07 23:32:30 INFO mapreduce.Job: Job job_1554688341612_0008 completed successfully
19/04/07 23:32:31 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=563560
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=470
		HDFS: Number of bytes written=1350990
		HDFS: Number of read operations=16
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=8
	Job Counters
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=0
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=18433
		Total vcore-seconds taken by all map tasks=18433
		Total megabyte-seconds taken by all map tasks=4718848
	Map-Reduce Framework
		Map input records=92416
		Map output records=92416
		Input split bytes=470
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=283
		CPU time spent (ms)=6860
		Physical memory (bytes) snapshot=619745280
		Virtual memory (bytes) snapshot=8289038336
		Total committed heap usage (bytes)=251920384
	File Input Format Counters
		Bytes Read=0
	File Output Format Counters
		Bytes Written=1350990
19/04/07 23:32:31 INFO mapreduce.ImportJobBase: Transferred 1.2884 MB in 37.5201 seconds (35.1632 KB/sec)
19/04/07 23:32:31 INFO mapreduce.ImportJobBase: Retrieved 92416 records.

[training@localhost scripts]$ avro-tools tojson hdfs://localhost/loudacre/accounts/CA/part-m-00000.avro
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
{"acct_num":{"int":1},"first_name":{"string":"Donald"},"last_name":{"string":"Becton"}}
{"acct_num":{"int":2},"first_name":{"string":"Donna"},"last_name":{"string":"Jones"}}
{"acct_num":{"int":3},"first_name":{"string":"Dorthy"},"last_name":{"string":"Chalmers"}}
{"acct_num":{"int":4},"first_name":{"string":"Leila"},"last_name":{"string":"Spencer"}}
{"acct_num":{"int":5},"first_name":{"string":"Anita"},"last_name":{"string":"Laughlin"}}
{"acct_num":{"int":6},"first_name":{"string":"Stevie"},"last_name":{"string":"Bridge"}}
{"acct_num":{"int":7},"first_name":{"string":"David"},"last_name":{"string":"Eggers"}}
{"acct_num":{"int":8},"first_name":{"string":"Dorothy"},"last_name":{"string":"Koopman"}}
{"acct_num":{"int":9},"first_name":{"string":"Kara"},"last_name":{"string":"Kohl"}}
{"acct_num":{"int":10},"first_name":{"string":"Diane"},"last_name":{"string":"Nelson"}}
{"acct_num":{"int":11},"first_name":{"string":"Robert"},"last_name":{"string":"Fisher"}}
{"acct_num":{"int":12},"first_name":{"string":"Marcia"},"last_name":{"string":"Roberts"}}
{"acct_num":{"int":13},"first_name":{"string":"Andres"},"last_name":{"string":"Cruse"}}
{"acct_num":{"int":14},"first_name":{"string":"Ann"},"last_name":{"string":"Moore"}}
{"acct_num":{"int":15},"first_name":{"string":"Joseph"},"last_name":{"string":"Lackey"}}
{"acct_num":{"int":16},"first_name":{"string":"Sarah"},"last_name":{"string":"Duvall"}}
{"acct_num":{"int":17},"first_name":{"string":"Lucy"},"last_name":{"string":"Corley"}}
{"acct_num":{"int":18},"first_name":{"string":"Roland"},"last_name":{"string":"Crawford"}}
{"acct_num":{"int":19},"first_name":{"string":"Leona"},"last_name":{"string":"Bray"}}
{"acct_num":{"int":20},"first_name":{"string":"Forrest"},"last_name":{"string":"Becker"}}
{"acct_num":{"int":21},"first_name":{"string":"Michele"},"last_name":{"string":"Bellows"}}
{"acct_num":{"int":22},"first_name":{"string":"Brittany"},"last_name":{"string":"Bowie"}}
{"acct_num":{"int":23},"first_name":{"string":"Emily"},"last_name":{"string":"Alsup"}}
{"acct_num":{"int":24},"first_name":{"string":"Gwendolyn"},"last_name":{"string":"Waters"}}
{"acct_num":{"int":25},"first_name":{"string":"Richard"},"last_name":{"string":"Adams"}}
{"acct_num":{"int":26},"first_name":{"string":"Larae"},"last_name":{"string":"Petit"}}
{"acct_num":{"int":27},"first_name":{"string":"Sheri"},"last_name":{"string":"Jacobsen"}}
{"acct_num":{"int":28},"first_name":{"string":"Victoria"},"last_name":{"string":"Jones"}}
{"acct_num":{"int":29},"first_name":{"string":"Lynn"},"last_name":{"string":"Elsner"}}
{"acct_num":{"int":30},"first_name":{"string":"Angela"},"last_name":{"string":"Young"}}
{"acct_num":{"int":31},"first_name":{"string":"Brent"},"last_name":{"string":"Bean"}}
{"acct_num":{"int":32},"first_name":{"string":"Linda"},"last_name":{"string":"Vaught"}}
{"acct_num":{"int":33},"first_name":{"string":"Marcia"},"last_name":{"string":"Prince"}}
{"acct_num":{"int":34},"first_name":{"string":"Lela"},"last_name":{"string":"Polk"}}
{"acct_num":{"int":35},"first_name":{"string":"Peggy"},"last_name":{"string":"Miller"}}
{"acct_num":{"int":36},"first_name":{"string":"Adrienne"},"last_name":{"string":"Woods"}}
{"acct_num":{"int":37},"first_name":{"string":"Cheryl"},"last_name":{"string":"West"}}
{"acct_num":{"int":38},"first_name":{"string":"Sabrina"},"last_name":{"string":"Macklin"}}
{"acct_num":{"int":39},"first_name":{"string":"Leroy"},"last_name":{"string":"Sears"}}
</code></pre>
