# Kafka knownledge
Kafka version: 3.1
## Kafka Fundamental
Apache Kafka is a streaming platform that is free and open-source.
<details>
  <summary>Some features of Kafka</summary>
  <br/>
  
  + High-throughput: Kafka has a built-in patriation system known as a Topic
  + Fault-Tolerant: Kafka is resistant to node/machine failure within a cluster.
  + Durability: As Kafka supports messages replication, so,  messages are never lost. It is one of the reasons behind durability.
  + Scalability: Kafka can be scaled-out, without incurring any downtime on the fly by adding additional nodes.
  
</details>

<details>
  <summary>Kafka Replicaton</summary>
  <br/>
  
  + High-throughput: Kafka has a built-in patriation system known as a Topic
  + Fault-Tolerant: Kafka is resistant to node/machine failure within a cluster.
  + Durability: As Kafka supports messages replication, so,  messages are never lost. It is one of the reasons behind durability.
  + Scalability: Kafka can be scaled-out, without incurring any downtime on the fly by adding additional nodes.
  
</details>

## Zookeeper
ZooKeeper is a centralized service for maintaining configuration information, naming, providing distributed synchronization, and providing group services.

<details>
  <summary>Why is Zookeeper necessary for Apache Kafka?</summary>
  <br/>
  
  Zookeeper has four primary functions
  1. **Controller Election**
  2. **Cluster Membership:** Zookeeper also maintains a list of all the brokers (ex: ISRs, ...)
  3. **Topic Configuration:** ZooKeeper maintains the configuration of all topics, including the list of existing topics, number of partitions for each topic, location of the replicas, configuration overrides for topics, preferred leader node, among other details.
  4. **Access Control Lists:** Access control lists or ACLs for all the topics
  
  + Ref: https://www.cloudkarafka.com/blog/cloudkarafka-what-is-zookeeper.html
  + Ref: https://data-flair.training/blogs/zookeeper-in-kafka/
  
</details>


## Broker

### Controller

<details>
  <summary>What is controller?</summary>
  <br/>
  
  A controller is not too complex — it is a normal broker that simply has additional responsibility. It's responsible for managing the state of all partitions and replicas
  
  For example:
  + When the leader partition fails, the controller is responsible for selecting a new leader replica for the partition.
  + When the ISR set of a partition changes, the controller is responsible for notifying all brokers to update their metadata information
  + When increasing the number of partitions for a topic, the controller is responsible for the reallocation of partitions.

</details>

<details>
  <summary>Controller election</summary>
  <br/>

  When the controller goes down,
  1. Zookeeper informs all the brokers that the controller failed.
  2. All the brokers will apply to be the controller.
  3. The first broker who applies for this position will become the controller.
  
  + Ref: https://hackernoon.com/apache-kafkas-distributed-system-firefighter-the-controller-broker-1afca1eae302
  + Ref: https://developpaper.com/kafka-controller-election-principle/
  + Ref: https://cwiki.apache.org/confluence/display/KAFKA/KIP-631%3A+The+Quorum-based+Kafka+Controller
  
</details>

### Fault tolerance

Fault tolerance in Kafka is done by copying the partition data to other brokers which are known as **replicas**. Its also called a _replication factor_.

Each broker will hold one or more partitions. And each of these partitions can either be a **replica** or **leader** for the topic. All the writes and reads to a topic go through the **leader** and the **leader** coordinates to update replicas with new data.

<details>
  <summary>Leader, follower, ISRs</summary>
  <br/>

  + **Leader partition:** A partition in the topic and is elected as leader. The leader partition responsible for reading/writing data
  + **Follower partition:** A replica of leader on other brokers.
  + **ISRs(in-sync replica):** the replicated partitions (followers) that are in sync with its leader.
  
</details>

<details>
  <summary>Leader partition election</summary>
  <br/>

  When the leader parition goes down:
  1. The Zookeeper informs the Controller.
  2. The controller selects one of the in-sync replicas (ISR) as the leader.
  3. When the broker comes back up, then it will be assigned again as the leader.
  
  + Ref: https://www.confluent.io/blog/hands-free-kafka-replication-a-lesson-in-operational-simplicity/#:~:text=KAFKA%20REPLICATION:%200%20TO%2060%20IN%201%20MINUTE&text=Every%20topic%20partition%20in%20Kafka,in%20the%20presence%20of%20failures.
  + Ref: https://medium.com/@anchan.ashwithabg95/fault-tolerance-in-apache-kafka-d1f0444260cf
</details>

### Quorum


<details>
  <summary>Formula</summary>
  <br/>

  Quorum can be defined with a formula.
  ```
  q = 2n+1
  ```
  
  `q` is the total number of nodes, and `n` is the number of allowed failure nodes.

  For example: if `n` = **2**, quorum size is **5**.
  
  + Ref: https://stackoverflow.com/questions/58761164/in-kafka-ha-why-minimum-number-of-brokers-required-are-3-and-not-2#:~:text=While%20doing%20R%26D%2C%20we%20found,zookeeper%20%26%20kafka%20brokers%20are%203.
