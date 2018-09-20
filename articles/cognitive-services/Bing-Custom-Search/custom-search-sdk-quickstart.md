---
title: Aangepaste Search SDK-C# snelstartgids | Microsoft Docs
titleSuffix: Cognitive Services
description: Setup Custom Search SDK C#-consoletoepassing.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 01/31/2018
ms.author: rosh
ms.openlocfilehash: 6b41dfbde0c2af776ee2c35220f731e40de334a0
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46367845"
---
# <a name="custom-search-sdk-c-quickstart"></a>C#-snelstart voor Aangepaste zoekopdrachten SDK

De Bing Custom Search SDK bevat de functionaliteit van de REST-API voor entiteiten zoeken en parseren van de resultaten.

De broncode voor dit voorbeeld is beschikbaar via [Github](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingCustomWebSearch).
## <a name="application-dependencies"></a>Afhankelijkheden van de toepassing

Als u een consoletoepassing met behulp van de Bing Custom Search SDK instelt, blader naar de `Manage NuGet Packages` optie vanuit de Solution Explorer in Visual Studio. Voeg het pakket `Microsoft.Azure.CognitiveServices.Search.CustomSearch` toe.

Installeren van de [NuGet Custom Search](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) pakket ook afhankelijkheden, met inbegrip van de volgende assembly's geïnstalleerd:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="entity-search-client"></a>Entiteit zoeken client

Voor het maken van een exemplaar van de client CustomSearchAPI using-instructies toevoegen:
```
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;

```

Exemplaar maken van de client aangepaste zoekopdrachten: Vervang `YOUR-CUSTOM-SEARCH-KEY` en `YOUR-CUSTOM-CONFIG-ID` met uw toegangssleutel en de configuratie van de API-eindpunten ID toegewezen op [mijn exemplaren](https://www.customsearch.ai/).
```
var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));

```
De client gebruiken om te zoeken met de tekst van een query:
```
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;

```
## <a name="parse-the-results"></a>Parseren van de resultaten

De `SearchAsync` methode retourneert een `WebData` -object dat bevat `WebPages` als deze worden gevonden voor de query. Met deze code vindt u het eerste resultaat en wordt de `Name` en `URL`.
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
## <a name="complete-console-application"></a>Consoletoepassing voltooien

De volgende code wordt gezocht voor query 'Xbox' en afgedrukt `Name` en `URL` voor eerste Webresultaat.
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

[Voorbeelden voor Cognitive Services .NET SDK](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
