# reporting.device_monitoring
---

### Schema

The `DEVICE_MONITORING` dataset contains a replica of data stored in the `analytics` repository, from the _devices/datasets_ API. 

The data is stored as a column family in the `reporting.period` table.

Schema elements and recommended values are shown below.

Element             | Recommended Value
---                 | ---
instance id         | `reporting`
table name          | `period`
column family       | `DEVICE_MONITORING`
row id              | _`<device_type>#<device_id>#<YYYYMMDDHHmm>`_


### Column qualifiers

Column qualifiers / properties for the `DEVICE_MONITORING` column family / kind are shown below, based on each <device_type>: 

PMS             | MPPT              | Inverter       
---             | ---               | ---   
`pms_id`<br>`pack_id`<br>`pack_volts`<br>`pack_amps`<br>`pack_watts`<br>`pack_vcl`<br>`pack_vch`<br>`pack_dock`<br>`pack_temp_top`<br>`pack_temp_mid`<br>`pack_temp_bottom`<br><br>`sender`<br>`time_zone`<br>`time_processing`<br><br>`dataitem` | `mppt_id`<br><br><br><br><br>`sender`<br>`time_zone`<br>`time_processing`<br><br>`dataitem` | `inverter_id`<br><br><br><br><br>`sender`<br>`time_zone`<br>`time_processing`<br><br>`dataitem`

`time_event` is stored for each data element in the cell _timestamp_ property.  

The `dataitem` column contains a complete monitoring message including the cell _timestamp_, as described in the `analytics` dataset pages.

- _[pms_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/pms_monitoring)_
- _[mppt_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/mppt_monitoring)_
- _[inverters_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/inverter_monitoring)_