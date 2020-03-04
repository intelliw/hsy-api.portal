# reporting.device_monitoring
---

### Schema

The `DEVICE_MONITORING` dataset is stored as a column family in the `reporting.period` table. 

Schema elements and recommended values are shown below.

Element             | Recommended Value
---                 | ---
instance id         | `reporting`
table name          | `period`
column family       | `DEVICE_MONITORING`
row id              | _`<device_type>#<device_id>#<YYYYMMDDHHmm>`_
column qualifier    | _see below_

### Column qualifiers

Column qualifiers / properties for the `DEVICE_MONITORING` column family / kind are shown below, based on each <device_type>: 

PMS             | MPPT              | Inverter       
---             | ---               | ---   
`pms_id`<br>`pack_id`<br>`pack`  _{..}_<br>`cell`  _[ {..} ]_<br>`fet`  _[ {..} ]_<br>`status{}`<br><br>`sender`<br> `time_event`<br>`time_zone`<br>`time_processing` | `mppt_id`<br>`pv`  _[ {..} ]_<br>`battery`  _{..}_<br>`load`  _[ {..} ]_<br>`status`  _{..}_<br><br><br>`sender`<br> `time_event`<br>`time_zone`<br>`time_processing` | `inverter_id`<br>`pv`  _{..}_<br>`battery`  _{..}_<br>`load`  _[ {..} ]_<br>`grid`  _[ {..} ]_<br>`status`<br><br>`sender`<br>`time_event`<br>`time_zone`<br>`time_processing`
 
