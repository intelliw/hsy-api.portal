# reporting.energy_period

---

### Schema

The `ENERGY_PERIOD` dataset is stored as a column family in the `reporting.period` table. 

Schema elements and recommended values are shown below.

Element             | Recommended Value
---                 | ---
instance id         | `reporting`
table name          | `period`
column family       | `ENERGY_PERIOD`
row id              | _`<device_type>#<device_id>#<YYYYMMDDHHmm>`_


### Column qualifiers

Column qualifiers / properties for the `ENERGY_PERIOD` column family / kind are shown below.

These period columns will provide the data for the energy API, in a format which is closely matched to the [API response](/docs/api.sundaya.monitored.equipment/0/c/Examples/GET/energy%20GET%20example).

Each row / entity will have a particular period column depending on the row / entity id: 

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
`day`               | `dayhour`, `daytimeofday`
`week`              | `weekday`
`month`             | `monthday`, `monthweek`
`quarter`           | `quartermonth`
`year`              | `yearmonth`, `yearquarter`
`fiveyear`          | `fiveyearyear`
