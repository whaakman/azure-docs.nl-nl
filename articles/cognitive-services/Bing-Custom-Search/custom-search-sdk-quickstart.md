---
title: Aangepaste zoeken SDK C# Quick Start | Microsoft Docs
titleSuffix: Cognitive Services
description: Het instellen van aangepaste zoeken SDK C#-consoletoepassing.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 01/31/2018
ms.author: rosh
ms.openlocfilehash: 59b208b53bec974433c50c0e2304dc96bd9bd09e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344421"
---
# <a name="custom-search-sdk-c-quickstart"></a>Aangepaste zoeken SDK C# Quick Start

De Bing aangepaste Search SDK bevat de functionaliteit van de REST-API voor het zoeken van de entiteit en parseren van de resultaten.

## <a name="application-dependencies"></a>Afhankelijkheden voor toepassingen

Als u een consoletoepassing met de Bing aangepaste Search SDK instelt, blader naar de `Manage NuGet Packages` optie in Solution Explorer in Visual Studio. Voeg de `Microsoft.Azure.CognitiveServices.Search.CustomSearch` pakket.

Installeren van de [NuGet aangepaste zoekactie](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) pakket installeert ook afhankelijkheden, met inbegrip van de volgende assembly's:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="entity-search-client"></a>Entiteit zoeken client

Voor het maken van een exemplaar van de client CustomSearchAPI toevoegen met behulp van instructies:
```
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;

```

Exemplaar maken van de client aangepaste zoekactie: Vervang `YOUR-CUSTOM-SEARCH-KEY` en `YOUR-CUSTOM-CONFIG-ID` met uw toegangssleutel en de API-eindpuntconfiguratie ID toegewezen op [mijn exemplaren](https://www.customsearch.ai/).
```
var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));

```
De client gebruiken om te zoeken met de tekst van een query:
```
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;

```
## <a name="parse-the-results"></a>Parseren van de resultaten

De `SearchAsync` methode retourneert een `WebData` object met `WebPages` aangetroffen voor de query. Deze code vindt u het eerste resultaat en haalt de `Name` en `URL`.
```
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
 
Console.WriteLine("Searched for Query# \" Xbox \"");

 //WebPages
if (webData?.WebPages?.Value?.Count > 0)
{
    // find the first web page
    var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

    if (firstWebPagesResult != null)
    {
        Console.WriteLine("Webpage Results#{0}", webData.WebPages.Value.Count);
        Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
        Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
    }
    else
    {
        Console.WriteLine("Couldn't find web results!");
    }
}
else
{
    Console.WriteLine("Didn't see any Web data..");
}

```
## <a name="complete-console-application"></a>Volledige consoletoepassing

De volgende code van de query 'Xbox' gezocht en wordt aangegeven `Name` en `URL` voor eerste Webresultaat.
```
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;

namespace CustomSrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {

            var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));

            try
            {
                // This will look up a single query (Xbox).
                var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
                Console.WriteLine("Searched for Query# \" Xbox \"");

                //WebPages
                if (webData?.WebPages?.Value?.Count > 0)
                {
                    // find the first web page
                    var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

                    if (firstWebPagesResult != null)
                    {
                        Console.WriteLine("Webpage Results#{0}", webData.WebPages.Value.Count);
                        Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
                        Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find web results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any Web data..");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }

            Console.WriteLine("Any key to exit...");
            Console.ReadKey();
        }

    }
}


```

## <a name="next-steps"></a>Volgende stappen

[Cognitieve services .NET SDK-voorbeelden](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
