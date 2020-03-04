# Write Coalescing
---

Write coalescing is used with compression techniques to simplify device logic and to reduce write traffic from edge to cloud, which is typically bandwidth-constrained.

    
### Devices 

Devices will sample and transmit single datapoints to the edge cloud through a simple recurring schedule. 

- the schedule interval can be configured for each datapoint depending on its sensitivity.

- each datapoint represents a single data element in the complete API message schema.

No further logic should be implemented on-device. This makes it possible and easy to implement dynamic logic for traffic optimisation in the edge cloud. 


### Edge-cloud 

The edge-cloud endpoint will limit the received messages to 'change data capture'.

- which means if a value has changed it is stored in the transmit buffer and if it has not changed it is discarded.

- a change occurs when a value exceeds an allowed tolerance compared to its previous changed/stored value.

- change tolerances can be set for each datapoint, as a percentage or an absolute quantity.

        
Changed values are stored for later transmission in a 'incremental change message' transmit queue.

- multiple incremental change messages are combined and written once to the Cloud API.

- each incremental change message has a dedicated sub path in the Cloud API endpoint.

- incremental change message transmission can be configured to be triggered by number of changes, or time interval.


When the edge-cloud stores a change it also updates that element in its 'complete message' singleton.  

- the edge-cloud host overlays the changed value onto a complete message persisted as a singleton.

- the singleton structure contains all elements and complies with the message schema specified for the parent API endpoint.

- the complete message is timestamped and sent to the cloud as a heartbeat message.

- a heartbeat message is sent to the parent API endpoint before combined incremental change `messages are sent to each sub path. 


### Cloud endpoint       

The Cloud API endpoint provides a sub path for each incremental change message.

The endpoint host combines the incremental change message with the hearbeat message to form a complete message.

- one of each incremental change message type (if present) is combined with a heartbeat message in a time window.

- the time window is defined by the set of unique incremental change messages (one of each type) and the first heartbeat received (in event time) within that window.

- multiple complete message will be produced if multiple incremental change messages of a given type existed in a time window.

- the combined messages are replayed to the cloud API host and stored as complete time-series data records.

---
