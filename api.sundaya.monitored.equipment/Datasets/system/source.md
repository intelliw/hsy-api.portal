# system.source
---

This dataset provides reference data for platform services and the data management system, including data needed for security, traceability, and data provenance. It includes configuration data needed for provisioning and commissioning monitored devices and systems.


---

# Dataset Structure 

The following attributes are added to request message attributes at the first stage of processing the `/devices` POST request. 

The added timestamps are based on `time_local` sent in the request message, which is replaced by these timestamps.  

The dataset timestamps are stored in canonical timestamp format ('YYYY-MM-DD HH:mm:ss.SSSS') as shown in `monitoring` dataset examples.

Attribute | Metric | Data | Constraint | Description
--- | --- | --- | --- | ---
`id` | - | string | - | The short identifier of the source system owner: a source system is one which sends monitoring data. This identifier is based on the API key sent in in monitoring datasets requests, and is added to the `sys.source` attribute by the API host to provides traceability and data provenance.
`name` | - | string | - | The name of the vendor or development organisatopm responsible for the source system. 

```

