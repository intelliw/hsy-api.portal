# Devices
---

### POST /devices/dataset/mppt request - for EPSolar MPPT/Charge Controllers 'IT6415AD-01-001' and 'IT6415AD-01-002'

[/devices/dataset/mppt](http:/api.endpoints.sundaya.cloud.goog/devices/dataset/mppt)

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
Content-Length: 250	

```


```json
{
  "datasets": [
    { "controller": "IT6415AD-01-001", 
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
    { "controller": "IT6415AD-01-002", 
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
