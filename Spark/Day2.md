<pre><code>
import sys
from pyspark import SparkContext

if __name__ == "__main__":
    if len(sys.argv) < 2:
        print >> sys.stderr, "Usage: CountJPGs.py <logfile>"
        exit(-1)

    sc = SparkContext()

    sc.setLogLevel("ERROR")
    counts =  sc.textFile(sys.argv[1]).filter(lambda line: ".jpg" in line).count()

    print counts

    sc.stop()

    # Replace this line with your code:    
    # print("TODO: complete this exercise")

</code></pre>


<pre><code>
In [1]: accounts=sc.\
   ...: textFile("/loudacre/accounts/part-m-00000")
In [2]: print accounts.toDebugString()
</code></pre>

<pre><code>
(2) /loudacre/accounts/part-m-00000 MapPartitionsRDD[1] at textFile at NativeMethodAccessorImpl.java:-2 []
 |  /loudacre/accounts/part-m-00000 HadoopRDD[0] at textFile at NativeMethodAccessorImpl.java:-2 []
</code></pre>

<pre><code>
In [4]: accounts=sc.\
   ...: textFile("/loudacre/accounts/part-m-00000",3)
In [5]: print accounts.toDebugString()
</code></pre>

<pre><code>
(3) /loudacre/accounts/part-m-00000 MapPartitionsRDD[3] at textFile at NativeMethodAccessorImpl.java:-2 []
 |  /loudacre/accounts/part-m-00000 HadoopRDD[2] at textFile at NativeMethodAccessorImpl.java:-2 []
</code></pre>

<pre><code>
In [6]: accounts=sc.\
   ...: textFile("/loudacre/accounts/*",3)
In [7]: print accounts.toDebugString()
</code></pre>

