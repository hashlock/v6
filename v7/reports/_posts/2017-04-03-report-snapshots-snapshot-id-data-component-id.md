---
parent_category: reports
parent_category_label: Reports

path: '{{ site.version_url_prefix_request }}/report-snapshots/{snapshotId}/{widgetId}'
title: 'Report snapshot data'

sortorder: 12
category-sortorder: 55
type: GET

layout: null
---

This endpoint returns actual metrics used in the generation of the report snapshot shown.

### Parameters

* `aid={aid}` optional, changes the account group context of the current request. If an invalid account group ID is specified as a parameter, the response will come back as an HTTP/400 error.
* `{snapshotId}` the ID of the report snapshot you're interested in.
* `{widgetId}` the ID of the widget for which to retrieve data.

### Request

* no request body

### Example

`$ curl https://api.thousandeyes.com{{ site.version_url_prefix_request}}/report-snapshots/60886ebb-2466-444d-bbd8-74d5ea1402d2/1234f.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

### Response

Sends back a list of data elements associated with the report snapshot. The data components are aggregated by our services, then reported back in bins: each bin represents a point on the chart: the granularity of the points shown in the chart is based on the data range shown in the report snapshot. Bin sizes are based on the table below:

Report time range   |  Bin size
:-------------------|-------------|
 up to 1 day        |  5 minutes
1 day - 14 days    |   1 hour
15 days - 30 days   |   2 hours
31 days - 50 days   |   4 hours
51 days - 93 days   |   6 hours

If your data is grouped, a value will be shown for each data grouping for each bin.

Corresponding data is returned according to the format for widget data. Check the endpoint to return widget data for reference.

#### Header

```HTTP/1.1 200 OK
Server: nginx
Date: Thu, 13 Apr 2017 14:29:51 GMT
Content-Type: application/json;charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Cache-Control: no-store
X-Organization-Rate-Limit-Limit: 970
X-Organization-Rate-Limit-Remaining: 969
X-Organization-Rate-Limit-Reset: 1492093800
Strict-Transport-Security: max-age=31536000
X-Server-Name: 1-2```

#### Body

```
{
    "dateFrom": "2019-07-24 16:00:00",
    "dateTo": "2019-07-31 16:00:00",
    "groupLabels": [
        {
            "groupProperty": "Countries",
            "groupLabels": [
                {
                    "groupId": "DE",
                    "groupLabel": "Germany"
                },
                {
                    "groupId": "AU",
                    "groupLabel": "Australia"
                },
                ...
            ]
        }
    ],
    "binSize": 3600,
    "data": {
        "points": [
            {
                "timestamp": 1563984000,
                "numberOfDataPoints": 62,
                "value": 569.0499070690524,
                "groups": [
                    {
                        "groupProperty": "Countries",
                        "groupValue": "AU"
                    }
                ]
            },
            ...
        ]
    }
}
```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
