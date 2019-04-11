# Security
---

The API requires a secret Key to authenticate your account. 

Your client applicaiton authenticates to the API by providing a secret key in the request header.

### Requesting an API key

You can [request access](mailto:admin@api.sundaya.com) by signing up for an account. 

You can also request an additional key if you need to replace your key in future.

### API key usage

The API key must be provided in all requests as a query parameter, as shown:

Parameter | In | Description
--- | --- | ---
`api_key` | Request header | Each client application has its own unique key. The API uses the key for basic authentication and data provenance. 


*** REQUEST ***	
GET /energy/hse/period/week/20190204/ HTTP/1.1	
Host: api.endpoints.sundaya.cloud.goog
Accept: application/vnd.collection+json	
api_key: X2zaSyASFGxf4PmOitVS1Dt911PcZ4IQ8PUUMqA


`401 Unauthorized` is returned if the API key is not valid or does not have access to the requested path.

*** RESPONSE ***	
401 Unauthorized HTTP/1.1	
Content-Type: application/json; charset=utf-8
Content-Length: 286

### Developer portal 

The developer portal injects an `api_key` **query** parameter for the 'Try this API' interface.

The query parameter is not inspected by the API host and is required only by the portal. 

The portal does however also provide an `api_key` **header** parameter which should be populated and with the 'Try this API' interface for testing security. 