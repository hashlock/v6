---
parent_category: reports
parent_category_label: Reports

path: '{{ site.version_url_prefix_request }}/report-snapshots'
title: 'Report snapshots list'

sortorder: 10
category-sortorder: 55
type: GET

layout: null
---

{.inline-code}This endpoint returns a list of report snapshots configured in ThousandEyes in the context of the user's current account group. This endpoint requires the `View Snapshots` permission be assigned to the role of the user accessing this endpoint. Use this data to find a report snapshot in your account, which is then used in other endpoint to access aggregated data.

### Parameters

* `aid={aid}` optional and requires the user to be assigned to the target account group, specifies the account group context of the request, obtained from the `/account-groups` endpoint.  Specifying this parameter without the user being assigned to the target account will result in an error response. See [Account group context][overview-accountcontext] for more information

* `reportId={reportId}` optional parameter to filter the list of snapshots by report ID.

### Request

* no request body

### Example

`$ curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/report-snapshots.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

`$ curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/report-snapshots.json\?reportId\=1 \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

### Response

Sends back an array of report snapshots configured in the current account group. The response has the following format:

Field Name | Data Type | Notes
:----------|-----------|----------|
pages | page object | Object containing a pagination link to the next page: `next` 
reportSnapshots | array of report snapshot objects | See below

Each report snapshot object contains the following fields:

Field Name | Data Type | Notes
:----------|-----------|----------|
snapshotId | string | unique ID of the report snapshot
snapshotName | string | name of the report snapshot
createdDate | dateTime | the date/time when report snapshot was created
isScheduled | boolean | true if report snapshot was generated from a schedule
isShared | boolean | true if report snapshot is shared
accountId | integer | ID of the account group that the snapshot belongs to
timespan.startDate | dateTime | the date/time of beginning of report snapshot
timespan.duration | integer | duration of report snapshot in seconds
permalink | string | link to report snapshot in ThousandEyes Application
apiLinks | array of apiLink objects | a list of links which can be followed to pull more information
report | object | report this report snapshot is based upon.
report.reportId | string | unique ID of the report
report.title | string | title of the report
report.isBuiltIn | boolean | true for built-in reports, false for user-created reports
report.apiLinks | array of apiLink objects | a list of links which can be followed to pull more information
report.accountId | integer | ID of the account group that the report belongs to
report.createdBy | integer | ID of the user that created the report for which the snapshot was generated
report.modifiedBy | integer | ID of the user that last modified the report at the time of snapshot creation
report.modifiedDate | dateTime | the date/time when the report was last modified at the time the snapshot was created
permalink | string | link to report snapshot in ThousandEyes Application

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

```{
    "pages": {
        "current": 1
    },
    "reportSnapshots": [
        {
            "apiLinks": [...],
            "permalink": "https://api.thousandeyes.com/reports/snapshots/123__a=1",
            "createdDate": "2019-09-02 18:00:00",
            "report": {
                "apiLinks": [...],
                "reportId": "1",
                "title": "areport",
                "isBuiltIn": false,
                "accountId": 1,
                "createdBy": 2,
                "modifiedBy": 3,
                "modifiedDate": "2019-09-01 19:00:00",
            },
            "isScheduled": true,
            "isShared": false,
            "snapshotId": "123",
            "accountId": 1,
            "snapshotName": "HTTP Server Report Snapshot",
            "timeSpan": {
                "duration": 604800,
                "startDate": "2019-09-01 00:00:00"
            }
        },
        ...
    ]
}```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
