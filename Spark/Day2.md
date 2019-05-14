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