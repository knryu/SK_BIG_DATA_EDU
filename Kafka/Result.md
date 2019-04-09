1. Open a new gateway terminal window on your Get2Cluster remote desktop and create a Kafka
topic named weblogs that will contain messages representing lines in Loudacre’s web server
logs.

<pre><code>
[training@localhost ~]$ kafka-topics --create \
> --zookeeper localhost:2181 \
> --replication-factor 1 \
> --partitions 1 \
> --topic weblogs
Created topic "weblogs".
</code></pre>

2. Display all Kafka topics to confirm that the new topic you just created is listed:

<pre><code>
[training@localhost ~]$ kafka-topics --list \
> --zookeeper localhost:2181
weblogs
</code></pre>

3. Review the details of the weblogs topic.

<pre><code>
[training@localhost ~]$ kafka-topics --describe weblogs \
> --zookeeper localhost:2181
Topic:weblogs	PartitionCount:1	ReplicationFactor:1	Configs:
	Topic: weblogs	Partition: 0	Leader: 0	Replicas: 0	Isr: 0
</code></pre>  
