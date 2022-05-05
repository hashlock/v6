---
parent_category: reports
parent_category_label: Reports

path: '{{ site.version_url_prefix_request }}/reports/{reportId}'
title: 'Report detail'

sortorder: 2
category-sortorder: 55
type: GET

layout: null
---

{.inline-code}This endpoint returns a list of widgets configured in reports configured in ThousandEyes. Seed this endpoint with a `reportId` found from the `/reports` endpoint. This endpoint requires the `View Reports` permission be assigned to the role of the user accessing this endpoint.

### Parameters

* `aid={aid}` optional and requires the user to be assigned to the target account group, specifies the account group context of the request, obtained from the `/account-groups` endpoint.  Specifying this parameter without the user being assigned to the target account will result in an error response. See [Account group context][overview-accountcontext] for more information
* `{reportId}` the ID of the report you're interested in.

### Request

* no request body


### Example

`$ curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/reports/1.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

### Response

{.inline-code}Returns report metadata information and associated widget list. Each widget will show a list of data components shown in the widget. Each data component includes a `metric`, `measure`, and grouping information, as well as details around any embeddings which exist for the widget:

Field Name | Data Type | Notes
:----------|-----------|----------|
reportId | string | unique ID of the report
title | string | title of the report
createdBy | integer | ID of the user who created the report, as returned by the `{{ site.version_url_prefix_request }}/users` API endpoint
modifiedBy | integer | ID of the user who last modified the report, as returned by the `{{ site.version_url_prefix_request }}/users` API endpoint
modifiedDate | string | YYYY-MM-DD HH:mm:ss formatted date of last report modification time, shown in UTC
description | string | description of the report
isBuiltIn | boolean | true for built-in reports, false for user-created reports
accountId | integer | ID of the account that the report belongs to
isDefaultForUser | boolean | true if this report is the default for the user, false otherwise
isDefaultForAccount | boolean | true if this report is the default for the account group, false otherwise
widgets | array | an array of widget objects
apiLinks | array of apiLink objects | a list of links which can be followed to pull more information

Each widget object has the following common fields (Except Number Widget):

Field Name | Data Type | Notes
:----------|-----------|----------|
id | string | unique widget identifier inside the report. Note: Only unique within the same report, not across all reports
type | string | widget type: `Table`, `Multi Metric Table`, `Map`, `Number`, `Pie Chart`, `Time Series: Line`, `Time Series: Stacked Area`, `Box and Whiskers`, `Bar Chart: Stacked`, or `Bar Chart: Grouped`
visualMode | string | (optional, defaults to `Full`) Visual mode in the UI: `Full` or `Half screen`
fixedTimespan | string | (optional) fixed timespan to do aggregation, in seconds
title | string | (optional, defaults to widget type string) widget title
dataSource | string | data source of widget. Can be `Alerts`, `Cloud & Enterprise Agents`, `Devices`, `Endpoint Agents`, etc.
metricGroup | string | metric group of widget as it appears in the UI. Can be `Web - HTTP Server`, `Voice - SIP Server`, `Network - Agent to Agent`, etc. Note: May not be required in some cases
metric | string | metric of widget. Can be `Response Time`, `Total Error Count`, `Page Load Time`, `Marker Time`, `Packet Loss`, `Throughput`, etc.
direction | string | direction of agent to agent metric: `Source to Target`, `Target to Source`, or `Both Directions`. (Required for some metrics)
measure | object | measure configuration of the widget
isEmbedded | boolean | true if widget is marked as embedded, false otherwise
embedUrl | string | if widget is marked as embedded, `embedUrl` is provided
filters | array of filter mappings | (optional) where the widget is filtered, filters property is shown. Each filter mapping will map a filter name to a list of filtered values. Filter keys can be `Agents`, `Agent Labels`, `Tests`, `Monitors`, etc. The list for each key holds the IDs of the property, i.e. `testId`s, `agentId`s, etc. 
apiLink | string | link to the data of the widget


Each widget object will have some of these fields, depending on the widget type as they appear in the UI.

Field Name | Data Type | Notes
:----------|-----------|----------|
groupBy | string | Property name to group by. Can be `Tests`, `Test Labels`, `Agents`, `Countries`, etc.
rowGroupBy | string | Property name to group by on the rows.  Can be `Tests`, `Test Labels`, `Agents`, `Countries`, etc.
columnGroupBy | string | Property name to group by on the columns.  Can be `Tests`, `Test Labels`, `Agents`, `Countries`, etc.
axisGroupBy | string | Property name to group by on the axis. Can be `Tests`, `Test Labels`, `Agents`, `Countries`, etc.
sortBy | string | Property to sort by: `Value` or `Alphabetical`. Exception: Multi-metric table widget
sortDirection | string | Direction to sort by: `Ascending` or `Descending`
minScale | float | (optional) minimum scale configured in the widget
maxScale | float | (optional) maximum scale configured in the widget
isGeoMapPerTest | boolean | (optional) show only one test in each map for the Map widget
limit | integer | (optional) limit configured in the widget
showLabels | boolean | (optional, defaults to false) true if show labels checkbox is checked, false otherwise
isHorizontalBarChart | boolean | (optional, defaults to false) true if direction of barchart is horizontal, false otherwise
compareToPreviousValue | boolean | (optional, defaults to false) Compare next value with previous value
multiMetricColumns | array of multrimetric column objects | array of multimetric column objects only applicable to multimetric column widget
numberCards | array of card objects | array of card objects only applicable to number widget
showTimeseriesOverallBaseline | boolean | (optional, defaults to false) Should show baseline. Only applicable to timeseries widget
isTimeseriesOneChartPerLine | boolean | (optional, defaults to false) Should show only one line for every timeseries chart. Only applicable to the timeseries widget.



Each measure object has the following fields:

Field Name | Data Type | Notes
:----------|-----------|----------|
type | string | measure type; `Maximum`, `Mean`, `Median`, `Minimum`, `nth Percentile`, `Standard Deviation`
percentileValue | float | (only for type `nth Percentile`) Percentile value

Each multimetric column object has the following fields:

Field Name | Data Type | Notes
:----------|-----------|----------|
id | string | unique widget identifier inside the report. Note: Only unique within the same report, not across all reports
dataSource | string | data source of widget. Can be `Alerts`, `Cloud & Enterprise Agents`, `Devices`, `Endpoint Agents`, etc.
metricGroup | string | metric group of widget as it appears in the UI. Can be `Web - HTTP Server`, `Voice - SIP Server`, `Network - Agent to Agent`, etc. Note: May not be required in some cases
metric | string | metric of widget. Can be `Response Time`, `Total Error Count`, `Page Load Time`, `Marker Time`, `Packet Loss`, `Throughput`, etc.

measure | object | measure configuration of the widget

Each card object has the following fields:

Field Name | Data Type | Notes
:----------|-----------|----------|
id | string | unique widget identifier inside the report. Note: Only unique within the same report, not across all reports
description | string | description of the card
dataSource | string | data source of widget. Can be `Alerts`, `Cloud & Enterprise Agents`, `Devices`, `Endpoint Agents`, etc.
metricGroup | string | metric group of widget as it appears in the UI. Can be `Web - HTTP Server`, `Voice - SIP Server`, `Network - Agent to Agent`, etc. Note: May not be required in some cases
metric | string | metric of widget. Can be `Response Time`, `Total Error Count`, `Page Load Time`, `Marker Time`, `Packet Loss`, `Throughput`, etc.
measure | object | measure configuration of the widget
compareToPreviousValue | boolean | (optional, defaults to false) Compare current value with previous value
fixedTimespan | string | (optional) fixed timespan to do aggregation in seconds
minScale | string | (optional) minimum scale configured in the widget
maxScale | string | (optional) maximum scale configured in the widget
filters | array of filter mappings | (optional) where the widget is filtered, filters property is shown. Each filter mapping will map a filter name to a list of filtered values. Filter keys can be `Agents`, `Agent Groups`, `Tests`, `Monitors`, etc. The list for each key holds the Ids of the property, i.e. test ids, agent ids, etc. 



#### Header

```HTTP/1.1 200 OK
Server: nginx
Date: Mon, 03 Sep 2019 18:00:00 GMT
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

