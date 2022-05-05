---
parent_category: dashboards
parent_category_label: Dashboards

path: '{{ site.version_url_prefix_request }}/dashboards/{dashboardId}/{widgetId}'
title: 'Dashboard widget data'

sortorder: 3
category-sortorder: 55
type: GET

layout: null
---

{.inline-code}This endpoint returns the raw data shown in a particular widget of a given dashboard.

### Optional (Querystring) Parameters
* `aid={aid}` optional, changes the account group context of the current request. If an invalid account group ID is specified as a parameter, the response will come back as an HTTP/400 error.
* `rows={integer}` optional, the number of elements to receive per page. Only available for Alert List and Test Table Widget.
* `page={integer}` optional, specifies the page number for the data response. Only available in Test Table widget.
* `sortProperty={string}` optional, specifies the property to sort by. Only available for Alert List and Test Table Widget. See below for accepted values for each widget.
* `sortDirection={string}` optional, specifies the direction to sort by. Only available for Alert List and Test Table Widget. The possible values are `Ascending` or `Descending`. The default value is `Descending`.

### Request
* `{dashboardId}` the ID of the dashboard you’re interested in.
* `{widgetId}` the ID of the widget for which to retrieve data.

* no request body

### Response

Sends back a list of data elements associated with the dashboard. For Report widgets refer to the Reports page in the documentation. For Dashboard widgets, the data is returned with the following fields:

Field Name | Data Type | Notes
:----------|-----------|----------|
dateFrom | dateTime | the start of the data shown in the API output
dateTo | dateTime | end of the data window shown in the API output
binSize | integer | duration of each bin (if applicable)
groupLabels | array of group labels | see groupLabel fields below (if widget groups by any property)
data.points | array of data points | see data point below
data | array of non-aggregated points | see below for Test Table, Alert Grid, Alert List and Agent Status
status | string | Message for not fully configured widget or no data


The fields in **data.point** are the following:

Field Name | Data Type | Notes
:----------|-----------|----------|
timestamp | integer | timestamp of the aggregated data point
numberOfDataPoints | integer | number of test data points aggregated into the widget data point
value | float | aggregated value
groups | array of group property and value | this corresponds to the groups used for the aggregation

The fields returned in **data** are dependent on the widget:



**Agent Status**

Field Name | Data Type | Notes
:----------|-----------|----------|
summary | object | object with a summary of how many agents are online, offline and disabled
agents | array | list of agents

Each agent in the agent status data response will contain these properties

Field Name | Data Type | Notes
:----------|-----------|----------|
agentId | string | ID of the agent
status | string | `ONLINE`, `OFFLINE` or `DISABLED`
ipInfo | object | information about the IP of the agent if `Enterprise` or operating system version for `Endpoint`
agentName | string | name of the agent
location | object | contains the latitude, longitude and locationName of the agent



**Alert List**

This widget accepts the following query params to paginate the response:
* `rows={integer}` optional, the number of alerts in the response. The default value is the `limitBy` configured for the widget.
* `sortProperty={string}` optional, specifies the property to sort by. The possible values are `alertStatus` or `startTime`. The default value is `alertStatus`


Field Name | Data Type | Notes
:----------|-----------|----------|
totalAlerts | integer | total number of alerts that were active within the timespan configured
activeAlerts | integer | number of the alerts that are still active
alerts | array | array of alerts

Each alert in the alert list data response will contain these properties

Field Name | Data Type | Notes
:----------|-----------|----------|
active | boolean | `true` or `false`
alertId | integer | id of the alert
testId | integer | id of the test
sourceId | integer | id of the agent, monitor or device producing the alert
sourceName | string | name of the agent, monitor or device producing the alert
alertRule | string | name of the alert rule that this alert belongs to
alertType | string | name of the alert type
startTime | string | date when the alert was first active
durationInSeconds | integer | number of seconds the alert was active. If it's still active, this number will increase every second.



**Alert Grid**

Field Name | Data Type | Notes
:----------|-----------|----------|
alertType | string | type of alerts: `Agent` or `BGP`
totalAlerts | integer | total number of alerts that were active in the last 24 hours
numActiveAlerts | integer | number of the alerts that are active
numClearedOrDisabledAlertsInLastDay | integer | number of alerts that were cleared or disabled in the last 24 hours
alerts | array | array of source alerts

Each source alert in the alert grid data response will contain these properties

Field Name | Data Type | Notes
:----------|-----------|----------|
isActive | boolean | `true` or `false`
alertId | integer | id of the alert
testId | integer | id of the test
prefixId | integer | id of the monitor prefix
sourceId | integer | id of the agent or monitor or device producing the alert
alertType | string | name of the alert type
startTime | string | date when the alert was first active
duration | integer | number of seconds the alert was active. Max is 24 hours
location | string | name of the location of the agent or monitor producing the alert



**Test Table**

This widget accepts the following query params to paginate the response:
* `rows={integer}` optional, the number of alerts in the response. The default value is 10.
* `page={integer}` optional, specifies the page number for the data response. The default value is 1.
* `sortProperty={string}` optional, specifies the property to sort by. The possible values are `alertStatus`, `testName` or `testType`. The ordering may differ from the web application. The default value is `alertStatus`.

Field Name | Data Type | Notes
:----------|-----------|----------|
tests | array | list of test

Each test in the test table data response will contain these properties

