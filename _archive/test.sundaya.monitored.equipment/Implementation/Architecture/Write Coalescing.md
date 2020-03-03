# Write Coalescing
---

Write coalescing is used to simplify device logic, and to compress and reduce write traffic from edge to cloud.

    
### Devices 

Devices will sample and transmit single datapoints to the edge cloud through a simple recurring schedule.

- the schedule interval can be configured for each datapoint depending on its sensitivity.

- each datapoint represents a single data element in the complete API message schema.
        

### Edge-cloud 

The edge-cloud endpoint will limit the received messages to 'change data capture'.

- which means if a value has changed it is stored in the transmit buffer, if it has not changed it is discarded.

- a change occurs when a value exceeds an allowed tolerance compared to its previous changed/stored value.

- change tolerances can be set for each datapoint, as a percentage or an absolute quantity.

        
Changed values are stored for later transmission in a 'partial message' transmit queue.

- multiple partial messages are combined and written once to the Cloud API.

- each partial message has a dedicated sub path in the Cloud API endpoint.

- partial message transmission can be configured to be triggered by number of changes, or time interval.


When the edge-cloud stores a change it also updates its complete message singleton.

- it overlays the changed value onto a complete message persisted as a singleton.

- the complete message is timestamped and sent to the cloud as a heartbeat message.

- heartbeat messages are sent at a recurring interval.

### Cloud endpoint       

The Cloud API endpoint provides a sub path for each partial message.

The endpoint host combines the partial message with the hearbeat message to form a complete message.

- one of each partial message type (if present) is combined with a heartbeat message in a time window.

- the time window is defined by the set of unique partial messages (one of each type) and the first heartbeat received (in event time) within that window.

- multiple complete message will be produced if multiple partial messages of a given type existed in a time window.

- the combined messages are replayed to the cloud API host and stored as complete time-series data records.

---
