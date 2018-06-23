---
title: Oproep-eindpunt met C# - Bing aangepaste zoekactie - Microsoft cognitieve Services
description: Deze snelstartgids laat zien hoe zoekresultaten aanvragen bij uw aangepaste zoekactie-exemplaar met behulp van C# om aan te roepen van het eindpunt van de aangepaste Bing-zoekopdracht.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: be4cc79d16b9a22124f16878b11ca04a916f98ae
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344899"
---
# <a name="call-bing-custom-search-endpoint-c"></a>Bing aangepaste zoekactie eindpunt (C#) aanroepen

Deze snelstartgids laat zien hoe zoekresultaten aanvragen bij uw aangepaste zoekactie-exemplaar met behulp van C# om aan te roepen van het eindpunt van de aangepaste Bing-zoekopdracht. 

## <a name="prerequisites"></a>Vereisten

-  Een exemplaar van de kant-en-klare aangepaste zoekactie. Zie [maken van uw eerste exemplaar van Bing aangepaste zoekactie](quick-start.md).
-  [.NET core](https://www.microsoft.com/net/download/core) geïnstalleerd.
- Een [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing zoeken-API's**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) voldoende is voor deze snelstartgids. U moet de toegangssleutel die is opgegeven bij het activeren van uw gratis proefversie of u kunt de sleutel van een betaald abonnement van uw Azure-dashboard.  

  >[!NOTE]  
  >Bestaande Bing aangepaste zoekactie-klanten die beschikken over een sleutel preview is ingericht op of vóór 15 oktober 2017, worden de sleutels gebruiken totdat het 30 November 2017 of totdat ze zijn het maximum aantal query's zijn toegestaan. Daarna nodig ze hebben om te migreren naar de versie van het algemeen beschikbaar op Azure. 
 
## <a name="run-the-code"></a>De code uitvoeren

U voert dit voorbeeld, de volgende stappen uit:

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
5. Vervang **uw ABONNEMENTSSLEUTEL** en **uw-aangepaste-CONFIG-ID** met uw sleutel en configuratie-ID.

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
6. De toepassing met de volgende opdracht maakt. Noteer het dll-pad waarnaar wordt verwezen door de uitvoer van de opdracht.
    <pre>
    dotnet build 
    </pre>
7. De toepassing uitvoeren met de volgende opdracht vervangen **pad-uitvoer** met het pad waarnaar wordt verwezen door de build-stap.
    <pre>    
    dotnet **PATH TO OUTPUT**
    </pre>

## <a name="next-steps"></a>Volgende stappen
- [Configureer uw gehoste UI-mogelijkheden](./hosted-ui.md)
- [Decoration markeringen gebruiken om te markeren van tekst](./hit-highlighting.md)
- [Pagina webpagina 's](./page-webpages.md)