Field Name | Data Type | Notes
:----------|-----------|----------|
testId | integer | id of the test
testName | string | name of the test
target | string | target configured in the test
testType | string | type of test
alertStatus | integer | number of active alerts for the test
isShared | boolean | flag set to true if the test has been shared
graphlets | array | list of timeseries points for test metrics in the last 12 hours 

Each graphlet object will contain these properties

Field Name | Data Type | Notes
:----------|-----------|----------|
metric | string | name of the metric 
testId | integer | id of the test
points | array | list of `x` and `y` datapoints where `x` is the timestamp of the point and `y` is the value

### Example

`$ curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/dashboards/2/vlvb1.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

#### Header

```HTTP/1.1 200 OK
Server: nginx
Date: Mon, 02 Sep 2019 18:00:00 GMT
Content-Type: application/json;charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Cache-Control: no-store
X-Organization-Rate-Limit-Limit: 240
X-Organization-Rate-Limit-Remaining: 235
X-Organization-Rate-Limit-Reset: 1492076520
Strict-Transport-Security: max-age=31536000
X-Server-Name: 1-2```

#### Body

##### Color Grid
```
{
    "dateFrom": "2020-01-16 22:15:00",
    "dateTo": "2020-01-16 23:15:00",
    "groupLabels": [],
    "binSize": 300,
    "data": {
        "points": [
            {
                "timestamp": 1579216200,
                "numberOfDataPoints": 3270,
                "value": 344.00582535755046,
                "groups": [
                    {
                        "groupProperty": "Continents",
                        "groupValue": "AS"
                    }
                ]
            },
            {
                "timestamp": 1579216200,
                "numberOfDataPoints": 5082,
                "value": 156.37737466049668,
                "groups": [
                    {
                        "groupProperty": "Continents",
                        "groupValue": "EU"
                    }
                ]
            },
            ...
        ]
    }
}
```

##### Alert Grid
```
{
    "dateFrom": "2020-01-15 23:25:13",
    "dateTo": "2020-01-16 23:25:13",
    "binSize": 300,
    "data": {
        "alertGridData": {
            "alertType": "Agents",
            "alerts": [
                {
                    "testId": 68185,
                    "sourceId": 37,
                    "alertId": 1422994,
                    "isActive": true,
                    "alertType": "PathTrace",
                    "location": "Singapore",
                    "startTime": "2019-08-06 16:50:18",
                    "duration": 86400
                },
                {
                    "testId": 68185,
                    "sourceId": 813,
                    "alertId": 1422994,
                    "isActive": true,
                    "alertType": "PathTrace",
                    "location": "Tokyo, Japan",
                    "startTime": "2019-08-06 16:50:11",
                    "duration": 86400
                },
                ...
            ],
            "totalAlerts": 14,
            "numActiveAlerts": 14,
            "numClearedOrDisabledAlertsInLastDay": 0
        }
    }
}
```

##### Alert List
```
{
    "dateFrom": "2020-01-16 22:28:52",
    "dateTo": "2020-01-16 23:28:52",
    "data": {
        "alertsData": {
            "totalAlerts": 590,
            "activeAlerts": 483,
            "alerts": [
                {
                    "alertId": 2004945,
                    "testId": 56512,
                    "ruleId": 281724,
                    "alertSource": "AI Team Web Http Test 2",
                    "alertRule": "AI dynamic baseline verification 11-21",
                    "alertType": "Web - HTTP Server",
                    "startTime": "2020-01-16 23:28:05",
                    "durationInSeconds": 47,
                    "active": true
                },
                {
                    "alertId": 2004941,
                    "testId": 67035,
                    "ruleId": 202156,
                    "alertSource": "A2A uni CEA-13201 T10 Copy",
                    "alertRule": "Delay always on",
                    "alertType": "Network - Path Trace",
                    "startTime": "2020-01-16 23:26:07",
                    "durationInSeconds": 165,
                    "active": true
                },
                ...
            ]
        }
    }
}
```

##### Test Table
```
{
    "pages": {
        "next": "https://api.thousandeyes.com/v7/dashboards/5e2f669636256d0053617499/cogto?page=2"
    },
    "dateFrom": "2020-01-30 17:04:10",
    "dateTo": "2020-01-31 05:04:10",
    "binSize": 300,
    "data": {
        "tests": [
            {
                "testId": 68256,
                "testName": "$$$",
                "target": "https://www.google.com",
                "testType": "Web - HTTP Server",
                "alertStatus": 24,
                "isShared": true,
                "graphlets": [
                    {
                        "metric": "Availability",
                        "testId": 68256,
                        "points": [
                            {
                                "x": 1580403900,
                                "y": 0.0
                            },
                            ...
                        ]
                    },
                                        {
                        "metric": "Response Time",
                        "testId": 68256,
                        "points": [
                            {
                                "x": 1580406780,
                                "y": 128.249
                            },
                            ...
                        ]
                    }
                ]
            },
            ...
        ]
    }
}
```

##### Agent Status
```
{
    "dateTo": "2020-01-31 05:07:18",
    "binSize": 300,
    "data": {
        "summary": {
            "online": 10,
            "offline": 2,
            "disabled": 3
        },
        "agents": [
            {
                "agentId": "6522",
                "status": "OFFLINE",
                "ipInfo": {
                    "publicIp": "172.58.92.31",
                    "privateIp": "172.17.0.3"
                },
                "agentName": "0c3898000117",
                "location": {
                    "latitude": 37.77493,
                    "longitude": -122.41942,
                    "locationName": "San Francisco, California, US"
                }
            },
            ...
        ]
    }
}
```


For error responses, see the [response status codes documentation][overview-responsestatuscodes].
