---
parent_category: reports
parent_category_label: Reports

path: '{{ site.version_url_prefix_request }}/reports/{reportId}/{widgetId}'
title: 'Report widget data'

sortorder: 3
category-sortorder: 55
type: GET

layout: null
---

This endpoint returns actual metrics used in the generation of the reports shown. Unlike the metadata options, this endpoint accepts parameters for a time range shown in the data, which defaults to 7 days.

### Parameters

* `window=[0-9]+[smhdw]?` specifies a window of time for the result set. See [Time Ranges][overview-timerange] for more information.
* `from=YYYY-mm-ddTHH:MM:SS&to=YYYY-mm-ddTHH:MM:SS` specifies an explicit start (and optionally, end) for your range of data. See [Time Ranges][overview-timerange] for more information.
* `aid={aid}` optional, changes the account group context of the current request. If an invalid account group ID is specified as a parameter, the response will come back as an HTTP/400 error.
* `{reportId}` the ID of the report you’re interested in.
* `{widgetId}` the ID of the widget for which to retrieve data.

### Request

* no request body

### Example

`$ curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/reports/2/vlvb1.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

### Response

Sends back a list of data elements associated with the report. The data components are aggregated by our services, then reported back in bins: each bin represents a point on the chart: the granularity of the points shown in the chart is based on the data range shown in the report. Bin sizes are based on the table below:

Report time range   |  Bin size
:-------------------|-------------|
 up to 1 day        |  5 minutes
1 day - 14 days    |   1 hour
15 days - 30 days   |   2 hours
31 days - 50 days   |   4 hours
51 days - 93 days   |   6 hours

If your data is grouped, a value will be shown for each data grouping for each bin.

Corresponding data is returned according to the following fields:

Field Name | Data Type | Notes
:----------|-----------|----------|
dateFrom | dateTime | the start of the data shown in the API output
dateTo | dateTime | end of the data window shown in the API output
binSize | integer | duration of each bin
groupLabels | array of group labels | see groupLabel fields below
data.points | array of data points | see data point below
data.cards | array of card data points | see card data point below (Only used for Number Widget)
data.columns | array of multimetric column points | see multimetric column point below (Only used for Multi Metric Table Widget)
status | string | Message for not fully configured widget or no data


Data Point fields:

Field Name | Data Type | Notes
:----------|-----------|----------|
timestamp | integer | timestamp of the aggregated data point
numberOfDataPoints | integer | number of test data points aggregated into the widget data point
value | integer | aggregated value
groups | array of group property and value | this corresponds to the groups used for the aggregation

Card Data Point fields:

Field Name | Data Type | Notes
:----------|-----------|----------|
cardId | integer | unique ID of card
dateFrom | dateTime | the start of the data shown in the API output
dateTo | dateTime | end of the data window shown in the API output
previousValue | double | previous value if `compareToPreviousValue` was set to true in configuration
binSize | integer | duration of each bin
timestamp | integer | timestamp of the aggregated point
numberOfDataPoints | integer | number of points aggregated into the datapoint
value | integer | aggregated value
status | string | Message for not fully configured card or no data

Multri Metric Column Data Point fields:

Field Name | Data Type | Notes
:----------|-----------|----------|
columnId | integer | unique ID of column
binSize | integer | duration of each bin
status | string | Message for not fully configured card or no data
points | array of Data Point | List of data points

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
X-Organization-Rate-Limit-Reset: 1492076520
Strict-Transport-Security: max-age=31536000
X-Server-Name: 1-2```

#### Body

###### Number Card Widget
```
{
    "groupLabels": [],
    "data": {
        "cards": [
            {
                "cardId": "lrxxr",
                "dateFrom": "2019-08-28 18:00:00",
                "dateTo": "2019-09-04 18:00:00",
                "binSize": 3600,
                "timestamp": 1567616400,
                "numberOfDataPoints": 24192,
                "value": 100.0
            }
        ]
    }
}
```


###### Multi Metric Column Widget
```
{
    "dateFrom": "2019-08-28 19:00:00",
    "dateTo": "2019-09-04 19:00:00",
    "groupLabels": [],
    "data": {
        "columns": [
            {
                "columnId": "938to",
                "binSize": 3600,
                "points": [
                    {
                        "timestamp": 1567620000,
                        "numberOfDataPoints": 456189,
                        "value": 100.0,
                        "groups": []
                    }
                ]
            },
            {
                "columnId": "qqd2w",
                "binSize": 3600,
                "points": [
                    {
                        "timestamp": 1567620000,
                        "numberOfDataPoints": 4863654,
                        "value": 4715.388999999999,
                        "groups": []
                    }
                ]
            }
        ]
    }
}
```

###### Other widgets
```
{
    "dateFrom": "2019-08-28 19:00:00",
    "dateTo": "2019-09-04 19:00:00",
    "groupLabels": [
        {
            "groupProperty": "Agents",
            "groupLabels": [
                {
                    "groupId": "1",
                    "groupLabel": "Singapore"
                }
            ]
        }
    ],
    "binSize": 3600,
    "data": {
        "points": [
            {
                "timestamp": 1567620000,
                "numberOfDataPoints": 23304,
                "value": 100.00000833068279,
                "groups": [
                    {
                        "groupProperty": "Agents",
                        "groupValue": "1"
                    }
                ]
            }
        ]
    }
}
```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
