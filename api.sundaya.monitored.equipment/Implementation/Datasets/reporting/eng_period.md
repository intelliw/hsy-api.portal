
# Energy Periods (eng_period)
---

This **eng_period** dataset contains period-aligned tranforms of monitoring data received through the _devices/datasets_ POST API.

The dataset contains the following data:

- **energy** - period-aligned energy data aggregates. 

- **monitoring** - raw monitoring data, the same data stored in the `analytics` OLAP repository, but replicated here in `reporting` for fast OLTP access. Rows in this data subset is aligned to the _instant_ period.




### Repository

Storage for the **Reporting** repository may be provided by one of the following technologies:

- Wide-column key-value repository for high scale and performance.
- Document database for low cost.

The equivalent schema elements and recommended value for the above repository types are shown below.

Wide-column repo.       | Document db.              | Recommended Value
---                     | ---                       | ---
Instance ID             |                           | `reporting`
Table name              |                           | `periods`
Column Families         | Kind                      | _`<period_name>`_
Row Id<br><br>          | Ancestry<br>Entity ID     | _`<device_type>#<device_id>#<YYYYMMDDHHmm>`_<br>_`<device_id>#<YYYYMMDDHHmm>`_
Column Qualifier        | Property                  | _see below_




### Column families and qualifiers

Column familes are based on categorical periods as summarised in the table below. 

The table also lists the type of data contained in each column family.  

Note that there is no `SECOND` column family. 

- This is because the smallest aggregation period for a row is a 1 minute.

Column family   | Column qualifiers-PMS     | Column qualifiers-MPPT    | Column qualifiers-Inverter
---             | ---                       | ---                       | --- 
`INSTANT`<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>  |  `pms_id`<br>`sender`<br>`t_zone`<br>`t_processing`<br><br>`_dataitem`<br><br>`pack_id`<br>`pack_v`<br>`pack_i`<br>`pack_w`<br>`pack_vcl`<br>`pack_vch`<br>`pack_dock`<br>`pack_temp_top`<br>`pack_temp_mid`<br>`pack_temp_bottom` | `mppt_id`<br>`sender`<br>`t_zone`<br>`t_processing`<br><br>`_dataitem`<br><br><br><br><br><br><br><br><br><br><br><br> | `inverter_id`<br>`sender`<br>`t_zone`<br>`t_processing`<br><br>`_dataitem`<br><br><br><br><br><br><br><br><br><br><br><br> | 
`MINUTE`<br>`QTRHOUR`<br>`HOUR`<br>`TIMEOFDAY`<br>`DAY`<br>`WEEK`<br>`MONTH`<br>`QUARTER`<br>`YEAR`<br>`FIVEYEAR`<br><br><br><br> | `pack_in_mj`<br>`pack_out_mj`<br><br><br><br><br><br><br><br><br><br><br><br><br>            | `battery_in_mj`<br>`battery_out_mj`<br>`pv_1_mj`<br>`pv_2_mj`<br>`pv_3_mj`<br>`pv_4_mj`<br>`load_1_mj`<br>`load_2_mj`<br><br><br><br><br><br><br>               | `battery_in_mj`<br>`battery_out_mj`<br>`pv_1_mj`<br>`pv_2_mj`<br>`pv_3_mj`<br>`pv_4_mj`<br>`load_1_mj`<br>`load_2_mj`<br>`grid_1_in_mj`<br>`grid_1_out_mj`<br>`grid_2_in_mj`<br>`grid_2_out_mj`<br>`grid_3_in_mj`<br>`grid_3_out_mj`

A row is scoped first and foremost to a minute as indicated by its _Row id_ (_YYYYMMDDHHmm_).

- Every row has an `INSTANT` _tempolumn family_ which contains all **monitoring** data samples logged during that minute. 

- Every row also has a `MINUTE` column family which contains **energy** data aggregates for the minute

The rest of the _tempolumn families_ will be present in a row only if the date-time component of the row id (_YYYYMMDDHHmm_) coincides with the period epoch (the start) of the period. 

- For example a row with an id of _PMS-01-006#202002091500_ will have _tempolumn families_ for `HOUR` and `MINUTE` as the date-time in the id (**1500**) coincides with the epoch (start) of an hour and minute period.

- Similarly a row with an id of _PMS-01-006#202002090000_ will have _tempolumn families_ for `DAY`, `HOUR`, and `TIMEOFDAY` as the date-time in the id (**090000**) coincides with the start of all three periods.

---


### Monitoring column qualifiers

Monitoring data is stored in the `INSTANT` _tempolumn family_.

Monitoring _tempolumn qualifiers_ are different for each device type (based on `<device_type>` in the row id) as shown in the table above.

`t_event` is stored for each data element in the cell _timestamp_ property.  

The `_dataitem` column contains a complete monitoring message including the cell _timestamp_, as described in the following `analytics` dataset pages.

- _[pms_telemetry](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/pms_telemetry)_
- _[mppt_telemetry](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/mppt_telemetry)_
- _[inverters_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/inverter_telemetry)_


---

### Energy column qualifiers

Energy _tempolumn qualifiers_ are for megajoules produced or consumed during the period indicated by the column family. 

The data is closely aligned to the Energy API [response](/docs/api.sundaya.monitored.equipment/0/c/Examples/GET/energy%20GET%20example), which presents these _tempolumn qualifiers_ as `harvest`, `yield`, `store`, and `buy`/`sell`.

The _tempolumns_ are different for each device type as shown in the table above (The `<device_type>` is in the row identifier).


--- 