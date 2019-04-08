#1.
#From the accounts table, import only the primary key, along with the first name, last name to HDFS directory /loudacre/accounts/user_info. Please save the file in text #format with tab delimiters.


<pre><code>
[training@localhost scripts]$ sqoop eval --connect jdbc:mysql://localhost/loudacre --username training --password training --query "desc accounts"
19/04/07 22:12:34 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/04/07 22:12:34 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/04/07 22:12:34 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
---------------------------------------------------------------------------------------------------------
| Field                | Type                 | Null | Key | Default              | Extra                |
---------------------------------------------------------------------------------------------------------
| acct_num             | int(11)              | NO  | PRI | (null)               |                      |
| acct_create_dt       | datetime             | NO  |     | (null)               |                      |
| acct_close_dt        | datetime             | YES |     | (null)               |                      |
| first_name           | varchar(255)         | NO  |     | (null)               |                      |
| last_name            | varchar(255)         | NO  |     | (null)               |                      |
| address              | varchar(255)         | NO  |     | (null)               |                      |
| city                 | varchar(255)         | NO  |     | (null)               |                      |
| state                | varchar(255)         | NO  |     | (null)               |                      |
| zipcode              | varchar(255)         | NO  |     | (null)               |                      |
| phone_number         | varchar(255)         | NO  |     | (null)               |                      |
| created              | datetime             | NO  |     | (null)               |                      |
| modified             | datetime             | NO  |     | (null)               |                      |
---------------------------------------------------------------------------------------------------------

[training@localhost scripts]$ sqoop import --connect jdbc:mysql://localhost/loudacre --username training --password training --table accounts --columns "acct_num,first_name,last_name" --target-dir /loudacre/accounts/user_info --fields-terminated-by "\t" --as-textfile
19/04/07 22:19:23 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/04/07 22:19:23 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/04/07 22:19:23 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
19/04/07 22:19:23 INFO tool.CodeGenTool: Beginning code generation
19/04/07 22:19:24 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:19:24 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/04/07 22:19:24 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-training/compile/c6412ac9165ac95f892582af2924b83b/accounts.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
19/04/07 22:19:26 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-training/compile/c6412ac9165ac95f892582af2924b83b/accounts.jar
19/04/07 22:19:26 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/04/07 22:19:26 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/04/07 22:19:26 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/04/07 22:19:26 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/04/07 22:19:26 INFO mapreduce.ImportJobBase: Beginning import of accounts
19/04/07 22:19:26 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
19/04/07 22:19:26 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/04/07 22:19:27 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/04/07 22:19:27 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
19/04/07 22:19:29 INFO db.DBInputFormat: Using read commited transaction isolation
19/04/07 22:19:29 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`acct_num`), MAX(`acct_num`) FROM `accounts`
19/04/07 22:19:29 INFO db.IntegerSplitter: Split size: 32440; Num splits: 4 from: 1 to: 129761
19/04/07 22:19:29 INFO mapreduce.JobSubmitter: number of splits:4
19/04/07 22:19:29 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1554688341612_0003
19/04/07 22:19:30 INFO impl.YarnClientImpl: Submitted application application_1554688341612_0003
19/04/07 22:19:30 INFO mapreduce.Job: The url to track the job: http://localhost:8088/proxy/application_1554688341612_0003/
19/04/07 22:19:30 INFO mapreduce.Job: Running job: job_1554688341612_0003
19/04/07 22:19:37 INFO mapreduce.Job: Job job_1554688341612_0003 running in uber mode : false
19/04/07 22:19:37 INFO mapreduce.Job:  map 0% reduce 0%
19/04/07 22:19:43 INFO mapreduce.Job:  map 25% reduce 0%
19/04/07 22:19:48 INFO mapreduce.Job:  map 50% reduce 0%
19/04/07 22:19:53 INFO mapreduce.Job:  map 75% reduce 0%
19/04/07 22:19:58 INFO mapreduce.Job:  map 100% reduce 0%
19/04/07 22:19:58 INFO mapreduce.Job: Job job_1554688341612_0003 completed successfully
19/04/07 22:19:59 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=560464
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=470
		HDFS: Number of bytes written=2615920
		HDFS: Number of read operations=16
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=8
	Job Counters
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=0
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=15233
		Total vcore-seconds taken by all map tasks=15233
		Total megabyte-seconds taken by all map tasks=3899648
	Map-Reduce Framework
		Map input records=129761
		Map output records=129761
		Input split bytes=470
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=209
		CPU time spent (ms)=3500
		Physical memory (bytes) snapshot=498008064
		Virtual memory (bytes) snapshot=8262369280
		Total committed heap usage (bytes)=251920384
	File Input Format Counters
		Bytes Read=0
	File Output Format Counters
		Bytes Written=2615920
19/04/07 22:19:59 INFO mapreduce.ImportJobBase: Transferred 2.4947 MB in 31.6664 seconds (80.6724 KB/sec)
19/04/07 22:19:59 INFO mapreduce.ImportJobBase: Retrieved 129761 records.

[training@localhost scripts]$ hdfs dfs -ls /loudacre/accounts/user_info/
Found 5 items
-rw-rw-rw-   1 training supergroup          0 2019-04-07 22:19 /loudacre/accounts/user_info/_SUCCESS
-rw-rw-rw-   1 training supergroup     638090 2019-04-07 22:19 /loudacre/accounts/user_info/part-m-00000
-rw-rw-rw-   1 training supergroup     649567 2019-04-07 22:19 /loudacre/accounts/user_info/part-m-00001
-rw-rw-rw-   1 training supergroup     649000 2019-04-07 22:19 /loudacre/accounts/user_info/part-m-00002
-rw-rw-rw-   1 training supergroup     679263 2019-04-07 22:19 /loudacre/accounts/user_info/part-m-00003
[training@localhost scripts]$ hdfs dfs -tail /loudacre/accounts/user_info/part-m-00000
lsh
32390	Olga	Lipson
32391	Eddie	Hedrick
32392	Alvin	Phillips
32393	Travis	Ainsworth
32394	Allen	Pruitt
32395	Clay	Sherrill
32396	Janice	Padgett
32397	Lisa	Backes
32398	Albert	Walters
32399	Anna	Rathbone
32400	Gwen	Nelson
32401	Judith	Sullivan
32402	Linda	Davis
32403	Deborah	Felipe
32404	Tami	Ketcham
32405	Suzanne	Alexander
32406	Anita	Byrd
32407	Kenneth	Khalil
32408	Jennifer	Merida
32409	Kevin	Gibson
32410	Sheila	Morgan
32411	Kenny	Conger
32412	Angel	Scoggins
32413	Ida	Castro
32414	Tammy	Buxton
32415	James	Ward
32416	Emma	Carmichael
32417	Carmen	Lane
32418	Andrea	Holt
32419	Catherine	Roberts
32420	Erin	Little
32421	Johnny	Graves
32422	Leonard	Lamm
32423	Dennis	Hodge
32424	Nadine	Frias
32425	Juan	Mendel
32426	Dorothy	Thompson
32427	Tara	Jenkins
32428	Philip	Gamble
32429	Marvin	Potts
32430	Stephanie	Wootton
32431	Judith	Lindgren
32432	Darlene	Gonzales
32433	Nicholas	Monger
32434	Anne	Boyle
32435	Marjorie	Morrison
32436	Helen	Perez
32437	Melissa	Hayes
32438	Violet	Searcy
32439	Eunice	Myers
32440	Robert	Huskey
</code></pre>
