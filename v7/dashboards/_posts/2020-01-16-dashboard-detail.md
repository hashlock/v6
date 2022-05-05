---
parent_category: dashboards
parent_category_label: Dashboards

path: '{{ site.version_url_prefix_request }}/dashboards/{dashboardId}'
title: 'Dashboard detail'

sortorder: 2
category-sortorder: 55
type: GET

layout: null
---

{.inline-code}This endpoint returns a list of widgets configured in a ThousandEyes dashboard in addition to the dashboard's metadata.

### Optional (Querystring) Parameters

* `aid={aid}` optional and requires the user to be assigned to the target account group. Specifies the account group context of the request, obtained from the `/account-groups` endpoint.  Specifying this parameter without the user being assigned to the target account will result in an error response. See [Account group context][overview-accountcontext] for more information


### Request
* `{dashboardId}` the ID of the desired dashboard which can be found from the `/dashboards` endpoint.
* no request body


### Response

{.inline-code}Returns dashboard metadata information and associated widget list. Each widget will contain these properties in its configuration.

Field Name | Data Type | Notes
:----------|-----------|----------|
dashboardId | string | unique ID of the dashboard
title | string | title of the dashboard
createdBy | integer | ID of the user who created the dashboard, as returned by the `{{ site.version_url_prefix_request }}/users` API endpoint
modifiedBy | integer | ID of the user who last modified the dashboard, as returned by the `{{ site.version_url_prefix_request }}/users` API endpoint
modifiedDate | string | YYYY-MM-DD HH:mm:ss formatted date of last dashboard modification time, shown in UTC
description | string | description of the dashboard
isBuiltIn | boolean | true for built-in reports, false for user-created reports
accountId | integer | ID of the account that the dashboard belongs to
isPrivate | boolean | true if this dashboard is private
isDefaultForUser | boolean | true if this dashboard is the default for the user, false otherwise
isDefaultForAccount | boolean | true if this dashboard is the default for the account group, false otherwise
widgets | array | an array of widget objects
apiLink | string | link to the dashboard

The data returned for every widget will depend on the widget type. For Report widgets, refer to the Reports section of the documentation. Dashboard widgets will contain the following common properties:

Field Name | Data Type | Notes
:----------|-----------|----------|
id | string | unique widget identifier inside the report. Note: Only unique within the same report, not across all reports
type | string | widget type: `Agent Status`, `Alert List`, `Color Grid`, `Alert Grid`, `Test Table`
visualMode | string | (optional, defaults to `Full`) Visual mode in the UI: `Full` or `Half screen`
fixedTimespan | object | (optional) fixed timespan to do aggregation over. Specified with a value and unit (Days, Hours, or Minutes).
title | string | (optional, defaults to widget type string) widget title
isEmbedded | boolean | true if widget is marked as embedded, false otherwise
embedUrl | string | if widget is marked as embedded, `embedUrl` is provided
apiLink | string | link to the data of the widget

Each dashboard widget object will have some of these fields, depending on the widget type as they appear in the UI.

**Agent Status**

Field Name | Data Type | Notes
:----------|-----------|----------|
agents | string | type of agent: `Enterprise` or `Endpoint`
show | string | ownership of agents: `Owned Agents` or `All Assigned Agents`

**Alert List**

Field Name | Data Type | Notes
:----------|-----------|----------|
alertTypes | array | list of alert types configured in the widget. An empty list means all alert types.
limitTo | integer | limit the number of alerts to be shown in the widget
activeWithin | object | timespan during which alerts had to be active to be shown in the widget

**Color Grid**

Field Name | Data Type | Notes
:----------|-----------|----------|
dataSource | string | data source of widget. Can be `Alerts`, `Cloud & Enterprise Agents`, `Devices`, `Endpoint Agents`, etc.
metricGroup | string | metric group of widget as it appears in the UI. Can be `Web - HTTP Server`, `Voice - SIP Server`, `Network - Agent to Agent`, etc. Note: May not be required in some cases
metric | string | metric of widget. Can be `Response Time`, `Total Error Count`, `Page Load Time`, `Marker Time`, `Packet Loss`, `Throughput`, etc.
direction | string | direction of agent to agent metric: `Source to Target`, `Target to Source`, or `Both Directions`. (Required for some metrics)
measure | object | measure configuration of the widget
cards | string | Property name to aggregate by. Can be `Tests`, `Test Labels`, `Continents`, `Countries`, etc.
groupCardsBy | string | Property name to group cards by. Can be `Tests`, `Test Labels`, `Continents`, `Countries`, etc.
minScale | float | (optional) minimum scale configured in the widget
maxScale | float | (optional) maximum scale configured in the widget
unit | string | (optional) Typically this is automatically configured. Possible options: `Kbps`, `Mbps`, `Gbps`, `Kpps`, `Mpps`, `Gpps`
columns | integer | (optional) number of columns: 1 or 2
limit | integer | (optional) limit configured in the widget
sortBy | string | Property to sort by: `Value` or `Alphabetical`. Exception: Multi-metric table widget
sortDirection | string | Direction to sort by: `Ascending` or `Descending`
filters | array of filter mappings | (optional) where the widget is filtered, filters property is shown. Each filter mapping will map a filter name to a list of filtered values. Filter keys can be `Agents`, `Agent Labels`, `Tests`, `Monitors`, etc. The list for each key holds the IDs of the property, i.e. `testId`s, `agentId`s, etc. 

