---
parent_category: test_data
parent_category_label: Test Data

path: '/voice/metrics/{testId}'
title: '(Voice) Metrics'
type: GET

sortorder: 20
category-sortorder: 3
layout: null
---

Returns voice metrics (loss, latency, jitter, MOS score) from each agent, for each *roundId* in the requested window.  A **time frame** must be specified, or the current round of data will be returned.

### Optional Parameters

* `format=json|xml` (Optional) specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `window=[0-9]+[smhdw]?` (Optional) specifies a window of time for the result set.  See [Time Ranges][overview-timerange] for more information.
* `from=YYYY-mm-ddTHH:MM:SS&to=YYYY-mm-ddTHH:MM:SS` (Optional) specifies an explicit start (and optionally, end) for your range of data.  See [Time Ranges][overview-timerange] for more information.
* `aid={accountId}` optional and requires the user to be assigned to the target account, specifies the account context of the request, obtained from the `/accounts` endpoint.  Specifying this parameter without the user to be assigned to the target account will result in an error response. See [Account Context][overview-accountcontext] for more information

### Request

* `{testId}` (Required) the ID of the Voice test you for wish to retrieve data

### Response

Field | Data Type | Units | Notes
:------------|-------------|-------------|-------------|
agentId | integer | n/a | unique ID of agent, from `/agents` endpoint
agentName | string | n/a | display name of the agent responding
countryId | string | n/a | ISO-3166-1 alpha-2 country code of the agent
date | dateTime | n/a | yyyy-MM-dd hh:mm:ss, in UTC
roundId | long | seconds | epoch time (seconds) indicating the start time of the round
permalink | url | n/a | link to jump to this result in the front end
dscp | string | n/a | DSCP value received by the server from the agent
dscpName | string | n/a | name of DSCP value received by the server from the agent
mos | float | n/a | Mean opinion score for agent's stream
codecName | string | n/a | name of codec used by agent
codecMaxMos | float | n/a | maximum value of Mean Opinion Score based on codec selection
loss | float | percentage | percentage value of packets sent from agent not received by server
discards | integer | n/a | number of packets discarded
latency | integer | milliseconds | time to send packets from source to server
pdv | float | milliseconds | variation in packet delay, measured in milliseconds
errorDetail | string | n/a | error details, if an error was encountered.


### Example

`$curl https://api.thousandeyes.com/voice/metrics/6627.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

#### Header

```HTTP/1.1 200 OK
Date: Thu, 08 Nov 2013 07:32:48 GMT
Server: Apache/2.2.22 (Ubuntu)
Transfer-Encoding: chunked
Content-Type: application/json```

#### Body

```{
    "voice": {
        "test": {
            "enabled": 1,
            "savedEvent": 0,
            "testId": 6627,
            "testName": "my second voice test",
            "type": "voice",
            "interval": 300,
            "server": "vps-lhr4.thousandeyes.com:49152",
            "codec": "G.722 @ 32 Kbps",
            "codecId": 2,
            "dscpId": 8,
            "jitterBuffer": 40,
            "alertsEnabled": 1,
            "liveShare": 0,
            "targetAgentId": 11,
            "modifiedDate": "2014-07-11 22:53:04",
            "createdBy": "API Sandbox User (noreply@thousandeyes.com)",
            "modifiedBy": "API Sandbox User (noreply@thousandeyes.com)",
            "dscp": "CS 1 (8)",
            "createdDate": "2014-06-13 23:19:23",
            "apiLinks": [...]
        },
        "metrics": [
            {
                "agentName": "va-office",
                "countryId": "US",
                "date": "2014-08-07 01:25:31",
                "dscp": 8,
                "mos": 3.95378,
                "codecName": "G.722 @ 32 Kbps",
                "codecMaxMos": 4.03,
                "dscpName": "CS 1 (8)",
                "loss": 0,
                "discards": 0,
                "permalink": "https://app.thousandeyes.com/voice/metrics?__a=11&testId=6627&roundId=1407374700&agentId=97",
                "agentId": 97,
                "roundId": 1407374700,
                "latency": 75,
                "pdv": 0,
                "errorDetail": ""
            }
        ]
    },
    "pages": {}
}```

For more information on our HTTP response status codes, see the [response status codes documentation][overview-responsestatuscodes].