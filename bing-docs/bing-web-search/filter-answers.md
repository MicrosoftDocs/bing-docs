---
title: How to filter search results
titleSuffix: Bing Search Services
description: You can filter the types of answers that Bing includes in the response (for example images, videos, and news) by using the 'responseFilter' query parameter.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Filter the answers that Bing returns

When you query the web, Bing returns all relevant content that it finds. This could include webpages, images, news, videos, and more. But lets say that you're only interested in webpages and news. How can you tell Bing you're not interested in any other answers? You use the [responseFilter](reference/query-parameters.md#responsefilter) query parameter.

You can specify the answers to include in the response or those that you want to exclude from the response, or not both. To include only the webpage and news answers, specify *responseFilter* as:

```
&responseFilter=webpages,news
```

Remember that you need to URL encode all query parameters, so the parameter actually looks like:

```
&responseFilter=webpages%2Cnews
```

Bing includes the answers you request only if it finds relevant content that ranks high enough for the page of results you requested. For example, if you filter the response for images, videos, and news but Bing doesn't find relevant videos and news results that rank high enough for the first page, the response includes only images. But if you page through more results, they may include videos and news content.

To exclude specific answers from the response, prefix a minus sign (-) to the answer's name. For example, "-images." 

In theory, you could use *responseFilter* to filter for a single answer like only images or news but you're strongly discouraged from doing so. Instead, you should use the answer-specific endpoint to get richer results and better performance. For example, to receive only images, send the request using [Image Search API](../bing-image-search/overview). The Image Search API offers filters that are not available to the Web Search API.  


## Getting results from a specific site

To get search results from a specific domain, use Bing's [site:](https://help.bing.microsoft.com/#apex/18/en-US/10001/-1) operator in the query string.  

```
q=sailing+dinghies+site:contososailing.com
```

Note that the response may contain results from other sites depending on the number of relevant results found on the specified site.

> [!NOTE]
> Depending on the query, if you use the `site:` query operator, there is the chance that the response may contain adult content regardless of the [safeSearch](reference/query-parameters.md#safesearch) setting. You should use `site:` only if you are aware of the content on the site and your scenario supports the possibility of adult content.


## Specifying the content's freshness

It’s possible that Bing will return content that’s older than 30 days. If you want to ensure the freshness of the content that Bing returns, use the [freshness](reference/query-parameters.md#freshness) query parameter. Freshness refers to the date that Bing originally discovered the webpage, not when the publisher published the webpage. The [Webpage](reference/response-objects.md#webpage) object's `datePublished` field tells you when Bing originally discovered the page.

You may set the parameter to one of the following time periods:

- Day &mdash; Return webpages that Bing discovered within the last 24 hours
- Week &mdash; Return webpages that Bing discovered within the last 7 days
- Month &mdash; Return webpages that Bing discovered within the last 30 days

The following example filters the webpage results to those that Bing discovered in the last seven days.

```
freshness=week
```

You may also set this parameter to a custom date range in the form, `YYYY-MM-DD..YYYY-MM-DD`. For example:

```
freshness=2019-02-01..2019-05-30
```

To limit the results to a single date, set the *freshness* parameter to the specific date in the form, `YYYY-MM-DD`.

The results may include webpages that fall outside the specified period if the number of webpages that Bing finds that matches your criteria is less than the number of webpages you requested (or the default number that Bing returns).


## Returning the top n answers

A response may include any number of answer types. To limit the number of answers in the response to the top two ranked answers, set the [answerCount](reference/query-parameters.md#answercount) query parameter to 2. Bing chooses the answers based on ranking. For example, if Bing ranks the answers as webpages, images, videos, and relatedSearches, the response includes only webpages and images.

If the request includes the [responseFilter](reference/query-parameters.md#responsefilter) and *answerCount* query parameters, both apply. For example, if you set *responseFilter* to webpages and news and *answerCount* to 2, the response contains only webpages since news is not ranked.


## Promoting answers that are not ranked

To include answers that Bing would otherwise exclude because of ranking, use the [promote](reference/query-parameters.md#promote) query parameter. Set *promote* to a comma-delimited list of answer names that you want the response to include regardless of their ranking. For example, if the top ranked answers that Bing returns for a query are webpages, images, videos, and relatedSearches, the response would include those answers. But if you set [answerCount](reference/query-parameters.md#answercount) to 2, Bing returns the top two ranked answers, which are webpages and images. If you want to ensure that Bing includes images and videos in the response, set *promote* to images and videos.

```  
answerCount=2&promote=images%2Cvideos
```  

Bing returns the top two answers, webpages and images, and promotes videos into the response.

If you set *promote* to news, the response doesn't include the news answer because it is not a ranked answer &mdash; you can promote only ranked answers.

The answers that you want to promote do not count against the *answerCount* limit. For example, if the ranked answers are news, images, and videos, and you set *answerCount* to 1 and *promote* to news, the response contains news and images. Or, if the ranked answers are videos, images, and news, the response contains videos and news.

You may use *promote* only if you specify the *answerCount* query parameter.


## Limiting the number of webpages

By default, Web Search API returns 10 webpages. If you want to receive a different number of webpages, use the [count](reference/query-parameters.md#count) query parameter. The following example shows how to get back only 5 webpages.

```
count=5
```

The *count* query parameter affects only the number of webpages that Bing returns; it does not affect the number of results that Bing includes for the other answers (for example, images or videos).

You can also use the *count* and *offset* query parameters together to page through all the webpages that match the user’s intent. For details about paging results, see [Paging results](page-results.md).


## Filtering adult content

The [safeSearch](reference/query-parameters.md#safesearch) query parameter lets you filter out webpages, images, and videos with adult content. You may set this parameter to one of the following values:

- Off &mdash; Returns content with adult text and images but not adult videos.
- Moderate &mdash; Returns content with adult text but not adult images or videos.
- Strict &mdash; Does not return content with adult text, images, or videos.

The default is Moderate.


## Next steps

- Learn about the [response](search-responses.md) that Bing returns for the user's query.
- Learn how to [highlight the user's search terms](hit-highlighting.md) in the results that Bing returns.
- Learn about the [quickstarts](quickstarts.md) and [samples](samples.md) that are available to help you get up and running fast.
