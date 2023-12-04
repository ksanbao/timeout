## Introduction：

This is a TTL callback server.


Typical application scenarios are heartbeat alert and delayed callback jobs.

## TTL Server Address：
Listen :2212

## Protocol：
HTTP POST

## TTL Callback API：

- /setTimeout：create/update TTL callback
- /clearTimeout：destroy TTL callback

## TTL API Description:
### setTimeout
```json
{
  name: "xxx", // string, the name, non-empty, case-sensitive
  ttl: 10, // number, TTL time value (seconds)
  expiredTime: 0, // number, TTL timestamp, default 0; format as javascript Date.now(), priority over 'ttl'
  backURL: "http(s)://xxx", // string, callback URL，backURL="false" means calling nothing
  data: any, // callback params, default null; arrayMode=false and data=null USE 'GET'，otherwise USE 'POST'
  oncreate: null | { data: any }, // if oncreate has data and TTL object is being created, callback with ocreate.data
  arrayMode: false, // boolean, callback with array, default false
  arrayLimit: 0, // number, if arrayMode is true, limit the maximum number, default 100.
}
```

RESPONSE:
request params fine >> HTTP 200
request params bad >> HTTP 400
service is exiting >> HTTP 502

#### Callabck Format Table:
| arrayMode=false and data=null | http.GET(backURL) |
| --- | --- |
| arrayMode=false and data≠null | http.POST(backURL, data) |
| arrayMode=true | http.POST(backURL, [data1, data2, ...] |
|  |  |


### clearTimeout
```json
{
  name: "xxx" // string, the name
}
```

RESPONSE:
request params fine >> HTTP 200
request params bad >> HTTP 400
service is exiting >> HTTP 502
