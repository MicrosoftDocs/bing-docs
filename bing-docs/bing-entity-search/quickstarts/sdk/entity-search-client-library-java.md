---
title: Bing Entity Search Java client library quickstart 
titleSuffix: Bing Search Services
description: The Entity Search API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this Java quickstart to send search requests and get back results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing Entity Search Java client library

Use this quickstart to begin searching for entities with the Bing Entity Search client library for Java. While Bing Entity Search has a REST API compatible with most programming languages, the client library provides an easy way to integrate the service into your applications. The source code for this sample can be found on [GitHub](https://github.com/microsoft/bing-search-sdk-for-java/tree/main/samples/sdk/EntitySearchSample).

## Prerequisites

* The [Java Development Kit(JDK)](https://www.oracle.com/technetwork/java/javase/downloads/)

* The Bing Entity Search client library for Java

Install the Bing Entity Search client library dependencies by using Maven, Gradle, or another dependency management system. The Maven POM file requires the declaration:

```xml
<dependency>
    <groupId>com.microsoft.bing</groupId>
    <artifactId>bing-entitysearch</artifactId>
    <version>1.0.0</version>
</dependency>
```

<!--
[!INCLUDE [bing-news-search-signup-requirements](../../../../includes/bing-entity-search-signup-requirements.md)]
-->

## Create and initialize a project

1. Create a new Java project in your favorite IDE or editor, and import the following libraries.

    ```java
        package com.microsoft.bing.samples;
        import com.microsoft.bing.entitysearch.models.EntityScenario;
        import com.microsoft.bing.entitysearch.models.ErrorResponseException;
        import com.microsoft.bing.entitysearch.models.Place;
        import com.microsoft.bing.entitysearch.models.SearchResponse;
        import com.microsoft.bing.entitysearch.models.Thing;
        import com.microsoft.bing.entitysearch.implementation.EntitySearchClientImpl;
        import com.microsoft.rest.credentials.ServiceClientCredentials;
        import okhttp3.*;
        import okhttp3.OkHttpClient.Builder;
        import java.io.IOException;
        import java.util.List;
        import java.util.ArrayList;
    ```

2. Create a variable for your subscription key

    ```java
    String subscriptionKey = "your-key-here"
    ```

## Create a search client

1. Implement the  client, which requires your API endpoint, and an instance of the `ServiceClientCredentials` class. 

    ```java
    EntitySearchClientImpl client = new EntitySearchClientImpl(endpoint,credentials);
    ```

    To implement the `ServiceClientCredentials`, follow these steps:

   1. override the `applyCredentialsFilter()` function, with a `OkHttpClient.Builder` object as a parameter. 
        
       ```java
       //...
       new ServiceClientCredentials() {
               @Override
               public void applyCredentialsFilter(OkHttpClient.Builder builder) {
               //...
               }
       //...
       ```
    
   2. Within `applyCredentialsFilter()`, call `builder.addNetworkInterceptor()`. Create a new `Interceptor` object, and override its `intercept()` method to take a `Chain` interceptor object.

       ```java
       //...
       builder.addNetworkInterceptor(
           new Interceptor() {
               @Override
               public Response intercept(Chain chain) throws IOException {
               //...    
               }
           });
       ///...
       ```

   3. Within the `intercept` function, create variables for your request. Use `Request.Builder()` to build your request. Add your subscription key to the `Ocp-Apim-Subscription-Key` header, and return `chain.proceed()` on the request object.
            
       ```java
       //...
       public Response intercept(Chain chain) throws IOException {
           Request request = null;
           Request original = chain.request();
           Request.Builder requestBuilder = original.newBuilder()
                   .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
           request = requestBuilder.build();
           return chain.proceed(request);
       }
       //...
       ```
## Send a request and receive a response

1. Create a new instance of the search client with your subscription key. use `client.entities().search()` to send a search request for the search query `satya nadella`, and get a response. 
    
    ```java
        entityData = client.entities().search("satya nadella");
    ```

1. If any entities were returned, convert them into a list. Iterate through them, and print the dominant entity.

    ```java
    if (entityData.entities().value().size() > 0){
        // Find the entity that represents the dominant entity
        List<Thing> entries = entityData.entities().value();
        Thing dominateEntry = null;
        for(Thing thing : entries) {
            if(thing.entityPresentationInfo().entityScenario() == EntityScenario.DOMINANT_ENTITY) {
                System.out.println("\r\nSearched for \"Satya Nadella\" and found a dominant entity with this description:");
                System.out.println(thing.description());
                break;
            }
        }
    }
    ```

## Next steps

> [!div class="nextstepaction"]
> [Build a single-page web app](../../tutorial/bing-entities-search-single-page-app.md)

* [What is the Bing Entity Search API?](../../overview.md)
