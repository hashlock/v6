---
parent_category: reports
parent_category_label: Reports

path: '{{ site.version_url_prefix_request }}/reports/create'
title: 'Creating a report'

sortorder: 4
category-sortorder: 55
type: POST

layout: null
---

{.inline-code}This endpoint creates a new report for the account group that the user belongs to. This endpoint requires the `Edit Reports` permission be assigned to the role of the user accessing this endpoint.

### Parameters

* `aid={aid}` optional and requires the user to be assigned to the target account group, specifies the account group context of the request, obtained from the `/account-groups` endpoint.  Specifying this parameter without the user being assigned to the target account will result in an error response. See [Account group context][overview-accountcontext] for more information

### Request

A Report object with or without Widgets. See Report List for reference.

### Example

`$ curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/reports/create \
  -d '{
    "title": "HTTP Server Widgets"
  }' \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`


### Response

Sends back the created report configured in the current account group. Each report contains the following fields:

Field Name | Data Type | Notes
:----------|-----------|----------|
reportId | string | unique ID of the report
title | string | title of the report
isBuiltIn | boolean | true for built-in reports, false for user-created reports
accountId | integer | ID of the account that the report belongs to
createdBy | integer | ID of the user that created the report
modifiedDate | dateTime | the date/time when the report was last modified
isDefaultForUser | boolean | true if this report is the default for the user, false otherwise
isDefaultForAccount | boolean | true if this report is the default for the account group, false otherwise
widgets | array of widget objects | an array of widget objects
apiLinks | array of apiLink objects | a list of links which can be followed to pull more information

{.inline-code}The user can specify custom `widgetId`s if they prefer to do so. These IDs have to match a 5 characters alphanumeric string and be unique across all widgets in the report. Alternatively, the `widgetId`s will be generated for them.

#### Header

```HTTP/1.1 200 OK
Server: nginx
Date: Mon, 03 Sep 2019 18:00:00 GMT
Content-Type: application/json;charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Cache-Control: no-store
X-Organization-Rate-Limit-Limit: 970
X-Organization-Rate-Limit-Remaining: 968
X-Organization-Rate-Limit-Reset: 1492076520
Strict-Transport-Security: max-age=31536000
X-Server-Name: 1-2```

#### Body

```
[
    {
        "reportId": "1",
        "title": "HTTP Server Widgets",
        "isBuiltIn": false,
        "accountId": 1,
        "createdBy": 1,
        "isDefaultForUser": false,
        "isDefaultForAccount": false,
        "widgets": [...],
        "apiLinks": [...]
    },
    ...
]
```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
