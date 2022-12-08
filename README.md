# Apache Kafka Series - Learn Apache Kafka for Beginners v3

Apache Kafka Series - Learn Apache Kafka for Beginners v3 by Stephane Maarek

## Folder structure

-

## Details

<details open>
  <summary>Click to Contract/Expend</summary>

## Section 01: Kafka Introduction

### 2. Apache Kafka in 5 minutes

#### Why Apache Kafka

- Created by LinkedIn, now Open-source project mainly maintained by Confluent, IBM, Cloudera
- Horizontal scalability:
  - Can scale to 100s of brokers
  - Can scale to millions of messages per second
- High performance (latency of less than 10ms) - real time
- Used by the 2000+ firms, 80% of the Fortune 100

### Use cases

- Messaging system
- Activity Tracking
- Gather metrics from many different locations
- Application logs gathering
- Stream processing (with the Kafka Streams API for example)
- De-coupling of system dependencies
- Integration with Spark, Flink, Storm, Hadoop, and many other Big Data technologies
- Micro-services pub/sub

### For example...

- Netflix
  - uses Kafka to apply recommendations in real-time while you're watching TV shows
- Uber
  - uses Kafka to gather user, taxi, and trip data in real-time to compute and forecast demand, and compute surge pricing in real-time
- LinkedIn
  - uses Kafka to prevent spam, collect user interactions to make better connection recommendations in real time

> Remember that Kafka is only used as a transportation mechanism!

## Section 04: Kafka Theory

### 7. Topics, Partitions and Offsets

### Topics

- Topics: a particular stream of data
- like a table in a database (without all the constraints)
- You can have as many topics as you want
- A topic is identified by its name
- Any kind of message format
- The sequence of messages is called a data stream
- You cannot query topics, instead, use Kafka Producers to send data \
  and Kafka consumers to read the data

#### Partitions and offsets

- Topics are split in partitions (example: 100 partitions)
  - Messages within each partition are ordered
  - Each message with a partition gets an incremental id, called `offset`
- Kafka topics are immutable: once data is written to a partition, it cannot be changed

#### Topics, partitions and offsets - important notes

- Once the data is written to a partition, it cannot be changed (immutability)
- Data is kept only for a limited time (default is one week - configurable)
- Offset only have a meaning for a specific partition
  - E.g. offset 3 in partition 0 doesn't represent the same data as offset 3 in partition 1
  - Offsets are not re-used even if previous messages have been deleted
- Order is guaranteed only within a partition (not across partitions)
- Data is assigned randomly to a partition unless a key is provided (more on this later)

### 8. Producers and Message Keys

#### Producers

- Producers write data to topics (which are made of partitions)
- Producers know to which partition to write to (and which Kafka broker has it)
- In case of Kafka broker failures, Producers will automatically recover

#### Producers: Message keys

- Producers can choose to send a key with the message (string, number, binary, etc...)
- If key=null, data is sent round robin (partition 0, then 1, then 2...)
- If key!=null, then all messages for that key will always go to the same partition (hashing)
- A key is typically sent if you need message ordering for a specific field (eg: truck_id)

#### Kafka Message Serializer

- Kafka only accepts bytes as an input from producers and sends bytes out as an output to cunsumers
- Message Serialization means transforming objects / data into bytes
- They are used on the value and the key
- Common Serializers
  - String (incl. JSON)
  - Int, Float
  - Avro
  - Protobuf

#### For the curious: Kafka Message Key Hashing

- A Kafka partitioner is a code logis that takes a record and determines to which partition to sent it into
- **Key Hashing** is the process of determining the mapping of a key to a partition
- In the default Kafka partitioner, the keys are hashed using the **murmur2 algorithm**, with the formula below for the curious:\
  `targetPartition = Math.abs(Utils.murmur(keyBytes)) % (numPartitions - 1)`

### 9. Consumers & Deserialization

#### Consumers

- Consumers read data from a topic (identified by name) - pull model
- Consumers automatically know which broker to read from
- In case of broker failures, consumers know how to recover
- Data is read in order from low to high offset **with each partitions**

#### Consumers Deserializer

- Deserialize indicates how to transform bytes into objects / data
- They are used on the value and the key of the message
- Common Deserializers
  - String (incl. JSON)
  - Int, Float
  - Avro
  - Protobuf
- The serialization / deserialization type must not change during a topic lifecycle \
  (create a new topic instead)

### 10. Consumer Groups & Consumer Offsets

- All the consumers in an application read data as a consumer groups

> Decided not to write all summaries for this Kafka Theory section. \
> The images in the lecture will help you to understand a lot better than these texts \
> Tepeat them until understood

## Section 05: Starting Kafka

### 20. Mac OS X - Download and Setup Kafka in PATH

