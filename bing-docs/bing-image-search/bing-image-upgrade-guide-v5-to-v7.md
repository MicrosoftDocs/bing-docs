---
title: Upgrade from Bing Image Search API v5 to v7
titleSuffix: Bing Search Services
description: This upgrade guide describes changes between version 5 and version 7 of the Bing Image Search API. Use this guide to help you identify the parts of your application that you need to update to use version 7.
services: bing-search-services
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 06/30/2023
---

# Bing Image Search API v7 upgrade guide

This upgrade guide identifies the changes between version 5 and version 7 of the Bing Image Search API. Use this guide to help you identify the parts of your application that you need to update to use version 7.

## Breaking changes

### Endpoints

- Changed the `cognitive` subdomain to `bing`.
- Changed the endpoint's version number from v5 to v7.
- Removed the `/bing` folder.

New search endpoint: `https://api.bing.microsoft.com/v7.0/images/search`

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

### Use Bing Visual Search API to get image insights

Use [Bing Visual Search API](../bing-visual-search/overview.md) instead of the `/images/search` endpoint to get insights about images.

### Query parameters

- Changed the list of supported markets of the Shopping filter value to en-US only. See [imageType](reference/query-parameters.md#imagetype).  

### Response objects

- Renamed the `insightsSourcesSummary` field of the [Image](reference/response-objects.md#image) object to `insightsMetadata`.  

- Renamed the `InsightsSourcesSummary` object to [InsightsMetadata](reference/response-objects.md#insightsmetadata).  

- Removed the `displayShoppingSourcesBadges` and `displayRecipeSourcesBadges` fields from [ImageAnswer](reference/response-objects.md#imageanswer).  

- Renamed the `nextOffsetAddCount` field of [ImageAnswer](reference/response-objects.md#imageanswer) to `nextOffset`. The way you use the offset has also changed. Previously, you set the [offset](reference/query-parameters.md#offset) query parameter to the `nextOffsetAddCount` value plus the previous offset value plus the number of images in the result. Now, you set `offset` to the `nextOffset` value.  

## Non-breaking changes

### Query parameters

- Added Transparent as a possible [imageType](reference/query-parameters.md#imagetype) filter value. The Transparent filter returns only images with a transparent background.

- Added the Any as a possible [license](reference/query-parameters.md#license) filter value. The Any filter returns only images that are under license.

- Added the [maxFileSize](reference/query-parameters.md#maxfilesize) and [minFileSize](reference/query-parameters.md#minfilesize) query parameters. Use these filters to return images within a range of file sizes.  

- Added the [maxHeight](reference/query-parameters.md#maxheight), [minHeight](reference/query-parameters.md#minheight), [maxWidth](reference/query-parameters.md#maxwidth), [minWidth](reference/query-parameters.md#minwidth) query parameters. Use these filters to return images within a range of heights and widths.  

### Object changes

- Added `similarTerms` to the [ImageAnswer](reference/response-objects.md#imageanswer) object. This field contains a list of terms that are similar in meaning to the user's query string.  
