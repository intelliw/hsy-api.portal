# inverter POST example
---

### POST /dataset/inverter request

[/devices/dataset/inverter](http:/api.endpoints.sundaya.cloud.goog/devices/dataset/mppt)

The example below shows datasets collected at a BTS site with 2 Kehua SPI-B2 Inverter / Charge Controllers ('SPI-B2-01-001' and 'SPI-B2-01-002').

*Note: A site can have any number of Inverter-controllers, and each controller can have multiple PV strings and Loads. Presently upto 4 PV strings per controller are supported. In future each string will have its own dedicated controller*.

The example shows two data samples for each Inverter Controller taken 10 seconds apart (SPI-B2-01-001 at **15:00.06** and SPI-B2-01-002 at **15:00.07**).

In this example each Inverter controller has :

- 2 PV strings as shown by the data arrays in `pv.volts` and `pv.amps`.

- 2 AC Loads as shown by `load.volts` / `load.amps`.  

The size of each Inverter dataset is 367 bytes including whitespace, or 248 bytes if whitespace is stripped (using JSON.stringify).


```
*** REQUEST ***	
POST /devices/dataset/inverter HTTP/1.1	
Host: api.endpoints.sundaya.cloud.goog
Accept: application/json
Content-Type: application/json
    
```

```json
{
  "datasets": [
    { "inverter": { "id": "SPI-B2-01-001" }, 
      "data": [
        { "time_local": "20190209T150006.032+0700",
          "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0] },
          "battery": { "volts" : 55.1 }, 
          "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2] }, 
          "grid": { "volts": [48.000, 48.000, 48.000], "amps": [1.2, 1.2, 1.2], "pf": [0.92, 0.92, 0.92] }
        },
        { "time_local": "20190209T150016.022+0700",
          "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0] },
          "battery": { "volts" : 55.1 }, 
          "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2] },
          "grid": { "volts": [48.000, 48.000, 48.000], "amps": [1.2, 1.2, 1.2], "pf": [0.92, 0.92, 0.92] }
        }
      ]
    },
    { "inverter": { "id": "SPI-B2-01-002" }, 
      "data": [
        { "time_local": "20190209T150007.032+0700",
          "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0] },
          "battery": { "volts" : 55.1 }, 
          "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2] },
          "grid": { "volts": [48.000, 48.000, 48.000], "amps": [1.2, 1.2, 1.2], "pf": [0.92, 0.92, 0.92] }
        },
        { "time_local": "20190209T150017.022+0700",
          "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0] },
          "battery": { "volts" : 55.1 }, 
          "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2] },
          "grid": { "volts": [48.000, 48.000, 48.000], "amps": [1.2, 1.2, 1.2], "pf": [0.92, 0.92, 0.92] }
        }
      ]
    }
  ]
}
```

### POST /dataset/inverter response

A 200 Response will be sent if the request is accepted and queued for asynchronous processing. 

```
*** RESPONSE ***	
200 OK HTTP/1.1	
Content-Type: application/json
Content-Length: 1171	

```

```json
{
    "status": "200",
    "message": "ok",
    "details": [
        {
            "message": "Data queued for processing.",
            "target": "dataset:inverter"
        }
    ]
}
```

If the request is processed synchronously a 201 Response will be returned as follows. 

```
*** RESPONSE ***	
201 Created HTTP/1.1	
Location: http:/api.endpoints.sundaya.cloud.goog/device/SPI-B2-01-001/dataset/inverter/period/minute/20190209T1500+0700/1
Location: http:/api.endpoints.sundaya.cloud.goog/device/SPI-B2-01-002/dataset/inverter/period/minute/20190209T1500+0700/1
Content-Type: application/json
Content-Length: 1171	

```

```json
{
    "status": "201",
    "message": "Created",
    "details": [
        {
            "message": "New data logs created",
            "target": "device:SPI-B2-01-001 | dataset:inverter | events:2"
        },
        {
            "message": "New data logs created.",
            "target": "device:SPI-B2-01-002 | dataset:inverter | events:2"
        }
    ]
}
```

The initial data item format sent to the message broker is shown in the followng example:

```
*** MESSAGE ***
Topic: inverter
Key: SPI-B2-01-001
Value:	
```

```json
{ 
    "time_processing_utc":"2019-02-09T09:31:05.0110+0000",
    "time_utc": "2019-02-09T09:30:00.0200+0000",
    "time_local": "2019-02-09T16:30:00.0200+0700",
    "id": "SPI-B2-01-001",
    "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0], "watts": 288.01 },
    "battery": { "volts" : 55.1 }, 
    "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2], "watts": 57.60 },
    "grid": { "volts": [48.000, 48.000, 48.000], "amps": [1.2, 1.2, 1.2], "pf": [0.92, 0.92, 0.92], "watts": [91.782, 91.782, 91.782] }
},
---