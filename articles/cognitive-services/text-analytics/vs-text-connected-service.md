---
title: Text Analytics-C#-zelfstudie | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Verbinding maken met Text Analytics vanuit een ASP.NET Core-webtoepassing.
services: cognitive-services
author: ghogen
manager: douge
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 06/01/2018
ms.author: ghogen
ms.openlocfilehash: c97f75e0a41a4bf314963dc26c6424a0b773822b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38665232"
---
# <a name="connect-to-the-text-analytics-service-by-using-connected-services-in-visual-studio"></a>Verbinding maken met de Text Analytics-Service met behulp van Connected Services in Visual Studio

U kunt met behulp van de Text Analytics-Service, extraheren van uitgebreide informatie om te categoriseren en verwerken van visuele gegevens en gebruik toezicht op afbeeldingen met machines om uw services te voeren.

In dit artikel en de bijbehorende artikelen bieden details voor het gebruik van de Visual Studio Connected Service-functie voor de Text Analytics-Service. De mogelijkheid is beschikbaar in zowel Visual Studio 2017 15,7 of hoger, met de Cognitive Services-extensie geïnstalleerd.

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
- Visual Studio 2017 versie 15,7, met de ontwikkeling van Web-werkbelasting geïnstalleerd. [Nu downloaden](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="add-support-to-your-project-for-the-text-analytics-service"></a>Ondersteuning toegevoegd aan uw project voor de Text Analytics-Service

1. Maak een nieuwe ASP.NET Core webproject TextAnalyticsDemo genoemd. Gebruik de projectsjabloon webtoepassing (Model-View-Controller) met de standaardinstellingen. Het is belangrijk om de naam van het project MyWebApplication, zodat de naamruimte overeenkomt met wanneer u code hebt gekopieerd in het project.  Het voorbeeld in dit artikel gebruikt MVC, maar u kunt de Text Analytics Connected Service gebruiken met elk type ASP.NET-project.

1. In **Solution Explorer**, dubbelklik op de **Connected Service** item.
   De Connected Service-pagina wordt weergegeven met services die u aan uw project toevoegen kunt.

   ![Schermafbeelding van Connected Service in Solution Explorer](../media/vs-common/Connected-Services-Solution-Explorer.PNG)

1. Kies in het menu van de beschikbare services, **gevoel evalueren met Tekstanalyse**.

   ![Schermafbeelding van Connected Services scherm](./media/vs-text-connected-service/Cog-Text-Connected-Service-0.PNG)

   Als u bent aangemeld bij Visual Studio en Azure-abonnement dat is gekoppeld aan uw account hebt, wordt een pagina weergegeven met een vervolgkeuzelijst met uw abonnementen.

   ![Schermafbeelding van de Text Analytics Connected Service scherm](media/vs-text-connected-service/Cog-Text-Connected-Service-1.PNG)

1. Selecteer het abonnement dat u wilt gebruiken, en kies een naam voor de Text Analytics-Service, of kies de **bewerken** koppeling naar de automatisch gegenereerde naam wijzigen, kiest u de resourcegroep en de prijscategorie.

   ![Schermafbeelding van de resourcegroep en prijzen laag velden](media/vs-text-connected-service/Cog-Text-Connected-Service-2.PNG)

   Volg de koppeling voor meer informatie over de Prijscategorieën.

1. Kies **toevoegen** ondersteuning voor de Connected Service toevoegen.
   Visual Studio Hiermee wijzigt u het project om toe te voegen van de NuGet-pakketten, vermeldingen in configuratie en andere wijzigingen voor de ondersteuning van een verbinding met de Text Analytics-Service. De **uitvoervenster** ziet u het logboek van wat in uw project gebeurt er. U ziet er ongeveer als volgt uit:

   ```output
    [6/1/2018 3:04:02.347 PM] Adding Text Analytics to the project.
    [6/1/2018 3:04:02.906 PM] Creating new Text Analytics...
    [6/1/2018 3:04:06.314 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Language' version 1.0.0-preview...
    [6/1/2018 3:04:56.759 PM] Retrieving keys...
    [6/1/2018 3:04:57.822 PM] Updating appsettings.json setting: 'ServiceKey' = '<service key>'
    [6/1/2018 3:04:57.827 PM] Updating appsettings.json setting: 'ServiceEndPoint' = 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0'
    [6/1/2018 3:04:57.832 PM] Updating appsettings.json setting: 'Name' = 'TextAnalyticsDemo'
    [6/1/2018 3:05:01.840 PM] Successfully added Text Analytics to the project.
    ```
 
## <a name="use-the-text-analytics-service-to-detect-the-language-for-a-text-sample"></a>De Text Analytics-Service gebruiken voor het detecteren van de taal voor een tekstvoorbeeld van.

1. Voeg de volgende using-instructies in Startup.cs.
 
   ```csharp
   using System.IO;
   using System.Net.Http;
   using System.Net.Http.Headers;
   using System.Text;
   using Microsoft.Extensions.Configuration;
   ```
 
1. Voeg een veld voor configuratie en een constructor die het configuratie-veld in de opstartklasse configuratie inschakelen in uw programma initialiseert toevoegen.

   ```csharp
      private IConfiguration configuration;

      public Startup(IConfiguration configuration)
      {
          this.configuration = configuration;
      }
   ```

1. Toevoegen van een klassebestand in de map van de Controllers DemoTextAnalyzeController genoemd en vervang de inhoud ervan door de volgende code:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    using Microsoft.Extensions.Configuration;
    using TextAnalyticsDemo.Models;
    
    namespace TextAnalyticsDemo.Controllers
    {
        public class DemoTextAnalyzeController : Controller
        {
            private IConfiguration configuration;
    
            public DemoTextAnalyzeController(IConfiguration configuration)
            {
                this.configuration = configuration;
            }
    
            public IActionResult TextAnalyzeResult(TextAnalyzeModel model)
            {
    
                if (!string.IsNullOrWhiteSpace(model.TextStr))
                {
                    ITextAnalyticsAPI client = this.GetTextAnalyzeClient(new MyHandler());
                    model.AnalyzeResult = client.DetectLanguage(
                        new BatchInput(
                            new List<Input>()
                            {
                                new Input("id",model.TextStr)
                            }));
                }
                return View(model);
            }
    
            [HttpPost("Analyze")]
            public IActionResult Analyze(TextAnalyzeModel model)
            {
                return RedirectToAction("TextAnalyzeResult", model);
            }
    
            // Using the ServiceKey from the configuration file,
            // get an instance of the Text Analytics client.
            private ITextAnalyticsAPI GetTextAnalyzeClient(DelegatingHandler handler)
            {
                string key = configuration.GetSection("CognitiveServices")["TextAnalytics:ServiceKey"];
    
                ITextAnalyticsAPI client = new TextAnalyticsAPI( handlers: handler);
                client.SubscriptionKey = key;
                client.AzureRegion = AzureRegions.Westus;
    
                return client;
            }
        }
    }
    ```
    
    De code bevat GetTextAnalyzeClient om op te halen van de client-object dat u kunt gebruiken voor het aanroepen van de Tekstanalyse-API en een aanvraaghandler waarmee DetectLanguage wordt aangeroepen op een opgegeven tekst.

1. De MyHandler helperklasse die wordt gebruikt door de bovenstaande code toevoegen.

    ```csharp
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

1. In de map modellen, voegt u een klasse voor het model.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    
    namespace Demo.Models
    {
        public class TextAnalyzeModel
        {
            public string TextStr { get; set; }
    
            public LanguageBatchResult AnalyzeResult { get; set; }
    
            public SentimentBatchResult AnalyzeResult2 { get; set; }
        }
    }
    ```

1. De weergave waarin de geanalyseerde tekst, de taal bepaald en de score die het niveau van betrouwbaarheid in de analyse is toegevoegd.
    
    ```cshtml
    @using System
    @model Demo.Models.TextAnalyzeModel
    
    @{
        ViewData["Title"] = "TextAnalyzeResult";
    }
    
    <h2>Text Language</h2>
    
    <div class="row">
        <section>
            <form asp-controller="DemoTextAnalyze" asp-action="Analyze" method="POST"
                  class="form-horizontal" enctype="multipart/form-data">
                <table width="90%">
                    <tr>
                        <td>
                            <input type="text" name="TextStr" class="form-control" />
                        </td>
                        <td>
                            <button type="submit" class="btn btn-default">Analyze</button>
                        </td>
                    </tr>
                </table>
            </form>
        </section>
    </div>
    
    <h2>Result</h2>
    <div>
        <dl class="dl-horizontal">
            <dt>
                Text :
            </dt>
            <dd>
                @Html.DisplayFor(model => model.TextStr)
            </dd>
            <dt>
                Language Name :
            </dt>
            <dd>
                @Html.DisplayFor(model => model.AnalyzeResult.Documents[0].DetectedLanguages[0].Name)
            </dd>
            <dt>
                Score :
            </dt>
            <dd>
                @Html.DisplayFor(model => model.AnalyzeResult.Documents[0].DetectedLanguages[0].Score)
            </dd>
        </dl>
    </div>
    <div>
        <hr />
        <p>
            <a asp-controller="Home" asp-action="Index">Return to Index</a>
        </p>
    </div>
    
    ```
 
1. Ontwikkel en voer het voorbeeld lokaal. Voer tekst en zien welke taal Text Analytics wordt gedetecteerd.
   
## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de resourcegroep. Hiermee verwijdert u de cognitive service en gerelateerde bronnen. De resourcegroep verwijderen via de portal:

1. Voer de naam van uw resourcegroep in het vak Zoeken bovenaan de portal in. Wanneer u de resourcegroep die wordt gebruikt in deze zelfstudie in de zoekresultaten ziet, selecteert u deze.
2. Selecteer **Resourcegroep verwijderen**.
3. Typ in het vak **TYP DE NAAM VAN DE RESOURCEGROEP** de naam van de resourcegroep en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de Text Analytics-Service door het lezen van de [documentatie voor Text Analytics-Service](index.yml).
