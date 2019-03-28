# Energy Data Requests
---

The `/energy` path returns a ‘cube’ of energy data starting at a specified epoch, including links to navigation to adjacent periods. 

- /energy/{`energy`}/periods/{`period`}/{`epoch`}/{`duration`}?site={`site`}

    e.g. [http://http://api.endpoints.sundaya.cloud.goog/energy/hse/periods/week/20150204/1?site=999](http://http://api.endpoints.sundaya.cloud.goog/energy/hse/periods/week/20150204/1?site=999 "energy=hse, period=week, duration=1, site=999")

## Parameters

Parameter | Description
--- | --- 
`energy` | The type of energy flow.
`period` | The time window for which total energy is aggregated. The only exception is 'instant' which is for a single point in time (a millisecond), without aggregation.
`duration` | The number of periods to return starting at epoch. This defaults to 1.
`site` | Identifier of the customer site where energy assets have been installed.

### period, epoch, duration

- The returned data contains data for the child and grandchild of the requested `period`. For example if a request is made for a */week* the response will contain total energy for each *day* and a breakdown of energy for each *hour*. 

- The `duration` parameter specifies the third dimension. It returns multiples of the above period data arrays. For example if a request is made for a */week* period with a duration of 3, the response will contain 3 collections of weekly energy data as described above, starting at the requested epoch. 

- `epoch` specifies  the starting date-time of the period. The epoch is alsio displayed in links next to the period. 

The following table describes each `period` and formats used for `epoch` in the returned data. The *compresed* format is used in hyperlinks (in the href attribute) and the *uncopmpressed* format is used in display atributes .

Period | Description | Compressed format | Uncompressed format
--- | --- | --- | --- 
`instant` |   | YYYYMMDDTHHmmss.SSS | DD/MM/YY HHmmss.SSS
`second` |   | YYYYMMDDTHHmmss | DD/MM/YY HHmm:ss
`minute` |   | YYYYMMDDTHHmm | DD/MM/YY HH:mm
`hour` |   | YYYYMMDDTHHmm | DD/MM/YY HH:mm
`timeofday` |   | YYYYMMDDTHHmm | DD/MM/YY HH:mm
`day` |   | YYYYMMDD | DD/MM/YY
`week` |   | YYYYMMDD | DD/MM/YY
`month` |   | YYYYMMDD | DD/MM/YY
`quarter` |   | YYYYMMDD | DD/MM/YY
`year` |   | YYYYMMDD | DD/MM/YY
`fiveyear` |   | YYYYMMDD | DD/MM/YY

