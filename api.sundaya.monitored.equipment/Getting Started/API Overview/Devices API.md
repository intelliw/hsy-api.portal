# Devices API

The `/devices` path is for receiving monitoring data from trackable devices such as _[pms](https://docs.sundaya.monitored.equipment/docs/api.sundaya.monitored.equipment/0/c/Implementation/Device%20Datasets/pms%20Dataset)_, _[mppt](https://docs.sundaya.monitored.equipment/docs/api.sundaya.monitored.equipment/0/c/Implementation/Device%20Datasets/mppt%20Dataset)_, and _[inverters](https://docs.sundaya.monitored.equipment/docs/api.sundaya.monitored.equipment/0/c/Implementation/Device%20Datasets/inverter%20Dataset)_.

These devices continuously produce monitoring data which is then collected and uploaded by device controllers (e.g. Bus Bar Controller) and gateways (e.g. EHub Gateway) through the `/devices` API path.

The collected data is then aggregated into time-windows to provide energy accounting data and event notifications, for users and vendors to monitor and control their energy and devices.


# /devices/dataset POST
---

The `/devices/{dataset}` path is for vendors and systems integrators to POST device data.

[https:/api.sundaya.monitored.equipment/devices/**{dataset}**](https:/api.sundaya.monitored.equipment/devices/dataset/pms)

- This route allows device **controllers** (e.g. Bus Bar Controller) and **gateways** (e.g. EHub Gateway) to accumulate and periodically send data from multiple monitored devices installed at a site.

### {dataset} parameter ###

The following dataset types may be specified as the `{dataset}` parameter in a `/device` path.

{dataset} | Description
--- | --- | --- 
[pms](https://docs.sundaya.monitored.equipment/docs/api.sundaya.monitored.equipment/0/c/Implementation/Device%20Datasets/pms%20Dataset) | Data from pack management systems (PMS) for cabinets, including monitored epacks, cells, and mosfets.
[mhttps:https://docs.sundaya.monitored.equipment/docs/api.sundaya.monitored.equipment/0/c/Implementation/Devichttps:atasets/mppt%20Dataset) | Data from Maximum Power Point Tracking (MPPTcontrollers, including connected PV strings, batteries, and DC loads.
[inverter](https://docs.sundaya.monitored.equipment/docs/api.sundaya.monitored.equipment/0/c/Implementation/Device%20Datasets/inverter%20Dataset) | Data from Inverter charge controllers, including connected pv strings, batteries, and AC loads.


# /device/dataset GET
---

The `/device/{device-id}/dataset/{dataset}` path allows field engineers to monitor an individual device during operation.
 
 [https:/api.sundaya.monitored.equipment/device/**{device-id}**/dataset/**{dataset}**/period/week/20150204/1](https:/api.sundaya.monitored.equipment/device/MPPT-01-002/dataset/mppt/period/week/20150204/1)

- This path provide a dedicated endpoint to retrive data for an individual **device** and dataset. 

### Path parameters

The following path parameters are required in device GET requests. If a path parameter is omitted it will be substituted as described.    

Parameter | Description 
--- | --- 
`device` | The device identifier. 
`dataset` | The dataset type (see section titled _{dataset} parameter_ above). 

### Query parameters
There are no query parameters for the `/device/dataset` route.

---