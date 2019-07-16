# /energy GET
---

The `/energy` path returns a ‘cube’ of energy data for a specified number of periods starting at an epoch. 

It response contains aggregated data for the requested period, and for its child period. For example a request for a *week* period will produce a response with data aggregated for the *week* and for its child period: *day*.

The response will also include links to navigate from the reqested period to adjacent periods (next week, month etc.). 

- /energy/{`energy`}/period/{`period`}/{`epoch`}/{`duration`}?site={`site`}

    e.g. [http://api.endpoints.sundaya.cloud.goog/energy/hse/period/week/20150204/1?site=999](http://api.endpoints.sundaya.cloud.goog/energy/hse/period/week/20150204/1?site=999 "energy=hse, period=week, duration=1, site=999")

### Path parameters

The following path parameters are required in energy data requests. If a parameter is omitted it will be defaulted as shown. If the paramter was provided it will be validated agains the controlled value lists (enums) specified in the API.     

Parameter | Description | Default
--- | --- | --- 
`energy` | The type of energy flow. | *hse*
`period` | The time window for which total energy is aggregated. The only exception is 'instant' (which is for a single point in time, a millisecond) which is presented without aggregation. | *week*
`epoch` | The starting date and time for the period. | current UTC date-time
`duration` | The number of periods to return starting at epoch. | *1*
`site` | The customer site where energy assets have been installed. | *999*

### period, epoch, duration
- The returned data includes the child and grandchild of the requested `period`. For example if a request is made for a */week* the response will contain total energy for each *day* and a breakdown of energy for each *hour*. 

- The `duration` parameter specifies the third dimension. It returns multiples of the above period data arrays. For example if a request is made for a */week* period with a duration of 3, the response will contain 3 collections of weekly energy data as described above, starting at the requested epoch. 

- `epoch` specifies  the starting date-time of the period. The epoch is displayed in links in the the `href` attribute, after the `period`. 

The following table describes each `period` and formats used for `epoch` in the returned data. 

- The *compressed* format is used in hyperlinks (in the `href` attribute) and the *uncompressed* format is intended for use in display labels.

- An `instant` represents a single point in time (a millisecond). As such there is no data aggregation for `instant`.

- `second` and `minute` periods both include aggregates of any instants in which data was logged.

- `timeofday` refers to 6-hourly blocks of time for Morning, Afternoon, Evening, Night.

Period | Child Period | Duration | Grandchild Period | Duration | Format (*compressed*) | (*uncompressed*)
--- | --- |--- | --- | --- | --- | --- 
`instant` | - | - | - | - | YYYYMMDDTHHmmss.SSS | DD/MM/YY HHmmss.SSS
`second` | `instant` | *varies* | - | - | YYYYMMDDTHHmmss | DD/MM/YY HHmm:ss
`minute` | `second` | 60 | `instant` | *varies* | YYYYMMDDTHHmm | DD/MM/YY HH:mm
`hour` | `minute` | 60 | `second` | 60 (360) | YYYYMMDDTHHmm | DD/MM/YY HH:mm
`timeofday` | `hour` | 6 | `minute` | 60 (360) | YYYYMMDDTHHmm | DD/MM/YY HH:mm
`day` | `hour` | 4 | `minute` | 60 (240) | YYYYMMDD | DD/MM/YY
`week` | `day` | 7 | `timeofday` | 4 (28) | YYYYMMDD | DD/MM/YY
`month` | `day` | *varies* | `hour` | *varies* | YYYYMMDD | DD/MM/YY
`quarter` | `month` | 3 | `day` | *varies* | YYYYMMDD | DD/MM/YY
`year` | `quarter` | 4 | `month` | *varies* | YYYYMMDD | DD/MM/YY
`fiveyear` | `year` | 5 | `quarter` | 4 (20) | YYYYMMDD | DD/MM/YY

### Query parameters
In all requests the caller must also provide the following query parameters:

Parameter | Description | Default
--- | --- | --- 
`site` | Identifier of the customer site where energy assets have been installed. | *999*

### Body parameters
The `productCatalogItems` optional body parameter specifies a query filter for `/energy` data to be restricted to one or more products. 

The query response will contain data for *any* of the product categories, subcategories, and product types specified in the parameter. 

- If the `productCatalogItems` parameter is missing data will be returned for all products.

- If multiple products are specified data will be returned for *any* of those products.

- If a `productCategory` is specified without a subcategory or product type data will be returned for *all* subcategories and types in that category.

- The same applies if a category and `productSubcategory` is specified without a `productType`.


# /devices/dataset POST
---

The `/devices/dataset/{dataset}` path is for vendors and systems integrators to POST device data.

[http:/api.endpoints.sundaya.cloud.goog/devices/{dataset}](http:/api.endpoints.sundaya.cloud.goog/devices/dataset/epack)

- This route allows device **controllers** (e.g. Bus Bar Controller) and **gateways** (e.g. EHub Gateway) to accumulate and periodically send data from multiple monitored devices installed at a site.

### {dataset} parameter ###

The following types may be specified as the `{dataset}` parameter in all `/device` paths.

dataset | Description
--- | --- | --- 
`epack` | Monitoring data from pack management systems (PMS) for cabinets, including data for monitored epacks, cells, and mosfets.
`mppt` | Monitoring data from Maximum Power Point Tracking (MPPT) charge controllers, including data for connected PV strings, batteries, and DC loads.
`inverter` | Monitoring data for Inverter charge controllers, including data for connected pv strings, batteries, and AC loads.


### 'epack' dataset Body parameters

The following snippet shows the data structure for the `epack` dataset type:

```json
{
  "datasets": [
    { "cabinet": "CAB-01-001", 
      "data": [
        { "time": "20190209T150006.032-0700",
          "pack": { "id": "EPACK-01-001", "slot": "01", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        },
```

The dataset attributes are specified below. 

Attribute | Metric | Data Type | Constraint | Optionality | Description
--- | --- | --- | --- | --- | --- 
`cabinet` | (*id*) | string | string | mandatory | Id of the epack Cabinet. *(Note that each PMS controller (BBC) is able to handle 4 cabinets, with upto 12 packs in each cabinet, and 14 cells per pack)*.
`time` | (*event time*) | datetime | RFC 3339 | mandatory | The time of the event which produced this data sample, in compressed `ISO 8601/RFC3339` (YYYYMMDDThhmmss±hhmm).
`pack.id` | (*id*) | string |  | mandatory | The pack identifier. Ideally this should be a logical identifier (not the MCU hardware id) which is flashed onto the pack control board when it is first commissioned, or returned to service after repairs. 
`pack.slot` |  | integer | 1-12 | mandatory | The cabinet slot (1-12) in which this pack is installed. 
`pack.amps` | amps | float |  | mandatory | The current draw from this pack. *(Note: pack volts is calculated by the back-end data consumer by adding together `cell.volts` and is therefore not included in this dataset)*.  
`pack.soc` | % | float | 0-100 | required on change | The % state of charge of this pack.
`pack.temp` | degC | float |  | required on change | The temperature of the cell-pack at its top, middle, and bottom. 
`cell.volts` | volts | float |  | mandatory | Voltage readings for 14 cellblocks inside this pack.
`fet.temp` | degC | float |  | required on change | The temperature of the input (CMOS) and output (DMOS) MOSFETS.
`fet.status` | (*open/closed*) | boolean | 1/0 | required on change | The status of the input (CMOS) and output (DMOS) MOSFETS (*1=open/0=closed*).
 
- Attributes marked as '*required on change*' may be ommitted if the value has not changed since the last successful post (a POST is successful if acknowledged by the server with a 201 response).

### 'mppt' dataset Body parameters

The following snippet shows the data structure for the `mppt` dataset type:

```json
{
  "datasets": [
    { "mppt": "IT6415AD-01-001", 
      "data": [
        { "time": "20190209T150006.032-0700",
          "pv": { "volts": ["48.000", "48.000"], "amps": ["6.0", "6.0"] },
          "battery": { "volts" : "55.1" }, 
          "load": { "volts": ["48.000", "48.000"], "amps": ["1.2", "1.2"] }
        },
```

### 'inverter' dataset Body parameter

The following snippet shows the data structure for the `mppt` dataset type:

```json
{
  "datasets": [
    { "inverter": "SPI-B2-01-001", 
      "data": [
        { "time": "20190209T150006.032-0700",
          "pv": { "volts": ["48.000", "48.000"], "amps": ["6.0", "6.0"] },
          "battery": { "volts" : "55.1" }, 
          "load": { "volts": ["48.000", "48.000"], "amps": ["1.2", "1.2"] }
        },
```

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
`dataset` | The dataset type (see **{dataset} parameter** above). 

### Query parameters
There are no query parameters for the `/device/dataset` route.

---