[conduktor docs: apach kafka on mac](https://www.conduktor.io/kafka/how-to-install-apache-kafka-on-mac)

1. Install Java JDK version 11
2. Download Apache Kafka from https://kafka.apache.org/downloads under 'Binary Downloads'
3. Extract the contents on your Mac
4. Start Zookeeper using the binaries
   `~/kafka_2.13-3.0.0/bin/zookeeper-server-start.sh ~/kafka_2.13-3.0.0/config/zookeeper.properties`
5. Start Kafka using the binaries in another process
   `~/kafka_2.13-3.0.0/bin/kafka-server-start.sh ~/kafka_2.13-3.0.0/config/server.properties`
6. Setup the $PATH environment variables for easy access to the Kafka binaries
   `PATH="$PATH:/Users/stephanemaarek/kafka_2.13-3.0.0/bin"`

### 22. Mac OS X - Using brew

[conduktor docs: apach kafka on mac with homebrew](https://www.conduktor.io/kafka/how-to-install-apache-kafka-on-mac-with-homebrew)

1. Install Homebrew
2. Run brew install kafka
3. Start Zookeeper using the CLI
   `/usr/local/bin/zookeeper-server-start /usr/local/etc/zookeeper/zoo.cfg`
4. Start Kafka using the CLI in another terminal
   `/usr/local/bin/kafka-server-start /usr/local/etc/kafka/server.properties`

## Section 06: Starting Kafka without Zookeeper

### 31. Mac OS X - Start Kafka in KRaft mode

1. Install Java JDK version 11
2. Download Apache Kafka from https://kafka.apache.org/downloads under 'Binary Downloads'
3. Extract the contents on your Mac
4. Generate a cluster ID and format the storage using kafka-storage.sh
   - `~/kafka_2.13-3.0.0/bin/kafka-storage.sh random-uuid`
   - `~/kafka_2.13-3.0.0/bin/kafka-storage.sh format -t <uuid> -c ~/kafka_2.13-3.0.0/config/kraft/server.properties`
5. Start Kafka using the binaries
   `~/kafka_2.13-3.0.0/bin/kafka-server-start.sh ~/kafka_2.13-3.0.0/config/kraft/server.properties`
6. Setup the $PATH environment variables for easy access to the Kafka binaries

## Section 07: CLI (Command Line Interface) 101

### 34. CLI Introduction

#### commands

- command: `kafka-topics`
  - linux, mac : `kafka-topics.sh`
  - windows : `kafka-topics.bat`
  - homebrew, apt... : `kafka-topics`
- Use the `--bootstrap-server` option everywhere, not `--zookeeper`
  - `kafka-topics --bootstrap-server localhost:9092`

#### For me using [Kafka with docker-compose](https://www.conduktor.io/kafka/how-to-start-kafka-using-docker)

```sh
docker compose up
docker exec -it kafka1 /bin/bash
kafka-topics --version
# 7.3.0-ccs (Commit:b8341813ae2b0444690121942f62c3a125fbf4b3)
```

### 36. Kafka Topics CLI

`kafka-topics`

#### Get in the container

```sh
docker exec -it kafka1 -- bash
```

#### List topics

```sh
# List topics
kafka-topics --bootstrap-server localhost:9092 --list
```

#### Create topics

```sh
# Create a topic
kafka-topics --bootstrap-server localhost:9092 --create --topic first_topic
# WARNING: Due to limitations in metric names, topics with a period ('.') or underscore ('_') could collide. To avoid issues it is best to use either, but not both.
# Created topic first_topic.

# Create second topic
kafka-topics --bootstrap-server localhost:9092 --create --topic second_topic --partitions 3

# Try
kafka-topics --bootstrap-server localhost:9092 --create --topic third_topic --partitions 3 --replication-factor 2
# Error while executing topic command : Replication factor: 2 larger than available brokers: 1.
# [2022-12-06 04:47:13,059] ERROR org.apache.kafka.common.errors.InvalidReplicationFactorException: Replication factor: 2 larger than available brokers: 1.

# Create third topic
kafka-topics --bootstrap-server localhost:9092 --create --topic third_topic --partitions 3 --replication-factor 1
```

> we cannot use `first_topic` and `first.topic` together \
> for `--replication-factor 2`, we need more brokers (kafka servers?)

#### Inspect topics (--describe)

```sh
kafka-topics --bootstrap-server localhost:9092 --describe --topic first_topic
# Topic: first_topic	TopicId: Dr4-iljBQj-QGyp60b-2nw	PartitionCount: 1	ReplicationFactor: 1	Configs:
# 	Topic: first_topic	Partition: 0	Leader: 1	Replicas: 1	Isr: 1

kafka-topics --bootstrap-server localhost:9092 --describe --topic second_topic
# Topic: second_topic	TopicId: PmtGa4ZsTH2tMX77QZMbuw	PartitionCount: 3	ReplicationFactor: 1	Configs:
# 	Topic: second_topic	Partition: 0	Leader: 1	Replicas: 1	Isr: 1
# 	Topic: second_topic	Partition: 1	Leader: 1	Replicas: 1	Isr: 1
# 	Topic: second_topic	Partition: 2	Leader: 1	Replicas: 1	Isr: 1

kafka-topics --bootstrap-server localhost:9092 --describe
```

#### Delete topics

```sh
kafka-topics --bootstrap-server localhost:9092 --delete --topic first_topic
kafka-topics --bootstrap-server localhost:9092 --delete --topic second_topic
kafka-topics --bootstrap-server localhost:9092 --delete --topic third_topic

kafka-topics --bootstrap-server localhost:9092 --list
```

### 37. Kafka Console Producer CLI

`kafka-console-producer`

```sh
kafka-topics --bootstrap-server localhost:9092 --create --topic first_topic --partitions 3

# producing
kafka-console-producer --bootstrap-server localhost:9092 --topic first_topic
>My name is Noah from ANSTO
>I love Kafka

# producing with properties
kafka-console-producer --bootstrap-server localhost:9092 --topic first_topic --producer-property acks=all
>some message that is acked
>just for fun
>fun learning!

# producing to a non existing topic (creating a new topic)
kafka-console-producer --bootstrap-server localhost:9092 --topic new_topic
>hello world!
# [2022-12-06 05:38:29,593] WARN [Producer clientId=console-producer] Error while fetching metadata with correlation id 4 : {new_topic=LEADER_NOT_AVAILABLE} (org.apache.kafka.clients.NetworkClient)
>another message

kafka-topics --bootstrap-server localhost:9092 --describe --topic new_topic

# produce with keys
kafka-console-producer --bootstrap-server localhost:9092 --topic first_topic --property parse.key=true --property key.separator=:
>example key:example value
>name:Noah
>user_id_1234:Noah
>hello world!
# org.apache.kafka.common.KafkaException: No key separator found on line number 4: 'hello world!'
# 	at kafka.tools.ConsoleProducer$LineMessageReader.parse(ConsoleProducer.scala:374)
# 	at kafka.tools.ConsoleProducer$LineMessageReader.readMessage(ConsoleProducer.scala:349)
# 	at kafka.tools.ConsoleProducer$.main(ConsoleProducer.scala:50)
# 	at kafka.tools.ConsoleProducer.main(ConsoleProducer.scala)
```

### 38. Kafka Console Consumer CLI

`kafka-console-consumer`

```sh
# consuming (starting from tail)
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic

# other terminal
kafka-console-producer --bootstrap-server localhost:9092 --topic first_topic
>its working!
>other test

# consuming from beginning
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic --from-beginning
# in order within partition, but not in order in total(?)

# display key, values and timestamp in consumer
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic --formatter kafka.tools.DefaultMessageFormatter --property print.timestamp=true --property print.key=true --property print.value=true --from-beginning
```

### 39. Kafka Consumers in Group

```sh
# start one consumer
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic --group my-first-application

# open other terminal and check it
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic --group my-first-application

# on the other terminal
kafka-console-producer --bootstrap-server localhost:9092 --topic first_topic
# it doesn't do partition balancing

# hmm.. I don't know what it is..
kafka-console-producer --bootstrap-server localhost:9092 --topic first_topic --max-partition-memory-bytes 1

# check on the Q&A
# As Dimistri mentioned, this is the new Producer algorithm, sticky partition. www.confluent.io/ blog/ apache-kafka-producer-improvements-sticky-partitioner/
# As opposed to the old "Round Robin" methodology, Kafka > 2.4 is using sticky partition.
```

### 40. Kafka Consumer Groups CLI

```sh
kafka-consumer-groups --bootstrap-server localhost:9092 --list
# my-first-application
# my-second-application

kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-first-application
# GROUP                TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
# my-first-application first_topic     0          12              12              0               -               -               -
# my-first-application first_topic     1          37              37              0               -               -               -
# my-first-application first_topic     2          22              22              0               -               -               -

kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-second-application
# GROUP                 TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
# my-second-application first_topic     0          0               12              12              -               -               -
# my-second-application first_topic     1          24              37              13              -               -               -
# my-second-application first_topic     2          6               22              16              -               -               -
```

```sh
# run consumer and check the description again (maybe two or three consumers)
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic --group my-first-application
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic --group my-first-application
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic --group my-first-application

# Now it has CONSUMER-ID
kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-first-application
```

```sh
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic --from-beginning
kafka-consumer-groups --bootstrap-server localhost:9092 --list
# console-consumer-25162
# my-first-application
# my-second-application

# console-consumer-25162 is a temporary consumer
```

### 41. Resetting Offsets

```sh
# describe the consumer group
kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-first-application

# change the offset to the beginning
kafka-consumer-groups --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --to-earliest --execute --all-topics

# describe the consumer group
kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-first-application

# consume all
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic --group my-first-application --from-beginning

# describe the consumer group
kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-first-application
```

```sh
# change offset -4
kafka-consumer-groups --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --shift-by -4 --execute --all-topics

# consume them
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic --group my-first-application

# change offset +2
kafka-consumer-groups --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --shift-by -2 --execute --all-topics
```

</details>

```sh
# shortcut
docker exec -it kafka1 bash
alias kt="kafka-topics --bootstrap-server localhost:9092"
```
