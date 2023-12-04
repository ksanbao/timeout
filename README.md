## Introduction

This is a TTL callback server.


Typical application scenarios are heartbeat alert and delayed callback jobs.

## TTL Server Address
Listen :2212

## Protocol
HTTP POST

## TTL Callback API

- /setTimeout：create/update TTL callback
- /clearTimeout：destroy TTL callback

## TTL API Description
### setTimeout
| Field | Type | Description |
| --- | --- | --- |
| name | string | an unique name, non-empty, case-sensitive |
| ttl | number | TTL time value (seconds) |
| expiredTime | number | TTL timestamp, default 0; format as javascript Date.now(), priority over 'ttl' |
| oncall | null or {url: string, data: any} | callback params |
| oncreate | null or {data: any} | if not null and TTL object is being created, callback with ocreate.data |
| arrayMode | boolean | callback data may be an array, default false |
| arrayLimit | number | if arrayMode is true, it is the maximum number, default 100 |
|  |  |  |


**RESPONSE**


request params fine: HTTP 200


request params bad: HTTP 400


service is exiting: HTTP 502


#### Callabck Format Table
| Params Condition | Callback |
| --- | --- |
| arrayMode=false and data=null | http.GET(backURL) |
| arrayMode=false and data≠null | http.POST(backURL, data) |
| arrayMode=true | http.POST(backURL, [data1, data2, ...]) |
|  |  |


### clearTimeout
| Field | Type | Description |
| --- | --- | --- |
| name | string | the name, non-empty, case-sensitive |
|  |  |  |

**RESPONSE**


request params fine: HTTP 200


request params bad: HTTP 400


service is exiting: HTTP 502
