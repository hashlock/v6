---
parent_category: alerts
parent_category_label: Alerts

path: '{{ site.version_url_prefix_request }}/alerts/{alertId}'
title: 'Alert Detail'
type: GET

sortorder: 3
category-sortorder: 6
layout: null
---

Returns details about an alert.

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `aid={accountId}` optional and requires the user to be assigned to the target account, specifies the account context of the request, obtained from the `/accounts` endpoint.  Specifying this parameter without the user to be assigned to the target account will result in an error response. See [Account Context][overview-accountcontext] for more information

### Request

* no request body

### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/alerts/34203.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

### Response

Sends back detailed information about a specific alertId.  If the alertId doesn't exist or is inaccessible by your account, an empty response will be returned.

Field | Data Type | Units | Notes
:------------|-------------|-------------|-------------|
alertId | integer | n/a | unique ID of the alert; each alert occurrence is assigned a new unique ID
testId | integer | n/a | unique ID of the test (see `/tests/{testId}` endpoint for more detail)
ruleId | integer | n/a | unique ID of the alert rule (see `/alert-rules` endpoint for more detail)
testName | string | n/a | name of the test
active | integer | n/a | 0 for inactive, 1 for active, 2 for disabled. Alert is disabled if either alert rule itself has been deleted or the test it is applied to has been disabled, deleted, disabled alerting, or disassociated the alert rule from the test
ruleExpression | string | n/a | string expression of alert rule
dateStart | dateTime | yyyy-MM-dd hh:mm:ss | the date/time where an alert rule was triggered
dateEnd | dateTime | yyyy-MM-dd hh:mm:ss | the date/time where the alert was marked as cleared
violationCount | integer | n/a | number of sources currently meeting the alert criteria
ruleName | string | n/a | name of the alert rule
permalink | string | n/a | hyperlink to alerts list, with row expanded
type | string | n/a | type of alert being triggered
agents | array of agent objects | n/a | array of monitors where the alert has at some point been active since the point that the alert was triggered.  Not shown on BGP alerts.
monitors | array of monitor objects | n/a | array of monitors where the alert has at some point been active since the point that the alert was triggered.  Only shown on BGP alerts.
apiLinks | array of links | n/a | list of hyperlinks to other areas of the API


Agent and Monitor objects (see above) reflect the following content:

Field | Data Type | Units | Notes
:------------|-------------|-------------|-------------|
dateStart | dateTime | yyyy-MM-dd hh:mm:ss | reflects the date that the source began reporting a measurement that exceeded the alert rule's threshold
dateEnd | dateTime | yyyy-MM-dd hh:mm:ss | reflects the earlier of the date that the alert was cleared, or the source reported a measurement that was under the alert rule's threshold
active | integer | n/a | if the particular source is alerting when the API is queried, this flag will be set to 1.  After an alert has cleared, this flag (regardless of the source's metrics) will be set to 0, even if the particular source has not cleared the alert rule.  If the rule has been subsequently disabled, the alert value will be set to 2.
metricsAtStart | string | n/a | string representation of the metric at the time that the source began alerting.  Note that the alert start and dateStart for a particular source do not need to be the same, as sources may change alerting status throughout an alert's lifecycle
metricsAtEnd | string | n/a | string representation of the metric or metrics being considered in the alert rule at the point that the alert was cleared.  If the alert is not yet cleared, this field reflects the last round of data gathered from the source.
agentId/monitorId | integer | n/a | unique ID of agent or monitor violating the alert rule.  See `/agents` or `/bgp-monitors` for more detail
agentName/monitorName | string | n/a | display name of the agent or monitor violating the alert rule
permalink | string | n/a | hyperlink to alerts list, with row expanded


#### Header

```HTTP/1.1 200 OK
Date: Thu, 07 Nov 2013 07:32:48 GMT
Server: Apache/2.2.22 (Ubuntu)
Transfer-Encoding: chunked
Content-Type: application/json```

#### Body

```{
    "alert": [
        {
            "ruleId": 172,
            "alertId": 34203,
            "testId": 5176,
            "testName": "API created test",
            "active": 1,
            "ruleExpression": "Error type is not \"None\"",
            "dateStart": "2014-03-24 19:02:00",
            "violationCount": 2,
            "ruleName": "Default HTTP Alert Rule",
            "permalink": "https://app.thousandeyes.com/alerts/list?__a=11&alertId=34203",
            "type": "HTTP Server",
            "agents": [
                {
                    "dateStart": "2014-03-24 19:02:00",
                    "active": 1,
                    "metricsAtStart": "Error type: \"DNS\"",
                    "metricsAtEnd": "Error type: \"DNS\"",
                    "agentId": 107,
                    "agentName": "Vancouver, Canada",
                    "permalink": "https://app.thousandeyes.com/web/http-server?__a=11&testId=5176&roundId=1395699129&agentId=107"
                },
                {
                    "dateStart": "2014-03-24 19:01:48",
                    "active": 1,
                    "metricsAtStart": "Error type: \"DNS\"",
                    "metricsAtEnd": "Error type: \"DNS\"",
                    "agentId": 108,
                    "agentName": "Boston, MA",
                    "permalink": "https://app.thousandeyes.com/web/http-server?__a=11&testId=5176&roundId=1395699129&agentId=108"
                }
            ],
            "apiLinks": [...]
        }
    ]
}```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
