---
parent_category: alerts
parent_category_label: Alerts

path: '{{ site.version_url_prefix_request }}/alert-rules'
title: 'Alert Rules'
type: GET

sortorder: 1
category-sortorder: 6
layout: null
---

Returns a list of all alert rules configured under your account in ThousandEyes.

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `aid={accountId}` optional and requires the user to be assigned to the target account, specifies the account context of the request, obtained from the `/accounts` endpoint.  Specifying this parameter without the user to be assigned to the target account will result in an error response. See [Account Context][overview-accountcontext] for more information

### Request

* no request body

### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/alert-rules.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

### Response

Sends back an array of alert rules, indicating ruleID, whether or not the alert is enabled, recipient lists, and the rule criteria and clearing logic.  Default rules for each type are indicated with a bit response (1 or 0); default alert rules are assigned by default to each type of test to which they apply.

Field | Data Type | Units | Notes
:------------|-------------|-------------|-------------|
ruleId | integer | n/a | unique ID of the alert rule
ruleName | string | n/a | name of the alert rule
notes | string | n/a | additional content sent to notification recipients in alert emails
expression | string | n/a | string expression of alert rule
notifyOnClear | boolean | n/a | 1 to send notification when alert clears
roundsBeforeTrigger | integer | n/a | Valid options are 1-4; indicates the number of rounds where the criteria are met before triggering an alert
default | boolean | n/a | Alert rules allow up to 1 alert rule to be selected as a default for each type.  By checking the default option, this alert rule will be automatically included on subsequently created tests that test a metric used in alerting here
recipient | array of email addresses | n/a | notification recipients are specified in an array of email addresses
alertType | string | n/a | type of alert rule, as determined by metric selection
minimumSources | integer | n/a | the minimum number of agents or monitors that must meet the specified criteria in order to trigger the alert

#### Header

```HTTP/1.1 200 OK
Date: Thu, 07 Nov 2013 07:32:48 GMT
Server: Apache/2.2.22 (Ubuntu)
Transfer-Encoding: chunked
Content-Type: application/json```

#### Body

```{
    "alertRules": [
        {
            "ruleId": 322,
            "ruleName": "Default BGP Alert Rule",
            "notes": "",
            "expression": "((reachability < 100%))",
            "notifyOnClear": 0,
            "roundsBeforeTrigger": 1,
            "default": 1,
            "recipient": [],
            "alertType": "BGP"
        },
        {
            "ruleId": 300,
            "ruleName": "Default DNS Server Alert Rule",
            "notes": "",
            "expression": "((probDetail != \"\"))",
            "minimumSources": 2,
            "notifyOnClear": 0,
            "roundsBeforeTrigger": 1,
            "default": 1,
            "recipient": [
                "noreply@thousandeyes.com"
            ],
            "alertType": "DNS Server"
        }
    ]
}```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
