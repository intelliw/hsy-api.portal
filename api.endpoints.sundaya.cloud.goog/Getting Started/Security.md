# Security
---

The API requires a secret Key to authenticate your account. 

### Requesting an API key

You can [request access](mailto:admin@api.sundaya.com) by signing up for an account. 

You can also request an additional key if you need to replace your key in future.

### API key usage

The API key must be provided in all requests as a query parameter, as shown:

Parameter | Description 
--- | --- 
`api_key` | Each client application has a key which is used to configure basic authentication and data provenance. 

e.g. [/energy/hse/period/week/20190210?site=999&**api_key**=AIzaSyASFQxf4PmOutVS1Dt99TPcZ4IQ8PDUMqY](http:/api.endpoints.sundaya.cloud.goog/energy/hse/period/week/20190210?site=999&api_key=AIzaSyASFQxf4PmOutVS1Dt99TPcZ4IQ8PDUMqY)

`401 Unauthorized` is returned if the API key is not valid or does not have access to the requested path.







