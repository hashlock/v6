---
parent_category: labels
parent_category_label: Labels

path: '{{ site.version_url_prefix_request }}/groups/{type}/{groupId}'
title: 'Label details (by label type)'
type: GET

sortorder: 4
category-sortorder: 7
layout: null
---

Returns a list of all labels (formerly called groups) configured in ThousandEyes.  This includes both Agent and Test labels.

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `aid={aid}` optional and requires the user to be assigned to the target account group, specifies the account group context of the request, obtained from the `/account-groups` endpoint.  Specifying this parameter without the user being assigned to the target account will result in an error response. See [Account group context][overview-accountcontext] for more information

### Request

* `{type}` corresponds to the following options:
  * `agents`
  * `tests`
* `{groupId}` the ID of the label to retrieve
* no request body

### Response

Returns the label details.  Metadata of the response is returned according to the following table:

Field | Data Type | Units | Notes
:------------|-------------|-------------|-------------|
name | string | n/a | Name of the label
groupId | integer | n/a | Unique ID of the label; this number is negative for built-in labels.  Query the `/groups/{id}` endpoint to see a list of agents/tests with this label.
type | string | n/a | either `tests` or `agents`, indicates the type of label requested.
builtin | integer | n/a | 1 for built-in labels, and 0 for user-created labels.  Note that built-in labels are read-only.
agents | array of agent objects | n/a | Agent objects are shown for an `agents` type label only.  See [Agent Metadata][agent-metadata] for more details on agent objects.
tests | array of test objects | n/a | Test objects are shown for a `tests` type label only.  See [Test Metadata][test-metadata] for more detail on test objects.

### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/groups/tests/958.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

#### Header

```HTTP/1.1 200 OK
Server: nginx
Date: Tue, 15 Mar 2016 00:20:26 GMT
Content-Type: application/json;charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Strict-Transport-Security: max-age=31536000
X-Server-Name: 1-5```

#### Body

```{
    "groups": [
        {
            "name": "Dave's tests",
            "groupId": 958,
            "type": "tests",
            "builtin": 0,
            "tests": [
                {
                    "createdDate": "2014-06-13 04:01:07",
                    "modifiedDate": "2015-01-06 19:27:03",
                    "createdBy": "Dave Fraleigh (dave@thousandeyes.com)",
                    "modifiedBy": "Dave Fraleigh (dave@thousandeyes.com)",
                    "enabled": 1,
                    "savedEvent": 0,
                    "testId": 6600,
                    "testName": "Bozo BGP test 1",
                    "type": "bgp",
                    "prefix": "38.122.0.0/16",
                    "alertsEnabled": 0,
                    "liveShare": 0,
                    "includeCoveredPrefixes": 0,
                    "apiLinks": [
                        {
                            "rel": "self",
                            "href": "https://api.thousandeyes.com{{ site.version_url_prefix_response }}/tests/6600"
                        },
                        {
                            "rel": "data",
                            "href": "https://api.thousandeyes.com{{ site.version_url_prefix_response }}/net/bgp-metrics/6600"
                        }
                    ]
                }
            ]
        }
    ]
}```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
