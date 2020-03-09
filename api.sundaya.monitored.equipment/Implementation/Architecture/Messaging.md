# Messaging
---

The messsaging platform leverages a message flow framework which is implemented by all platform services, and provides access to multiple broker technologies both in the cloud and the edge.

---

### Messaging framework

![Message brokers](/images/message-broker.png)

Message flows are processed through the following sequence of interactions.

1. **Consumer** - instantiates a **Subscriber** and provides a _listen_ endpoint for it to callback whenever there is an event.

2. **Subscriber** - receives messaging events through the Message Broker queue or API and invokes the **Consumer** callback _listen_ endpoint. 

3. **Consumer** - will _validate_ and _analyse_ the incoming message, then invoke the _produce_ method on the **Producer**.

4. **Producer** - will _transform_ the message into the application-specified format for the target queue, then it calls the _write_ method on the **Storage** class. Optionally it can also call _publish_ to forward the transformed message to a new **Publisher**.

5. **Storage** - will normalise the message into the format required by the repository, and call a client library to deliver it to the Repository. 

6. **Publisher** - if the **Producer**  had optionally published the mesage (in step 4) the **Publisher** will similarly use its client library to deliver the transformed message to a new topic in the Message Broker.

---

### Producers/Consumers

The messaging framework provides a bridge for datasets to be produced or consumed through multiple brokers and storage repositories.

The topics and targets are summarised below. 

- `Producer` and `Consumer` classes provide wrappers for each different **Dataset**.

- **Features** propogate 'feature flag' changes to each configurable service.

- **Monitoring** streams device monitoring messages through the API host to the `analytics` repository.


Service                    | Source                   | Topic           | Subscription  
---                        | ---                      | ---             | ---
**Monitoring**     | [dataset/pms&nbsp;POST](/docs/api.sundaya.monitored.equipment/0/routes/devices/dataset/pms/post)<br>[dataset/mppt&nbsp;POST](/docs/api.sundaya.monitored.equipment/0/routes/devices/dataset/mppt/post)<br>[dataset/inverter&nbsp;POST](/docs/api.sundaya.monitored.equipment/0/routes/devices/dataset/inverter/post) | `monitoring.pms`<br>`monitoring.mppt`<br>`monitoring.inverter` | [analytics.pms_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/pms_monitoring)<br>[analytics.mppt_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/mppt_monitoring)<br>[analytics.inverter_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/inverter_monitoring) 
**Features**         | [api/features&nbsp;GET](/docs/api.sundaya.monitored.equipment/0/routes/api/features/get) | `system.feature` | `env.active.features` 


#### Publishers/Subscribers

The messaging framework supports multiple brokers and storage repositories for the cloud and edge as shown in the following table.

A particular message broker can be made exclusively active through a service configuration change (and corresponding deployment of the broker). 

The choice of broker and repository will usually depend on hosting costs and messaging volumes. As expected these requirements and the choice of broker will change in each environment as the platform evolves and the number of connected devices increases.

Service              | Class                         | Cloud                          | Edge                  
---                  | ---                           | ---                            | ---                   
**Message Broker**   | `Publisher`,<br>`Subscriber`   | `NATS`, `PubSub`,<br>`Kafka`  | `KubeMQ`,<br>`Redis`  
**Repository**       | `Storage`                     | `BigQuery`, `GCS`,<br>`Bigtable`, `Datastore` | `Redis`,<br>`Bitsy` | 

---


#### Kafka

Kafka is implemented in a multi-node cluster. The standard cluster is configured for high availability with  3 masters and N workers: 

- Worker nodes run __Kafka__ on port `9092`. 
- Master nodes run __Zookeeper__ on port `2181`, and do _not_ run brokers.

Topics are divided into `N` __partitions__ and have `f+1` __replicas__. 

The cluster is able to tolerate `f` failures with `f+1` replicas. 

Partition size will be reconfigured during operation based on the number of consumer groups; and message volumes and cardinality of keys used for producing messages.

The __default__ configuration has 3 partitions and 2 replicas per topic.

The following __topics__ are created at startup.

      `monitoring.pms:3:2`, `monitoring.mppt:3:2`, `monitoring.inverter:3:2`

- **kafka publishers** - With Kafka as the message broker the platform's publishers are Kafka producers. These are implemented with KafkaJS. 

   The producers require __acknowledgement__ from the leader only, acknowledgement from replicas is not required. 

   The default max number of __retries__ per call is 8.

   Each node in the producer cluster is given a unique __client id__, suffixed with a 4 digit random number:

      e.g. `producer.<nnnn>`

   The message key, which determines the partition, is the dataset id. 

      e.g. `pms.id`, `mppt.id` or `inverter.id`

- **kafka subscribers** With Kafka as the message broker the platform's Subscribers are actually Kafka consumers. These are implemented by KafkaJS. 

   The number of nodes in the consumer cluster must be less than or equal to the number of topic partitions.

   Each node in the consumer cluster is given a unique __client id__, suffixed with a 4 digit random number.

      e.g. `consumer.<nnnn>`

   Each node has multiple consumer listeners, one for each of the folowing monitoring __topics__.

      e.g. `monitoring.pms`, `monitoring.mppt`, `monitoring.inverter`

   Consumer listeners on all nodes share one of the following __group ids__, depending on which topic they listen to:

      e.g. `group.monitoring.pms`, `group.monitoring.mppt`, `group.monitoring.inverter`

---
