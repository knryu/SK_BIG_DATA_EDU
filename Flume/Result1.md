1. Create a new flume configuration file with the following:

<pre><code>
# exec.conf: A Spooling Directory Source

# Name the components on this agent
agent1.sources = exec-source
agent1.sinks = exec-sink
agent1.channels = exec-channel

# Describe/configure the source
agent1.sources.exec-source.type = netcat
agent1.sources.exec-source.bind = localhost
agent1.sources.exec-source.port = 44444
agent1.sources.exec-source.channels = exec-channel

# Describe the sink
agent1.sinks.exec-sink.type = logger
agent1.sinks.exec-sink.channel = exec-channel

# Use a channel which buffers events in memory
agent1.channels.exec-channel.type = memory
agent1.channels.exec-channel.capacity = 1000
agent1.channels.exec-channel.transactionCapacity = 100
</code></pre>
