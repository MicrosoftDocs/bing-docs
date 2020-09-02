---
title: Bing Visual Search Java client library quickstart 
titleSuffix: Bing Search Services
description: The Visual Search API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this quickstart to send search requests and get back results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing Visual Search Java client library

Use this quickstart to begin getting image insights from the Bing Visual Search service, using the Java client library. While Bing Visual Search has a REST API compatible with most programming languages, the client library provides an easy way to integrate the service into your applications. The source code for this quickstart can be found on [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingVisualSearch).

Use the Bing Visual Search client library for Java to:

* Upload an image to send a visual search request.
* Get the image insight token and visual search tags.

[Reference documentation](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/bingvisualsearch?view=azure-java-stable) | [Library source code](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Search.BingVisualSearch) | [Artifact (Maven)](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-visualsearch/) | [Samples](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)

## Prerequisites

* Azure subscription - [Create one for free](https://azure.microsoft.com/free/cognitive-services/)
* The current version of the [Java Development Kit(JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* The [Gradle build tool](https://gradle.org/install/), or another dependency manager

[!INCLUDE [bing-visual-search-signup-requirements](../../../../includes/bing-visual-search-signup-requirements.md)]

After you get a key from your resource, [create an environment variable](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) for the key, named `BING_SEARCH_V7_SUBSCRIPTION_KEY`.

### Create a new Gradle project

In a console window (such as cmd, PowerShell, or Bash), create a new directory for your app, and navigate to it. 

```console
mkdir myapp && cd myapp
```

Run the `gradle init` command from your working directory. This command will create essential build files for Gradle, including *build.gradle.kts* which is used at runtime to create and configure your application.

```console
gradle init --type basic
```

When prompted to choose a **DSL**, select **Kotlin**.

Locate *build.gradle.kts* and open it with your preferred IDE or text editor. Then copy in this build configuration:

```kotlin
plugins {
    java
    application
}
application {
    mainClassName = "main.java.BingVisualSearchSample"
}
repositories {
    mavenCentral()
}
dependencies {
    compile("org.slf4j:slf4j-simple:1.7.25")
    compile("com.microsoft.azure.cognitiveservices:azure-cognitiveservices-visualsearch:1.0.2-beta")
    compile("com.google.code.gson:gson:2.8.5")
}
```

Create a folder for your sample app. From your working directory, run the following command:

```console
mkdir -p src/main/java
```

Create a folder for the image you want to upload to the API. Place the image inside the **resources** folder.

```console
mkdir -p src/main/resources
``` 

Navigate to the new folder and create a file called *BingVisualSearchSample.java*. Open it in your preferred editor or IDE and add the following `import` statements:

<!--
[!code-java[Import statements](~/cognitive-services-java-sdk-samples/Search/BingVisualSearch/src/main/java/BingVisualSearchSample.java?name=imports)]
-->

```java
package main.java;

import com.google.common.io.ByteStreams;
import com.google.gson.Gson;
import com.microsoft.azure.cognitiveservices.search.visualsearch.BingVisualSearchAPI;
import com.microsoft.azure.cognitiveservices.search.visualsearch.BingVisualSearchManager;
import com.microsoft.azure.cognitiveservices.search.visualsearch.models.CropArea;
import com.microsoft.azure.cognitiveservices.search.visualsearch.models.ErrorResponseException;
import com.microsoft.azure.cognitiveservices.search.visualsearch.models.Filters;
import com.microsoft.azure.cognitiveservices.search.visualsearch.models.ImageInfo;
import com.microsoft.azure.cognitiveservices.search.visualsearch.models.ImageKnowledge;
import com.microsoft.azure.cognitiveservices.search.visualsearch.models.ImageTag;
import com.microsoft.azure.cognitiveservices.search.visualsearch.models.KnowledgeRequest;
import com.microsoft.azure.cognitiveservices.search.visualsearch.models.VisualSearchRequest;```

Then create a new class.

```java
public class BingVisualSearchSample {
}
```

In the application's `main` method, create variables for your resource's Azure endpoint and key. If you created the environment variable after you launched the application, you will need to close and reopen the editor, IDE, or shell running it to access the variable. Then create a `byte[]` for the image you'll be uploading. Create a `try` block for the methods you'll define  later, and load the image and convert it to bytes using `toByteArray()`.

<!--
[!code-java[Main method](~/cognitive-services-java-sdk-samples/Search/BingVisualSearch/src/main/java/BingVisualSearchSample.java?name=main)]
-->

```java
    public static void main(String[] args) {

        // Set the BING_SEARCH_V7_SUBSCRIPTION_KEY environment variable with your subscription key,
        // then reopen your command prompt or IDE. If not, you may get an API key not found exception.
        final String subscriptionKey = System.getenv("BING_SEARCH_V7_SUBSCRIPTION_KEY");

        BingVisualSearchAPI client = BingVisualSearchManager.authenticate(subscriptionKey);

        //runSample(client);
        byte[] imageBytes;

        try {
            imageBytes = ByteStreams.toByteArray(ClassLoader.getSystemClassLoader().getResourceAsStream("image.jpg"));
            visualSearch(client, imageBytes);
            searchWithCropArea(client, imageBytes);
            // wait 1 second to avoid rate limiting
            Thread.sleep(1000);
            searchWithFilter(client);
            searchUsingCropArea(client);
            searchUsingInsightToken(client);
        }
        catch (java.io.IOException f) {
            System.out.println(f.getMessage());
            f.printStackTrace();
        }
        catch (java.lang.InterruptedException f){
            f.printStackTrace();
        }

    }
```

### Install the client library

This quickstart uses the Gradle dependency manager. You can find the client library and information for other dependency managers on the [Maven Central Repository](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-textanalytics/).

In your project's *build.gradle.kts* file, be sure to include the client library as an `implementation` statement. 

```kotlin
dependencies {
    compile("org.slf4j:slf4j-simple:1.7.25")
    compile("com.microsoft.azure.cognitiveservices:azure-cognitiveservices-visualsearch:1.0.2-beta")
    compile("com.google.code.gson:gson:2.8.5")
}
```

## Code examples

These code snippets show you how to do the following tasks with the Bing Visual Search client library and Java:

* [Authenticate the client](#authenticate-the-client)
* [Send a visual search request](#send-a-visual-search-request)
* [Print the image insight token and visual search tags](#print-the-image-insight-token-and-visual-search-tags)

## Authenticate the client

> [!NOTE]
> This quickstart assumes you've [created an environment variable](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) for your Bing Visual Search key, named `BING_SEARCH_V7_SUBSCRIPTION_KEY`.


In your main method, be sure to use your subscription key to instantiate a [BingVisualSearchAPI](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.visualsearch.bingvisualsearchapi?view=azure-java-stable) object.

```csharp
BingVisualSearchAPI client = BingVisualSearchManager.authenticate(subscriptionKey);
```

## Send a visual search request

In a new method, send the image byte array (which was created in the `main()` method) using the client's [bingImages().visualSearch()](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.visualsearch.bingimages.visualsearch?view=azure-java-stable#com_microsoft_azure_cognitiveservices_search_visualsearch_BingImages_visualSearch__) method. 

<!--
[!code-java[visualSearch() method](~/cognitive-services-java-sdk-samples/Search/BingVisualSearch/src/main/java/BingVisualSearchSample.java?name=visualSearch)]
-->

```java
    public static void visualSearch(BingVisualSearchAPI client, byte[] imageBytes){
        System.out.println("Calling Bing Visual Search with image binary");
        ImageKnowledge visualSearchResults = client.bingImages().visualSearch()
                .withImage(imageBytes)
                .execute();
        PrintVisualSearchResults(visualSearchResults);

    }
```

## Print the image insight token and visual search tags

Check if the [ImageKnowledge](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.visualsearch.models.imageknowledge?view=azure-java-stable) object is null. If not, print the image insights token, the number of tags, the number of actions, and the first action type.

<!--
[!code-java[Print token and tags](~/cognitive-services-java-sdk-samples/Search/BingVisualSearch/src/main/java/BingVisualSearchSample.java?name=printVisualSearchResults)]
-->

```java
    static void PrintVisualSearchResults(ImageKnowledge visualSearchResults) {
        if (visualSearchResults == null) {
            System.out.println("No visual search result data.");
        } else {
            // Print token

            if (visualSearchResults.image() != null && visualSearchResults.image().imageInsightsToken() != null) {
                System.out.println("Found uploaded image insights token: " + visualSearchResults.image().imageInsightsToken());
            } else {
                System.out.println("Couldn't find image insights token!");
            }

            // List tags

            if (visualSearchResults.tags() != null && visualSearchResults.tags().size() > 0) {
                System.out.format("Found visual search tag count: %d\n", visualSearchResults.tags().size());
                ImageTag firstTagResult = visualSearchResults.tags().get(0);

                // List of actions in first tag

                if (firstTagResult.actions() != null && firstTagResult.actions().size() > 0) {
                    System.out.format("Found first tag action count: %d\n", firstTagResult.actions().size());
                    System.out.println("Found first tag action type: " + firstTagResult.actions().get(0).actionType());
                }
            } else {
                System.out.println("Couldn't find image tags!");
            }
        }
    }
```

## Run the application

You can build the app with:

```console
gradle build
```

Run the application with the `run` goal:

```console
gradle run
```


## Next steps

> [!div class="nextstepaction"]
> [Build a single-page web app](../../tutorial/visual-search-single-page-app.md)
