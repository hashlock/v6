---
parent_category: reports
parent_category_label: Reports

path: '{{ site.version_url_prefix_request }}/report-snapshots/create'
title: 'Creating a report snapshot'

sortorder: 13
category-sortorder: 55
type: POST

layout: null
---

{.inline-code}This endpoint creates a new report snapshot for the account group that the user belongs to. This endpoint requires the `Edit Snapshots` permission be assigned to the role of the user accessing this endpoint.

### Parameters

* `aid={aid}` optional and requires the user to be assigned to the target account group, specifies the account group context of the request, obtained from the `/account-groups` endpoint.  Specifying this parameter without the user being assigned to the target account will result in an error response. See [Account group context][overview-accountcontext] for more information

### Request

The endpoint expects a JSON object with the following fields:

Field Name | Data Type | Notes
:----------|-----------|----------|
displayName | string | The name of the snapshot. Does not have to be unique.
from | dateTime | The date and time to start aggregating data
to | string | The date and time to end aggregating data
reportId | string | The ID of the report to generate a snapshot from

### Example

`$ curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/report-snapshots/create \
  -d '{
    "displayName": "snapshot from API",
    "from": "2019-09-02 18:00:00",
    "to": "2019-09-03 18:00:00",
    "reportId": "123"
}' \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

### Response

Sends back the id of the created report snapshot:

Field Name | Data Type | Notes
:----------|-----------|----------|
snapshotId | string | unique ID of the report snapshot that is being generated

#### Header

```HTTP/1.1 200 OK
Server: nginx
Date: Mon, 02 Sep 2019 18:00:00 GMT
Content-Type: application/json;charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Cache-Control: no-store
X-Organization-Rate-Limit-Limit: 970
X-Organization-Rate-Limit-Remaining: 968
X-Organization-Rate-Limit-Reset: 1492076340
Strict-Transport-Security: max-age=31536000
X-Server-Name: 1-2```

#### Body

```
{
    "snapshotId": "123"
}
```
For error responses, see the [response status codes documentation][overview-responsestatuscodes].
