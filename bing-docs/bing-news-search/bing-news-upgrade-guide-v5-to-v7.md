---
title: Upgrade Bing News Search API v5 to v7
titleSuffix: Bing Search Services
description: This upgrade guide identifies the changes between version 5 and version 7 of Bing News Search API. Use this guide to help you identify the parts of your application that you need to update to use version 7.
services: bing-search-services
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 07/07/2023
---

# News Search API upgrade guide

This upgrade guide identifies the changes between version 5 and version 7 of Bing News Search API. Use this guide to help you identify the parts of your application that you need to update to use version 7.

## Breaking changes

### Endpoints

- Changed the `cognitive` subdomain to `bing`.
- Changed the endpoint's version number from v5 to v7.
- Removed the `/bing` folder.

New search endpoint: `https://api.bing.microsoft.com/v7.0/news/search`

### Error response objects and error codes

- All failed requests should now include an `ErrorResponse` object in the response body.

- Added the following fields to the `Error` object.  
  - `subCode`&mdash;Partitions the error code into discrete buckets, if possible.
  - `moreDetails`&mdash;Additional information about the error described in the `message` field.

- Replaced the v5 error codes with the following possible `code` and `subCode` values.

|Code|SubCode|Description
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NotImplemented|Bing returns ServerError whenever any of the sub-code conditions occur. The response includes these errors if the HTTP status code is 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Blocked|Bing returns InvalidRequest whenever any part of the request is not valid. For example, a required parameter is missing or a parameter value is not valid.<br/><br/>If the error is ParameterMissing or ParameterInvalidValue, the HTTP status code is 400.<br/><br/>If the error is HttpNotAllowed, the HTTP status code 410.
|RateLimitExceeded||Bing returns RateLimitExceeded whenever you exceed your queries per second (QPS) or queries per month (QPM) quota.<br/><br/>Bing returns HTTP status code 429 if you exceeded QPS and 403 if you exceeded QPM.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing returns InvalidAuthorization when Bing cannot authenticate the caller. For example, the `Ocp-Apim-Subscription-Key` header is missing or the subscription key is not valid.<br/><br/>Redundancy occurs if you specify more than one authentication method.<br/><br/>If the error is InvalidAuthorization, the HTTP status code is 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing returns InsufficientAuthorization when the caller does not have permissions to access the resource. This can occur if the subscription key has been disabled or has expired. <br/><br/>If the error is InsufficientAuthorization, the HTTP status code is 403.

- The following maps the previous error codes to the new codes. If you've taken a dependency on v5 error codes, update your code accordingly.

|Version 5 code|Version 7 code.subCode
|-|-
|RequestParameterMissing|InvalidRequest.ParameterMissing
RequestParameterInvalidValue|InvalidRequest.ParameterInvalidValue
ResourceAccessDenied|InsufficientAuthorization
ExceededVolume|RateLimitExceeded
ExceededQpsLimit|RateLimitExceeded
Disabled|InsufficientAuthorization.AuthorizationDisabled
UnexpectedError|ServerError.UnexpectedError
DataSourceErrors|ServerError.ResourceError
AuthorizationMissing|InvalidAuthorization.AuthorizationMissing
HttpNotAllowed|InvalidRequest.HttpNotAllowed
UserAgentMissing|InvalidRequest.ParameterMissing
NotImplemented|ServerError.NotImplemented
InvalidAuthorization|InvalidAuthorization
InvalidAuthorizationMethod|InvalidAuthorization
MultipleAuthorizationMethod|InvalidAuthorization.AuthorizationRedundancy
ExpiredAuthorizationToken|InsufficientAuthorization.AuthorizationExpired
InsufficientScope|InsufficientAuthorization
Blocked|InvalidRequest.Blocked

### Object changes

- Added the `contractualRules` field to the [NewsArticle](reference/response-objects.md#newsarticle) object. The `contractualRules` field contains a list of rules that you must follow (for example, article attribution). You must apply the attribution provided in `contractualRules` instead of using `provider`. The article includes `contractualRules` only when the [Web Search API](../bing-web-search/overview.md) response contains a News answer.

## Non-breaking Changes

### Query parameters

- Added Products as a possible value that you may set the [category](reference/query-parameters.md#category) query parameter to. See [Categories By Markets](reference/query-parameters.md#news-categories-by-market).

- Added the [SortBy](reference/query-parameters.md#sortby) query parameter, which returns trending topics sorted by date with the most recent first.

- Added the [Since](reference/query-parameters.md#since) query parameter, which returns trending topics that were discovered by Bing on or after the specified UNIX epoch timestamp.

### Object changes

- Added the `mentions` field to the [NewsArticle](reference/response-objects.md#newsarticle) object. The `mentions` field contains a list of entities (persons or places) that were found in the article.

- Added the `video` field to the [NewsArticle](reference/response-objects.md#newsarticle) object. The `video` field contains a video that's related to the news article. The video is either an \<iframe\> that you can embed or a motion thumbnail.

- Added the `sort` field to the [NewsAnswer](reference/response-objects.md#newsanswer) object. The `sort` field shows the sort order of the articles. For example, the articles are sorted by relevance (default) or date.

- Added the [SortValue](reference/response-objects.md#sortvalue) object, which defines a sort order. The `isSelected` field indicates whether the response used the sort order. If **true**, the response used the sort order. If `isSelected` is **false**, you can use the URL in the `url` field to request a different sort order.
