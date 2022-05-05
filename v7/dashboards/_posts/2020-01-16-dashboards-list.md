---
parent_category: dashboards
parent_category_label: Dashboards

path: '{{ site.version_url_prefix_request }}/dashboards'
title: 'Dashboard list'

sortorder: 1
category-sortorder: 55
type: GET

layout: null
---

{.inline-code}This endpoint returns a list of dashboards configured in ThousandEyes in the context of the user's login account group. Use this response to find a dashboard in your account group, which can be used to pull data of the specific dashboard.

### Optional (Querystring) Parameters

* `aid={aid}` optional and requires the user to be assigned to the target account group. Specifies the account group context of the request, obtained from the `/account-groups` endpoint.  Specifying this parameter without the user being assigned to the target account will result in an error response. See [Account group context][overview-accountcontext] for more information

### Request

* no request body

### Response

Sends back an array of dashboards configured in the current account group. Each dashboard contains the following fields:

Field Name | Data Type | Notes
:----------|-----------|----------|
dashboardId | string | unique ID of the dashboard
title | string | title of the dashboard
description | string | description of the dashboard
isBuiltIn | boolean | true for built-in dashboards, false for user-created dashboards
accountId | integer | ID of the account that the dashboard belongs to
createdBy | integer | ID of the user that created the dashboard
modifiedBy | integer | ID of the user that last modified the dashboard
modifiedDate | dateTime | the date/time when the dashboard was last modified
isPrivate | boolean | true if this dashboard is private
isDefaultForUser | boolean | true if this dashboard is the default for the user, false otherwise
isDefaultForAccount | boolean | true if this dashboard is the default for the account group, false otherwise
apiLink | string | link to the dashboard

### Example

`$ curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/dashboards.json \
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
X-Organization-Rate-Limit-Remaining: 225
X-Organization-Rate-Limit-Reset: 1492076520
Strict-Transport-Security: max-age=31536000
X-Server-Name: 1-2```

#### Body

```
[
    {
        "dashboardId": "5e20da3b72fce8003257bcba",
        "title": "Dashboard Title A",
        "isBuiltIn": false,
        "accountId": 11,
        "createdBy": 6828,
        "modifiedBy": 6828,
        "modifiedDate": "2020-01-16 22:06:38",
        "isPrivate": false,
        "isDefaultForUser": false,
        "isDefaultForAccount": false,
        "apiLink": "https://api.thousandeyes.com/v7/dashboards/5e20da3b72fce8003257bcba"
    },
    ...
]
```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
