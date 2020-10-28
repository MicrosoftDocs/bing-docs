---
title: "Quickstart: Send a search request to the REST API using PHP - Bing Entity Search"
titleSuffix: Bing Search Services
description: Use this quickstart to send a request to Bing Entity Search REST API using PHP.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Send a search request to Bing Entity Search REST API using PHP

Use this quickstart to make your first call to Bing Entity Search API and view the JSON response. This simple PHP application sends a news search query to the API, and displays the response. 

Although this application is written in PHP, the API is a RESTful Web service compatible with most programming languages.

## Prerequisites

* [PHP 5.6.x](https://php.net/downloads.php) or later

<!--
[!INCLUDE [bing-news-search-signup-requirements](../../../../includes/bing-entity-search-signup-requirements.md)]
-->

## Search entities

To run this application, follow these steps:

1. Create a new PHP project in your favorite IDE.
2. Add the code provided below.
3. Replace the `key` value with an access key valid for your subscription.
5. Run the program.

```php
<?php

// NOTE: Be sure to uncomment the following line in your php.ini file.
// ;extension=php_openssl.dll

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
$subscriptionKey = 'ENTER KEY HERE';

$host = "https://api.bing.microsoft.com";
$path = "/v7.0/entities";

$mkt = "en-US";
$query = "italian restaurants near me";

function search ($host, $path, $key, $mkt, $query) {

	$params = '?mkt=' . $mkt . '&q=' . urlencode ($query);

	$headers = "Ocp-Apim-Subscription-Key: $key\r\n";

	// NOTE: Use the key 'http' even if you are making an HTTPS request. See:
	// https://php.net/manual/en/function.stream-context-create.php
	$options = array (
		'http' => array (
			'header' => $headers,
			'method' => 'GET'
		)
	);
	$context  = stream_context_create ($options);
	$result = file_get_contents ($host . $path . $params, false, $context);
	return $result;
}

$result = search ($host, $path, $subscriptionKey, $mkt, $query);

echo json_encode (json_decode ($result), JSON_PRETTY_PRINT);
?>
```

## Example JSON response

A successful response is returned in JSON, as shown in the following example: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "https://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

## Next steps

> [!div class="nextstepaction"]
> [Build a single-page web app](../../tutorial/bing-entities-search-single-page-app.md)

* [What is the Bing Entity Search API?](../../overview.md )
* [Bing Entity Search API reference](../../reference/endpoints.md).
