# system.source
---

This dataset provides reference data for platform services and the data management system, including data needed for security, traceability, and data provenance. It includes configuration data needed for provisioning and commissioning monitored devices and systems.


---

# Dataset Structure 

The following attributes are present in the `system.source` dataset tab. 

Attribute | Metric | Data | Constraint | Description
--- | --- | --- | --- | ---
`id` | - | string | - | The short identifier of the source system owner: a source system is a device which sends monitoring data. This identifier is based on the API key sent in in monitoring datasets requests, and is added to the `sys.source` attribute by the API host to provides traceability and data provenance.
`name` | - | string | - | The name of the vendor or development organisatopm responsible for the source system. 

```

