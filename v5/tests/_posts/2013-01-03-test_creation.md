---
parent_category: tests
parent_category_label: Tests

path: '{{ site.version_url_prefix_request }}/tests/{testType}/new'
title: 'Creating a Test'
type: POST

sortorder: 5
category-sortorder: 2
layout: null
---

Creates a new test in ThousandEyes, based on properties provided in the POST data.  In order to create a new test, the user attempting the creation must be an Account Admin.

Regular users are blocked from using any of the POST-based methods.

Note: When creating or updating a test and assigning alert rules, that alert rules are based on specific measurements being available.  For example, when creating an HTTP server test with network measurements disabled, you will not be able to assign any alert rules that are based on network metrics.  The same applies to BGP measurements.

### Optional Parameters

* `aid={accountId}` optional and requires the user to be assigned to the target account, specifies the account context of the request, obtained from the `/accounts` endpoint.  Specifying this parameter without the user to be assigned to the target account will result in an error response. See [Account Context][overview-accountcontext] for more information

### Request

* `{testType}` corresponds to any of the following options:
 * bgp
 * network
 * http-server
 * page-load
 * transactions
 * dns-trace
 * dns-server
 * dns-dnssec
 * voice

See the [Test Metadata][test-metadata] page for fields available during test creation.

### Response

If a test is successfully created, an HTTP/201 CREATED response will be returned, and the test definition will be returned.  See the example below:

### Example

Please note, test creation/modification/deletion is not allowed on the Sandbox API account, and will not work if attempted.  The following example is presented for documentation and reference purposes only.

```$curl -i https://api.thousandeyes.com{{ site.version_url_prefix_request }}/tests/network/new.json \
-d '{ "interval": 300, \
    "agents": [\
      {"agentId": 113}
    ],
    "testName": "API network test addition for www.thousandeyes.com",
    "server": "www.thousandeyes.com",
    "port": 80,
   "alertsEnabled": 0
  }' \
-H "Content-Type: application/json" \
-u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2```

#### Header

```HTTP/1.1 201 CREATED
Date: Thu, 07 Nov 2013 07:32:48 GMT
Server: Apache/2.2.22 (Ubuntu)
Transfer-Encoding: chunked```

For more information on our HTTP response status codes, see the [response status codes documentation][overview-responsestatuscodes].
