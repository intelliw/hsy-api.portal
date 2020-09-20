# Devices API

The `/devices` path is for receiving monitoring data from trackable devices such as _[pms](/docs/api.sundaya.monitored.equipment/0/c/Examples/POST/pms%20POST%20example)_, _[mppt](/docs/api.sundaya.monitored.equipment/0/c/Examples/POST/mppt%20POST%20example)_, and _[inverters](/docs/api.sundaya.monitored.equipment/0/c/Examples/POST/inverter%20POST%20example)_.

These devices continuously produce monitoring data which is then collected and uploaded by device controllers (e.g. Bus Bar Controller) and gateways (e.g. EHub Gateway) through the `/devices` API path.

The collected data is then aggregated into time-windows to provide energy accounting data and event notifications, for users and vendors to monitor and control their energy and devices.


# /devices/dataset POST
---

The `/devices/{dataset}` path is for vendors and systems integrators to POST device data.

[https:/api.sundaya.monitored.equipment/devices/**{dataset}**](https:/api.dev.sundaya.monitored.equipment/devices/dataset/pms)

- This route allows device **controllers** (e.g. Bus Bar Controller) and **gateways** (e.g. EHub Gateway) to accumulate and periodically send data from multiple monitored devices installed at a site.

### {dataset} parameter ###

The following canonical dataset types may be used in the `{dataset}` parameter.

/{dataset} | Description
--- | --- 
[pms](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/pms_telemetry) | Data from pack management systems (PMS) for cabinets, including monitored epacks, cells, and mosfets.
[mppt](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/mppt_telemetry) | Data from Maximum Power Point Tracking (MPPTcontrollers, including connected PV strings, batteries, and DC loads.
[inverter](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/inverter_telemetry) | Data from Inverter charge controllers, including connected pv strings, batteries, and AC loads.


# /device/dataset GET
---

The `/device/{device-id}/dataset/{dataset}` path allows field engineers to monitor an individual device during operation.
 
 [https:/api.sundaya.monitored.equipment/device/**{device-id}**/dataset/**{dataset}**/period/week/20150204/1](https:/api.dev.sundaya.monitored.equipment/device/MPPT-01-002/dataset/mppt/period/week/20150204/1 "device=MPPT-01-002, dataset=mppt, period=week, epoch=20150204, duration=1")

- This path provide a dedicated endpoint to retrieve data for an individual **device** and dataset. 

### Path parameters

The following path parameters are required in device GET requests. If a path parameter is omitted it will be substituted as described.    

Parameter | Description 
--- | --- 
`device` | The device identifier. 
`dataset` | The dataset type (see section titled _{dataset} parameter_ above). 

### Query parameters
There are no query parameters for the `/device/dataset` route.

---