<pre><code>
(5) /loudacre/accounts/* MapPartitionsRDD[5] at textFile at NativeMethodAccessorImpl.java:-2 []
 |  /loudacre/accounts/* HadoopRDD[4] at textFile at NativeMethodAccessorImpl.java:-2 []
</code></pre>

<pre><code>
In [8]: accountsByID = accounts \
   ...: .map(lambda s: s.split(',')) \
   ...: .map(lambda values: \
   ...: (values[0],values[4] + ',' + values[3]))
In [9]: userReqs = sc\
   ...: .textFile("/loudacre/weblogs/*2.log")\
   ...: .map(lambda line: line.split())\
   ...: .map(lambda words: (words[2],1))\
   ...: .reduceByKey(lambda v1,v2: v1+v2)
In [10]: accountHits = accountsByID.join(userReqs)\
    ...: .values()
In [11]: print accountHits.toDebugString()
</code></pre>

<pre><code>
(23) PythonRDD[19] at RDD at PythonRDD.scala:43 []
 |   MapPartitionsRDD[18] at mapPartitions at PythonRDD.scala:374 []
 |   ShuffledRDD[17] at partitionBy at NativeMethodAccessorImpl.java:-2 []
 +-(23) PairwiseRDD[16] at join at <ipython-input-10-ef6ba18ef7f5>:1 []
    |   PythonRDD[15] at join at <ipython-input-10-ef6ba18ef7f5>:1 []
    |   UnionRDD[14] at union at NativeMethodAccessorImpl.java:-2 []
    |   PythonRDD[12] at RDD at PythonRDD.scala:43 []
    |   /loudacre/accounts/* MapPartitionsRDD[5] at textFile at NativeMethodAccessorImpl.java:-2 []
    |   /loudacre/accounts/* HadoopRDD[4] at textFile at NativeMethodAccessorImpl.java:-2 []
    |   PythonRDD[13] at RDD at PythonRDD.scala:43 []
    |   MapPartitionsRDD[11] at mapPartitions at PythonRDD.scala:374 []
    |   ShuffledRDD[10] at partitionBy at NativeMethodAccessorImpl.java:-2 []
    +-(18) PairwiseRDD[9] at reduceByKey at <ipython-input-9-94c2517c27ca>:1 []
       |   PythonRDD[8] at reduceByKey at <ipython-input-9-94c2517c27ca>:1 []
       |   /loudacre/weblogs/*2.log MapPartitionsRDD[7] at textFile at NativeMethodAccessorImpl.java:-2 []
       |   /loudacre/weblogs/*2.log HadoopRDD[6] at textFile at NativeMethodAccessorImpl.java:-2 []
</code></pre>

<pre><code>
In [12]: accountHits.\
    ...: saveAsTextFile("/loudacre/userreqs")
</code></pre>

<pre><code>
In [4]: userReqs = sc.textFile("/loudacre/weblogs/*2.log").map(lambda line: line.split()).map(lambda words: (words[2],1)).reduceByKey(lambda v1,v2: v1+v2)

In [5]: accounts = sc.textFile("/loudacre/accounts/*").map(lambda s: s.split(',')).map(lambda values: (values[0],values[4] + ',' + values[3]))

In [6]: accountHits=accounts.join(userReqs).map(lambda (userid,values): values)

In [7]: accountHits\
   ...: .filter(lambda (firstlast,hitcount): hitcount > 5)\
   ...: .count()
Out[7]: 5872                                                                    
</code></pre>

<pre><code>
In [8]: accountHits.persist()
Out[8]: PythonRDD[16] at RDD at PythonRDD.scala:43
</code></pre>

<pre><code>
In [9]: accountHits.count()
Out[9]: 17328                                                                   
</code></pre>

<pre><code>
In [10]: accountHits.toDebugString()
Out[10]: '(23) PythonRDD[16] at RDD at PythonRDD.scala:43 [Memory Serialized 1x Replicated]\n |        CachedPartitions: 23; MemorySize: 311.2 KB; ExternalBlockStoreSize: 0.0 B; DiskSize: 0.0 B\n |   MapPartitionsRDD[14] at mapPartitions at PythonRDD.scala:374 [Memory Serialized 1x Replicated]\n |   ShuffledRDD[13] at partitionBy at NativeMethodAccessorImpl.java:-2 [Memory Serialized 1x Replicated]\n +-(23) PairwiseRDD[12] at join at <ipython-input-6-1a97a1a68718>:1 [Memory Serialized 1x Replicated]\n    |   PythonRDD[11] at join at <ipython-input-6-1a97a1a68718>:1 [Memory Serialized 1x Replicated]\n    |   UnionRDD[10] at union at NativeMethodAccessorImpl.java:-2 [Memory Serialized 1x Replicated]\n    |   PythonRDD[8] at RDD at PythonRDD.scala:43 [Memory Serialized 1x Replicated]\n    |   /loudacre/accounts/* MapPartitionsRDD[7] at textFile at NativeMethodAccessorImpl.java:-2 [Memory Serialized 1x Replicated]\n    |   /loudacre/accounts/* HadoopRDD[6] at textFile at NativeMethodAccessorImpl.java:-2 [Memory Serialized 1x Replicated]\n    |   PythonRDD[9] at RDD at PythonRDD.scala:43 [Memory Serialized 1x Replicated]\n    |   MapPartitionsRDD[5] at mapPartitions at PythonRDD.scala:374 [Memory Serialized 1x Replicated]\n    |   ShuffledRDD[4] at partitionBy at NativeMethodAccessorImpl.java:-2 [Memory Serialized 1x Replicated]\n    +-(18) PairwiseRDD[3] at reduceByKey at <ipython-input-4-94c2517c27ca>:1 [Memory Serialized 1x Replicated]\n       |   PythonRDD[2] at reduceByKey at <ipython-input-4-94c2517c27ca>:1 [Memory Serialized 1x Replicated]\n       |   /loudacre/weblogs/*2.log MapPartitionsRDD[1] at textFile at NativeMethodAccessorImpl.java:-2 [Memory Serialized 1x Replicated]\n       |   /loudacre/weblogs/*2.log HadoopRDD[0] at textFile at NativeMethodAccessorImpl.java:-2 [Memory Serialized 1x Replicated]'
</code></pre>

<pre><code>
In [11]: accountHits.unpersist()
Out[11]: PythonRDD[16] at RDD at PythonRDD.scala:43
</code></pre>

<pre><code>
In [12]: from pyspark import StorageLevel
In [13]: accountHits.persist(StorageLevel.DISK_ONLY)
Out[13]: PythonRDD[16] at RDD at PythonRDD.scala:43
</code></pre>

<pre><code>
In [14]: accountHits.count()
Out[14]: 17328                                                                  
</code></pre>

<pre><code>
In [15]: accountHits.toDebugString()
Out[15]: '(23) PythonRDD[16] at RDD at PythonRDD.scala:43 [Disk Serialized 1x Replicated]\n |        CachedPartitions: 23; MemorySize: 0.0 B; ExternalBlockStoreSize: 0.0 B; DiskSize: 311.2 KB\n |   MapPartitionsRDD[14] at mapPartitions at PythonRDD.scala:374 [Disk Serialized 1x Replicated]\n |   ShuffledRDD[13] at partitionBy at NativeMethodAccessorImpl.java:-2 [Disk Serialized 1x Replicated]\n +-(23) PairwiseRDD[12] at join at <ipython-input-6-1a97a1a68718>:1 [Disk Serialized 1x Replicated]\n    |   PythonRDD[11] at join at <ipython-input-6-1a97a1a68718>:1 [Disk Serialized 1x Replicated]\n    |   UnionRDD[10] at union at NativeMethodAccessorImpl.java:-2 [Disk Serialized 1x Replicated]\n    |   PythonRDD[8] at RDD at PythonRDD.scala:43 [Disk Serialized 1x Replicated]\n    |   /loudacre/accounts/* MapPartitionsRDD[7] at textFile at NativeMethodAccessorImpl.java:-2 [Disk Serialized 1x Replicated]\n    |   /loudacre/accounts/* HadoopRDD[6] at textFile at NativeMethodAccessorImpl.java:-2 [Disk Serialized 1x Replicated]\n    |   PythonRDD[9] at RDD at PythonRDD.scala:43 [Disk Serialized 1x Replicated]\n    |   MapPartitionsRDD[5] at mapPartitions at PythonRDD.scala:374 [Disk Serialized 1x Replicated]\n    |   ShuffledRDD[4] at partitionBy at NativeMethodAccessorImpl.java:-2 [Disk Serialized 1x Replicated]\n    +-(18) PairwiseRDD[3] at reduceByKey at <ipython-input-4-94c2517c27ca>:1 [Disk Serialized 1x Replicated]\n       |   PythonRDD[2] at reduceByKey at <ipython-input-4-94c2517c27ca>:1 [Disk Serialized 1x Replicated]\n       |   /loudacre/weblogs/*2.log MapPartitionsRDD[1] at textFile at NativeMethodAccessorImpl.java:-2 [Disk Serialized 1x Replicated]\n       |   /loudacre/weblogs/*2.log HadoopRDD[0] at textFile at NativeMethodAccessorImpl.java:-2 [Disk Serialized 1x Replicated]'
</code></pre>
