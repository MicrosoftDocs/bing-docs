---
title: What is the Bing Spell Check API?
titleSuffix: Bing Search Services
description: Learn about the Bing Spell Check API, which uses machine learning and statistical machine translation for contextual spell checking.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-spell-check
ms.topic: overview
ms.date: 07/15/2020
ms.author: scottwhi
---

# What is the Bing Spell Check API?

The Bing Spell Check API enables you to perform contextual grammar and spell checking on text. While most spell-checkers rely on dictionary-based rule sets, the Bing spell-checker leverages machine learning and statistical machine translation to provide accurate and contextual corrections. 

## Features

| Feature | Description |
|---------|---------|
|Multiple spell check modes     | Multiple spell check modes enable you to get corrections focused on grammar and/or spelling. |
|Slang and informal language recognition     | Recognize common expressions and informal terms used in text.         |
|Differentiate between similar words     | Find the correct usage between words that sound similar but differ in meaning (for example, "see" and "sea")        |
|Brand, title, and popular usage support     | Recognize new brands, titles, and other popular expressions as they emerge |

## Workflow

The Bing Spell Check API is easy to call from any programming language that can make HTTP requests and parse JSON responses. The service is accessible using the REST API or the Bing Spell Check SDKs. 

1. Create a [Cognitive Services API account](../cognitive-services-apis-create-account.md) with access to the Bing Search APIs. If you don't have an Azure subscription, you can create a free account. 
2. Send a request to the Bing Web Search API.
3. Parse the JSON response

