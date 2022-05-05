---
parent_category: dashboards
parent_category_label: Dashboards

path: '{{ site.version_url_prefix_request }}/dashboards/create'
title: 'Creating a dashboard'

sortorder: 4
category-sortorder: 55
type: POST

layout: null
---

{.inline-code}This endpoint creates a new dashboard for the Login Account Group. User must have  the `Edit dashboard templates for all users in account group` permission (Account Admin) or `Edit own dashboard templates` permission (Regular User) to create dashboards.

### Optional (Querystring) Parameters

* `aid={aid}` optional and requires the user to be assigned to the target account group, specifies the account group context of the request, obtained from the `/account-groups` endpoint.  Specifying this parameter without the user being assigned to the target account will result in an error response. See [Account group context][overview-accountcontext] for more information.

### Request

A Dashboard object with or without Widgets. See [Dashboard Detail][dashboards-detail] for reference.


### Response

Sends back the created dashboard configured in the current account group. Each dashboard contains the following fields:

Field Name | Data Type | Notes
:----------|-----------|----------|
dashboardId | string | unique ID of the dashboard
title | string | title of the dashboard
isBuiltIn | boolean | true for built-in dashboards, false for user-created dashboards
accountId | integer | ID of the account that the dashboard belongs to
createdBy | integer | ID of the user that created the dashboard
modifiedDate | dateTime | the date/time when the dashboard was last modified
isPrivate | boolean | true if this dashboard is private
isDefaultForUser | boolean | true if this dashboard is the default for the user, false otherwise
isDefaultForAccount | boolean | true if this dashboard is the default for the account group, false otherwise
widgets | array of widget objects | an array of widget objects
apiLink | string | link to the dashboard

{.inline-code}The user can specify custom widget `id`s if they prefer to do so. These IDs have to match a 5 characters alphanumeric string and be unique across all widgets in the dashboard. Alternatively, the widget `id`s will be generated for them.

### Example

`$ curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/dashboards/create \
  -d '{
    "title": "HTTP Server Widgets"
  }' \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
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
X-Organization-Rate-Limit-Remaining: 237
X-Organization-Rate-Limit-Reset: 1492076520
Strict-Transport-Security: max-age=31536000
X-Server-Name: 1-2```

#### Body

```
{
    "dashboardId": "5e1f7a99143ae6004fdc3bb4",
    "title": "HTTP Server Widgets",
    "isBuiltIn": false,
    "accountId": 1,
    "createdBy": 1,
    "isPrivate": false,
    "isDefaultForUser": false,
    "isDefaultForAccount": false,
    "widgets": [...],
    "apiLink": "https://api.thousandeyes.com/v7/dashboards/5e1f7a99143ae6004fdc3bb4"
}
```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
