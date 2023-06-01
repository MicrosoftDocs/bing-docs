---
title: Error codes used by Bing Image Search APIs
titleSuffix: Bing Services
description: Provides the list of HTTP status codes and service error codes that Bing Image Search may return.
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-image-search
ms.topic: reference
ms.date: 06/01/2023
---

# HTTP status codes that Bing Image Search APIs may return

> [!NOTE]
> If you are using the Bing APIs, bring your own LLM $15 instance and expect to exceed the 1M queries per day quota, you should consider implementing monitoring to detect when you are nearing your limit so you can automate switching to the next tier, $16.

The following are the possible HTTP status codes that a request may return:
  
|Status code|Description  
|-|-  
|200|Success.
|400|One of the query parameters is missing or not valid.
|401|The subscription key is missing or is not valid.
|403|The user is authenticated (for example, they used a valid subscription key) but they don’t have permission to the requested resource.<br/><br/>Bing may also return this status if the caller exceeded their queries per day or queries per month quota.
|410|The request used HTTP instead of the HTTPS protocol. HTTPS is the only supported protocol.
|429|The caller exceeded their queries per second quota.
|500|Unexpected server error.

If the request fails, the response contains an [ErrorResponse](response-objects.md#errorresponse) object, which contains a list of [Error](response-objects.md#errorresponse) objects that describe what caused the error. If the error is related to a parameter, the `parameter` field identifies the parameter that caused the issue. And if the error is related to a parameter value, the `value` field identifies the value that is not valid.

```json
{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidRequest", 
      "subCode": "ParameterMissing", 
      "message": "Required parameter is missing.", 
      "parameter": "q" 
    }
  ]
}

{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidAuthorization", 
      "subCode": "AuthorizationMissing", 
      "message": "Authorization is required.", 
      "moreDetails": "Subscription key is not recognized."
    }
  ]
}
```

## Error codes

The following are the possible error code and sub-error code values.

|Error code|SubCode|Description
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NotImplemented|HTTP status code is 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Blocked|Bing returns InvalidRequest whenever any part of the request is not valid. For example, a required parameter is missing or a parameter value is not valid.<br/><br/>If the error is ParameterMissing or ParameterInvalidValue, the HTTP status code is 400.<br/><br/>If you use the HTTP protocol instead of HTTPS, Bing returns HttpNotAllowed, and the HTTP status code is 410.
|RateLimitExceeded|No sub-codes|Bing returns RateLimitExceeded whenever you exceed your queries per second (QPS) or queries per month (QPM) quota.<br/><br/>If you exceed QPS, Bing returns HTTP status code 429, and if you exceed QPM, Bing returns 403.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing returns InvalidAuthorization when Bing cannot authenticate the caller. For example, if the [Ocp-Apim-Subscription-Key](headers.md#subscriptionkey) header is missing or the subscription key is not valid.<br/><br/>Redundancy occurs if you specify more than one authentication method.<br/><br/>If the error is InvalidAuthorization, the HTTP status code is 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing returns InsufficientAuthorization when the caller does not have permissions to access the resource. This can occur if the subscription key has been disabled or has expired. <br/><br/>If the error is InsufficientAuthorization, the HTTP status code is 403.
