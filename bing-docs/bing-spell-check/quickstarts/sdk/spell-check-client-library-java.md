---
title: Bing SpellCheck Java client library quickstart 
titleSuffix: Bing Sesarch Services
description: The SpellCheck API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this Java quickstart to send search requests and get back results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-spellcheck
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing SpellCheck Java client library

Use this quickstart to begin searching for news with the Bing SpellCheck client library for Java. While Bing SpellCheck has a REST API compatible with most programming languages, the client library provides an easy way to integrate the service into your applications. The source code for this sample can be found on [GitHub](https://github.com/microsoft/bing-search-sdk-for-java/tree/main/samples/sdk/SpellCheckSample), with additional annotations, and features.

## Prerequisites

* The [Java Development Kit(JDK)](https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html)

* The [Gson library](https://github.com/google/gson)


Install the Bing SpellCheck client library dependencies by using Maven, Gradle, or another dependency management system. The Maven POM file requires the following declaration:

```xml
  <dependencies>
    <dependency>
      <groupId>com.microsoft.bing</groupId>
      <artifactId>bing-spellcheck</artifactId>
      <version>1.0.0</version>
    </dependency>
  </dependencies> 
```

## Create and initialize a project


Create a new Java project in your favorite IDE or editor, and import the following libraries.

```java
    package com.microsoft.bing.samples;

    import com.microsoft.bing.spellcheck.models.Mode;
    import com.microsoft.bing.spellcheck.models.SpellCheck;
    import com.microsoft.bing.spellcheck.models.SpellingFlaggedToken;
    import com.microsoft.bing.spellcheck.models.SpellingTokenSuggestion;
    import com.microsoft.bing.spellcheck.implementation.SpellCheckClientImpl;
    import com.microsoft.rest.credentials.ServiceClientCredentials;
    import okhttp3.*;
    import okhttp3.OkHttpClient.Builder;
    import java.io.IOException;
    import java.util.List;
```

## Create a search client

1. Implement the `SpellCheckClientImpl` client, which requires your API endpoint, and an instance of the `ServiceClientCredentials` class.

    ```java
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
    SpellCheckClientImpl client = new SpellCheckClientImpl(endpoint,credentials);
    ```

    To implement `ServiceClientCredentials`, follow these steps:

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

## Send a search request and receive the response 

1.  Send a Spell Check request using the client, with `SwiftKey` as the search term. If the spellcheck API returned a result, print them 
    
    ```java
    Mode proofmode = Mode.fromString("proof");
    SpellCheck result = client.spellChecker("Bill Gatas", null, null, null, null, null, null, null, null, null, null, null, "en-us", null, null,null, proofmode, null, null);


    if (result.flaggedTokens().size() > 0)
            {
                // find the first spellcheck result
                SpellingFlaggedToken firstspellCheckResult = result.flaggedTokens().get(0);

                if (firstspellCheckResult != null)
                {
                    System.out.println(String.format("SpellCheck Results#%d", result.flaggedTokens().size()));
                    System.out.println(String.format("First SpellCheck Result token: %s ", firstspellCheckResult.token()));
                    System.out.println(String.format("First SpellCheck Result Type: %s ", firstspellCheckResult.type()));
                    System.out.println(String.format("First SpellCheck Result Suggestion Count: %d ",
                            firstspellCheckResult.suggestions().size()));

                    List<SpellingTokenSuggestion> suggestions = firstspellCheckResult.suggestions();
                    if (suggestions.size() > 0)
                    {
                        SpellingTokenSuggestion firstSuggestion = suggestions.get(0);
                        System.out.println(String.format("First SpellCheck Suggestion Score: %f ", firstSuggestion.score()));
                        System.out.println(String.format("First SpellCheck Suggestion : %s ", firstSuggestion.suggestion()));
                    }
                }
                else
                {
                    System.out.println("Couldn't get any Spell check results!");
                }
            }
            else
            {
                System.out.println("Didn't see any SpellCheck results..");
            }
        }
    ```

3. Call the search method from your main method.

    ```java
        public static void main(String[] args) {
        try {
            //=============================================================
            // Authenticate
            // Set the BING_SEARCH_V7_SUBSCRIPTION_KEY environment variable with your subscription key, 
            // then reopen your command prompt or IDE. If not, you may get an API key not found exception.
            final String subscriptionKey = "8bcbe10fa0674c9c8366ea2e8d914250";

            // Add your Bing Search V7 endpoint to your environment variables.
            String endpoint = "https://api.bing.microsoft.com" + "/v7.0";
            
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
            SpellCheckClientImpl client = new SpellCheckClientImpl(endpoint,credentials);

            runSample(client);
        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
    ```

## Next steps

> [!div class="nextstepaction"]

## See also 

* [What is the Bing SpellCheck API?](../../overview.md)
* [BingApis .NET SDK samples](https://github.com/microsoft/bing-search-sdk-for-java/tree/main/samples/sdk/SpellCheckSample)
