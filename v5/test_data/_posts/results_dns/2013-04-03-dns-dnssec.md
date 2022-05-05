---
parent_category: test_data
parent_category_label: Test Data

path: '{{ site.version_url_prefix_request }}/dns/dnssec/{testId}'
title: '(DNS) DNSSEC'
type: GET

sortorder: 15
category-sortorder: 3
layout: null
---

Returns the keychain validity for a record on a domain secured using DNSSEC extensions.

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `window=[0-9]+[smhdw]?` specifies a window of time for the result set.  See [Time Ranges][overview-timerange] for more information.
* `from=YYYY-mm-ddTHH:MM:SS&to=YYYY-mm-ddTHH:MM:SS` specifies an explicit start (and optionally, end) for your range of data.  See [Time Ranges][overview-timerange] for more information.
* `aid={accountId}` optional and requires the user to be assigned to the target account, specifies the account context of the request, obtained from the `/accounts` endpoint.  Specifying this parameter without the user to be assigned to the target account will result in an error response. See [Account Context][overview-accountcontext] for more information

### Request

* `{testId}` the ID of the DNSSEC test you wish to retrieve

### Response

Validity can be measured using the errorDetails element, which will be populated in the event of an error validating the keychain.

Field | Data Type | Units | Notes
:------------|-------------|-------------|-------------|
agentName | string | n/a | display name of the agent responding
countryId | string | n/a | ISO-3166-1 alpha-2 country code of the agent
date | dateTime | n/a | yyyy-MM-dd hh:mm:ss, in UTC
agentId | integer | n/a | unique ID of agent, from `/agents` endpoint
roundId | long | seconds | epoch time (seconds) indicating the start time of the round
valid | boolean | n/a | 1 for valid, 0 for invalid (see errorDetails field)
permalink | url | n/a | link to jump to this result in the front end
errorDetails | string | n/a | if an error was encountered, error text


### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/dns/dnssec/822.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

#### Header

```HTTP/1.1 200 OK
Date: Thu, 08 Nov 2013 07:32:48 GMT
Server: Apache/2.2.22 (Ubuntu)
Transfer-Encoding: chunked
Content-Type: application/json```

#### Body

```{
    "dns": {
        "test": {
            "enabled": 1,
            "testId": 822,
            "testName": "thousandeyes.com A",
            "type": "dns-dnssec",
            "interval": 900,
            "domain": "thousandeyes.com A",
            "createdBy": "API Sandbox User (noreply@thousandeyes.com)",
            "createdDate": "2012-06-28 20:48:01",
            "apiLinks": [...]
        },
        "dnssec": [
            {
                "agentName": "Hong Kong",
                "countryId": "HK",
                "date": "2013-11-13 04:30:01",
                "valid": 0,
                "roundId": 1384317000,
                "permalink": "https://app.thousandeyes.com/dns/dnssec?__a=75&testId=822&roundId=1384317000&agentId=12",
                "agentId": 12,
                "errorDetails": "No DNSSEC public key(s) for thousandeyes.com A"
            },
            ...
        ]
    },
    "pages": {
        "current": 1
    }
}```

For more information on our HTTP response status codes, see the [response status codes documentation][overview-responsestatuscodes].