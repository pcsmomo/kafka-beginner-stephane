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

</details>
