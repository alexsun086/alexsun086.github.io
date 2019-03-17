### Start ZooKeeper
```
bin/zookeeper-server-start.sh config/zookeeper.properties
bin/kafka-server-start.sh config/server.properties
```
### Single Node-Single Broker Configuration
Creating a Kafka Topic
```
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 
--partitions 1 --topic topic-name

bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1   
--partitions 1 --topic Hello-Kafka
```

List of Topics
```
bin/kafka-topics.sh --list --zookeeper localhost:2181
```

Start Producer to Send Messages
```
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic topic-name

bin/kafka-console-producer.sh --broker-list localhost:9092 --topic Hello-Kafka
```

Start Consumer to Receive Messages
```
bin/kafka-console-consumer.sh --zookeeper localhost:2181 —topic topic-name 
--from-beginning

bin/kafka-console-consumer.sh --zookeeper localhost:2181 —topic Hello-Kafka 
--from-beginning
```

### Single Node-Multiple Brokers Configuration
Create Multiple Kafka Brokers
config/server-one.properties
```
# The id of the broker. This must be set to a unique integer for each broker.
broker.id=1
# The port the socket server listens on
port=9093
# A comma seperated list of directories under which to store log files
log.dirs=/tmp/kafka-logs-1
```
config/server-two.properties
```
# The id of the broker. This must be set to a unique integer for each broker.
broker.id=2
# The port the socket server listens on
port=9094
# A comma seperated list of directories under which to store log files
log.dirs=/tmp/kafka-logs-2
```
Start Multiple Brokers
```
Broker1
bin/kafka-server-start.sh config/server.properties
Broker2
bin/kafka-server-start.sh config/server-one.properties
Broker3
bin/kafka-server-start.sh config/server-two.properties
```
Creating a Topic
```
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 3 
-partitions 1 --topic topic-name

bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 3 
-partitions 1 --topic Multibrokerapplication
```

The Describe command is used to check which broker is listening on the current created topic as shown below
```
bin/kafka-topics.sh --describe --zookeeper localhost:2181 
--topic Multibrokerappli-cation
```

Start Producer to Send Messages
```
bin/kafka-console-producer.sh --broker-list localhost:9092 
--topic Multibrokerapplication
```

Start Consumer to Receive Messages
```
bin/kafka-console-consumer.sh --zookeeper localhost:2181 
—topic Multibrokerapplica-tion --from-beginning
```

### Basic Topic Operations
Modifying a Topic
```
bin/kafka-topics.sh —zookeeper localhost:2181 --alter --topic topic_name 
--parti-tions count

We have already created a topic “Hello-Kafka” with single partition count and one replica factor. 
Now using “alter” command we have changed the partition count.
bin/kafka-topics.sh --zookeeper localhost:2181 
--alter --topic Hello-kafka --parti-tions 2
```

Deleting a Topic
```
bin/kafka-topics.sh --zookeeper localhost:2181 --delete --topic topic_name
```
