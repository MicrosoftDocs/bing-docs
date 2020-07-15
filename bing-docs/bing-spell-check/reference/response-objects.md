---
title: Bing Spell Check API v7 response objects
titleSuffix: Bing Services
description: Describes the response objects that Bing Spell Check API may return in the JSON response.
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-spell-check
ms.topic: reference
ms.date: 07/15/2017
ms.author: scottwhi
---

# Spell Check API v7 response objects

This section contains the JSON objects that the response may include. If the request is successful, the top-level object in the response is the [SpellCheck](#spellcheck) object. If the request fails, the top-level object is [ErrorResponse](#errorresponse). 

For a list of possible objects, see **In this article** in the right pane.

  
## Error  

Defines the error that occurred.  
  
|Name|Value|Type 
|-|-|-  
|<a name="error-code"></a>code|The error code that identifies the category of error. For a list of possible codes, see [Error codes](error-codes.md#error-codes).|String
|<a name="error-message"></a>message|A description of the error.|String 
|<a name="error-moredetails"></a>moreDetails|A description that provides additional information about the error.|String
|<a name="error-parameter"></a>parameter|The query parameter in the request that caused the error.|String
|<a name="error-subcode"></a>subCode|The error code that identifies the error. For example, if `code` is InvalidRequest, `subCode` may be ParameterInvalid or ParameterInvalidValue.|String
|<a name="error-value"></a>value|The query parameter's value that was not valid.|String
  
## ErrorResponse  

The top-level object that the response includes when the request fails.  
  
|Name|Value|Type
|-|-|-
|_type|Type hint, which is set to ErrorResponse.|String
|<a name="errors"></a>errors|A list of errors that describe the reasons why the request failed.|[Error](#error)[]  

## FlaggedToken

The word that is not spelled correctly or is grammatically incorrect.  
  
|Name|Value|Type
|-|-|-
|<a name="flaggedtoken-offset"></a>offset|The zero-based offset from the beginning of the [text](query-parameters.md#text) query string to the word that was flagged.|Integer  
|<a name="flaggedtoken-suggestions"></a>suggestions|A list of words that correct the spelling or grammar error. The list is in decreasing order of preference.|[TokenSuggestion](#tokensuggestion)[]  
|<a name="flaggedtoken-token"></a>token|The word in the [text](query-parameters.md#text) query string that is not spelled correctly or is grammatically incorrect.|String
|<a name="flaggedtoken-type"></a>type|The type of error that caused the word to be flagged. The following are the possible values:<ul><li>RepeatedToken &mdash; The word was repeated in succession (for example, the warm **warm** weather)<br/><br/></li><li>UnknownToken &mdash; All other spelling or grammar errors.</li></ul>|String 
  
## SpellCheck  

The top-level object that the response includes when the request succeeds.  
  
If the service suspects a denial of service attack, the request succeeds (HTTP status code is 200 OK), but the body of the response is empty.  
  
|Name|Value|Type
|-|-|-
|_type|A type hint, which is set to SpellCheck.|String
|<a name="flaggedtokens"></a>flaggedTokens|A list of words in [text](query-parameters.md#text) that were flagged as not being spelled correctly or are grammatically incorrect.<br/><br/>If there were no spelling or grammar errors found, or the specified market is not supported, the array is empty.|[FlaggedToken](#flaggedtoken)[] 
  
## TokenSuggestion  

The suggested spelling or grammar correction.  
  
|Name|Value|Type
|-|-|-
|<a name="score"></a>score|A value that indicates the level of confidence that the suggested correction is correct. If the [mode](query-parameters.md#mode) query parameter is set to Spell, this field is set to 1.0.|Double 
|<a name="suggestion"></a>suggestion|The suggested word to replace the flagged word with.<br/><br/>If the flagged word is a repeated word (see the [type](#flaggedtoken-type) field), this string is empty.|String  
  