**Test Table** 

Field Name | Data Type | Notes
:----------|-----------|----------|
filter | object | multisearch filter (see below). Valid keys for filters: `Anything`, `Test Name`, `Target`, `Test ID`, `Test Type`, `Label ID`
exclude | object | multisearch filter (see below)

**Alert Grid**

Field Name | Data Type | Notes
:----------|-----------|----------|
filter | object | multisearch filter (see below). Valid keys for filters: `Anything`, `Test Name`, `Test Type`, `Agent Name`
exclude | object | multisearch filter (see below)

**Multi Search Filter** 

Field Name | Data Type | Notes
:----------|-----------|----------|
filters | list | list of object pairs with a `key` and a `value` as shown in the app.
type | string | string that defines the logical operator to be applied to filters: `all` or `any`

### Example

`$ curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/dashboards/1.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`


#### Header

```HTTP/1.1 200 OK
Server: nginx
Date: Mon, 03 Sep 2019 18:00:00 GMT
Content-Type: application/json;charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Cache-Control: no-store
X-Organization-Rate-Limit-Limit: 240
X-Organization-Rate-Limit-Remaining: 232
X-Organization-Rate-Limit-Reset: 1492076520
Strict-Transport-Security: max-age=31536000
X-Server-Name: 1-2```

#### Body

```
{
    "dashboardId": "5e1f7a99143ae6004fdc3bb4",
    "title": "Dashboard API",
    "isBuiltIn": false,
    "accountId": 11,
    "createdBy": 6103,
    "modifiedBy": 6103,
    "modifiedDate": "2020-01-16 21:09:09",
    "isPrivate": false,
    "isDefaultForUser": true,
    "isDefaultForAccount": false,
    "widgets": [
        {
            "id": "xtzbr",
            "type": "Color Grid",
            "title": "Color Grid",
            "visualMode": "Full",
            "dataSource": "Cloud & Enterprise Agents",
            "metricGroup": "Web - HTTP Server",
            "metric": "Response Time",
            "measure": {
                "type": "Mean"
            },
            "fixedTimespan": {
                "value": 1,
                "unit": "Hours"
            },
            "apiLink": "https://api.thousandeyes.com/v7/dashboards/5e1f7a99143ae6004fdc3bb4/xtzbr",
            "cards": "Tests",
            "groupCardsBy": "Agents",
            "columns": 1,
            "sortBy": "Value",
            "sortDirection": "Ascending"
        },
        {
            "id": "vxqve",
            "type": "Agent Status",
            "title": "Agent Status",
            "visualMode": "Full",
            "apiLink": "https://api.thousandeyes.com/v7/dashboards/5e1f7a99143ae6004fdc3bb4/vxqve",
            "agents": "Enterprise Agents",
            "show": "Owned Agents"
        },
        {
            "id": "0kepj",
            "type": "Alert List",
            "title": "Alert List",
            "visualMode": "Full",
            "fixedTimespan": {
                "value": 2,
                "unit": "Days"
            },
            "apiLink": "https://api.thousandeyes.com/v7/dashboards/5e1f7a99143ae6004fdc3bb4/0kepj",
            "alertTypes": [
                "Voice - RTP Stream",
                "DNS+ Server",
                "Network - End-to-End (Server)",
                "EndpointWeb - HTTP Server",
                "Web - FTP Server",
                "Voice - SIP Server",
                "Device Interface",
                "Web - Page Load",
                "DNS Server",
                "DNS+ Domain",
                "Web - Transaction",
                "Endpoint - End-to-End (Server)",
                "Network - End-to-End (Agent)",
                "Network - Path Trace",
                "Endpoint - Path Trace",
                "DNSSEC",
                "Web - HTTP Server",
                "Device",
                "Web - Transaction (Classic)",
                "DNS Trace",
                "Routing - BGP"
            ],
            "limitTo": 10,
            "activeWithin": 3600
        },
        {
            "id": "ghhli",
            "type": "Alert Grid",
            "title": "Alert Grid",
            "visualMode": "Full",
            "fixedTimespan": {
                "value": 5,
                "unit": "Minutes"
            },
            "apiLink": "https://api.thousandeyes.com/v7/dashboards/5e1f7a99143ae6004fdc3bb4/ghhli",
            "alertType": "Agents",
            "filter": {
                "filters": [
                    {
                        "key": "Test Type",
                        "value": "DnsServer"
                    }
                ],
                "type": "all"
            },
            "exclude": {
                "filters": [
                    {
                        "key": "Anything",
                        "value": "Some string"
                    }
                ],
                "type": "any"
            }
        },
        {
            "id": "o6mjm",
            "type": "Test Table",
            "title": "Tests",
            "visualMode": "Full",
            "fixedTimespan": {
                "value": 12,
                "unit": "Hours"
            },
            "apiLink": "https://api.thousandeyes.com/v7/dashboards/5e1f7a99143ae6004fdc3bb4/o6mjm",
            "filter": {
                "filters": [
                    {
                        "key": "Test Name",
                        "value": "123"
                    }
                ],
                "type": "all"
            },
            "exclude": {
                "filters": [
                    {
                        "key": "Label ID",
                        "value": "25236"
                    }
                ],
                "type": "all"
            }
        }
    ],
    "apiLink": "https://api.thousandeyes.com/v7/dashboards/5e1f7a99143ae6004fdc3bb4"
}
```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
