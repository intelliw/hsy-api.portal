# Security
---

## API Keys

The API requires a secret Key to authenticate your account. 

Your client application authenticates to the API by providing a secret key in the request header.

### Requesting an API key

You can [request access](mailto:admin@api.sundaya.com) by signing up for an account. 

You can also request an additional key if you need to replace your key in future.

### API key usage

Each client application vendor has a unique key. The API gateway pre-authorises requests using this key and references it in request logs for traceability.

An API key is required in all paths except the following:

- The 'GET' operations in the `/api/versions` path does not require an API key.

- The 'GET' operations in the `/energy` path does not require an API key for the default site (`site=999`) which is reserved for aggregate querying.

In all other paths an API key is required using one of the following options.

Method | Parameter | In | Description
--- | --- | --- | ---
GET | `api_key` | query | `api_key` query parameter is accepted only in GET requests. 
POST, GET | `x-api-key` | header | POST requests must provide a `x-api-key` header. This option is also preferred for GET requests.

```
*** REQUEST ***	
POST /devices/dataset/pms HTTP/1.1	
Host: api.endpoints.sundaya.cloud.goog
Accept: application/vnd.collection+json	
x-api-key: X2zaSyASFGxf4PmOXtVS1Dt911PcZ4IQ8PUUMqX
```

```
*** REQUEST ***	
GET energy/hse/period/week/20190930T1200?api_key=X2zaSyASFGxf4PmOXt911PcZ4IQ8PUUMqX HTTP/1.1
Host: api.endpoints.sundaya.cloud.goog
Accept: text/html	
api_key: X2zaSyASFGxf4PmOXt911PcZ4IQ8PUUMqX

```

`401 Unauthorized` is returned if the API key is not valid or does not have access to the requested path.

```
*** RESPONSE ***	
401 Unauthorized HTTP/1.1	
Content-Type: application/json; charset=utf-8
Content-Length: 286
```

### Portal usage

The developer portal injects a `api_key` **query** parameter when using the 'Try this API' interface for GET requests. The parameter is not inspected by the API host and is required only for portal functionality. 

A `x-api-key` **header** must be populated and sent when using 'Try this API' with POST requests. 
