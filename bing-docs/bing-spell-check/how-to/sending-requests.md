---
title: Sending requests to the Bing Spell Check API
titleSuffix: Bing Search Services
description: Learn about the Bing Spell Check modes, settings, and other information relating to the API.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Check text for spelling and grammar mistakes

Checking a text string for spelling and grammar mistakes is easy if you have your subscription key. Just send an HTTP GET request to the following endpoint:

```
https://api.bing.microsoft.com/v7.0/spellcheck
```

Here's a cURL example that shows you how to call the endpoint using your subscription key. Set the [text](../reference/query-parameters.md#text) query parameter to the string to proof.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing
```


## Request and response headers

Although that's all you need to do to spell check on a text string, Bing suggests you include a couple of other headers to provide a better experience for your user. Those headers include:

- User-Agent &mdash; Lets Bing know whether needs a mobile or desktop experience.
- X-MSEdge-ClientID &mdash; Provides continuity of experience.
- X-MSEdge-ClientIP &mdash; Provides the user's location for location aware queries.
- X-Search-Location &mdash; Provides the user's location for location aware queries.

The more information you can provide Bing, the better the experience will be for your users. To learn more about these headers, see [Request headers](../reference/headers.md#request-headers).

Here's a cURL example that includes these headers.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" -H "X-MSEdge-ClientID: 00B4230B74496E7A13CC2C1475056FF4" -H "X-MSEdge-ClientIP: 11.22.33.44" -H "X-Search-Location: lat:55;long:-111;re:22" -A "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.65 Safari/537.36" https://api.bing.microsoft.com/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing
```

Bing returns a couple of headers you should capture. 

- BingAPIs-TraceId &mdash; ID that identifies the request in the log file.
- X-MSEdge-ClientID &mdash; The ID that you need to pass in subsequent request to provide continuity of experience.
- BingAPIs-Market &mdash; The market used by Bing for the request.

To learn more about these headers, see [Response headers](../reference/headers.md#response-headers).

Here's a cURL call that returns the response headers. If you want to remove the response data so you can see only the headers, include the `-o nul` parameter.

```curl
curl -D - -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing
```


## Query parameters

The only query parameter that you must pass is the *text* parameter, which you set to the text string to proof. You must URL-encode the text string and all query parameter values that you pass.

The API supports a number of query parameters that you can pass in your request. Here's a list of the ones you're most likely to pass:

- *mkt* &mdash; Used to specify the market where the results come from, which is typically the market where the user is making the request from.
- *mode* &mdash; Used to specify the type of spelling and grammar checks to perform. Possible values are Proof (default) and Spell.  
  
  Use Proof mode for document scenarios. Proof mode provides the most comprehensive checks, adding capitalization, basic punctuation, and other features to aid document creation, but it's available only in the en-US (English-United States), es-ES (Spanish-Spain), pt-BR (Portuguese-Brazil) markets. In this mode, the text string is limited to 4,096 characters.  
  
  Use Spell mode for search scenarios. Spell mode is more aggressive in order to return better search results. Spell mode finds most spelling mistakes but doesn't find some of the grammar errors that Proof catches; for example, capitalization and repeated words. In this mode, the text string is limited to 130 characters for these languages: en, de, es, fr, pl, pt, sv, ru, nl, nb, tr-tr, it, zh, ko; and 65 characters for all others.

To learn more about these parameters and other parameters that you may specify, see [Query parameters](../reference/query-parameters.md).

Here's a cURL example that includes these query parameters.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us&mode=proof
```


## Should I use POST and GET?

The API supports both POST and GET requests. Which you use depends on the length of text you plan to proof. If the strings are always less than 1,500 characters, go ahead and use a GET. But for documentation scenarios where the text could be longer, use POST. 

Here's a cURL example that uses a POST request to send the text string in the body of the request.

```curl
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" -d "text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us&mode=proof" https://api.cognitive.microsoft.com/v7.0/spellcheck
```


## Next steps

- Learn about the [spell check response](search-response.md) that Bing returns.
- Learn about the [quickstarts](../quickstarts/quickstarts.md) and [samples](../samples.md) that are available to help you get up and running fast.

