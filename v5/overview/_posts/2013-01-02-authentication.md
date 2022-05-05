---
parent_category: overview
parent_category_label: Overview

title: 'Authentication'

sortorder: 2
category-sortorder: 1
layout: null
---

The ThousandEyes API accepts Basic HTTP or OAuth bearer token as authentication methods.  This is specified using the HTTP request wrapper of your choice.  Both the Basic Authentication Token and the OAuth Bearer Token referenced here and throughout the developer reference are available from your [Account Settings > Users and Roles page][app-accountsettings] under the "Profile" tab, in the "User API Tokens" section.

The example below shows a standard request using cURL:

`$curl -i https://api.thousandeyes.com{{ site.version_url_prefix_request }}/status \
   -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

The authentication parameters can be provided programmatically using whichever HTTP request object is being used (most all support Basic HTTP authentication), or prepended to the target URL, in the following format:

`https://{email}:{authToken}@api.thousandeyes.com`

{.inline-code}When providing the email address prepended to the URL, it must be URL-encoded to allow the request to proceed correctly.  The `@` symbol corresponds to `%40`.  See [UrlEncoding characters][external-urlencoding] for more information on properly formatting character strings.  When using cURL, using the `-u user:token` method is strongly recommended.

#Powershell Syntax

We've been asked by a number of people how to effectively create and leverage credentials against the API in Windows Powershell.  Effectively, base64 encoding the email:authtoken and setting an Authorization header will allow this to be done.  The following example takes two inputs and sets the required headers to work with the ThousandEyes API:

```[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$apiuser       = "noreply@thousandeyes.com"
$apipassword   = "g351mw5xqhvkmh1vq6zfm51c62wyzib2"
$authorization = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($apiuser + ":" + $apipassword))
$headers       = @{"accept"= "application/json"; "content-type"= "application/json"; "authorization"= "Basic " + $authorization}
$response      = Invoke-WebRequest https://api.thousandeyes.com{{ site.version_url_prefix_request }}/agents.json -Headers $headers
$response.content
```

NOTE: the "password" used by ThousandEyes basic HTTP authentication is the authToken, available from your [account settings page][app-accountsettings] under the "Profile" tab, in the "User API Tokens" section.

#Account Lockout

If our system detects 10 unsuccessful attempts to access the API over a period of 1 hour, the IP address making the login attempts will be banned for a period of 1 hour, beginning with the last request.

If attempts to reach the API are returning an HTTP/429 (too many requests) response code, it is likely that an attempt to login from this IP was unsuccessful.  Contact support for more information and instructions on remediation.

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
