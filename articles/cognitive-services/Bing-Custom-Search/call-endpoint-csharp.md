---
title: Eindpunt aanroepen met behulp van C# - Bing Custom Search - Microsoft Cognitive Services
description: Deze quickstart laat zien hoe zoekresultaten van uw exemplaar voor aangepast zoeken met behulp van C# om aan te roepen van het eindpunt van de Bing Custom Search aanvraagt.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 87970e1c5e8487f9afca2acc680bdfeb610dc89f
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "41987545"
---
# <a name="call-bing-custom-search-endpoint-c"></a>Bing Custom Search-eindpunt (C#) aanroepen

Deze quickstart laat zien hoe zoekresultaten van uw exemplaar voor aangepast zoeken met behulp van C# om aan te roepen van het eindpunt van de Bing Custom Search aanvraagt. 

## <a name="prerequisites"></a>Vereisten

-  Een exemplaar voor aangepast zoeken van kant-en-klare. Zie [maken van uw eerste exemplaar van de Bing Custom Search](quick-start.md).
-  [.NET core](https://www.microsoft.com/net/download/core) geïnstalleerd.
- Een [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing zoeken-API's**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) is voldoende voor deze Quick Start. Moet u de toegangssleutel die is opgegeven wanneer u uw gratis proefversie activeren, of u de sleutel van een betaald abonnement van uw Azure-dashboard kunt.  

  >[!NOTE]  
  >Bestaande Bing Custom Search-klanten die een preview-sleutel is ingericht op of vóór 15 oktober 2017, worden hun sleutels gebruiken tot en met 30 November 2017, of totdat ze zijn uitgeput, het maximum aantal query's zijn toegestaan. Daarna, moeten ze migreren naar de algemeen beschikbare versie van Azure. 
 
## <a name="run-the-code"></a>De code uitvoeren

Als u wilt uitvoeren in het volgende voorbeeld, de volgende stappen uit:

1. Maak een map voor uw code.
2. Navigeer naar de map die u zojuist hebt gemaakt vanaf een opdrachtprompt of terminal.
3. Voer de volgende opdrachten uit:
    ```
    dotnet new console -o BingCustomSearch
    cd BingCustomSearch
    dotnet add package Newtonsoft.Json
    dotnet restore
   ```

4. Kopieer de volgende code aan Program.cs.
5. Vervang **uw-SUBSCRIPTION-KEY** en **uw-aangepaste-CONFIG-ID** met uw sleutel en -configuratie-ID.

    ``` CSharp
    using System;
    using System.Net.Http;
    using System.Web;
    using Newtonsoft.Json;
    
    namespace bing_custom_search_example_dotnet
    {
        class Program
        {
            static void Main(string[] args)
            {
                var subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
                var customConfigId = "YOUR-CUSTOM-CONFIG-ID";
                var searchTerm = args.Length > 0 ? args[0]: "microsoft";            
    
                var url = "https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?" +
                    "q=" + searchTerm +
                    "&customconfig=" + customConfigId;
    
                var client = new HttpClient();
                client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
                var httpResponseMessage = client.GetAsync(url).Result;
                var responseContent = httpResponseMessage.Content.ReadAsStringAsync().Result;
                BingCustomSearchResponse response = JsonConvert.DeserializeObject<BingCustomSearchResponse>(responseContent);
                
                for(int i = 0; i < response.webPages.value.Length; i++)
                {                
                    var webPage = response.webPages.value[i];
                    
                    Console.WriteLine("name: " + webPage.name);
                    Console.WriteLine("url: " + webPage.url);                
                    Console.WriteLine("displayUrl: " + webPage.displayUrl);
                    Console.WriteLine("snippet: " + webPage.snippet);
                    Console.WriteLine("dateLastCrawled: " + webPage.dateLastCrawled);
                    Console.WriteLine();
                }            
            }
        }
    
        public class BingCustomSearchResponse
        {        
            public string _type{ get; set; }            
            public WebPages webPages { get; set; }
        }
    
        public class WebPages
        {
            public string webSearchUrl { get; set; }
            public int totalEstimatedMatches { get; set; }
            public WebPage[] value { get; set; }        
        }
    
        public class WebPage
        {
            public string name { get; set; }
            public string url { get; set; }
            public string displayUrl { get; set; }
            public string snippet { get; set; }
            public DateTime dateLastCrawled { get; set; }
            public string cachedPageUrl { get; set; }
            public OpenGraphImage openGraphImage { get; set; }        
        }
        
        public class OpenGraphImage
        {
            public string contentUrl { get; set; }
            public int width { get; set; }
            public int height { get; set; }
        }
    }
    ```
6. De toepassing ontwikkelen met de volgende opdracht uit. Houd er rekening mee de dll-pad waarnaar wordt verwezen door de uitvoer van de opdracht.

    <pre>
    dotnet build 
    </pre>
7. Voer de toepassing met behulp van de volgende opdracht vervangt **pad-uitvoer** met het pad waarnaar wordt verwezen door de build-stap.

    <pre>    
    dotnet **PATH TO OUTPUT**
    </pre>

## <a name="next-steps"></a>Volgende stappen
- [Configureren van uw gehoste gebruikersinterface-ervaring](./hosted-ui.md)
- [Decoration markeringen gebruiken om te markeren van tekst](./hit-highlighting.md)
- [Pagina webpagina 's](./page-webpages.md)
