---
parent_category: tests
parent_category_label: Tests

path: '{{ site.version_url_prefix_request }}/tests/{testType}'
title: 'Test List by Type'
type: GET

sortorder: 2
category-sortorder: 2

layout: null
---

{.inline-code}Returns a list of all tests of the type specified, configured in ThousandEyes.  Also returns data for saved events, which are indicated by a boolean field, `"savedEvent": 1`

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `aid={accountId}` optional and requires the user to be assigned to the target account, specifies the account context of the request, obtained from the `/accounts` endpoint.  Specifying this parameter without the user to be assigned to the target account will result in an error response. See [Account Context][overview-accountcontext] for more information

### Request

* `{testType}` corresponds to any of the following options:
  * network
  * bgp
  * http-server
  * page-load
  * transactions
  * dns-trace
  * dns-server
  * dns-dnssec
  * dnsp-domain
  * dnsp-server
  * voice
* no request body

### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/tests/http-server.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

### Response

Sends back an array of tests of that type.  See [Test Metadata][test-metadata] page for information on fields returned by this endpoint.

#### Header

```HTTP/1.1 200 OK
Date: Thu, 07 Nov 2013 07:32:48 GMT
Server: Apache/2.2.22 (Ubuntu)
Transfer-Encoding: chunked
Content-Type: application/json```

#### Body

```{
    "test": [
        {
            "enabled": 1,
            "testId": 817,
            "testName": "http://www.thousandeyes.com",
            "interval": 900,
            "url": "http://www.thousandeyes.com",
            "modifiedDate": "2013-05-11 02:02:21",
            "networkMeasurements": 1,
            "createdBy": "API Sandbox User (noreply@thousandeyes.com)",
            "modifiedBy": "API Sandbox User (noreply@thousandeyes.com)",
            "createdDate": "2012-06-28 19:33:12",
            "apiLinks": [...]
        }
    ]
}```

For more information on our HTTP response status codes, see the [response status codes documentation][overview-responsestatuscodes].