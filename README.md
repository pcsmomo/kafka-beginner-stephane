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

[conduktor docs](https://www.conduktor.io/kafka/how-to-install-apache-kafka-on-mac)

1. Install Java JDK version 11
2. Download Apache Kafka from https://kafka.apache.org/downloads under 'Binary Downloads'
3. Extract the contents on your Mac
4. Start Zookeeper using the binaries
   `~/kafka_2.13-3.0.0/bin/zookeeper-server-start.sh ~/kafka_2.13-3.0.0/config/zookeeper.properties`
5. Start Kafka using the binaries in another process
   `~/kafka_2.13-3.0.0/bin/kafka-server-start.sh ~/kafka_2.13-3.0.0/config/server.properties`
6. Setup the $PATH environment variables for easy access to the Kafka binaries
   `PATH="$PATH:/Users/stephanemaarek/kafka_2.13-3.0.0/bin"`

</details>
