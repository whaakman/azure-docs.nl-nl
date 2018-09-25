---
title: Aangepaste Search SDK-C# snelstartgids | Microsoft Docs
titleSuffix: Cognitive Services
description: Setup Custom Search SDK C#-consoletoepassing.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/06/2018
ms.author: scottwhi
ms.openlocfilehash: 6c9917e3a63515f36b386e444edcc53de07799fc
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46949924"
---
# <a name="c-sdk-quickstart"></a>SDK voor C#-snelstartgids

De Bing Custom Search SDK biedt een eenvoudiger programmeermodel dan de Bing Custom Search REST-API. In deze sectie helpt u bij het maken van uw eerste Custom Search-aanroepen met behulp van de SDK voor C#.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze quickstart te voltooien:

- Een exemplaar voor aangepast zoeken van kant-en-klare. Zie [maken van uw eerste exemplaar van de Bing Custom Search](quick-start.md).  
  
- De abonnementssleutel van een. Krijgt u een abonnementssleutel wanneer u activeert de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search), of kunt u een betaald abonnement-sleutel in uw Azure-dashboard (Zie [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).  
  
- Visual Studio 2017 is geïnstalleerd. Als u er nog geen hebt, kunt u downloaden de **gratis** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).  
  
- De [NuGet Custom Search](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) pakket is geïnstalleerd. Vanuit de Solution Explorer in Visual Studio, met de rechtermuisknop op uw project en selecteer `Manage NuGet Packages` in het menu. Installeer het `Microsoft.Azure.CognitiveServices.Search.CustomSearch`-pakket.

Installatie van het pakket NuGet Custom Search, installeert ook de volgende assembly's:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json



## <a name="run-the-code"></a>De code uitvoeren

Als u wilt uitvoeren in het volgende voorbeeld, de volgende stappen uit:

1. Open Visual Studio 2017.
  
2. Klik op de **bestand** menu, klikt u op **nieuw**, **Project**, en vervolgens de **Visual C#-consoletoepassing** sjabloon.
  
3. Naam van uw oplossing CustomSearchSolution en blader naar de map in te zetten.
  
4. Klik op OK als u wilt maken van de oplossing.  
  
4. Vanuit de Solution Explorer met de rechtermuisknop op het project (deze heeft dezelfde naam als de oplossing) en selecteer `Manage NuGet Packages`. Klik op **Bladeren** in de NuGet Package Manager. Voer Microsoft.Azure.CognitiveServices.Search.CustomSearch in het zoekvak en druk op enter. Selecteer het pakket en klik op installeren.  
  
4. Kopieer de volgende code in het bestand Program.cs. Vervang **uw-SUBSCRIPTION-KEY** en **uw-aangepaste-CONFIG-ID** met uw abonnementssleutel en de configuratie-ID.  
  
    ```csharp
    using System;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.CustomSearch;

    namespace CustomSrchSDK
    {
        class Program
        {
            static void Main(string[] args)
            {

                var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-SUBSCRIPTION-KEY"));

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
                            Console.WriteLine("Number of webpage results {0}", webData.WebPages.Value.Count);
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

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

        }
    }
    ```  
  
5. Bouwen en uitvoeren (foutopsporing) de oplossing. 




## <a name="breaking-it-down"></a>De analyseren

Na de installatie van het pakket NuGet Custom Search, dat u wilt toevoegen een richtlijn voor het.

```csharp
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;
```

Vervolgens exemplaar maken van de aangepaste zoekopdrachten-client, die u kunt aanvragen zoeken. Vervang `YOUR-SUBSCRIPTION-KEY` met uw sleutel.

```csharp
var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));
```

Nu kunt u de client een zoekaanvraag te verzenden. Vervang uw `YOUR-CUSTOM-CONFIG-ID` met de ID van de configuratie van uw exemplaar (u vindt de ID in de [Custom Search-portal](https://www.customsearch.ai/)). In dit voorbeeld wordt gezocht naar Xbox. De update zo nodig.

```csharp
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
```

De `SearchAsync` methode retourneert een `WebData` object. Gebruik `WebData` te doorlopen en alle `WebPages` die zijn gevonden. Met deze code vindt u het eerste webpagina resultaat en van de webpagina worden afgedrukt `Name` en `URL`.

```csharp
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


## <a name="next-steps"></a>Volgende stappen

Kijk eens naar de SDK-voorbeelden. Elk voorbeeld bevat een Leesmij-bestand met informatie over vereisten en installatie/uitvoeren van de voorbeelden.

* Aan de slag met [.NET-voorbeelden](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7) 
    * [NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0)
    * Zie ook [.NET-bibliotheken](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingCustomSearch) voor definities en afhankelijkheden.
* Aan de slag met [NodeJS-voorbeelden](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
    * Zie ook [NodeJS-bibliotheken](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/customSearch) voor definities en afhankelijkheden.
* Aan de slag met [Java-voorbeelden](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
    * Zie ook [Java-bibliotheken](https://github.com/Azure/azure-sdk-for-java/tree/master/cognitiveservices/azure-customsearch) voor definities en afhankelijkheden.
* Aan de slag met [voorbeelden van Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) 
    * Zie ook [Python-bibliotheken](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-customsearch) voor definities en afhankelijkheden.

