---
title: Use and Display requirements of Bing Search APIs, with your LLM
titleSuffix: Bing Search Services
description: The requirements for displaying search results from the Bing Search APIs with LLM in your applications.
services: bing-search-services
author: alekhyasasi
ms.author: v-alpunnamar
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 01/29/2024
---

# Use and Display requirements of Bing Search APIs, with your LLM

> [!NOTE]
> The Use and Display requirements on this page apply to the Bing Search APIs, with your LLM. For Use and Display Requirements specific to using the Bing Search APIs, refer [here](use-display-requirements.md).

These Bing Search APIs, with your LLM use and display requirements apply to any implementation of the content and associated information from the following Bing Search APIs, including relationships, metadata, and other signals. Specific terms apply to use of Bing Search APIs, with your LLM used in connection with large language models (LLMs), see "LLM use and display requirements" below.

- Bing Entity Search
- Bing Image Search
- Bing News Search
- Bing Video Search
- Bing Web Search
- Bing Spell Check
- Bing Autosuggest

Note use of Bing Custom Search and Bing Visual Search is not included in Bing Search APIs, with your LLM.

## Definitions

|Term|Description
|-|-
|Answer|A category of results returned in a response. For example, a response from the Bing Web Search API can include answers in the categories of webpage results, image, video, and news.
|Grounding|The process of allowing an LLM model to temporarily access and use the Web Results to formulate or augment an LLM response for a single query and user, but not for the purposes of training the LLM model with such data for future use.
|LLM(s)|Large language model(s).
|Response|Any and all answers and associated data received in response to a single call to a Search API.
|Result|An item of information in an answer. For example, the set of data connected with a single news article is a result in a news answer.
|Search APIs|Collectively, Entity Search, Image Search, News Search, Video Search, and Web Search APIs.
|Web Results|The title, URL and snippet for the top ten webpage results returned from the Bing Web Search API.

## Bing Spell Check and Bing Autosuggest API restrictions

**Do not:**

- Copy, store, or cache any data you receive from the Bing Spell Check or Bing Autosuggest APIs.
- Use data you receive from the Bing Spell Check or Bing Autosuggest APIs as part of any machine learning or similar algorithmic activity. Do not use this data to train, evaluate, or improve new or existing services that you or third parties might offer.
- Display data received from the Bing Spell Check or Bing Autosuggest APIs on the same page as content from any general web search engine, LLMs (except as permitted below) or advertising network.
- Attribute to Microsoft responses (or parts of responses) displayed from the Bing Spell Check or Bing Autosuggest APIs, unless Microsoft specifies otherwise in writing for your particular use.

## Bing Search APIs

**Note**:

The requirements in this section apply to only the Search APIs, which does not include Bing Spell Check or Bing Autosuggest. This section applies to use and display of Responses from the Search APIs.

When using in connection with LLMs specific additional terms apply, see "LLM use and display requirements" below.

### Internet search experience requirements

All data returned in Responses may only be used in internet search experiences. An internet search experience means the content displayed:

- Is relevant and responsive to the end user's direct query, or other indication of their search interest and intent (for example, a user-indicated search query).
- Helps users find and navigate to the response's data sources. For example, providing clickable links from hyperlinks in the response.
- Includes multiple results for the user to select from.
- Are in a placement that enables users to search.
- Includes a visible indication that the content is an internet search result. For example, a statement that the content is "from the web".
- Includes any other appropriate measures to ensure your Bing Search API data does not violate any applicable laws or rights of, or duties or obligations owed by you to, third parties. Consult your legal advisors to determine what measures may be appropriate.

### Restrictions

**Do not:**

- Copy, store, or cache any data from Responses.
- Use data received from the Search APIs as part of any machine learning or similar algorithmic activity. Do not use this data to train, evaluate, or improve new or existing services that you or third parties might offer.
- Display data received from the Search APIs on the same page as content from any general web search engine, LLMs (except as permitted below) or advertising network.
- Modify the results content (other than to reformat them in a way that does not violate any other requirement), unless required by law or agreed to by Microsoft.
- Omit attribution information and URLs associated with results content.
- Reorder, including by omission, the results displayed in an answer when an order or ranking is provided, unless required by law or agreed to by Microsoft.
- Attribute to Microsoft Responses (or parts of Responses) displayed from the Search APIs, unless Microsoft specifies otherwise in writing for your particular use.
- Display content that was not included within any part of a Response in a way that would lead a user to believe that content is part of the Response.
- Use Responses for websites where you are restricted by the website from using such content, including but not limited to, where your crawler has been blocked via robots.txt.
- Display advertising that is not provided by Microsoft on any page that displays any part of a Response.
- Display any advertising on pages with Responses:
  - From the Bing Image, News Search, or Video Search APIs, or
  - That are filtered or limited primarily (or solely) to image, news and/or video results.

### Privacy Notice

**Do:**

- Prominently include a functional hyperlink to the [Microsoft Privacy Statement](https://go.microsoft.com/fwlink/?LinkId=521839), near each point in the user experience (UX) that offers a user the ability to input a search query. Label the hyperlink **Microsoft Privacy Statement**.

## LLM use and display requirements for Search APIs, Bing Spell Check API and Bing Autosuggest API

When using Search APIs, Bing Spell Check API, or Bing Autosuggest API in connection with LLMs, you must follow the guidelines below:

**You may <u>only</u>:**

- Display responses received from the Search APIs, Bing Spell Check API and Bing Autosuggest API on the same webpage as content from an LLM, provided the Search APIs, Bing Spell Check API and Bing Autosuggest API responses are clearly separated from the LLM content (e.g. the LLM content should not be inserted in between the Search APIs Results); and/or
- Use Web Results for Grounding an LLM.

**Do not:**

- Use any responses (including, without limitation, web, images, videos, news, entity data, auto suggest, spell etc.) to train, evaluate or improve any LLM, including your proprietary LLM;
- Use Responses for websites where you are restricted by the website from using such content, including but not limited to, where your crawler has been blocked via robots.txt;
- Display advertising that is not provided by Microsoft; or
- Attribute the Web Results for Grounding your LLM to Microsoft, unless Microsoft specifies otherwise in writing for your particular use.

**LLM Attribution:**

- You must provide source attribution and a link back to each source that is used for the content being displayed from the LLM, when using Web Results for Grounding an LLM. The source attribution must be near the displayed LLM content.

## GDPR compliance

With respect to any personal data subject to the European Union General Data Protection Regulation (GDPR) and that is processed in connection with calls to the Search APIs, Bing Spell Check API, or Bing Autosuggest API, you understand that you and Microsoft are independent data controllers under the GDPR. You are independently responsible for your compliance with the GDPR.
