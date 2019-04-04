# Security
---

The API requires a secret Key to authenticate your account. 

### Requesting an API key
You can [request access](mailto:admin@api.sundaya.com) by signing up for an account. 

You can also request an additional key if you need to rotate your key in future.

### API key usage
The API key must be provided with the request in a query parameter as shown:

e.g. [http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/week/20190210?site=999&api_key=AIzaSyASFQxf4PmOutVS1Dt99TPcZ4IQ8PDUMqY](http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/week/20190210?site=999&api_key=AIzaSyASFQxf4PmOutVS1Dt99TPcZ4IQ8PDUMqY)

A `401' *Unauthorized* response is returned if an API key is not valid, or if the key is not authorised to access an API path.






