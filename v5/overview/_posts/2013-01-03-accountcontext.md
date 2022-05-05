---
parent_category: overview
parent_category_label: Overview
path: '{{ site.version_url_prefix_request }}/accounts'
type: GET

title: 'Account Context'

sortorder: 3
category-sortorder: 1
layout: null
---

{.inline-code}Users assigned to multiple account groups can change account context by using the `aid={accountId}` parameter.  A list of account IDs can be found using the `/accounts` endpoint.  

Returns a list of all account groups in the user’s organization that the user can access. Users without  access will receive an HTTP/403 Forbidden response, indicating that the account does not have sufficient permissions to access this resource. If the user specifies an invalid account ID, the user will receive an HTTP/400 Bad Request response.

### Example

Note: the Sandbox account provided does not have access to multiple account groups, and therefore cannot be used to validate this endpoint. The example below is shown only for the purposes of continuity.

`$curl -i https://api.thousandeyes.com{{ site.version_url_prefix_request }}/accounts \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

### Response

Error responses as follows:

* Users may receive an HTTP/400 Bad Request response, in the event that an invalid account ID is specified.
* Users attempting to access an account group to which they do not belong will receive an HTTP/403 Forbidden response, indicating a lack of permissions to the resource.

If successful, returns a list of account groups available to the authenticated user. The “default” flag indicates that this is the main account of the user, and that any query not including an aid parameter will be executed in that account group context.

The “current” flag indicates that this is the account group context you are in at present. For example, calling `https://api.thousandeyes.com{{ site.version_url_prefix_request }}/accounts?aid=354` would change the account context, and show current=1 for account ID 354, even though the user's default account ID is 353.  Data is shown below using this call as an example.

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
            "current": 0,
            "default": 1
        }
        {
            "aid": 354,
            "accountName": "Test Account Name #2",
            "current": 1,
            "default": 0
        }
        {
            "aid": 355,
            "accountName": "Test Account Name #3",
            "current": 0,
            "default": 0
        }
    ]
}```