</details>

## Producer
### Message Delivery Guarantees
<details>
  <summary>Types of delivery</summary>
  <br/>

  ![](images/types_of_delivery.PNG)
  
  1. At-most once: Message loss is possible if the producer doesn’t retry on failures.
  2. At-least-once: There is no chance of message loss but the message can be duplicated if the producer retries when the message is already persisted.
  3. Exactly-once: Every message is guaranteed to be persisted in Kafka exactly once without any duplicates and data loss even where there is a broker failure or producer retry.
  
  Ref: https://ssudan16.medium.com/exactly-once-processing-in-kafka-explained-66ecc41a8548#:~:text=Exactly%2Donce%3A%20Every%20message%20is,broker%20failure%20or%20producer%20retry.
</details>
<details>
  <summary>Exactly-Once Processing</summary>
  <br/>
  
  There are two points to archive "_Exactly-Once_":
  1. Idempotent Guarantee
  2. Transactional Guarantee
  
  + Ref: https://www.javacodegeeks.com/2020/05/kafka-exactly-once-semantics.html
  + Ref: https://ssudan16.medium.com/exactly-once-processing-in-kafka-explained-66ecc41a8548#:~:text=Exactly%2Donce%3A%20Every%20message%20is,broker%20failure%20or%20producer%20retry.
  + Ref: https://blog.clairvoyantsoft.com/unleash-kafka-producers-architecture-and-internal-working-f33cba6c43aa
  + Ref: https://cwiki.apache.org/confluence/display/KAFKA/KIP-98+-+Exactly+Once+Delivery+and+Transactional+Messaging
</details>
<details>
  <summary>Terminologies</summary>
  <br/>
  
  1. _Producer ID (PID)_
  
  A Unique Identifier assigned to the producer by the broker.

  If `transactional.id` is not specified, a fresh PID is generated every-time on producer initialization. If `transactional.id` is specified,the broker stores mapping of Transactional ID to PID so that it can return the same PID on producer restart.
  
  2. _Epoch Number_
  
  The epoch number is an integer that is used alongside PID to uniquely identify the latest active producer which is only relevant if `transactional.id` is set.
  
  3. _Sequence Number_
  
  The producer maintains Sequence Number for every message per PID and Topic Partition combination. The boroker will reject if it receives a message whoes **Sequence Number** is not exactly one greater than what was stored in the broker.
  
  4. _Control Message_
  
  The two types of Control Messages are `COMMIT` and `ABORT`.
  
  5. _Transaction Coordinator_
  
  Transaction Coordinator maintains a map of `transactional.id` holds the metadata includes: PID, Epoch Number, transaction timeout, last updated time of the transaction, transaction status, list of Topic Partitions
  
  6. _Transaction Log_
  
  __transaction_state topic
  
  + Ref: https://ssudan16.medium.com/exactly-once-processing-in-kafka-explained-66ecc41a8548#:~:text=Exactly%2Donce%3A%20Every%20message%20is,broker%20failure%20or%20producer%20retry.
</details>
<details>
  <summary>Idempotent Guarantee</summary>
  <br/>

  This means that even if the producer attempts to send the same message repeatedly, only one copy of the message will be actually sent to the Kafka cluster.
  
  With **idempotent guarantee**, this ensures _exactly-one_ only in a **single producer session**. _Exactly-one_ is not guaranteed when the producer is restarted.      
  When the producer is restarted, it will get a new `PID` (producer ID).
  
  ![](images/idempotent-producer.png)
  
  ```
  producerProps.put("enable.idempotence", "true");
  producerProps.put("transactional.id", "100");
  ```
  
  + Ref: https://medium.com/@shesh.soft/kafka-idempotent-producer-and-consumer-25c52402ceb9
</details>
<details>
  <summary>Transactional Guarantee</summary>
  <br/>

  **1. Producer**
  
  
  
  **2. Consumer**
  
  + Ref: https://stackoverflow.com/questions/57321763/kafka-producer-idempotence-exactly-once-or-just-producer-transaction-is-enough
  + Ref: https://stackoverflow.com/questions/56156749/how-does-kafka-know-whether-to-roll-forward-or-roll-back-a-transaction
</details>

<details>
  <summary>Guaranteed Message Ordering</summary>
  <br/>

  The `max.in.flight.requests.per.connection` setting can be used to increase throughput by allowing the client to send multiple unacknowledged requests before blocking. However it can be is a risk of message re-ordering occurring when retrying due to errors.
  
  ![](images/message-ordering-loss-sequence.png)
  
</details>


<details>
  <summary>Configuration</summary>
  <br/>

  
