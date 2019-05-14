<pre><code>
# Stub code to copy into Spark Shell

import xml.etree.ElementTree as ElementTree

# Optional: Set logging level to WARN to reduce distracting info messages
sc.setLogLevel("WARN")  

# Given a string containing XML, parse the string, and 
# return an iterator of activation XML records (Elements) contained in the string

def getActivations(s):
    filetree = ElementTree.fromstring(s)
    return filetree.getiterator('activation')
    
# Given an activation record (XML Element), return the model name
def getModel(activation):
    return activation.find('model').text 

# Given an activation record (XML Element), return the account number 
def getAccount(activation):
    return activation.find('account-number').text 
</code></pre>

<pre><code>
logfiles="/loudacre/activations/*"
myOutput = sc.textFile(logfiles).map(lambda line: line.split(',')).map(lambda arr: (arr[0],arr[3]+" "+arr[4]))
</code></pre>

<pre><code>
myOutputRDD1 = sc.wholeTextFiles(logfiles)
myOutputRDD2 = myOutputRDD1.flatMap(lambda (fname, s):getActivations(s))
myOutputRDD2.take(2)
</code></pre>

<pre><code>
[<Element 'activation' at 0x7f26900df410>,
 <Element 'activation' at 0x7f26900df6d0>]
</code></pre>

<pre><code>
myOutputRDD3 = myOutputRDD2.map(lambda line:getAccount(line) + ":" + getModel(line))
myOutputRDD3.take(3)
</code></pre>

<pre><code>
['9763:MeeToo 1.0', '426:Titanic 1000', '383:Sorrento F00L']
</code></pre>

<pre><code>
myOutputRDD3.saveAsTextFile("/loudacre/account-models")
logfiles="/loudacre/weblogs/*2.log"
myRDD1 = sc.textFile(logfiles).map(lambda line: (line.split(' ')[2],1))
myRDD1.take(2)
</code></pre>

<pre><code>
[(u'67858', 1), (u'67858', 1)]
</code></pre>

<pre><code>
myRDD2 = myRDD1.reduceByKey(lambda v1,v2:v1+v2)
myRDD2.take(2)
</code></pre>

<pre><code>
[(u'3922', 6), (u'104959', 2)]
</code></pre>

<pre><code>
myRDD3 = myRDD2.map(lambda (userid,count):(count,userid))
myRDD3.take(2)
</code></pre>

<pre><code>
[(6, u'3922'), (2, u'104959')]
</code></pre>

<pre><code>
print myRDD3.countByKey()
</code></pre>

<pre><code>
defaultdict(<type 'int'>, {128: 9, 2: 7239, 3: 36, 4: 4155, 5: 26, 6: 2162, 7: 14, 8: 1409, 9: 14, 10: 878, 11: 12, 12: 549, 13: 7, 14: 308, 15: 8, 16: 155, 17: 4, 146: 8, 19: 2, 20: 41, 21: 1, 22: 17, 150: 11, 152: 11, 132: 12, 154: 5, 27: 1, 156: 5, 158: 6, 160: 8, 162: 5, 164: 3, 134: 9, 166: 3, 168: 4, 170: 3, 172: 6, 174: 2, 24: 6, 176: 2, 136: 11, 178: 1, 188: 1, 138: 6, 190: 1, 130: 10, 140: 14, 142: 8, 86: 1, 144: 7, 100: 1, 104: 1, 106: 1, 18: 76, 110: 4, 112: 1, 116: 1, 118: 4, 120: 5, 148: 2, 122: 4, 124: 7, 126: 5})
</code></pre>

<pre><code>
myRDD4 = sc.textFile(logfiles).map(lambda line: (line.split(' ')[2],line.split(' ')[0]))
myRDD4.take(2)
</code></pre>

<pre><code>
[(u'67858', u'131.166.169.114'), (u'67858', u'131.166.169.114')]
</code></pre>

<pre><code>
myRDD5 = myRDD4.groupByKey()\
    .mapValues(list)
myRDD5.take(10)
</code></pre>

