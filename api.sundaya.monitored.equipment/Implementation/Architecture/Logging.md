# Logging
---

Logs are produced through a framework which provides statement wrappers over _Stackdriver_'s multiple APIs: `Logging`, `Error Reporting`, `Tracing`.

It decouples the parts which make up the logging process and makes these independently configurable and extensible.

- Output (`appenders`)
- Content (`verbosity`)
- Coverage (`statements`)

The `Logger` class aggregates 5 `Statement` classes for each statement type, and provides these to clients through a singleton. 

- Messaging
- Data
- Exception
- Error
- Trace

![Logger class hierarchy](/images/logging.png)

## configurables

The `Logger` class provides a public interface with the following helper methods for each statement type.


```javascript

    - log.messaging (topic, id, messages, quantity, sender)

    - log.data (dataset, table, id, rows) 

    - log.exception (label, message, event) 

    - log.error (label, stacktrace) {                                 

    - log.trace (label, value, payload) 

```


## configurables

The Logger is aware of which environment it is running in. 

- At startup it looks up configuration details for the environment in which it is running.

- logging configurations can be changed through [api\logging](/docs/api.sundaya.monitored.equipment/0/routes/api/logging/get)) 

- configuration changes are propogated to downstream services through a [Features](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Architecture/Messaging) message. 

Logger configurations are held in these artefacts:

- _enums.js_   -   enumerations for   `logging`, `features`
- _env.js_     -   configurations per environment for  `logging`, `features`, `stackdriver` 

The configuration groups are listed and described int he following sections.


