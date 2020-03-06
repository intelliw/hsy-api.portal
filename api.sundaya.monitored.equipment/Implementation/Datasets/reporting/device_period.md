
# reporting.device_period 
---

The **device_period** dataset contains period-aligned tranformations of streaming monitoring data received through the _devices/datasets_ POST API.

The dataset consists of the following content:

- **ENERGY** - period-aligned energy data aggregates.

- **MONITORING** - raw monitoring data - the same data as in the `analytics` repository, but replicated here in `reporting` repository for fast OLTP access.

Storage for this dataset may be provided by either of the following technologies:

- a Wide-column key-value repository for very high scale and performance.
- a Document database for low cost.

The equivalent schema elements and recommended value for the above repository types are shown below.

Wide-column repo.       | Document db.      | Recommended Value
---                     | ---               | ---
Instance ID             |                   | `reporting`
Table name              |                   | `device_period`
Column Families         | Kind              | _`<period>_ENERGY`<br>`MONITORING`_
Row Id                  | Ancestry          | _`<device_type>#<device_id>#<YYYYMMDDHHmm>`_
<i></i>                 | Entity ID         | _`<device_id>#<YYYYMMDDHHmm>`_
Column Qualifier        | Property          | _see below_


---

# ENERGY data

### Column families

ENERGY _column families_ are named according to the format `<period>_ENERGY`, where the `<period>` prefix refers to a canonical period name.

For example: `HOUR_ENERGY`.

A particular period _column family_ will be present in a row only if the date-time component of the row / entity id (_YYYYMMDDHHmm_) coincides with a period epoch (the start of the period).

- For example a row / entity with an id of **PMS-01-006#202002091500** will have _column families_ for `HOUR` and `MINUTE` as the date-time in the id (**1500**) coincides with the epoch (start) of an hour and minute period.

- Similarly a row / entity with an id of **PMS-01-006#202002090000** will have _column families_ for `DAY`, `HOUR`, and `TIMEOFDAY` as the date-time in the id (**090000**) coincides with the start of all three periods.

- A `MINUTE` _column family_ will be present as every row is scoped to 1 minute.

### Column qualifiers

ENERGY _column qualifiers_ are closely aligned to the [Energy API response](/docs/api.sundaya.monitored.equipment/0/c/Examples/GET/energy%20GET%20example).

The _columnn qualifiers_ are different for each `<device_type>` (in the row id) but are the same across all ENERGY _column families_. 

_column families_ and _column qualifiers_ are summarised in the table below.


Column families   | PMS qualifiers  | MPPT qualifiers   | Inverter qualifiers
---               | ---             | ---               | ---
`MINUTE_ENERGY`<br>`QTRHOUR_ENERGY`<br>`HOUR_ENERGY`<br>`TIMEOFDAY_ENERGY`<br>`DAY_ENERGY`<br>`WEEK_ENERGY`<br>`MONTH_ENERGY`<br>`QUARTER_ENERGY`<br>`YEAR_ENERGY`<br>`FIVEYEAR_ENERGY`               | 2             | 3               | 4



x | `pack_in_joules`<br>`pack_out_joules` | `pv_<4>_joules`<br>`load_<2>_joules` | `pv_<4>_joules`<br>`load_<2>_joules`

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

PMS qualifiers  | MPPT qualifiers   | Inverter qualifiers
---             | ---               | ---   
`pms_id`<br>`pack_id`<br>`pack_volts`<br>`pack_amps`<br>`pack_watts`<br>`pack_vcl`<br>`pack_vch`<br>`pack_dock`<br>`pack_temp_top`<br>`pack_temp_mid`<br>`pack_temp_bottom`<br><br>`sender`<br>`time_zone`<br>`time_processing`<br><br>`dataitem` | `mppt_id`<br><br><br><br><br><br><br><br><br><br><br><br><br>`sender`<br>`time_zone`<br>`time_processing`<br><br>`dataitem` | `inverter_id`<br><br><br><br><br><br><br><br><br><br><br><br><br>`sender`<br>`time_zone`<br>`time_processing`<br><br>`dataitem`

`time_event` is stored for each data element in the cell _timestamp_ property.  

The `dataitem` column contains a complete monitoring message including the cell _timestamp_, as described in the `analytics` dataset pages.

- _[pms_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/pms_monitoring)_
- _[mppt_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/mppt_monitoring)_
- _[inverters_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/inverter_monitoring)_


--- 