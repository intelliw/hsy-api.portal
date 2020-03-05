
# reporting.device_period 
---

#### reporting.device_period implementation

The `device_period` dataset may be implemented in:

- a Wide-column key-value repository for very high scale and performance.
- a Document database for low cost.

The equivalent schema elements and recommended values for each repository type are shown below.

Wide-column repo.       | Document db.      | Recommended Value
---                     | ---               | ---
instance id             |                   | `reporting`
table name              |                   | `device_period`
column families         | kind              | `<period>_ENERGY`<br>`<device_type>_MONITORING`
row id                  | ancestry          | _`<device_type>#<device_id>#<YYYYMMDDHHmm>`_
<i></i>                 | entity id         | _`<device_id>#<YYYYMMDDHHmm>`_
column qualifier        | property          | _see below_

---

# ENERGY Column families 

The `<period>_ENERGY` columns families store period-aligned energy data aggregated from monitoring data received through the _devices/datasets_ POST API.

### ENERGY Column qualifiers

Column qualifiers / properties for the `<period>_ENERGY` column families / kinds are shown below.

These period columns will provide the data for the Energy API, in a format which is closely matched to the [API response](/docs/api.sundaya.monitored.equipment/0/c/Examples/GET/energy%20GET%20example).

Each row / entity will have a particular period column depending on the `<device_type>` in the row id: 

A period column will be present in the row only if the timestamp component in the row / entity id (`HHmm`) coincides with a period epoch.

- For example a row / entity with an id of **PMS-01-006#202002091500** will have period data for `hour`,`hourminute`, and `hourqtrhour` as the date-time in the id (**1500**) coincides with the epoch (start) of an hour period.

- Similarly a row / entity with an id of **PMS-01-006#202002090000** will have period data for `day`,`dayhour`,`daytimeofday` as well as `hour`,`hourminute`,`hourqtrhour`, as the date-time in the id (**090000**) coincides with the start of a day and hour period.

At least one `minute` entry will be present in every row.

Period              | Child Periods
---                 | ---            
`minute`            | 
`qtrhour`           | `qtrhourminute`
`hour`              | `hourminute`, `hourqtrhour`
`timeofday`         | `timeofdayhour`
`day`               | `dayhour`,`daytimeofday`
`week`              | `weekday`
`month`             | `monthday`,`monthweek`
`quarter`           | `quartermonth`
`year`              | `yearmonth`,`yearquarter`
`fiveyear`          | `fiveyearyear`

---

# MONITORING Column families 

The `MONITORING` columns families contain replicas of data stored in the `analytics` repository, which is the raw monitoring data received through the _devices/datasets_ POST API. 

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
`pms_id`<br>`pack_id`<br>`pack_volts`<br>`pack_amps`<br>`pack_watts`<br>`pack_vcl`<br>`pack_vch`<br>`pack_dock`<br>`pack_temp_top`<br>`pack_temp_mid`<br>`pack_temp_bottom`<br><br>`sender`<br>`time_zone`<br>`time_processing`<br><br>`dataitem` | `mppt_id`<br><br><br><br><br><br><br><br><br><br><br><br><br>`sender`<br>`time_zone`<br>`time_processing`<br><br>`dataitem` | `inverter_id`<br><br><br><br><br><br><br><br><br><br><br><br><br>`sender`<br>`time_zone`<br>`time_processing`<br><br>`dataitem`

`time_event` is stored for each data element in the cell _timestamp_ property.  

The `dataitem` column contains a complete monitoring message including the cell _timestamp_, as described in the `analytics` dataset pages.

- _[pms_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/pms_monitoring)_
- _[mppt_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/mppt_monitoring)_
- _[inverters_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/inverter_monitoring)_


--- 