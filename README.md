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
