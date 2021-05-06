---
title: Bing Autosuggest Search Java client library quickstart 
titleSuffix: Bing Search Services
description: The Autosuggest Search API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this Java quickstart to send search requests and get back results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-autosuggest-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing Autosuggest Search Java client library

Use this quickstart to begin Autosuggest with the Bing Autosuggest Search client library for Java. While Bing Autosuggest Search has a REST API compatible with most programming languages, the client library provides an easy way to integrate the service into your applications. The source code for this sample can be found on [GitHub](https://github.com/microsoft/bing-search-sdk-for-java/tree/main/samples/sdk/AutoSuggestSample).

## Prerequisites

Install the Bing AutoSuggest Search client library dependencies using Maven, Gradle, or another dependency management system. The Maven POM file requires the following declaration:

```xml
    <dependencies>
    <dependency>
      <groupId>com.microsoft.bing</groupId>
      <artifactId>bing-autosuggest</artifactId>
      <version>1.0.0</version>
    </dependency>
    </dependencies>
```


## Create and initialize a project

Create a new Java project in your favorite IDE or editor, and import the following libraries.

```java
package com.microsoft.bing.samples;

import com.microsoft.bing.autosuggest.models.SearchAction;
import com.microsoft.bing.autosuggest.models.Suggestions;
import com.microsoft.bing.autosuggest.implementation.AutoSuggestClientImpl;
import com.microsoft.rest.credentials.ServiceClientCredentials;
import okhttp3.*;
import okhttp3.OkHttpClient.Builder;
import java.io.IOException;
```

## Create a search client and store credentials

1. Create  a new `AutoSuggestClientImpl` search client. Add your endpoint as the first parameter for the new`AutoSuggestClientImpl` object, and a new `ServiceClientCredentials` object to store your credentials. 

    ```java
    AutoSuggestClientImpl client = new AutoSuggestClientImpl(endpoint,credentials);
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

1. Create a method that calls `getClient()` and sends a search request to the Bing Autosuggest Search service. Filter the search with the *market* and *count* parameters, then print information about the Autosuggest.

    ```java
    public static boolean runSample(AutoSuggestClientImpl client) {
        try {

            //=============================================================
            // This will request suggestions for "Satya Nadella" and print out the results

            System.out.println("Searched for \"Satya Nadella\" and print out the returned suggestions");

            Suggestions suggestions = client.autoSuggest("Satya Nadella");
            if (suggestions != null && suggestions.suggestionGroups() != null && suggestions.suggestionGroups().size() > 0) {
                System.out.println("Found the following suggestions:");
                for (SearchAction suggestion: suggestions.suggestionGroups().get(0).searchSuggestions()) {
                    System.out.println("....................................");
                    System.out.println(suggestion.query());
                    System.out.println(suggestion.displayText());
                    System.out.println(suggestion.url());
                    System.out.println(suggestion.searchKind());
                }
            } else {
                System.out.println("Didn't see any suggestion...");
            }

            return true;
        } catch (Exception f) {
            System.out.println(f.getMessage());
            f.printStackTrace();
        }
        return false;
    }

    
    ```

2. Add your search method to a `main()` method to execute the code.

    ```java 
    public static void main(String[] args) {
        try {
            //=============================================================
            // Authenticate
            // Add your Bing Autosuggest subscription key to your environment variables.
            String subscriptionKey = System.getenv("BING_AUTOSUGGEST_SUBSCRIPTION_KEY");
            String endpoint = System.getenv("BING_AUTOSUGGEST_ENDPOINT") +  "/v7.0";
            ServiceClientCredentials credentials = new ServiceClientCredentials() {
                @Override
                public void applyCredentialsFilter(Builder builder) {
                    builder.addNetworkInterceptor(
                        new Interceptor() {
                            @Override
                            public Response intercept(Chain chain) throws IOException {
                                Request request = null;
                                Request original = chain.request();
                                Request.Builder requestBuilder = original.newBuilder();
                                requestBuilder.addHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
                                request = requestBuilder.build();
                                return chain.proceed(request);
                            }
                        }
                    );
                }
            };
            AutoSuggestClientImpl client = new AutoSuggestClientImpl(endpoint,credentials);
            runSample(client);
        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
    ```

## Next steps

> [!div class="nextstepaction"]
