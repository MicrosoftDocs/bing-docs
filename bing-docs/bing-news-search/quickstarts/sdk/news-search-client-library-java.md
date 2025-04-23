---
title: Bing News Search Java client library quickstart 
titleSuffix: Bing Search Services
description: The News Search API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this Java quickstart to send search requests and get back results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: v-grvanpelt
---

# Quickstart: Use the Bing News Search Java client library

Use this quickstart to begin searching for news with the Bing News Search client library for Java. While Bing News Search has a REST API compatible with most programming languages, the client library provides an easy way to integrate the service into your applications. The source code for this sample can be found on [GitHub](https://github.com/microsoft/bing-search-sdk-for-java/tree/main/samples/sdk/NewsSearchSample).

## Prerequisites

Install the Bing News Search client library dependencies using Maven, Gradle, or another dependency management system. The Maven POM file requires the following declaration:

```xml
    <dependencies>
    <dependency>
        <groupId>com.microsoft.azure.cognitiveservices</groupId>
        <artifactId>azure-cognitiveservices-newssearch</artifactId>
        <version>0.0.1-beta-SNAPSHOT</version>
    </dependency>
    </dependencies>
```

<!--
[!INCLUDE [bing-news-search-signup-requirements](../../../../includes/bing-news-search-signup-requirements.md)]
-->

## Create and initialize a project

Create a new Java project in your favorite IDE or editor, and import the following libraries.

```java
import com.microsoft.azure.cognitiveservices.newssearch.*;
import com.microsoft.azure.cognitiveservices.newssearch.implementation.NewsInner;
import com.microsoft.azure.cognitiveservices.newssearch.implementation.NewsSearchAPIImpl;
import com.microsoft.azure.cognitiveservices.newssearch.implementation.TrendingTopicsInner;
import com.microsoft.rest.credentials.ServiceClientCredentials;
import okhttp3.Interceptor;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;
import java.io.IOException;
```

## Create a search client and store credentials

1. Create a method called `getClient()` that returns a new `NewsSearchAPIImpl` search client. Add your endpoint as the first parameter for the new`NewsSearchAPIImpl` object, and a new `ServiceClientCredentials` object to store your credentials. 

    ```java
    public static NewsSearchAPIImpl getClient(final String subscriptionKey) {
        return new NewsSearchAPIImpl("https://api.bing.microsoft.com/bing/v7.0/",
                new ServiceClientCredentials() {
                });
    }
    ```

2. To create the `ServiceClientCredentials` object, override the `applyCredentialsFilter()` function. Pass a `OkHttpClient.Builder` to the method, and use the builder's `addNetworkInterceptor()` method to create your credentials for the client library call.

    ```java
    new ServiceClientCredentials() {
        @Override
        public void applyCredentialsFilter(OkHttpClient.Builder builder) {
            builder.addNetworkInterceptor(
                    new Interceptor() {
                        @Override
                        public Response intercept(Chain chain) throws IOException {
                            Request request = null;
                            Request original = chain.request();
                            // Request customization: add request headers.
                            Request.Builder requestBuilder = original.newBuilder()
                                    .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
                            request = requestBuilder.build();
                            return chain.proceed(request);
                        }
                    });
        }
    });
    ```

## Send and receive a search request

1. Create a method that calls `getClient()` and sends a search request to the Bing News Search service. Filter the search with the *market* and *count* parameters, then print information about the first news result: name, URL, publication date, description, provider name, and total number of estimated matches for your search.

    ```java
    public static void newsSearch(String subscriptionKey)
    {
        NewsSearchAPIImpl client = getClient(subscriptionKey);
        String searchTerm = "Quantum Computing";
    
        NewsInner newsResults = client.searchs().list(searchTerm, null, null, null,
                null, null, 100, null, "en-us",
                null, null, null, null, null,
                null, null);
    
        if (newsResults.value().size() > 0)
        {
            NewsArticle firstNewsResult = newsResults.value().get(0);
    
            System.out.println(String.format("TotalEstimatedMatches value: %d", newsResults.totalEstimatedMatches()));
            System.out.println(String.format("News result count: %d", newsResults.value().size()));
            System.out.println(String.format("First news name: %s", firstNewsResult.name()));
            System.out.println(String.format("First news url: %s", firstNewsResult.url()));
            System.out.println(String.format("First news description: %s", firstNewsResult.description()));
            System.out.println(String.format("First news published time: %s", firstNewsResult.datePublished()));
            System.out.println(String.format("First news provider: %s", firstNewsResult.provider().get(0).name()));
        }
        else
        {
            System.out.println("Couldn't find news results!");
        }
    
    }
    
    ```

2. Add your search method to a `main()` method to execute the code.

    ```java 
    public static void main(String[] args) {
        String subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
        NewsSearchSDK.newsSearch(subscriptionKey);
    }
    ```

## Next steps

> [!div class="nextstepaction"]
> [Create a single-page web app](../../tutorial/bing-news-search-single-page-app.md)
