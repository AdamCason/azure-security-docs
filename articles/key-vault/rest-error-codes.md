---
title: REST API error codes - Azure Key Vault
description: These error codes could be returned by an operation on an Azure Key Vault web service.
keywords: 
services: machine-learning
author: msmbaldwin
ms.custom: seodec18
ms.author: mbaldwin

ms.service: key-vault
ms.topic: reference
ms.date: 12/16/2019
---
 
# Azure Key Vault REST API Error Codes
 
The following error codes could be returned by an operation on an Azure Key Vault web service.
 
## Unauthenticated Request (HTTP 401)

401 means that the request is unauthenticated for Key Vault. 

A request is authenticated if:

- The key vault knows the identity of the caller; and
- The caller is allowed to try to access Key Vault resources. 

There are several different reason why a request may return 401.

### No authentication token attached to the request. 

Here is an example PUT request, set the value of a secret:

``` 
PUT https://putreqexample.vault.azure.net//secrets/DatabaseRotatingPassword?api-version=7.0 HTTP/1.1
x-ms-client-request-id: 03d275a2-52a4-4bed-82c8-6fe15165affb
accept-language: en-US
Authorization: Bearer     eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSIsImtpZCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSJ9.eyJhdWQiOiJodHRwczovL3ZhdWx0LmF6dXJlLm5ldCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpYXQiOjE1NDg2OTc1MTMsIm5iZiI6MTU0ODY5NzUxMywiZXhwIjoxNTQ4NzAxNDEzLCJhaW8iOiI0MkpnWUhoODVqaVBnZHF5ZlRGZE5TdHY3bGUvQkFBPSIsImFwcGlkIjoiZmFkN2Q1YjMtNjlkNi00YjQ4LTkyNTktOGQxMjEyNGUxY2YxIiwiYXBwaWRhY3IiOiIxIiwiaWRwIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3LyIsIm9pZCI6IjM5NzVhZWVkLTdkMDgtNDUzYi1iNmY0LTQ0NWYzMjY5ODA5MSIsInN1YiI6IjM5NzVhZWVkLTdkMDgtNDUzYi1iNmY0LTQ0NWYzMjY5ODA5MSIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInV0aSI6IjItZ3JoUmtlSWs2QmVZLUxuNDJtQUEiLCJ2ZXIiOiIxLjAifQ.fgubiz1MKqTJTXI8dHIV7t9Fle6FdHrkaGYKcBeVRX1WtLVuk1QVxzIFDlZKLXJ7QPNs0KWpeiWQI9IWIRK-8wO38yCqKTfDlfHOiNWGOpkKddlG729KFqakVf2w0GPyGPFCONRDAR5wjQarN9Bt8I8YbHwZQz_M1hztlnv-Lmsk1jBmech9ujD9-lTMBmSfFFbHcqquev119V7sneI-zxBZLf8C0pIDkaXf1t8y6Xr8CUJDMdlWLslCf3pBCNIOy65_TyGvy4Z4AJryTPBarNBPwOkNAtjCfZ4BDc2KqUZM5QN_VK4foP64sVzUL6mSr0Gh7lQJIL5b1qIpJxjxyQ
User-Agent: FxVersion/4.7.3324.0 OSName/Windows OSVersion/6.2.9200.0 Microsoft.Azure.KeyVault.KeyVaultClient/3.0.3.0
Content-Type: application/json; charset=utf-8
Host: putreqexample.vault.azure.net
Content-Length: 31

{
   "value": "m*gBJ7$Zuoz)"
}
```

The "Authorization" header is the access token that is required with every call to the Key Vault for data-plane operations. If this header is missing, then the response must be 401.

### The token lacks the correct resource associated with it. 

When requesting an access token from the Azure OAUTH endpoint, a parameter called "resource" is mandatory. The value is important for the token provider because it scopes the token for its intended use. The resource for ALL tokens that want to access a Key Vault is: <https://vault.keyvault.net>. Note that there is no trailing slash on the resource.

### The token is expired

We can check the values in any access token by decoding it. Tokens are base64 encoded and there are a lot of websites that will decode it for you, such as <http://jwt.calebb.net>. This is the token from the above decoded:

We can see many important parts in this token:

- aud (audience): The resource of the token. Notice that this is <https://vault.azure.net>. This token will NOT work for any resource that does not explicitly match this value, such as graph.
- iat (\[issued at): The number of ticks since the start of the epoch when the token was issued.
- nbf (\[not before): The number of ticks since the start of the epoch when this token becomes valid.
- exp (expiration): The number of ticks since the start of the epoch when this token expires.
- appid (application id): The GUID for the application id making this request.
- tid (tenant id): The GUID for the tenant id of the principal making this request

It is important that all of the values be properly identified in the token in order for the request to work. If everything is correct, then the request will not result in 401.

### Troubleshooting 401

401s need to be investigated from the point of generation of the token, before the request is made to the Key Vault. Generally code is being used to request the token. Once the token is received, it is passed into the Key Vault request. If the code is running locally, you can use Fiddler to capture the request/response to <https://login.microsoftonline.com>. This is what a request looks like:

``` 
POST https://login.microsoftonline.com/<key vault tenant id>/oauth2/token HTTP/1.1
Accept: application/json
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Host: login.microsoftonline.com
Content-Length: 192

resource=https%3A%2F%2Fvault.azure.net&client_id=<registered app id>&client_secret=<registered app secret>&client_info=1&grant_type=client_credentials
```

The following user-supplied information mush be correct:

- The key vault tenant id
- Resource value set to https%3A%2F%2Fvault.azure.net (URL encoded)
- Client id
- Client secret

Ensure the rest of the request is nearly identical.

If you can only get the response access token, you can decode it (as shown above) to ensure the tenant id, the client id (app id), and the resource.

## HTTP 403

HTTP 403 means that the request was authenticated (it knows the requesting identity) but the identity does not have permission to access the requested resource. There are two reasons for this:

- There is not access policy for the identity
- The IP address of the requesting resource is not whitelisted in the Key Vault's firewall settings.

HTTP 403 often occurs when the customer's application is not using the client id that the customer thinks it is. That usually means that the access policies is not correctly set up for the actual calling identity.

### Troubleshooting 403

The number one step to quickly troubleshooting 403 is to turn the customer's logging on; for instructions on how to do so, see [Azure Key Vault logging](key-vault-logging.md).

Once logging is turned on, you can determine if the 403 is due to access policy or firewall policy.

#### Error due to firewall policy

"Client address (00.00.00.00) is not authorized and caller is not a trusted service"

There is a limited list of "Azure Trusted Services". Azure Web Sites are \*not\* a Trusted Azure Service. For more information, see the blog post [Key Vault Firewall access by Azure App Services](https://azidentity.azurewebsites.net/post/2019/01/03/key-vault-firewall-access-by-azure-app-services).

You must add the IP address of the Azure Web Site to the Key Vault in order for it to work.

If due to access policy: find the object id for the request and ensure that the object id matches the object to which the user is trying to assign the access policy. There will often be multiple objects in the AAD which have the same name, so choosing the correct one is very important. By deleting and re-adding the access policy, it is possible to see if multiple objects exist with the same name.

In addition: most cases of access policies DO NOT require the use of the "Authorized application" as shown in the portal. This is used for "on-behalf-of" authentication scenarios and is not generally used.
