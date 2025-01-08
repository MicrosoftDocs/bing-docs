---
title: What is the Bing Custom Search API?
titleSuffix: Bing Search Services
description: The Bing Custom Search enables you to create tailored search experiences for topics that you care about.
services: bing-search-services
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-custom-search
ms.topic: overview
author: alekhyasasi
ms.author: v-alpunnamar
ms.date: 09/20/2023
---

# What is Bing Custom Search?

Bing Custom Search lets you create a tailored, ad-free search experiences for topics that your users care about. You specify the domains and webpages that Bing searches. You can also pin, boost, and demote specific content to create a custom view of the web that helps your users quickly find relevant content. To create your custom view, use the [Bing Custom Search portal](https://customsearch.ai).

After defining your view, integrate it into your website or application by calling Bing Custom Search API. As an option, you can configure a searchable user interface that you render within your website or application.

![How Bing Custom Search works](media/BCS-Overview.png "How Bing Custom Search works.")

## Get started

To get started using the API, pick the subscription you want from <a href="https://aka.ms/bingsearchapipricing" target="_blank">Bing API Pricing</a>. After getting your subscription key, you're all set to make your first call.

You can easily call the API by sending a native HTTP GET request or by using the Custom Search SDK.

## Features

Custom Search provides the following features:

|Feature|Description
|-|-
|[Define your view of the web](how-to/define-your-custom-view.md)|Identify the content that your users care about.
|[Define a search experience that you host within your website or app](how-to/hosted-ui.md)|Configure and host a search experience that you can integrate into a webpage or app using JavaScript.
|[Share your custom search instance](how-to/share-your-custom-search.md)|Collaboratively edit and test your search instance with teammates.

After defining your custom view of the web, use the following APIs to query its content.

- Custom Search API  &mdash; Lets your users search for webpages and more from the domains and sites specified in your custom search instance.
- [Custom Image API](how-to/get-images-from-instance.md) &mdash; Lets your users search for images from the domains and sites specified in your custom search instance.
- [Custom Video API](how-to/get-videos-from-instance.md) &mdash; Lets your users search for videos from the domains and sites specified in your custom search instance.
- [Custom Autosuggest API](how-to/configure-custom-autosuggest.md) &mdash; Lets you provide suggested search strings to your users as they type in your search box.

### Search or search-like experience

Bing Custom Search API may only be used as a result of a direct user query or search, or as a result of an action within an app or experience that logically can be interpreted as a user’s search request. For illustration purposes, the following are some examples of acceptable search or search-like experiences.

- User enters a query directly into a search box in an app.
- User selects specific text or image and requests “more information” or “additional information”.
- User asks a search bot about a particular topic.
- User dwells on a particular object or entity in a visual search type scenario.

If you are not sure if your experience can be considered a search-like experience, check with Microsoft.

## Next steps

- Learn about other APIs in the [family of Bing Search APIs](../bing-web-search/bing-api-comparison.md).
- Learn about [use and display requirements](../bing-web-search/use-display-requirements.md) for Bing Image Search.  
- Learn how to [create a custom search instance](how-to/define-your-custom-view.md).
- Learn how to [search your custom search instance](how-to/search-your-custom-view.md).
- Review [Custom Search API v7 reference](reference/endpoints.md) documentation.  
