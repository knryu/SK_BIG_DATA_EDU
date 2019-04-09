3. From another terminal start telnet and connect to port 4444. Start typing and you should see the
results from the other terminal. Provide a screenshot of your results.

Input
<pre><code>
[training@localhost ~]$ telnet localhost 44444
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
111
OK
Hello world!
OK
</code></pre>

Output(Console)
<pre><code>
2019-04-08 18:54:16,702 (SinkRunner-PollingRunner-DefaultSinkProcessor) [INFO - org.apache.flume.sink.LoggerSink.process(LoggerSink.java:94)] Event: { headers:{} body: 31 31 31 0D                                     111. }
2019-04-08 18:54:28,544 (SinkRunner-PollingRunner-DefaultSinkProcessor) [INFO - org.apache.flume.sink.LoggerSink.process(LoggerSink.java:94)] Event: { headers:{} body: 48 65 6C 6C 6F 20 77 6F 72 6C 64 21 0D          Hello world!. }
</code></pre>
