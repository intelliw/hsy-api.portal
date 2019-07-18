# Devices
---

### POST /dataset/inverter request

[/devices/dataset/inverter](http:/api.endpoints.sundaya.cloud.goog/devices/dataset/mppt)

The example below shows datasets collected at a BTS site with 2 Kehua SPI-B2 Inverter / Charge Controllers ('SPI-B2-01-001' and 'SPI-B2-01-002').

*Note: A site can have any number of Inverter-controllers, and each controller can have multiple PV strings and Loads. Presently upto 4 PV strings per controller are supported. In future each string will have its own dedicated controller*.

The example shows two data samples for each Inverter Controller taken 10 seconds apart (SPI-B2-01-001 at **15:00.06** and SPI-B2-01-002 at **15:00.07**).

In this example each Inverter controller has :

- 2 PV strings as shown by the data arrays in `pv.volts` and `pv.amps`.

- 2 AC Loads as shown by `load.volts` / `load.amps`.  

The size of each Inverter dataset is 367 bytes including whitespace, or 248 bytes if whitespace is stripped (by using JSON.stringify).


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
    { "mppt": { "id": "SPI-B2-01-001" }, 
      "data": [
        { "time": "20190209T150006.032-0700",
          "pv": { "volts": ["48.000", "48.000"], "amps": ["6.0", "6.0"] },
          "battery": { "volts" : "55.1" }, 
          "load": { "volts": ["48.000", "48.000"], "amps": ["1.2", "1.2"] }
        },
        { "time": "20190209T150016.022-0700",
          "pv": { "volts": ["48.000", "48.000"], "amps": ["6.0", "6.0"] },
          "battery": { "volts" : "55.1" }, 
          "load": { "volts": ["48.000", "48.000"], "amps": ["1.2", "1.2"] }
        }
      ]
    },
    { "mppt": { "id": "SPI-B2-01-002" }, 
      "data": [
        { "time": "20190209T150007.032-0700",
          "pv": { "volts": ["48.000", "48.000"], "amps": ["6.0", "6.0"] },
          "battery": { "volts" : "55.1" }, 
          "load": { "volts": ["48.000", "48.000"], "amps": ["1.2", "1.2"] }
        },
        { "time": "20190209T150017.022-0700",
          "pv": { "volts": ["48.000", "48.000"], "amps": ["6.0", "6.0"] },
          "battery": { "volts" : "55.1" }, 
          "load": { "volts": ["48.000", "48.000"], "amps": ["1.2", "1.2"] }
        }
      ]
    }
  ]
}
```

### POST /dataset/inverter response

```
*** RESPONSE ***	
201 OK HTTP/1.1	
Location: http:/api.endpoints.sundaya.cloud.goog/device/SPI-B2-01-001/dataset/inverter/period/minute/20190209T1500-0700/1
Location: http:/api.endpoints.sundaya.cloud.goog/device/SPI-B2-01-002/dataset/inverter/period/minute/20190209T1500-0700/1
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
            "message": "New data logs created",
            "target": "device:SPI-B2-01-002 | dataset:inverter | events:2"
        }
    ]
}
```