<pre><code>
[(u'3922',
  [u'195.220.211.104',
   u'195.220.211.104',
   u'138.217.174.182',
   u'138.217.174.182',
   u'138.217.174.182',
   u'138.217.174.182']),
 (u'104959', [u'183.123.205.115', u'183.123.205.115']),
 (u'90396', [u'191.120.254.24', u'191.120.254.24']),
 (u'62733',
  [u'93.120.232.94', u'93.120.232.94', u'92.75.142.64', u'92.75.142.64']),
 (u'30390', [u'235.242.157.100', u'235.242.157.100']),
 (u'84780',
  [u'148.5.198.57',
   u'148.5.198.57',
   u'148.5.198.57',
   u'148.5.198.57',
   u'240.70.72.108',
   u'240.70.72.108']),
 (u'54217',
  [u'236.59.12.138',
   u'236.59.12.138',
   u'121.125.136.169',
   u'121.125.136.169',
   u'122.72.182.201',
   u'122.72.182.201',
   u'85.209.207.112',
   u'85.209.207.112',
   u'212.95.104.25',
   u'212.95.104.25',
   u'212.95.104.25',
   u'212.95.104.25',
   u'5.82.216.41',
   u'5.82.216.41',
   u'5.82.216.41',
   u'5.82.216.41',
   u'218.169.205.19',
   u'218.169.205.19',
   u'218.169.205.19',
   u'218.169.205.19']),
 (u'60986',
  [u'81.217.213.96',
   u'81.217.213.96',
   u'44.89.72.134',
   u'44.89.72.134',
   u'249.13.225.46',
   u'249.13.225.46',
   u'249.13.225.46',
   u'249.13.225.46',
   u'88.110.41.147',
   u'88.110.41.147']),
 (u'44490', [u'83.100.72.186', u'83.100.72.186']),
 (u'54604',
  [u'159.112.48.88',
   u'159.112.48.88',
   u'110.199.28.152',
   u'110.199.28.152',
   u'197.162.156.152',
   u'197.162.156.152',
   u'41.69.4.131',
   u'41.69.4.131',
   u'144.133.38.146',
   u'144.133.38.146',
   u'144.133.38.146',
   u'144.133.38.146'])]
</code></pre>

<pre><code>
logfiles2="/loudacre/accounts/*"
myRDD6 = sc.textFile(logfiles2).map(lambda line: (line.split(',')[0],line))
myRDD6.take(3)
</code></pre>

<pre><code>
[(u'1',
  u'1,2008-10-23 16:05:05.0,\\N,Donald,Becton,2275 Washburn Street,Oakland,CA,94660,5100032418,2014-03-18 13:29:47.0,2014-03-18 13:29:47.0'),
 (u'2',
  u'2,2008-11-12 03:00:01.0,\\N,Donna,Jones,3885 Elliott Street,San Francisco,CA,94171,4150835799,2014-03-18 13:29:47.0,2014-03-18 13:29:47.0'),
 (u'3',
  u'3,2008-12-21 09:19:50.0,\\N,Dorthy,Chalmers,4073 Whaley Lane,San Mateo,CA,94479,6506877757,2014-03-18 13:29:47.0,2014-03-18 13:29:47.0')]
</code></pre>

<pre><code>
myRDD7 = myRDD6.join(myRDD2)
myRDD7.take(3)
</code></pre>

<pre><code>
[(u'89371',
  (u'89371,2013-09-08 02:21:15.0,2014-01-19 12:17:06.0,Ricky,Pope,4535 Highland Drive,Portland,OR,97212,5033136196,2014-03-18 13:32:36.0,2014-03-18 13:32:36.0',
   4)),
 (u'99996',
  (u'99996,2013-03-14 19:19:45.0,2014-02-07 16:32:29.0,Garrett,Allen,495 Wilson Street,Prescott,AZ,86360,9280545713,2014-03-18 13:32:56.0,2014-03-18 13:32:56.0',
   2)),
 (u'69171',
  (u'69171,2012-10-03 22:42:16.0,2013-11-06 06:34:53.0,Richard,Tarver,1714 Melm Street,Santa Ana,CA,92718,6574989054,2014-03-18 13:32:00.0,2014-03-18 13:32:00.0',
   6))]
</code></pre>

<pre><code>
myRDD8 = myRDD7.map(lambda (userid, (value1,value2)): \
                    userid + "  " + str(value2) + "  " + value1.split(',')[3] + "  " + \
                    value1.split(',')[4] )
myRDD8.take(10)
</code></pre>

<pre><code>
[u'89371  4  Ricky  Pope',
 u'99996  2  Garrett  Allen',
 u'69171  6  Richard  Tarver',
 u'90311  2  David  Rosenberg',
 u'36848  6  Aaron  Hutson',
 u'83612  2  Richard  Brown',
 u'50672  2  Mark  Cowan',
 u'2302  2  Larry  Larson',
 u'172  126  Lois  Owens',
 u'48225  6  Colleen  Jackson']
</code></pre>
