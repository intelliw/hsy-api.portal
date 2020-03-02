# reporting.device_period

---

# Schema

The schema for the `device_period` dataset may be implemented in:

- a Wide-column key-value repository for very high scale and performance.
- a Document database for low cost.

The equivalent schema elements and recommended values for each repository type are shown below.

Wide-column repo.       | Document db.      | Recommended Value
---                     | ---               | ---
instance id             |                   | `reporting`
table name              |                   | `device_period`
column family           | kind              | `ENERGY_PERIOD`<br>`DEVICE_MONITORING`
row id                  | ancestry          | _`<device_type>#<device_id>#<YYYYMMDDHHmm>`_
<i></i>                 | entity id         | _`<device_id>#<YYYYMMDDHHmm>`_
column qualifier        | property          | _see below_


### DEVICE_MONITORING Column qualifier/ Property

Column qualifiers / properties for the `DEVICE_MONITORING` column family / kind are shown below, based on each <device_type>: 

PMS             | MPPT              | Inverter       
---             | ---               | ---   
`pms_id`<br>`pack_id`<br>`pack`  _{..}_<br>`cell`  _[ {..} ]_<br>`fet`  _[ {..} ]_<br>`status{}`<br><br>`sender`<br> `time_event`<br>`time_zone`<br>`time_processing` | `mppt_id`<br>`pv`  _[ {..} ]_<br>`battery`  _{..}_<br>`load`  _[ {..} ]_<br>`status`  _{..}_<br><br><br><br>`sender`<br> `time_event`<br>`time_zone`<br>`time_processing` | `inverter_id`<br>`pv`  _{..}_<br>`battery`  _{..}_<br>`load`  _[ {..} ]_<br>`grid`  _[ {..} ]_<br>`status`<br><br>`sender`<br>`time_event`<br>`time_zone`<br>`time_processing`
 
### ENERGY_PERIOD Column qualifier/ Property 

Column qualifiers / properties for the `ENERGY_PERIOD` column family / kind are shown below: 

Each row / entity will have a period entry depending on the row / entity id: 

A particular period column will be present in the row only if the timestamp component in the row / entity id (`HHmm`) coincides with a period epoch.

- For example a row / entity with an id of `PMS-01-006#202002091500` will have period data for `hour`,`hourminute`, and `hourqtrhour` as the timestamp component in the id (`1500`) coiicides with the epoch (start) of an hour period.

- Similarly a row / entity with an id of `PMS-01-006#202002090000` will have period data for `day`,`dayhour`,`daytimeofday` as well as `hour`,`hourminute`,`hourqtrhour`, as the timestamp component in the id (`090000`) coincides with the start of a day and hour period.

At least one `minute` entry will be present in every row.

Period              | Child Periods
---                 | ---            
`minute`            | 
`qtrhour`           | `qtrhourminute`
`hour`              | `hourminute`<br>`hourqtrhour`
`timeofday`         | `timeofdayhour`
`day`               | `dayhour`<br>`daytimeofday`
`week`              | `weekday`
`month`             | `monthday`<br>`monthweek`
`quarter`           | `quartermonth`
`year`              | `yearmonth`<br>`yearquarter`
`fiveyear`          | `fiveyearyear`
