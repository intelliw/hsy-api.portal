# Environment Configurations (sys_env_config)
---

The `sys_env_config` dataset contains environment configurations and is stored as a JSSON document in _src/environment/env.js_. 

- Data is mastered in the `api-host` project and the master copy is stored in its source code repository.

- Other projects share the configuration data through symlinks to the `api-host` project tree.

---


# Logging

This sections describes configuration details for [Logging](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Architecture/Logging). 


#### enumerations

Configurable values for Logging are constrained by the folowing Enumerations for logging and error reporting in `enums.js`.

```json
    enums.logging: {
        "statements": {
            "data": "data",
            "error": "error",
            "exception": "exception",
            "messaging": "messaging",
            "trace": "trace"
        },
        "verbosity": {
            "none": "none",
            "info": "info",
            "debug": "debug"
        },
        "appenders": {   
            "console": "console",
            "stackdriver": "stackdriver"
        }
    }
```
__statements__  - determines which log statements will produce log output.

__verbosity__   - log levels for error reporting and logged events.

__appenders__   - output options for logging and error reporting.


#### env.LOGGING 


```json
    env._LOGGING: {
            "statements": [ enums.logging.statements ],
            "verbosity": [ enums.logging.verbosity ],
            "appenders": [ enums.logging.appenders ]
        }
```
- __statements__    - determines which statements will be logged (`data`, `error`, `exception`, `messaging`, `trace`). 

- __verbosity__     - determines which content will be logged (`info`, `debug`, `none`). 

    Only one option is applicable: if multiple options are present `none` overrides `debug` which overrides `info`.

    If verbosity is set to `none` all logging is effectively turned off.
    
- __appenders__     - determines the destination for logging output (`stackdriver`, or `console`).
                    
    Both options can be active. For example the DEV environment may be configured to log to both `stackdriver` and `console`.

If a `statement`, `verbosity`, or `appender` option is excluded the corresponding Statement methods are annulled at startup (and after reconfiguration)

This minimises the cost to performance from disabled statements (the 'cost of not logging') until they are needed.


#### env._SHARED.STACKDRIVER 
```json
    env._SHARED.STACKDRIVER: {
        "logging": { "logName": "monitoring_prod", "resourceType": "gce_instance" },  
        "errors": { "reportMode": "always", "logLevel": 5 },                          
        "trace": {
            "samplingRate": 500, "enabled": true, "flushDelaySeconds": 1,             
            "ignoreUrls": [/^\/static/], "ignoreMethods": ["OPTIONS", "PUT"]        
        }
    }
```
- __logging__   
    
    use 'generic_task' (not tested) for microservices. 
                
    Cloud Run resourceType is "cloud_run_revision".

    For GCE VM Instances the resourceType is ''gce_instance'

    logName appears in Stackdriver logs as _jsonPayload.logName_: "projects/sundaya/logs/monitoring"

    The format is "projects/[PROJECT_ID]/logs/[LOG_ID]"

- __errors__    
    
    `production` (default), `always`, or `never` 

    `production` will not log unless _NODE-ENV=production_

    `logLevel` specifies when errors are reported to the Error Reporting Console: 2 (warnings). 0 (no logs) 5 (all logs)      

- __trace__     

    enabled=false to turn OFF tracing. 

    samplingRate 500 means sample 1 trace every half-second, 5 means at most 1 every 200 ms. 

    flushDelaySeconds = seconds to buffer traces before publishing to Stackdriver, keep short to allow cloud run to async trace immedatily after sync run.

    ignoreUrls is configured to ignore /static "path".

    ignoreMethods is configured to ignore requests with OPTIONS & PUT methods (case-insensitive).


---

# Features

This sections describes configuration details for [Features](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Architecture/Messaging) message. 

#### enumerations

Configurable flags for Features are constrained by the folowing Enumerations for feature toggles in `enums.js`.

```json
    enums.features = {
        "release": { "none": "none" },
        "operational": {
            "none": "none",
            "logging": "logging",                             
            "validation": "validation"                        
        },
        "experiment": { "none": "none" },
        "permission": { "none": "none" }
    }
```
__validation__  - whether or not to to perform in-depth input validation for post requests.

__logging__     - logging reconfiguration feature.


#### env.FEATURES 

```json
    env._FEATURES: {
        "release": [ enums.features.release ],
        "operational": [ enums.features.operational ],
        "experiment": [ enums.features.experiment ],
        "permission": [ enums.features.permission ]
    }
```
Toggles features on and off.

The logging feature (`enums.features.operational.logging`) detrermines whether logging can be reconfigured at runtime.

If the feature is present Logger can be reconfigured at runtime through the `api/logging` path and configuration changes will be propogated through the message broker to other microservices.


__release__     - feature is made active for a particular release.

    automatically deactivated when a new release is made

    if multiple releases are present in the environment (e.g. canary release) only the specified release(s) will provide the feature. 

__operational__ - feature is intended for operational use (not business logic)

__experiment__  - feature is experimental and may be restricted by time and scope

__permission__  - feature restricts access to certain users


---