# /devices/dataset POST
---

The `/devices/dataset/{dataset}` path is for vendors and systems integrators to POST device data.

[http:/api.endpoints.sundaya.cloud.goog/devices/{dataset}](http:/api.endpoints.sundaya.cloud.goog/devices/dataset/pms)

- This route allows device **controllers** (e.g. Bus Bar Controller) and **gateways** (e.g. EHub Gateway) to accumulate and periodically send data from multiple monitored devices installed at a site.

### {dataset} parameter ###

The following types may be specified as the `{dataset}` parameter in a `/device` path.

dataset | Description
--- | --- | --- 
[pms](https://endpointsportal.sundaya.cloud.goog/docs/api.endpoints.sundaya.cloud.goog/0/c/Implementation/Device%20Datasets/pms%20Dataset) | Data from pack management systems (PMS) for cabinets, including monitored epacks, cells, and mosfets.
[mppt](https://endpointsportal.sundaya.cloud.goog/docs/api.endpoints.sundaya.cloud.goog/0/c/Implementation/Device%20Datasets/mppt%20Dataset) | Data from Maximum Power Point Tracking (MPPTcontrollers, including connected PV strings, batteries, and DC loads.
[inverter](https://endpointsportal.sundaya.cloud.goog/docs/api.endpoints.sundaya.cloud.goog/0/c/Implementation/Device%20Datasets/inverter%20Dataset) | Data from Inverter charge controllers, including connected pv strings, batteries, and AC loads.


# /device/dataset GET
---

The `/device/{device-id}/dataset/{dataset}` path allows field engineers to monitor an individual device during operation.
 
 [http:/api.endpoints.sundaya.cloud.goog/device/{**device-id**}/dataset/{**dataset**}/period/week/20150204/1](http:/api.endpoints.sundaya.cloud.goog/device/BBC-PR1202-999/dataset/BBC-MPPT/period/week/20150204/1)

- This path provide a dedicated endpoint to retrive data for an individual **device** and dataset. 

### Path parameters

The following path parameters are required in device GET requests. If a path parameter is omitted it will be substituted as described.    

Parameter | Description 
--- | --- 
`device` | The device identifier. 
`dataset` | The dataset type (see section titled _{dataset} parameter_ above). 

### Query parameters
There are no query parameters for the `/device/dataset` route.
