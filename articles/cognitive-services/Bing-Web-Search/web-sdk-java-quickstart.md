---
title: Web Search SDK Java Quick Start | Microsoft Docs
description: Instellingen voor Web Search SDK-consoletoepassing.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 02/16/2018
ms.author: v-gedod
ms.openlocfilehash: 73af92ca0d6ed1a270eaea26e79c7341680dce17
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345728"
---
# <a name="web-search-sdk-java-quickstart"></a>Web Search SDK Java Quick Start

De Bing Web Search SDK bevat de functionaliteit van de REST-API voor web-query's en parseren resultaten. 

De [broncode voor voorbeelden van Java Bing Web Search SDK](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingWebSearch) beschikbaar is op de Git-Hub.

## <a name="application-dependencies"></a>Afhankelijkheden voor toepassingen
Ophalen van een [cognitieve toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) onder *Search*. Installeren met behulp van Maven, Gradle of een ander beheersysteem van afhankelijkheid Bing Web Search SDK-afhankelijkheden. Het bestand Maven POM is vereist:
```
  <dependencies>
    <dependency>
        <groupId>com.microsoft.azure.cognitiveservices</groupId>
        <artifactId>azure-cognitiveservices-websearch</artifactId>
        <version>0.0.1-beta-SNAPSHOT</version>
    </dependency>
  </dependencies>
```
## <a name="web-search-client"></a>WebClient zoeken
Invoer toevoegen aan de implementatie van de klasse:
```
import com.microsoft.azure.cognitiveservices.websearch.*;
import com.microsoft.azure.cognitiveservices.websearch.implementation.SearchResponseInner;
import com.microsoft.azure.cognitiveservices.websearch.implementation.WebSearchAPIImpl;
import com.microsoft.rest.credentials.ServiceClientCredentials;
import okhttp3.Interceptor;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
```
Implementeer de `WebSearchAPIImpl` -client, die een exemplaar van vereist de `ServiceClientCredentials`:
```
public static WebSearchAPIImpl getClient(final String subscriptionKey) {
    return new WebSearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
            new ServiceClientCredentials() {
                @Override
                public void applyCredentialsFilter(OkHttpClient.Builder builder) {
                    builder.addNetworkInterceptor(
                            new Interceptor() {
                                @Override
                                public Response intercept(Chain chain) throws IOException {
                                    Request request = null;
                                    Request original = chain.request();
                                    // Request customization: add request headers
                                    Request.Builder requestBuilder = original.newBuilder()
                                            .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
                                    request = requestBuilder.build();
                                    return chain.proceed(request);
                                }
                            });
                }
            });
}

```
Zoeken naar resultaten.  De volgende methode met een enkele query 'Xbox', zoekt en afgedrukt `name`, en `URL` voor de eerste web, afbeeldingen, nieuws en video's resultaten.
```
public static void WebSearchResultTypesLookup(String subscriptionKey)
{
    WebSearchAPIImpl client = getClient(subscriptionKey);

    try
    {
        SearchResponseInner webData = client.webs().search("Yosemite");
        System.out.println("\r\nSearched for Query# \" Yosemite \"");

        //WebPages
        if (webData.webPages().value().size() > 0)
        {
            // find the first web page
            WebPage firstWebPagesResult = webData.webPages().value().get(0);

            if (firstWebPagesResult != null)
            {
                System.out.println(String.format("Webpage Results#%d", webData.webPages().value().size()));
                System.out.println(String.format("First web page name: %s ", firstWebPagesResult.name()));
                System.out.println(String.format("First web page URL: %s ", firstWebPagesResult.url()));
            }
            else
            {
                System.out.println("Couldn't find web results!");
            }
        }
        else
        {
            System.out.println("Didn't see any Web data..");
        }

        //Images
        if (webData.images().value().size() > 0)
        {
            // find the first image result
            ImageObject firstImageResult = webData.images().value().get(0);

            if (firstImageResult != null)
            {
                System.out.println(String.format("Image Results#%d", webData.images().value().size()));
                System.out.println(String.format("First Image result name: %s ", firstImageResult.name()));
                System.out.println(String.format("First Image result URL: %s ", firstImageResult.contentUrl()));
            }
            else
            {
                System.out.println("Couldn't find first image results!");
            }
        }
        else
        {
            System.out.println("Didn't see any image data..");
        }

        //News
        if (webData.news().value().size() > 0)
        {
            // find the first news result
            NewsArticle firstNewsResult = webData.news().value().get(0);

            if (firstNewsResult != null)
            {
                System.out.println(String.format("News Results#%d", webData.news().value().size()));
                System.out.println(String.format("First news result name: %s ", firstNewsResult.name()));
                System.out.println(String.format("First news result URL: %s ", firstNewsResult.url()));
            }
            else
            {
                System.out.println("Couldn't find any News results!");
            }
        }
        else
        {
            System.out.println("Didn't see first news data..");
        }

        //Videos
        if (webData.videos() != null && webData.videos().value().size() > 0)
        {
            // find the first video result
            VideoObject firstVideoResult = webData.videos().value().get(0);

            if (firstVideoResult != null)
            {
                System.out.println(String.format("Video Results#%s", webData.videos().value().size()));
                System.out.println(String.format("First Video result name: %s ", firstVideoResult.name()));
                System.out.println(String.format("First Video result URL: %s ", firstVideoResult.contentUrl()));
            }
            else
            {
                System.out.println("Couldn't find first video results!");
            }
        }
        else
        {
            System.out.println("Didn't see any video data..");
        }
    }

    catch (ErrorResponseException ex)
    {
        System.out.println("Encountered exception. " + ex.getLocalizedMessage());
    }

}

```
Zoek naar 'Best restaurant in Haarlem', aantal resultaten controleren en afdrukken `name` en `URL` van het eerste resultaat:
```
public static void WebResultsWithCountAndOffset(String subscriptionKey)
{
    WebSearchAPIImpl client = getClient(subscriptionKey);

    try
    {
        SearchResponseInner webData = client.webs().search(
                "Best restaurants in Seattle", null, null, null, null, null, 10, null, 20, null, "en-us", null,
                        null, null, SafeSearch.STRICT, null, null, null);
        System.out.println("\r\nSearched for Query# \" Best restaurants in Seattle \"");

        if (webData.webPages().value().size() > 0)
        {
            // find the first web page
            WebPage firstWebPagesResult = webData.webPages().value().get(0);

            if (firstWebPagesResult != null)
            {
                System.out.println(String.format("Web Results#%d", webData.webPages().value().size()));
                System.out.println(String.format("First web page name: %s ", firstWebPagesResult.name()));
                System.out.println(String.format("First web page URL: %s ", firstWebPagesResult.url()));
            }
            else
            {
                System.out.println("Couldn't find first web result!");
            }
        }
        else
        {
            System.out.println("Didn't see any Web data..");
        }
    }
    catch (ErrorResponseException ex)
    {
        System.out.println("Encountered exception. " + ex.getLocalizedMessage());
    }
}

```
Zoek naar 'Microsoft' met antwoordfilters toegewezen aan `news`. Details van de eerste nieuwsitem afdrukken.
```
public static void WebSearchWithResponseFilter(String subscriptionKey)
{
    WebSearchAPIImpl client = getClient(subscriptionKey);

    try
    {
        List<AnswerType> responseFilterstrings = new ArrayList<AnswerType>();
        responseFilterstrings.add(AnswerType.NEWS);
        SearchResponseInner webData = client.webs().search(
        "Best restaurants in Seattle", null, null, null, null, null, 10, null, 20, null, "en-us", null,
                null, responseFilterstrings, SafeSearch.STRICT, null, null, null);

        System.out.println("\\r\\nSearched for Query# \" Microsoft \" with response filters \"news\"");

        //News
        if (webData.news() != null && webData.news().value().size() > 0)
        {
            // find the first news result
            NewsArticle firstNewsResult = webData.news().value().get(0);

            if (firstNewsResult != null)
            {
                System.out.println(String.format("News Results#%d", webData.news().value().size()));
                System.out.println(String.format("First news result name: %s ", firstNewsResult.name()));
                System.out.println(String.format("First news result URL: %s ", firstNewsResult.url()));
            }
            else
            {
                System.out.println("Couldn't find first News results!");
            }
        }
        else
        {
            System.out.println("Didn't see any News data..");
        }

    }
    catch (ErrorResponseException ex)
    {
        System.out.println("Encountered exception. " + ex.getLocalizedMessage());
    }
}

```
Zoeken met de query 'Niagara valt', met `answerCount` en `promote` parameters. Details van de resultaten afdrukken.
```
public static void WebSearchWithAnswerCountPromoteAndSafeSearch(String subscriptionKey)
{
    WebSearchAPIImpl client = getClient(subscriptionKey);

    try
    {
        List<AnswerType> promoteAnswertypeStrings = new ArrayList<AnswerType>();
        promoteAnswertypeStrings.add(AnswerType.VIDEOS);
        SearchResponseInner webData = client.webs().search(
            "Niagara Falls", null, null, null, null, null, 10, null, 20, null, "en-us", null,
            promoteAnswertypeStrings, null, SafeSearch.STRICT, null, null, null);
        System.out.println("\r\nSearched for Query# \" Niagara Falls \"");

        if (webData.videos().value().size() > 0)
        {
            VideoObject firstVideosResult = webData.videos().value().get(0);

            if (firstVideosResult != null)
            {
                System.out.println(String.format("Video Results#%d", webData.videos().value().size()));
                System.out.println(String.format("First Video result name: %s ", firstVideosResult.name()));
                System.out.println(String.format("First Video result URL: %s ", firstVideosResult.contentUrl()));
            }
            else
            {
                System.out.println("Couldn't find videos results!");
            }
        }
        else
        {
            System.out.println("Didn't see any data..");
        }
    }
    catch (ErrorResponseException ex)
    {
        System.out.println("Encountered exception. " + ex.getLocalizedMessage());
    }
}

```
De vorige methoden toevoegen aan een klasse met de belangrijkste functie de code uit te voeren:
```
package javaWebSDK;
import com.microsoft.azure.cognitiveservices.websearch.*;
public class webSDK{
    
    public static void main(String [ ] args) {
        
        WebSearchResultTypesLookup("YOUR-SUBSCRIPTION-KEY");
        WebResultsWithCountAndOffset("YOUR-SUBSCRIPTION-KEY");
        WebSearchWithResponseFilter("YOUR-SUBSCRIPTION-KEY");
        WebSearchWithAnswerCountPromoteAndSafeSearch("YOUR-SUBSCRIPTION-KEY");
        
    }
    // Add methods previoiusly documented.
}
```
## <a name="next-steps"></a>Volgende stappen

[Cognitieve Services Java SDK-voorbeelden](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)


