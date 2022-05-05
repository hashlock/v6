---
parent_category: labels
parent_category_label: Labels

path: '{{ site.version_url_prefix_request }}/groups/{type}'
title: 'Labels list by type'
type: GET

sortorder: 2
category-sortorder: 7
layout: null
---

Returns a list of all tests of the label (formerly called group) type specified, configured in ThousandEyes.

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `aid={aid}` optional, specifies the account group context for the request.

### Request

* `{type}` corresponds to the following options:
  * `agents`
  * `tests`
* no request body

### Response

Sends back an array of labels configured for the type specified in the platform. Metadata of the response is returned according to the following table:

Field | Data Type | Units | Notes
:------------|-------------|-------------|-------------|
name | string | n/a | Name of the label
groupId | integer | n/a | Unique ID of the label; this number is negative for built-in labels.  Query the `/groups/{id}` endpoint to see a list of agents/tests with this label.
type | string | n/a | Either `tests` or `agents`, indicates the type of label.
builtin | integer | n/a | 1 for built-in labels, and 0 for user-created labels.  Note that built-in labels are read-only.

### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/groups/tests.json \
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
            "name": "Shared",
            "groupId": -8,
            "type": "tests",
            "builtin": 1
        },
        {
            "name": "Saved Event",
            "groupId": -9,
            "type": "tests",
            "builtin": 1
        },
        {
            "name": "Dave's tests",
            "groupId": 958,
            "type": "tests",
            "builtin": 0
        },
        {
            "name": "Infrastructure tests",
            "groupId": 961,
            "type": "tests",
            "builtin": 0
        }
    ]
}```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
