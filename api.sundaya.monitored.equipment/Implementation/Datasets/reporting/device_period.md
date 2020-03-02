# reporting.device_period

---

# Schema

Bigtable            | Datastore         | Value / Example         
---                 | ---               | ---
instance id         |                   | `reporting`
table name          |                   | `device_period`
column family       | kind              | `ENERGY_PERIOD`<br>`DEVICE_MONITORING`
row id              | ancestry          | `<device_type>#<device_id>#<YYYYMMDDHHMM>`
<i></i>             | entity id         | `<device_id>#<YYYYMMDDHHMM>`
column qualifier    | property          | _(shown below)_


# Column qualifier/ Property
### DEVICE_MONITORING 

Column qualifiers / properties for the `DEVICE_MONITORING` column family / kind are shown below, based on each <device_type>: 

PMS             | MPPT              | Inverter       
---             | ---               | ---   
`pms_id`<br>`pack_id`<br>`pack{}`<br>`cell[{}]`<br>`fet[{}]`<br>`status{}`<br>`sender`<br> `time_event`<br>`time_zone`<br>`time_processing` |
`mppt_id`<br>`pv[{}]`<br>`battery{}`<br>`load[{}]`<br>`status{}`<br>`sender`<br> `time_event`<br>`time_zone`<br>`time_processing` | 
`inverter_id`<br>`pv{}`<br>`battery{}`<br>`load[{}]`<br>`grid[{}]`<br>`status`<br>`sender`<br>`time_event`<br>`time_zone`<br>`time_processing`

### ENERGY_PERIOD 

Column qualifiers / properties for the `ENERGY_PERIOD` column family / kind are shown below: 

Each row / entity will have a period entry depending on the row / entity id: 

The period column will be present in the row only if the timestamp in the row / entity id coincides with a period epoch.

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
