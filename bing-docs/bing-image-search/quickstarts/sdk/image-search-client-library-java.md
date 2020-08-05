---
title: Bing Image Search Java client library quickstart 
titleSuffix: Bing Search Services
description: The Image Search API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this quickstart to send search requests and get back results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing Image Search Java client library

Use this quickstart to make your first image search using the Bing Image Search client library, which is a wrapper for the API and contains the same features. This simple Java application sends an image search query, parses the JSON response, and displays the URL of the first image returned.

The source code for this sample is available [on GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingImageSearch/Quickstart) with additional error handling and annotations.

## Prerequisites

The latest version of the [Java Development Kit](https://aka.ms/azure-jdks) (JDK)

Install the Bing Image Search client library dependencies by using Maven, Gradle, or another dependency management system. The Maven POM file requires the following declaration:

```xml
 <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.cognitiveservices</groupId>
      <artifactId>azure-cognitiveservices-imagesearch</artifactId>
      <version>1.0.1</version>
    </dependency>
 </dependencies>
```

[!INCLUDE [bing-image-search-signup-requirements](../../../../includes/bing-image-search-signup-requirements.md)]

## Create and initialize the application

1. Create a new Java project in your favorite IDE or editor, and add the following imports to your class implementation:

    ```java
    import com.microsoft.azure.cognitiveservices.search.imagesearch.BingImageSearchAPI;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.BingImageSearchManager;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.models.ImageObject;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.models.ImagesModel;
    ```

2. In your main method create variables for your subscription key, and search term. Then instantiate the Bing Image Search client.

    ```java
    final String subscriptionKey = "COPY_YOUR_KEY_HERE";
    String searchTerm = "canadian rockies";
    //Image search client
    BingImageSearchAPI client = BingImageSearchManager.authenticate(subscriptionKey);
    ```

## Send a search request to the API

1. Using `bingImages().search()`, send the HTTP request containing the search query. Save the response as a `ImagesModel`.

   ```java
    ImagesModel imageResults = client.bingImages().search()
                .withQuery(searchTerm)
                .withMarket("en-us")
                .execute();
    ```

## Parse and view the result

Parse the image results returned in the response.
If the response contains search results, store the first result and print out its details, such as a thumbnail URL, the original URL, along with the total number of returned images.  

```java
if (imageResults != null && imageResults.value().size() > 0) {
    // Image results
    ImageObject firstImageResult = imageResults.value().get(0);

    System.out.println(String.format("Total number of images found: %d", imageResults.value().size()));
    System.out.println(String.format("First image thumbnail url: %s", firstImageResult.thumbnailUrl()));
    System.out.println(String.format("First image content url: %s", firstImageResult.contentUrl()));
}
else {
        System.out.println("Couldn't find image results!");
     }

```

## Next steps

> [!div class="nextstepaction"]
> [Bing Image Search single-page app tutorial](../../tutorial/bing-image-search-single-page-app.md)

## See also

* [What is Bing Image Search?](../../overview.md)  
* [Java samples for the Azure Cognitive Services SDK](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)
* [Bing Image Search API reference](../../reference/endpoints.md)
