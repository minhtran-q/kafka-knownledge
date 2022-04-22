# Kafka knownledge
Kafka version: 3.1
## Kafka
## Producer
## Consumer
<details>
  <summary>Poll and Internal Threads Behavior</summary>
  <br/>
  
  
  Ref: https://www.conduktor.io/kafka/kafka-consumer-important-settings-poll-and-internal-threads-behavior
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
