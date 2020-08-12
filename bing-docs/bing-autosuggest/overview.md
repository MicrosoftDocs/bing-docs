---
title: What is Bing Autosuggest?
titleSuffix: Bing Search Services
description: Use Bing Autosuggest API to improve your apps search box experience by adding suggested search terms as the user types their query string.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-autosuggest
ms.topic: overview
ms.date: 07/15/2020
ms.author: scottwhi
---

# What is Bing Autosuggest?

Use Bing Autosuggest API to improve your users' search box experience by providing a list of suggested query strings with each character they type.

As the user types their search query, send Bing the partial query string and get back suggestions. The more complete the user’s query string is, the more relevant the list of suggested query terms will be. For example, the suggestions that Bing might return for *m* are likely to be less relevant than the suggestions Bing returns for *micro*. 

The suggestions are based on user intent and past searches made by the user and others.


## Get started

To get started using the API, pick the subscription you want from <a href="https://www.microsoft.com/en-us/bing/apis/pricing" target="_blank">Bing API Pricing</a>. After getting your subscription key, you're all set to make your first call. 

You can easily call the API by sending a native HTTP GET request or by using the Autosuggest SDK. For examples to help you get up and running quickly for either option, see [Quickstarts](quickstarts/quickstarts.md).


## Next steps

- Learn about other APIs in the [family of Bing Search APIs](../bing-web-search/bing-api-comparison.md).
- Learn about [use and display requirements](use-display-requirements.md) for Bing Search APIs.  
- Learn about [calling the API](get-suggestions.md).
- Review [Autosuggest API v7 reference](reference/endpoints.md) documentation.  


