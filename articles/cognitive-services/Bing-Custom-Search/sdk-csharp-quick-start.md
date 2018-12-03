---
title: 'Snelstartgids: Custom Search SDK, C#'
titleSuffix: Azure Cognitive Services
description: Lees hier informatie over de instellingen voor het maken van een C#-consoletoepassing met behulp van Custom Search SDK.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 09/06/2018
ms.author: scottwhi
ms.openlocfilehash: 7ac298ad5c5b93b5dce0ce2dd59ffe541888db88
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52307760"
---
# <a name="quickstart-using-the-bing-custom-search-sdk-with-c"></a>Snelstartgids: Bing Custom Search SDK gebruiken met C#

De Bing Custom Search SDK biedt een eenvoudiger programmeermodel dan de Bing Custom Search REST-API. In dit gedeelte vindt u stapsgewijze instructies voor het maken van uw eerste Custom Search-aanroepen met behulp van de SDK voor C#.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze quickstart te voltooien:

- Een exemplaar voor aangepaste zoekopdrachten dat klaar is voor gebruik. Zie [Uw eerste Bing Aangepaste zoekopdrachten-exemplaar maken](quick-start.md) voor meer informatie.  
  
- Een abonnementssleutel. U kunt een abonnementssleutel opvragen wanneer u uw [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) activeert, of u kunt een sleutel voor een betaald abonnement gebruiken uit uw Azure-dashboard (zie [Snelstartgids: Een Cognitive Services-account maken in de Azure-portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).  Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).
  
- Visual Studio 2017 is geïnstalleerd. Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken.  
  
- Het [NuGet-pakket Custom Search](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) is geïnstalleerd. Klik in Solution Explorer in Visual Studio met de rechtermuisknop op uw project en selecteer `Manage NuGet Packages` in het menu. Installeer het `Microsoft.Azure.CognitiveServices.Search.CustomSearch`-pakket.

Met de installatie van het NuGet-pakket Custom Search worden ook de volgende assembly's geïnstalleerd:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json



## <a name="run-the-code"></a>De code uitvoeren

> [!TIP]
> Haal de nieuwste code als Visual Studio-oplossing op uit [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingCustomWebSearch).

Volg deze stappen om het voorbeeld uit te voeren:

1. Open Visual Studio 2017.
  
2. Klik op het menu **File**, klik op **New**, **Project** en vervolgens op de sjabloon **Visual C# Console Application**.
  
3. Geef uw oplossing de naam CustomSearchSolution en blader naar de map waarin u deze wilt opslaan.
  
4. Klik op OK om de oplossing te maken.  
  
4. Klik in Solution Explorer met de rechtermuisknop op uw project (met dezelfde naam als de oplossing) en selecteer `Manage NuGet Packages`. Klik op **Browse** in NuGet Package Manager. Voer Microsoft.Azure.CognitiveServices.Search.CustomSearch in het zoekvak in en druk op Enter. Selecteer het pakket en klik op Install.  
  
4. Kopieer de volgende code naar het bestand Program.cs. Vervang **YOUR-SUBSCRIPTION-KEY** en **YOUR-CUSTOM-CONFIG-ID** door uw abonnementssleutel en configuratie-id.  
  
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
  
5. Bouw de oplossing en voer deze uit (eventueel met foutopsporing). 




## <a name="breaking-it-down"></a>Analyse

Na installatie van het NuGet-pakket Custom Search, moet u een using-instructie toevoegen voor het pakket.

```csharp
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;
```

Vervolgens maakt u een instantie van de client voor aangepaste zoekopdrachten, die u gaat gebruiken om aanvragen te verzenden. Vergeet niet om `YOUR-SUBSCRIPTION-KEY` te vervangen door uw sleutel.

```csharp
var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));
```

Nu kunt u de client gebruiken om een zoekaanvraag te verzenden. Vervang `YOUR-CUSTOM-CONFIG-ID` door de id van de configuratie van uw exemplaar (u vindt de id in de [portal van Custom Search](https://www.customsearch.ai/)). In dit voorbeeld wordt gezocht naar Xbox. U kunt dit desgewenst aanpassen.

```csharp
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
```

De methode `SearchAsync` retourneert een `WebData`-object. Gebruik `WebData` om de `WebPages` te doorlopen die zijn gevonden. Met deze code vindt u het eerste resultaat dat uit een webpagina bestaat en worden de `Name` en `URL` van de webpagina weergegeven.

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

Kijk eens naar de SDK-voorbeelden. Elk voorbeeld omvat een ReadMe-bestand met informatie over vereisten en het installeren/uitvoeren van de voorbeelden.

* Aan de slag met [.NET-voorbeelden](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7) 
    * [NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0)
    * Zie ook [.NET-bibliotheken](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingCustomSearch) voor definities en afhankelijkheden.
* Aan de slag met [NodeJS-voorbeelden](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
    * Zie ook [NodeJS-bibliotheken](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/customSearch) voor definities en afhankelijkheden.
* Aan de slag met [Java-voorbeelden](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
    * Zie ook [Java-bibliotheken](https://github.com/Azure/azure-sdk-for-java/tree/master/cognitiveservices/azure-customsearch) voor definities en afhankelijkheden.
* Aan de slag met [Python-voorbeelden](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) 
    * Zie ook [Python-bibliotheken](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-customsearch) voor definities en afhankelijkheden.

