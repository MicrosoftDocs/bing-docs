---
title: Handle the Bing Spell Check API response
titleSuffix: Bing Search Services
description: The Bing Spell Check API response identifies spell and grammar errors in a text string. This topics shows you what the JSON response looks like and how to process it.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Handling the spell check response

When you send a request to Spell Check API, it returns a [SpellCheck](../reference/response-objects.md#spellcheck) object in the response body. The `flaggedTokens` list contains a [FlaggedToken](../reference/response-objects.md#flaggedtoken) object for each word that Bing flagged for spelling and grammar issues. If the API didn't find an error, or the specified market is not supported, the list is empty.

```json
{
  "_type" : "SpellCheck",  
  "flaggedTokens" : [ ... ]
}
```

The **FlaggedToken** object identifies the word that caused the issue, the location in the text string where it was found, and a list of suggestions for fixing the issue.

```json
    {
      "offset": 5,
      "token": "its",
      "type": "UnknownToken",
      "suggestions": [
        {
          "suggestion": "it's",
          "score": 0.8048868675051712
        }
      ]
    },
```

In the text string, replace word in `token` with the word in `suggestion`. Use the zero-based offset in `offset` to find the token in the text string. 

If the `type` field is RepeatedToken, you also replace `token` with `suggestion`, but you may need to remove the trailing space.


```json
    {
      "offset": 19,
      "token": "turn",
      "type": "RepeatedToken",
      "suggestions": [
        {
          "suggestion": "",
          "score": 0
        }
      ]
    },
```

If an error occurs, the response body contains an [ErrorResponse](../reference/response-objects.md#errorresponse) object. Bing returns an error response for all 400 level HTTP status codes. [Read more](../reference/error-codes.md).

```json
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

Based on the text string, *when its your turn turn, john, come runing*, the following table shows a side-by-side comparison of the spelling and grammar errors that Bing found using both modes.

|Proof mode|Spell mode
|-|-
|{<br/>&nbsp;&nbsp;"_type": "SpellCheck",<br/>&nbsp;&nbsp;"flaggedTokens": [<br/>&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"offset": 5,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"token": "its",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "UnknownToken",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"suggestions": [<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"suggestion": "it's",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"score": 0.8048868675051712<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]<br/>&nbsp;&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"offset": 19,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"token": "turn",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "RepeatedToken",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"suggestions": [<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"suggestion": "",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"score": 0<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]<br/>&nbsp;&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"offset": 36,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"token": "runing",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "UnknownToken",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"suggestions": [<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"suggestion": "running",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"score": 0.8048868675051712<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]<br/>&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;]<br/>}|{<br/>&nbsp;&nbsp;"_type": "SpellCheck",<br/>&nbsp;&nbsp;"flaggedTokens": [<br/>&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"offset": 5,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"token": "its",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "UnknownToken",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"suggestions": [<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"suggestion": "it's",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"score": 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]<br/>&nbsp;&nbsp;&nbsp;&nbsp;},<br/>&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"offset": 36,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"token": "runing",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "UnknownToken",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"suggestions": [<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"suggestion": "running",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"score": 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]<br/>&nbsp;&nbsp;&nbsp;&nbsp;}<br/>&nbsp;&nbsp;],<br/>&nbsp;&nbsp;"correctionType": "High"<br/>}


## Next steps  

- Learn about [use and display requirements](../../bing-web-search/use-display-requirements.md) for displaying Bing Spell Check results.  
- Learn about the [JSON objects](../reference/response-objects.md) found in the response.  
- Learn about the [quickstarts](../quickstarts/quickstarts.md) and [samples](../samples.md) that are available to help you get up and running fast.
