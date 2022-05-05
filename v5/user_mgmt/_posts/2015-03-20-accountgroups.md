---
parent_category: user_mgmt
parent_category_label: User Management

path: '{{ site.version_url_prefix_request }}/accounts'
title: 'Account Groups List'
type: GET

sortorder: 2
category-sortorder: 8

layout: null
---

Returns a list of all account groups available to the current user.  The aid value returned for an account in this list of account groups can be used in queries elsewhere within the app.  See [Account Context][overview-accountcontext] for more information on changing Account Group context.

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `aid=x` optional, changes the account group context of the current user.  If an invalid account ID is specified as a parameter, the response will come back as an HTTP/400 error

### Request

There is no request body for this request.

### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/accounts.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

### Response

Sends back an array of account groups.

If successful, returns a list of accounts available to the authenticated user.  The "default" flag indicates that this is the "login account group" for the current user, and that any query not including an aid parameter will be executed in that account group context.

The "current" flag indicates that this is the account context you are in at present.  For example, calling `https://api.thousandeyes.com{{ site.version_url_prefix_request }}/accounts?aid=354` would change the account context, and show current = 1 for account ID 354, even though the user's default account ID is 353.  Data is shown below using this call as an example.

If your user is assigned to account groups that exist in multiple organizations, the "organizationName" field will be shown for each account group.

#### Header

```HTTP/1.1 200 OK
Date: Thu, 07 Nov 2013 07:32:48 GMT
Server: Apache/2.2.22 (Ubuntu)
Transfer-Encoding: chunked
Content-Type: application/json```

#### Body

```{
    "account": [
        {
            "aid": 353,
            "accountName": "Test Account Name #1",
            "organizationName": "Documentation",
            "current": 0,
            "default": 1
        }
        {
            "aid": 354,
            "accountName": "Test Account Name #2",
            "organizationName": "Documentation",
            "current": 1,
            "default": 0
        }
        {
            "aid": 355,
            "accountName": "Test Account Name #3",
            "organizationName": "Another Organization",
            "current": 0,
            "default": 0
        }
    ]
}```

For more information on our HTTP response status codes, see the [response status codes documentation][overview-responsestatuscodes].