```
{
    "reportId": "1",
    "title": "API Report",
    "isBuiltIn": false,
    "accountId": 1,
    "createdBy": 2,
    "modifiedBy": 3,
    "modifiedDate": "2019-09-03 18:00:00",
    "isDefaultForUser": true,
    "isDefaultForAccount": false,
    "widgets": [
        {
            "id": "abcd1",
            "type": "Multi Metric Table",
            "title": "Multi Metric Table",
            "visualMode": "Full",
            "apiLink": "https://api.thousandeyes.com/v7/reports/1/abcd1",
            "rowGroupBy": "Devices",
            "sortBy": "Default (Devices)",
            "sortDirection": "Ascending",
            "multiMetricColumns": [
                {
                    "id": "abcd2",
                    "dataSource": "Devices",
                    "metric": "Availability",
                    "measure": {
                        "type": "% Active"
                    }
                },
                {
                    "id": "abcd3",
                    "dataSource": "Devices",
                    "metric": "Availability",
                    "measure": {
                        "type": "% Inactive"
                    }
                }
            ]
        },
        {
            "id": "zyxwb",
            "type": "Map",
            "title": "Map: Default",
            "visualMode": "Full",
            "dataSource": "Cloud & Enterprise Agents",
            "metricGroup": "Network - Agent to Server",
            "metric": "Available Bandwidth",
            "measure": {
                "type": "Maximum"
            },
            "filters": {
                "Tests": [
                    12345
                ]
            },
            "apiLink": "https://api.thousandeyes.com/v7/reports/1/zyxwb",
            "groupBy": "Agents",
            "sortBy": "Value",
            "sortDirection": "Ascending"
        },
        {
            "id": "jv9wo",
            "type": "Time Series: Line",
            "title": "Time Series: Asia Pacific",
            "visualMode": "Full",
            "embedUrl": "https://embed.thousandeyes.com/e/icqzjuhdah",
            "isEmbedded": true,
            "dataSource": "Cloud & Enterprise Agents",
            "metricGroup": "Network - Agent to Server",
            "metric": "Latency",
            "measure": {
                "type": "Maximum"
            },
            "filters": {
                "Tests": [
                    12345,
                    12346
                ]
            },
            "apiLink": "https://api.thousandeyes.com/v7/reports/1/jv9wo",
            "groupBy": "Tests",
            "isTimeseriesOneChartPerLine": false
        }
    ],
    "apiLinks": [
        {
            "rel": "self",
            "href": "https://api.thousandeyes.com/v7/reports/1"
        },
        {
            "rel": "snapshots",
            "href": "https://api.thousandeyes.com/v7/report-snapshots?reportId=1"
        }
    ]
}
```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
