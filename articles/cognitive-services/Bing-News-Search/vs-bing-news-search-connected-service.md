---
title: Bing nieuws zoeken in C#-zelfstudie | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Verbinding maken met Cognitive Services-Bing nieuws zoeken vanuit een ASP.NET Core-webtoepassing.
services: cognitive-services
author: ghogen
manager: douge
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: conceptual
ms.date: 03/01/2018
ms.author: ghogen
ms.openlocfilehash: 5cfa82067d28b05f32bd87e0e83d55a03da8d508
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38665218"
---
# <a name="connect-to-bing-news-search-api-by-using-connected-services-in-visual-studio"></a>Verbinding maken met Bing nieuws zoeken-API met behulp van Connected Services in Visual Studio

U kunt met behulp van zoeken in Bing nieuws, apps en gebruik de kracht van een engine voor zoeken zonder advertenties binnen het bereik van de web-services inschakelen. Zoeken in Bing nieuws is een van de search-services beschikbaar met Cognitive Services.

Dit artikel bevat informatie voor het gebruik van de functie voor Visual Studio Connected Service voor zoeken in Bing nieuws. De mogelijkheid is beschikbaar in Visual Studio 2017 15,7 of hoger, met de Cognitive Services-extensie geïnstalleerd.

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
- Visual Studio 2017 versie 15,7, met de ontwikkeling van Web-werkbelasting geïnstalleerd. [Nu downloaden](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="add-support-to-your-project-for-bing-news-search-api"></a>Ondersteuning toegevoegd aan uw project voor Bing nieuws zoeken-API

1. Maak een nieuwe ASP.NET Core web-project met de naam MyWebApplication. Gebruik de **webtoepassing (Model-View-Controller)** projectsjabloon, maken, met de standaardinstellingen. Het is belangrijk om de naam van het project MyWebApplication, zodat de naamruimte overeenkomt met wanneer u code hebt gekopieerd in het project. 

1. In **Solution Explorer**, kiest u **toevoegen** > **Connected Service**.
   De Connected Service-pagina wordt weergegeven met services die u aan uw project toevoegen kunt.

   ![Schermafbeelding van de Add Connected Service menu-item](../media/vs-common/Connected-Service-Menu.PNG)

1. Kies in het menu van de beschikbare services, **intelligente zoeken naar uw Apps brengen**.

   ![Schermafbeelding van de lijst met verbonden services](./media/vs-bing-news-search-connected-service/Cog-Search-Connected-Service-0.PNG)

   Als u bent aangemeld bij Visual Studio en Azure-abonnement dat is gekoppeld aan uw account hebt, wordt een pagina weergegeven met een vervolgkeuzelijst met uw abonnementen. Selecteer het abonnement dat u wilt gebruiken, en kies vervolgens een naam op voor de Bing nieuws zoeken-API. U kunt ook **bewerken** te wijzigen van de automatisch gegenereerde naam.

   ![Schermopname van het abonnement en de naam van velden](media/vs-bing-news-search-connected-service/Cog-Search-Connected-Service-1.PNG)

1. Kies de resourcegroep en de prijscategorie.

   ![Schermafbeelding van de resourcegroep en prijzen laag velden](media/vs-bing-news-search-connected-service/Cog-Search-Connected-Service-2.PNG)

   Als u meer informatie over de Prijscategorieën wilt, selecteert u **revisie prijzen**.

1. Kies **toevoegen** ondersteuning voor de Connected Service toevoegen.
   Visual Studio Hiermee wijzigt u het project om toe te voegen van de NuGet-pakketten, vermeldingen in configuratie en andere wijzigingen voor de ondersteuning van een verbinding met de Bing nieuws zoeken-API. De uitvoer toont het logboek van wat in uw project gebeurt er. U ziet er ongeveer als volgt uit:

   ```output
   [5/4/2018 12:41:21.084 PM] Adding Intelligent Search to the project.
   [5/4/2018 12:41:21.271 PM] Creating new Intelligent Search...
   [5/4/2018 12:41:24.128 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.ImageSearch' version 1.2.0...
   [5/4/2018 12:41:24.135 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.NewsSearch' version 1.2.0...
   [5/4/2018 12:41:24.154 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.WebSearch' version 1.2.0...
   [5/4/2018 12:41:24.168 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.CustomSearch' version 1.2.0...
   [5/4/2018 12:41:24.187 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.VideoSearch' version 1.2.0...
   [5/4/2018 12:42:07.287 PM] Retrieving keys...
   [5/4/2018 12:42:07.741 PM] Updating appsettings.json setting: 'ServiceKey' = 'c271412f3e4c4e1dacc7c4145fa0572a'
   [5/4/2018 12:42:07.745 PM] Updating appsettings.json setting: 'ServiceEndPoint' = 'https://api.cognitive.microsoft.com/bing/v7.0'
   [5/4/2018 12:42:07.749 PM] Updating appsettings.json setting: 'Name' = 'WebApplicationCore-Search_IntelligentSearch'
   [5/4/2018 12:42:10.217 PM] Successfully added Intelligent Search to the project.
   ```

   Het bestand appsettings.json bevat nu de volgende nieuwe instellingen:

   ```json
   "CognitiveServices": {
     "IntelligentSearch": {
       "ServiceKey": "<your service key>",
       "ServiceEndPoint": "https://api.cognitive.microsoft.com/bing/v7.0",
       "Name": "WebApplicationCore-Search_IntelligentSearch"
     }
   }
   ```
 
## <a name="use-the-bing-news-search-api-to-add-search-functionality-to-a-web-page"></a>De Bing nieuws zoeken-API gebruiken zoekfunctionaliteit toevoegen aan een webpagina

Nu u ondersteuning voor de Bing nieuws zoeken-API hebt toegevoegd aan uw project, hier is het gebruik van de API om toe te voegen intelligent zoeken naar een webpagina.

1.  In *Startup.cs*, in de `ConfigureServices` methode, Voeg een aanroep naar `IServiceCollection.AddSingleton`. Dit maakt het configuratieobject met de belangrijkste instellingen die beschikbaar is voor de code in uw project.
 
   ```csharp
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddMvc();
            services.AddSingleton<IConfiguration>(Configuration);
        }
   ```


1. Voeg een nieuw klassebestand onder de **modellen** map, met de naam *BingNewsModel.cs*. Als u de naam van uw project anders, gebruik van de naamruimte van uw eigen project, in plaats van MyWebApplication. Vervang de inhoud door de volgende code:
 
    ```csharp
    using Microsoft.Azure.CognitiveServices.Search.NewsSearch.Models;
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    
    namespace MyWebApplication.Models
    {
        public class BingNewsModel
        {
            public News SearchResult { get; set; } 
            public string SearchText { get; set; }
        }
    }
    ```

   Dit model wordt gebruikt voor het opslaan van de resultaten van een aanroep naar de zoeken in Bing nieuws-service.
 
1. In de **Controllers** map toevoegen van een nieuwe klassebestand met de naam *IntelligentSearchController.cs*. Vervang de inhoud door de volgende code:

   ```csharp
    using System.Net.Http;
    using System.Threading.Tasks;
    using MyWebApplication.Models;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Azure.CognitiveServices.Search.NewsSearch;
    using Microsoft.Extensions.Configuration;
    
    namespace MyWebApplication.Controllers
    {
        // A controller to handle News Search requests
        public class IntelligentSearchController : Controller
        {
            private IConfiguration configuration;
  
            // Set up the configuration that contains the keys
            // (from the appsettings.json file)
            // that you will use to access the service  
            public IntelligentSearchController(IConfiguration configuration)
            {
                this.configuration = configuration;
            }

            // Call the Bing News Search API and put the result in the model object.    
            public async Task<IActionResult> BingSearchResult(BingNewsModel model)
            {
                if (!string.IsNullOrWhiteSpace(model.SearchText))
                { 
                    INewsSearchAPI client = this.GetNewsSearchClient(new MyHandler());
                    model.SearchResult = await client.News.SearchAsync(model.SearchText);
                }
                return View(model);
            }
    
            // Forward requests to the Search endpoint to the BingSearchResult method
            [HttpPost("Search")]
            public IActionResult Search(BingNewsModel model)
            {
                return RedirectToAction("BingSearchResult", model);
            }

            // Get the search client object
            private INewsSearchAPI GetNewsSearchClient(DelegatingHandler handler)
            {
                string key =
                   configuration.GetSection("CognitiveServices")["IntelligentSearch:ServiceKey"];
    
                INewsSearchAPI client = new NewsSearchAPI(
                   new ApiKeyServiceClientCredentials(key), handlers: handler);
    
                return client;
            }
        }
    }
   ```

   In deze code wordt de constructor stelt u het configuratieobject die uw sleutels bevat. De methode voor het `Search` route is slechts een omleiding naar de `BingSearchResult` functie. Hiermee wordt de `GetNewsSearchClient` methode om op te halen de `NewsSearchAPI` clientobject.  De `NewsSearchAPI` clientobject bevat de `SearchAsync` methode die daadwerkelijk roept de service en de resultaten in de `SearchResult` model dat u zojuist hebt gemaakt. 

1. Voeg een klasse `MyHandler`, waarmee wordt verwezen in de bovenstaande code. Dit voor netwerkapparaten delegeert de asynchrone aanroep van de search-service naar de basisklasse `DelegatingHandler`.

   ```csharp
    using System.Net.Http;
    using System.Threading.Tasks;
    using System.Threading;

    class MyHandler : DelegatingHandler
    {
        protected async override Task<HttpResponseMessage> SendAsync(
        HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Call the inner handler.
            var response = await base.SendAsync(request, cancellationToken);
            
            return response;
        }
    }
   ```

1. Het toevoegen van ondersteuning voor het indienen van zoekopdrachten en de resultaten weer te geven de **weergaven** map, maak een nieuwe map met de naam **IntelligentSearch**. In deze nieuwe map, een weergave toevoegen voor *BingSearchResult.cshtml*. Kopieer in de volgende code:

    ```cshtml
    @using System
    @model MyWebApplication.Models.BingNewsModel
    
    @{
        ViewData["Title"] = "BingSearchResult";
    }
    
    <h2>Search News</h2>
    
    <div class="row">
        <section>
            <form asp-controller="IntelligentSearch" asp-action="Search" method="POST"
                  class="form-horizontal" enctype="multipart/form-data">
                <table width ="90%">
                    <tr>
                        <td>
                            <input type="text" name="SearchText" class="form-control" />
                        </td>
                        <td>
                            <button type="submit" class="btn btn-default">Search</button>
                        </td>
                    </tr>
                </table>
            </form>
        </section>
    </div>
    <h2>Search Result</h2=
    <table>
    @if (!string.IsNullOrEmpty(Model.SearchText)) {
        foreach (var item in Model.SearchResult.Value) {
        <tr>
            <td rowspan="2" width="90">
               <img src=@item?.Image?.Thumbnail?.ContentUrl width="80" height="80" />
            </td>
            <td><a href=@item.Url>@item.Name</a></td>
        </tr>   
        <tr>
            <td>@item.Description</td>
        </tr>
        <tr height="10">
            <td/><td/>
         </tr>
        } }
     </table>
    <div>
        <hr />
        <p>
            <a asp-controller="Home" asp-action="Index">Return to Index</a>
        </p>
    </div>
    ```

1. Start de web-App lokaal en voer de URL voor de pagina die u zojuist hebt gemaakt (/ IntelligentSearch/BingSearchResult) en een zoekaanvraag plaatsen met behulp van de knop zoeken.

   ![Schermopname van zoeken in Bing nieuws-resultaten](media/vs-bing-news-search-connected-service/Cog-News-Search-Results.PNG)
           
## <a name="clean-up-resources"></a>Resources opschonen

Als de resourcegroep niet meer nodig is, kunt u deze kunt verwijderen. Hiermee verwijdert u de cognitive service en gerelateerde bronnen. De resourcegroep verwijderen via de portal:

1. Voer de naam van de resourcegroep in het zoekvak boven aan de portal. Selecteer de resourcegroep die u wilt verwijderen.
2. Selecteer **Resourcegroep verwijderen**.
3. In de **typt u de naam van de Resource** vak, voer de naam van de resourcegroep en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Bing nieuws zoeken-API, [wat is er zoeken in Bing nieuws?](index.yml).
