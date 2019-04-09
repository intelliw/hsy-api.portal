# Energy data
---

The `/energy` path returns a ‘cube’ of energy data for a specified number of periods starting at an epoch. 

The response includes links to navigate from the reqested period to adjacent periods (next week, month etc.). 

- /energy/{`energy`}/periods/{`period`}/{`epoch`}/{`duration`}?site={`site`}

    e.g. [http://api.endpoints.sundaya.cloud.goog/energy/hse/periods/week/20150204/1?site=999](http://api.endpoints.sundaya.cloud.goog/energy/hse/periods/week/20150204/1?site=999 "energy=hse, period=week, duration=1, site=999")

### /energy Path parameters

The following path parameters are required in energy data requests. If a parameter is omitted it will be defaulted as shown. If the paramter was provided it will be validated agains the controlled value lists (enums) specified in the API.     

Parameter | Description | Default
--- | --- | --- 
`energy` | The type of energy flow. | *hse*
`period` | The time window for which total energy is aggregated. The only exception is 'instant' (which is for a single point in time, a millisecond) which is presented without aggregation. | *week*
`epoch` | The starting date and time for the period. | current UTC date-time
`duration` | The number of periods to return starting at epoch. This defaults to 1. | *1*
`site` | The customer site where energy assets have been installed. | *999*

### period, epoch, duration
- The returned data contains data for the child and grandchild of the requested `period`. For example if a request is made for a */week* the response will contain total energy for each *day* and a breakdown of energy for each *hour*. 

- The `duration` parameter specifies the third dimension. It returns multiples of the above period data arrays. For example if a request is made for a */week* period with a duration of 3, the response will contain 3 collections of weekly energy data as described above, starting at the requested epoch. 

- `epoch` specifies  the starting date-time of the period. The epoch is displayed in links in the the `href` attribute, after the `period`. 

The following table describes each `period` and formats used for `epoch` in the returned data. 

- The *compressed* format is used in hyperlinks (in the `href` attribute) and the *uncompressed* format is intended for use in display labels.

- An `instant` represents a single point in time (a millisecond). As suchg there is no data aggregation for `instant`.

- `second` and `minute` periods both include aggregates of any instants in which data was logged.

- `timeofday` refers to 6-hourly blocks of time for Morning, Afternoon, Evening, Night.

Period | Child Period | Duration | Format (*compressed*) | (*uncompressed*)
--- | --- |--- | --- | --- 
`instant` | - | - | YYYYMMDDTHHmmss.SSS | DD/MM/YY HHmmss.SSS
`second` | `instant` | 1000 | YYYYMMDDTHHmmss | DD/MM/YY HHmm:ss
`minute` | `second` | 60 | YYYYMMDDTHHmm | DD/MM/YY HH:mm
`hour` | `minute` | 60 | YYYYMMDDTHHmm | DD/MM/YY HH:mm
`timeofday` | `hour` | 6 | YYYYMMDDTHHmm | DD/MM/YY HH:mm
`day` | `timeofday` | 4 | YYYYMMDD | DD/MM/YY
`week` | `day` | 7 | YYYYMMDD | DD/MM/YY
`month` | `day` | *varies*  | YYYYMMDD | DD/MM/YY
`quarter` | `month` | 3 | YYYYMMDD | DD/MM/YY
`year` | `quarter` | 4 | YYYYMMDD | DD/MM/YY
`fiveyear` | `year` | 5 | YYYYMMDD | DD/MM/YY

### Query parameters
In all requests the caller must also provide the following query parameters:

Parameter | Description | Default
--- | --- | --- 
`site` | Identifier of the customer site where energy assets have been installed. | *999*
`api_key` | Each client application has a key which is used to configure basic authentication and data provenance. | 

### Body parameters
The `product-catalogue-items` body parameter specifies an optional filter for `/energy` data to be restricted to certain products. 

The response will contain data for *any* of the product categories, subcategories, and product types specified in the parameter. 

- If the `product-catalogue-items` parameter is not provided data will be returned for all products.

- If multiple products are specified data will be returned for *any* of those products.

- If a `product-category` is specified without a subcategory or product type data will be returned for *all* subcategories and types in that category. The same applies if a category and `product-subcategory` is specified without a `product-type`.

# Device data
---

The `/devices` path is for vendors and systems integrators to log device data, and for field engineers to monitor and control device operation.

The fully qualified `/devices` path includes a `device-identfier` and a `device-dataset` as shown in the following example. 

  [http:/api.endpoints.sundaya.cloud.goog/devices/{**device-identifier**}/datasets/{**device-dataset**}](http:/api.endpoints.sundaya.cloud.goog/devices/BBC-PR1202-999/datasets/MPPT-SNMP)

- This route provides a dedicated endpoint for each end **device** to stream its data. 

- Body payload data for devices or datasets which do not match those specified in the path (based on the *href* attribute of the data payload) will be ignored.

The `/devices` path can also be called without a device identifier and/or dataset as shown in the following examples. 

  [http:/api.endpoints.sundaya.cloud.goog/devices](http:/api.endpoints.sundaya.cloud.goog/devices)

  [http:/api.endpoints.sundaya.cloud.goog/devices/{device-identifier}](http:/api.endpoints.sundaya.cloud.goog/devices/BBC-PR1202-999)

- These routes are for **gateways** to accumulate data from multiple devices and datasets, and post these periodically wihout any addressing logic needed at the time of delivery.


### Body parameters

The `device-dataset-data` body parameter contains one or more data items, each consisting of:

- a *href* identifier for the device and dataset.

- an `event.time` for each dataset.   

- a canonical set of attributes for the dataset.

If the root `\devices` path is called the payload must provide device and dataset details in the *href* attribute of each data item as shown in the following snippet.

```json
items": [
{ "href": "http:/api.endpoints.sundaya.cloud.goog/devices/BBC-PR1202-999/datasets/MPPT-SNMP",
  "data": [
  { "name": "event.time", "value": "20190209T150006.022-0700",
    "data": [
      { "name": "pv1", "value": "99" },
```

### /devices Path parameters

The following path parameters are required in device requests. If a path parameter is omitted it will be substituted as described.    

Parameter | Description | Substitute
--- | --- | --- 
`device` | The device identifier. | If missing the `device` in the *href* attribute of each data item in the body parameter is used instead. 
`dataset` | A canonical set of pre-configured data attributes. | If missing the `dataset` in the *href* attribute of each data item in the body parameter is used instead.

### Query parameters
There are no query parameters for the `/devices` route.




---