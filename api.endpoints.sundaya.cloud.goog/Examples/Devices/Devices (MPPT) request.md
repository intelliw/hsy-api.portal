# Devices
---

### POST /devices/dataset/mppt request and response

[/devices/dataset/mppt](http:/api.endpoints.sundaya.cloud.goog/devices/dataset/mppt)

The example below shows datasets collected at a BTS site with 2 EPSolar MPPT Charge Controllers ('IT6415AD-01-001' and 'IT6415AD-01-002').

*Note: A site can have any number of MPPT controllers, and each controller can have multiple PV strings and Loads. Presently upto 4 PV strings per controller are supported. In future each string will have its own dedicated controller*.

The example shows two data samples for each MPPT Controller taken 10 seconds apart (IT6415AD-01-001 at **15:00.06** and IT6415AD-01-002 at **15:00.07**).

In this example each MPPT controller has :

- 2 PV strings as shown by the data arrays in `pv.volts` and `pv.amps`.

- 2 Loads as shown by `load.volts` / `load.amps`: Load 1 is the VSAT system and Load 2 is the BTS.  

The size of each MPPT dataset is 367 bytes including whitespace, or 248 bytes if whitespace is stripped (by using JSON.stringify)..


```
*** REQUEST ***	
POST /devices/dataset/mppt HTTP/1.1	
Host: api.endpoints.sundaya.cloud.goog
Accept: application/json
Content-Type: application/json
    
*** RESPONSE ***	
201 OK HTTP/1.1	
Location: http:/api.endpoints.sundaya.cloud.goog/device/IT6415AD-01-001/dataset/mppt/period/minute/20190209T1500-0700/1
Location: http:/api.endpoints.sundaya.cloud.goog/device/IT6415AD-01-002/dataset/mppt/period/minute/20190209T1500-0700/1
Content-Type: application/json
Content-Length: 1171	

```


```json
{
  "datasets": [
    { "mppt": { "id": "IT6415AD-01-001" }, 
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
    { "mppt": { "id": "IT6415AD-01-002" }, 
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