</details>

### Partitioning strategies

<details>
  <summary>Types of Strategies</summary>
  <br/>
   
  
  + Ref: https://www.codetd.com/en/article/13051951
  + Ref: https://www.confluent.io/blog/apache-kafka-producer-improvements-sticky-partitioner/
</details>

## Consumer

### Partition assignment strategies

<details>
  <summary>Types of Strategies</summary>
  <br/>
  
  
  + Ref: https://medium.com/streamthoughts/understanding-kafka-partition-assignment-strategies-and-how-to-write-your-own-custom-assignor-ebeda1fc06f3#:~:text=Kafka%20Clients%20provides%20three%20built,%3A%20Range%2C%20RoundRobin%20and%20StickyAssignor.
</details>

### How do consumers work?

<details>
  <summary>Poll and Internal Threads Behavior</summary>
  <br/>
  
  ![](images/consumer-fetch.png)
  
  Ref: https://www.conduktor.io/kafka/kafka-consumer-important-settings-poll-and-internal-threads-behavior
</details>

<details>
  <summary>Some important consumers</summary>
  <br/>
  
  `fetch.min.bytes`
  
  `fetch.max.bytes`
  
  `fetch.max.wait.ms`
  
  `max.partition.fetch.bytes`
  
  `max.poll.records`
  
  `max.poll.interval.ms`
  
  `session.timeout.ms`
  
  `partition.assignment.strategy`
  
  + Ref: https://www.oreilly.com/library/view/kafka-the-definitive/9781491936153/ch04.html#:~:text=fetch.max.wait.ms,amount%20of%20data%20to%20return
  + Ref: https://cwiki.apache.org/confluence/display/KAFKA/KIP-74%3A+Add+Fetch+Response+Size+Limit+in+Bytes
</details>

<details>
  <summary>__consumer_offsets topic</summary>
  <br/>

  Since 0.9v Kafka stores topic offsets on the broker directly instead of relying on Zookeeper.
  
  Offsets in Kafka are stored as messages in a separate topic named `__consumer_offsets` . Each consumer commits a message into the topic at periodic intervals.
  
  The **Consumer Groups** are stored in the `__consumer_offsets` topic. That topic contains both the committed offsets and the groups metadata (group.id, members, generation, leader, ...). Groups are stored using `GroupMetadataMessage` messages (Offsets use `OffsetsMessage`).
  
  _Dump the group metadata:_
  
  ```
  ./bin/kafka-console-consumer.sh \
  --formatter "kafka.coordinator.group.GroupMetadataManager\$GroupMetadataMessageFormatter" \
  --bootstrap-server localhost:9092 \
  --topic __consumer_offsets
  ```
  
  + Ref: https://hackernoon.com/kafka-and-zookeeper-offsets-vvbe3xj7
  + Ref: https://stackoverflow.com/questions/59433201/where-are-consumer-groups-list-stored-in-recent-kafka-version#:~:text=Since%20Kafka%200.10%2C%20the%20list,leader%2C%20...).
    
</details>

### Consumer group

<details>
  <summary>Consumers & consumer groups</summary>
  <br/>
  
  ![](images/consumer-group.png)
  
</details>

## Feature
### Transaction
<details>
  <summary>Why does Kafka producer produce 2 offsets per message in <strong>transaction</strong> mode?</summary>
  <br/>
  
  This is a design of Kafka. When producer publish a message or a batch of messages, it adds a extra message as a commit message to complete a transaction.
  
  For example: 
  + The producer publish 10 messages, and the current offset will be 11.
  + The producer publish 1 message, and current offset will be 2
  
  Ref: https://stackoverflow.com/questions/59152915/spring-kafka-transaction-causes-producer-per-message-offset-increased-by-two#:~:text=The%20offset%20is%20increased%20by,t%20commit%20the%20consuming%20offset.&text=However%20the%20count%20of%20messages,the%20msgs%20from%20topic2%20continuously.
</details>

### Data retention
<details>
  <summary>Types of Cleanup Policies</summary>
  <br/>
  
  + delete
  + compact
  + delete, compact
  
</details>
<details>
  <summary>Setting data retention</summary>
  <br/>
  
  To configure the cleanup policy, please follow the below steps:
  1. Choose cleanup policy
  
  `cleanup.policy`
  
  + **Default:**	delete
  + **Valid Values:**	[compact, delete]
  + **Server Default Property:** log.cleanup.policy
  
  Compact policy:
  
  + 
  
  Ref: https://medium.com/@sunny_81705/kafka-log-retention-and-cleanup-policies-c8d9cb7e09f8
</details>
<details>
  <summary>Kafka tombstone</summary>
  <br/>
  
  
  
  Ref: https://medium.com/@sunny_81705/kafka-log-retention-and-cleanup-policies-c8d9cb7e09f8
</details>

### Dead letter queue
