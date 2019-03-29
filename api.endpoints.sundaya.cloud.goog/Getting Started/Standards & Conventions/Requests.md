# Energy Data Requests
---

The `/energy` path returns a ‘cube’ of energy data starting at a specified epoch, including links to navigation to adjacent periods. 

- /energy/{`energy`}/periods/{`period`}/{`epoch`}/{`duration`}?site={`site`}

    e.g. [http://http://api.endpoints.sundaya.cloud.goog/energy/hse/periods/week/20150204/1?site=999](http://http://api.endpoints.sundaya.cloud.goog/energy/hse/periods/week/20150204/1?site=999 "energy=hse, period=week, duration=1, site=999")

### Path parameters

The following path parameters are required in energy data requests. If a parameter is invalid or omitted it will be defaulted as shown.   

Parameter | Description | Default
--- | --- | --- 
`energy` | The type of energy flow. | *week*
`period` | The time window for which total energy is aggregated. The only exception is 'instant' which is for a single point in time (a millisecond), without aggregation. | The current UTC date-time.
`duration` | The number of periods to return starting at epoch. This defaults to 1. | *1*
`site` | Identifier of the customer site where energy assets have been installed. | *999*

### period, epoch, duration
- The returned data contains data for the child and grandchild of the requested `period`. For example if a request is made for a */week* the response will contain total energy for each *day* and a breakdown of energy for each *hour*. 

- The `duration` parameter specifies the third dimension. It returns multiples of the above period data arrays. For example if a request is made for a */week* period with a duration of 3, the response will contain 3 collections of weekly energy data as described above, starting at the requested epoch. 

- `epoch` specifies  the starting date-time of the period. The epoch is also displayed in links next to the period. 

The following table describes each `period` and formats used for `epoch` in the returned data. The *compressed* format is used in hyperlinks (in the href attribute) and the *uncompressed* format is used in display attributes.
An `instant` represents a single point in time (a millisecond) without data aggregation.
`second` and `minute` periods both include aggregates of any instants in which data was logged.

Period | Child | Duration | Compressed format | Uncompressed format
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
`api_key` | The key configures basic authentication for each client application. | 


---